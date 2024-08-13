***Prepare Terraform Environment on Windows***

We should be setting up this 

1. Terraform
1. VS Code
1. AWSCLI

***Install Terraform***

- Download terraform the latest version from ([Download:)](https://developer.hashicorp.com/terraform/downloads)
- Setup environment variable 
  - Click on start à search "**edit the environment variables**" and click on it
  - Under the advanced tab, chose "**Environment variables**" 

    ` `à under the system variables select path variable and add terraform    location in the path variable.

  - System variables à select pathàadd new àTerraformhome\_Path
    in my system, this path location is C:\terraform
- Run the below command to check if terraform is installed or not
  - Terraform - - version

`  `3.1) if it’s installed properly it will show something like this

`         `Terraform v1. 9.2

`         `on windows\_amd64

` `3.2) If it’s not installed properly click on this ([Ref:](https://www.youtube.com/watch?v=Cn6xYf0QJME&ab_channel=AWSMadeEasy))

***Install Visual Studio code***

We use VS code to write the terraform script.

Download vs code latest version from [Download](https://code.visualstudio.com/download) and install it.

***AWS-Cli Installation***

We use the AWS-CLI in order to connect from the local system to AWS console using CLI (Command line interface). So,we can download the CLI in two from’s one is from the link give below and the other is by running the commands in the powershell which is not a good practice.I recommend to download it and install in the local.

- Download AWSCLI latest version from [Download](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and install it.
- ** You can run the command in powershell or the command prompt given in the link above.

- Run this command to check if its installed or not 
  - aws –version

    aws-cli/2.14.2 Python/3.11.6 Windows/10 exe/AMD64 prompt/off


