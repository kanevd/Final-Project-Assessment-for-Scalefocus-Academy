# Final-Project-Assessment-for-Scalefocus-Academy

Prerequisites:
1. Install the necessary tools: Minicube, Helm and Jenkins.
- Minikube: https://minikube.sigs.k8s.io/docs/start/
- Helm: https://github.com/helm/helm/releases
- Jenkins: https://www.jenkins.io/doc/book/installing/windows/

2. Separate repo in your GitHub Profile named: Final Project Assessment for Scalefocus Academy.
- Press on the + sign in the top right corner -> New Repository -> name it Final Project Assessment for Scalefocus Academy.

Requirement for the Project Assessment:
1. Download Helm chart for WordPress. (Bitnami chart:https://github.com/bitnami/charts/tree/main/bitnami/wordpress)
- I downloaded and installed it locally:
![](images/wordpressdownloaded.PNG)
2. In values.yaml, you need to change line 543 from type: LoadBalancer to type: ClusterIP:
- Then, open up and edit the values.yaml file line 534 from LoadBalancer to ClusterIP:
![](images/clusterip.PNG)
- After changing the value, I had to upgrade the file with the following command so the changes can take effect:
![](images/upgradehelm.PNG)
- To install the chart, I used the following command:
![](images/installwpchart.PNG)
- And verify that it's of type ClusterIP:
![](images/clusteripservice.PNG)
- Port-forward it so we can login locally:
![](images/portforward.PNG)
- Check the sample login page:
![](images/localhostsamplepage.PNG)
3. Create a Jenkins pipeline that checks if wp namespace exists, if it doesn’t then it creates one.
   Checks if WordPress exists, if it doesn’t then it installs the chart.
- 
