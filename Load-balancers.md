### Load Balancers
- This is when an application/system can handle greater loads by adapting. There are 2 types: 

- Vertical Scalability – when you add more power to your system 
- Horizontal Scalability (=  elasticity) 

1. Vertical – increasing the size of the instance common for a database. There’s a limit to how much you can vertically scale.  
2. Horizontal – This is increasing the numbers of instances/systems to handle your load  

- High Availability – Running your application in multiple locations to ensure if one fails another takes over 

 

## Load Balancing is a way to distribute work across different servers.  Why use a load balancer? 

1. Seamlessly handle failures of downstream instances  
2. Do regular health checks to your instances 
3. Provide SSL termination for your websites HTTPS 
4. Separate public traffic from private 

## Why use an Elastic Load Balancer? 
- This is a managed load balancer by AWS. They guarantee it will be working and will take care of upgrades. 
 

## What are the different types of load balancers?  

1. Classic Load Balancer (v1 – old gen) - CLB  

- HTTP, HTTPS, TCP, SSL  

2. Application Load Balancer (v2 – new gen) - ALB 

- HTTP, HTTPS, Websocket 

3. Network Load Balancer (v2 – new gen) - NLB 

- TCP, TLS (secure TCP), UDP 
- Designed for no latency and high throughput  

4. Gateway Load Balancer – GWLB 

- Operates at layer 3 (network layer) - IP protocol. Designed to handle 3rd party applications such as firewalls  

 

## Load balancer Security Groups  

The security group controls who is allowed to send traffic to the load balancer and what type of traffic  

## Application Load Balancer 

- Layer 7 (HTTP) 
- Load balancing to multiple HTTP applications across machines (target groups) 
- Support for HTTP/2 and websocket  

What are the features? 

1. Route tables to different target groups 
2. Route based on path in URL  
3. Route based on hostname in URL  
4. Route based on Query String 
5. Has a port mapping to redirect to a dynamic port in ECS  
6. Need multiple Classic load balancer per application  

What are target groups? 

- This is a collection of targets like EC2 instances, containers, or Lambda functions that a load balancer can send traffic to  
- Health checks are at the target group level  
- ALBs can route to multiple target groups  
- IP Addresses – must be private IPs 

 

Good to knows: 

- ALB gives each load balancer a fixed host name in the format of XXX.region.elb.amazonaws.com  
- The application servers don’t see the IP of the client directly  

 

## Network Load Balancer (v2) 

- Network load balancers are layer 4 in the OSI model handling TCP and UDP traffic to your instances.  
- It handles millions of requests per second and there is a low latency. It is used for extreme performance which is not included in the AWS free tier  
- NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IP) 

NLB TCP based traffic: 

1. Listens for TCP connections on a specific port and forwards the traffic to a target group without inspecting the content 
2. Maintains original client IP address 

- Sticky sessions (Session Affinity) 

1. Stickiness is used so that the same client is always redirected to the same instance behind a load balancer 
2. Works for CLBs, ALBs and NLBs  
3. For CLB and ALB, the cookie used for stickiness has an expiration date you control. Stickiness is used so the user doesn’t lose his session data, however, stickiness may bring imbalance to the load over the backend EC2 instances  

 

## SSL/TLS basics 

An SSL certificate allows traffic between your clients and load balancer to be encrypted in transit (in-flight encryption) 

- SSL refers to Secure Sockets Layer used to encrypt connections  
- TLS refers to Transport Layer Security – a newer version. SSLs have an expiration date and must be renewed.  

Load balancer – SSL cert 

The load balancer uses an X.509 cert – SSL/TSL server cert 

You can manage certificates using ACM (AWS Certificate Manager) 

## SSL – Server Name Indication (SNI) 

- SNI solves the problem of loading multiple SSL certs onto one web server to serve multiple websites  
- Requires the client to indicate the hostname of the target server in the initial SSL handshake.  
- Server will then find the correct cert, or return the default one.  
- Only works for ALB & NLB 

## What is an Auto Scaling Group? 

- What are these? These are incredibly useful when dealing with websites and applications where traffic can change drastically at any time. In the cloud, scaling resources up or down is super easy and fast, and that's exactly what ASG helps with. 
- The primary goal of an ASG or scaling group is to adjust the number of running instances based on the load. If your traffic increases, the ASG will scale out, meaning that it will add more instances to keep up with the demand. When the load decreases, the ASG will scale in, removing the extra instances you no longer need. This way you're not paying for instances you don't need. 
- The Launch template ensures every instance that is loaded up has the same configurations every time.  

 

## Auto Scaling – CloudWatch alarms and scaling  

Cloudwatch alarms monitor a metric such as CPU, memory etc. Based on the alarm:

- We can create scale-out policies (increase the number of instances) 
- We can create scale-in policies (decrease the number of instances) 

 

## Scaling Policies 

- Dynamic Scaling : 3 types  

1. Target tracking scaling  
- Simple to set up  
- ‘I want the average ASG CPU to stay at around 40%’ 

2. Simple / Step Scaling  
- When a CloudWatch alarm is triggered ‘CPU > 70%’ then add 2 units  
- When a CloudWatch alarm is triggerred ‘CPU < 30%’ then remove 1  

3. Scheduled Scaling  
- Anticipate a scaling based on known usage patterns  
- ‘increase the min capacity to 10 at 5pm on fridays’ 