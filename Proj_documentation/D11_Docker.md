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
- You can attach the below code to your existing file or create a new file copying the existing code
  
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
```
- run the above playbook
```
ansible-playbook -i hosts ansible_jenkins_slave_setup2.yaml --check  ## To dry run the playbook
ansible-playbook -i hosts ansible_jenkins_slave_setup2.yaml          ## To run the playbook
```




### Docker File


```
FROM openjdk:8
ADD jarstaging/com/valaxy/demo-workshop/2.1.2/demo-workshop-2.1.2.jar ttrend.jar
ENTRYPOINT [ "java", "-jar","ttrend.jar" ]
```


- Creation and publishing of docker image on artifactory 
