# Responsibilites

- Deploy the application as fast as possible with speed and accuracy
- Monitor the applications continuously
- DevOps is a process which can be improved continuously

## Devops Lifecycle
- continuous Development
- continuous Integration
- continuous Testing
- continuous Delivery
- continuous Deployment
- Continuous Monitoring

## Software development Lifecycle
- Analysis, Planning, Architecture Design, Development, Testing, and Maintenance.

## Tools and their purpose

- Configuration Management: Ansible
- Continuous Integration: Jenkins (Code Pipeline)
- Source Code Management: GitHub (Code Commit)
- Continuous Deployment: Ansible (Code Deploy)
- Build: Maven, NPM
- Cloud Infrastructure: AWS, Azure, GCP or something else
- Monitoring: Prometheus, ELK, New Relic

## 3 Tier architecture

- Web Tier: Delivers the frontend content E.g. HTML, CSS, JavaScript
- Application Tier: Code related to that application E.g. Java, .Net, Python, Go
- Database Tier: MySQL, PostgreSQL etc
  - Only application can communicate with this component
- In addition to this, we have a load balancer which manages the traffic and routes it to the available free server
- Load Balancer also comes under Web Tier

## Forward Proxy vs Reverse Proxy

- Forward proxy
Sits in front of a client and prevents servers from communicating directly with the client. Forward proxies can regulate traffic, mask client IP addresses, and enforce security policies.
- Reverse proxy
Sits in front of a server and prevents clients from communicating directly with the server. Reverse proxies can act as a protective barrier for servers by accepting client requests, forwarding them to the appropriate server, and returning the results to the client. 

## Softlink vs Hardlink
- Hard links
A hard link is a mirror copy of the original file, and points directly to the data on a storage device. Hard links share the same inode number as the original file, and can't span different file systems. Deleting the original file doesn't affect a hard link. 
Soft links
- Also known as symbolic links, soft links point to the path to the data, or another filename that points to information on a storage device. Soft links have a unique inode number, and can link files across different file systems. Deleting the original file breaks a soft link because it points to the file name, not the content. 