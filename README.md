# DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL8-Pod-based
![image](https://github.com/user-attachments/assets/4874083f-ffd5-4092-94af-e0491525c5bd)


In the Architecture diagram shown above a basic architecture of three-tier application is shown. In this diagram the first layer or tier is Presentation Layer or web tier. The second layer or tier is Application Layer or Business Tier and third layer or tier is Database Layer or Database tier. For web tier Nginx Service, for Application tier Tomcat and for Database tier MySQL and Memcache(for cache purpose) has been used as shown in the Architecture diagram above.

![image](https://github.com/user-attachments/assets/1eba8154-6f45-4d42-995a-c9bb55e900de)

![image](https://github.com/user-attachments/assets/2a7ca154-a504-4935-a4de-ea20fb47fb5d)

Architecture diagram for three-tier Application and three-tier Application deployment is as shown above.

I have used terraform script as provided in this repository to create the Azure VMs, Application Gateways, AKS and Azure Database for PostgreSQL Flexible Servers.

I have created RabbitMQ cluster with three kubernetes pods. Follow below procedures to achieve the same.

Create Azure DevOps pipeline for deployment of Tomcat Application Pods, RabbitMQ Pods, Memcached Pods and MySQL Pods using the azure-pipelines.yaml file prresent with this repository.

```
Add a user and permission to the user in RabbitMQ. Assign Role for High Availability (HA) using the commands as shown below.

rabbitmqctl add_user test test
rabbitmqctl set_user_tags test administrator
rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
rabbitmqctl set_policy ha-all ".*" '{"ha-mode":"all","ha-sync-mode":"automatic"}' 
```
![image](https://github.com/user-attachments/assets/3b44c24e-d66f-4973-ada1-ec9bce83ec29)

Create Ingress rule using the rabbitmq-ingress-rule.yaml file as present in this repository and do the entry in Azure dns-zone for Hosts corresponding to IP Address as shown below.
![image](https://github.com/user-attachments/assets/16616516-e18b-4cec-b05f-1da2f1834c7f)
![image](https://github.com/user-attachments/assets/94fa423d-413d-49aa-afc8-a22b0bb4f903)
![image](https://github.com/user-attachments/assets/d73ba45a-c1b6-4266-a9d8-6dc6a4fd4edd)

The source code is present in Azure Repos. I have taken the Source Code present in GitHub Repository https://github.com/kamalmohan217/Three-tier-WebApplication.git and did changes in pom.xml, Dockerfile, application.properties as shown below.
![image](https://github.com/user-attachments/assets/d570b3f2-88b6-4a63-afad-911b1112711a)
![image](https://github.com/user-attachments/assets/a3b3988a-a145-4b13-a0bf-c7a895018a11)
![image](https://github.com/user-attachments/assets/cc67ec39-5df9-414c-b87e-48741cc5a01e)
![image](https://github.com/user-attachments/assets/0e361d92-b5e4-45c4-8c8a-7c5d58878b5c)

For Azure Artifacts, copy below section and paste to pom.xml as I have shown in above screenshot.

![image](https://github.com/user-attachments/assets/d781918a-7baf-47da-be3e-a8b4a61b6da6)
![image](https://github.com/user-attachments/assets/7b734dd6-64ba-4171-81db-e8f1b4225ff5)
