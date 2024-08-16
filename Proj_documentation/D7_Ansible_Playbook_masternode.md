# Configuring the the Ansible playbook

1.**Jenkins Debian Packages**:[Click Here](https://pkg.jenkins.io/debian-stable/)

1.**Apt key**:[Click here](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_key_module.html)

2.**Adding repo**:[Click here](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_repository_module.html)

3.**Openjdk-11-jdk**: Includes the JRE plus additional tools needed for developing Java applications. Install this if you are a Java developer.

4.**Openjdk-11-jre**: Includes only the runtime environment for running Java applications, without the development tools. Install this if you only need to run Java applications.





```
---
- name: Setting up Master node
  hosts: jenkins-master
  become: true

  tasks:
    # Installing apt repo key

    - name: Installing jenkins repo key  
      ansible.builtin.apt_key:
        url: https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
        state: present

    # Adding Jenkins Repository

    - name: Add jenkins repo
      ansibile.builtin.apt_repository:
        repo: 'deb https://pkg.jenkins.io/debian-stable binary/'
        state: present

    # Installing jdk 11

    - name: Installing java jdk11
      apt:
        name: openjdk-11-jdk
        state: present

    # Installing jenkins

    - name: Installing jenkins
      apt:
        name: jenkins
        state: present

    # Start jenkins service

    - name: Start the jenkins service
      service:
        name: jenkins
        state: present
    
    # Restarting the service 

    - name: Restarting the Jenkins service
      service:
        name: Jenkins
        enabled: yes
```

5.To run the ansible playbook

- ansible_jenkins_master_setup          //my ansible playbook name

```
ansible-playbook -i hosts ansible_jenkins_master_setup.yaml
```

6. Checking whether its installed or not

- Connect to your jenkins-master server through public i/p of that machine
- To check jenkins is up and running

```
service jenkins status
```

7. Checking it with the Public ip of the jenkins master

```
publicip:8080  // as jenkins runs on 8080 port
```
