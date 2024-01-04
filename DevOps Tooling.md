# Devops Tooling Website Solution
---
In this project, we will be building a comprehensive DevOps tooling website solution by exploring the integration of various tools and technology to create a unified platform that enhances automation, 
collaboration and efficiency for software development and operation teams.

In a previous Project, [WordPress Website](https://github.com/Saidat23/devops.pbl/blob/main/WordPress%20Website%20with%20LVM%20Storage%20Management.md), we implemented a WordPress based solution that is ready to be filled with content and can be used as a website or blog. In this project, we will introduce a set of DevOps tools that will help our team in day to day activities like managing, developing, testing, deploying and monitoring different projects.

The DevOps Tooling Solution will consist of:


1. **Jenkins** - is a free and open source automation server used to build CI/CD pipelines.

2. **Kubernetes** - an open-source container-orchestration system for automating computer application deployment, scaling, and management.

3. **Jfrog Artifactory** - Universal Repository Manager supporting all major packaging formats, build tools and CI servers. Artifactory.

4. **Rancher** - an open source software platform that enables organizations to run and manage Docker and Kubernetes in production.

5. **Grafana** - a multi-platform open source analytics and interactive visualization web application.

6. **Prometheus** - An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.

7. **Kibana** - Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.


In this project, we will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible and this will consists of following components:

1. **Infrastructure**: AWS

2. **Webserver Linux**: Red Hat Enterprise Linux 8

3. **Database Server**: Ubuntu  20.04 + MySQL

4. **Storage Server**: Red Hat Enterprise Linux 8 + NFS Server

5. **Programming Language**: PHP

6. **Code Repository**: GitHub


For Rhel 8 server, use this **ami RHEL-8.6.0_HVM-20220503-x86_64-2-Hourly2-GP2 (ami-035c5dc086849b5de)**


On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage. Even though the NFS server might be located on a completely separate hardware - for Web Servers it look like a local file system from where they can serve the same files.


It is important to know what storage solution is suitable for what use cases, for this - you need to answer following questions: what data will be stored, in what format, how this data will be accessed, by whom, from where, how frequently, etc. Base on this you will be able to choose the right storage system for your solution.



**Step 1- Prepare NFS Server**


1. Spin up a new EC2 instance with RHEL Linux 8 Operating System.


2. Based on your **LVM** experience from  [WordPress Website](https://github.com/Saidat23/devops.pbl/blob/main/WordPress%20Website%20with%20LVM%20Storage%20Management.md), Configure LVM on the Server.

* Instead of formating the disks as **ext4** you will have to format them as **xfs**

* Ensure there are 3 Logical Volumes. **lv-opt**, **lv-apps** and **lv-logs**

3. Create mount points on **/mnt** directory for the logical volumes as follow:

Mount **lv-apps** on **/mnt/apps**  - To be used by webservers
Mount **lv-logs** on  **/mnt/logs** - To be used by webserver logs
Mount **lv-opt**  on  **/mnt/opt**  - To be used by Jenkins server in the next project.



4. Install **NFS server**, configure it to start on reboot and make sure it is up and running with the commands below.

```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```


Export the mounts for webservers' **subnet cidr** to connect as clients. All the three Web Servers will be installed  inside the same subnet, but in a production set-up you will probably want to separate each tier inside its own subnet for higher level of security.

To check your **subnet cidr**, open your EC2 details in AWS Console and locate **Networking** tab. Open a Subnet link:


 Set up permission that will allow our Web servers to read, write and execute files on NFS:
 
```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt


sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt


sudo systemctl restart nfs-server.service
```


Configure access to **NFS** for clients within the same subnet (example of Subnet CIDR - 172.31.32.0/20 ):

```
sudo vi /etc/exports

/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

sudo exportfs -arv
```

Check the port used by **NFS** and open it using Security Groups (add new Inbound Rule)

```rpcinfo -p | grep nfs```

To access the **NFS** server from your client, open the following ports: **TCP 111**, **UDP 111**, **UDP 2049** and **TCP 2049**

**Step 2 — Configure the database server**

1. Install **MySQL** server
  
2. Create a database and name it **tooling**

3. Create a database user and name it **webaccess**

4. Grant permission to **webaccess** user on **tooling** database to do anything only from the webservers **subnet cidr**

**Step 3 — Prepare the Web Servers**

We need to make sure that our Web Servers can serve the same content from shared storage solutions- NFS Server and MySQL database. For storing shared files that our Web Servers will use, we will utilize **NFS** and mount previously created Logical Volume **lv-apps** to the folder where **Apache** stores files to be served to users **(/var/www)**.
This approach will make our Web Servers stateless, which means we will be able to add or remove new ones when needed, and the integrity of the data (in the database and on NFS) will be preserved.

* Configure NFS client on all three servers.

* Next, deploy a Tooling application from the Web Servers into a shared NFS folder.

* Then, configure the Web Servers to work with a single MySQL database.



1. Launch a new EC2 instance with **RHEL 8 Operating System**


2. Install NFS client running the command below.

```sudo yum install nfs-utils nfs4-acl-tools -y```


3. Mount **/var/www/** and target the NFS server's export for apps with the command,
   
```
sudo mkdir /var/www

sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
```


4. Verify that NFS was mounted successfully by running ```df -h```.

 Make the changes persist on your Web Server after reboot by opening your **/etc/fstab** to edit it. Run the command below.

```sudo vi /etc/fstab```

Add the following line 

```<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0```


5. Install **Remi's repository**, Apache and PHP

```sudo yum install httpd -y```

```sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm```

```sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm```

```sudo dnf module reset php```

```sudo dnf module enable php:remi-7.4```

```sudo dnf install php php-opcache php-gd php-curl php-mysqlnd```

```sudo systemctl start php-fpm```

```sudo systemctl enable php-fpm```

```setsebool -P httpd_execmem 1```


Repeat **steps 1-5** for the other 2 Web Servers.


6. Verify that Apache files and directories are available on the Web Server in **/var/www** and also on the NFS server in **/mnt/apps**. If the same files are present, that means NFS is mounted correctly. Create a new file **touch test.txt** on one server and check if the same file is accessible from other Web Servers.


7. Locate the log folder for **Apache** on the **Web Server** and mount it to **NFS server's** export for logs. Repeat **step 4** to make sure the mount point will persist after reboot.


8. Fork the **tooling source code** from **[A Github Account](https://github.com/darey-io/tooling)** to your Github account. Watch how to fork a repo [here](https://www.youtube.com/watch?v=f5grYMXbAV0).


9. Deploy the **tooling website's code** to the **Webserver**. Ensure that the **html folder** from the repository is deployed to **/var/www/html**.


**Note 1**: Open TCP port 80 on the Web Server.
**Note 2**: If you encounter 403 Error - check permissions to your **/var/www/html** folder and also disable **SELinux sudo setenforce 0**. To make this change permanent, open **config file** with: 

```sudo vi /etc/sysconfig/selinux``` and set **SELINUX=disabled**, then restart httpd.




10.Update the website's configuration to connect to the database in **/var/www/html/functions.php file**. 

Apply **tooling-db.sql** script to your database using this command

```mysql -h <databse-private-ip> -u <db-username> -p <db-pasword> < tooling-db.sql>```


11. Create a new **admin user** in MySQL with username: **myuser** and password: **password**

* INSERT INTO **users** (**ID**, **username**, **password**, **email**, **user_type**, **status**)
* VALUES (**1**, **myuser**, **5f4dcc3b5aa1**, **user@mail.com**, **admin**, **1**)



Open the website in your browser **http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php** and make sure you can login into the websute with **myuser** user.










