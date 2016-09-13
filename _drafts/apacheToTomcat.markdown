This post demonstrates how to redirect requests from apache HTTP server to Jetty.

Real-world websites use apache HTTP server to serve static content because

* Apache server is faster than Jetty/Tomcat when it comes to serving static pages.
* Apache server is robust and configurable to support many modules such as perl,PHP.

We would like Apache HTTP Server to serve HTML pages, Images to clients and forward other requests for dynamic content to underlying application server.

How that redirection happens?
More specifically, we will get answers to the following questions in the post by taking jetty as our application server

1. How will Apache know which request / type of requests should be forwarded to Jetty?
2. How will Apache forward these requests to Jetty?
3. How will Jetty accept and handle these requests?

 
