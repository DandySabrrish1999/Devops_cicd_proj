## Grafana
1. What is Grafana
-Grafana IS multi-platform Open-source analytics and interactive visualization web application.
- It provides charts, graphs and alerts for the web when connected to supported data services.
- Grafana allows us to query, visualize, alert on and understand our metrics, no matter where they are stored. Some supported data sources in addition to Prometheus are AWS
  CloudWatch, AzureMonitor, PostgreSQL, Elasticsearch and many more.
- We can create our own dashboards or use the existing ones provided by Grafana. We can personalize the dashboards as per our requirements.

#### Example:
Imagine you run a smart home system with different devices like thermostats, lights, and security cameras. 
You want to monitor how much energy your home is using, the temperature in each room, and whether all your 
security cameras are online.

Here’s how Grafana would work in this situation:

- Data Collection:
All your smart home devices (thermostats, cameras, lights) collect data. For example,
the thermostat records temperature, and the security system monitors camera status. This data is sent to a storage system, like a database.

- Data Visualization with Grafana:
Grafana takes this data and displays it in an easy-to-read dashboard. It creates charts showing the temperature in each room over time, graphs showing energy usage, and status panels indicating whether all your security cameras are working.

- Custom Dashboards:
You can customize your Grafana dashboard to show only the things you care about. For example, you can create a section that shows the average temperature of your home, another section for security camera status, and a third section for tracking energy consumption over the past week.

- Alerts:
Grafana can also be set up to send alerts. For example, if the temperature in one room goes above a certain level or if a security camera goes offline, Grafana will notify you (via email, Slack, or other methods).


### Accessing grafana
```
kubectl get all -n monitoring
```
![image](https://github.com/user-attachments/assets/21a4468a-881e-44b6-9f1e-62768fa402ea)
- The above is the service will be working on to access
```
kubectl edit svc prometheus-grafana -n monitoring
  - To make necessary changes in prometheus-grafana service
```
- ![image](https://github.com/user-attachments/assets/268678c6-8b8a-4087-be90-435a1fb404f6)
    - have to change from ClusterIP to Loadbalance inorder to access it
- Go to your aws load balancer console where there will a other LB created
![image](https://github.com/user-attachments/assets/e88f0b86-b915-4e56-8089-283c62acb542)
- Copy the DNS and paste it new browser
- You will be prompted to enter the credentials to acess grafana
- Even thought if the password is changed you refer to the documentation 
```
username: admin
password: prom-operator
```

### Wokring with Grafana

![image](https://github.com/user-attachments/assets/1e6aaec0-90ec-463e-b09c-78e637532917)
  - The above is the grafana UI and on the lefthand side you can see a the dashboard button click on     that and search for USE(Utilization, Saturation, Errors)

- ![image](https://github.com/user-attachments/assets/638829e7-2384-4e3e-a0d8-b3cd238cb884)
  - Above is the dashboard for utilizations of CPU currently running
  - ```instance```: This is a filter on top where you can find wrt the i/p address
- You can check out for other monitoring service
    - Kubernetes / Compute Resources / Cluster
    - Kubernetes / API server
- If you want to create a own dashboard
    - Click on dashboard
    - Top right click on new
    - click on new dashboard
