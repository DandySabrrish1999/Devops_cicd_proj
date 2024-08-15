# Running the first terraform code creating EC2 instance


## Establishing the connection between local system and AWS using AWSCLI

1\. In order to run the terraform we need to establish the connection between local system and AWS console

`1.1) First you run the command

`Aws configure

AWS Access Key ID [\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*aaaa]:

AWS Secret Access Key [\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*aaaa]:

Default region name [us-east-1]:

Default output format [None]:

So in order to generate the above \*\*Access key\*\* and \*\*Secret key\*\*

-->Login to AWS console

-->Go to IAM

-->On the left hand side console you can see USERS click on that

-->Give some name 'terraform\_local' click on next

-->Click on attach policies in the below search bar type ADMIN you will find something like this [AdministratorAccess] and select that and click next

--> Now click on create user and now go to the IAM console

-->Click on the user you created and go to secrurity credential

-->Scroll down you find a option to create a access key click on CLI as we are using (AWS CLI)

-->Then there will be Access key id and Secret key generated make sure you save them the best practice is to click on download .csv so it stores the cred's in your local system

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Writing the terraform code to create an EC2 instance**

1\. Provider block is where we give the cloud platform name, name for the project we are working on 
```
provider "aws" {

`  `region = "us-east-1"

}
```
2\. Resource block is where we give the type of resource we are using and the necessary dependenies used to run the resource,before heading over this people who are not aware of what this code is all about,I suggest first to create the EC2 instance manually from the console as you get to know what the dependencies we are using to create then this terraform script seems easy to you.

So the resources required to create a EC2 instance

- Instance name = aws\_instance
- Name= dev\_proj
- Type of OS (AMI)= ami-03972xxxxx
- Instance type= t2.micro
- Key\_name= devops\_proj\_udemey
- Subnet\_id= subnet-08e16549a1b912fb1
```
resource "aws\_instance" "demo-server" {

`ami           = "ami-03972092xxxxxxx"

`instance\_type = "t2.micro"

`key\_name      = "devops\_proj\_udemey"

`subnet\_id     = "subnet-08e16549a1b912fb1"

}
```
**Encountered Error:**

This error encountered for when I had run the command(Terraform apply)If this error encounters its just your ec2 dosent have a VPC created.So it’s a new rule the aws has released that only specific regions dosent need to mention subnet-id in the terraform script as im working on (US-east-1) it needs to be specified.
```
Error: creating EC2 Instance: operation error EC2: RunInstances, https response error StatusCode: 400, RequestID: e2cb5f8e-d258-4a92-8e0a-3fb535861db6, 

api error VPCIdNotSpecified: No default VPC for this user. GroupName is only 

supported for EC2-Classic and default VPC.

│

│   with aws\_instance.demo-server,

│   on p1-ec2.tf line 6, in resource "aws\_instance" "demo-server":

│    6: resource "aws\_instance" "demo-server" {

│
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Executing the above terraform Code**

3\. There basic Terraform commands used to run any type of terraform script

- Terraform init: Used to initialise terraform which basically downloads the required packages and dependencies 
- Terraform fmt: Used to correct the foramtting of the script
- Terraform validate: used to validate the script
- Terraform plan: which is kind of a dry run
- Terraform apply: which means your finalizing the changes and reflect the same to your aws account

Want to explore more tags or more resources you want to mention check out this terraform official documentation ([click here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance))
