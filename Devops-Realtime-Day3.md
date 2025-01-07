
# Continuous Integration
* What are different CI Engines/tools available according to the timeline.
* The landscape of Continuous Integration (CI) tools has evolved significantly over the years, reflecting changesin software development practices and technology. Here's a timeline of notable CI engines and tools,categorized into different generations based on their characteristics and functionalities.

## First Generation CI Tools
1. CruiseControl (2001): One of the earliest CI tools, it allowed developers to automate the build processand run tests. It laid the groundwork for many subsequent tools.

2. Jenkins (2011): Forked from Hudson after a dispute with Oracle, Jenkins quickly became the leadingCI/CD platform. Its extensibility through plugins and strong community support helped it dominate themarket for many years. Jenkins introduced a job-based approach where users could chain taskstogether to create complex CI/CD pipelines

3. TeamCity (2006): Developed by JetBrains, TeamCity provided a robust CI server with features like buildhistory, version control integration, and a user-friendly interface.

4. Bamboo (2007): Created by Atlassian, Bamboo integrated seamlessly with other Atlassian productsand offered a comprehensive CI/CD solution with strong support for deployment projects

# Second Generation CI Tools

1. Travis CI (2011): This tool popularized cloud-based CI, automatically detecting commits in GitHubrepositories and running tests in response. It uses a simple YAML configuration file for setup, making itaccessible for smaller projects.

2. CircleCI (2011): Similar to Travis CI, CircleCI offers cloud-based CI services with a focus on speed andefficiency, allowing for easy customization of test environments and workflows.

3. GitLab CI (2014): Integrated directly into GitLab, this tool offers a seamless experience for CI/CD withinthe GitLab ecosystem, allowing users to define pipelines in the same repository as their code.

# Third Generation CI Tools

1. GitHub Actions (2019): This tool introduced CI/CD capabilities directly within GitHub, allowing users toautomate workflows based on repository events. It supports a wide range of actions and integrations,making it highly flexible and user-friendly.

2. Azure DevOps (formerly VSTS) (2018): Microsoft’s Azure DevOps provides a comprehensive suite forCI/CD, including build pipelines, release management, and integration with Azure services, catering toenterprise needs.

3. Buddy (2018): A relatively newer tool, Buddy focuses on ease of use with a visual interface for creatingpipelines and supports various integrations, appealing to teams looking for a straightforward CI/CDsolution

## History of Jenkins

* Sun Microsystems introduced a project of Continuous Integration and they called it as hudson.
* This project was open-source
* Oracle acquired sun microsytems and the idea was to make hudson paid software
* Jenkins is a project which is developed in Java
* Idea is to provide feedback to the developer who has pushed the commit, regarding its correctness.
* Installing jenkins on Linux
    * install jdk 17
    * [Refer Here](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu) for the rest of the steps

* Commands

```
sudo apt update
sudo apt install openjdk-17-jdk -y
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```
## Maven Steps for Installation:
* Install Java

```
sudo apt update && sudo apt install openjdk-17-jdk -y 
----------------------Or -------------------------
sudo apt update
sudo apt-cache search jdk
sudo apt install openjdk-17-jdk -y
```
* maven (tar based installed): Lets download maven from [Refer Here](https://maven.apache.org/download.cgi)
```
cd /tmp
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
tar -xvf apache-maven-3.9.6-bin.tar.gz
sudo mv apache-maven-3.9.6 /opt/maven
```

  
* ADD opt/maven/bin to PATH in ``` /etc/environment ```

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/opt/maven/bin"
```
* exit and relogin


* Execute ``` mvn -version ``` command

## To remove OpenJDK (the one you've already installed)

```
update-alternatives --list java
sudo apt-get purge openjdk-\* -y
sudo apt autoremove
#relogin
```

* This installation is used to run jenkins as a service/daemon on ubuntu.
* Installation of jenkins on linux leads to creation of a user jenkins.
* Jenkins runs on 8080 port
* Navigate to ``` http://<ip>:8080 ``` and enter the password as suggested and proceed with defaults.

* Jenkins can be defined as CRON on steriods
* In Jenkins we create projects and schedule them to run based on

    * manual trigger
    * peroidic trigger
    * based of version control trigger
    
* Jenkins Projects are of 3 types

    * Free style project (UI Based)
    * Scripted Pipeline
    * Declarative Pipeline


## Free style project
* This is classic project type where we use jenkins UI to define our CI
* Free style project has following sections

    * General
    * Source Code Management
    * Build Triggers
    * Build Environment
    * Build steps:These are individual build steps
    * Post Build Actions

## JENKINS HOME DIRECTORY:
* on linux machines this is equivalent to ``` /var/lib/jenkins ```.
* Jenkins stores everything in Jenkins Home Directory.
* Backup of Jenkins is nothing but backup of Jenkins Home Directory.
* TO change this directory set environmental variable ``` JENKINS_HOME_DIRECTORY ```

* When we create a new project a folder will be created in ``` /var/lib/jenkins/jobs ```
* Every jenkins project has a workspace i.e. the folder in which the job is executed
   * /var/lib/jenkins/<project-name>
   
* Jenkins can do any activity which a system user ``` jenkins ``` can do
* From jenkins if you need elevated permissions add ``` jenkins ``` user to the sudoers group or give sudo permissions


## CI/CD Implementation
* Continuous Integration and Continuous Deployment (CI/CD) is a set of practices that enable software development teams to automate the process of integrating code changes, testing, and deploying applications quickly and efficiently. AWS, Jenkins, Docker, and Kubernetes are popular tools used in the CI/CD pipeline to streamline software development and deployment.

## PREREQUISITES
* AWS Account
* Github Account
* Dockerhub Account

## Setups to do CI/CD using AWS, Jenkins, Docker:
* CREATE A T2.MEDIUM UBUNTU EC2 INSTANCE IN AWS IN REGION

![Preview](./Images/cicd.PNG)

* Install JDK on AWS EC2 Instance

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

![Preview](./Images/cicd0.PNG)

* Install and Setup Jenkins [Refer Here](https://www.jenkins.io/doc/book/installing/linux/) click on ubuntu

```shell
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
* Whenever we install jenkins a jenkins user will be created in backend
* We need to setup a new password

```
sudo passwd jenkins
# set any password you like
```
* In AWS related EC2 instances by default password based authentication will be disabled.
* To enable it execute the below commands
```
vi /etc/ssh/sshd_config
# go here: PasswordAuthentication yes
# change from no to yes
# restat the ssh service to rectify the chnages
sudo service sshd restart
```
* Update visudo and assign administration privileges to jenkins user :
* Open the file /etc/sudoers in vi mode
```
sudo vi /etc/sudoers
sudo visudo
jenkins ALL=(ALL:ALL) NOPASSWD:ALL

 ```
* Add the following line at the end of the file
![Preview](./Images/cicd11.PNG)

* After adding the line save and quit the file.Now we can use Jenkins as root user and for that run the following command
![Preview](./Images/cicd12.PNG)

![Preview](./Images/cicd10.PNG)

![Preview](./Images/cicd1.PNG)

![Preview](./Images/cicd2.PNG)

* Now go to AWS dashboard -> EC2 -> Instances(running)and click on Jenkins-EC2
* Copy the Public IPv4 address.
* Change the SG to open for Jenkins.
* Alright now we know the public IP address of the EC2 machine, so now we can access Jenkins from the browser using the public IP address followed by port 8080.
* Copy the below key and paste it on JENKINS
* After completing the installation of the suggested plugin you need to set the First Admin User for Jenkins.

![Preview](./Images/cicd3.PNG)

![Preview](./Images/cicd4.PNG)

![Preview](./Images/cicd5.PNG)

![Preview](./Images/cicd6.PNG)

![Preview](./Images/cicd7.PNG)

![Preview](./Images/cicd8.PNG)

![Preview](./Images/cicd9.PNG)



# Optional ignore the below docker & aws cli installation 
* Install Docker with user jenkins :

```shell
sudo apt install docker.io
docker --version
docker ps
sudo usermod -aG docker jenkins
sudo reboot
```
![Preview](./Images/cicd13.PNG)

* Install and Setup AWS CLI :

```shell
sudo apt install awscli
curl "https://awscli.amazonaws.com/awscli-exe-linuxx86_
64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --update
aws –version
```

![Preview](./Images/cicd14.PNG)

## CI/CD Project Structure

![Preview](./Images/cicd20.png)

# Sample Pipeline Project

```Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Checking Java Version..!!!') {
            steps {
                sh 'java --version'
            }
        }
    }
}
```

### Adding an SSH Based Agent to Jenkins
## Prerequisites 
- Virtual Machine running Ubuntu 22.04 or newer
### Update Package Repository and Upgrade Packages

``` shell title="Run from shell prompt" linenums="1"
sudo apt update
sudo apt upgrade
```

## Create Jenkins User
``` shell title="Run from shell prompt" linenums="1"
sudo adduser jenkins
```
* In AWS related EC2 instances by default password based authentication will be disabled.
* To enable it execute the below commands
```
sudo vi /etc/ssh/sshd_config
# go here: PasswordAuthentication yes
# change from no to yes
# restat the ssh service to rectify the chnages
sudo service sshd restart
```
* After that execute below commands
```
sudo visudo
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
jenkins ALL=(ALL:ALL) NOPASSWD:ALL
```
Logout and ssh back as user Jenkins

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

## Docker
### Install using the repository

Install Docker Engine
``` shell title="Run from shell prompt" linenums="1"
sudo apt-get update
sudo apt-get install docker.io -y
```
### Manage Docker as a non-root(jenkins) user
* Login as a jenkins user ``` su -l jenkins ```
Add your user to the docker group.
``` shell title="Run from shell prompt" linenums="1"
sudo usermod -aG docker $USER
```
Run the following command to activate the changes to groups:
``` shell title="Run from shell prompt" linenums="1"
newgrp docker
```
Verify that you can run docker commands without sudo.
``` shell title="Run from shell prompt" linenums="1"
docker run hello-world
```

## Connect to Remote SSH Agent
* Login to ``` Jenkins Master ``` as ``` jenkins ``` user, from that server (ec2 instance) login to agent server
From the Jenkins UI (Controller)
``` shell title="Run from shell prompt" linenums="1"
ssh jenkins@$AGENT_HOSTNAME
```
Create private and public SSH keys. The following command creates the private key jenkinsAgent_rsa and the public key jenkinsAgent_rsa.pub. It is recommended to store your keys under ~/.ssh/ so we move to that directory before creating the key pair.
``` shell title="Run from shell prompt" linenums="1"
 mkdir ~/.ssh; cd ~/.ssh/ && ssh-keygen -t rsa -m PEM -C "Jenkins agent key" -f "jenkinsAgent_rsa"
```
Add the public SSH key to the list of authorized keys on the agent machine
``` shell title="Run from shell prompt" linenums="1"
cat jenkinsAgent_rsa.pub >> ~/.ssh/authorized_keys
```
Ensure that the permissions of the ~/.ssh directory is secure, as most ssh daemons will refuse to use keys that have file permissions that are considered insecure:
``` shell title="Run from shell prompt" linenums="1"
chmod 700 ~/.ssh
 chmod 600 ~/.ssh/authorized_keys ~/.ssh/jenkinsAgent_rsa
```
Copy the private SSH key (~/.ssh/jenkinsAgent_rsa) from the agent machine to your OS clipboard
``` shell title="Run from shell prompt" linenums="1"
cat ~/.ssh/jenkinsAgent_rsa
```

# Now you can add the Agent on the Jenkins UI (Controller)
* Go to Manage Jenkins , click on Nodes
![Preview](./Images/master-slave1.PNG)

![Preview](./Images/master-slave2.PNG)

![Preview](./Images/master-slave3.png)

![Preview](./Images/master-slave4.PNG)

![Preview](./Images/master-slave5.PNG)




![Preview](./Images/pro1.PNG)

![Preview](./Images/pro2.PNG)

![Preview](./Images/pro3.PNG)

![Preview](./Images/pro4.PNG)

![Preview](./Images/pro5.PNG)

![Preview](./Images/pro6.PNG)

![Preview](./Images/pro7.PNG)

![Preview](./Images/pro8.PNG)

# Add Github Credentials to Jenkins

![Preview](./Images/pro9.PNG)

![Preview](./Images/pro10.PNG)

![Preview](./Images/pro11.PNG)

![Preview](./Images/pro12.PNG)

![Preview](./Images/pro13.PNG)

![Preview](./Images/pro14.PNG)

![Preview](./Images/pro15.PNG)

![Preview](./Images/pro16.PNG)

![Preview](./Images/pro17.PNG)

![Preview](./Images/pro18.PNG)

## Sample CICD Pipeline Script

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
    }
}
```



