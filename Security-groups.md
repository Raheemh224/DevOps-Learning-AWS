### Security Groups 
- Security groups determine what leaves and enter your instance like virtual firewalls. They only contain allow rules. They regulate: 

- Access to ports  
- Authorised IP ranges – IPv4 and IPv6  
- Control of inbound network  
- Control of outbound network 

They can be attached to multiple instances: 

- Locked down to a region  
- All inbound traffic is blocked by default  
- All outbound traffic is authorised by default  

## Ports to know 

1. 22 = SSH (Secure Shell) - log into a Linux instance  
2. 21 = FTP (File Transfer Protocol) - upload files into a file share 
3. 22 = SFTP (Secure File Transfer Protocol) - upload files using SSH 
4. 80 = HTTP (Secure File Transfer Protocol) - Access unsecured websites 
5. 443 = HTTPS – Access secured websites  
6. 53 = DNS – for DNS queries and resolving 
7. 3389 = RDP (Remote Desktop Protocol) - log into a Windows instance  

## Elastic IPs 
- An elastic IP is a public IP address which you own, if you reserve it then you get charged by AWS unless you attach it to instances. Keeping the same IP will speed up start up times 

## When do you use?  
- You can only have 5 elastic IPs. Try avoid because they reflect poor architectural decisions. Use a load balancer and don’t use a public IP.  
