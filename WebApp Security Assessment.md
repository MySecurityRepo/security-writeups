# WebApp Security Assessment

During the development and deployment of a web application i considered a great learning opportunity to address its security on my own.
In the following there are some consideration and steps that i took in order to provide myself the security level i was striving for.


****************************1 Architecture****************************

The Web Application we are talking about is essentially a blog/social network.

The Application Architecture is constitued by:
- A Database: the database is where all the data is stored. The secure set up for this service is to not be exposed on external IP interfaces, have strong authentication mechanism (good user/password combination, default users disabled), and possiblly follow the principle of least privilege (for which db users should be able to perform only actions strictly required to them).  
- A Backend API Layer: This is the level which communicates with both the db and the frontend interface and where resides the application logic. This service like the DB one, should run on internal IP interfaces only (loopback address), otherwise it would be exposed on the internet and attackers would be able to bypass the webserver logic to directly make Requests to the API endpoints. 
- A Frontend Application Layer: This is the layer where the user interacts with the application. It communicates with the frontend through REST API. Security wise request coming from the frontend level should be treated as potentially malicious.
- A Webserver: This service has the role to serve applications resources. In our case it would serve the frontend static applications files (when requested through the browser). It is also this service which routes the requests made by the frontend to the correct API endpoints, acting as a reverse proxy. This service is also capable to enhance security, enforcing the https protocol for all the requests and through the set up of specific HTTP headers.
- A CDN (Content Delivery Network): The role of a CDN is to speed up the connection through specific configurations and to efficiently cache content. This caching capability is very useful as it avoids to be requested for all the content the CDN has cached, thus diminishing the amount of requests being made to our origin. It can also enhance security through the use of a WAF (Web Application Firewall). Lastly we have to specify that the CDN routes all the requests made to our domain to the webserver, thus acting as a first reverse proxy.

So in this case the request flow is:

![diagram](images/Diagram1.png)





****************************1 CDN****************************

We will not go through the secure configuration of the CDN, because is CDN specific and it has to be done on the CDN site. However, i'd suggest to not skip that part as it provides another layer of security to internet facing services.

A point that is worth mentioning is that, if we configured the CDN with DNS records pointing to our domain, when a request is made the CDN will process it then it will redirect all the requests made to our domain to our origin (IP address of our server). But if someone already knows the actual IP address of our server, it will be able to direclty make requests to it, thus bypassing the CDN service and all its protection mechanisms.


****************************2 Server Configuration****************************

With server configuration we mean the configuration of the machine hosting the website/application...to not be confused with a web server service (like nginx, apache, IIS) which performs the actions explained above. To secure the remote server i did the following:
- Closed all the ports, on external interfaces, except the ports i needed, through firewall rules.
- Hardened the ssh configuration.
- Limited access to sensible folders only to the corresponding user.
- Applied the principle of least privilege to users and running services (for example the user running the webserver shouldn't be the same user which runs the db service, and they should be both non-privileged users, for example without sudo privileges).
- Configured service which don't need to be exposed on the internet to run only on internal ports.
- Applied full disk encryption; this is a security measure against physical theft.
- Set up AV scanning for the upload folder, as a defense in depth measure against known malware.

Moving forward other possible measure that can be taken (but which are out of scope initially given the app dimension) are:

- Implementation of IPS/IDS for traffic analysis.
- Implementation of log monitoring systems and SIEM.
- Implementation of other in depth measures (ex: YARA...).





****************************3 Web Server Configuration****************************

These are some security configurations i enforced on the webserver:
- Chosen only modern SSL protocols like TLSv1.2, TLSv1.3: this disable insicure and cryptographically weak versions of the protocol.
- Applied rate limiting: We can limit the requests made per seconds from a certain IP; this is a line of defense against brute forcing and certain types of DOS attacks.
- Redirected to port 443 over https all the request made to port 80 over http.
- Added HTTP headers; the right HTTP headers enable us to enhance security disallowing certain types of attacks (like XXS or clickjacking). Examples of such headers are: X-Content-Type-Options, Content-Security-Policy, X-Frame-Options, Referrer-Policy, Strict-Transport-Security.
- Limit requests size to disallow certain types of DOS attacks.

In the following instead of focusing on the measures we have implemented respectively on the frontend and the backend, we will do a summary of the principal types of vulnerabilities, and explain how we are defending against them.

****************************4 Vulnerability Types****************************


*********************4.1 SQL Injection*********************
A safe setup against SQL injection attacks is the use of spefic libraries, considered safe for that purpose, in the language you are using. For example, in my case i was using Python. A database library that is considered safe in python is SQLAlchemy. This is because when you pass a parameter in a SQLAlchemy operation, it will build a query passing a placeholder to it; then it will pass the parameter value at execution time as a string.











