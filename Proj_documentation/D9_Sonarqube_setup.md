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
    - 
