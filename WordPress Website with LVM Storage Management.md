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

11. Then verify that the VG has been created successfully by runing ```sudo vgs```

![Screenshot 2023-11-29 200738](https://github.com/Saidat23/devops.pbl/assets/138054715/a394be9a-c7ea-4740-a5e0-ea3273ac9ab8)





































