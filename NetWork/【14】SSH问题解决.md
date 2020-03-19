# 【14】SSH问题解决
## 问题一：SSH保活
> 为了防止SSH连接远程主机在一段时间内断开，
> 进入/etc/ssh/ssh_config
> 加入
```
ServerAliveInterval 60 
TCPKeepAlive no
```
> `ServerAliveInterval`作用是每隔60秒发送一个保活报文。
> `TCPKeepAlive` 关闭TCP保活按钮

## 问题二：SSH出现`packet_write_wait：xxx Broken pipe`
> 在ssh登陆时指定`-o IPQoS=throughput`参数
```
ssh -o IPQoS=throughput name@ip
```
> 或者在/etc/ssh/ssh_config中加入`IPQoS throughput`参数即可解决。

## 问题三：SSH换了网络之后在登陆时出现`packet_write_wait：xxx Broken pipe`
> 目前定位是调制解调器的问题。
> 在ssh登陆时指定`-o IPQoS=0`或者`-o IPQoS=cs0`参数。
> 或者将上面任意一个参数写入/etc/ssh/ssh_config中。