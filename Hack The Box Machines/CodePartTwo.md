******CodePartTwo******

CodePartTwo is an HTB Linux machine classified as Easy.

*******1 Service Enumeration*******

I always feel safer if i can do an overall port scan. So if the process is fast enough i try it:

![diagram](../images/CodePartTwo_nmapscan1.png)

The result shows that for tcp only ports 22 and 8000 are open.

I tried to fingerprint the services.

![diagram](../images/CodePartTwo_nmapscan1.png)

This didn't give too much information, so at this point i visited the web service with the browser.

*******2 Searching the Foothold*******

On the web page we have the option to register, login or download the app source code.

So i downloaded the code and then i proceeded with registration and login.

Once signed in we are given the possibility to run code. At this point to have a better grasp of the attack angle i went to look at the app source code.

These are the principal components we can find:

![diagram](../images/CodePartTwo_appcontent.png)

Looking at app.py, this is the flask route which let's us run the code:

![diagram](../images/CodePartTwo_appcode.png)

We can see that the code submitted by us in the browser is posted on this route on the backend and then is run with the js2py.eval_js function:

