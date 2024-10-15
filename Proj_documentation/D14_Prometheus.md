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
- So when you see the list of services under namspace monitoring you can see the service TYPE it says cluster IP. So in order to access it we have to change it to Nodeport

