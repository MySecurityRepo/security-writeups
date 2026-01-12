****************************WebApp Security Assessment****************************

During the development and deployment of a web application i considered a great learning opportunity to address its security on my own.
In the following there are some consideration and steps that i took in order to provide myself the security level i was striving for.

****************************Architecture****************************

The Web Application we are talking about is essentially a blog/social network.

The Application Architecture is constitued by:
- A Database: the database is where all the data is stored. The secure set up for this service is to not be exposed on external IP interfaces, have strong authentication mechanism (good user/password combination, default users disabled), and possiblly follow the principle of least privilege (for which db users should be able to perform only actions strictly required to them).  
- A Backend API Layer: This is the level which communicates with both the db and the frontend interface and where resides the application logic. This service like the DB one, should run on internal IP interfaces only (loopback address), otherwise it would be exposed on the internet and attackers would be able to bypass the webserver logic to directly make Requests to the API endpoints. 
- A Frontend Application Layer: This is the layer where the user interacts with the application. It communicates with the frontend through REST API. Security wise request coming from the frontend level should be treated as potentially malicious.
- A Webserver: This service has the role to serve applications resources. In our case it would serve the frontend static applications files (when requested through the browser). It is also this service which routes the requests made by the frontend to the correct API endpoints, acting as a reverse proxy. This service is also capable to enhance security, enforcing the https protocol for all the requests and through the set up of specific HTTP headers.
- A CDN (Content Delivery Network): The role of a CDN is to speed up the connection through specific configurations and to efficiently cache content. This caching capability is very useful as it avoids to be requested for all the content the CDN has cached, thus diminishing the amount of requests being made to our origin. It can also enhance security through the use of a WAF (Web Application Firewall). Lastly we have to specify that the CDN routes all the requests made to our domain to the webserver, thus acting as a first reverse proxy.

So in this case the request flow is:

![diagram](Diagram1.png)




