# DevOps-Project-3tier-Application-Deployment-AKS-RabbitMQ-Memcache-MySQL8-Pod-based
![image](https://github.com/user-attachments/assets/35709490-0b26-4bf1-af44-30a72ff74cc6)

In the Architecture diagram shown above a basic architecture of three-tier application is shown. In this diagram the first layer or tier is Presentation Layer or web tier. The second layer or tier is Application Layer or Business Tier and third layer or tier is Database Layer or Database tier. For web tier Nginx Service, for Application tier Tomcat and for Database tier MySQL and Memcache(for cache purpose) has been used as shown in the Architecture diagram above.

![image](https://github.com/user-attachments/assets/6b183cd5-6b8d-4bea-84b5-3a836f3e2d9d)

![image](https://github.com/user-attachments/assets/d8ea5471-2628-4018-aac3-f7b720442fd7)

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
![image](https://github.com/user-attachments/assets/e9d73082-fd5c-4805-a1e7-62c4c2b951cc)

Create Ingress rule using the rabbitmq-ingress-rule.yaml file as present in this repository and do the entry in Azure dns-zone for Hosts corresponding to IP Address as shown below.
![image](https://github.com/user-attachments/assets/fe0a0349-729e-4b5d-805e-4cc6c54ab8b7)
![image](https://github.com/user-attachments/assets/de8b359e-0c55-46cd-b911-fbdcab8ed567)
![image](https://github.com/user-attachments/assets/a159ecb4-3ab9-44a0-bcde-c988fe8fc3c0)

The source code is present in Azure Repos. I have taken the Source Code present in GitHub Repository https://github.com/singhritesh85/Three-tier-WebApplication.git and did changes in pom.xml, Dockerfile, application.properties as shown below.
![image](https://github.com/user-attachments/assets/93e45705-d8af-4470-9b4b-6a61795e61b7)
![image](https://github.com/user-attachments/assets/52a4d878-e348-4879-9080-520fd8ae0144)
![image](https://github.com/user-attachments/assets/165fcdd2-bd18-4216-9d68-4dd870ff4315)
![image](https://github.com/user-attachments/assets/17350f3f-b2ab-4249-a487-0dbd0ef4fdaf)
![image](https://github.com/user-attachments/assets/6ce3a359-5e45-4c06-9873-b2fdfa12bd4b)

For Azure Artifacts, copy below section and paste to pom.xml as I have shown in above screenshot.

![image](https://github.com/user-attachments/assets/fd075de2-d74e-4ea6-9dbf-01df84a32d95)

