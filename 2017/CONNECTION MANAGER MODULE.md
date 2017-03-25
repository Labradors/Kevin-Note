# CONNECTION MANAGER MODULE

> Note:这是翻译版本，英语不好，有不正确之处，请指点。[官方网页](http://igniterealtime.org/projects/openfire/connection_manager.jsp)
>
> 时间: 2016-11-16
>
> 标签: openfire

##### What is it?

Each Openfire Connection Manager module you deploy improves the scalability of your Openfire server by handling a portion of the client connections. It's suitable for very large installations of Openfire (many thousand concurrent users). Download the connection manager module implementation below.

##### How many users can each connection manager handle?

Each connection manager should handle at least five thousand concurrent users. Experimental support for non-blocking connections is under development, which will greatly increase the number of connections that each connection manager module can support.

##### Can connection managers be used with other servers?

Yes (in theory). The connection manager protocol is being developed through the open [XEP](http://www.xmpp.org/extensions/) process. We hope to work with other server vendors so that the entire XMPP/Jabber community supports the protocol. We also expect there will be other implementations of connection managers that will be compatible with Openfire. For example, a connection manager written with native code may be able to achieve very high scalability on a specific platform.

##### Does Openfire Connection Manager Module use the same license as Openfire?

Yes, the module is dual-licensed under the Open Source [GPL](http://www.gnu.org/copyleft/gpl.html) license or a comercial software license agreement available from Jive Software.

##### How do I get support?

Support is available from the user community in the [discussion forums](http://community.igniterealtime.org/main-threads.jspa). Enterprise-grade email and phone support is also [available](http://www.jivesoftware.com/products/openfire?source=Website-Ignite) from Jive Software.

##### Download

Download the release of the Openfire connection manager module (requires Openfire 3.0 ). Full setup and usage instructions are included in the release.

#### image

![](http://igniterealtime.org/images/connection-managers.gif)