# Integration & Setup of SonarQube

1. We will be integrating SonarQube and Jenkins
2. We have to create a sonarqube account ```https://sonarcloud.io/```
3. I have directly created the account using Github
4. On the top right you see ```Account```
    - Go to security
    - Enter the token name : ```jenkins_token_sonar``` (copy the token name anywhere )
5. Login to your jenkins dashboard
    - Go to ```Manage Jenkins```
    - Click on ```Credentials```
    - Under ```Stores scoped to Jenkins``` 
    - click ```System```
    - click ```Global Credential``` and ```add credentials```
    - kind: ```Secret text```
    - Secret: ``` Add the token generated above in 4 ```
    - ID: ```SonarQube_key``` and click ``` Create```


   ## Configuring SonarQube in Jenkins
1. Go to jenkins dashboard
2. Click on ```Manage Jenkins```
3. click on ```Plugin Manager```
4. Search for ```SonarQube Scanner``` and install
5. Go to ```Manage Jenkins```
6. Under ```System Configuration``` click on ```System```
7. Scroll down you can find ```SonarQube Server```
8. Click on ```Add Sonarqube```
   - Name: ```SonarQube_Server_devopsudemy```
   - Server URL: ```https://sonarcloud.io/```
   - Server Aunth Token: ```SonarQube_key```
   - click ```apply``` and ```save```

## Configuring Sonarscanner in Jenkins
1. Go to jenkins dashboard
2. Click on ```Manage Jenkins```
3. Go to ```Global tool cofiguration
4. Under ```SonarQube Scanner``` click on ```Add SonarQube Scanner```
5. Name: ```SonarScanner_devopsudemy```
6. Make sure you [x] Install automatically option
7. 
