## Setting up docker in salve and Creation of Docker file 
![image](https://github.com/user-attachments/assets/9ed3ec86-02f7-464a-a192-725fd85f7cbf)

## 1.What is Docker?
Docker is a tool that allows you to package and run applications in a lightweight, isolated environment called a container. Think of a container like a box that holds everything an application needs to run, including the code, libraries, and system tools. This makes it easier to run the application anywhere—on your computer, on a server, or in the cloud—without worrying about compatibility or setup issues.

Here’s a simple Example:
Imagine you want to move your furniture from one house to another. If you just try to move everything one piece at a time, things might get lost or damaged, and it might not fit properly in the new house. But if you pack everything neatly into boxes, you can move it easily, and everything stays organized.

Similarly, Docker packages an application and its dependencies into a container, so it runs smoothly no matter where it's deployed, without worrying about the underlying system.

## 2.Installing Docker in Jenkins-Slave machine using ansible machine

- Get to the Ansible system where we created a playbook to install maven ```ansible_jenkins_slave_setup.yaml```
- I have created a new setup file ```ansible_jenkins_slave_setup2.yaml```.So it's the same script we add a task to install docker and start the service
- You can attach the below code to your existing file or create a new file copying the existing code and its case sensitive else it wont work.
- After executing the below playbook execute this ```ls -ltr /var/run/docker.sock``` to make sure the required permissions are given.
  
```
# Installing Docker in the Slave 2 system
  - name: Install docker
    apt:
      name: docker.io
      state: present

# Starting the Docker service
  - name: Starting the service
    service:
      name: docker
      state: started

# Giving Permission
  - name: give 777 permission on /var/run.docker.sock
    file:
      path: /var/run/docker.sock
      state: file
      mode: 0777

# Auto starting Docker
  - name: Restarting the docker service
    service:
      name: docker
```
- Run the above playbook
```
ansible-playbook -i hosts ansible_jenkins_slave_setup2.yaml --check  ## To dry run the playbook
ansible-playbook -i hosts ansible_jenkins_slave_setup2.yaml          ## To run the playbook
```
- Go to your slave system and check whether docker is installed or not
```
service docker status             ## Check the status of docker
```

### Docker File
- We crete a ```dockerfile``` in the ttrend proj environment where we have created our sonar file and jenkins file
  
```
FROM openjdk:8
ADD jarstaging/com/valaxy/demo-workshop/2.1.2/demo-workshop-2.1.2.jar ttrend.jar
ENTRYPOINT [ "java", "-jar","ttrend.jar" ]


## Description of the above snippet of the code
1. FROM openjdk:8
This line tells Docker to start with a pre-made base image that includes OpenJDK 8. OpenJDK is the free and open-source implementation of Java, so this means you’re starting with a basic environment that already has Java 8 installed. You need Java to run your application.
Think of it like you're starting with a computer that already has Java set up and ready to use.

2. ADD jarstaging/com/valaxy/demo-workshop/2.1.2/demo-workshop-2.1.2.jar ttrend.jar
The ADD command is like copying a file into your Docker image. Here, you’re taking a file named demo-workshop-2.1.2.jar from a directory inside your project (jarstaging/com/valaxy/demo-workshop/2.1.2/) and adding it to the container. Once added, it will be renamed as ttrend.jar inside the container.
In simple terms, you're copying your Java application (which is packaged as a .jar file) into the container and renaming it to make it easier to reference later.

3. ENTRYPOINT [ "java", "-jar", "ttrend.jar" ]
This line specifies the command that should be run when the container starts. In this case, it tells Docker to run the command:
java: This is the command to run Java programs.
-jar: This tells Java to run a specific .jar file (which is like a package containing your application).
ttrend.jar: This is the .jar file (your Java application) that you copied into the container earlier.
So, when the container starts, it automatically runs your Java application by executing java -jar ttrend.jar.

Overall Summary:
You start with a base environment that has Java 8 installed.
You copy your application’s .jar file (which is your Java app) into the container and rename it.
When the container starts, it runs your Java application using the java -jar command.
```
- Now we push the docker file using gitbash from local and check the branch if its successfully executed or not

### Docker repository in Jfrog Artifactory
- On your JFrog dashboard on the top right under your profile there is a option called ```Quick Repository creation```
- click on that select ```docker``` and give the name you like
- My repo name : ```testdocker```

(OR)
- Go to ```Admninistration``` on the top panel
- the left hand side you have a option ```Repositories``` its where you get to know the different repositories on your profile
- On the top right ```Create a Repository```
- Click on ```Pre-Built Setup```
- Select ```docker``` and follow the steps

