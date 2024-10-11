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

1. Installing helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

2. Checking if its installed or not
```
helm version             ## To check the version
helm list                ## To check the repo
```

3. Configuring .kube/config
```
aws eks update-kubeconfig --region us-east-1 --name devops_udemy_eks01
```

4. Searching for a particular repo and checking if helm is working or not
```
helm repo list                                        ## To check the repo
helm repo add stable https://charts.helm.sh/stable    ## We are a repo called stable
helm repo update                                    
helm search repo <helm-chart>                          ## To search a particular repo 
    - helm search repo stable
```


5. Installation of Mysql charts
- ```demo-mysql```: You can give any name you wanted to but make sure you rememer moving forward its the same thing we use
```
helm install demo-mysql stable/mysql
helm pull stable/mysql
tar -xzvf mysql-1.6.9.tgz
```


6. Modification and test
```
ls                            ## You can find a folder called mysql
cd mysql/
ls
vi Chart.yaml
    - Change the version to version: 1.7.0
vi values.yaml
    
 - type: NodePort        ## Change to type to NodePort make sure you follow the same its case sensitive
   port: 3306
   nodePort: 32000        ## uncomment this line 
   # loadBalancerIP:

helm list
    - helm uninstall ---pod name---       ## If anything is present unistall it
helm install demo-mysql mysql            ## we run this again becaue to reflect the above made changes and make sure you run this command in home directory sudo su - ubuntu the wokrspace we are working on its just come outside the mysql folder as simple

```




### Working on creation of ttrend helm chart

1. Creation of  ttrend helm
```
helm create ttrend            ## ttrend folder with helm chart defaults
```

2. Inserting our .yaml k8's configuration files
```
ls
cd ttrend
cd templates
ls
rm -rf *
sudo su - ubuntu
ls
cd /opt/kubernetes
mv deployment.yaml namespace.yaml secret.yaml service.yaml ttrend/templates
kubectl get all -n dev-udemy-namespace            ## To get the pods under the ns
kubectl delete --pod name--- -n dev-udemy-namespace        ## have to delete before we create the same through helm charts
```
3. Deploying our ttrend helm
- helm install ttrend-v1 ttrend
    - ```ttrend-v1```: Name of your choice
    - ```ttrend```: Folder where we have modified
```
helm install ttrend-v1 ttrend
kubectl get ns
kubectl get all -n dev-udemy-namespace 
```

4. Creating the whole same thing with jenkins deploy job
- Now we have created and tested if our ttrend manaully now we deploy through jenkins job
```
## Eitheir just replace the deploy stage in jenkins with the below one
 stage(" Deploy ") {
       steps {
         script {
            echo '<--------------- Helm Deploy Started --------------->'
            sh 'helm install ttrend-v1.0 ttrend-0.1.0.tgz'
            echo '<--------------- Helm deploy Ends --------------->'
         }
       }
     }
```

4.1
```
helm uninstall ttrend-v1
kubectl get ns
helm list
```
4.2
```
## If you already have that folder delete it and put your new folder 
helm package ttrend                ## This basically creates a zip file of our ttrend folder
    - ttrend ttrend-0.1.0.tgz       ## This is how it creates
```
- Now drag and drop the zip folder form the mobaxterm ui to ttrend folder in our local system
```
git status
git add .
git commit -m "pushing ttrend zip helm"
git push origin main
```
- Now before pushing  the code just make sure you doble check that we have deleted the pods from the manual creation 
```
helm list
kubectl get ns
```
- After successful running of the pipeline go to your slave system
```
helm list
kubectl get ns
```

- The below screenshot shows you how to delete the service and the how to see under the ns
####
![image](https://github.com/user-attachments/assets/ffc684f4-5adb-4ec3-bbab-33afcfa337aa)





------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Playbook to install helm and mysql
- First install it manually and then go to this automation script
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
