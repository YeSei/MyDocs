# 【19】String，StringBuilder和Stringbuffer
```
String string = "";
for(int i=0;i<10000;i++){
    string += "hello";
}
```
> 使用String对象每次赋值都会在常量池中新建一个对象。以上代码创建10000个新的常量池对象，浪费资源。所以引入StringBuilder每次只对StringBuilder对象进行修改。
> StringBuffer是对StringBuilder的线程安全版本。