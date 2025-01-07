
# How to Install Sonarqube in Ubuntu Linux
SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs and code smells on 29 programming languages.
## Prerequsites 
- Virtual Machine running Ubuntu 22.04 or newer
### Update Package Repository and Upgrade Packages

``` shell title="Run from shell prompt" linenums="1"
sudo apt update
sudo apt upgrade
```
## PostgreSQL
### Add PostgresSQL repository
``` shell title="Run from shell prompt" linenums="1"
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null
```
### Install PostgreSQL
``` shell title="Run from shell prompt" linenums="1"
sudo apt update
sudo apt-get -y install postgresql postgresql-contrib
sudo systemctl enable postgresql
```
### Create Database for Sonarqube
``` shell title="Set password for postgres user" linenums="1"
sudo passwd postgres
```
``` shell title="Change to the postgres user" linenums="1"
su - postgres
```

``` sql title="Set password and grant privileges" linenums="1"
# create a user 
createuser sonar
# now login to psql 
psql 
# execute below 3 lines
ALTER USER sonar WITH ENCRYPTED password 'sonar';
CREATE DATABASE sonarqube OWNER sonar;
grant all privileges on DATABASE sonarqube to sonar;
# now exit by executing below command
\q
exit
```
## Adoptium Java 17
``` shell title="Switch to root user" linenums="1"
sudo bash
```
### Add Adoptium repository
``` shell title="Add adoptium repository" linenums="1"
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo apt-key add -
echo "deb https://packages.adoptium.net/artifactory/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
```
### Install Java 17
``` shell title="Update repository and install Java" linenums="1"
sudo apt update
sudo apt install temurin-17-jdk -y
update-alternatives --config java
/usr/bin/java --version
exit 
```
## Linux Kernel Tuning
### Increase Limits
``` shell title="Run from shell prompt" linenums="1"
sudo vim /etc/security/limits.conf
```
Paste the below values at the bottom of the file
``` shell title="Add these values" linenums="1"
sonarqube   -   nofile   65536
sonarqube   -   nproc    4096
```
### Increase Mapped Memory Regions
``` shell title="Run from shell prompt" linenums="1"
sudo vim /etc/sysctl.conf
```
Paste the below values at the bottom of the file
``` shell title="Add these values" linenums="1"
vm.max_map_count = 262144
```
### Reboot System
``` shell title="Run from shell prompt" linenums="1"
sudo reboot
```
## Sonarqube
### Download and Extract
``` shell title="Run from shell prompt" linenums="1"
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
sudo apt install unzip
sudo unzip sonarqube-9.9.0.65466.zip -d /opt
sudo mv /opt/sonarqube-9.9.0.65466 /opt/sonarqube
```
### Create user and set permissions
``` shell title="Run from shell prompt" linenums="1"
sudo groupadd sonar
sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar
sudo chown sonar:sonar /opt/sonarqube -R
```
### Update Sonarqube properties with DB credentials
``` shell title="Run from shell prompt" linenums="1"
sudo vim /opt/sonarqube/conf/sonar.properties
```
Find and replace the below values, you might need to add the sonar.jdbc.url
``` shell title="Run from shell prompt" linenums="1"
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```
Create service for Sonarqube
``` shell title="Run from shell prompt" linenums="1"
sudo vim /etc/systemd/system/sonar.service
```
Paste the below into the file
``` shell title="Paste the below contents" linenums="1"
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```
Start Sonarqube and Enable service
``` shell title="Paste the below contents" linenums="1"
sudo systemctl start sonar
sudo systemctl enable sonar
sudo systemctl status sonar
```
Watch log files and monitor for startup
``` shell title="Watch logs" linenums="1"
sudo tail -f /opt/sonarqube/logs/sonar.log
```
Access the Sonarqube UI
``` shell title="Paste the below contents" linenums="1"
http://<IP>:9000
```
# Jenkins & Sonarqube Integration
* Follow Steps as shown below
  
![Preview](./Images/sonar1.PNG)

![Preview](./Images/sonar2.PNG)

* Go to Jenkins Dashboard: Manage Jenkins: Tools
![Preview](./Images/sonar3.PNG)

![Preview](./Images/sonar-config1.PNG)

![Preview](./Images/sonar-config2.PNG)


![Preview](./Images/sonar-config3.PNG)

![Preview](./Images/sonar-config4.PNG)

![Preview](./Images/sonar-config5.PNG)

![Preview](./Images/sonar-config6.PNG)

![Preview](./Images/sonar-config7.PNG)

![Preview](./Images/sonar-config8.PNG)

* Go to Jenkins Dashboard: Manage Jenkins: System
![Preview](./Images/sonar4.PNG)

# Pipeline Project

```Jenkinsfile
pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/DevopsEasy/CultiGestApp.git'
            }

        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }

        }
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }

        }
    }
}
```



# Adding Docker in Pipeline Project:1
* Go to Jenkins and install docker related plugins
* Add docker hub credentials in Global Credentials
* Pipeline Script as been provided below

```Jenkinsfile
pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "cultigestapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "devopseasy"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"

    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/DevopsEasy/CultiGestApp.git'
            }

        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }

        }
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }

        }
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
```
# Kubernetes Setup

## Step 1: Update System Packages

```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl wget
```

## Step 2: Install K3s Kubernetes Cluster

```
curl -sfL https://get.k3s.io | sh -
sudo systemctl status k3s
```

## Step 3: Configure Kubectl

```
mkdir ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config && sudo chown $USER ~/.kube/config
sudo chmod 600 ~/.kube/config && export KUBECONFIG=~/.kube/config
```
* Execute the below commands

```
kubectl get nodes
kubectl cluster-info
```
## Step 4: Testing (Optional)

```
kubectl create deployment  nginx-deployment --image nginx --replicas 2
kubectl get deployment nginx-deployment
kubectl get pods
```
* Now, expose this deployment, run

```
kubectl expose deployment nginx-deployment --type NodePort --port 80
kubectl get svc nginx-deployment
curl <<Your-Ubuntu-System-IP-Address>:<port>>
```

# Install ArgoCD

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
* Change Service to NodePort
   * Edit the service can change the service type from ``` ClusterIP ``` to ``` NodePort ```

   ```
   kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}' 
   ```
   * Fetch password

   ```
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
   ```

# Deploy Demo Application
* You can use the below repository to deploy a demo nginx application

```
https://github.com/DevopsEasy/ArgoCD.git
```
* Scale replicaset

```
kubectl scale --replicas=3 deployment nginx -n default
```
* Clean Up

```
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete namespace argocd
```
# Task to implement for Project:2
* Deploy a springboot application provided the code below

```
https://github.com/devops-easy/spring-petclinic.git
```
# Note: Build time is more than 20 minutes
* Gitops Repo also provided below

```
https://github.com/devops-easy/gitops-springpetclinic.git
```

# Task to implement for Project:3
* Deploy a insurance application provided the code below

```
https://github.com/DevopsEasy/star-agile.git
```
# Note: Skip the tests
* Gitops Repo also provided below

```
https://github.com/devops-easy/gitops-springpetclinic.git
```

