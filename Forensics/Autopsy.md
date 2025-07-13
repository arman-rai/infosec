# room/autopsy2ze0

## Apache log analysis
The most significant attack surface on the server is probably the web service; fortunately, the Apache access log keeps a history of all of the requests sent to the webserver and includes:
1. source address
2. response code and length
3. user-agent

Every request from a modern browser should contain a user-agent string that can be used to identify it roughly. The content of the user-agent string is then used to modify the experience by displaying the mobile version of a site or disabling features not supported on older browsers.  

  

We can also use this string to identify traffic from potentially malicious tools as scanning tools like Nmap, SQLmap, DirBuster and Nikto leak their identity's through default user-agent strings.

  

Answer the questions below