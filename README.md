# Jenkins-Zero-To-Hero

Are you looking forward to learn Jenkins right from Zero(installation) to Hero(Build end to end pipelines)? then you are at the right place. 

## Installation on EC2 Instance

YouTube Video ->
https://www.youtube.com/watch?v=zZfhAXfBvVA&list=RDCMUCnnQ3ybuyFdzvgv2Ky5jnAA&index=1


![Screenshot 2023-02-01 at 5 46 14 PM](https://user-images.githubusercontent.com/43399466/216040281-6c8b89c3-8c22-4620-ad1c-8edd78eb31ae.png)

Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances

<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">

### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">


### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Click on Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Jenkins Installation is Successful. You can now starting using the Jenkins 

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Wait for the Jenkins to be restarted.


## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.

🚀 My End-to-End CI/CD Pipeline Project (Jenkins · Argo CD · Kubernetes)

Resume-ready DevOps project demonstrating CI/CD automation, GitOps deployment, and Kubernetes orchestration on AWS EC2.

📌 Project Summary

This project demonstrates a complete end-to-end CI/CD pipeline built using Jenkins for Continuous Integration and Argo CD for GitOps-based Continuous Delivery.

A Spring Boot application is built, containerized, and deployed automatically to Kubernetes (Minikube) running on an AWS EC2 instance.

🧱 Tech Stack

CI/CD: Jenkins

Build Tool: Maven

Containerization: Docker

Container Registry: Docker Hub

CD / GitOps: Argo CD

Orchestration: Kubernetes (Minikube)

Cloud: AWS EC2 (Ubuntu)

Application: Spring Boot (Java)

🏗 Architecture
Developer Push
      ↓
GitHub Repository
      ↓
Jenkins (CI)
  - Build & Test
  - Docker Image Build
  - Push to Docker Hub
  - Update K8s Manifest
      ↓
Argo CD (GitOps CD)
      ↓
Kubernetes (Minikube on EC2)
      ↓
Spring Boot Application

⚙️ Continuous Integration (Jenkins)

The Jenkins pipeline automates the following steps:

Pulls source code from GitHub

Builds the Spring Boot application using Maven

Creates a Docker image

Tags the image using the Jenkins build number

Pushes the image to Docker Hub

Updates Kubernetes deployment manifests in GitHub

📸 Jenkins Pipeline Screenshot


<img width="1900" height="924" alt="Screenshot 2026-01-24 172955" src="https://github.com/user-attachments/assets/f605d8a9-c0a5-4730-8eab-48c8b7db39f7" />


/screenshots/jenkins-pipeline-success.png

🔄 Continuous Delivery (Argo CD – GitOps)

Argo CD is configured to:

Continuously monitor the Git repository

Detect changes to Kubernetes manifests

Automatically sync and deploy changes

Maintain desired application state

Self-heal if configuration drift occurs

📸 Argo CD Application View


<img width="1920" height="940" alt="Screenshot 2026-01-24 180617" src="https://github.com/user-attachments/assets/87d2f9c5-d2a8-4964-9201-e493c5b7ec7d" />


/screenshots/argocd-application-synced.png

☸️ Kubernetes Deployment

Application deployed as a Deployment with multiple replicas

Exposed using a NodePort Service

Namespace-based isolation implemented

Service-to-pod routing verified via endpoints

🔍 Kubernetes Verification
kubectl get pods -n apps
kubectl get svc -n apps
kubectl get endpoints -n apps spring-boot-app-service

📸 Kubernetes Pods & Services

ubuntu@ip-172-31-44-117:~$ kubectl get pods -n apps
NAME                               READY   STATUS    RESTARTS   AGE
spring-boot-app-6f94ccb4fd-4ljms   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-9hsvt   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-dvl5f   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-kc87z   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-kfj94   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-lk4z8   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-ltfsc   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-tp4lg   1/1     Running   0          44m
spring-boot-app-6f94ccb4fd-vs8qk   1/1     Running   0          44m
ubuntu@ip-172-31-44-117:~$ kubectl get svc -n apps
NAME                      TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
spring-boot-app-service   NodePort   10.107.125.244   <none>        80:30080/TCP   44m
ubuntu@ip-172-31-44-117:~$ kubectl get endpoints -n apps spring-boot-app-service
Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
NAME                      ENDPOINTS                                                        AGE
spring-boot-app-service   10.244.0.41:8080,10.244.0.42:8080,10.244.0.43:8080 + 6 more...   44m


🌐 Application Access

The application was successfully accessed via Kubernetes networking:

minikube ip
curl http://<MINIKUBE-IP>:30080

📸 Application Running in Browser

<img width="1905" height="927" alt="Screenshot 2026-01-24 173057" src="https://github.com/user-attachments/assets/384e3a10-ce70-424e-8de8-bf72cb57a54d" />


/screenshots/application-running.png








