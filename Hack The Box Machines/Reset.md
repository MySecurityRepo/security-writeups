******Reset******

Reset is an HTB Linux machine classified as Easy.

*******1 Service Enumeration*******

As always, approaching this machine i did a full TCP scan.

![diagram](../images/Reset/Reset_nmap.png)

The scan result shows, beside ssh and http, three interesting services: rsh, rlogin and rexec. I tried for a while to check misconfigurations on those, but i couldn't find anything. 
Then i moved to the http web server.

*******2 Foothold*******

Navigating to the target we can see an admin login portal:

![diagram](../images/Reset/Reset_admin_portal.png)

Trying the some users with the change password functionality, we can find the user *admin*.

![diagram](../images/Reset/Reset_rest_password.png)

While when trying a not registered user, we get an error message:

![diagram](../images/Reset/Reset_wrong_user.png)

Examining the response we can see that the new password value is sent in the response body:

![diagram](../images/Reset/Reset_rest_response.png)

So now we have the credentials to access the web appplication. From within we can see that we are provided with the functionality to view logs:

![diagram](../images/Reset/Reset_logs2.png)

Even though only two options are given from the frontend we can always try to include differenet files, crafting our requests. 
I tried to include /etc/passwd but it doesn't work.
So noticing that the webserver is running apache We can try to include apache logs:

![diagram](../images/Reset/Reset_apche_logs.png)

![diagram](../images/Reset/Reset_apche_logs2.png)

We see that we could successfully include apache logs. So now we should try *Log Poisoning*.
First we send a request with the payload in the user agent:

![diagram](../images/Reset/Reset_logs_poisoning.png)

Then we make a request to the access.log again:

![diagram](../images/Reset/Reset_logs_poisoning2.png)

Form the response we see that the *id* command is correctly executed.
So we can use the following request to get a reverse shell:

![diagram](../images/Reset/Reset_logs_poisoning3.png)

![diagram](../images/Reset/Reset_shell.png)

*******3 Privilege Escalation*******

