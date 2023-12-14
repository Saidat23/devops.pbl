# IMPLENENTING WORDPRESS WEBSITE WITH LVM STORAGE MANAGEMENT.
---
In this project, we will build and manage a scalable WordPress website using LVM(Logical Volume Management) Storage and AWS EC2. This is tailored to gain the knowledge and skills needed to deploy and maintain a robust WordPress site on the AWS cloud platform. It provide a step by step introduction to implenemt WordPress on AWS EC2 using Red Hat as the operating system.

We will set up an EC2 instance, configure security groups, and establish a reliable connection to your website. Through hands-on exercise, we will delve into the complexity of LVM storage management on Red Hat by creating logical volumes, managing disk spaces and dynamically resizing volumes to accommodate changing storage requirements. As part of this project, we will gain proficiency in installing and configuring WordPress, configure themes and adding essential plug-ins to enhance the website's functionality. We'll cover performance optimization techniques and best practice for securing your WordPress installation on AWS cloud. 

## UNDERSTANDING 3 TIER ARCHITECTURE
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

### Provision your EC2 instance.

