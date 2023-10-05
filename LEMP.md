# WEB STACK IMPLEMENTATION (LEMP STACK)
---
  This project is similar to Project 1. LEMP is an acronymn for **Linux**, **Nginx**, **MySQL**, **PHP** or **Python**, or **Perl**. This are individual technologies used together for a specific technology product.
In this project, Similar stack would be implemented, using an alternative Web Server – **NGINX**, to create a dynamic and high-performing websites. We would be looking into the architecture of the **LEMP** stack, understanding how **Linux** provides a solid foundation, **Nginx** serves as a Powerful web server, Database is handled by **MySQL** and **PHP** empowers server-side. In this project, we would set up a **Linux** environment, configure **Nginx** for optimal performance, manage **MySQL** databases and develop 
**PHP** code to bring the applications to life. Through this exercise, we would explore techniques for handling user requests, interact with databases, process forms and implement robust security measures. We would work with popular development frameworks and tools that elevates productivity and simplify the web application development process. 




## LAUNCHING YOUR INSTANCE.
---
Log into your AWS account to launch an EC2 instance.
<P>Click on EC2. Then Click on Launch Instance.</P>


![Screenshot 2023-09-27 175849](https://github.com/Saidat23/devops.pbl/assets/138054715/68acf810-60a0-47dd-9d41-a9ca0271b009)

Fill in the Name, select Ubuntu under the Quick Start section and select an Amazon Machine Image (AMI).

![Screenshot 2023-09-27 182258](https://github.com/Saidat23/devops.pbl/assets/138054715/23c703ff-1104-47d4-b4eb-6f690a2ce43f)

Select a free tier Instance type and your key pair, if you have one already or click on create new key pair. 

![Screenshot 2023-09-27 182348](https://github.com/Saidat23/devops.pbl/assets/138054715/ec602634-9b42-494c-9fcf-c8f38133a5ad)

 To create key pair:
<P> Type in your key pair name,</P>
 <P>Select key pair type ( .pem for Windows 10 above & Mac and .ppk for Windows lower than 10.)</P> 
 <P>Then click on Create key pair.</P>
 
![Screenshot 2023-09-27 184021](https://github.com/Saidat23/devops.pbl/assets/138054715/971ad914-3c0c-42c7-be4c-cd80d6087977)

Click on Launch Instance.

![Screenshot 2023-09-27 184118](https://github.com/Saidat23/devops.pbl/assets/138054715/df19dc31-ca77-46f6-ba4c-26ef34425836)

Click on the box in front of your EC2 instance. Then click on connect at the top right corner of the page. 

![Screenshot 2023-09-27 220333](https://github.com/Saidat23/devops.pbl/assets/138054715/7bc40df2-df3c-4967-84de-9534cd8a53a3)

<P>Click on SSH Client and copy the SSH Command.</P>

![Screenshot 2023-09-27 184231](https://github.com/Saidat23/devops.pbl/assets/138054715/bc9a4597-31e1-4ef7-a47e-d4cafe116623)

Open your terminal on your computer and change the directory to the folder where you have your key pair using the command below. 
```bash
cd <key pair location>.
```
I have mine in the Downloads folder hence,

```bash
 cd Downloads 
```
 The command below is used to connect to your EC2 Instance.
 ```bash
 < ssh -i < "access key" > Ubuntu@< public ip address >.
```
On your terminal, paste the copied EC2 SSH link and click enter to connect to your instance. Mine is shown below:
```bash
ssh -i "EC2-key-pair.pem" ubuntu@ec2-51-20-73-26.eu-north-1.compute.amazonaws.com
```

 ![Screenshot 2023-09-27 184413](https://github.com/Saidat23/devops.pbl/assets/138054715/96b2718f-6412-4dfa-a1ef-8edae2fadca1)


## INSTALLING THE NGINX WEB SERVER
---
 Nginx is used to display our web pages to the site visitors. The apt package manager would be used to install this package.
 
Update the server’s package index with the command:
```
sudo apt update
```
Then install Nginx using the command:
```
sudo apt install nginx
```
To check that the nginx is successfully installated and running as a service in Ubuntu, Run the command: 

```
sudo systemctl status nginx
```

The image below indicate that the nginx web server is successfully installed and running.

![Screenshot 2023-07-05 220552](https://github.com/Saidat23/devops.pbl/assets/138054715/caf4a99a-d517-43cb-b4e3-7737062433c7)

## INSTALLING MYSQL
---
  MySQL is a popular relational database management system used within PHP environments to store and manage data for your site.
  Install MYSQL with the command:
  ```
 sudo apt install mysql-server
```
 Connect to MySQL server as the administrative database user root using the command:
 ```
 sudo mysql
```
 Output will look like this:
 
![mysql installed](https://github.com/Saidat23/devops.pbl/assets/138054715/36144c9f-6490-445b-ac37-6f6301b51f92)

It is recommended that we run a security script that comes pre-installed with MYSQL. This script will remove insecure default settings and lock down access to your database system.
Set a password for the root user before running the script with the command:

```
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PASSWORD';
```
 Before running the script, set password for the root user , using
 
 ```
 mysql_native_password
``` 
Exit the MySQL shell with

```
mysql> exit
```
To start the interactive script, run:

```
sudo mysql_secure_installation
```
You would be asked if you want to configure the **VALIDATE PASSWORD PLUGIN**.
If it is enabled, any password that does not match the specified criteria will be rejected by **MYSQL**. It is save to leave validation disabled but you should always use a strong and unique password for the database credentials. You can answer **Y** for **Yes** or anything else to continue without enabling it. If you answer **YES**, you'll need to select a level of password validation. Selecting **2**, which is the strongest level, means that when you attempt to set any password which dose not contain upper and lowercase letters, numbers and special characters or common dictionary words it will give you an error message. You will see something like the message below.

**There are three levels of password validation policy:
LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, and special characters and dictionary file
Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG :1**

Regardless of whether we chose to set up the **VALIDATION PASSWORD PLUGIN**, the server will next time ask us to select and confirm a password for the **MYSQL root user** which is different from the **System root**. The **database root user** is an administrative user with a full privilege over the database system. A strong password should be set up as an additional safety measure.

<P> If password validation is enabled, you'll  be shown the password strenght for the root password you just set and your server will ask if you want to continue with the password. </P>
 
  **Estimate strength of the password: 100
 Do you wish to continue with the password provided? (Press y/Y for Yes, any other key for NO):y**

For the rest of the questions, press **Y** and then hit **Enter** key at each prompt. This will prompt you to change the root password, remove some anonymous users, test the database, disable remote root logins and load the new rules so that MySQL will immediately start responding to the changes made. Test if you're able to log in to the  MySQL console with this command:
```
sudo mysql -p
```
 The **-p** flag in the command will prompt you for the password used after changing the **root** user password.
 
To exit MySQL console, Type:
```
mysql> exit
```

 ## INSTALLING PHP
---
 PHP  processes code to display dynamic content to the end user.  
While the Apache server embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and thus, act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most of the PHP-based websites, while requiring additional configuration. I’ll need to install php-fpm, (PHP fastCGI process manager), and direct Nginx to pass the PHP requests to the software for processing. I’ll also need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. The Core PHP packages will be installed automatically as dependencies.

To install the 2 packages at once, "sudo apt install php-fpm php-mysql" is used and when prompted, type Y and press ENTER to confirm installation.
Now the PHP components is installed. Next is to configure Nginx to use them.
 
![php installed](https://github.com/Saidat23/devops.pbl/assets/138054715/7632d4e1-604c-41c8-bb0d-bcbb669ea9c1)

 ## CONFIGURING NGINX TO USE PHP PROCESSOR
 ---
 When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. 

On Ubuntu 20.04, by default, Nginx has only one server block enabled and it is configured to serve documents out of a directory at /var/www/html. It can be difficult to manage if one is hosting multiple sites. Rather than modifying /var/www/html, I’ll create a directory structure within /var/www for the "your_domain" website, leaving /var/www/html in place as the default directory to be used if a client request does not match any other sites.
To create the root web directory for the "your_domain" Website, run "sudo mkdir /var/www/projectLEMP" then, assign ownership of the directory with the $USER environment variable "sudo chown -R $USER:$USER /var/www/projectLEMP", this will reference your current system user.
Now, We'll open a new configuration file in Nginx’s sites-available directory using any preferred command-line editor. Here, Nano editor was used with the command "sudo nano /etc/nginx/sites-available/projectLEMP". The bare-bones configuration was pasted in the blank and activated by linking it to the config file from Nginx's sites-enabled directory using "sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/".
Then the configuration was tested for syntax errors with the command "sudo nginx -t". The following response (nginx: the configuration file /etc/nginx/nginx.conf syntax is ok,
nginx: configuration file /etc/nginx/nginx.conf test is successful) indicate that there is no syntax error and the config test is ok.
The default Nginx host currently configured to use port 80, would need to be disable for it to run. This is done by running the command "sudo unlink /etc/nginx/sites-enabled/default" and then , reload Nginx using the command "sudo systemctl reload nginx" to apply the changes. 

Now, the new website is active, but the web root /var/www/projectLEMP is still empty.To test that the new server block is working as expected, an index.html file is created in same location and command "sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html" is ran. which give a response :
![Screenshot 2023-07-05 223955](https://github.com/Saidat23/devops.pbl/assets/138054715/79ee6e18-a78a-4f22-8e3f-d51976fdd993)

## TESTING PHP WITH NGINX
---
The LEMP stack set up is now complete and fully functional. To validate that the Nginx can correctly hand .php files off to the PHP processor,a test PHP file in the document root would be created and a new file called info.php is opened within the document root in the text editor using "sudo nano /var/www/projectLEMP/info.php" command. Then paste the command 
"<?php
phpinfo();". The response is shown below: 

![Screenshot 2023-07-05 224223](https://github.com/Saidat23/devops.pbl/assets/138054715/16c5744c-3e13-4d57-a669-d235fb86e7d0)

## RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
---
A test database (DB) was create with simple “To do list” and the access configured so that the Nginx website would be able to query data from the DB and display it. We’ll need to create a new user with the "mysql_native_password" authentication method to connect to the MySQL database from PHP. We'll also create a database named "example_database" and a user named "example_user".
To start with, connect to the MySQL console through the root account using the command "sudo mysql".
A new database was created using "CREATE DATABASE `example_database`;" command. A new user was also created using "CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';" and giving full previledge on the created database with the command "GRANT ALL ON example_database.* TO 'example_user'@'%';". "Exit" command was used to exit the mysql shell.
To test if the new database has the proper permissions, log back into the myqsl consoleusing the custom user credentials "mysql -u example_user -p". 
It will prompt for the password used when creating the "example_user" user. After logging in to the MySQL console, confirm that you have access to the "example_database" database with the command "SHOW DATABASES;" which will give the following output.

![Screenshot 2023-07-07 233111](https://github.com/Saidat23/devops.pbl/assets/138054715/10a05c88-627d-4682-8832-c926f794c1d9)

Next we need to create a test table named "todo_list" using the mysql console by running the command "CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));".
Insert few rows of content into the test teble using the command "INSERT INTO example_database.todo_list (content) VALUES ("My first important item");" changing values for each row.
To comfirm that the data was succesfully saved in the table, run the command "SELECT * FROM example_database.todo_list;". Exit mysql with the "exit" command. 
Now we need to create a PHP script that will connect to MySQL database and query content. To do this we have to create new PHPfile in the custom web root directory using vi editor with the command "vim /var/www/projectLEMP/todo_list.php". Insert your PHP script, save and close the file.
Now, we can access the page on the web browser by visiting the domain name or public IP address configured for the website, followed by /todo_list.php i.e "http://<Public_domain_or_IP>/todo_list.php"
You would see a page like this, showing the content inserted in your test table. This means the PHP environment is ready to connect and interact with your MySQL server.
![Screenshot 2023-07-05 232212](https://github.com/Saidat23/devops.pbl/assets/138054715/9dc82552-32e8-4236-9ce7-3a61a1a303cc)






