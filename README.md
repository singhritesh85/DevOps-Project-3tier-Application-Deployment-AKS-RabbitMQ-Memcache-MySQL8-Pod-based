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
![image](https://github.com/user-attachments/assets/6ce3a359-5e45-4c06-9873-b2fdfa12bd4b)

For Azure Artifacts, copy below section and paste to pom.xml as I have shown in above screenshot.

![image](https://github.com/user-attachments/assets/fd075de2-d74e-4ea6-9dbf-01df84a32d95)
![image](https://github.com/user-attachments/assets/17350f3f-b2ab-4249-a487-0dbd0ef4fdaf)

Provide Contributor Access for Azure Artifacts as shown in screenshot below.

![image](https://github.com/user-attachments/assets/7ab147b5-9250-4ff6-a972-7953924f4a53)
![image](https://github.com/user-attachments/assets/6448484b-2f7e-4e73-97cd-bbc04a08b62f)

I have created configmap to import the tables into the database. This step was done in the Azure-Pipeline itself as shown in the screenshot below.
![image](https://github.com/user-attachments/assets/5fd34021-94e9-4783-82ba-72e7b0dfc459)
![image](https://github.com/user-attachments/assets/39904f02-3f8d-4dce-8370-98e59545be29)

I have created Service service Connection for SonarQube, Azure Artifacsts, Azure Container Registries and DockerHub Repository as shown below.

![image](https://github.com/user-attachments/assets/ddb22a7f-c30e-444c-972f-5779257456fe)
![image](https://github.com/user-attachments/assets/b41e22a6-f3e1-458d-92fe-6b825cc8cbac)

Now Run the Azure Pipeline. Create the URL using ingress rule for service present in the file ingress-rule.yaml in this repository. Do the entry for this URL with Public IP in Record Set of Azure DNS Zone. Access the newly created URL and provide username admin_vp and password admin_vp.
![image](https://github.com/user-attachments/assets/f9031450-3c7c-40c4-bb7b-abaec0130166)
![image](https://github.com/user-attachments/assets/251c4f21-3bfc-4a61-9b2e-2c3f2635dad5)
![image](https://github.com/user-attachments/assets/e180d826-05ea-426a-992d-79b4a666d4a4)
![image](https://github.com/user-attachments/assets/2255875d-e0a6-4548-95bc-629829faf123)

When you click on the User for the first time it will get the values from MySQL Database and store it in Memcache, so that next time when you click on the same user it will provide the values from the Memcache itself.

![image](https://github.com/user-attachments/assets/74e36c97-17d3-4770-8dd0-dc0271d604cd)
![image](https://github.com/user-attachments/assets/80d161a9-4d66-4612-86ba-a58ce105ba15)

After running the Azure Pipeline Screenshots for RabbitMQ, SonarQube, Azure Artifacts are as shown in the Screenshot below.
![image](https://github.com/user-attachments/assets/99232d56-42d6-40fd-9352-f73b1a1aa07a)
![image](https://github.com/user-attachments/assets/00baccfa-c68b-443e-9b81-52c29e6ced08)
![image](https://github.com/user-attachments/assets/de4ae218-df46-4d7b-8c8c-587d91955835)
![image](https://github.com/user-attachments/assets/9573a2aa-5595-47a6-8dad-5d83e4a45fd6)
![image](https://github.com/user-attachments/assets/7bac1ffa-b9bf-46e3-853f-f4ccb081109d)

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
Source Code:-  https://github.com/singhritesh85/Three-tier-WebApplication.git
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
