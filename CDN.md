### Amazon CloudFront 

CloudFront is a content delivery network (CDN):
1. Improves read performance, content is cashed at the edge 
2. Improves user experience  
3. DDos protection (because worldwide), integration with Shield, AWS Web Application Firewall 

## Amazon CloudFront: Understanding Origins 

What Is an Origin in CloudFront? 

- An origin is the original source of content that CloudFront fetches and delivers to end users via its global edge locations. 
- CloudFront supports two main types of origins: 
   -  Amazon S3 buckets (for static content) 
   - Custom Origins (for HTTP-based backends like ALBs or EC2) 

 

1. S3 Bucket as Origin 

- Ideal for serving static assets such as images, videos, JavaScript, and CSS files. 
- Requires setting up Origin Access Control (OAC) to secure content: 
  - OAC replaces the older Origin Access Identity (OAI). 
  - Ensures only CloudFront can access your S3 content, blocking direct access from the public web. 
- Bonus Tip: CloudFront can also be used for file uploads to S3, acting as an ingress. 

 

2. Custom Origin 

- Used when your content is hosted on HTTP-based services, including: 
 - Application Load Balancers (ALBs) 
 - EC2 instances 
 - On-premises servers 
 - Other websites or web apps 
- Suitable for dynamic content or custom applications. 
- If using an S3 bucket as a custom origin, ensure that Static Website Hosting is enabled on the bucket. 

 

## CloudFront with S3 as the Origin 

Overview 

-  Amazon CloudFront can use an S3 bucket as its origin for delivering content such as images, videos, or other static files. 
- This combination is designed to deliver content quickly, securely, and from the nearest location to the user. 

## How It Works 

1. S3 Stores the Content 
- An Amazon S3 bucket holds the original content (e.g., media files, documents). 
- This bucket acts as the origin for CloudFront. 

2. CloudFront Distributes via Edge Locations 
- CloudFront has a network of Points of Presence (POPs) or edge locations across the globe – e.g., Los Angeles, Mumbai, São Paulo, Melbourne. 
- These locations cache content closer to users to minimise latency. 

3. Content Delivery Process 
- When a user requests a file: 
  - CloudFront first checks the nearest edge location for a cached copy. 
  - If the file exists in the cache, it is delivered instantly from the edge. 
  - If the file is not cached, CloudFront fetches it from the S3 bucket and then caches it for future requests. 

 
## Security with Origin Access Control (OAC) 

- When using private S3 buckets, you can enforce secure access by: 
  - Enabling Origin Access Control (OAC). 
  - Applying bucket policies that restrict public access, allowing only CloudFront to retrieve the files. 
- This setup prevents users from bypassing CloudFront and accessing S3 files directly. 

 

## Key Benefits 
1. Improved performance: Files are cached closer to users for faster delivery. 
2. Reduced load on S3: Fewer direct requests to the bucket due to caching. 
3. Enhanced security: OAC ensures only CloudFront can access protected content. 
4. Scalable delivery: Efficiently handles high traffic across global locations. 

 

## CloudFront with ALB or EC2 as the Origin 

Overview 

- CloudFront can be used to distribute traffic to an Application Load Balancer (ALB) or directly to EC2 instances. 
- The goal is to improve performance and scalability, while ensuring secure communication between CloudFront and your backend services. 

## How It Works 

1. User Requests via Edge Locations 
  - Users connect to the nearest CloudFront edge location. 
  - Instead of accessing an S3 bucket, requests are routed to an ALB or EC2 instance. 

2. Origin Options 
  - ALB as Origin: 
    1. The ALB must be public, accepting traffic from CloudFront edge locations. 
    2. The ALB then forwards requests to private backend services, such as EC2 instances. 
  - EC2 as Origin: 
    1. If using EC2 directly, the instances must be publicly accessible. 
    2. These instances must allow traffic from CloudFront’s public IP ranges. 

## Security Group Configuration 

- It is essential to configure your security groups (SGs) correctly: 
- Only CloudFront's public IP addresses should be allowed as inbound rules. 
- AWS provides a public list of CloudFront IP ranges – this should be referenced and kept up to date. 
- For ALB setups, only allow CloudFront edge locations to communicate with the ALB. 
- For EC2 setups, only permit traffic from CloudFront to the EC2 instance directly. 

 
## Architectural Examples 

- Private EC2 Behind ALB (Recommended)
  - Public-facing ALB handles external requests. 
  - Private EC2 instances remain hidden behind the ALB. 
  - Enhances security and isolation of backend services. 
- Public EC2 Directly Behind   CloudFront: 
  - EC2 instances handle requests directly from CloudFront. 
  - Must be hardened and secured, allowing only CloudFront IPs. 
  - Less secure than using an ALB, but still effective for simple setups. 

## Key Benefits 
- Global performance: CloudFront speeds up access by using edge locations worldwide. 
- Security: By restricting access to CloudFront IPs, your application is protected from unsolicited traffic. 
- Scalability: CloudFront handles caching and request distribution, reducing load on backend services. 
- Flexibility: Suitable for both load-balanced and directly hosted web applications. 

 