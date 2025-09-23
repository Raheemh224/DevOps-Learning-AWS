### Container-related services on AWS  

1. Amazon Elastic Container Service (Amazon ECS) 
- Amazons' own container platform that allows you to run docker containers without having to install orchestration services  

2. Amazon Elastic Kubernetes Service (Amazon EKS) 
- Amazons managed Kubernetes (open source) 

3. AWS Fargate  
- Amazons own Serverless container platform  
- Works with ECS and with EKS  

4. Amazon ECR  
- Stores container images  

## Amazon ECS – EC2 launch types  

- Amazon ECS/ ECS Cluster is a service that lets you run and manage containers on AWS. ECS is the manager that takes care of running your containers and making sure they're available and scaling them up or down as needed 
- EC2 launch type means you are responsible for the infrastructure.  
- Each instance must run the ECS agent to register in the ECS cluster. AWS takes care of stopping/starting containers  
- ECS Fargate – This is serverless – which means you don’t worry about managing any EC2 instances, AWS handles all of it. 

 

## IAM roles for ECS 
EC2 Instance Profile (EC2 Launch Type Only): 

1. Used by ECS agent  
2. Pull Docker images from ECR  
3. Send container logs to CloudWatch logs  

ECS Task Role: 

1. Allows each task to have a specific role  
2. Use different roles for the different ECS services you run  
3. Task role is defined in the task definition 

## Load Balancer Integrations for ECS 
- Application Load Balancer is supported and works for most use cases. Supports HTTPS so it’s ideal for web apps and databases. 
- Network Load Balancer is recommended only for high performance use cases, or to pair it with AWS Private Link 
- Classic Load Balancer is supported but not recommended – no advanced features – no fargate  

- ECS auto-scaling:automatically increase/decrease desired number of ECS tasks  

## EKS overview 

- Kubernetes is an open-source system that you can call a container orchestration. It automates the deployment, scaling, and management of container applications. If you used containers before, Kubernetes can help orchestrate and manage them at scale. 
= EKS is your solution if you want to leverage Kubernetes in AWS with all the management handled by AWS. 

## EKS – Node Types  

1. AWS Managed Node Groups  
- Creates and Manages nodes (EC2 Instances) for you 
- Nodes are part of an ASG managed by EKS  
- Supports On-Demand or Spot instances 

2. Self-Managed Nodes 
- Created by you and registered to the EKS cluster and managed by an ASG  
- Use prebuilt AMI – Amazon EKS optimized AMI 
- Supports On-Demand or spot instances 

3. AWS Fargate  
- No maintenance required; no nodes managed  

 