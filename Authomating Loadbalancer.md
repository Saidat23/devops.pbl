# AUTHOMATING LOADBALANCER CONFIGURATION WITH SHELL SCRIPTING.
This project demonstrate how to automate the setup and maintainance of your loadbalancer using a freestyle job to enhance and reduce manual effort. Shell Scripting and simple CI/CD on Jenkins streamline the loadbalancer configuration with ease.

## Automating the Deploment of Webservers.

As DevOps Engineers, automation is the heart of our work. Automation helps us speed the deployment of services and reduce error making in our day to day activities.<br/>
In the implementation of [Loadbalancing with Nginx project](https://github.com/Saidat23/devops.pbl/edit/main/Loadbalancing%20with%20Nginx.md), we deployed two backend servers with a loadbalancer distributing traffic across the webservers. This was done by typing the commands right on the terminal.<br/>
In this project, we will be automating the entire process by writing a shell script that when ran, it will automatically run all we did manually in the Nginx project.<br/>

## Deploying and Configuring the Webservers.

**Step 1:** Provision an EC2 instance running ubuntu 20.04. Refer to the project **Loadbalancing with Nginx**.

**Step 2:** Open port 8000 to allow traffic from anywhere using the security group.

**Step 3:** Connect to the webserver via the terminal using SSH client.  

**Step 4:** Creat/Open a file on your terminal.

  ``` sudo vi install.sh```
         
Paste the script below. The shell script below is a codified process we need to deploy our webservers.

         
   ``` 
       #!/bin/bash 
       
#################################################################################################################### 
##### This automates the installation and configuring of apache webserver to listen on port 8000 
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below: 
######## ./install_configure_apache.sh 127.0.0.1 
#################################################################################################################### 

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html> 
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html 
sudo systemctl restart apache2  
  ```

To close the file, press **esc** button on your keyboard then **Shift + :wq!**.  

**Step 5:** Change the permissions on the file to make it executable with the command:

  ```   sudo chmod +x install.sh ```

  **Step 6:** Run the shell script with the command below. Ensure you read the instructions in the shell script on how to use it.

  ```   ./install.sh PUBLIC_IP ```

## Deploying and Configuring Nginx Load Balancer Using Shell Script.

All the steps implemented in the **Implementing Load balance with Nginx** project has been codified in the script below. Follow the instructions to learn how to use the script.

  ``` 

#!/bin/bash

######################################################################################################################
##### This automates the configuration of Nginx to act as a load balancer
##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
#####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
############################################################################################################# 

PUBLIC_IP=$1
firstWebserver=$2
secondWebserver=$3

[ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

[ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

[ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure


sudo apt update -y && sudo apt install nginx -y
sudo systemctl status --no-pager nginx

if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
fi

sudo nginx -t

sudo systemctl restart nginx

  ```








