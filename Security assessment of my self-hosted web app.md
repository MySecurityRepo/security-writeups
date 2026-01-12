During the development and deployment of a web application i considered a great learning opportunity to address its security on my own.
In the following there are some consideration and steps that i took in order to provide myself the security level i was striving for.

The Web Application Architecture is essentially constitued by:
- A Database
- A Backend application layer
- A Frontend application layer
- A Webserver; having also a role of a reverse proxy
- A CDN; this too having also the role of a reverse proxy

So the request flow, would be:

[ Untrusted Zone ]

   Browser
   
      
      v
      
     CDN
     
      |
      v
      
-----------------------------  ← trust boundary

[ Partially Trusted Zone ]
   Web Server
      |
      v
Backend Application
      |
      v
-----------------------------  ← strongest trust boundary
[ Trusted Zone ]
     Database




