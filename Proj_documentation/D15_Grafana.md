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
