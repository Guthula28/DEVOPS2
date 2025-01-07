## Nodejs Application Deployment (Online Exam Portal)
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
