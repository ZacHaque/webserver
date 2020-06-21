# Demo Web Application Server

**Preface:**
This is an IaC solution in the response of provided DevOpsTest.md using AWS cloud-formation.

**What:**
Running the VPC stack will create the underlying VPC structure including NAT Gateway, Internet Gateway, Route Tables, Subnets, NACL and EIPs spread across 3 different availability zones. It provides a platform for deploying a highly available, fault tolerant web application.

The second part of the deployment(webserver), deploys a Apache web application service utilizing various AWS native features like Classic Load Balancer, Autoscaling Group, Launch Configuration, Cloud watch alarm, Cloudwatch log group, EC2, Instance Profile and AWS SSM.

**Deployment:**
It consists of two different stacks. VPC deployment and Web Server deployment

**Part1 - Deploy the VPC:**
We can deploy the VPC stack by going to the root of the repository and run following from AWS CLI

```
aws cloudformation create-stack --stack-name demo-vpc --template-body file://demo-vpc.yaml
```
Once deployed above, it will create the VPC and all other required AWS resources. Moreover it exports Names of those resources to be used on the next stack. So the following stack depends on the VPC stack.

**Part2 - Deploy Web Application:**
Now we will deploy the webserver stack in the similar way. The command is given as per below.

*Note: The parameters file need to be updated as per your requirement.*
```
aws cloudformation create-stack --stack-name demo-webserver --template-body file://demo-webserver.yaml --parameters file://params-webserver.json --capabilities CAPABILITY_IAM
```
Running above will deploy all the resources as discussed above. It will then start the apache webserver, listening on port 80. 


**Outcome:**
The output of the webserver stack provide the ServiceURL to browse our applicaion.

**Addressing assesment requirement:**

Part-1:
- When designing above solution I have Applied AWS best practices architecture and security. I have deployed the web server in private tier of the VPC where it is not exposed to the public internet. A Load balancer has been deployed in the public tier to accept web request from the internet then relay to the web server. Public tiers have routes to the internet through IGW but Private tiers don't. Private tier only has an outbound route through NAT Gateway, So EC2 instances can reach the internet for updates, patches etc. Inbound connection not allowed. We have configured Security group allowing web traffic only.
- As suggested Amazon Linux 2 AMI has been used
- A basic "Hello, World" webpage served on the url created as the output on the webserver stack. The url is basically DNS name of the load balancer, which can be uplifted with an proper Domain name using Route53 alias name.
- We have created an auto scaling group as part of the deployment. It deploys additional EC2 instances and add it to the load balancer when EC2 CPU usage crosses 60%. 
- It is capable of removing unhealthy instances from the load balancer and terminate, using a health check.
- The VPC have 2 tier and spread across three Availability Zones

Part-2:
- I have avoided using ssh port and I also did not not opened that port in the SG, as an extra layer of security. For the management of the server we can use SSM access from the AWS System manager. Instance profile I have created for the EC2 have the required managed policy attached and Amazon linux 2 already comes with SSM agent pre-installed.
- I have installed and made use of aws log agent on EC2 instance.  The log agent pushes Apache "access" and "error" logs to CloudWatch. It creates two different log groups called webserver-access_log and webserver-error_log

**Assumtions:**
- AWS CLI is inastalled and AWS credential have sufficiant permission to deploy above resources.
- SSM logging to s3 is not enforced on the AWS accounts. if yes, S3 access policy to be attched to instance profile.
- The AWS account is not controlled by SCP to restrict the use of above resources

**Conclution:**
- Further upliftment like serving on HTTPS can be implemented either from the Load Balancer or from the instance itself.
- SNS topic can be deployed and configured to alert engineer when an autoscaling event (LAUNCH/LAUNCH_ERROR/TERMINATE/TERMINATE_ERROR) occurs.

**References:**
- VPC template is a modified version of AWS boiler plate template can be found on the link below -
https://github.com/awslabs/aws-cloudformation-templates/blob/master/aws/services/VPC/VPC_With_Managed_NAT_And_Private_Subnet.yaml

- Webserver template is a havily modified version of atlassian bitbucket data center template can be found on below link
https://github.com/aws-quickstart/quickstart-atlassian-bitbucket

- Multi tier conecpt
https://aws.amazon.com/blogs/aws/building-three-tier-architectures-with-security-groups/
https://medium.com/the-andela-way/designing-a-three-tier-architecture-in-aws-e5c24671f124
