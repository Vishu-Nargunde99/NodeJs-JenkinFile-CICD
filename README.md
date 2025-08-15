# NodeJs Application Deployment Via Jenkins-CICD



## prerequisite :
- Jenkin-server Set up 

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



