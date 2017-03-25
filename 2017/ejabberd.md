# ejabberd

## ejabberd介绍

>  If deployed on a 16 GB RAM machine with at least 4 cores, a single ejabberd node can typically handle 200-300 K online users. This setup is suitable for systems with up to 10 nodes.

> If the service requires a cluster of more than 10 nodes, we recommend not relying on Mnesia clusting mode. Many solutions are available, the easiest and more inexpensive being to rely on [ejabberd Software-as-a-Service](http://www.process-one.net/en/ejabberd/saas/) approach.

> - Jingle, XMPP based voice protocol
> - SIP (Session Initiation Protocol): Yes, you can pass SIP calls using ejabberd :)
> - ICE (Interactive Connectivity Establishment: A Protocol for Network Address Translator (NAT) Traversal)
> - STUN
> - TURN
> - Proxy65 media relay
>
> This makes ejabberd the best XMPP server to support SIP and WebRTC based communication tools.

## 面向管理员

>  *cross-platform*
>
>  *Distributed*
>
>  Fault-tolerant
>
>  *Administrator Friendly*
>
>  - Comprehensive documentation.
>  - Straightforward installers for Linux, Mac OS X, and Windows.
>  - Web Administration.
>  - Shared Roster Groups.
>  - Command line administration tool.
>  - Can integrate with existing authentication mechanisms.
>  - Capability to send announce messages.
>
>  Modular
>
>  - Load only the modules you want.
>  - Extend `ejabberd` with your own custom modules.
>
>  Security
>
>  - SASL and STARTTLS for c2s and s2s connections.
>  - STARTTLS and Dialback s2s connections.
>  - Web Admin accessible via HTTPS secure access.
>
>  Databases
>
>  - Internal database for fast deployment (Mnesia).
>  - Native MySQL support.
>  - Native PostgreSQL support.
>  - ODBC data storage support.
>  - Microsoft SQL Server support.
>  - SQLite support.
>  - Riak NoSQL database support.
>
>  Authentication
>
>  - Internal Authentication.
>  - PAM, LDAP, SQL and Riak.
>  - External Authentication script.
>
>  Others
>
>  - Support for virtual hosting.
>  - Compressing XML streams with Stream Compression ([`XEP-0138`](http://xmpp.org/extensions/xep-0138.html)).
>  - Statistics via Statistics Gathering ([`XEP-0039`](http://xmpp.org/extensions/xep-0039.html)).
>  - IPv6 support both for c2s and s2s connections.
>  - [`Multi-User Chat`](http://xmpp.org/extensions/xep-0045.html) module with support for clustering and HTML logging.
>  - Users Directory based on users vCards.
>  - [`Publish-Subscribe`](http://xmpp.org/extensions/xep-0060.html) component with support for [`Personal Eventing via Pubsub`](http://xmpp.org/extensions/xep-0163.html).
>  - Support for web clients: Support for [XMPP subprotocol for Websocket](https://tools.ietf.org/html/rfc7395) and [`HTTP Binding (BOSH)`](http://xmpp.org/extensions/xep-0206.html) services.
>  - IRC transport.
>  - SIP support.
>  - Component support: interface with networks such as AIM, ICQ and MSN installing special tranports.

**有关具体配置文件部分，自行查看官方文档**

## 面向开发者

后端解决方案: https://kamranzafar.org/2015/08/05/xmpp-web-chat-with-ejabberd-converse-js-and-spring-mvc/

移动端解决方案: smack

## 概要

## 用例

## 问题

