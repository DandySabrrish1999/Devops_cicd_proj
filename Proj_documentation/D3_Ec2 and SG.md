#**Creating a SG(Security group) attaching it with EC2**


1. The below code for creating a default SG group and attaching it to the EC2

-  Ingree: Which traffic to allow
-  egress: Which traffic to send
-  You can refer to link for more information[Click Here](https://registry.terraform.io/providers/hashicorp/aws/3.34.0/docs/resources/security_group)


Code:
```
    provider "aws" {
    region = "us-east-1"

}

resource "aws_instance" "dev_proj_ec2" {
  ami           = "ami-03972092c42e8c0ca"
  instance_type = "t2.micro"
  key_name      = "devops_proj_udemey"
  security_groups = ["dev_proj_sg"]

}

resource "aws_security_group" "dev_proj_sg" {
  name        = "dev_proj_sg"
  

  ingress {
    description = "SSH Access"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "dev_proj_sg"
  }
}
```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
2. 
•	VPC (Virtual Private Cloud): A Virtual Private Cloud (VPC) is a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you define. It’s like your own private network within AWS, giving you full control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.
•	Subnet: It’s the range of IP address available in your VPC.It can be either Public or Priv.
•	CIDR Block: Defines the range of IP addresses available in your VPC or subnet.
•	Internet Gateway (IGW): Provides a target in your VPC route tables for internet-routable traffic.
•	NAT Gateway: Allows instances in a private subnet to connect to the internet or other AWS services, but                        prevents the internet from initiating connections with those instances.
•	Security Groups: These act as virtual firewalls for your instances to control inbound and outbound traffic.   They are attached to instances.

Official Documentation:
•	VPC: [Click here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
•	Subnet: [Click here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet)
•	IGW: [Click here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/internet_gateway.html)
•	Route Table: [Click here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table)
•	
