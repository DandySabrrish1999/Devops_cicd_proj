# Jfrog Artifactory setup

1. What is Jfrog artifactory?

-JFrog Artifactory is like a warehouse for software components. Imagine you're building something out of LEGO blocks (in this case, software), and you need a place to store, organize, and retrieve those blocks whenever needed. Artifactory acts as a central location where developers store their software packages (like code libraries, dependencies, or even full applications), and it makes it easier to manage, track versions, and share them with other developers. It supports many different types of packages, like Docker images, Java, Python, etc.

- It has two types either you can install in VM directly or get a cloud based. I'm going with cloud based as it is more convieneint and its the practice widely used in corp sector

2. Creation of Artifactory account
3. Generate access token with username
4. Add Setup the environemnt in jenkins
- Username and password
- Installing artifactory plugin
- Updating jenkins file with JAR stage
  
5.Creation of Docker file
- Creation and publishing of docker image on artifactory 
