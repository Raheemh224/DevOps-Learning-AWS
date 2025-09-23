# AWS Networking

## Understanding CIDR – IPv4

**Classless Inter-Domain Routing (CIDR)** – a method for allocating IP addresses.  

- Used in Security Group rules and AWS networking  
- Helps define an IP address range  

A CIDR consists of two components:

### Base IP
- Represents an IP contained in the range (XX.XX.XX.XX)

### Subnet Mask
- Defines how many bits can change in the IP (e.g., `/0`, `/24`, `/32`)  

#### Understanding Subnet Mask
- Allows part of the underlying IP to get additional values beyond the base IP  
- Number of IPs doubles as the subnet mask becomes less restricted  

---

## Private vs Public IP (IPv4)

**Private IP ranges:**
- 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) – large networks  
- 172.16.0.0 - 172.31.255.255 (172.16.0.0/12) – AWS default VPC  
- 192.168.0.0 - 192.168.255.255 (192.168.0.0/16) – home networks  

All other IP addresses are public.

---

## VPC – Subnet (IPv4)

- AWS reserves **5 IP addresses** (first 4 + last 1) in each subnet  
- These 5 cannot be assigned to EC2 instances  

---

## Internet Gateway (IGW)

- Allows resources (e.g., EC2 instances) in a VPC to connect to the internet  
- Scales horizontally, highly available and redundant  
- Must be created separately from a VPC  
- One VPC can only attach to one IGW  
- Route tables must be updated  

---

## Bastion Hosts

- Acts as a bridge to private instances  
- Located in a **public subnet** and connected to private subnets  
- Security group rules:  
  - Inbound from the internet on port 22 (restricted CIDR)  
  - EC2 instance SG must allow the Bastion SG or its private IP  

---

## NAT Gateway

- Allows instances in a **private subnet** to access the internet while blocking incoming traffic  
- No Security Groups to manage; AWS handles it  
- Must create NAT Gateway in **at least 2 AZs** for high availability  

---

## Network Access Control List (NACL)

- Functions like a **firewall** at the subnet level  
- One NACL per subnet  
- Rules have numbers; lower numbers have higher precedence  
- First rule match decides traffic action  
- Last rule is `*` (deny all unmatched traffic)  
- AWS recommends adding rules in increments of 100  
- Newly created NACLs **deny everything** by default  
- Useful for blocking specific IPs at the subnet level  

---

## Security Groups vs NACL

Security Groups (SG) vs NACL:

- SGs operate at the **instance level**, controlling traffic to/from instances  
- NACLs operate at the **subnet level**, controlling traffic at the boundary affecting all resources  

---

## VPC Peering

- Privately connect VPCs using AWS internal network  
- No overlapping CIDRs allowed  
- Update route tables in each VPC’s subnets to enable EC2 communication  

---

## VPC Endpoints in AWS

### What Is a VPC Endpoint?
- Enables private connectivity to supported AWS services **without using the public internet**  
- Powered by **AWS PrivateLink**  

### Why Use VPC Endpoints?
- **Security:** Keeps traffic private  
- **Reliability:** Highly available, horizontally scalable  
- **Cost & Simplicity:** Removes need for IGW/NAT Gateway, allows private access to services like S3, SNS  

### Traditional vs VPC Endpoint Access

Without VPC Endpoint:
- Access Method: Through NAT Gateway over internet  
- Security: Exposed to internet  
- Cost: NAT Gateway costs  

With VPC Endpoint:
- Access Method: Direct via AWS network  
- Security: Traffic remains internal  
- Cost: Avoid NAT Gateway costs  

### Common Troubleshooting Tips
- **Check DNS Resolution:** Enable DNS hostnames & resolution  
- **Check Route Tables:** Ensure subnets route to the VPC endpoint  

---

## AWS VPC Endpoints Types

### 1. Interface Endpoints
- Uses **AWS PrivateLink** and creates an **ENI** in your subnet  
- Suitable for services like SNS, SQS  
- Requires Security Group  
- Charged per hour + per GB  

### 2. Gateway Endpoints
- Creates a gateway (no ENI)  
- Supports **S3 and DynamoDB only**  
- No Security Groups needed  
- Free of charge  

Summary of Endpoints:

- **Interface Endpoint**
  - Services: Most AWS services  
  - Uses ENI: Yes  
  - Security Groups: Required  
  - Route Table Entry: Not required  
  - Cost: Per hour + per GB  
  - Best For: Broad private access  

- **Gateway Endpoint**
  - Services: S3 & DynamoDB  
  - Uses ENI: No  
  - Security Groups: Not required  
  - Route Table Entry: Required  
  - Cost: Free  
  - Best For: S3/DynamoDB access  

**Key Takeaway:**  
- Use **Interface Endpoints** for broad AWS service access  
- Use **Gateway Endpoints** for S3/DynamoDB to reduce cost and avoid internet exposure  

---

## Introduction to IPv6

### What is IPv6?
- Successor to IPv4 (4.3 billion addresses)  
- IPv6 supports ~3.4 × 10³⁸ addresses  

### Key Characteristics in AWS
- All IPv6 addresses are public and internet-routable  
- No concept of private IPv6 ranges  
- Resources with IPv6 are reachable via the internet unless secured  

### IPv6 Address Format
- Hexadecimal notation: `0–9`, `A–F`  
- Segments separated by `:`  
- Example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`  
- Shortening rules:  
  - `::` for consecutive zeros  
  - Remove leading zeros  

### Why Use IPv6?
- Scalability and future-proofing  
- Supports growth in IoT, mobile, cloud  
- Public by default in AWS  

---

## IPv6 in AWS VPC: Dual Stack Mode

- IPv4 **cannot be disabled**  
- IPv6 runs alongside IPv4 (dual-stack)  
- EC2 instances receive:  
  - Private IPv4  
  - Public IPv6  

### Benefits
- Flexibility for applications  
- Smooth migration to IPv6  
- Ensures future readiness  

---

## IPv6 Troubleshooting

- IPv6 cannot replace IPv4; IPv4 addresses are still required  
- Instance launch may fail due to **IPv4 exhaustion**, not IPv6  

**Solution:** Expand IPv4 CIDR block in the subnet  

---

## Egress-Only Internet Gateway
- IPv6 equivalent of NAT Gateway  
- Allows outbound IPv6 traffic but prevents inbound connections  
- Must update route tables  

---

## IPv6 Routing in Dual-Stack VPC

### Subnet Configuration

**Public Subnet:**
- Enabled for IPv4 + IPv6  
- EC2 configuration: Private IPv4, Public IPv4 Elastic IP, Public IPv6  
- Outbound routed via Internet Gateway  

**Private Subnet:**
- Dual-stack, no direct internet access  
- EC2 configuration: Private IPv4 + IPv6  
- IPv4 → NAT Gateway  
- IPv6 → Internet Gateway  

### Key Components

- **NAT Gateway:** Private IPv4 internet access  
- **Egress-Only IGW:** Private IPv6 outbound only  
- **Internet Gateway:** Public IPv4 & IPv6 access  

### Route Table Configuration
- Public Subnet: Routes IPv4/IPv6 internet traffic via Internet Gateway  
- Private Subnet: IPv4 → NAT Gateway, IPv6 → Egress-Only IGW


 

 

    

 

