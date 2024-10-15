## Prometheus
1. What is prometheus?
- Prometheus is an Open-source systems monitoring and alerting toolkit.
- Prometheus collects and stores the metrics as time series data.
- Prometheus Scrapes targets
- PromQL is language to query time series in Prometheus
- Service Discovery helps find our services and monitor them
- Exporters helps to monitor 3rd party components
- Prometheus can send alerts to the Alert manager
- Prometheus runs on port 9090 and Alert Manager runs on 9093

## Installing prometheus in Jenkins_slave/build_node and deploying using helm charts
- Lets see the step by step process how to install helm
1.  we create a namespace called monitoring
```
kubectl create namespace monitoring
kubectl get ns
```


2. Adding prometheus helm chart
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```


3. Now we Update the helm chart repository
```
helm repo update          ## This is not necessary its when you feel if the repo is outdated you can do else you can skip
helm repo list
```


4. Now we install prometheus in monitoring namespace
```
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring
  -prometheus : deployment name
  -prometheus-community/kube-prometheus-stack : Helm chart name
  -monitoring: Under monitoring namespace

helm list
helm list -A                        ## Gives all the deployments under all the namespaces
```


5. If you want to download the prometheus into your system
```
helm pull prometheus-community/kube-prometheus-stack v65.2.0
ls
tar -xzvf kube-prometheus-stack v65.2.0.tgz
ls
cd kube-prometheus-stack
cd ..
```

6. Checking the services under monitoring
```
kubectl get all -n monitoring
```
## Accessing prometheus dashboard
- So when you see the list of services under namspace monitoring you can see the service TYPE it says cluster IP. So in order to access it we have to change it to Nodeport its the same thin we have dont in D13 to under installation of mysql helmchart.
- As its a time taking process to get into  each file and changing for temporary we do run-time edit, its nothing but we make temp changes for that particular point of time once we delete the service and install again its goes to the base version not the changes we have done.
```
cd kube-prometheus-stack
ls
cd ..
kubectl edit svc prometheus-kube-prometheus-prometheus -n monitoring
    - svc: service
    - prometheus-kube-prometheus-prometheus: Service we are working on
```
- ![image](https://github.com/user-attachments/assets/4b69c98a-9537-40ff-a4f0-fbe52ed23d6a)
    - type: LoadBalancer
    - We change it fro Clusterip to LB

```
kubectl get all -n monitoring

![image](https://github.com/user-attachments/assets/7a01e541-612a-4a32-b8a5-3e7f68660563)
  - Now its changed to LB from clusterip
```

## Accessing the Prometheus from AWS console
- Go to your aws console
- Open loadbalancer
- You can see a LB created now click on it
![image](https://github.com/user-attachments/assets/3b9e0bcc-a00f-4334-97c4-58a4acf5f301)
- The above is the home page of the LB so you can copy paste the DNS name
      - ad3a9a4f8cd2549879ede45d3c31b7aa-650070942.us-east-1.elb.amazonaws.com:9090
- ![image](https://github.com/user-attachments/assets/9e1c7a3e-08c9-453a-a43f-2eca90bc5d9b)
    - The above is the GUI of prometheus
    - ```machine_cpu_cores```: Is used to retrieve the utilization it executes as a promql query
 
- Just go through the GUI and explore the other options like alrets,graphs and all









