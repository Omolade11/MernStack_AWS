# WEB STACK IMPLEMENTATION (MERN STACK) IN AWS
In this project, we will implement a web solution based on the MERN stack in AWS Cloud.
MERN Web stack consists of the following components:
1. MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
2. ExpressJS: A server-side Web Application framework for Node.js.
3. ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
4. Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

Project Prequisite: AWS EC2 instance, Postman and mLab for mongoDB database.

## Steps Involved
1. BACKEND CONFIGURATION
2. INSTALLING EXPRESSJS
3. MODELS
4. MONGODB DATABASE
5. FRONTEND CREATION

## Implementation
Since we are using the Ubuntu AMI of AWS EC2 instance to host our application. On our terminal, we will change directory to the folder we have our .pem file. In this case, it is in the Downloads folder

```
cd ~/Downloads
```

Now, we will change permissions for the private key file (.pem), otherwise we will get the error “Bad permission”

```
sudo chmod 0400 <private-key-name>.pem
```

We will then connect to the instance by running 

```
ssh -i <private-key-name>. pem ubuntu@<Public-IP-address>
```
## BACKEND CONFIGURATION
We will update ubuntu by running

```
sudo apt update
```
We will upgrade ubuntu by running
```
sudo apt upgrade
```
Let’s get the location of Node.js software from Ubuntu repositories.
```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
We will install Node.js on the server
Install Node.js with the command below
```
sudo apt-get install -y nodejs
```
Note: The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.
We will verify the node installation with the command below
```
node -v
```
Verify the node installation with the command below
```
npm -v 
```
### Application Code Setup
Since we will be creating a todo application, we will create a new directory for our To-Do project:
```
mkdir Todo
```
We will run the command below to verify that the Todo directory is created with ls command
```
ls
```
Now we will change our current directory to the newly created one:
```
cd Todo
```
Next, we will use the command npm init to initialise our project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. We can press Enter several times to accept default values, then we will accept to write out the package.json file by typing yes.
```
npm init
```
![]()
