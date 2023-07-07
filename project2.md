# WEB STACK IMPLEMENTATION (LEMP STACK)
  This is a similar stack to project1. LEMP is an acronymn for Linux, Nginx, MySQL, PHP or Python, or Perl which are individual technologies used together for a specific technology product.
In this project I would be implementing a similar stack, but with an alternative Web Server – NGINX, it is also popular and widely used by websites.
With Git Bash downloaded and launched, I would ssh my EC2 instance using the command:  
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>

## INSTALLING THE NGINX WEB SERVER
 Nginx would be used to display our web pages to the site visitors. The apt package manager would be used to install this package.
First, the server’s package index is updated then Nginx installed using the command "sudo apt update"  -- for updating the server package index and "sudo apt install nginx"  -- for installing the nginx.
To check that the nginx is successfully installated and running as a service in Ubuntu, the command "sudo systemctl status nginx" is used.
The image below indicate that your nginx web server is successfully installed and running.

![Screenshot 2023-07-05 220552](https://github.com/Saidat23/devops.pbl/assets/138054715/caf4a99a-d517-43cb-b4e3-7737062433c7)

## INSTALLING MYSQ

With the web server up and running, next is to install a Database Management System (DBMS). MySQL is a popular relational database management system used within PHP environments to store and manage data for sites.
"sudo apt install mysql-server" command is used to acquire and install the software. 
When the installation is finished, log in to the MySQL console by typing 
"sudo mysql" command which is used to connect to the MySQL server as the administrative database user root.
 Output will look like this:
 
![mysql installed](https://github.com/Saidat23/devops.pbl/assets/138054715/36144c9f-6490-445b-ac37-6f6301b51f92)

"ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PASSWORD';" command was used to remove insecure default settings and lock down access to the database system. Before running the script, password for the root user was set, using mysql_native_password as default authentication method.  
Exit the MySQL shell using "exit" command.
The interactive script is run by  using the command "sudo mysql_secure_installation"
This will ask if you would like to configure the VALIDATE PASSWORD PLUGIN. 

If enabled, then, passwords that do not match the specified criteria will be rejected by MySQL with an error. If validation is disabled, a strong, unique passwords for database credentials should be used.
"sudo mysql -p" is used to test if you’re able to log in to the MySQL console.
Exit the MySQL console with the "exit".

## INSTALLING PHP
 PHP  processes code to display dynamic content to the end user.  
While the Apache server embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and thus, act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most of the PHP-based websites, while requiring additional configuration. I’ll need to install php-fpm, (PHP fastCGI process manager), and direct Nginx to pass the PHP requests to the software for processing. I’ll also need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. The Core PHP packages will be installed automatically as dependencies.

To install the 2 packages at once, "sudo apt install php-fpm php-mysql" is used and when prompted, type Y and press ENTER to confirm installation.
Now the PHP components is installed. Next is to configure Nginx to use them.
 
![php installed](https://github.com/Saidat23/devops.pbl/assets/138054715/7632d4e1-604c-41c8-bb0d-bcbb669ea9c1)

 ## CONFIGURING NGINX TO USE PHP PROCESSOR
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
The LEMP stack set up is now complete and fully functional. To validate that the Nginx can correctly hand .php files off to the PHP processor,a test PHP file in the document root would be created and a new file called info.php is opened within the document root in the text editor using "sudo nano /var/www/projectLEMP/info.php" command. Then paste the command 
"<?php
phpinfo();". The response is shown below: 

![Screenshot 2023-07-05 224223](https://github.com/Saidat23/devops.pbl/assets/138054715/16c5744c-3e13-4d57-a669-d235fb86e7d0)

## RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
A test database (DB) was create with simple “To do list” and the access configured so that the Nginx website would be able to query data from the DB and display it. We’ll need to create a new user with the "mysql_native_password" authentication method to connect to the MySQL database from PHP. We'll also create a database named "example_database" and a user named "example_user".
To start with, connect to the MySQL console through the root account using the command "sudo mysql".
A new database was created using "CREATE DATABASE `example_database`;" command. A new user was also created using "CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';" and giving full previledge on the created database with the command "GRANT ALL ON example_database.* TO 'example_user'@'%';". "Exit" command was used to exit the mysql shell.
To test if the new database has the proper permissions, log back into the myqsl consoleusing the custom user credentials "mysql -u example_user -p". 
It will prompt for the password used when creating the "example_user" user. After logging in to the MySQL console, confirm that you have access to the "example_database" database with the command "SHOW DATABASES;" which will give the following output.

![Screenshot 2023-07-07 233111](https://github.com/Saidat23/devops.pbl/assets/138054715/10a05c88-627d-4682-8832-c926f794c1d9)

Next we need to create a test table named "todo_list" using the mysql console by running the command "CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));".
Insert few rows of content into the test teble using the command "INSERT INTO example_database.todo_list (content) VALUES ("My first important item");" changing values for each row.
To comfirm that the data was succesfully saved in the table, run the command "SELECT * FROM example_database.todo_list;". Exit mysql with the "exit" command. 







