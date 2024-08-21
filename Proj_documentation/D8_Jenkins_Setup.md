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


1. So we can write the pipeline in two types
   - Declarative pipeline
   - Scripted pipeline
