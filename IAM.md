### IAM (Identity and Access Management) 

## Users & Groups 
- Individual people who need specific access 

## Permissions 
- Users or groups can be assigned JSON documents called policies – these define the permissions of a user. Always apply the least privilege principle: don’t give a user more permissions than it needs. 

## Policy Structure  

  
## How to access AWS? 

- AWS Management Console – Protected by password – MFA 
- AWS Command Line Interface (CLI) protected by access keys 
- AWS Software Developer KIT (SDK) - for code: protected by access keys 
- Access Key ID – Username. 
- Secret Access Key – Password  

## AWS SDK 
- Set of libraries which enables you to access and manages AWS services programmatically 

## IAM Roles for services 
- IAM role lets services get temporary access to services without a permanent access key. E.g EC2 instance roles  

## IAM Security tools 
- IAM Credentials report (account-level) - a report that lists all your account's users and status of their various credentials 
- IAM Access Advisor (user-level) - Shows the service permissions granted to a user and when it was last accessed. Can use this to revise policies. 

 
