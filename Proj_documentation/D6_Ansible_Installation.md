# Installation of Ansible and Setting up the environment

**1. Ansible Installation in Ubuntu Version 22.04**
```
sudo apt update
sudo apt install software-properties-common       //used to install
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```
**2.Configuring Jenkins(Slave/Master) to Ansible control node**

1. After updating the system and installing ansible
   
2. In the same  ansible system go to /opt directory
```
cd opt
vi hosts //to create hosts file
```
3. jenkins master /Slave script
```
[jenkins-master]
priv i/p              // I/P of the master system
[jenkins-master:vars] // we use this variables to declare the user name and password
ansible_user=ubuntu  // As we are using Ubuntu System,change it according the system you use
ansible_ssh_private_key_file=/opt/devops_proj_udemey.pem  //change the key accordingly

[jenkins-slave]
priv i/p              // I/P of the slave system
[jenkins-slave:vars]// we use this variables to declare the user name and password
ansible_user=ubuntu  // As we are using Ubuntu System,change it according the system you use
ansible_ssh_private_key_file=/opt/devops_proj_udemey.pem  //change the key accordingly

:wq  //is to save and exit from the file
cat filename //check the content in the file
```

4. Now we have to import the .pem file to ansible machine as im using mobaxterm i can direclty drag and drop

5. Now the .pem file is in the home directory 
```
cd /home/ubuntu
mv _____.pem /opt      //now we have to move it to opt directory
```

6. Now we have to change the permissions for the .pem file
```
chmod 400 devops_proj_udemey.pem
```

7. Now we have to execute the Hosts file and test the connection
```
ansilbe -i hosts all -m ping

```

