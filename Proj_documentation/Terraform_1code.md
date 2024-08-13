**V0.2**
** _Running the first terraform code creating EC2 instance_** 

1. In order to run the terraform we need to establish the connection between local system and AWS console
   1.1)First you run the command
        Aws configure
AWS Access Key ID [*****************aaaa]:
AWS Secret Access Key [****************aaaa]:
Default region name [us-east-1]:
Default output format [None]:
So in order to generate the above **Access key** and **Secret key**
-->Login to AWS console
-->Go to IAM
-->On the left hand side console you can see USERS click on that
-->Give some name 'terraform_local' click on next
-->Click on attach policies in the below search bar type ADMIN you will find something like this [AdministratorAccess] and select that and click next
--> Now click on create user and now go to the IAM console
-->Click on the user you created and go to secrurity credential
-->Scroll down you find a option to create a access key click on CLI as we are using (AWS CLI)
-->Then there will be Access key id and Secret key generated make sure you save them the best practice is to click on download .csv so it stores the cred's in your local system
   
