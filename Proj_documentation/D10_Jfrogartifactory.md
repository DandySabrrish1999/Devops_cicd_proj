# Jfrog Artifactory setup

## What is Jfrog artifactory?  
[Ref: https://jfrog.com/]-->products-->Jfrog artifactory

- JFrog Artifactory is like a warehouse for software components. Imagine you're building something out of LEGO blocks (in this case, software), and you need a place to store, organize, and retrieve those blocks whenever needed. Artifactory acts as a central location where developers store their software packages (like code libraries, dependencies, or even full applications), and it makes it easier to manage, track versions, and share them with other developers. It supports many different types of packages, like Docker images, Java, Python, etc.

- It has two types either you can install in VM directly or get a cloud based. I'm going with cloud based as it is more convieneint and its the practice widely used in corp sector



## Creation of Artifactory account
- Go to --> [Ref: https://jfrog.com/]
- Products-->Jfrog Artifcatory--> Start a trail [https://jfrog.com/start-free/]  ### The link should look like this if not its just giving the readonly version of Jfrog if your facing a issue use the above link 
- Give the basic details email and all those
- Under free trail--> Select cloud trail(14 day cloud trail)
```
hostname:devopsudemy
Host Platform: aws
Cloud region: US East(N.Virginia)    ## Location whereever is your VM's running
```
- There will be a window where it pop-ups for packages (or) in the welcome screen
![image](https://github.com/user-attachments/assets/e2b19521-4981-4982-a0ba-e157fe19ebdb)
- Click on Maven and the name as it used in the coming days just copy paste the link generated
- Click on Articatory--> Artifacts (To check whether the maven packages are installed or not)


## Generate access token with username
![image](https://github.com/user-attachments/assets/7cec6bca-5c37-470d-a014-59ee9012cf26)
- Click on Administration
- User managememnt --> Access token --> Generate token
```
Description:jfrog_jenkins_devopsudemytoken
Username: Keep the emailid registered for Jfrog
```
- Click on generate token and paste it in the notepad

### Adding token in jenkins
- Go to Jenkins dashboard
- Manage Jenkins
- Credentials
- Under stores scoped to Jenkins
- System --> global credentials
- Add Credentials
```
Username: Email-id used for jfrog registration
Password: Paste the token generated in Jfrog
Give required description's
```

## Add Setup the environemnt in jenkins
- Username and password
- Installing artifactory plugin
- Updating jenkins file with JAR stage
  
## Creation of Docker file
- Creation and publishing of docker image on artifactory 
