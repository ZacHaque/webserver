#  AWS DevOps Test

##  Part One

###  Instructions

Using an orchestration tool of your choice (TerraForm, CloudFormation etc), create a template that defines an application stack in the AWS ap-southeast-2 region.  This test does not require you to show us a running application stack; we will deploy your template(s).  The stack should meet the following criteria:

- Apply AWS best practices in solution architecture and security.
- Use an official, public, Debian, Ubuntu or Amazon Linux 2 AMI for any EC2 resources.
- Serve a basic "Hello, World" webpage.
- Scale capacity to meed demand.
- Manage unhealthy instances.

You may chose to include all components from the VPC up, in the stack.  However, given the amount of effort involved, you can also exclude the VPC, subnets, and associated networking components from your stack and configure them as input variables to be provided by ourselves upon deployment.  You can assume the following about the environment into which your stack will be deployed:

- A VPC with public and private subnets in up to 3 x Availability Zones.
- Internet Gateway and Nat Gateway present to allow outbound internet connectivity to TCP/80 and TCP/443 from all subnets in the VPC.


### Notes:


- AWS publish a variety of boilerplate templates.  Its ok to use them as a starting point, provided you modify them enough to demonstrate your own skills.
- Your application stack may or may not be containerised.
- Use Git version control and commit your solution to a Git repository that we can access. (Github, Bitbucket or Gitlab are the obvious choices)
- Please include a README, citing any third-party code, tutorials or documentation you have used.
- If your solution includes any unusual deployment steps, please note them in your README file
- If you have chosen to exclude the networking components from your solution and provide them as input variables, please note the requirements in the README.
- Serving your "Hello, World" page over HTTPS is an added bonus, but not necessary for the purposes of this exercise.


## Part Two


Please describe how you would implement the following requirements:
- How would you provide shell access into the application stack for operations staff who may want to log into an instance.
- Make access and error logs available in CloudWatch Logs
