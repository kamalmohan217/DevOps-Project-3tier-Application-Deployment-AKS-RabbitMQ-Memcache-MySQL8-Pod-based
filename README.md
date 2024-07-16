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

Provide Contributor Access for Azure Artifacts as shown in screenshot below.

![image](https://github.com/user-attachments/assets/a26ab93a-97a7-4115-89ec-5914c32a2c4f)
![image](https://github.com/user-attachments/assets/37f58cac-8408-436e-b8fd-3a2d36fb4726)

I have created configmap to import the tables into the database. This step was done in the Azure-Pipeline itself as shown in the screenshot below.
![image](https://github.com/user-attachments/assets/c2c38d12-e45e-4029-a479-93b74ce786bd)
![image](https://github.com/user-attachments/assets/d150fc5d-715f-408e-854f-895672a85ac8)

I have created Service service Connection for SonarQube, Azure Artifacsts, Azure Container Registries and DockerHub Repository as shown below.

![image](https://github.com/user-attachments/assets/98f98925-a52f-405a-8e90-4ef3ab39275a)
![image](https://github.com/user-attachments/assets/7e58016c-336c-4fde-9cf3-d3170b1a28e1)

Now Run the Azure Pipeline. Create the URL using ingress rule for service present in the file ingress-rule.yaml in this repository. Do the entry for this URL with Public IP in Record Set of Azure DNS Zone. Access the newly created URL and provide username admin_vp and password admin_vp.
![image](https://github.com/user-attachments/assets/84aeb827-0ee4-4dfe-b10d-13e8f41f82d6)
![image](https://github.com/user-attachments/assets/503afb7d-2988-43e1-8916-6b091820eac4)
![image](https://github.com/user-attachments/assets/33773fbd-c200-4312-adee-b3ea1d6df737)
![image](https://github.com/user-attachments/assets/972e6574-c8af-4a65-9323-2011908afb85)

When you click on the User for the first time it will get the values from MySQL Database and store it in Memcache, so that next time when you click on the same user it will provide the values from the Memcache itself.

![image](https://github.com/user-attachments/assets/646800cd-a606-405e-917d-a62054f40ffc)
![image](https://github.com/user-attachments/assets/39ac9f0b-5aea-48f2-ab86-8dea34190d7d)

After running the Azure Pipeline Screenshots for RabbitMQ, SonarQube, Azure Artifacts are as shown in the Screenshot below.
![image](https://github.com/user-attachments/assets/6b5dabff-3883-47ff-9a31-19f6d02465ef)
![image](https://github.com/user-attachments/assets/2f4b4e1e-273e-4755-8e8d-e31137480906)
![image](https://github.com/user-attachments/assets/73b8f0d1-5716-43b3-9618-64802161c8f3)
![image](https://github.com/user-attachments/assets/2571decc-fe04-4c04-9a70-958085e71484)
![image](https://github.com/user-attachments/assets/56ee1656-5acc-4d26-b1de-14bb67c3b0bf)

```
Create secret in Kubernetes for DockerHub Repository access

kubectl create secret docker-registry dockerhub-auth --docker-server=docker.io --docker-username=XXXXXXXXX --docker-password=XXXXXXXXX -n mysql
```

```
Create secret in Kubernetes for Azure Container Registries access

kubectl create secret docker-registry backend-auth --docker-server=https://akscontainer24registry.azurecr.io --docker-username=akscontainer24registry --docker-password=cXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXR -n backend
```
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
```
Source Code:-  https://github.com/kamalmohan217/Three-tier-WebApplication.git
```
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
<br><br/>
```
Reference:-  https://github.com/logicopslab/vprofile-project.git
```
