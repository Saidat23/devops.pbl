# IMPLEMENTING LOAD BALANCING WITH NGINX.
---
## INTRODUCTION TO LOAD BALANCING AND NGINX.
## LOAD BALANCING

Load balancing is the efficient distribution of incoming network traffic across a group of backend servers.

Modern websites with high‑traffic that serves hundreds of thousands of concurrent requests from users or clients requires adding more servers to meet these high volumes to respond correctly to text, images, video, or application data, in a fast, reliable and cost‑effective  manner.

A load balancer acts as the “traffic cop” positioned in front of your servers and routing client requests across all servers capable of fulfilling those requests in a manner that maximizes speed and capacity utilization. It ensures that no server is overworked to prevent degrading performance. If a single server goes down, the load balancer redirects traffic to the remaining active servers. And when a new server is added to the server group, the load balancer automatically starts to send requests to it.

### Functions of a load balancer:

- Distributes client requests or network load efficiently across multiple servers<br/>
- Ensures high availability and reliability by sending requests only to servers that are online<br/>
- Provides the flexibility to add or subtract servers as demand dictates<br/>

### Load Balancing Algorithms
Load  balancer algorithms are techniques used to distribute incoming network traffic across multiple servers, ensuring efficient utilization of resources and improving overall system performance, reliability and availability. Load balancing algorithms provide different benefits and the choice of load balancing method used depends on your needs. Here are some common load balancer algorithms.

**Round Robin** – Requests are distributed across the group of servers in the pool sequentially. It is simple to implement and ensures an even distribution of the traffic. It works well when the servers have capabilities and resources that are similar.<br/>
**Least Connections** – A new request is sent to the server with the fewest current connections to clients. The relative computing capacity of each server is factored into determining which one has the least connections.<br/>
**Weighted Round Robin** - This is similar to the Round Robin algorithm, but the servers are assigned different weights based on their capabilities. Servers with higher performance level receives more request.<br/>
**Weighted Least Connections** - This is similar to the Least Connections Algorithm, servers are assingned different weights based on their capabilities. Servers with higher capacities receives more connections.<BR/>
**IP Hash** – Uses the Hash function based on the IP address of the client to consistently map the client to a specific server. This ensures that the same client always reaches the same server.


### Load Balancing Benefits 

-Reduced downtime.<br/>
-Scalable<br/>
-Redundancy<br/>
-Flexibility<br/>
-Efficiency<br/>

## NGINX

NGINX is an open source software for web serving, caching, reverse proxying, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers. Nginx configuration depends on the use case.
## SETTING UP A BASIC LOAD BALANCER.
We will provision three EC2 instances running on Ubuntu Server 22.04. Install apache webserver on two of the instances and open port 8000 to allow traffic from anywhere, then update the default page of the webservers to display their public IP addresses.<br/>
On the third EC2 instance, install Nginx and configure it to act as a load balancer to distribute traffic across the webservers.<br/>

**STEP 1 :** Provision an EC2 Instance.<br/>
* Open your AWS Management Console, click on **EC2**, then click on **Launch Instance**.
  
  ![Screenshot 2023-11-12 153950](https://github.com/Saidat23/devops.pbl/assets/138054715/b5ec3b9b-52ed-40a7-956e-a705c9766dcc)
 ![Screenshot 2023-11-12 154509](https://github.com/Saidat23/devops.pbl/assets/138054715/ddbdf0cb-642b-47ad-8e39-90ffe65acfc3)

* Under **Name and tags**, provide a unique name for each of your webservers.<br/>
* Under **Applications and OS Images**, click on Ubuntu then select **Ubuntu Server 22.04** Volume Type.
  
![Screenshot 2023-11-09 121540 - Copy](https://github.com/Saidat23/devops.pbl/assets/138054715/fa39943b-15e7-439b-827b-219f851d1183)

*Under **Instance type**, select t3.micro.

![Screenshot 2023-11-12 194000](https://github.com/Saidat23/devops.pbl/assets/138054715/6b3e0e4f-fb5f-4c8b-b773-6e70a57e11aa)

* Under **Key Pair**, click on **Create new key pair** if you do not have one or select a **key pair name** if you have one already. You can use the same key pair for all the instances you provision for this lesson.

![Screenshot 2023-11-12 193036](https://github.com/Saidat23/devops.pbl/assets/138054715/47d93532-7917-49a7-9827-7b21fd8a2316)

* Under **Firewall (Security groups)**, Select **Create security group** or **Select existing security group** if you have one. Then, select **Allow SSH traffic from** **Anywhere** and **Allow HTTP traffic from the internet**.

![Screenshot 2023-11-12 195140](https://github.com/Saidat23/devops.pbl/assets/138054715/144272f6-a1d5-4d03-a670-d93fc5c52a85)

* Under **Summary**, select the Number of instances (3) and click **Launch instance**.

![Screenshot 2023-11-09 121719](https://github.com/Saidat23/devops.pbl/assets/138054715/1a85e6d3-d8ae-4f4a-9f31-52f3a89bc4b9)

* Click on **View all instances** to see all your provisioned instances.

![Screenshot 2023-11-09 122212](https://github.com/Saidat23/devops.pbl/assets/138054715/cd3f6ddc-3f5d-45c6-bc96-be068a032412)

**Step 2 :** We will be running our webservers on port 8000 while the load balancer runs on port 80. We need to open port 8000 to allow traffic from anywhere. We need to add a rule to the security group of each webserver to allow this.

* Select your instance and click on **Security** then click on your **Security groups** (looks like sg-12e34567---e41).

![Screenshot 2023-11-09 124043](https://github.com/Saidat23/devops.pbl/assets/138054715/185a151e-c84a-435d-a62e-84120a16513c)

* Click on **Edit inbound rules**.

![Screenshot 2023-11-09 124056](https://github.com/Saidat23/devops.pbl/assets/138054715/fa9bebb9-5b1d-4f83-866e-e5e3278bd4c4)

* On the **Edit inbound rules page**, click on **Add rule** and type in **8000** in the **Port range** box then select **Anywhere** and save rules.

 ![Screenshot 2023-11-09 124136](https://github.com/Saidat23/devops.pbl/assets/138054715/56fe4804-2ac4-4ec2-9644-7dde2331fe21)
 
Your Inbound rules will look like this.

![Screenshot 2023-11-09 124136](https://github.com/Saidat23/devops.pbl/assets/138054715/56fe4804-2ac4-4ec2-9644-7dde2331fe21)

 **Step 3 :** Install Apache Webserver.<br/>
 After we have provisioned both webservers and opened the necessary ports, next is to install apache software on both webservers. To do this, we must first connect to each webserver via SSH. we can then run commands on the terminal of our webservers.<br/>
* Connecting to the webservers: Select your instance and click on **Connect** at the top right of the page.

![Screenshot 2023-11-12 205039](https://github.com/Saidat23/devops.pbl/assets/138054715/75c53d8d-268c-410f-a6cf-36e735d9418b)

* Click on **SSH client** and copy the **SSH** command. (Looks like this- ssh -i "keypair.pem" ubuntu@ec2-IP address.com)

![Screenshot 2023-11-12 205106](https://github.com/Saidat23/devops.pbl/assets/138054715/66eac392-47ab-40c7-9f8a-4a97466c636c)

* Open a terminal on your local machine, **cd** into your Downloads folder.Then paste the SSH command you copied from the previous step onto your terminal and press enter. Type **Yes** when prompted. You should be connected to a terminal on your instance.
  
   ![Screenshot 2023-11-09 123129](https://github.com/Saidat23/devops.pbl/assets/138054715/96a74adb-77e0-40c7-9c40-a4f69464a7c4)
* Next, update and install apache with the command below:
  
  ``` sudo apt update -y && sudo apt install apache2 -y ``` 

![Screenshot 2023-11-09 123604](https://github.com/Saidat23/devops.pbl/assets/138054715/ac84907b-98b0-4a17-b740-771ff88dcdca)

* Verify that apache is running with the command:

 ``` sudo systemctl status apache2 ```
 
![Screenshot 2023-11-09 123738](https://github.com/Saidat23/devops.pbl/assets/138054715/62891585-2f78-4c58-a92f-37be625c476e)

* Paste your webserver's IP address on the browser to view your page.

![Screenshot 2023-11-09 123832](https://github.com/Saidat23/devops.pbl/assets/138054715/2df88999-c66a-4521-8281-4a286c5d7ec0)


**Step 4 :** Configure Apache to serve a page showing its IP address.<br/>

We will start by configuring **Apache** webserver to serve content on port 8000 instead of its default which is port 80. Then we will create a new **index.html** file.The file will contain code to display the public IP of the EC2 instance. We will then override the apache webserver's default html file with our new file.

  1. Configure Apache to serve content on port 8000:
     
 * Cd into /etc/apache2/. Open the file ports.conf using a text editor with the command

     ``` sudo vi ports.conf ```
     

![Screenshot 2023-11-09 194526](https://github.com/Saidat23/devops.pbl/assets/138054715/62f296ac-8bc5-4587-ade9-95709bac1d13) <br/>
  
  * Type I to switch to insert mode than add a new Listen directive for port 8000. Save and exit.
   
![Screenshot 2023-11-09 165530](https://github.com/Saidat23/devops.pbl/assets/138054715/96eacaca-24c4-42fe-a9d6-1203b68c9878)<br/> 

   * Cd into /etc/apache2/site-enabled/, open the file 000-default.conf using a text editor with the command<br/>

    ``` sudo vi 000-default.conf ```


![Screenshot 2023-11-09 164733](https://github.com/Saidat23/devops.pbl/assets/138054715/5297736c-e579-4462-ae95-eb783d47c86d)

  * Configure and change port 80 on the virtualhost to 8000 like in the screenshot below. 

 
![Screenshot 2023-11-09 164935](https://github.com/Saidat23/devops.pbl/assets/138054715/934ab654-bc70-4395-84d5-0456699d9f87) 

  * Close the file by first pressing the **esc** key on your keyboard then the command below.
    
     ``` wq!```
    
  *Restart apache to load the new configuration using the command below.

     ``` sudo systemctl restart apache2 ```
    
  2. Creating our new html file:
     
   * Create/Open a new file: index.html with the command below.

     ``` sudo vi index.html ```
     
   * Switch the Vi editor to insert mode and paste the file below. On your AWS Manageement Console, copy the public IP address of your EC2 instance and replace the placeholder text for IP address in the html file.
       
      ```         <!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: YOUR_PUBLIC_IP</p>
        </body>
        </html>

       ```
      
![Screenshot 2023-11-09 211343](https://github.com/Saidat23/devops.pbl/assets/138054715/74cae4fb-c050-4ff1-b031-6014e1ee8062)

   * Change file ownership of the index.html with the command below.
    
    ```
    sudo chown www-data:www-data ./index.html
    ```
    
![Screenshot 2023-11-09 204627](https://github.com/Saidat23/devops.pbl/assets/138054715/233d91cc-a3cd-48c1-8e74-444cafc418cd)

  3. Overriding the default html file of Apache Webserver.
     
  * Replace the default html file with our new html file using the command below.

       ``` sudo cp -f ./index.html /var/www/html/index.html ```

  * Restart the webserver to load the new configuration using the command below.

       ``` sudo systemctl restart apache2 ```
       
![Screenshot 2023-11-09 205039](https://github.com/Saidat23/devops.pbl/assets/138054715/f7c5a896-7f81-4c5e-bec3-9043556d2ece)

  * Your page on the browser should look like this. Remmember you are working on both webservers.

![Screenshot 2023-11-09 212303](https://github.com/Saidat23/devops.pbl/assets/138054715/8f5f5000-761f-4e30-a81c-c70c9ee1ca3c)

![Screenshot 2023-11-09 212234](https://github.com/Saidat23/devops.pbl/assets/138054715/14026053-a3dd-440f-8061-2d77687cb624)

  **Step 5:** Configuring Nginx as a Load Balancer.

  * On the third EC2 instance provisioned, confirm that port 80 is opened to accept traffic from anywwhere. Refer to **Step 2**.

  * Next SSH into the instance. Refer to **Step 3**.
 
  * Update and Install Nginx using the command below.

 ``` sudo apt update -y && sudo apt install nginx -y ```
 
![Screenshot 2023-11-09 212518](https://github.com/Saidat23/devops.pbl/assets/138054715/87d63fdd-1bee-47a1-b55e-292301f2d57d)

  * Verify that Nginx is installed with the command below.

 ``` sudo systemctl status nginx ```
 
![Screenshot 2023-11-09 212643](https://github.com/Saidat23/devops.pbl/assets/138054715/b4ecc01e-2fd2-4c5f-938a-ef8af48745f2)

  * Copy your Load Balancer's public IP, paste it on the browser. Your page on the browser would look like this.

  ![Screenshot 2023-11-09 212219](https://github.com/Saidat23/devops.pbl/assets/138054715/048c5d63-53ee-4fc8-829d-2a469a6fbb1d)  




     
![Screenshot 2023-11-09 215237](https://github.com/Saidat23/devops.pbl/assets/138054715/b6d10fd1-9bd3-49ba-92e9-fb1c922f60d3)

![Screenshot 2023-11-09 215219](https://github.com/Saidat23/devops.pbl/assets/138054715/6bdd003d-3df0-455d-aa82-0881a1b0f783)

 
  6. 




























