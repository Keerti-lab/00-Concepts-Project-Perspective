# Manual installation of project

Date: 07-08-2023

- Every application these days follow 3 tier architecture i.e. Web Tier, Application Tier and Database Tier
- Web Tier includes Load Balancer component as well

## Public IP, Private IP and How Internet works

- Each and every device that is connected to internet has public and private IP
- Till 1995, we only had Public IP
- After that we have Network Address Translation (NAT)
- Within a network, each device is assigned with a Private IP by the Network modem
- Devices that are connected with in a network can communicate with each other
- A modem is assigned with a Public IP by the ISP
- A DNS (Domain Name System) contains a list of domain names linked with its IP address
- ISP is responsible for resolving the IP address of a domain
- When a request is raised by the user, it goes through the following steps:
  1. Requests the modem
  2. Reaches the ISP provider
  3. ISP checks with all the available home networks
  4. If not, it will reach a **peering/exchange point** to check with all the ISP providers
      - All the ISP providers in the country are connected using an exchange point
  5. If not, it will reach other peering points in the world to fetch the IP address
  6. If not, there is no IP address linked with the domain name that we are trying to reach for

## Monolithic vs Microservices architecture

### Monolithic

- Earlier everything used to be present in one server
- A simple change also need to be deployed again
- Maintainence and monitoring is a big headache
- Each application that is developed is released as an **.ear** file i.e. enterprise archive on the server
- For every new change, the entire application needs to be changed and a new .ear file is generated
- This leads to downtime of the application
- Migration is very difficult in monolithic
- All the components should be programmed in one programming language
- Scaling is very difficult at the time of more traffic

### API based

- All the components are communicated using API calls
- Frontend always communicates with the backend through API and backend responds to the request with a JSON response
- Now, The applications are deployed using .war file i.e Web Archive
- To overcome the disadvantages with the Monolithic based architecture, Microservices were introduced
- With microservices, the communication is established using Private IPs and API calls
- Each component can be programmed in any programming language and expose its functionality using APIs
- Entire application is developed by the developers and they provide us with the documentation in which the dependencies information is mentioned

## Security Group

- A Security group is a firewall
- Here we have Inbound and outbound traffic information
- In inbound, we specify the rules for incoming traffic
- Similarly in outbound, we specify the rules for outgoing traffic

# Manual installation of project

## 3 Tier Architecture

- Web Tier / Frontend
- App / API / backend
- DB Tier

## AWS

- AWS resources (such as servers, databases, Load Balancer) are mostly region based i.e. one resource cannot be transferred from one region to another region
- There are very few services (such as IAM) are global
- Every cloud as Regions and Availability zones
- N. Virgina (us-east-1) is the cheapest amongst all the regions
- AZs are used for Disaster Recovery purpose and High Availability
  - Therefore, we create our resources in 2 Azs
  - We can move our resources from one AZ to another AZ
- Each region has a minimum of 2 AZs

### What is an AMI?

- Amazon Machine Image
- Use Case: We frequently need NGiNX servers in our projects
- There are multiple steps involved in it such as OS + Install Package + Configuring service/package
- Rather than repeating these steps, we can create an image from the server i.e. capturing everything inside the server
- If we take an image from an instance when it is in running state, there is a possibility for altering its state
- Therefore its always better to stop the instance and create an image from the instance
- Actions -> Image and Templates -> Create image

## Who choose what?

- Developers and architects choose which programming language, tools and its versions to use in the project ?
- DevOps architects choose which DevOps tools to use and which version to use?

## Course lab

- We use custom AMI's such as Centos-8-DevOps-Practice AMI (AMI ID: ami-03265a0778a880afb) which is present in the community section
- It has password authentication enabled
- The username is centos and password is DevOps321

## Build process

- Developer writes the code, compiles and builds it to generate a package
- Compile - Check for syntax errors
- Once the build is complete, the package is generated as per the programming language used
  - For e.g. An application written in Java generates a .jar file, other programming languages generate .zip file
- Once the package is generated, we store them in Nexus Repository or S3 etc
- Finally we fetch the packages and deploy them inside the server

## Deployment Process

1. Create server
2. Create environment such as NodeJS, Golang, Java etc
3. Fetch the package and install dependencies
4. Create a service
5. Some Configuration
6. Finally run the application

## NGiNX

- NGiNX is a webserver and serves as a reverse proxy
- The default configuration of NGiNX is present inside `/etc/nginx/nginx.conf`
- User defined configuration can be placed under `/etc/nginx/default.d/` directory
- **When ever we make any change to the configuration file, we should restart our server**
- Logs are present inside `/var/log/nginx`
- To view the real-time logs, we can use: `tail -f /var/log/nginx/access.log`
  - `-f` - follow
- There are 2 types of proxy:
  1. Forward Proxy:
      - E.g. when connecting to the internet, it passes through a proxy server (i.e. on behalf of you) that checks whether the request can be allowed or not, VPN
      - Configuration is client-centric i.e. only client is aware that there is a proxy configured
      - Content filtering and Access control
      - Caching: Frequently accessed content can be cached inside the proxy server of the ISP
  2. Reverse Proxy:
      - Server centric i.e. clients are not aware of the proxies on the server side
      - For e.g. they can only view the web tier but inside that we configure our nginx server to route the traffic to a particular server
      - Load Balancer
      - SSL / HTTPS
      - Security purpose

## MongoDB

- RDBMS: Relation Database Management System
  - SQL based i.e. we have rows and columns
  - E.g. PostgreSQL, MySQL etc
- NoSQL:
  - Stricly no SQL
  - No tables, no rows and colums
  - Rather the data is based on documents and collections which are JavaScript Object Notation (.json) files
  - E.g. MongoDB
- When installing mongodb-org package with yum, it looks for its source from where it can download inside `/etc/yum.repos.d/` directory
- If its unable to find, we need to specify the source file (i.e. .repo file) inside this directory
- After installation, if we want to check if the mongodb installation is successful or not using: `netstat -lntp` or `ps -ef | grep mongo` or `systemctl status mongod`
- MongoDB opens the port 27017
- By default some severs mostly DB servers runs on localhost (127.0.0.1) for security reasons
- But for our practice purpose, we will open it to accept all traffic from any IPv4 by changing the **bindIp** from 127.0.0.1 to 0.0.0.0 inside `/etc/mongod.conf`
- After that we need to restart the mongod service

## Node js

- It is microservice developed in NodeJS by the developer
- What it does? It connects to the MongoDB, fetches the products and shows on the frontend
- To work with this NodeJs application, we need a NodeJS environment
- To run the application, we need to install all the necessary dependencies/packages that are necessary for it to run
- Different build tools specify the list of dependencies in different files:
  1. Maven: pom.xml
  2. nodejs/any js: package.json
  3. python: requirements.txt

### Different types of users

- Normal user: Human
- System users (non-human): Instead of running the applications as a root user, applications have dedicated users to run the application for security reasons

# Manual installation of project

## 3 Tier Architecture

- DB Tier
  - Also called as stateful
  - State represents the data
  - The data should be persistent i.e. it should be securly stored, taken backups regularly etc
- App / API / backend
  - Performs CRUD operations on DB Tier which are Stateless in nature
- Web Tier / Frontend
  - Fetch data from API Tier
  - Also Stateless in nature

## Systemd service

- Linux looks for the services inside this directory at the time of start: `/etc/systemd/system/`
- If a service is placed in this directory, we can enable or disable it using `systemctl` command options
- The applications that are created by `yum` package manager has the service file inside this directory
- We need to create service files inside this directory for our user-defined applications such catalogue, user etc inorder to manage it as native systemctl service

## Node Js

- DB Tier are heavy applications, suggested to use t3.micro
- For any 3 Tier application, we have an admin site to make entries inside the database
- Developer also provides with the schema and sample data inside the schema/catalogue.js file
- Mongodb-org is the server and mongodb-org-shell is the client to load the data into the server
- `mongo --host <IP-address> < <location-of-schema-file>`
- nodejs usually runs on port 8080

## Status codes

- 2** - Success
- 3** - redirection
- 4** - client side error: Not giving proper URL
- 5** - server side error: There is something wrong inside server/code

# Manual installation of project

Date: 11-08-2023

## Release another version

1. Stop the server
2. Delete the old package
3. Place the new package
4. Restart the server

- SOP: Standard Operating Procedure
- Each project has a standard documentation called as SOP which has the information on installing the necessary tools

## Redis

- It is a popular cache server and in-memory database
- Querying the database is an expensive operation because no: of steps that it needs to go through, the latency increases.
  1. Connect to DB
  2. Fetch the data
  3. Close the Connection
- To reduce the latency, a cache server is used which stores the data in a key-value pair format after the first request
- RPM: Redhat Package Manager
- All packages in RedHat are RPM's
- What is the difference between YUM and RPM?
  - `yum` helps us with dependency resolution before installing the requested package
- Starting from CentOS 8, Redis provided with a single repo file with which we can either upgrade or downgrade the versions easily by enabling them at the time of installation

## Node js

- Similar approach like catalogue as its written in NodeJS
- This also depends on MongoDB and Redis

## node js

- Similar to User and catalogue i.e. written in NodeJS
- Depends on Redis and Catalogue

## MySQL

- We can have a larger instance type i.e. for e.g. t3.micro
- By default, CentOS 8 yum package manager installs MySQL v8 but our project is dependent on MySQL v5.7
- Therefore, we disable it and manually configure MySQL repo

## java

- Depends on MySQL for loading the country and city information
- Maven is build tool for Java
- When we install Maven, it will install Java automatically
- `mvn clean package` - Installs dependencies and creates a .jar package inside **target/** directory
  - .jar - Java Archive file
- Maven installs dependencies that are listed in `pom.xml` file

# Manual installation of project

Date: 14-08-2023

## RabbitMQ

- Its one of the popular Messaging Queue Software
- ActiveMQ, Kafka are some other message queue softwares
- AMQP: Advanced Messaging Queue Protocol

### Synchronous and Asynchronous communication

- In Synchronous communication, we get an immediate response
- When a HTTP request is sent and if an immediate response is exepected to receive, then it is said to be Synchronous in nature
- What if the server is down? at that point we will not get any response from the server
- Lets consider the case of what happends with WhatsApp?
- If the other person is unavailable, the message that we send will be lost incase of HTTP Synchronous request
- To overcome this issue, WhatsApp implemented a **MQ Sever**
- Any message that is sent is first stored in the MQ Server
- Any user that is registered with WhatsApp is also subscribed the MQ Server
- A MQ Server consists of two things.
  1. Queue: Point-to-point communication (One to One)
  2. Topic: Publish and subscribe (One to Many)

#### Queue

- Each user has his own queue inside the MQ Server and is subscribed to that queue
- When a message is sent to a user, it will be stored inside his respective queue
- If the user is online, the messages will be send to him
- If he is offline, the messages are stored in his queue for certain number of days (e.g. 30 days) and will be delivered to him once he is online

#### Topic

- Let's understand the analogy using Youtube
- When a user is subscribed to a channel, he gets notified once a new content is published to that channel
- Within Youtube server, there is topic for each channel and each and every user subscribe to that channel are subscribed to that topic
- Any content that is published on the channel will trigger a message to the topic with which all of its subscribers are notified immediately
- All subscribed users are actively listening to the topic continously because of which a notification is sent to them at a very fast pace

## python

- Written in Python
- Dependent on Cart, User and Rabbitmq

## Dispatch

- Always listens to Payments Queue for product and shipping information
- Developed in Golang

## Disadvantages with Manual installation

1. Everytime we need to configure same steps of creating the microservice (Ansible)
2. We should map the IP address to a Domain name as the Public IP changes when we restart the server
3. What if the traffic is more? We need to scale the severs ---> Autoscaling
4. We need to monitor for errors. System resources should be monitored
5. Resource Utilisation --> Min. servers but more performance ---> Monitoring
6. Automatic Deployment (CI/CD) -> Whenever there is a new release, we need to:
    - Compile
    - Package
    - Scan
    - Remove old version
    - Release new version
7. Automate the infrastructure creation ---> Using terraform
8. We should install HTTPS certificate for our domain names

## How DNS works?

- When we enter an website address in a browser, it will follow this process for finding the IP address of that website
  1. Check the browser cache if is already present or not
  2. Check in OS Cache and OS is configured with the ISP information
  3. Check with ISP through Modem, if its present in its DNS server and its DNS Cache
  4. ISPs are configured with Root servers and therefore they reach out to them for the IP address of the website
     - Root servers are managed by independent organisations which is called as ICANN (Interent Coorporation of Assigned Names and Numbers)
     - ICANN manages the DNS Root servers
     - IANA: Internet Assigned Numbers Authority
     - Root servers contains top level domains (TLDs) such as .com, .us, .in, .net etc
     - When we purchase a domain from a domain broker, its their responsiblity to update root servers about this domain
     - In addition that, a domain name is also allocated with Name servers i.e. who is managing the domain
     - Root servers provide the information about the name servers that are linked with the website address
  5. Finally ISP contacts the Nameservers for the IP address and sends it back to the client
- We need to always create records for associating an IP address to a web address
- But we need to change the ownership of the Name Servers to AWS for automation purpose using Route53 service in AWS
- Once we made the change, hostinger will connect with the root severs and updates the change in name servers
- TTL: Time to Leave is similar to Cache i.e. how long the information related to the record should be there

## Types of Records

- A Record: domain name pointing to a IPv4 address
- CName Record:
- 


