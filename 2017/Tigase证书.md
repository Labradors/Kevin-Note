## Tigase证书

### 在pem文件中创建和加载服务证书

- 如果在socket连接中想使用安全的SSL/TLS连接。服务器证书是必须的。
- 你可以自己生成自己的服务器证书或者使用第三方受信任机构的证书。
- 下面的步骤是如何获取受信任组织的第三方证书。

> 在你获取或者生成证书之前，你需要一个私有的key来生成证书请求。
>
> 还包含域名的相关信息等等。
>
> - 生成一个证书请求:
>
> - `openssl req -nodes -new -newkey rsa:2048 -keyout yourdomain.com.key -out yourdomain.com.csr`
>
> - 填写需要的信息后，将会生成2个文件。
>
>   1. yourdomain.com.csr
>   2. yourdomain.com.key
>
>   5.0版本之后，tigase会根据你的服务器自动生成自建证书



> 来自第三方的证书。
>
> 有很多免费或者收费的证书提供商。但是有一些新的提供商，别的xmpp不能识别。
>
> - [CAcert](https://www.cacert.org/) - free certificates with an excellent Web GUI for managing generated certificates and identities.
> - [StartCom](https://www.startssl.com/) - both free and paid certificates, class 1, 2 and 3. Very good GUI for managing certificates and identities.
> - [Verisign](https://www.verisign.com/) - very expensive certificates comparing to above provides but the provider is recognized by everybody. If you have a certificate from Verisign you can be sure it is identified as a valid certificate.
> - [Comodo Certificate Authority](http://www.comodo.com/business-security/digital-certificates/ssl-certificates.php) offers different kind of commercial certificates
>
> 

### 从tigase server中安装和加载证书

- 对于自建证书，因为没有证书链，所有生成.pem文件是给非常容易的。

- `cat yourdomain.com.crt yourdomain.com.key > yourdomain.com.pem`

- 如果证书是由第三方授权的，您还要附加证书链，即生成证书的权限的证书,您通常需要从生成证书的授权机构获取您的链条的证书.例如，您有XMPP联盟的证书，您需要下载StartCom根证书和中间ICA证书。在这种情况下，使用以下命令创建pem文件:

- ```
  cat yourdomain.com.crt yourdomain.com.key sub.class1.xmpp.ca.crt ca.crt > yourdomain.com.pem
  ```



### 在你的Linux系统中安装StartCom证书

- 免费XMPP服务器证书的第三方权限是Startcom。您的系统通常不知道Startcom根证书作为有效证书并显示为自签名证书。

- 要使系统知道您有必要将其安装在系统中

- 在任何情况下或任何操作系统中，您必须从发行者网站下载证书。[web site](http://cert.startcom.org/?lang=en&app=110)

- Copy downloaded ca.crt file to /etc/ssl/certs/starcom_ca.crt file.

- Run command:

  ```
  update-ca-certificates
  ```

All done. To test it run following command:

```
openssl s_client -connect tigase.org:5223 -CApath /etc/ssl/certs
```

Scroll the output up and look for something like:

```
verify return:1
```

Which means certificate verification was successful. If you find however:

```
verify return:0
```

Look one line up for an error message which may look like this:

```
verify error:num=19:self signed certificate in certificate chain
```

Which means the root certificate is still not recognized in your system.



### 使用Keytool和Keystore的服务器证书



#### 自签名证书

* 有些客户端不能正确使用DSA密钥，所以我们需要使用RSA算法。要生成私钥和公钥密钥对，你应该使用keytool：
* `keytool -genkey -alias yourdomain -keystore rsa-keystore \    -keyalg RSA -sigalg MD5withRSA`
* 您的域是Jabber / XMPP服务器上JID的域的一部分。如果您想要为虚拟域提供TLS支持，则必须为每个虚拟域创建证书。
* 如果您只有一个域，或者由于某种原因，您必须为所有域使用一个证书，请使用默认值作为别名。
* Now you have to copy file rsa-keystore to directory certs/ in your tigase server installation. The file could also be installed in different location but then corrections to config file are required. Refer to configuration documentation for details.
* ​

#### CA证书

* 如果您还没有第三方签署的证书，但是您想要有一个可以免费从cacert.org获取的证书。

* ```
  keytool -certreq -alias yourdomain -keystore rsa-keystore
  ```

* 其中yourdomain是生成此证书的域名。,如果您需要多个虚拟域的支持，则需要分别为每个域配置证书，并为证书分配适当的别名.如果您只有一个域，或者由于某种原因，您必须为所有域使用一个证书，请使用默认值作为别名。

* 现在将请求发送到您的CA. CA颁发签名证书并将其发送给您。它可能看起来像：

* 您应该将证书保存到磁盘文件。

  >​

  - 如果您已经拥有第三方定罪证书，则必须使用keytool程序将其导入服务器证书存储。

  - 注意！您必须拥有根CA证书！您可以从CA下载证书（即：root.crt）并导入：

  - ```
    keytool -import -keystore rsa-keystore -file root.crt \
        -alias root
    ```

  - 最后一步是将证书导入您的密钥库：

  - ```
    keytool -import -alias yourdomain -keystore rsa-keystore \
        -file your-certificate.cer
    ```

  - Where yourdomain is a domain name for which this certificate has been generated.