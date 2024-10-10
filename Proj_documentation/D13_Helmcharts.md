![image](https://github.com/user-attachments/assets/f7359c3a-5836-4201-a84b-34efe91bedd6)


## Helm 
- Helm is a pcakage manager for kubernetes
- A chart is a package of pre-configured kubernetes resources
- A repository is a group of published charts which can be made availabe to others
- Helm is used to repeat the tasks and applications
- Helm should be installed on jenkins slave or build server/node

#### Quick difference between docker,docker-hub & Helm charts
**Docker and Docker Hub:**
- Imagine you bake a cake. The cake is your app.
- Docker is like the cake box that wraps up your cake so you can transport it anywhere without it falling apart. This box makes sure the cake stays in perfect condition, whether it's on your table or someone else's.
- Docker Hub is like an online bakery store where you can upload your cake (inside the Docker box) so other people can download it and enjoy the same cake. They don’t need to bake it from scratch because the whole cake (your app) is ready.

**Helm:**
- Now, you don’t just need a cake; you want to set up a full party (like a big app with many pieces).

- Helm is like a party planner that organizes everything for you: cake (your Docker app), decorations, drinks, plates, and music (other configurations and services). You don’t have to do all these things one by one—Helm plans the whole party and sets it up.

- So, while Docker helps package the cake, Helm helps plan the whole party by organizing everything (including the cake) and setting it up in Kubernetes.


### Installation of Helm charts
- So i have written a automation script to install helm and mysql
- Dont foget you have to put this playbook in ansible system
    - ```vi jenkins_slave_helm1.yaml```
```
---
- name: Automate Helm installation and deploy MySQL chart
  hosts: jenkins-slave
  become: yes
  tasks:

    # Step 1: Install curl (for Ubuntu)
    - name: Install curl on Ubuntu
      apt:
        name: curl
        state: present
      when: ansible_os_family == "Debian"

    # Step 2: Download Helm installation script
    - name: Download Helm installation script
      get_url:
        url: https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
        dest: /tmp/get_helm.sh
        mode: '0700'

    # Step 3: Run the Helm installation script
    - name: Install Helm
      command: /tmp/get_helm.sh
      args:
        creates: /usr/local/bin/helm

    # Step 4: Validate Helm installation
    - name: Validate Helm installation
      command: helm version
      register: helm_version_output

    - name: Print Helm version
      debug:
        msg: "Helm is installed successfully! Version: {{ helm_version_output.stdout }}"

    # Step 5: Configure AWS EKS kubeconfig
    - name: Configure AWS EKS kubeconfig
      shell: /usr/local/bin/aws eks update-kubeconfig --region us-east-1 --name devops_udemy_eks01
      args:
        executable: /bin/bash
      register: kubeconfig_output

    - name: Print kubeconfig setup status
      debug:
        msg: "Kubeconfig updated: {{ kubeconfig_output.stdout }}"

    # Step 6: Set up Helm repo and update
    - name: Add stable Helm repo
      command: helm repo add stable https://charts.helm.sh/stable

    - name: Update Helm repo
      command: helm repo update

    # Step 7 (NEW): Uninstall existing MySQL Helm chart if present
    - name: Uninstall existing MySQL Helm chart if present
      command: helm uninstall demo-mysql
      ignore_errors: yes  # Ignore errors if the release doesn't exist

    # Step 8: Install MySQL Helm chart using dynamic release name
    - name: Install MySQL Helm chart with dynamic name
      command: helm install demo-mysql-{{ 99999 | random }} stable/mysql
      register: helm_install_output

    - name: Print Helm MySQL install status
      debug:
        msg: "{{ helm_install_output.stdout }}"
```
- Run the playbook
    - ```ansible-playbook -i hosts jenkins_slave_helm1.yaml --check```  ## For dry-run
    - ```ansible-playbook -i hosts jenkins_slave_helm1.yaml ```  ## To run 

- To search a repo in helm with mysql : ```helm search repo mysql```
- To check if its installed or not: ```helm version```
- To check the repo: ```helm list```
- 
####
![image](https://github.com/user-attachments/assets/ffc684f4-5adb-4ec3-bbab-33afcfa337aa)
