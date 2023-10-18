# System Design

## Scale the system
<img width="485" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/e31871c9-a48e-4a59-9c1e-2ec5e9c8f8c1">

<img width="517" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/d701e7f5-84c5-405e-b145-f2da780c2d5a">

- If web server goes down or we server reaches its load limit, users may fail to connect to the server. Thus need to add a load balancer:
<img width="497" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/26bc2e58-2963-4e87-b14b-43e405a94cf7">

- Add database replications:
<img width="419" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/59f83e94-a506-41d0-bc7b-fb6a5c4ad93e">

Then:
<img width="452" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/0ce4bd82-2c73-4ad8-b53a-de019e3f4935">
• A user gets the IP address of the load balancer from DNS.
• A user connects the load balancer with this IP address.
• The HTTP request is routed to either Server 1 or Server 2.
• A web server reads user data from a slave database.
• A web server routes any data-modifying operations to the master database. This includes
write, update, and delete operations.

- To improve the load/response time:
<img width="536" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/f54425e0-d893-43b2-9ab4-3afeb9f33293">



### Databases
Main difference between SQL and NoSQL databases is that: NoSQL doesn't support **join** operations.

### Vertical & Horizontal Scaling
Vertical means adding more CPU, RAM etc. power to each server. 

Horizontal means adding more servers.

### Load balancer
Users connect to the public IP of the load balancer, web servers are unreachable directly anymore.

Private IP is reachable only between servers in the same network.

### Database replication
**Master (original) and slaves(copies)** relationship. 

Master database generally only supports write (data modifying) operations, slaves support only read operations.

More slaves will allow more quries to be processed in parallel.

If the master database goes offline, a slave database will be elected to be the new master (and **run data recovery scripts** in case the slave's data is not up to date) and a new slave will be added to replace the old one.

### Cache
#### Read-through cache
<img width="535" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/ca6756dc-2045-43db-9fd1-14d1b2248af3">

If cache restarts, all the data in cache memory will be lost, thus cache is not for persistent data stores.

There can be **inconsistency** between the database and cache. 

### Content Delivery Network (CDN)
A CDN is a network of geographically dispersed servers used to deliver **static content**. CDN servers cache static content like images, videos, CSS, JavaScript files, etc.

When a user visits a website, a CDN server closest to the user will deliver static content. 

<img width="460" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/a12ba7ed-d7d7-45a9-8ec9-07404593304d">
<img width="545" alt="image" src="https://github.com/XinyuKang/techInterviewPrep/assets/46883505/53ed9e0f-a8ec-4d80-b3e5-6ceca96aab3c">

The difference between CDN and cache is that **CDN is located closer to end users than origin servers and cache is located closer to the origin servers**.

- Note that **static contents**  refers to files and data that don't change or get generated dynamically as a result of a specific user action or a server-side computation. They are served as-is to the user. 





