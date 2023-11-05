# UNDERSTANDING CLIENT-SERVER ARCHITECTURE.
## CLIENT-SERVER
Client-Server refers to an architecture where two or more computers are connected to each other over a network to send and receive requests between one another.In this type of communication, each machine has its own role to play: the machine sending the requests is referred to as **Client** while the machine responding to the request is called **Server**.<br/>
Below is a diagram of a Web Client-Server architecture where a machine is trying to access a Website using Web browser or a simply 'curl' command as a client and it sends HTTP requests to a Web server (Apache. Nginx, IIS etc) over the internet. 



![Screenshot 2023-11-05 212046](https://github.com/Saidat23/devops.pbl/assets/138054715/07a44dd0-daad-4e2e-805b-5c185edfa021)



If the concept is extended further and a Database Server is added to the archtecture, the Web Server will take the role of a Client that connects and read/write to/from a Database Server ( Oracle, MYSQL, MongoDB, SQL Server) communicating over the Local Network. It can also be over the internet connection, but it is a common practice to have the Web Server and the Database Server in same local network.
The diagram below is a generic Web Stack architecture (LAMP,LEMP,MEAN,MERN) which can be implemented with many other technologies from small single page applications **SPA** to large and complex portals.
![Screenshot 2023-11-05 211957](https://github.com/Saidat23/devops.pbl/assets/138054715/2932373f-0e40-4d03-a42a-84287e2b569e) <br/>
In a project implemented earlier, a **LAMP STACK** website was deployed. This website server can be located anywhere in the world and can be reached from any part of the globe over the global network-Internet.<br/>
Take for example, we type **www.google.com** on our browser. It means that your browser is considered as the **"Client"** sending request to the remote server and in turn, would be expecting some response from the remote server. 
