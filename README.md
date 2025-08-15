# NodeJs Application Deployment Via Jenkins-CICD

## Introduction :

This project demonstrates the Continuous Integration and Continuous Deployment (CI/CD) pipeline setup for a Node.js application using Jenkins. The pipeline automates the entire process — from fetching the latest code from the Git repository, installing dependencies, running tests, building the application, and deploying it to the target server — ensuring faster, more reliable, and repeatable releases. This CI/CD pipeline reduces manual effort, minimizes deployment errors, and improves delivery speed — empowering development teams to focus more on writing code and less on managing releases.



## 🛠️ Prerequisites
Before starting, ensure you have:
- **Node.js** installed on your local and target server
- **GitHub repository** containing your Node.js application
- **Jenkins** installed and running on the CI server
- **PM2** process manager installed on the target server
- **SSH key-based authentication** between Jenkins server and deployment server
- Basic knowledge of **shell scripting** and **Jenkins Pipelines**

For jenkin set you can refer https://github.com/Vishu-Nargunde99/Jenkins-Set-Up

## Step 1 : Create Nodejs-App Server 
Lunch Instnace 
- Name and tags : Node-app- Server
- Os Images - Ubuntu
- Instance type - t2.micro
- Security group - Existing One 
- Lunch Instnace

![project screenshot](/Images/node-app.PNG)

Also enable 3000 port in security group and For jenkins server you have to enable 8080 port 

![project screenshot](/Images/SG.PNG)


## Step 2 : Set-Up node-app-server
- Update the OS
```
sudp apt update 
```
- Install Prerequisite of node-app
```
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
sudo apt install -y nodejs
```
- Then install npm
```
sudo apt install npm -y
```
- Install process management 
```
sudo npm install -g pm2
```
## Step 3 : Copy .pem file to Jenkins server
Now go to the where your .pem key is located and copy that file to your jenkin server by using below cmd this cmd run on gitbash
```
scp -i <key-file-name> <key-file-name> username@public-ip:defaultpath

example :
scp -i jenkins-file.pem jenkins-file.pem ubuntu@54.81.24.101:/home/ubuntu
```
## Step 4 : 

In the jenkins you have to install some plugins for build CICD pipeline Fr that you have to follow below steps 

- Log in into jenkins server 
- Click on Setting 
- Open plugins --> Available plugins 
- Search 
    - ssh agen --> install
    - pipeline --> install 
    - ssh build agent --> install
    - github --> install
- Now restart the jenkins 

## Step 5 : Create Job For node-js deployment

- Click on "New Item"
- Enter an item name : NodeJs-Deployment
- Select an item type : pipeline
- Click on Ok

![project screenshot](/Images/job.png)

For Node Js application code you take from this repository 
https://github.com/Vishu-Nargunde99/NodeJs-JenkinFile-CICD

You can clone this repo on your local machine and upload to your GitHub account. This code we can use anytime when you want to install nodejs application. 

## Step 6 : Create Credential
- Click on Setting 
- In security section --> Credential --> Click on global --> Add Credential
    - Kind - SSH Username with private key
    - Scope - Global(jenkins, nodes, items)
    - ID - ssh-key-credential
    - Description - ssh-key-credential
    - Username - ubuntu
    - Private Key - Check in - Entire directly
    - Click on Add
    - Copy your private-key from your local machine 
    - Create 
Here our credential is created

![project screenshot](/Images/credential.PNG)

## Step 6 : Configure Job 

- Go to created job "NodeJS-Deployment"
- click on Configure

![project screenshot](/Images/configure.png)

- Scoll down 
- In trigger section --> check in - GitHub hook trigger GITScm polling 
- Pipeline  
    - Definition - Pipeline script from SCM
        - SCM - Git
            - Repository - Copy your repository HTTPS where your jenkinfile stored
            - Branch - main/master (which is your branch name)
        - Script Path - Change Jenkinsfile To jenkinfile
- Click on "Save".
- Click on "Build Now"

![project screenshot](/Images/build%20now%20success.PNG)

Here build now is successfull. 

![project screenshot](/Images/output.PNG)

## Step 7 : Generate Automate code push 
- Log in into your GitHub Account 
- Go to your code repository of Nodejs
- Click on setting 

![projcet screenshot](/Images/webhook.PNG)

- Click on webhook
- Add webhook
- payload URl
```
http://<jenkins-server-public-ip>:8080/github-webhook/

Example
http://54.81.24.101:8080/hithub-webhook/
```
- Add webhook
- Now you can just chnage your code then add, commit and push code to your repo 

![project screenshot](/Images/final%20output.PNG)


Congratualion!.....You can deployed your Nodejs application using CICD

## Summary
This project showcases a Node.js application deployment pipeline using Jenkins CI/CD. It automates code checkout from GitHub, dependency installation, testing, building, and deployment to a remote server with zero downtime using PM2. The setup includes GitHub webhooks for automatic builds, SSH-based secure deployment, and a Jenkins Pipeline (Jenkinsfile) for managing the process. This approach ensures faster, reliable, and repeatable deployments while minimizing manual intervention and reducing errors.




