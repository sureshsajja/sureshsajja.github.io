---
layout: post
title: Configure SSL on Apache
description: How to create own root certificate authority, how to create SSL certificates for Apache Server, How to configure SSL Certificates on Apache, what is CAcreateserial.
comments: true
categories:
- Aache HTTP
- SSL
- HTTPS
tags:
- Apache HTTP
- SSL
- HTTPS
author: sureshsajja
---

This post demonstrates how to configure SSL on Apache HTTP Server. It demonstrates how to create SSL certificate authority to use on Apache.
 
### What is SSL?

SSL, stands for Secure Socket Layer, creates an encrypted connection between a web server and a client web browser allowing for private information to be transmitted without the problems of eavesdropping, data tampering, or message forgery.

All browsers have the capability to interact with secured web servers using the SSL protocol. However, the browser and the server need what is called an SSL Certificate to be able to establish a secure connection.
SSL Certificates have a key pair: a public and a private key. These keys work together to establish an encrypted connection. The certificate also contains what is called the “subject,” which is the identity of the certificate/website owner.

### How Does the SSL Certificate Create a Secure Connection?

When a browser attempts to access a website that is secured by SSL, the browser and the web server establish an SSL connection using a process called an `SSL Handshake`. Note that the SSL Handshake is invisible to the user and happens instantaneously.

* Browser connects to a web server (website) secured with SSL (https). Browser requests that the server identify itself.
* Server sends a copy of its SSL Certificate, including the server’s public key.
* Browser checks the certificate root against a list of trusted CAs and that the certificate is unexpired, unrevoked, and that its common name is valid for the website that it is connecting to. If the browser trusts the certificate, it creates, encrypts, and sends back a symmetric session key using the server’s public key.

At this point, If the server cannot be authenticated, the user is warned of the problem that an encrypted and authenticated connection cannot be established

* Server decrypts the symmetric session key using its private key and sends back an acknowledgement encrypted with the session key to start the encrypted session.
* Server and Browser now encrypt all transmitted data with the session key.

### How to get SSL Certificate

To get a certificate, we must create a Certificate Signing Request (CSR) on our server. This process creates a private key and public key on your server.
The CSR data file that we send to the SSL Certificate issuer (called a Certificate Authority or CA) contains the public key.
The CA uses the CSR data file to create a data structure to match our private key without compromising the key itself. Certificate authorities charges some nominal amount to issue these SSL certificates.

For our example, we are going to create our own certificate Authority to generate server certificates.

### Create Own SSL Certificate Authority

We use OpenSSL to create your own private certificate authority. The process for creating your own certificate authority

* Create a private key

```
openssl genrsa -des3 -out rootCA.key 2048
```

By using above command, we can generate private key with 2048 bit long. This is the basis of all trust for your certificates. Please keep the password safe, and if someone gets a hold of it, they can generate certificates that your browser will accept

* Self-sign

```
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
```

This will ask for various information. Once done, this will create an SSL certificate called rootCA.pem, signed by itself, valid for 1024 days, and it will act as our root certificate.
The interesting thing about traditional certificate authorities is that root certificate is also self-signed.

[![Self Sign](/images/SelfSign.jpg)

* Install root CA on your various workstations
In Windows machine, Go to IE, Internet Options, go to the Content tab, then hit the Certificates button. This will take you to the the Windows certificate repository.
Import the rootCA.pem (not the key) under the Trusted Root Certificate Authorities tab.

[![Certificate Import](/images/CertImport.jpg)

From above three steps, we have root certificate authority is ready and installed in our browser. Now we are going to create SSL certificate for our Apache HTTP Server 

### Create Server Certificate

* Create a private key

```
openssl genrsa -out server.key 2048
```

Creates private key for server

* certificate signing request

```
openssl req -new -key server.key -out server.csr
```

You’ll be asked various questions (Country, State/Province, etc.). Answer them how you see fit. The important question to answer though is common-name.

```
Common Name (e.g. server FQDN or YOUR name) []:localhost
```

Whatever you see in the address field in your browser when you go to your device must be what you put under common name, even if it’s an IP address.  Yes, even an IP (IPv4 or IPv6) address works under common name. If it doesn’t match, even a properly signed certificate will not validate correctly and you’ll get the “cannot verify authenticity” error.

Once CSR is ready, we need rootCA.key to sign this CSR

```
openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 500 -sha256
```

This creates a signed certificate called device.crt which is valid for 500 days

#### Note on CAcreateserial

The first time you use your CA to sign a certificate you can use the -CAcreateserial option.
This option will create a file (rootCA.srl) containing a serial number.
You are probably going to create more certificate, and the next time you will have to do that use the -CAserial option (and no more -CAcreateserial) followed with the name of the file containing your serial number.
This file will be incremented each time you sign a new certificate.
This serial number will be readable using a browser (once the certificate is imported to a pkcs12 format).
And we can have an idea of the number of certificate created by a CA.

### Configurations on Apache Server

* Enable SSL module by uncommenting `LoadModule ssl_module modules/mod_ssl.so` in conf/httpd.conf in Apache installation directory.
* Uncomment `LoadModule socache_shmcb_module modules/mod_socache_shmcb.so` This is required by SSL session cache.
* Uncomment `Include conf/extra/httpd-ssl.conf` to include SSL conf
* In `httpd-ssl.conf` file, set Server Name same as Common Name given in CSR

```
ServerName localhost:443
```

* Copy server.crt, server.key to conf directory of Apache
* Specify server.crt path against `SSLCertificateFile` in `httpd-ssl.conf`

```
SSLCertificateFile "c:/Apache24/conf/server.crt"
```

* Specify server.key path against `SSLCertificateKeyFile` in `httpd-ssl.conf`

```
SSLCertificateKeyFile "c:/Apache24/conf/server.key"
```

* Restart Apache Server. Our server is SSL ready.

If wanted to redirect request to any underlying services, we can add the required Rewrite rules in `httpd-ssl.conf` before `</VirtualHost>`

Refer to my [previous post](http://coderevisited.com/redirect-from-apache-http-server-to-jetty/) where i discussed about redirection from Apache Server to Jetty service. Using the same example, we have to the following rules in `httpd-ssl.conf`

```
RewriteEngine On
RewriteRule ^/hello http://localhost:8080/hello [P]
```

* Restart Apache Server, if we hit `https://localhost/hello`, it will get redirected to `http://localhost:8080/hello`

### FAQ

* `ERROR:` SSLSessionCache: 'shmcb' session cache not supported (known names: ). Maybe you need to load the appropriate socache mod
  ule (mod_socache_shmcb?).
 
  > Uncomment `LoadModule socache_shmcb_module modules/mod_socache_shmcb.so` in httpd.conf
  

### References
[Creating Your Own SSL Certificate Authority](https://datacenteroverlords.com/2012/03/01/creating-your-own-ssl-certificate-authority/)

