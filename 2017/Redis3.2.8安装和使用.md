---
Redis3.2.8安装和使用
---

## Centos7安装

```shell
#安装gcc
yum -y install gcc
#下载redis
curl -O  http://download.redis.io/releases/redis-3.2.8.tar.gz
#解压
tar -zxvf redis-3.2.8.tar.gz
#转换目录
cd redis-3.2.8/deps/
#编译依赖
make geohash-int hiredis jemalloc linenoise lua
#转换目录
cd ..
#编译Redis
make && make install
#转换目录
cd utils/
#使用脚本安装服务
./install_server.sh
#启动服务
systemctl start redis_6379
systemctl status redis_6379
```

## String的使用

> 一、概述：
>
> ```xml
>   字符串类型是Redis中最为基础的数据存储类型，它在Redis中是二进制安全的，这便意味着该类型可以接受任何格式的数据，如JPEG图像数据或Json对象描述信息等。在Redis中字符串类型的Value最多可以容纳的数据长度是512M。
> ```

