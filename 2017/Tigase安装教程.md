## Tigase安装教程



1. ```mvn -Pdist -f modules/master/pom.xml clean install```
2. **./scripts/db-create-mysql.sh root 123456 tigasedb root 123456 127.0.0.1**
3. 修改Jdk路径,优化线上环境
4. **./scripts/tigase.sh start etc/tigase.conf**(一定要以root方式运行)
5. ```lsof -iTCP:5277 -sTCP:ESTABLISHED -P -n```(测试集群是否成功)
6. ```dig _xmpp-client._tcp.example.com SRV```(验证域名设置是否成功)
7. 完事



## 测试一波波

- 在.tsung目录下: `tsung -f login.xml start`


- 在log目录下生成报表.`/usr/local/Cellar/tsung/1.6.0/lib/tsung/bin/tsung_stats.pl --stats tsung.log`

## Tigase线上优化

1. 针对tcp 3g,8g,12g

>net.ipv4.tcp_mem = 786432 2097152 3145728

2. 尽早杀掉孤儿链接

>net.ipv4.tcp_orphan_retries=1


3. 一些细节
```
#是否启用时间戳选项，该选项会影响net.ipv4.tcp_tw_reuse，默认开启。一般我们为了安全我们不关闭该选项。
net.ipv4.tcp_timestamps = 1

#是否快速回收处于TIME_WAIT状态下得链接，默认关闭，最好别开启，除非有很懂的人建议。
net.ipv4.tcp_tw_recycle = 0

4.前面我们说了FIN-WAIT-1，那如何快速释放FIN-WAIT-2呢，虽然该状态没有FIN-WAIT-1那么耗资源。
net.ipv4.tcp_fin_timeout=30


其余的都不是很复杂了，最复杂的就是上面几个。我贴一份全的给大家参考：

# 开启TCP syncookies，防止DDOS攻击
net.ipv4.tcp_syncookies = 1
#syn报文（每个报文都需要排队）队列长度，超过该长度，请求就被丢弃，内存大于128M的默认为1024
net.ipv4.tcp_max_syn_backlog = 65536
#每个网络接口接收数据包的速率比内核处理这些包的速率快时，允许送到队列的数据包的最大数目
net.core.netdev_max_backlog = 32768
#定义了系统中每一个端口最大的监听队列的长度,这是个全局的参数
net.core.somaxconn = 32768  
#是否启用时间戳选项，该选项会影响net.ipv4.tcp_tw_reuse，默认开启
net.ipv4.tcp_timestamps = 1
#是否快速回收处于TIME_WAIT状态下的socket，由于手机网络时间戳会出现乱跳，所以必须关闭，这个默认关闭。
net.ipv4.tcp_tw_recycle = 0
#被动接受tcp链接时，第二次握手发送SYNACKs的次数，默认为5，对应的时间大概为180秒，官方说法。
net.ipv4.tcp_synack_retries = 3
#跟上面刚好相反，是主动发起tcp链接，发送SYNs的次数，默认为5，对应的时间大概为180秒，官方时间。
net.ipv4.tcp_syn_retries = 3
#我们关闭了TIME_WAIT快速回收，我们通过tcp_tw_reuse和tcp_max_tw_buckets来控制TIME_WAIT避免吃光机器，该值默认180000.
#如果服务器是作为客户端存在的，因为客户端连接受本地端口数限制，所以最好通过tcp_max_tw_buckets控制一下；如果服务器是作为服务端存在的，那么没有端口数的限制，只要情况允许，最好把tcp_max_tw_buckets设置大一些。纯粹就是防御dos攻击的，最好别认为降低该值。
net.ipv4.tcp_max_tw_buckets=180000
#开启处于TIME_WAIT态的socket重用，默认关闭。这个重用的是TIME_WAIT的端口，不是内存等，这个对客户端有意义。
net.ipv4.tcp_tw_reuse=1
#确定TCP栈如何使用内存，当大于上限是报文将丢弃。一般按照缺省值分配，上面的例子就是读写均为8KB，共16KB
#1.6GB TCP内存能容纳的连接数，约为  1600MB/16KB = 100K = 10万
#4.0GB TCP内存能容纳的连接数，约为  4000MB/16KB = 250K = 25万
net.ipv4.tcp_mem = 786432 2097152 3145728
#表示如果套接字由本端要求关闭，这个参数决定了它保持在FIN-WAIT-2状态的时间，2.2中默认180秒，之后默认为60秒  
net.ipv4.tcp_fin_timeout=30
#丢弃已经建立的tcp链接之前，需要多少次重试，默认15次，根据RTO的值，大概13-30分钟
net.ipv4.tcp_retries2=5
#放弃回应一个tcp连接请求之前，需要多少次重试，默认为3
net.ipv4.tcp_retries1=3
#收包速度大于内核处理包的速度时，输入队列最大报文数
net.core.netdev_max_backlog =  32768
#listen系统调用，最大的accept队列长度，超过该值时，后续请求被丢弃
net.core.somaxconn=32768
#针对孤立的socket（已经从进程上下文中删除，可是还有些清理工作没有完成），我们重试的最大次数。也就是server端close之后发[F.]的次数-1（0会重试一次），重负载服务器建议调小，默认为7。
net.ipv4.tcp_orphan_retries=1
```

4. 允许进程同时保留的最大打开文件数。

> ```
> # sysctl fs.file-max
> fs.file-max = 358920
> ```

5. 如果您计划运行大量服务器连接的高负载服务，那么此参数应至少等于您希望支持的网络连接数量的两倍

> ```
> # sysctl -w fs.file-max=360000
> fs.file-max = 360000
> ```

6. ip v4

> ```
> # sysctl -w net.ipv4.ip_local_port_range="1024 65000"
> net.ipv4.ip_local_port_range = 1024 65000
> ```

7. 提高tcp可靠性

> ```
> # sysctl -w net.ipv4.tcp_keepalive_time="60"
> net.ipv4.tcp_keepalive_time = 60
> # sysctl -w net.ipv4.tcp_keepalive_probes="3"
> net.ipv4.tcp_keepalive_probes = 3
> # sysctl -w net.ipv4.tcp_keepalive_intvl="90"
> net.ipv4.tcp_keepalive_intvl = 90
> ```

8. 上面的属性是暂时的，重启就会被覆盖，要想用就行的改变,编辑 /etc/sysctl.conf

> ```
> fs.file-max=360000
> net.ipv4.ip_local_port_range=1024 65000
> net.ipv4.tcp_keepalive_time=60
> net.ipv4.tcp_keepalive_probes=3
> net.ipv4.tcp_keepalive_intvl=90
> ```

```
# sysctl -p（下次启动时，自动加载）
```

9. 具体系统的限制可以使用下面的命令

> ```
> ulimit -a
> ulimit -a -H
> ```

## 问题整理

- ​

> java.io.IOException: Too many open files
>
> 	at sun.nio.ch.ServerSocketChannelImpl.accept0(Native Method)

- ​
- ​

>```
>[os_mon] cpu supervisor port (cpu_sup): Erlang has closed
>```
>
>