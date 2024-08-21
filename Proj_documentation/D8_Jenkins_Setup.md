# Setting up the jenkins environment to run the pipelines

1. Adding Credentials in Jenkins
   - Click on Manage jenkins
   - Under security click on credentials
   - Click on system
   - Click on Global credentials (unrestricted)
   - Click on Add credentials
   - Kind: ``` SSH username with private key ```
   - Scope: ``` GLOBAL ```
   - ID: ``` Maven_slave ```
   - Description: ``` You can fill your own ```
   - Username: ``` ubuntu ```    ## Depends on the type of instance
   - Private Key:
       - Click on Enter directly
       - Click on Add
       - Open the .pem file in the IDE then you can see the private key
       - Copy paste in that block and click on create
   - Now we have successfully entered the credentials of the jenkins_slave system

   # Adding Node
   
   - Now click on Manage jenkins
   - Under System Configuration click on Nodes
   - Click on New node
   - Give the node name
   - Type: ``` Permanent Agent ```
   - Description: ```Give your own descrption ```
   - Number of Executors: ```2```
   - Remote root directory: ``` /home/ubuntu/jenkins ```
   - Usage: ``` Use this node as much as possible ```
   - Launch method:```Launch agent via ssh```
        - Host: ```Private I/P of the Jenkins_slave from aws console```
        - Credentials: ``` These are the credentials we have created above there
                            will be a drop-down where you can find it```
        - Host Key Verification Strategy:```Known hosts file Verification Strategy```
   - Availability:```Keep this agent online as much as possible```

2. Creating Test_Playbook
   - Go to your jenkins homepage
   - Top left click on All items
   - Give a name ```test_pipeline```` and select ```Freestyle project```
   - Description: ```Put your own content ```
   - select ``` Restrict where this project can be run```
   - Label expression: ```maven-agent``` ## given agent name
   - Build Steps: ```Execute shell```
         - ```echo "This is a test run" > /hom/ubuntu/testrunv1.txt```
   - Click on save and then you click on build now
   - Now go to the path ```/home/ubuntu/jenkins```
   - 

1. So we can write the pipeline in two types
   - Declarative pipeline
   - Scripted pipeline
