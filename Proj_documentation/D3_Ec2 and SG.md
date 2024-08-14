provider "aws" {

`  `region = "us-east-1"

}

resource "aws\_instance" "dev\_proj\_ec2" {

`  `ami           = "ami-03972092c42e8c0ca"

`  `instance\_type = "t2.micro"

`  `key\_name      = "devops\_proj\_udemey"

`  `security\_groups = ["dev\_proj\_sg"]

}

resource "aws\_security\_group" "dev\_proj\_sg" {

`  `name        = "dev\_proj\_sg"



`  `ingress {

`    `description = "SSH Access"

`    `from\_port   = 22

`    `to\_port     = 22

`    `protocol    = "tcp"

`    `cidr\_blocks = ["0.0.0.0/0"]

`  `}

`  `egress {

`    `from\_port   = 0

`    `to\_port     = 0

`    `protocol    = "-1"

`    `cidr\_blocks = ["0.0.0.0/0"]

`  `}

`  `tags = {

`    `Name = "dev\_proj\_sg"

`  `}

}

