## How to startup the machines
- When you destroy your machines and how to startup the machines
- Go to the terraform code in vpc
    - Uncomment the vpc and sgeks
```
// calling sg_eks module and eks module 
module "sgs" {
  source = "../sg_eks"
  vpc_id = aws_vpc.dev_proj_vpc.id
}

module "eks" {
  source     = "../eks"
  vpc_id     = aws_vpc.dev_proj_vpc.id
  subnet_ids = [aws_subnet.dev_proj_pubsubnet_01.id, aws_subnet.dev_proj_pubsubnet_02.id]
  sg_ids     = module.sgs.security_group_public
}
```
- Run the terraform commands
```
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply --auto-approve
```
- Copy your Jenkins_master public i/p
   - public i/p:8080
   - Update your public i/p in github-webhooks, if its failing just make sure delete the webhook and recreate the webhook again
- Update the sg for cluster
 
