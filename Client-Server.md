# UNDERSTANDING CLIENT-SERVER ARCHITECTURE.
## CLIENT-SERVER
Client-Server refers to an architecture where two or more computers are connected to each other over a network to send and receive requests between one another.In this type of communication, each machine has its own role to play: the machine sending the requests is referred to as **Client** while the machine responding to the request is called **Server**.<br/>
Below is a diagram of a Web Client-Server architecture where a machine is trying to access a Website using Web browser or a simply 'curl' command as a client and it sends HTTP requests to a Web server (Apache. Nginx, IIS etc) over the internet. 





![image](https://github.com/Saidat23/devops.pbl/assets/138054715/14318d75-acd2-4dc7-b198-09bd8f63f4d7)

If the concept is extended further and a Database Server is added to the archtecture, the Web Server will take the role of a Client that connects and read/write to/from a Database Server ( Oracle, MYSQL, MongoDB, SQL Server) communicating over the Local Network. It can also be over the internet connection, but it is a common practice to have the Web Server and the Database Server in same local network.
The diagram below is a generic Web Stack architecture (LAMP,LEMP,MEAN,MERN) which can be implemented with many other technologies from small single page applications **SPA** to large and complex portals.

![image](https://github.com/Saidat23/devops.pbl/assets/138054715/02cfb999-32a2-40d8-8f13-9ce062333d8b)
