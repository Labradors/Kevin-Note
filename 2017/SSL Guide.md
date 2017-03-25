# SSL Guide

> Note:这是翻译版本，英语不好，有不正确之处，请指点。[官方网页](http://igniterealtime.org/projects/openfire/connection_manager.jsp)
>
> 时间: 2016-11-16
>
> 标签: openfire

## Introduction

This document outlines how to **manually** customize the SSL support in Openfire. As of Openfire 3.2 certificate management can be performed from the Admin Console. However, if needed you can still manually manage certificates using JDK 1.5 tools. **Important note:** Once the setup process is completed Openfire will create self-signed certificates for the assigned Openfire's domain. Most users should either get the created certificates signed by a Certificate Authority or replace the created certificates with your own certificates.

Openfire's SSL support is built using the standard Java security SSL implementation (javax.net.ssl.SSLServerSocket). In this document, we will describe how use the standard JDK 1.5 tools to accomplish these tasks.



## Background

A server SSL connection uses two sets of certificates to secure the connection. The first set is called a "keystore". The keystore contains the keys and certificates for the server. These security credentials are used to prove to clients that the server is legitimately operating on behalf of a particular domain. If your server will only need to act as one domain, you only need one key entry and certificate in the keystore. Keys are stored in the keystore under aliases. Each alias corresponds to a domain name (e.g. "example.com").

The second set of certificates is called the "truststore" and is used to verify that a client is legitimately operating on behalf of a particular user. In the vast majority of cases, the truststore is empty and the server will not attempt to validate client connections using SSL. Instead, the XMPP authentication process verifies users in-band. However, you may wish to require SSL authentication for certain clients when security is especially important and the number of clients connection to the server is relatively small.

Certificates attempt to guarantee that a particular party is who they claim to be. Certificates are trusted based on who signed the certificate. If you only require light security, are deploying for internal use on trusted networks, etc. you can use "self-signed" certificates. Self-signed certificates encrypts the communication channel between client and server. However the client must verify the legitimacy of the self-signed certificate through some other channel. The most common client reaction to a self-signed certificate is to ask the user whether to trust the certificate, or to silently trust the certificate is legitimate. Unfortunately, blindly accepting self-signed certificates opens up the system to 'man-in-the-middle' attacks.

The advantage of a self-signed certificate is you can create them for free which is great when cost is a major concern, or for testing and evaluation. In addition, you can safely use a self-signed certificate if you can verify that the certificate you're using is legitimate. So if a system administrator creates a self-signed certificate, then personally installs it on a client's truststore (so that the certificate is trusted) you can be assured that the SSL connection will only work between the client and the correct server.

For higher security deployments, you should get your certificate signed by a certificate authority (CA). Clients truststores will usually contain the certificates of the major CA's and can verify that a CA has signed a certificate. This chain of trust allows clients to trust certificate from servers they've never interacted with before. Certificate signing is similar to a public notary (with equivalent amounts of verification of identity, record keeping, and costs).

## Sun JDK 1.5 security tools

The Sun JDK (version 1.5.x) ships with all the security tools you need to configure SSL with Openfire. The most important is the keytool located in the JAVA_HOME/bin directory of the JDK. Sun JVMs persist keystores and truststores on the filesystem as encrypted files. The keytool is used to create, read, update, and delete entries in these files. Openfire ships with a self-signed "dummy" certificate designed for initial evaluation testing. You will need to adjust the default configuration for most deployments.

In order to configure SSL on your server you need complete the following tasks:

1. Decide on your Openfire server's domain.
2. Create a self-signed SSL server certificate for your server domain. Note: you may already have one if your Openfire server domain matches an existing web domain with SSL. If so, you can skip to step 4.
3. [Optional] Have a certificate authority (CA) certify the SSL server certificate.
   1. Generate a certificate signing request (CSR).
   2. Submit your CSR to a CA for signing.
4. Import the server certificate into the keystore. Note: if you are going to use a self-signed certificate generated in step 2, the certificate is already imported and you can skip this step.
5. Remove default certificates from the keystore.
6. Import client certificates into the truststore.
7. Adjust the Openfire configuration with proper keystore and truststore settings.

### 1. Decide on a Server Domain

The Openfire server domain should match the host name of the server; for example, "example.com". Your user accounts will have addresses with the format "user@example.com" like email addresses. We'll assume the domain is "example.com" for the rest of the examples.

### 2. Create a self-signed server certificate

In order to create a self-signed server certificate go to the command line and change directories to the resources/security directory of your Openfire installation. You should see the default keystore and truststore files. First, you should change the default keystore password:

`keytool -storepasswd -keystore keystore`

keytool will ask for the old password (by default it is changeit) then the new password. Now we'll create a certificate using the keytool:

`keytool -genkey -keystore keystore -alias example.com`

where you should substitute your server's name for example.com. The keytool will ask for the store password, then several pieces of information required for the certificate. Enter all the information but remember to **complete with your server's name when asked for your first and last name**. After you have entered all the required information, keytool will ask you to verify the information and set a key password. **You must use the same key password as the store password.** By default you get this by simply hitting 'enter' when prompted for a key password.

If you later change the keystore password remember to change the entries' password as well using the keytool:

`keytool -keypasswd -alias example.com -keystore keystore`

Keytool will create certificates using the DSA algorithm by default. Some clients expect the server to have RSA certificates or they will fail to use TLS/SSL. Therefore, it is a good idea to also create RSA certificates in your keystore. To create certificates with the RSA algorithm you need to specify the algorithm to use like this:

`keytool -genkey -keystore keystore -alias example.com -keyalg RSA`

### 3. Obtain a CA signed certificate

If you decide to get a CA signed certificate, you must first export the certificate in the standard CSR format. You can do this with the keytool:

`keytool -certreq -keystore keystore -alias example.com -file certificate_file`

Where you should substitute your server's name for example.com and the name of the certificate file you wish to produce for certificate_file. Submit the generated CSR to the CA and follow their instructions to get it signed.

### 4. Import server certificates

If you had a CA sign your server certificate, or if you have an existing SSL certificate, you must import it using the keytool.

`keytool -import -keystore keystore -alias example.com -file signed_certificate_file`

It is important that the alias not already have an associated key or you'll receive an error.

### 5. Remove default certificates

After importing your certificate you must remove the default certificates using the keytool.

`keytool -delete -keystore keystore -alias rsa`

`keytool -delete -keystore keystore -alias dsa`

### 6. Import client certificates

If you require clients to verify themselves using certificates, obtain their certificates and import them into the truststore file rather than the keystore. First, you should change the default truststore password:

`keytool -storepasswd -keystore truststore`

keytool will ask for the old password (by default it is changeit) then the new password. Now import each certificate using the keytool:

`keytool -import -keystore truststore -alias user_name -file certificate_file`

### 7. Configure Openfire

Open the Openfire Admin Console in your favorite browser and add or change the following system properties:

- `xmpp.socket.ssl.active` -- set to 'true' to active SSL
- `xmpp.socket.ssl.port` -- the port to use for SSL (default is 5223 for XMPP)
- `xmpp.socket.ssl.storeType` -- the store type used ("JKS" is the Sun Java Keystore format used by the JDK keytool). If this property is not defined, Openfire will assume a value of "jks".
- `xmpp.socket.ssl.keystore` -- the location of the keystore file relative to your Openfire installation root directory. You can leave this property blank to use the default keystore.
- `xmpp.socket.ssl.keypass` -- the keystore/key password you changed in step 2.
- `xmpp.socket.ssl.truststore` -- leave blank to not use a truststore, otherwise the location of the truststore file relative to your Openfire installation root directory.
- `xmpp.socket.ssl.trustpass` -- the truststore/key password you changed in step 6.

You will need to restart the server after you have modified any of the above system properties.

