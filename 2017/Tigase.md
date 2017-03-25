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

推库

