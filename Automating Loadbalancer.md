# AUTOMATING LOADBALANCER CONFIGURATION WITH SHELL SCRIPTING.
This project demonstrate how to automate the setup and maintainance of your loadbalancer using a freestyle job to enhance and reduce manual effort. Shell Scripting and simple CI/CD on Jenkins streamline the loadbalancer configuration with ease.

## Automating the Deploment of Webservers.

As DevOps Engineers, automation is the heart of our work. Automation helps us speed the deployment of services and reduce error making in our day to day activities.<br/>
In the implementation of [Loadbalancing with Nginx project](https://github.com/Saidat23/devops.pbl/edit/main/Loadbalancing%20with%20Nginx.md), we deployed two backend servers with a loadbalancer distributing traffic across the webservers. This was done by typing the commands right on the terminal.<br/>
In this project, we will be automating the entire process by writing a shell script that when ran, it will automatically run all we did manually in the Nginx project.<br/>

## Deploying and Configuring the Webservers.

**Step 1:** Provision an EC2 instance running ubuntu 20.04. Refer to the project **Loadbalancing with Nginx**.

![Screenshot 2023-11-19 205615](https://github.com/Saidat23/devops.pbl/assets/138054715/ac865125-5e14-4407-a293-8af949ddcf81)

**Step 2:** Open port 8000 to allow traffic from anywhere using the security group.

**Step 3:** Connect to the webserver via the terminal using SSH client.  

![Screenshot 2023-11-19 205708](https://github.com/Saidat23/devops.pbl/assets/138054715/29232f95-da83-447f-aabf-9ddfc3425188)

**Step 4:** Creat/Open a file on your terminal.

  ``` sudo vi install.sh```
  
   ![Screenshot 2023-11-19 220855](https://github.com/Saidat23/devops.pbl/assets/138054715/d484fd87-1404-4f8e-9f54-2e270f2ccd3c)
   
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

![Screenshot 2023-11-19 211037](https://github.com/Saidat23/devops.pbl/assets/138054715/11b7b3a1-6b9f-4cd0-9579-8c971fd470c0)

**Step 5:** Change the permissions on the file to make it executable with the command:

  ```   sudo chmod +x install.sh ```

  **Step 6:** Run the shell script with the command below. Ensure you read the instructions in the shell script on how to use it.

  ```   ./install.sh PUBLIC_IP ```
  
  ![Screenshot 2023-11-19 220525](https://github.com/Saidat23/devops.pbl/assets/138054715/4f3ce6f4-9d96-44dd-81ec-470530726072)
  
  ## Deployment of Nginx as a Load Balancer using Shell Script.
  Having successfully deployed and configured our two weservers, we will now move to the load balancer. As prerequisite, we need to provision an EC2 instance running ubuntu 22.04, open port 80 to allow traffic from anywhere using the security group and connect to the load balancer via the terminal.

## Deploying and Configuring Nginx Load Balancer Using Shell Script.
Conect to the terminal using the instance SSH.
![Screenshot 2023-11-19 205820](https://github.com/Saidat23/devops.pbl/assets/138054715/bb973a55-5a12-4994-ba01-323db613e6dc)
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

## Steps to Run the Shell Script.

**Step 1:** On your terminal, open a file nginx.sh with the command below:

  ``` sudo vi nginx.sh ```
  
  ![Screenshot 2023-11-19 221359](https://github.com/Saidat23/devops.pbl/assets/138054715/75c26240-3a53-42fa-b2c9-6b2271b5a792)
  
**Step 2:** Copy and paste the script inside the file.

![Screenshot 2023-11-19 210108](https://github.com/Saidat23/devops.pbl/assets/138054715/3395308e-17e3-404b-b6fa-87ee62802fbe)

**Step 3:** Close the file using the command below:

  ```type esc the shift + :wqa!```

**Step 4:** Change the file permission to make it executable with the command below:

  ``` sudo chmod +x nginx.sh ```

  **Step 5:** Run the script with the command below.

    ``` ./nginx.sh PUBLIC_IP Webserver-1 Webserver-2 ```
    
![Screenshot 2023-11-19 205904](https://github.com/Saidat23/devops.pbl/assets/138054715/5cdca61c-ba41-44e9-97d2-f682b71b7903)

   ## Verifying the Setup

 **Screenshot For Webserver one**
 
 
![Screenshot 2023-11-19 200623](https://github.com/Saidat23/devops.pbl/assets/138054715/77dae558-152b-44f6-a69b-87dcfc114306)

 **Screenshot For Webserver two**
 
 
![Screenshot 2023-11-19 200611](https://github.com/Saidat23/devops.pbl/assets/138054715/c06e4def-811e-471a-95df-0f9019e148f3)

**Screenshot of Load balancer**
 

![Screenshot 2023-11-19 200602](https://github.com/Saidat23/devops.pbl/assets/138054715/583422f3-f9bc-4dcd-8e6f-0b1d12b10335)

![Screenshot 2023-11-19 204333](https://github.com/Saidat23/devops.pbl/assets/138054715/f637df03-5982-4fb1-95f1-1f985318665b)

![Screenshot 2023-11-19 204319](https://github.com/Saidat23/devops.pbl/assets/138054715/55b036fc-faca-4819-8fd3-7225b0ffd397)



























