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
  - Now go to your aws cluster console so change the security group
   - Go to your  cluster
   - click on security
   - click on the remoteAcess security group mentioned out of two
   - click on Edit inbound rule
   - click om add rule and follow the below screenshot for inboud rules
   - ![image](https://github.com/user-attachments/assets/eefdcd37-9e9d-4ae8-ab04-665b4ec30fc3)
   - add the rule and and now copy the cluster public i/p
   ```
   cluster public i/p:30082
   ```
 - Login to your Jenkins_slave (or) build_node
```
sudo su - ubuntu
aws eks update-kubeconfig --region us-east-1 --name devops_udemy_eks01       ## Refer D12.2(3)
kubectl get ns                                ## Gets all the namespace
chmod +x deploy.sh                            ## Permissions to execute
./deploy.sh                                   ## Execute
kubectl get ns                                ## Get all the namespace 
kubectl get all -n dev-udemy-namespace        ## Getting all the clusters in the namespace
```
- Copy your cluster public i/p:30082
- Make sure whenever you change the code commit and then push and refresh in order for the changes to affect
- 
