## Kubernetes and Environment setup
In simple terms, Kubernetes is like a manager or an operating system for a bunch of containers. It helps you run, manage, and scale applications that are packaged inside containers, like Docker containers.

#### Imagine This:
You’re running a restaurant, and you have several chefs (containers) who each prepare different parts of a meal. Now, if you only have one or two chefs, it’s easy to manage, but what if you had 100 chefs working across multiple kitchens (servers)? It would get really hard to keep track of them all, ensure they’re doing their job, and make sure everything is running smoothly.

Kubernetes is like the head manager of all those kitchens:
- Organizes the chefs (containers): Kubernetes keeps track of all the containers running your application. It knows where they are and makes sure they’re doing the right job.
- Automatically handles problems: If one of your chefs (containers) stops working (fails), Kubernetes will bring in another one to replace it.
- Scales your team as needed: If more customers (users) show up at your restaurant, Kubernetes can bring in more chefs (containers) to handle the extra demand, and if things slow down, it can send some chefs home to save resources.
- Distributes the work: Kubernetes makes sure the work is spread out evenly across all your chefs (containers) and kitchens (servers), so no one kitchen gets overloaded.
- Restarts failed containers: If a container crashes or stops working, Kubernetes automatically restarts it.
  
#### Key Concepts in Kubernetes (simplified):
- Pods: A pod is the smallest unit in Kubernetes, and it usually contains one or more containers that work together. Think of a pod as a small kitchen where chefs (containers) work together.
- Nodes: Nodes are the servers (machines) where your containers run. Kubernetes manages these nodes to ensure everything works smoothly.
- Cluster: A cluster is a group of nodes managed by Kubernetes. It’s like having several kitchens all managed by the same head manager.
- Service: A service is like the front desk of your restaurant. It ensures customers (users) can always reach your application (containers) no matter where they are or which container is working at the time.
- Scaling: Kubernetes can automatically add or remove containers (chefs) based on how busy things get.

  
#### Why Kubernetes Is Useful:
Automation: It takes care of things like restarting crashed containers or scaling up when there’s more demand, so you don’t have to manage it all manually.
Consistency: It ensures your application runs the same way no matter where it’s deployed (on your own servers, in the cloud, etc.).
Scalability: If your app gets more users, Kubernetes can automatically scale your application to handle the load.

#### EKS
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Setting up EKS Cluster 

- First to make things easier create a folder
    - ```terraform```(seperate files inside)-->```eks,vpc,sg_eks,vpc```
- Code for the eks(elastic kubernets service) all these files will be in ```eks``` file
- Filename ```eks.tf```
```
resource "aws_iam_role" "master" {
  name = "ed-eks-master"

  assume_role_policy = <<POLICY
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "eks.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
POLICY
}

resource "aws_iam_role_policy_attachment" "AmazonEKSClusterPolicy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
  role       = aws_iam_role.master.name
}

resource "aws_iam_role_policy_attachment" "AmazonEKSServicePolicy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSServicePolicy"
  role       = aws_iam_role.master.name
}

resource "aws_iam_role_policy_attachment" "AmazonEKSVPCResourceController" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSVPCResourceController"
  role       = aws_iam_role.master.name
}

resource "aws_iam_role" "worker" {
  name = "ed-eks-worker"

  assume_role_policy = <<POLICY
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
POLICY
}

resource "aws_iam_policy" "autoscaler" {
  name   = "ed-eks-autoscaler-policy"
  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeAutoScalingInstances",
        "autoscaling:DescribeTags",
        "autoscaling:DescribeLaunchConfigurations",
        "autoscaling:SetDesiredCapacity",
        "autoscaling:TerminateInstanceInAutoScalingGroup",
        "ec2:DescribeLaunchTemplateVersions"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
EOF

}

resource "aws_iam_role_policy_attachment" "AmazonEKSWorkerNodePolicy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
  role       = aws_iam_role.worker.name
}

resource "aws_iam_role_policy_attachment" "AmazonEKS_CNI_Policy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
  role       = aws_iam_role.worker.name
}

resource "aws_iam_role_policy_attachment" "AmazonSSMManagedInstanceCore" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"
  role       = aws_iam_role.worker.name
}

resource "aws_iam_role_policy_attachment" "AmazonEC2ContainerRegistryReadOnly" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
  role       = aws_iam_role.worker.name
}

resource "aws_iam_role_policy_attachment" "x-ray" {
  policy_arn = "arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess"
  role       = aws_iam_role.worker.name
}
resource "aws_iam_role_policy_attachment" "s3" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
  role       = aws_iam_role.worker.name
}

resource "aws_iam_role_policy_attachment" "autoscaler" {
  policy_arn = aws_iam_policy.autoscaler.arn
  role       = aws_iam_role.worker.name
}

resource "aws_iam_instance_profile" "worker" {
  depends_on = [aws_iam_role.worker]
  name       = "ed-eks-worker-new-profile"
  role       = aws_iam_role.worker.name
}

###############################################################################################################
resource "aws_eks_cluster" "eks" {
  name = "devops_udemy_eks01"                    // Update according to your naming convention
  role_arn = aws_iam_role.master.arn

  vpc_config {
    subnet_ids = [var.subnet_ids[0],var.subnet_ids[1]]
  }
  
  depends_on = [
    aws_iam_role_policy_attachment.AmazonEKSClusterPolicy,
    aws_iam_role_policy_attachment.AmazonEKSServicePolicy,
    aws_iam_role_policy_attachment.AmazonEKSVPCResourceController,
    aws_iam_role_policy_attachment.AmazonEKSVPCResourceController,
    #aws_subnet.pub_sub1,
    #aws_subnet.pub_sub2,
  ]

}
#################################################################################################################

resource "aws_eks_node_group" "backend" {
  cluster_name    = aws_eks_cluster.eks.name
  node_group_name = "dev"                           // Can you modify accordingly
  node_role_arn   = aws_iam_role.worker.arn
  subnet_ids = [var.subnet_ids[0],var.subnet_ids[1]]
  capacity_type = "ON_DEMAND"
  disk_size = "20"
  instance_types = ["t2.small"]            
  remote_access {
    ec2_ssh_key = "devops_proj_udemey"          // Have to attach the key used initially to configure terraform with aws
    source_security_group_ids = [var.sg_ids]
  } 
  
  labels =  tomap({env = "dev"})

  // you can modify according to your need
  scaling_config {
    desired_size = 2
    max_size     = 3
    min_size     = 1
  }

  update_config {
    max_unavailable = 1
  }

  depends_on = [
    aws_iam_role_policy_attachment.AmazonEKSWorkerNodePolicy,
    aws_iam_role_policy_attachment.AmazonEKS_CNI_Policy,
    aws_iam_role_policy_attachment.AmazonEC2ContainerRegistryReadOnly,
    #aws_subnet.pub_sub1,
    #aws_subnet.pub_sub2,
  ]
}
```

