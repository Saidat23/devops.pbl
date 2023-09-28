# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
---
 A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are specifically chosen to work together in creating a well-functioning software. the stack include LAMP Stack, LEMP Stack. MEAN Stack and MERN Stack. 
 
 LAMP stack is an acronymn for:
 <!-- UL -->
 * Linux - operating sysytem
 
 * Apache - a web server
 
 * MySQL - a database server and
 
 * PHP or Python, or Perl - programming language. 
 
 These are open source technologies used by developers for a specific technology product. 

## PREREQUISITE FOR IMPLEMENTING LAMP STACK.
---
In order to carry out this project we need an AWS account and a virtual server with Ubuntu Server OS. AWS is the largest cloud service provider and we are leveraging its free tier account to execute this project.

## LAUNCHING YOUR INSTANCE.
---
Log into your AWS account to launch an EC2 instance.
<P>Click on EC2. Then Click on Launch Instance.</P>


![Screenshot 2023-09-27 175849](https://github.com/Saidat23/devops.pbl/assets/138054715/68acf810-60a0-47dd-9d41-a9ca0271b009)

Fill in the Name, select Ubuntu under the Quick Start section and select an Amazon Machine Image (AMI).

![Screenshot 2023-09-27 182258](https://github.com/Saidat23/devops.pbl/assets/138054715/23c703ff-1104-47d4-b4eb-6f690a2ce43f)

Select a free tier Instance type and your key pair if you have one or click on create new key pair. 

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

 
##  APACHE
---
<P>Apache HTTP Server is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open source software available for free. It runs on about 67% of all webservers in the world. It is fast, reliable, and secure. It can be highly customized to meet the needs of different environments by using extensions and modules. Most WordPress hosting providers use Apache as their webserver software. High-profile companies such as Cisco, IBM, LinkedIn, Facebook, Hewlett-Packard, AT&T, Siemens, eBay and many more make use of Apache. It is well documented with has an active community of users, and has been in wide use for much of the history of the web, which makes it a great default choice for hosting a website</P>

## INSTALLING APACHE AND UPDATING THE FIREWALL.
---
Update the package manager with the command:
```bash
sudo apt update.
```
![Screenshot 2023-09-27 231154](https://github.com/Saidat23/devops.pbl/assets/138054715/bd031412-f710-43a6-950a-96137eea6fa7)
 Then run the Apache2 package installation with the command:
 ```bash
sudo apt install apache2
```

![Screenshot 2023-09-27 232502](https://github.com/Saidat23/devops.pbl/assets/138054715/15297ade-ad00-4060-95be-3c1a5e31c641)

  
Verify that the Apache2 is running as a Service on the OS with the command:

```bash
  sudo systemctl status apache2
```
If it is running, it would show active and running in green colour.

![Screenshot 2023-09-27 185517](https://github.com/Saidat23/devops.pbl/assets/138054715/38add2d5-9987-4dd4-a495-41b7b0bd206a)

We can now view our Web server on our browser.
Copy the IP address of the instance and paste it on the browser adding the port 80 after it.As shown below:
```bash
http://<Public-IP-Address>:80
```
The browser wouldn't connect and gave the below error.

![Screenshot 2023-09-27 185740](https://github.com/Saidat23/devops.pbl/assets/138054715/e86505ec-f19b-4b9b-accc-3083f79e9042)

In order to receive any traffic on the Web Server, TCP port 80, which is the default port that the web browsers use to access web pages on the Internet, needs to be opened.

#### OPENING PORT 80.

By default, we have TCP port 22 open on our EC2 machine to access it via SSH. 

Select your EC2 instance, click on security on the botton tab and then security groups.

![Screenshot 2023-09-27 185824](https://github.com/Saidat23/devops.pbl/assets/138054715/279c20b0-7db1-4d65-a5b9-32f1a40f01fa)

Click on Inbound rules, then on edit inbound rules.

![Screenshot 2023-09-27 185846](https://github.com/Saidat23/devops.pbl/assets/138054715/ef96a6f0-9772-4d11-b1ea-c5b7a9fcc76d)

Click on the drop down arrows to select HTTP port 80 and Anywhere (0.0.0.0/0) then click on save rules.

![Screenshot 2023-09-27 185924](https://github.com/Saidat23/devops.pbl/assets/138054715/63414fb3-8058-4743-8ff6-fa3fd88ffd94)

Re-fresh the browser to view your web page. The Apache2 Ubuntu Default page will appear with the word it works!
 
![Screenshot 2023-06-28 203909](https://github.com/Saidat23/devops.pbl/assets/138054715/74f0bffa-4a70-42e2-8c16-e7c5556fc340)

## INSTALLING MYSQ
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

## INSTALLING PHP
 PHP  processes code to display dynamic content to the end user. In addition to the php package, I would need php-mysql, which allows PHP to communicate with MySQL-based databases. And also libapache2-mod-php to enable Apache to handle the PHP files. The core PHP packages will be installed as dependencies automatically.
To install these 3 packages at a go, < sudo apt install php libapache2-mod-php php-mysql > command is used and once the installation completes, < php -v > command is used to confirm the PHP version. This shows the version of the PHP.

![php installed](https://github.com/Saidat23/devops.pbl/assets/138054715/7632d4e1-604c-41c8-bb0d-bcbb669ea9c1)

## CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE  


A directory was created with the command < sudo mkdir /var/www/projectlamp > and ownership of the directory was assign with the $USER environment variable to reference the current system user. This was done with the command < sudo chown -R $USER:$USER /var/www/projectlamp >.

Then, a new configuration file in Apache’s sites-available directory was created and opened using vim command line editor with the command < sudo vi /etc apache2/sites-available/projectlamp.conf >. The bare-bones configuration below was pasted in the new blank file created. The file should be in the insert mode: click " i " to move into the insert mode.

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

The file was saved with closed following the steps below:
1. Hit the esc button on the keybord
2. Type :
3. Type wq. w indicate write and q indicate quit.
4. Hit ENTER to save the file.

The ls command < sudo ls /etc/apache2/sites-available > was used to check the new file in the sites-available directory. With the virtualHost configuration, Apache will serve the projectlamp by using /var/etc/projectlamp as the web root directory. 



Then < sudo a2ensite projectlamp > command was used to enable the new virtual host and the default website that was installed with Apache is disabled using < sudo a2dissite 000-default > command. This is required if we are not using the custom domain name to prevent Apache's default configuration from overwriting our virtual host. 
The configuration file was checked for syntax errors by running < sudo apache2ctl configtest > command.Then, Apache was reloaded with the command < sudo systenctl reload apache2 > to effect the changes. Now our new website is active but the web root /var/www/projectlamp was still empty. An index.html file was created to test that the virtual host works as expected using the command below.

< sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html >
The text from the " echo " command was displayed on the browser.

![installing Apache and updating firewall](https://github.com/Saidat23/devops.pbl/assets/138054715/41c0bd72-a351-44d6-a56e-07b4eca34f90)

## ENABLE PHP ON THE WEBSITE
Finally, PHP script was created to test that PHP is correctly installed and configured on your server.
Created a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
A new file named index.php was created inside the custom web root folder using "vim /var/www/projectlamp/index.php" command which opens a blank page to insert
"<?php
phpinfo();".
The page below indicate that the PHP installation is working as expected.

![php](https://github.com/Saidat23/devops.pbl/assets/138054715/78462150-e2f1-4246-b4bb-8f0f3de0459c)






