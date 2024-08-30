# Setting up Webhooks

 
## Installing Plugins in Jenkins
1. Login to your jenkins dashboard
2. Go to the current pipeline your working on
    - ```ttrend_multibranch_pipeline```
4. Go the branch your working on
     - ```stage```
5. On the left panel you can see ```configure```
6. In ```General```
7. Scroll down to ```Scan Multibranch Pipeline Triggers```
8. Click on ```Scan by webhook```
9. Give any name you want for
    - ```Trigger token:devops_udemy_webhook_token```
    - Click on ```?``` which is right next to ```Trigger token```
    - There will be a message giving the URL of the webhooks
    - ```JENKINS_URL/multibranch-webhook-trigger/invoke?token=[Trigger token]```
    - ```JENKINS_URL:http://52.91.60.94:8080```
    - ```[Trigger token]:devops_udemy_webhook_token```
    - ```http://52.91.60.94:8080/multibranch-webhook-trigger/invoke token=devops_udemy_webhook_token```
10. Go to GitHub repo where you're working on ```DandySabrrish1999/ttrend```
    - Click on ```settings``` of the repo you can find it on the title bar of the repo where you have options like code,actions and all that and not the whole github profile settings
    - Click on ```webhooks```
    - Click on ``` Add webhook```
    - Payload URL: ```http://52.91.60.94:8080/multibranch-webhook-trigger/invoke 
                        token=devops_udemy_webhook_token```
    - Content Type: ``` application/json```
    - Which events would you like to trigger this webhook?:```Just the push event```


