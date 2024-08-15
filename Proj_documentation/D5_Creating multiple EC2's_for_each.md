# Creating multiple Machines and Updating the rules 

```
// Decalring the Cloud provider

provider "aws" {
  region = "us-east-1"

}

// Creating EC2

resource "aws_instance" "dev_proj_ec2" {
  ami                    = "ami-0a0e5d9c7acc336f1"
  instance_type          = "t2.small"
  key_name               = "devops_proj_udemey"
  vpc_security_group_ids = [aws_security_group.dev_proj_sg.id]
  subnet_id              = aws_subnet.dev_proj_pubsubnet_01.id

  //We use for_each to create 3 systems mentioned below,by using for_each 
  // you can create how many systems you need/ requirement
  for_each = toset(["Jenkins_master", "Jenkins_slave", "Ansible"])
  tags = {
    Name = "${each.key}"
  }

}


// Creating Security Group

resource "aws_security_group" "dev_proj_sg" {
  name   = "dev_proj_sg"
  vpc_id = aws_vpc.dev_proj_vpc.id

  ingress {
    description = "SSH Access"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }


  // Jenkins default port 
  ingress {
    description = "Jenkins GUI Access"
    from_port   = 8080
    to_port     = 8080
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

// Creating VPC

resource "aws_vpc" "dev_proj_vpc" {
  cidr_block       = "10.0.0.0/16"
  instance_tenancy = "default"

  tags = {
    Name = "dev_proj_vpc"
  }
}

// Creating Public subnet

resource "aws_subnet" "dev_proj_pubsubnet_01" {
  vpc_id                  = aws_vpc.dev_proj_vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = "true"
  availability_zone       = "us-east-1a"

  tags = {
    Name = "dev_proj_pubsubnet_01"
  }
}

// Creating IGW

resource "aws_internet_gateway" "dev_proj_igw" {
  vpc_id = aws_vpc.dev_proj_vpc.id

  tags = {
    Name = "dev_proj_igw"
  }
}

// Creating Route table

resource "aws_route_table" "dev_proj_rt" {
  vpc_id = aws_vpc.dev_proj_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.dev_proj_igw.id
  }

  tags = {
    Name = "dev_proj_rt"
  }
}

// Route Table assosciation
resource "aws_route_table_association" "dev_proj_rta_pubsubnet_1" {
  subnet_id      = aws_subnet.dev_proj_pubsubnet_01.id
  route_table_id = aws_route_table.dev_proj_rt.id
}
```
