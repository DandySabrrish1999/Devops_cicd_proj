# Jfrog Artifactory setup

## 1. What is Jfrog artifactory?  
[Ref: https://jfrog.com/]-->products-->Jfrog artifactory

- JFrog Artifactory is like a warehouse for software components. Imagine you're building something out of LEGO blocks (in this case, software), and you need a place to store, organize, and retrieve those blocks whenever needed. Artifactory acts as a central location where developers store their software packages (like code libraries, dependencies, or even full applications), and it makes it easier to manage, track versions, and share them with other developers. It supports many different types of packages, like Docker images, Java, Python, etc.

- It has two types either you can install in VM directly or get a cloud based. I'm going with cloud based as it is more convieneint and its the practice widely used in corp sector



## 2. Creation of Artifactory account
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


## 3. Generate access token with username
![image](https://github.com/user-attachments/assets/7cec6bca-5c37-470d-a014-59ee9012cf26)
- Click on Administration
- User managememnt --> Access token --> Generate token
```
Description:jfrog_jenkins_devopsudemytoken
Username: Keep the emailid registered for Jfrog
```
- Click on generate token and paste it in the notepad


### Adding token in Jenkins
- Go to Jenkins dashboard
- Manage Jenkins
- Credentials
- Under stores scoped to Jenkins
- System --> global credentials
- Add Credentials
```
ID: Jfrog_Jenkins_Token
Username: Email-id used for jfrog registration
Password: Paste the token generated in Jfrog
Give required description's
```

### Installing artifactory plugin to Jenkins
- Dashboard Jenkins
- Manage Jenkins
- Plugins
- Available Plugin : ```Artifactory```
- Install the Plugin

## 4.Updating Jenkins file with JAR stage
- Go this directory in you Jenkins_Master system
```
cd /home/ubuntu/jenkins/workspace/ttrend_multibranch_pipeline_main/jarstaging/com/valaxy/demo-workshop
```
- Where you can find the present working JAR file and the version of it
- referring from docs [https://jfrog.com/help/r/jfrog-integrations-documentation/working-with-pipeline-jobs-in-jenkins]
- we go with scrippted pipeline
- ![image](https://github.com/user-attachments/assets/0ba38267-2474-40d1-9b19-70c1aab67284)
- Variables to change in the below Jenkins file
  - def registry
  - credentialsID
  - target
```
     def registry = 'https://devopsudemy.jfrog.io'  ## This def goes right on the starting of your code and replace it with your designated URL  

         stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" credentialsId:"Jfrog_Jenkins_Token"   ## [credentialsId]too have to change with the token id you sepcified in jenkins credentials created in the above steps
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "maven_projudemy-libs-release-local/{1}",    ## replace with your target name will attach a screenshot above where to find
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }
- After making the necessary changes we have to commit the file 

  
## 5. Creation of Docker file
- Creation and publishing of docker image on artifactory 
