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



       ```  sudo vi install.sh ```
         
Paste the script below. The shell script below is a codified process we need to deploy our webservers.

         
       ```   #!/bin/bash
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
sudo systemctl restart apache2  ```



To close the file, press **esc** button on your keyboard then **Shift + :wq!** .       






