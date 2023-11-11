# IMPLEMENTING LOAD BALANCING WITH NGINX.
---
## INTRODUCTION TO LOAD BALANCING AND NGINX.
## LOAD BALANCING

Load balancing is the efficient distribution of incoming network traffic across a group of backend servers.

Modern websites with high‑traffic that serves hundreds of thousands of concurrent requests from users or clients requires adding more servers to meet these high volumes to respond correctly to text, images, video, or application data, in a fast, reliable and cost‑effective  manner.

A load balancer acts as the “traffic cop” positioned in front of your servers and routing client requests across all servers capable of fulfilling those requests in a manner that maximizes speed and capacity utilization. It ensures that no server is overworked to prevent degrading performance. If a single server goes down, the load balancer redirects traffic to the remaining active servers. And when a new server is added to the server group, the load balancer automatically starts to send requests to it.

### Functions of a load balancer:

- Distributes client requests or network load efficiently across multiple servers<br/>
- Ensures high availability and reliability by sending requests only to servers that are online<br/>
- Provides the flexibility to add or subtract servers as demand dictates<br/>

### Load Balancing Algorithms
Load  balancer algorithms are techniques used to distribute incoming network traffic across multiple servers, ensuring efficient utilization of resources and improving overall system performance, reliability and availability. Load balancing algorithms provide different benefits and the choice of load balancing method used depends on your needs. Here are some common load balancer algorithms.

**Round Robin** – Requests are distributed across the group of servers in the pool sequentially. It is simple to implement and ensures an even distribution of the traffic. It works well when the servers have capabilities and resources that are similar.<br/>
**Least Connections** – A new request is sent to the server with the fewest current connections to clients. The relative computing capacity of each server is factored into determining which one has the least connections.<br/>
**Weighted Round Robin** - This is similar to the Round Robin algorithm, but the servers are assigned different weights based on their capabilities. Servers with higher performance level receives more request.<br/>
**Weighted Least Connections** - This is similar to the Least Connections Algorithm, servers are assingned different weights based on their capabilities. Servers with higher capacities receives more connections.<BR/>
**IP Hash** – Uses the Hash function based on the IP address of the client to consistently map the client to a specific server. This ensures that the same client always reaches the same server.


### Load Balancing Benefits 

-Reduced downtime.<br/>
-Scalable<br/>
-Redundancy<br/>
-Flexibility<br/>
-Efficiency<br/>

## NGINX

NGINX is an open source software for web serving, caching, reverse proxying, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers. Nginx configuration depends on the use case.
## SETTING UP A BASIC LOAD BALANCER.
We will provision three EC2 instances running ubuntu 22.04 and install apache webserver in two and Nginx in the third one. In the two instances with apache, we will


