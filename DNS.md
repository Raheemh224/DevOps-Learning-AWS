### DNS Route 53 
- A highly available, scalable, fully managed and authoritative DNS 
- Authoritative – the customer (you) can update the DNS records  
- Route 53 is also a Domain Registrar  
- Ability to check the health of your resources 
- The only AWS service which provides 100% availability SLA 
- Why Route 53? 53 is a reference to the traditional DNS port  

## Hosted Zones  

- A container for records that define how to route traffic to a domain and it’s subdomains 
- Public Hosted Zones – contains records that specify how to route traffic on the internet (public domain names) application1.mypublicdomain.com 
- Private Hosted Zones – contains records that specify how you route traffic within one or more VPCs (Private domain names) application1.company.internal 

Each record contains: 
- domain/subdomain name 
- Record type -  AAAA or a  
- Value – 12.34,56,78 
- Routing Policy – how Route53 responds to queries 
- TTL – amount of time the record cached at DNS resolvers 

Route 53 supports many records: 
- A / AAAA / CNAME / NS / CAA / DS / MX / NAPTR / PTR / SOA / TXT / 

## Route 53 - Record Types 

1. A – maps a hostname to IPv4  
2. AAAA – maps a hostname to IPv6 
3. CNAME – maps a hostname to another hostname 
 - The target is a domain name which must have an A or AAAA record 
 - Can’t create a CNAME record for the top node of a DNS namespace (Zone Apex) 

4. NS – Name Servers for the Hosted Zone 
 - Control how traffic is routed for a domain 

## Route 53: TTL (Time To Live) and DNS Record Caching 

What Is TTL? 

- TTL (Time To Live) defines how long a DNS record is cached by clients or DNS resolvers before they query Route 53 again for updated information. 
- It directly impacts the freshness of DNS data and the amount of traffic sent to Route 53. 

 
High TTL (e.g. 24 hours) 

- Pros: 
 1. Reduces the number of DNS queries to Route 53. 
 2. Helps minimise costs due to lower query volumes. 

- Cons: 
1. Changes to DNS records (e.g. IP address updates) are not reflected immediately. 
2. Users may continue to see outdated records until the TTL expires. 

Low TTL (e.g. 6 seconds) 

- Pros: 
1. DNS changes propagate quickly. 
2. Ideal for environments where frequent changes occur (e.g. server migrations, load balancing). 

- Cons: 
1. Increases the number of queries to Route 53. 
2. Can result in higher operational costs due to increased traffic. 

## ALIAS Records Exception 

- TTL is mandatory for all DNS record types except ALIAS records. 
- ALIAS records are automatically managed by AWS, so TTL values do not need to be set manually. 

## Best Practice: Finding the Right Balance 

- Selecting a TTL value is a trade-off: 
 - A high TTL is more cost-effective but risks using stale DNS data. 
 - A low TTL provides real-time accuracy but comes with higher query costs. 
- Choose TTL values based on your application's needs for cost control vs DNS update speed. 

 

## CNAME vs Alias  

- CNAME – Points a hostname to another hostname 
- Alias – Points a hostname to an AWS record. Free of charge  

 

## Alias Records 

- Maps a hostname to an AWS resource  
- An extension to DNS functionality 
- Automatically recognises changes in the resource’s IP addresses 
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex)e.g example.com 
- Alias Record is always of type A/AAAA for AWS resources (IPv4/IPv6) 
- You can’t set the TTL 

## Alias Record targets: 
- ELBs 
- CloudFront Distributions 
- API gateway 
- Elastic Beanstalk environments 
- S3 Websites 
- VPC interface endpoints 
- Global Accelerator  
- Route 53 record in the same hosted zone
- You cannot set an ALIAS record for an EC2 DNS name 

## Route 53: Understanding Routing Policies 

- What Does “Routing” Mean in Route 53? 
 
 1. Routing in Route 53 is not load balancing or network-level routing. 
 2. Route 53 doesn’t route traffic directly — it responds to DNS queries with the appropriate IP address. 
 3. Think of it as a DNS-based traffic director — like a host guiding guests to the right table. 

 

## Types of Routing Policies in Route 53 
- Route 53 offers multiple routing policies to manage how DNS queries are answered, depending on performance, location, or availability needs. 

 

1. Simple Routing 
- Returns the same IP address every time a domain is queried. 
- Best for single-resource environments without special routing logic. 

 

2. Weighted Routing 
- Distributes traffic based on defined weights. 
- Example: 70% to Server A and 30% to Server B. 
- Ideal for A/B testing, gradual rollouts, or balancing load across multiple resources. 

 

3. Failover Routing 
- Designed for high availability. 
- Sends traffic to a primary resource, but automatically redirects to a backup if the primary becomes unhealthy. 
- Requires health checks to monitor the primary target. 

4. Latency-Based Routing 
- Routes users to the server with the lowest latency (fastest response time). 
- Ideal when you have global servers and want to improve user experience. 


5. Geolocation Routing 
- Routes traffic based on the location of the user (e.g. Europe, US). 
- Useful for regional content delivery or compliance-based access. 

6. Multi-Value Answer Routing 
- Returns multiple IP addresses in response to a single DNS query. 
- Adds a basic form of load distribution without a dedicated load balancer. 
- Can be used alongside health checks. 

7. Geoproximity Routing 
- Similar to geolocation, but offers greater control and customisation. 
- Routes traffic based on the location of resources and users. 
- Can be fine-tuned using Route 53 Traffic Flow to expand or shrink the influence of certain endpoints. 

8. Simple Routing  
- Typically, route traffic to a single resource 
- Can specify multiple values in the same record 
- If multiple values are returned, a random one is chosen by the client 
- When Alias is enabled, specify only one AWS resource 
- Can’t be associated with Health Checks 

## Route 53: Weighted Routing Explained 

- Weighted routing allows you to control how traffic is distributed between multiple resources. 
 - Ideal for gradual rollouts, A/B testing, and regional balancing without relying on traditional load balancers. 

# How It Works 

- You assign a weight to each DNS record associated with your domain. 
- The proportion of traffic each resource receives is based on the weight assigned relative to the total weight. 

## Advanced Features 
 
- Health checks can be associated with each DNS record. 
- If a resource becomes unhealthy, Route 53 automatically removes it from rotation. 
- Setting a weight to 0 effectively disables traffic to that resource without deleting the record. 
- This gives you fine-grained control over traffic flow. 

## Common Use Cases 

- Canary deployments: Gradually expose a new version of an application to a small percentage of users. 
- Cross-region load distribution: Control how much traffic each region or availability zone receives. 
- Disaster recovery: Keep traffic away from unstable regions by adjusting weights dynamically. 

 

## Latency Based Routing  
- Redirect to the resource that has the least latency close to us 
- Helpful when latency for users is a priority 
- Latency is based on traffic between users and AWS regions 
- Can be associated with health checks  

## Route 53 – health checks 

Purpose of Health Checks 

- Route 53 health checks allow you to monitor the availability and health of your resources. 
- If a resource becomes unhealthy, Route 53 can reroute traffic to a healthier endpoint. 
- Think of it as Route 53 regularly asking: "Are you still working properly?" 

## Types of Health Checks 

1. Endpoint Health Checks 

The most basic and direct type. 

Route 53 pings a specific endpoint (e.g. an EC2 instance, load balancer, or web application) to verify it is responsive. 

If no valid response is received, the endpoint is marked as unhealthy, and traffic is redirected to an alternative, healthy resource. 

 

2. Calculated Health Checks 

These checks aggregate multiple other health checks to provide a combined status. 

Useful for understanding the overall health of a system made up of multiple components. 

Example: You have health checks on three servers; a calculated check gives you the health status of the entire service. 

Acts like a dashboard view of multiple checks. 

 

3. CloudWatch Metric-Based Health Checks 

Advanced health checks that rely on CloudWatch metrics and alarms instead of simple availability. 

Can respond to performance issues like: 

- High CPU usage 
- Low memory availability 
- Database throttling 
- Route 53 adjusts routing based on
- CloudWatch alarm states. 

 

## Failover and Rerouting 

- Health checks are often used in failover routing policies. 
- If a primary region or server becomes unavailable: 
 - Route 53 automatically redirects traffic to a healthy backup in another region. 
 - Users experience minimal or no disruption. 

 

- Key Benefits 
1. Automatic traffic rerouting ensures high availability. 
2. Supports multiple health scenarios: from simple uptime to deep application performance monitoring. 
3. Seamless integration with Amazon CloudWatch provides enhanced observability and control. 

## Geolocation Routing 
- Different from legacy-based  
- Routing is based on user location  
- Specify location by continent or Country  
- Should create a “default” record  
- Use cases: website localization, restrict health distribution, load balancing 
- Can be associated with Health Checks 

## Geoproximity Routing 

- Route traffic to your resources based on the geographic location of users and resources  
- Ability to shift more traffic to resources based on the defined bias 
- To change the size of the geographic region, specify bias values: 

   - To expand (1-99) - more traffic to the resource 

   - To shrink (-1 to –99) - less traffic to the resource 

Resources can be:  
- AWS resources (specify AWS region) 
- Non-AWS resources (specify Latitude and Longitude) 
- You must use Route 53 traffic flow to use this feature 

## IP routing  
- This used to optimize performance and reduce network costs 
- Routing is based on client’s IP addresses 
- Provide a list of CIDRs for your clients and the corresponding endpoints/locations 

## Multi-value routing  

- Use when routing traffic to multiple resources  
- Route 53 return multiple values/resources 
- Can be associated with health checks  
- Up to 8 healthy records are returned for each multi-value query  
- Multi-value is not a substitute for having an ELB 

 

 

 