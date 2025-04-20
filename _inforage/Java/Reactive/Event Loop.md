![](../../../_res/Pasted%20image%2020250211122601.jpg)


Above is an abstract design of an _event loop_ that presents the ideas of reactive asynchronous programming:

- **The _event loop_ runs continuously in a single thread**, although we can have as many _event loops_ as the number of available cores.
- **The _event loop_ processes the events from an _event queue_ sequentially and returns immediately** after registering the _callback_ with the _platform._
- The _platform_ can trigger the completion of an operation, like a database call or an external service invocation.
- **The _event loop_ can trigger the _callback_ on the _operation completion_ notification and send back the result to the original caller.**

The _event loop_ _model_ is implemented in a number of platforms, including _[Node.js](https://nodejs.org/en/)_, _[Netty](https://netty.io/)_, and _[Ngnix](https://www.nginx.com/)_. They offer much better scalability than traditional platforms, like _[Apache HTTP Server](https://httpd.apache.org/)_, _[Tomcat](https://www.baeldung.com/tomcat)_, or _[JBoss](https://www.redhat.com/fr/technologies/jboss-middleware/application-platform)_.


