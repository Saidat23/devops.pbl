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

## IMPLEMENTING A CLIENT-SERVER COMMUNICATION.
Open your terminal and **'cd'** into where you have your **pem** key. Then run the curl command below.<br/>

``` curl -iV http://www.google.com```

![Screenshot 2023-11-05 214549](https://github.com/Saidat23/devops.pbl/assets/138054715/6293f065-3fca-4fae-a70b-8ca850d9c873)


If you do not have **'curl'** installed, you can do so with the command:<br/>
``` sudo apt install curl ```<br/>
In the example above, the terminal will be the **Client** while **www.google.com** will be the **server**. 
Another simple way to get a server's IP address is to use the diagnostic tool like **'ping'**. It also shows the round-trip time: the time for the packets to go to the server and back.<br/>
Use the command below to ping your browser.

```ping google.com```

![Screenshot 2023-11-05 215625](https://github.com/Saidat23/devops.pbl/assets/138054715/9034ccb4-6595-4dfa-9a5f-5bc39dd426b6)

 ## IMPLEMENTING A CLIENT-SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).
 MySQL is an open source, popular relational database management system used to store and manage data for Website.  To demonstrate a basic client server using MYSQL RDBMS, follow the steps explained below.
 
 **Step 1**: Create and configure two virtual servers (AWS EC2 instances) and name:<br/>
 Server A - **'mysql server'** <br/>
 Server B - **'mysql client'** <br/>
  **Step 2**: Connect **'mysql server'** on your terminal with the command:
  
   ``` ssh -i "access key" Ubuntu@public ip address ```
   
 * Replace the access key and public IP address with your own values.

  Then, cd into your download folder or wherever you have your key-pair located. Mine is in my download folder.

  ``` cd Downloads ```
  
  Run the command below to update MySQL on the terminal.
 
 ``` sudo apt update ```
 
 ![Screenshot 2023-11-05 224747](https://github.com/Saidat23/devops.pbl/assets/138054715/29357a5a-305c-4bb8-bbca-d3f9b6617715)
 
**STEP 3**:  Then install MySQL Server Software using the command:
  
  ``` sudo apt install mysql-server ```

  ![Screenshot 2023-11-02 104402](https://github.com/Saidat23/devops.pbl/assets/138054715/ab336669-50d0-4959-bd99-2ee471c7a236)
  
 Connect to MySQL server as the administrative database user root using the command:
 
 ``` sudo mysql ```
 
 Output will look like this:
 
![mysql installed](https://github.com/Saidat23/devops.pbl/assets/138054715/36144c9f-6490-445b-ac37-6f6301b51f92)



**Step 4**: Both the EC2 virtual servers are located in the same local virtual network by default, so they can communicate to each other using the local IP adderess.<br/> Use the local IP adderess of the mysql server to connect to mysql client. By default, MySQL server uses TCP port 3306. To connect, we have to open the port by creating a new Inbound rule in mysql server's Security Group. For extra security on the server, we do not allow all IP addresses to pass through the mysql server. Only allow access to mysql client Private IP address.  

![Screenshot 2023-11-03 204714](https://github.com/Saidat23/devops.pbl/assets/138054715/0359269c-1cec-493f-a4f3-828e38bc2e72)

**Step 5**: Configure mysql server to allow connections from remote hosts with the commands below.

``` cd /etc/mysql/mysql.conf.d/mysqld.cnf ```

 VI/VIM into it.

``` sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf ```
<br/>

 Then edit the **bind address** by replacing the **'127.0.0.1'** to **'0.0.0.0'**.
 
 ![Screenshot 2023-11-03 210053](https://github.com/Saidat23/devops.pbl/assets/138054715/c2593078-5069-4194-81cc-6bb6aaf12e04)
 
To confirm that the configuration is correct, run the command below.

``` sudo systemctl restart mysql.service```

![Screenshot 2023-11-03 211620](https://github.com/Saidat23/devops.pbl/assets/138054715/9096b6de-e0cc-4f20-ad41-833528630bd3)

**Step 6**: **'Cd'** back to home and than connect into mysql environment using the command

``` sudo mysql ```

 Create a user on the database in mysql console environment with the sql query command:

 ``` CREATE USER 'username'@'host' identified by password; ```
 
 Edit the username to your prefered name and the host to your IP address.
 
 ``` CREATE USER 'saidat'@'172.31.22.194' identified by 'password'; ```

 Then give permission to the user created in order to have access to the Database. use the command:

 ``` grant all privileges *.* TO 'saidat'@'172.31.22.194'; ```

 Exit mysql.

 ![Screenshot 2023-11-03 224210](https://github.com/Saidat23/devops.pbl/assets/138054715/42333512-6cbb-4bfe-ba51-913327d61a05)

**Step 7**: Open another terminal and SSH into it with the IP address of the mysql client. 

**'Cd'** into your key-pair location than . Update the server using 

``` sudo apt update```

Then install MySQL Client software with the command

``` sudo mysql-client -y ```



 From mysql client connect remotely to mysql server Database Engine using mysql utility.

 

 
 
 









 
 
