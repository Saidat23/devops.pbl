# WEB STACK IMPLEMENTATION (LEMP STACK)
  This is a similar stack to project1. LEMP is an acronymn for Linux, Nginx, MySQL, PHP or Python, or Perl which are individual technologies used together for a specific technology product.
In this project I would be implementing a similar stack, but with an alternative Web Server – NGINX, it is also popular and widely used by websites.
With Git Bash downloaded and launched, I would ssh my EC2 instance using the command:  
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
## INSTALLING THE NGINX WEB SERVER
Nginx would be used to display our web pages to the site visitors. The apt package manager would be used to install this package.
First, the server’s package index is updated then Nginx installed using the command "sudo apt update"  -- for updating the server package index and "sudo apt install nginx"  -- for installing the nginx.
To check that the nginx is successfully installated and running as a service in Ubuntu, the command "sudo systemctl status nginx" is used.
