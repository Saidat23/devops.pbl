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


On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage. Though, the NFS server might be located on a completely separate hardware, for Web Servers it looks like a local file system from where they can serve the same files.

![Screenshot 2024-01-04 161025](https://github.com/Saidat23/devops.pbl/assets/138054715/6975ca03-b4a9-426d-b45c-6d0c6d2b42ef)

It is important to know what storage solution is suitable for what use cases, what data will be stored, in what format, how this data will be accessed, by whom, from where, how frequently, etc. to be able to choose the right storage system for your solution.



**Step 1- Prepare NFS Server**


1. Spin up a new EC2 instance with RHEL Linux 8 Operating System.
* Click on **AMI**. 
![Screenshot 2024-01-04 171814](https://github.com/Saidat23/devops.pbl/assets/138054715/d41fc5a6-4d41-42ca-a543-3384dc5f92d2)

* Select **Public images**, type in the **RHEL Version** on the search to select it then, **Launch Instance from AMI**.

![Screenshot 2024-01-04 172022](https://github.com/Saidat23/devops.pbl/assets/138054715/30789c20-167e-47b0-ba01-22e9e34bd04f)

* Select **Instance type**, **Key pair**, create or select **security group**, **Number of instances** and **Launch instance**.
  
  ![Screenshot 2024-01-04 200727](https://github.com/Saidat23/devops.pbl/assets/138054715/dc29d674-8d38-4003-a502-425faa1bccfd)
  
![Screenshot 2024-01-04 203000](https://github.com/Saidat23/devops.pbl/assets/138054715/c34ab5bf-ec51-4060-8d1a-c4407fc0f60d)

* Launch Ubuntu server for the database.

![Screenshot 2024-01-04 215830](https://github.com/Saidat23/devops.pbl/assets/138054715/519eb2de-3495-4a6f-8b87-de2f1701fa3c)
![Screenshot 2024-01-04 220119](https://github.com/Saidat23/devops.pbl/assets/138054715/9562a72a-7fda-47f3-959d-f4671df7bc91)

* Create **3 Volumes**, name and attach them to the **NFS** server.
  
  ![Screenshot 2024-01-04 222019](https://github.com/Saidat23/devops.pbl/assets/138054715/25048569-6ce9-4b0b-847b-b18dcd53901e)
  
![Screenshot 2024-01-04 222311](https://github.com/Saidat23/devops.pbl/assets/138054715/0a96c14b-8ee8-4c0c-acdf-be0561349319)

![Screenshot 2024-01-04 222341](https://github.com/Saidat23/devops.pbl/assets/138054715/7985dc75-16f2-47b2-8a92-9812e85b96a3)

2. Based on your **LVM** experience from  [WordPress Website](https://github.com/Saidat23/devops.pbl/blob/main/WordPress%20Website%20with%20LVM%20Storage%20Management.md), Follow steps till creating logical volume.

* Ensure there are 3 Logical Volumes. **lv-opt**, **lv-apps** and **lv-logs**
  
![Screenshot 2024-01-05 012809](https://github.com/Saidat23/devops.pbl/assets/138054715/0183c1a9-1c83-484f-b2b9-82924d1f9d45)
![Screenshot 2024-01-05 012855](https://github.com/Saidat23/devops.pbl/assets/138054715/b3581715-7bbe-4832-b19d-bb2816dd2f38)

* Instead of formating the disks as **ext4** you will have to format them as **xfs**
  
![Screenshot 2024-01-05 013420](https://github.com/Saidat23/devops.pbl/assets/138054715/5aa48e87-0424-453b-8f9a-fdd6174a13f2)

3. Create mount points on **/mnt** directory for the logical volumes as follow:

Mount **lv-apps** on **/mnt/apps**  - To be used by webservers<br/>
Mount **lv-logs** on  **/mnt/logs** - To be used by webserver logs</br>
Mount **lv-opt**  on  **/mnt/opt**  - To be used by Jenkins server in the next project.

![Screenshot 2024-01-05 014235](https://github.com/Saidat23/devops.pbl/assets/138054715/295ee0ef-1d4d-4c29-8ea6-e0a785ff42cf)


4. Install **NFS server**, configure it to start on reboot and make sure it is up and running with the commands below.

```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![Screenshot 2024-01-05 014354](https://github.com/Saidat23/devops.pbl/assets/138054715/ae164179-e818-451d-9557-a855a3ecf02e)

![Screenshot 2024-01-05 022035](https://github.com/Saidat23/devops.pbl/assets/138054715/0bff8e35-0891-45d8-9295-6035f4b0283c)

![Screenshot 2024-01-05 022221](https://github.com/Saidat23/devops.pbl/assets/138054715/e235ad35-e5b8-440e-a2e2-899ea1130838)

Export the mounts for webservers' **subnet cidr** to connect as clients. All the three Web Servers will be installed  inside the same subnet, but in a production set-up you will probably want to separate each tier inside its own subnet for higher level of security.

To check your **subnet cidr**, open your EC2 details in AWS Console and locate **Networking** tab. Open a Subnet link:

![Screenshot 2024-01-05 020218](https://github.com/Saidat23/devops.pbl/assets/138054715/c8d267ed-168f-4b5c-90af-0f615bc562a3)

![Screenshot 2024-01-05 020240](https://github.com/Saidat23/devops.pbl/assets/138054715/6ecae25f-5f8a-4953-bd01-d2abb90a5588)

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

![Screenshot 2024-01-05 022737](https://github.com/Saidat23/devops.pbl/assets/138054715/be5b93b8-9ab9-4659-8ba5-41adeb1e4172)

Configure access to **NFS** for clients within the same subnet (example of Subnet CIDR - 172.31.32.0/20 ):

```
sudo vi /etc/exports

/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

sudo exportfs -arv
```
![Screenshot 2024-01-05 181504](https://github.com/Saidat23/devops.pbl/assets/138054715/debd234a-512f-4b80-9b3d-c3105450fca4)

![Screenshot 2024-01-05 023225](https://github.com/Saidat23/devops.pbl/assets/138054715/bb3e3f27-5027-466f-a7a7-d1784c1aef84)

![Screenshot 2024-01-05 023541](https://github.com/Saidat23/devops.pbl/assets/138054715/892f3023-c2e8-463b-995c-482af5a76320)

Check the port used by **NFS** and open it using Security Groups (add new Inbound Rule)

```rpcinfo -p | grep nfs```

![Screenshot 2024-01-05 023657](https://github.com/Saidat23/devops.pbl/assets/138054715/ca4930c6-a31d-40d3-bc30-51c3c415537e)

To access the **NFS** server from your client, open the following ports: **TCP 111**, **UDP 111**, **UDP 2049** and **TCP 2049**

![Screenshot 2024-01-05 024040](https://github.com/Saidat23/devops.pbl/assets/138054715/80ed4e40-f7fc-4e3e-ad01-b907944214f4)

![Screenshot 2024-01-05 024343](https://github.com/Saidat23/devops.pbl/assets/138054715/192b3547-c2ee-47b7-8fdd-502db0d43ea1)

**Step 2 — Configure the database server**

1. Install **MySQL** server
  
2. Create a database and name it **tooling**

3. Create a database user and name it **webaccess**

4. Grant permission to **webaccess** user on **tooling** database to do anything only from the webservers **subnet cidr**

![Screenshot 2024-01-05 021800](https://github.com/Saidat23/devops.pbl/assets/138054715/04399155-bad8-493d-bb16-21ad55138242)

**Step 3 — Prepare the Web Servers**

We need to make sure that our Web Servers can serve the same content from shared storage solutions- NFS Server and MySQL database. For storing shared files that our Web Servers will use, we will utilize **NFS** and mount previously created Logical Volume **lv-apps** to the folder where **Apache** stores files to be served to users **(/var/www)**.
This approach will make our Web Servers stateless, which means we will be able to add or remove new ones when needed, and the integrity of the data (in the database and on NFS) will be preserved.

* Configure NFS client on all three servers.

* Next, deploy a Tooling application from the Web Servers into a shared NFS folder.

* Then, configure the Web Servers to work with a single MySQL database.



1. Launch a new EC2 instance with **RHEL 8 Operating System**


2. Install NFS client running the command below.

```sudo yum install nfs-utils nfs4-acl-tools -y```

![Screenshot 2024-01-05 024835](https://github.com/Saidat23/devops.pbl/assets/138054715/24bfc189-b02e-497e-89c1-7132fd645957)


3. Mount **/var/www/** and target the NFS server's export for apps with the command,
   
```
sudo mkdir /var/www

sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
```

4. Verify that NFS was mounted successfully by running ```df -h```.
   
![Screenshot 2024-01-05 025443](https://github.com/Saidat23/devops.pbl/assets/138054715/c907e352-9e8e-41bc-92e3-e949d0ec6da5)

![Screenshot 2024-01-05 025557](https://github.com/Saidat23/devops.pbl/assets/138054715/2db16033-aa6c-4ca5-9a37-cbc87783be6f)

 Make the changes persist on your Web Server after reboot by opening your **/etc/fstab** to edit it. Run the command below.

```sudo vi /etc/fstab```

Add the following line 

```<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0```

![Screenshot 2024-01-05 184432](https://github.com/Saidat23/devops.pbl/assets/138054715/bab39841-05ae-4532-ab1e-683adef20ebf)

5. Install **Remi's repository**, Apache and PHP

```sudo yum install httpd -y```

![Screenshot 2024-01-05 030253](https://github.com/Saidat23/devops.pbl/assets/138054715/faebc52d-95f9-4ca3-aec0-955714962618)

![Screenshot 2024-01-05 030503](https://github.com/Saidat23/devops.pbl/assets/138054715/0aaecde3-0200-4a95-a346-b186b4710338)

```sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm```

![Screenshot 2024-01-05 030938](https://github.com/Saidat23/devops.pbl/assets/138054715/5232a3c1-a6cf-4098-8e5e-d45b193b2825)

```sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm```

![Screenshot 2024-01-05 031105](https://github.com/Saidat23/devops.pbl/assets/138054715/4be23837-107e-4581-8d9d-055be7205399)

![Screenshot 2024-01-05 031422](https://github.com/Saidat23/devops.pbl/assets/138054715/7aad5e9b-7081-4212-a33b-3c143bd82d1c)

```sudo dnf module reset php```

![Screenshot 2024-01-05 031642](https://github.com/Saidat23/devops.pbl/assets/138054715/c30cbc4c-a35b-4954-8882-baf04b2b4bdf)

```sudo dnf module enable php:remi-7.4```

![Screenshot 2024-01-05 031741](https://github.com/Saidat23/devops.pbl/assets/138054715/3e62dddd-e497-4988-801c-be012876a5df)

```sudo dnf install php php-opcache php-gd php-curl php-mysqlnd```

![Screenshot 2024-01-05 031852](https://github.com/Saidat23/devops.pbl/assets/138054715/50ed97b5-3c24-4c8a-82ba-658cff89f423)

```sudo systemctl start php-fpm```

```sudo systemctl enable php-fpm```

```setsebool -P httpd_execmem 1```

![Screenshot 2024-01-05 032122](https://github.com/Saidat23/devops.pbl/assets/138054715/eec9b9a9-0572-4727-beec-b8e882800b19)

Repeat **steps 1-5** for the other 2 Web Servers.


6. Verify that Apache files and directories are available on the Web Server in **/var/www** and also on the NFS server in **/mnt/apps**. If the same files are present, that means NFS is mounted correctly.

![Screenshot 2024-01-05 032529](https://github.com/Saidat23/devops.pbl/assets/138054715/4c155ae0-a257-4dd1-8262-437b1b03a6fc)

![Screenshot 2024-01-05 032559](https://github.com/Saidat23/devops.pbl/assets/138054715/d05a9742-5934-4fa3-8cd9-bee380b56000)

7. Create a new file **touch test.txt** on one server and check if the same file is accessible from other Web Servers.

![Screenshot 2024-01-05 032939](https://github.com/Saidat23/devops.pbl/assets/138054715/28faa410-3652-40b0-a740-bfae5d275601)

![Screenshot 2024-01-05 033022](https://github.com/Saidat23/devops.pbl/assets/138054715/b5f6f2a4-62fb-499b-be54-99d8731fcbf0)

8. Locate the log folder for **Apache** on the **Web Server** and mount it to **NFS server's** export for logs. Repeat **step 4** to make sure the mount point will persist after reboot.

![Screenshot 2024-01-05 040835](https://github.com/Saidat23/devops.pbl/assets/138054715/3e7238da-fbc3-4629-860b-f4f25611d741)


9. Fork the **tooling source code** from **[A Github Account](https://github.com/darey-io/tooling)** to your Github account. Watch how to fork a repo [here](https://www.youtube.com/watch?v=f5grYMXbAV0).

![Screenshot 2024-01-05 041308](https://github.com/Saidat23/devops.pbl/assets/138054715/6c37ae7f-6b42-4855-82e8-48bd6fcd8fff)

![Screenshot 2024-01-05 041349](https://github.com/Saidat23/devops.pbl/assets/138054715/75e27a8c-0ab1-42dd-b769-82156f85fd52)

![Screenshot 2024-01-05 041444](https://github.com/Saidat23/devops.pbl/assets/138054715/c21b242d-9d84-411a-bce9-23687e586a4a)

![Screenshot 2024-01-05 041507](https://github.com/Saidat23/devops.pbl/assets/138054715/a4fa61a3-895b-4c02-b668-3056e1c4e246)

10. Deploy the **tooling website's code** to the **Webserver**. Ensure that the **html folder** from the repository is deployed to **/var/www/html**.

![Screenshot 2024-01-05 041636](https://github.com/Saidat23/devops.pbl/assets/138054715/14aee8f2-0232-493c-86e9-7b7fc2f7ed64)


**Note 1**: Open TCP port 80 on the Web Server.
**Note 2**: If you encounter 403 Error - check permissions to your **/var/www/html** folder and also disable **SELinux sudo setenforce 0**. To make this change permanent, open **config file** with: 

```sudo vi /etc/sysconfig/selinux``` and set **SELINUX=disabled**, then restart httpd.

![Screenshot 2024-01-05 042827](https://github.com/Saidat23/devops.pbl/assets/138054715/5581c124-2e69-4979-82e0-1d7e4feec0f5)

![Screenshot 2024-01-05 042616](https://github.com/Saidat23/devops.pbl/assets/138054715/cd36f144-ddc3-4b85-a6c8-8ca56b0275ad)


11.Update the website's configuration to connect to the database in **/var/www/html/functions.php file**. 

![Screenshot 2024-01-05 043729](https://github.com/Saidat23/devops.pbl/assets/138054715/8db06253-d6e6-441f-b900-904f3f2568a9)

![Screenshot 2024-01-05 043441](https://github.com/Saidat23/devops.pbl/assets/138054715/20fa44b7-1720-4f26-8c36-cbae3f9bf871)

Apply **tooling-db.sql** script to your database using this command

```mysql -h <databse-private-ip> -u <db-username> -p <db-pasword> < tooling-db.sql>```

![Screenshot 2024-01-05 044256](https://github.com/Saidat23/devops.pbl/assets/138054715/c02da507-69a2-42fc-935b-9c4d77964d7e)

![Screenshot 2024-01-05 044649](https://github.com/Saidat23/devops.pbl/assets/138054715/4471d557-cf80-4ef6-b12f-3f48c91574bd)

12. Create a new **admin user** in MySQL with username: **myuser** and password: **password**

* INSERT INTO **users** (**ID**, **username**, **password**, **email**, **user_type**, **status**)
* VALUES (**1**, **myuser**, **5f4dcc3b5aa765d61d8327deb882cf99**, **user@mail.com**, **admin**, **1**)

![Screenshot 2024-01-05 045840](https://github.com/Saidat23/devops.pbl/assets/138054715/e58e2af4-d60e-4ce6-9ee2-8f858c1a5f42)


Open the website in your browser **http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php** and make sure you can login into the websute with **myuser** user.


![Screenshot 2024-01-05 050050](https://github.com/Saidat23/devops.pbl/assets/138054715/8992e870-e26d-44e6-bd41-a022fefdd969)

![Screenshot 2024-01-05 050742](https://github.com/Saidat23/devops.pbl/assets/138054715/4a006a28-ebcf-484e-af60-b57931e6039b)







