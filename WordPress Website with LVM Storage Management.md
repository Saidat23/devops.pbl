# IMPLENENTING WORDPRESS WEBSITE WITH LVM STORAGE MANAGEMENT.
---
In this project, we will build and manage a scalable WordPress website using LVM(Logical Volume Management) Storage and AWS EC2. This is tailored to gain the knowledge and skills needed to deploy and maintain a robust WordPress site on the AWS cloud platform. It provide a step by step introduction to implenemt WordPress on AWS EC2 using Red Hat as the operating system.

We will set up an EC2 instance, configure security groups, and establish a reliable connection to your website. Through hands-on exercise, we will delve into the complexity of LVM storage management on Red Hat by creating logical volumes, managing disk spaces and dynamically resizing volumes to accommodate changing storage requirements. As part of this project, we will gain proficiency in installing and configuring WordPress, configure themes and adding essential plug-ins to enhance the website's functionality. We'll cover performance optimization techniques and best practice for securing your WordPress installation on AWS cloud. 

## UNDERSTANDING 3-TIER ARCHITECTURE
---
### Web Solution With WordPress
We will prepare storage infrastructure on two Linux server and implement a basic web solution using WordPress. WordPress is an open-source content management system written in PHP and paired with MySQL as its backend Relational Database Management System (RDBMS). This consist of two parts:
1. Configure storage subsystem for Web and Database servers  based on Linux OS. The focus of this part is to give practical experience of working with disks. partitions and volumes in Linux.
2. Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify your skills of deploying Web and DB tiers of Web solution.

### Three-tier Architecture
Web or mobile solutions are implemented based on Three-tier architecture. Three-tier Architecture is a client-server software architecture pattern that comprise of 3 different layers.<br/>
1. **Presentation Layer**(PL): This is the user interface such as the client server or browser on your Desktop/Laptop.
2. **Business Layer**(BL): This is the backend program that implements business logic such as Application or Webserver.
3. **Data Access or Management Layer**(DAL):This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server or NFS Server.  

![image](https://github.com/Saidat23/devops.pbl/assets/138054715/2ee76451-15a4-4675-b436-d4782e84d695)

### Implementing LVM on Linux server (Web and Database server).
**Step 1**: Prepare a Web Server
1. Launch 2 EC2 instance using Red Hat to serve as **Web Server** and **Database Server**.
   
   ![Screenshot 2023-11-29 192519](https://github.com/Saidat23/devops.pbl/assets/138054715/2cd9ac34-6fa9-4a1b-8dad-d7c3b4cc1229)
   
2. Create 3 volumes in the same Availability Zone as your EC2 Web Server and Database, each of 10GiB.
    Click on **Volumes** on the left side under the **Elastic Block Store**
     
  ![Screenshot 2023-11-29 192600](https://github.com/Saidat23/devops.pbl/assets/138054715/53a6291a-ca69-4a79-ba8b-2b1041c93410) 
  
Click on **Create Volume** at the top right corner of your page.  

![Screenshot 2023-11-29 192701](https://github.com/Saidat23/devops.pbl/assets/138054715/0affe765-3050-43d6-bbdc-a61e49cdcb49)

On the **Create Volume** page, select your volume size (10GiB) , your availability zone (same as your instance) and click on **Create Volume**. 

![Screenshot 2023-11-29 192806](https://github.com/Saidat23/devops.pbl/assets/138054715/85a01704-00dc-48cd-8fcb-407448cb6869)

3. Name the volumes 
   
![Screenshot 2023-11-29 193051](https://github.com/Saidat23/devops.pbl/assets/138054715/191398ec-a7e5-4c81-92d9-9346e8b481d2)

 Attach all the volumes one at a time to your EC2 instance Web Server. Select your volume, then click on **Action** on the top right corner of the page. On the drop down, click on **Attach Volume**. 
 
![Screenshot 2023-11-29 193010](https://github.com/Saidat23/devops.pbl/assets/138054715/df989f1b-9456-4382-a946-f5583fa35cce)

 On the **Attach volume** page, select your server and click on **Attach volume**.
  
![Screenshot 2023-12-14 204017](https://github.com/Saidat23/devops.pbl/assets/138054715/7489635c-ec7e-401d-86bb-ae413ffdc58a)

SSH into your terminal, 
![Screenshot 2023-11-29 192436](https://github.com/Saidat23/devops.pbl/assets/138054715/3603fe81-aaed-48be-9885-4e8f3d5dc7d7)

Use the **```lsblk```** command to inspect what block devices are attached to the server.

![Screenshot 2023-12-14 220628](https://github.com/Saidat23/devops.pbl/assets/138054715/3fd669a6-d484-413b-8139-c25b5b49f7a9)

 Inspect the /dev/directory with **```ls/dev/```** to be sure all three newly create block devices are there. Their names will likely be **xvdf, xvdg, xvdh**.

![Screenshot 2023-11-29 192506](https://github.com/Saidat23/devops.pbl/assets/138054715/e431e871-7ff0-42b4-8b75-76512f423508)

4. Use **```df -h```** command to see all mounts and free space on your server.

![Screenshot 2023-11-29 193223](https://github.com/Saidat23/devops.pbl/assets/138054715/be25223d-7553-4a4c-9660-b35eca26faae)

 Use **```gdisk```** utility to create a single partition on each of the disks.

 ``` sudo gdisk /dev/xvdf``` then hit enter on your keyboard.
 
 Type **n**  for **Command (? for help):** prompt, then hit enter.
 
 Type **1** for **Partition number (1-128, default 1):** prompt, then hit enter for the next three prompt. As we are not effecting any changes.
 Then type **8e00** to change partition to **Linux LVM**, Then hit enter.
 
 Type **p**  for **Command (? for help):** prompt, then hit enter.

 Type **w**  for **Command (? for help):** prompt, then hit enter.
 
 Type **y** for **Do you want to proceed? (Y/N):** prompt, then hit enter.
 
 Follow this steps for the other two partitioning.
 
 
 ``` sudo gdisk /dev/xvdg```
 
 ``` sudo gdisk /dev/xvdh```

![Screenshot 2023-11-29 193429](https://github.com/Saidat23/devops.pbl/assets/138054715/07f33efa-8731-46ee-8fd7-bb019a8239f3)

5. Use **```lsblk```** utility to view the newly configured partition on each of the 3 disks 


![Screenshot 2023-11-29 195232](https://github.com/Saidat23/devops.pbl/assets/138054715/db46af01-6d47-41a8-aebb-856161d80f2f)

6. Install LVM2 package using the command ``` sudo yum install lvm2 -y ```.


![Screenshot 2023-11-29 195517](https://github.com/Saidat23/devops.pbl/assets/138054715/2d5e8ec6-5030-4e8d-8c0e-02d09752dbfc)

Check if the lvm is installed with the command ```which lvm```

![Screenshot 2023-11-29 195534](https://github.com/Saidat23/devops.pbl/assets/138054715/68f41261-4501-430c-8c92-0ab184c290ea)

7. Mark each of the 3 disks as a physical volume to be used by LVM with the command ```sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1```.
8. 
![Screenshot 2023-11-29 195758](https://github.com/Saidat23/devops.pbl/assets/138054715/d1155b26-2532-47e3-aa1f-6fde1244911a)

9. Verify that the physical volume has been created successfully by running ```sudo pvs```.

 ![Screenshot 2023-11-29 195818](https://github.com/Saidat23/devops.pbl/assets/138054715/1928372f-1607-436a-b0dc-ed3d955d3276)

10. Add all 3 disks to a volume group (VG) using vgcreate. Name the VG **vg-webdata**.

Run the command ``` sudo vgcreate vg-webdata /dev/xvdf1 /dev/xvdg1 /dev/xvdh1 ```.

![Screenshot 2023-11-29 200738](https://github.com/Saidat23/devops.pbl/assets/138054715/d09b0c44-dee4-4af0-9a29-c365b97ce754)

11. Then verify that the VG has been created successfully by runing ```sudo vgs```

![Screenshot 2023-11-29 200738](https://github.com/Saidat23/devops.pbl/assets/138054715/a394be9a-c7ea-4740-a5e0-ea3273ac9ab8)

12. Use **lvcreate** utility to create 2  logical volumes (app-lv and logs-lv). Use half of the PV size for each of the logical volume. The App-lv will be used to store data for the Website while logs-lv will be used to store data for logs.
Run the command below to create the logical volume.

```sudo lvcreate -n apps-lv -L 14G vg-webdata ```

```sudo lvcreate -n logs-lv -L 14G vg-webdata```

To confirm that the Logical Volume has been successfully created, run ```sudo lvs```.

![Screenshot 2023-11-29 201057](https://github.com/Saidat23/devops.pbl/assets/138054715/466d2908-ab9c-4067-abdc-f0e087c9be08)

13. Use mkfs.ext4 to format the logical volumes with ext4 filesystem using this commands.

```  sudo mkfs.ext4 /dev/vg-webdata/apps-lv ``` 

![Screenshot 2023-11-29 204049](https://github.com/Saidat23/devops.pbl/assets/138054715/456c2038-42c5-4e38-ba8c-a9d0e6df0a03)

```  sudo mkfs -t ext4 /dev/vg-webdata/logs-lv ```

![Screenshot 2023-11-29 204125](https://github.com/Saidat23/devops.pbl/assets/138054715/4c766da6-609e-4961-8390-b772be6a7b74)

14. Create var/www/html directory to store website file with the command below

 ``` sudo mkdir -p /var/www/html ``` 
 
 ![Screenshot 2023-11-29 204559](https://github.com/Saidat23/devops.pbl/assets/138054715/6840d8b6-260f-44cd-a7fc-c2fe6447740f)
 
15. Create /home/recovery/logs to store backup of log data

``` sudo mkdir -p /home/recovery/logs ```

![Screenshot 2023-11-29 204837](https://github.com/Saidat23/devops.pbl/assets/138054715/17784d2f-1f87-4d44-85bf-30fe96ab6b97)

16. Mount /var/www/html on app-lv logical volume with the command below.
    
``` sudo mount /dev/vg-webdata/apps-lv /var/www/html/ ```

17. Use rsync utility to backup all the files in the log directory **/var/log** into **/ home/recovery/logs** ( It is required to backup the content of the log directory before mounting the file system )

Check the content of the log file before backing up with the command 

``` ls -l /var/log ```

![Screenshot 2023-11-29 205901](https://github.com/Saidat23/devops.pbl/assets/138054715/70372574-2323-4910-8ae4-772a09562f93)

``` sudo rsync -av /var/log /home/recovery/logs ```

![Screenshot 2023-11-29 210354](https://github.com/Saidat23/devops.pbl/assets/138054715/4ede1cb6-0474-43e5-beaa-b7098883574d)

18. Check that the backup is stored in the **/home/recovery/logs** using the command below.
    
``` sudo ls -l /home/recovery/logs/log ```

![Screenshot 2023-11-29 211438](https://github.com/Saidat23/devops.pbl/assets/138054715/8ca5216e-5676-41d1-862e-656a8d329d27)

19. Mount **/var/log** on **logs-lv** logical volume. Note that all the existing data on /var/log will be deleted hence we need to backup the data. Run the command below.
    
``` sudo mount /dev/vg-webdata/logs-lv /var/log ```

![Screenshot 2023-11-29 211739](https://github.com/Saidat23/devops.pbl/assets/138054715/4f924f54-c39f-47a0-aff6-8dfcc77198e8)

20. Restore log files back into /var/log directory with the command below.

 ``` sudo rsync -av /home/recovery/logs/log/ /var/log ```

![Screenshot 2023-11-29 213307](https://github.com/Saidat23/devops.pbl/assets/138054715/da70233e-6c7a-4055-867f-4bb013dad450)


Double check to see if the /var/log directory has been updated with the command  below.

``` sudo ls -l /var/log ```

![Screenshot 2023-11-29 213402](https://github.com/Saidat23/devops.pbl/assets/138054715/6273dce2-89b1-4cfc-a2bb-2c9a6d2e6241)

21. Update **/etc/fstab** file so that the mount configuration will persist after restarting the server. Use the **UUID** of the device to update the **/etc/fstab** file. Run the **blkid** command to retrive the **UUID**.

     ``` sudo blkid ``` 
 
   ![Screenshot 2023-12-16 193700](https://github.com/Saidat23/devops.pbl/assets/138054715/7bd98535-a0f1-4938-83ba-5ad36d402bda)

22. Create and open the /etc/fstab file with the command bellow.

``` sudo vi /etc/fstab ```

![Screenshot 2023-12-16 192852](https://github.com/Saidat23/devops.pbl/assets/138054715/6d10d781-7612-48ee-99dc-62fd6763afff)

  Then, update the **/etc/fstab**  in this format using your own **UUID** and remember to remove the leading and ending quotes.  

 UUID=c2cd2766-5c6c-4f39-9c4c-452894c47be2 ext4   -- for app-lv </br>
 UUID=aee3d295-b171-4a14-a50e-027c8ebf8712 ext4   -- for logs-lv

![Screenshot 2023-12-16 194552](https://github.com/Saidat23/devops.pbl/assets/138054715/14ac5ae1-dd03-41b8-8793-6836b386e923)

Exit the vim editor.

23. Test the configuration with ``` sudo mount -a ``` and reload the daemon with ``` sudo systemctl daemon-reload ```. Then, verify your setup by running **df -h**. Output must look like this.

![Screenshot 2023-12-16 194854](https://github.com/Saidat23/devops.pbl/assets/138054715/f61396f8-5a1e-4fd5-8903-02ab07b7447c)

## Imstalling WordPress and Configuring to use MySQL Database.

**Step 2**: Prepare the Database Server

The second Red Hat EC2 instance launched, will be the used as **DB Server**. Repeat same steps carried out in the Web Server but instead of **app-lv** create **db-lv** and mount it to **/db** directory instead of **/var/www/html/**.           
- Create Logical Volume and highmark 20G to it.   

![Screenshot 2023-11-29 222646](https://github.com/Saidat23/devops.pbl/assets/138054715/1ccb052f-37cc-4ea5-8ba4-7b6ee451650f)   

- Create a mount point.
  
![Screenshot 2023-11-29 222956](https://github.com/Saidat23/devops.pbl/assets/138054715/13fe549a-868f-4c77-8303-01cf50258680)
                                                                                                        
- Check the content of the **/db**.

![Screenshot 2023-11-29 223223](https://github.com/Saidat23/devops.pbl/assets/138054715/1f3bc42e-c43a-4821-bd0e-569eefd87ebd)

- Mount **/dev/vg-database/db-lv** on **/db**
  
![Screenshot 2023-12-17 010141](https://github.com/Saidat23/devops.pbl/assets/138054715/456706e1-43b5-41a6-b04d-dda3c54ee17b)

- Make the mount persistent.
  
![Screenshot 2023-12-16 231227](https://github.com/Saidat23/devops.pbl/assets/138054715/9854ded8-9402-4643-9a99-950373a95f6f)

- Copy the **UUID** and paste into the /etc/fstab file. Don't forget to remove the quote.

![Screenshot 2023-12-16 230927](https://github.com/Saidat23/devops.pbl/assets/138054715/64a7a897-f466-4ad1-861c-e6000fd1e58e)

![Screenshot 2023-12-17 010055](https://github.com/Saidat23/devops.pbl/assets/138054715/819ddf51-ae82-4611-83d2-1c99276e0616)

**Step 3**: Install WordPress on your Web Server EC2.

1. Update the repository with ``` sudo yum -y update```. 

![Screenshot 2023-12-08 214305](https://github.com/Saidat23/devops.pbl/assets/138054715/d01014db-753e-49ec-a98e-a1ba310263e3)

2. Install wget, Apache and its dependencies with the command below.

   ``` sudo yum -y install wget httpd php php-myqslnd php-fpm php-json ```

![Screenshot 2023-12-08 232450](https://github.com/Saidat23/devops.pbl/assets/138054715/bac49291-0563-4635-b6a8-b406cb4ff0cf)

3. Start Apache with the commands

    ```sudo systemctl enable httpd ```

   ```sudo systemctl start httpd ```

![Screenshot 2023-12-08 234218](https://github.com/Saidat23/devops.pbl/assets/138054715/c6c6897c-c80f-4cea-969a-f8fe5d1f030a)

4. Install PHP and its dependencies using the commands below.

   ```
       sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm 
       sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
       sudo yum module list php
       sudo yum module reset php
       sudo yum module enable php:remi-7.4
       sudo yum install php php-opcache php-gd php-curl php-mysqlnd
       sudo systemctl start php-fpm
       sudo systemctl enable php-fpm
       sudo setsebool -P httpd_execmem 1 
    ```

![Screenshot 2023-12-08 234253](https://github.com/Saidat23/devops.pbl/assets/138054715/c1b24ac7-59a2-4ed0-89b7-539eb362b684)
![Screenshot 2023-12-08 234348](https://github.com/Saidat23/devops.pbl/assets/138054715/57824a43-d4dd-418b-807b-141f1ed10343)

![Screenshot 2023-12-08 234412](https://github.com/Saidat23/devops.pbl/assets/138054715/a5086075-a871-4932-9549-9740a974a15d)

![Screenshot 2023-12-08 234558](https://github.com/Saidat23/devops.pbl/assets/138054715/bb295781-06ad-4de1-9074-442dffb9c17c)

![Screenshot 2023-12-08 234626](https://github.com/Saidat23/devops.pbl/assets/138054715/10a22df8-7988-4b29-b8a9-dbd8f49f59d0)

![Screenshot 2023-12-08 234654](https://github.com/Saidat23/devops.pbl/assets/138054715/0c5709de-f9dc-4446-8232-82e91589e5a1)

![Screenshot 2023-12-08 234725](https://github.com/Saidat23/devops.pbl/assets/138054715/d9731197-c1a2-48de-bb87-4f4b97ad5b26)

![Screenshot 2023-12-08 234827](https://github.com/Saidat23/devops.pbl/assets/138054715/9600aec7-f33b-4057-b372-d6ba153a27f9)

5. Restart Apache with the command ``` sudo systemctl restart httpd ```

![Screenshot 2023-12-08 235034](https://github.com/Saidat23/devops.pbl/assets/138054715/68eabb83-c333-4888-96cf-4fa80c4d1ec3)

6. Download WordPress on your WebServer and copy the WordPress to **/var/www/html** with the command below.

   ```mkdir wordpress
      cd   wordpress
      sudo wget http://wordpress.org/latest.tar.gz
      sudo tar xzvf latest.tar.gz
      sudo rm -rf latest.tar.gz
      sudo cp wordpress/wp-config-sample.php wordpress/wp-config.php
      sudo cp -R wordpress /var/www/html/
   ```
![Screenshot 2023-12-08 235830](https://github.com/Saidat23/devops.pbl/assets/138054715/bf5acc26-20e6-4f40-ba76-68107b885b1c)

7. Configure SELinux Policies as a form of security to give ownership to Apache in order to access the WordPress site using the command below.

   ```
    sudo chown -R apache:apache /var/www/html/wordpress
    sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
    sudo setsebool -P httpd_can_network_connect=1
   ```
   ![Screenshot 2023-12-09 000311](https://github.com/Saidat23/devops.pbl/assets/138054715/2079ad98-8f84-4aaf-a618-8254ddb9c760)

**Step 4**:  Install MySQL on your DB Server EC2

Update and Install MySQL on the database server with the command below.

``` sudo yum update ```

``` sudo yum install mysql-server ``` 

![Screenshot 2023-12-09 000939](https://github.com/Saidat23/devops.pbl/assets/138054715/7b73b3a2-7353-416f-8da5-c55e42e73046)

Then, confirm that the service is up and running with the command ```sudo systemctl status mysqld ```. 

![Screenshot 2023-12-09 001232](https://github.com/Saidat23/devops.pbl/assets/138054715/f970095c-b736-4aa6-adeb-029451a4906f)

In case it's not running, restart the service with the command ``` sudo systemctl restart mysqld ``` and enable it with the ``` sudo systemctl enable mysqld ``` command.

**Step 5**: Configrue Database to Work with WordPress.

1. Run the command ```sudo mysql ``` to access the database environment. 
2. Then, create database with the name **wordpress** using the command ``` CREATE DATABASE wordpress ; ``` .
3. Run the command ``` show databse ``` to confirm that your database is created successfully.

![Screenshot 2023-12-09 002443](https://github.com/Saidat23/devops.pbl/assets/138054715/cc15469e-e3b3-4036-8c62-1f13b333f2de)

4. Create a user with the name **myuser** and password **mypass** running the command below. Don't forget to insert the private IP address of the WebServer.

``` CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';```

5. Then grant the user the premission to access the WordPress with the command below.

``` GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>'; ```
   
    Run ```FLUSH PRIVILEGES;``` for the command to take immediate effect.

6. Exit MySQL with the command ```exit```.

![Screenshot 2023-12-18 193027](https://github.com/Saidat23/devops.pbl/assets/138054715/7101fa4b-c930-436b-9850-711e6e80db57)

**Step 6**: Configure WordPress to connect to remote database.

1.Edit security group to allow trafic into the database. Configure the inbound rule of the database to allow WebServer to access the content of the database server.

![Screenshot 2023-12-18 203358](https://github.com/Saidat23/devops.pbl/assets/138054715/e6e5a827-678a-4209-966a-a1d5e7bb0a53)

2. Install MySQL Client on your WebServer and test if you can connect to your database server by running the command below. Don't forget to insert the database server's private IP address.
   
``` sudo yum install mysql ```

![Screenshot 2023-12-09 004943](https://github.com/Saidat23/devops.pbl/assets/138054715/59c42118-b746-4afa-9f5f-cd9d73df1158)

``` sudo mysql -u admin -p -h <DB-Server-Private-IP-address> ```.

![Screenshot 2023-12-09 005022](https://github.com/Saidat23/devops.pbl/assets/138054715/908a35c1-200c-460a-9b86-dbffa62638e8)

3. Confirm  if you can successfully execute ```show database;``` command to see a list of existing databases.

![Screenshot 2023-12-09 005237](https://github.com/Saidat23/devops.pbl/assets/138054715/9f0a30fe-fdf1-42de-8b63-115e7faab4f6)

4. Change permissions and configuration so that Apache can use WordPress. Please refer back to **Step 3 no 7**.

5. Enable TCP port 80 in Inbound Rule configuration for WebServer EC2. Refer back to **Step 6**.

6. Try accessing from your browser, the link to your WordPress ```http://<Web-Server-Public-IP-Address>/wordpress/```

![Screenshot 2023-12-09 010141](https://github.com/Saidat23/devops.pbl/assets/138054715/14edb3bb-08a1-4ccf-a317-1442f94e6c90)

7. Configure WordPress to connect to the Database Server by editing the **wordpress** in the /var/www/html directory. Then edit the content of **wp-config.php** using the **vi** code editor.
    
![Screenshot 2023-12-09 010701](https://github.com/Saidat23/devops.pbl/assets/138054715/eadbe350-4332-4071-92df-11c1e86fcafb)

Insert the appropriate values in the **DB_NAME**, **DB_USER**, **DB_PASSWORD** and **DB_HOST**.

![Screenshot 2023-12-09 011048](https://github.com/Saidat23/devops.pbl/assets/138054715/5257203a-7355-4e77-b41a-982688cf0609)

8. Restart your service by running ``` sudo systemctl restart httpd ```.

![Screenshot 2023-12-09 011204](https://github.com/Saidat23/devops.pbl/assets/138054715/5641b35a-52c5-4485-a5f8-b83cb3ea244f)

9. Refresh your browser, select a language and click on **Continue**


![Screenshot 2023-12-09 011220](https://github.com/Saidat23/devops.pbl/assets/138054715/7efae8c0-94f7-4775-a4ce-eceb43bd6da2)

10. Fill in the information needed and click on **Install WordPress**

![Screenshot 2023-12-09 011421](https://github.com/Saidat23/devops.pbl/assets/138054715/de531f87-b74d-41ee-800a-241e47093980)

11.Click on **Log In** to access your Webpage.

![Screenshot 2023-12-09 011438](https://github.com/Saidat23/devops.pbl/assets/138054715/9c61557d-16e1-458f-a019-5d5dfe27b551)



![Screenshot 2023-12-09 012246](https://github.com/Saidat23/devops.pbl/assets/138054715/1a60cde6-f13c-44ce-9bd2-35c3a7503e8b)


