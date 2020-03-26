# 【3】IO多路复用的原理
> IO多路复用包括select、poll和epoll三种机制。IO多路复用通过其中一种机制可以实现一个进程可以监视多个描述符。

**IO多路复用的应用场景**：
1. 当用户需要处理多个套接口时；
2. 当一个服务器即要处理TCP，又要处理UDP；
3. 一个TCP服务器既要处理监听套接口，又要处理已连接套接口。

## 3.1 select机制
- select机制中提供了一个**fd_set**，唯一的成员变量一个long int的数组。可以理解为一个集合，实际上是一个位图，每一个特定数组元素中的位来标志相应的文件描述符索引。**此数组大小具有限制**。大小取决于FD_SETSIZE/NFDBITS，一般为1024/32=32。能表示32*32=1024个文件描述符。
- 用户进程每次调用select都需要将整个文件描述符位图集合传到内核空间中去。然后通过轮询这个集合查找就绪时间，返回给用户进程。当数组较大轮询效率也就较低。
```
int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
//nfds: 要监听的文件描述符的范围， 通常使用select监听的所有描述符的最大值加1。
//readfds: 要监听是否有可读事件的文件描述符集
//writefds:要监听是否有可写事件的文件描述符集
//exceptfds:要监听是否有异常事件的文件描述符集
//timeout: 阻塞时间
```
## 3.2 poll机制
- poll定义了一种文件描述符结构体pollfd。每次调用poll需要传递这个结构体数组指针和长度。
```
struct pollfd
{
	int     fd;
	short   events;
	short   revents;
};
```
> poll机制传递的文件描述符数组大小没有限制，但是跟select一样都需要每次调用时，将数组从用户空间复制到内核空间。开销随着文件描述符数量的增加而线性增加。

## 3.3 epoll机制
- epoll使用一个文件描述符管理多个描述符，将用户进程监控的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的copy只需一次。
- epoll内部用了一个**红黑树**记录添加的socket，用了一个**双向链表**接收内核触发的事件。是系统级别的支持的。
```
struct eventpoll{
    ....
    /*红黑树的根节点，这颗树中存储着所有添加到epoll中的需要监控的事件*/
    struct rb_root  rbr;
    /*双链表中则存放着将要通过epoll_wait返回给用户的满足条件的事件*/
    struct list_head rdlist;
    ....
};
```
并使用三个函数来操作这个结构体：
```
int epoll_create(int size); //
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event); 
int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);
```
> **epoll_create**:当某一进程调用epoll_create方法时，Linux内核会创建一个eventpoll结构体。
> **epoll_ctl**:向epoll对象中添加进来的事件,这些事件都会挂载在红黑树中，如此，重复添加的事件就可以通过红黑树而高效的识别出来(红黑树的插入时间效率是lgn，其中n为树的高度)。
> 并且，所有通过**epoll_ctl**添加到epoll中的事件都会与设备(网卡)驱动程序建立回调关系,当相应的事件发生时会调用这个回调方法,它会将发生的事件添加到rdlist双链表中。epoll的基础就是回调呀！
> **epoll_wait**:观察这个list链表里有没有数据即可。有数据就返回，没有数据就sleep，等到timeout时间到后即使链表没数据也返回。

### epoll的优点
> 1. 如果有新的文件描述符需要加入监听队列中，只需要针对该描述符执行epoll_ctl的add即可，不用将所有的文件描述符都重新拷贝至内核空间，大大提高了效率。
> 2. epoll监听的文件描述符数量没有大小限制。
> 3. select、poll在阻塞监听时，需要轮询每个监听的文件描述符，而epoll只需要判断就绪队列是否为空。

### epoll的水平触发LT和边缘触发ET工作模式
LT：在监听事件触发时，将事件加入就绪链表当中，并通知用户进程处理，若用户进程不处理，会在每次执行epoll_wait时继续通知。即用户进程可以不立即处理该事件。
ET：当epoll_wait检测到描述符事件发生，并将该事件加入就绪队列。然后通知用户进程。下次调用epoll_wait时，不会再次响应应用程序并通知此事件。即用户进程必须立即处理该事件。