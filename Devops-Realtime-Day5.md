

# Project:4 Node JS Application Deployment

 ![Preview](./Images/CICD-.gif)
 
* We are going to deploy a reddit clone app
* Ci/CD Pipeline diagram provided below
  ![Preview](./Images/nodejs-deploy.png)
# Setup a jenkins Dashboard
* Shell Script to install jenkins

```shell
#!/bin/bash
sudo apt update -y
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo apt-key add -
echo "deb https://packages.adoptium.net/artifactory/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
sudo apt update -y
sudo apt install temurin-17-jdk -y
/usr/bin/java --version
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl status jenkins

```
* Shell Script to install Docker

```shell
#!/bin/bash
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker ubuntu
sudo usermod -aG docker jenkins
newgrp docker
sudo chmod 777 /var/run/docker.sock
```
* Now run a sonarqube as a docker container

```
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```
* Shell Script to install Trivy

```shell
#!/bin/bash
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```
![Preview](./Images/email1.PNG)

![Preview](./Images/email2.PNG)

![Preview](./Images/email3.PNG)

![Preview](./Images/email4.PNG)

![Preview](./Images/email5.PNG)

![Preview](./Images/email6.PNG)

![Preview](./Images/email7.PNG)

# Declarative script was provided below

```Jenkinsfile
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        APP_NAME = "reddit-clone-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "devopseasy"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/devops-easy/reddit-clone.git'
            }
        }
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Reddit-Clone-CI \
                    -Dsonar.projectKey=Reddit-Clone-CI'''
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube-Token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
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
	 stage("Trivy Image Scan") {
             steps {
                 script {
	              sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image devopseasy/reddit-clone-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table > trivyimage.txt')
                 }
             }
         }
	 stage ('Cleanup Artifacts') {
             steps {
                 script {
                      sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                      sh "docker rmi ${IMAGE_NAME}:latest"
                 }
             }
         }
    }
    post {
        always {
           emailext attachLog: true,
               subject: "'${currentBuild.result}'",
               body: "Project: ${env.JOB_NAME}<br/>" +
                   "Build Number: ${env.BUILD_NUMBER}<br/>" +
                   "URL: ${env.BUILD_URL}<br/>",
               to: 'devopseasy@gmail.com',                              
               attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
        }
     }
}
```
# Steps to setup EKS Cluster
* 1--Install kubectl on Jenkins Server
```
#!/bin/bash
 sudo apt update
 sudo apt install curl -y
 curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
 sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
 kubectl version --client
```
2--Install AWS Cli
```
#!/bin/bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws --version
```
3--Installing  eksctl
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
cd /tmp
sudo mv /tmp/eksctl /bin
eksctl version
```
4--Setup Kubernetes using eksctl (``` Attach IAM Role ``` for Jenkins Instance with admin access)
```
eksctl create cluster --name devopseasy-cluster \
--region ap-south-1 \
--node-type t2.small \
--nodes 3
```
5-- Verify Cluster with below command
```
 kubectl get nodes
```
# Install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm version
```

# ArgoCD Installation on Kubernetes Cluster and Add EKS Cluster to ArgoCD
1 ) First, create a namespace
```
kubectl create namespace argocd
```
2 ) Next, let's apply the yaml configuration files for ArgoCd
```
 kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
3 ) Now we can view the pods created in the ArgoCD namespace.
```    
kubectl get pods -n argocd
```
4 ) To interact with the API Server we need to deploy the CLI:
  ```  
sudo curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.4.7/argocd-linux-amd64
  
sudo chmod +x /usr/local/bin/argocd
  ``` 
5 ) Expose argocd-server
```
     kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
6 ) Wait about 2 minutes for the LoadBalancer creation
```
kubectl get svc -n argocd
```
7 ) Get pasword and decode it and login to ArgoCD on Browser. Go to user info and change the password
```
     kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
    # give your generated output password in the below command
     echo <password> | base64 --decode
# login to your browser and change password in UI in the user info section
```
8 ) login to ArgoCD from CLI
```
#  kubectl get svc -n argocd (copy your elb url)
argocd login <elb_url> --username admin    provide the password which you set above
```
9 ) Check available clusters in ArgoCD
```
argocd cluster list
```
10 ) Below command will show the EKS cluster details
```
kubectl config get-contexts
```

11 ) Add above EKS cluster to ArgoCD with below command
```
argocd cluster add i-**@devopseasy-cluster.ap-south-1.eksctl.io --name devopseasy-cluster
```
     
12 ) Now if you give command 

``` argocd cluster list ``` # you will get both the clusters EKS & AgoCD(in-cluster). This can be verified at ArgoCD Dashboard.

### Delete EKS Cluster
```
eksctl delete cluster devopseasy-cluster --region ap-south-1     OR    eksctl delete cluster --region=ap-south-1 --name=devopseasy-cluster
```


## Project:5 Python Application Deployment using CI/CD
* Before creating automation check manually
* Take one ubuntu ec2 instance in aws cloud
* Install python3(in ubuntu it will already present)
* Check python version before installing

```
python3 --version
## installation steps
sudo apt update
sudo apt install python3 -y
sudo apt install python3-pip -y
```
* Clone the project [Refer Here](https://github.com/devops-easy/StudentCourses.git) and run it manually

```
cd StudentCourse
pip3 install requirements.txt
python3 app.py
```
* Write a Dockerfile for python app

```Dockerfile
FROM python:3.7-alpine
LABEL author=DEVOPS
LABEL blog=devopseasy@gmail.com
ARG HOME_DIR='/studentcourses'
ADD . $HOME_DIR
ENV MYSQL_USERNAME='devopseasy'
ENV MYSQL_PASSWORD='devopseasy'
ENV MYSQL_SERVER='localhost'
ENV MYSQL_SERVER_PORT='3306'
ENV MYSQL_DATABASE='test'
EXPOSE 8080
WORKDIR $HOME_DIR
RUN pip install -r requirements.txt
ENTRYPOINT ["python", "app.py"]
```
* Automate using Jenkinsfile

```
pipeline {
    agent any 

    environment {    
        DOCKERHUB_CREDENTIALS = credentials('docker_cred')
    } 
    
    stages {
        stage('SCM Checkout') {
            steps {
                // Get some code from a GitHub repository
                 git branch: 'main', url: 'https://github.com/devops-easy/StudentCourses.git'
            }
        }
        stage("Docker build") {
            steps {
                sh 'docker version'
                sh "docker build -t devopseasy/studentapp:${BUILD_NUMBER} ."
                sh 'docker image list'
                sh "docker tag devopseasy/studentapp:${BUILD_NUMBER} devopseasy/studentapp:v1"
            }
        }
        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
       
        stage('Push to Docker Hub') {
            steps {
                sh "docker push devopseasy/studentapp:v1"
            }
        }
    }
}
```



# TASK: Nodejs Application Deployment (Online Exam Portal): Optional take this as a TASK
* Before implimenting anything doit manually
* Launch one ubuntu ec2 instance with t2.medium
* Install nodejs & npm

```shell
sudo apt remove --purge nodejs npm -y
sudo apt clean
sudo apt autoclean
sudo apt install -f
sudo apt autoremove
sudo apt install curl -y
cd ~
curl -fsSL https://deb.nodesource.com/setup_17.x | sudo -E bash -
sudo apt-get install -y nodejs
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn -y
node -v && npm -v
```
* After installing clone the project from [here](https://github.com/devops-easy/online-exam-portal)

### :running: Run Locally

Clone the project

```bash
  git clone https://github.com/devops-easy/online-exam-portal.git
```

Go to the project directory

```bash
  cd online-exam-portal
```

Install dependencies

```bash
  cd backend
  npm install
  cd ../frontend
  npm install
  cd ../user-portal-frontend
  npm install
```

Start the backend server

```bash
  cd backend
  npm start
```

Start the frontend client for admin

```bash
  cd frontend
  npm start
```

Start the frontend client for teacher/student

```bash
  cd user-portal-frontend
  npm start
```
* If you get success using manually then implement automation
* Write a Dockerfile for backend & frontend
* Backend Dockerfile

```Dockerfile
FROM node:13-alpine

WORKDIR /usr/backend

COPY . .
RUN rm -rf node_modules/ package-lock.json

RUN npm install nodemon
RUN npm install


CMD ["sh", "-c", "npm run start"]
```
* Frontend Dockerfile

```Dockerfile
FROM node:17-alpine

WORKDIR /usr/frontend

COPY . .
RUN rm -rf node_modules/ package-lock.json
RUN npm install

RUN npm run build


CMD ["sh", "-c", "npm start"]
```

* To automate write a Jenkinsfile

```
pipeline {
    agent any 

    environment {    
        DOCKERHUB_CREDENTIALS = credentials('docker_cred')
    } 
    
    stages {
        stage('SCM Checkout') {
            steps {
                // Get some code from a GitHub repository
                 git branch: 'main', url:'https://github.com/devops-easy/online-exam-portal.git'
            }
        }
        stage("Docker build Frontend") {
            steps {
                dir('frontend'){
                //   sh 'docker image rm -f \$(docker image ls -q)'
                  sh "docker build -t devopseasy/onlineapp:${BUILD_NUMBER} ."  
                }
                sh 'docker image list'
                sh "docker tag devopseasy/onlineapp:${BUILD_NUMBER} devopseasy/onlineapp:v1"
                sh "docker container run -d -p 3000:3000 devopseasy/onlineapp:v1"
            }
        }
      
        stage("Docker build Backend") {
            steps {
                dir('backend'){
                //   sh 'docker image rm -f \$(docker image ls -q)'
                  sh "docker build -t devopseasy/onlineappb:${BUILD_NUMBER} ."  
                }
                sh 'docker image list'
                sh "docker tag devopseasy/onlineappb:${BUILD_NUMBER} devopseasy/onlineappb:v1"
                sh "docker container run -d -p 5000:5000 devopseasy/onlineappb:v1"
            }
        }
        
        
        
        
    }
}
```

