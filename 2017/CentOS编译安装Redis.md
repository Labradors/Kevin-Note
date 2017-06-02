## 编译

```shell
tar -zxvf redis-2.8.19.tar.gz 
cd redis-2.8.19 
make
make PREFIX=/usr/local/redis install
```

## 配置

```shell
mkdir /usr/local/redis/etc/
cp redis.conf /usr/local/redis/etc/ 
cd /usr/local/redis/bin/
cp redis-benchmark redis-cli redis-server /usr/bin/
```

## 调整内存

```shell
#此参数可用的值为0,1,2 
#0表示当用户空间请求更多的内存时，内核尝试估算出可用的内存 
#1表示内核允许超量使用内存直到内存用完为止 
#2表示整个内存地址空间不能超过swap+(vm.overcommit_ratio)%的RAM值 
echo "vm.overcommit_memory=1">>/etc/sysctl.conf
sysctl -p
```

## 修改配置

```shell
vim /usr/local/redis/etc/redis.conf

# 修改一下配置
# redis以守护进程的方式运行
# no表示不以守护进程的方式运行(会占用一个终端)  
daemonize yes

# 客户端闲置多长时间后断开连接，默认为0关闭此功能                                      
timeout 300

# 设置redis日志级别，默认级别：notice                    
loglevel verbose

# 设置日志文件的输出方式,如果以守护进程的方式运行redis 默认:"" 
# 并且日志输出设置为stdout,那么日志信息就输出到/dev/null里面去了 
logfile stdout
```

## 配置环境变量

```shell
vim /etc/profile
export PATH="$PATH:/usr/local/php/bin:/usr/local/mysql/bin:/usr/local/redis/bin"
# 保存退出

# 让环境变量立即生效
source /etc/profile
```

## 启动脚本

```shell
#!/bin/bash
#chkconfig: 2345 80 90
# Simple Redis init.d script conceived to work on Linux systems
# as it does use of the /proc filesystem.

PATH=/usr/local/bin:/sbin:/usr/bin:/bin
REDISPORT=6379
EXEC=/usr/local/redis/bin/redis-server
REDIS_CLI=/usr/local/redis/bin/redis-cli
   
PIDFILE=/var/run/redis.pid
CONF="/usr/local/redis/etc/redis.conf"
   
case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF
        fi
        if [ "$?"="0" ] 
        then
              echo "Redis is running..."
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $REDIS_CLI -p $REDISPORT SHUTDOWN
                while [ -x ${PIDFILE} ]
               do
                    echo "Waiting for Redis to shutdown ..."
                    sleep 1
                done
                echo "Redis stopped"
        fi
        ;;
   restart|force-reload)
        ${0} stop
        ${0} start
        ;;
  *)
    echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2
        exit 1
esac
```

## 加入系统进程

```shell
# 复制脚本文件到init.d目录下
cp redis /etc/init.d/

# 给脚本增加运行权限
chmod +x /etc/init.d/redis

# 查看服务列表
chkconfig --list

# 添加服务
chkconfig --add redis

# 配置启动级别
chkconfig --level 2345 redis on
```

## 测试

```shell
service redis start   #或者 /etc/init.d/redis start  
service redis stop   #或者 /etc/init.d/redis stop

# 查看redis进程
ps -el|grep redis

# 端口查看
netstat -an|grep 3306
```

## 防火墙配置

```shell
# 打开防火墙规则
vim /etc/sysconfig/iptables

# 增加内容
-A INPUT -m state --state NEW -m tcp -p tcp --dport 6379 -j ACCEPT

# 重启火墙规则立即生效
service iptables restart 
```

