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

