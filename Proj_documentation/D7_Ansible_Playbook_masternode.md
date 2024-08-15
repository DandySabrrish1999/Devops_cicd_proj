# Configuring the the Ansible playbook



1. **apt key**:[Click here](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_key_module.html)
2. **adding repo**:[Click here](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_repository_module.html)

3.**openjdk-11-jdk**: Includes the JRE plus additional tools needed for developing Java applications. Install this if you are a Java developer.

4. **openjdk-11-jre**: Includes only the runtime environment for running Java applications, without the development tools. Install this if you only need to run Java applications.





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
