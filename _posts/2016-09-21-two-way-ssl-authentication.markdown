---
layout: post
title: Two Way SSL Authentication
description: How to Authenticate clients using certificates, How to configure two way SSL authentication, Mutual SSL Authentication, How to create client certificate
comments: true
categories:
- Aache HTTP
tags:
- Apache HTTP
- SSL
author: sureshsajja
---

This post demonstrate how to authenticate clients using Certificates. In another words, this post demonstrates the mutual SSL authentication.

Suppose, we have a service running in an application server and we wanted to expose it to outside world through Apache HTTP Server using two way SSL authentication. We would like to do the following

* Redirect requests from Apache HTTP Server to underlying Tomcat/Jetty Servers. This has been discussed [here](http://coderevisited.com/redirect-from-apache-http-server-to-jetty/)
* Enable SSL for Apache HTTP Server. This has been discussed [here](http://coderevisited.com/configure-ssl-on-apache/)
* How to set up two way SSL Authentication. I will discuss this in this post.


### What is Mutual SSL Authentication

Mutual SSL authentication or certificate based mutual authentication refers to two parties authenticating each other through verifying the provided digital certificate so that both parties are assured of the others' identity.

In technology terms, it refers to a client (web browser or client application) authenticating themselves to a server (website or server application) and that server also authenticating itself to the client through verifying the certificate issued by the trusted Certificate Authorities (CAs)

From a high-level point of view, the process of authenticating and establishing an encrypted channel using certificate-based mutual authentication involves the following steps:

* A client requests access to a protected resource.
* The server presents its certificate to the client.
* The client verifies the server’s certificate based on the root certificate authority.  
* If successful, the client sends its certificate to the server.
* The server verifies the client’s credentials based on the configured certificate authority. 
* If successful, the server grants access to the protected resource requested by the client.

![Mutual SSL](http://www.codeproject.com/KB/IP/326574/mutualssl.png)

Assuming that our service is ready to accept client requests over SSL. It has been discussed [here](http://coderevisited.com/configure-ssl-on-apache/)

We will see how to generate how to create client certificate and configure server to accept requests based on client authetication.

### Creating client certificate

At Client Side

* Create Client Private Key

```
openssl genrsa -out client.key 2048
```

* Generate CSR which we will send to CA for signing

```
openssl req -new -key client.key -out client.csr
```

![Client CSR](/images/ClientCSR.jpg)

We use the root certificate authority created [here](http://coderevisited.com/configure-ssl-on-apache/) to generate Client certificates.

* Signing CSR

```
openssl x509 -req -in client.csr -CA rootCA.pem -CAkey rootCA.key -CAserial rootCA.srl -out client.crt -
days 500 -sha256
```

![CSR Sign](/images/ClientSign.jpg)

client.crt will be sent to client. Client will use this and generate pkcs12 for use within a browser

```
openssl pkcs12 -export -in client.crt -inkey client.key -out client.p12
```

### Importing Client certificate to a browser

In Windows machine, Go to IE, Internet Options, go to the Content tab, then hit the Certificates button. This will take you to the the Windows certificate repository.
Import the rootCA.pem (not the key) under Personal.

After certificate import, if we hit our service running at `https://localhost/hello`, browser will shows us list of available certificates for use.
If we select appropriate certificate, we get response from the service.

![Certificate Select](/images/CertSelect.jpg)

We can on board as many clients we wanted this way. But what if client wants to use their own certificates. How can server accommodate this? We will see next.

### References

http://www.codeproject.com/Articles/326574/An-Introduction-to-Mutual-SSL-Authentication
