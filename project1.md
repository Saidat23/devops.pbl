# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
 A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are specifically chosen to work together in creating a well-functioning software. LAMP is an acronymn for Linux, Apache, MySQL, PHP or Python, or Perl which are individual technologies used together for a specific technology product.

## INSTALLING APACHE AND UPDATING THE FIREWALL
 Apache HTTP Server is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open source software available for free. It runs on 67% of all webservers in the world. It is fast, reliable, and secure. It can be highly customized to meet the needs of many different environments by using extensions and modules. Most WordPress hosting providers use Apache as their web server software. High-profile companies such as Cisco, IBM, LinkedIn, Facebook, Hewlett-Packard, AT&T, Siemens, eBay and many more use Apache.The Apache web server is among the most popular web servers in the world. It is well documented, has an active community of users, and has been in wide use for much of the history of the web, which makes it a great default choice for hosting a website.  

The EC2 instance was launched on AWS server, and Apache was installed using Ubuntu’s package manager ‘apt’.
To verify that apache2 is running as a Service on the OS, "sudo systemctl status apache2" command was used.
In order to receive any traffic on the Web Server, TCP port 80, which is the default port that web browsers use to access web pages on the Internet needs to be opened. So a rule for EC2 configuration to open inbound connection through port 80 was added to the security rule. By default, we have TCP port 22 open on our EC2 machine to access it via SSH. 

![Screenshot 2023-06-28 203909](https://github.com/Saidat23/devops.pbl/assets/138054715/74f0bffa-4a70-42e2-8c16-e7c5556fc340)

## INSTALLING MYSQ
  MySQL is a popular relational database management system used within PHP environments to store and manage data for your site.
"sudo apt install mysql-server" command is used to acquire and install the software.
"sudo mysql" command is used to connect to the MySQL server as the administrative database user root.
 Output will look like this:
![mysql installed](https://github.com/Saidat23/devops.pbl/assets/138054715/36144c9f-6490-445b-ac37-6f6301b51f92)

"ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PASSWORD';" command was used to remove insecure default settings and lock down access to the database system. Before running the script, password for the root user was set, using mysql_native_password as default authentication method.  
Exit the MySQL shell.

## INSTALLING PHP
 PHP  processes code to display dynamic content to the end user. In addition to the php package, I would need php-mysql, which allows PHP to communicate with MySQL-based databases. And also libapache2-mod-php to enable Apache to handle the PHP files. The core PHP packages will be installed as dependencies automatically.
To install these 3 packages at a go, "sudo apt install php libapache2-mod-php php-mysql" command is used and once the installation completes, "php -v" command is used to confirm the PHP version.

