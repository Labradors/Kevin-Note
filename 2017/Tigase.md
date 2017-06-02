# Tigase

单台并发50w

5）缺点：集群方式复杂--但是tigase 作者维护很活跃，集群测试结果能够支撑比较大的容量，这是吸引人的地方。但经过实际生产运营情况来看，由于其集群方案实现的复杂性，以及单节点容量的有限，对支撑到 50 万用户在集群节点上可能没有信心，所以在到达 50 万用户之前，自己要准备替代方案。 

6）扩展：可以创建一些自定义的扩展，来处理特定的业务逻辑。 

7）tigase官方网站吸引人的地方：http://www.tigase.org/ 
Cluster with over 1mln online users.--  集群支持一亿个用户在并发 
500k online users on a single machine.--50万个用户并发单机 
Great at XMPP service sharding. 
Running happily with 10MB of RAM. 
Long list of supported features. 
Scripting support for all major languages. 
All tests results are publicly available. 
Very flexible and extensible API. 
Monitoring via HTTP, SNMP, JMX, XMPP.

Zoosk，是一个具有5000万会员的浪漫的社交约会网站 

## 连接webui

bosh地址: ws://localhost:5290/bosh

ws://localhost:5290/

bosh://localhost:5280/

- 5222, 5223 - default XMPP socket ports
- 5280 - BOSH connections
- 5290 - WebSocket connections
- 5269 - s2s/federation port
- 5277 - cluster connections
- 9050 - JMX port
- ​


## 编译运行

```
mvn -Pdist -f modules/master/pom.xml clean install
```

## 找不到数据库

```
tigase.db.DBInitException: Problem initializing jdbc connection: jdbc:mysql://127.0.0.1:3306/tigasedb?user=root&password=123456
```

## 消息格式

```xml
<message id="7boj2-135" type="chat" xmlns="jabber:client" from="测试群一@muc-group.dc-a4b8eb92-xmpp.jiangtao.tech." to="ceshi2@dc-a4b8eb92-xmpp.jiangtao.tech.">
  <body>放反</body>
  <thread>c87dc2cc-2436-4e7d-9699-d75c7f082f78</thread>
  <data_type xmlns="data:extension"><type>text</type></data_type><message_type xmlns="message:extension"><type>groupChat</type></message_type>
  <message-sender xmlns="sender:extension"><value>测试三@dc-a4b8eb92-xmpp.jiangtao.tech./5e93813a-7992-4d43-a9e1-a3ad9ef81669</value></message-sender>
</message>
```

## mysql源码安装

```
问题1:
mysqld_safe Directory '/var/lib/mysql' for UNIX socket file don't exists.
The server quit without updating PID file (/var/lib/mysql/l[FAILED].localdomain.pid)
```

