During the development and deployment of a web application i considered a great learning opportunity to address its security on my own.
In the following there are some consideration and steps that i took in order to provide myself the security level i was striving for.

****************************Architecture****************************

The Web Application is essentially a blog/social network.

The Application Architecture is essentially constitued by:
- A Database: the database is where all the data is stored. The secure set up for this service is to not be exposed on external IP interfaces, have strong authentication mechanism (good user/password combination, default users disabled), and possiblly follow the principle of least privilege (for which db users should be able to perform only actions strictly required to them).  
- A Backend API Layer: This is the level which communicates with both the db and the frontend interface and where resides the application logic.
- A Frontend Application Layer: This is the layer where the user interacts with the application. It communicates with the frontend through REST API.
- A Webserver: having also a role of a reverse proxy
- A CDN; this too having also the role of a reverse proxy

So in this case  the request flow is:





