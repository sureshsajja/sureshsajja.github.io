---
layout: post
title: Redirect fom Apache HTTP server to Jetty
description: How to redirect requests from Apache HTTP server to Jetty, Configure virtual host with rewrite rules, redirect with mod_rewrite, mod_proxy, Changing Apache HTTP server port, Redirect from Apache to tomcat
comments: true
categories:
- Aache HTTP
tags:
- Apache HTTP
- Jetty
author: sureshsajja
---

This post demonstrates how to redirect requests from apache HTTP server to Jetty. These steps are independent of any application server and can be used for Tomcat.

Real-world websites use apache HTTP server to serve static content because

* Apache server is faster than Jetty/Tomcat when it comes to serving static pages.
* Apache server is robust and configurable to support many modules such as perl,PHP.

We would like Apache HTTP Server to serve HTML pages, Images to clients and forward other requests for dynamic content to underlying application server.

How that redirection happens?
More specifically, we will get answers to the following questions in the post by taking jetty as our application server

1. How will Apache know which request / type of requests should be forwarded to Jetty?
2. How will Apache forward these requests to Jetty?
3. How will Jetty accept and handle these requests?

### To Run a service with Jetty

I have used [dropwizard](http://www.dropwizard.io/1.0.0/docs/index.html) to developed a simple Hello World web service.

Code for this Hello Rest service can be found [here](https://github.com/sureshsajja/HelloRest).

After cloning the repository, execute `mvn clean install` to build target jar. 
Once target jar is ready, execute `java -jar hello-1.0-SNAPSHOT.jar server` to start service on 8080 port.

Our resource is located at /hello. Execute `http://localhost:8080/hello` to make sure our service is working
 
### Configure Apache server

Get the latest binary for Apache HTTP Server from [here](https://httpd.apache.org/). I have taken Apache 2.4 from [here](https://www.apachelounge.com/download/VC14/binaries/httpd-2.4.23-win64-VC14.zip)
Extracted zip in C drive by making APACHE_HOME located at `C:\Apache24`
Executed httpd.exe in bin directory to make sure, apache HTTP server is running at `http://localhost:9999/`
I had to change the port because of permission. you can change the port by editing `httpd.conf` in `C:\Apache24\conf`

we will use apache Module mod_rewrite to one URL to another URL. 
We will also enable virtual hosts where we will write redirection rules.
We can still do it without vhosts, but it will be a mess in `httpd.conf`

Make the following changes in `httpd.conf` to

* Enable mod_rewrite, Look for this `LoadModule rewrite_module modules/mod_rewrite.so` and uncomment it.
* Enable mod_proxy by uncommenting `LoadModule proxy_module modules/mod_proxy.so`
* Enable mod_proxy_http by uncommenting `LoadModule proxy_http_module modules/mod_proxy_http.so`
* Enable vhosts in by uncommenting `Include conf/extra/httpd-vhosts.conf`

comment any pre-configured virtual hosts in `conf/extra/httpd-vhosts.conf` Add the following virtual host snippet

```
<VirtualHost *:9999>
	DocumentRoot "C:/Apache24/htdocs"
    ErrorLog logs/helloworld-error.log
    CustomLog logs/helloworld-access.log combined
	LogLevel error
	<Directory "C:/Apache24/htdocs">
        Options MultiViews FollowSymLinks
        AllowOverride all
        Require all granted
    </Directory>
    RewriteEngine On
    RewriteRule ^/hello http://localhost:8080/hello [P]
</VirtualHost>
```

Our `httpd-vhosts.conf` will contain only the above VirtualHost directive.

Using above VirtualHost directive, 

* we are starting server instance at port 9999.
* RewriteEngine used to enable runtime rewrite engine on or off.
* RewriteRule is quite simple, It maps request `http://localhost:9999/hello` to `http://localhost:8080/hello`
* Use of the [P] flag causes the request to be handled by mod_proxy, and handled via a proxy request.

At this point, if you restart Apache HTTP server, it shouldn't be throwing any errors. It should work normal.

When you access `http://localhost:9999/hello` you should be presented with JSON message same as the message when you hit `http://localhost:8080/hello`

Read [here](http://httpd.apache.org/docs/current/mod/mod_rewrite.html) for more about mod_rewrite
Read [here](http://httpd.apache.org/docs/current/rewrite/intro.html) for more about regular expressions in mod_rewrite

That is it for this post. In the next post I will talk about how to configure SSL to Apache server.
