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
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-03%20at%2004.03.18.png)

We will run the command ls to confirm that we have package.json file created.
Next, we will Install ExpressJs and create the Routes directory.

## INSTALLING EXPRESSJS
Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore, it simplifies development, and abstracts a lot of low-level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

To use express, we will install it using npm:
```
npm install express
```
We will create a file index.js with the command below
```
touch index.js
```
We will run ls to confirm that our index.js file is successfully created
### Install the dotenv module
```
npm install dotenv
```
Open the index.js file with the command below
```
vim index.js
```
Type the code below into it and save. 
```const express = require('express');
require('dotenv').config();
 
const app = express();
 
const port = process.env.PORT || 5000;
 
app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});
 
app.use((req, res, next) => {
res.send('Welcome to Express');
});
 
app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```
Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.
Use: w to save in vim and use: qa to exit vim
Now it is time to start our server to see if it works. We will open your terminal in the same directory as our index.js file and type:
```
node index.js
```
The output displayed in the image below shows that everything went well
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-03%20at%2004.23.09.png)

Now, we need to open this port in EC2 Security Groups. Go to the security group of our instance. There we will create an inbound rule to open TCP port 5000 like this:

We will edit the inbound rule of the security group of the instance by adding the last rule in the image below.
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-03%20at%2004.33.01.png)

Open upyour browser and try to access our server’s Public IP or Public DNS name followed by port 5000:
```
http://<PublicIP-or-PublicDNS>:5000
```
This is the result we got.
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-03%20at%2004.36.55.png)

## Routes
There are three actions that our To-Do application needs to be able to do:
1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.
For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes
```
mkdir routes
```
Tip: We can open multiple shells in Putty or Linux/Mac to connect to the same EC2
Change directory to routes folder.
```
cd routes
```
Now, we will create a file api.js with the command below
```touch api.js```
we will open the file with the command below
```vim api.js```
we will press the i key for us to be able to edit the file.
Copy below code and paste it in the file. 
```
const express = require ('express');
const router = express.Router();
 
router.get('/todos', (req, res, next) => {
 
});
 
router.post('/todos', (req, res, next) => {
 
});
 
router.delete('/todos/:id', (req, res, next) => {
 
})
 
module.exports = router;
```
To save and close the file, we will press the esc button and thereafter type ":wq" where w for write and q is for quit. Afterward, we will click the enter button.


## Models
Now comes the interesting part, since the app is going to make use of Mongodb which is a NoSQL database, we will need to create a model.
A model is at the heart of JavaScript-based applications, and it is what makes it interactive.
We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document.
In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties
To create a Schema and a model, we will install mongoose which is a Node.js package that makes working with mongodb easier.
We will change directory back Todo folder with ```cd .. ```and install Mongoose
```npm install mongoose```
Create a new folder named models :
```mkdir models```
We will change the directory into the newly created ‘models’ folder with
```cd models```
Inside the models folder, we will create a file and name it todo.js
```touch todo.js```
Tip: All three commands above can be defined in one line to be executed consequently with help of && operator, like this:
```mkdir models && cd models && touch todo.js```
Open the file created with ```vim todo.js``` then paste the code below in the file:
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
 
//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})
 
//create model for todo
const Todo = mongoose.model('todo', TodoSchema);
 
module.exports = Todo;
```
Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.
In Routes directory, open api.js with ```vim api.js```, delete the code inside with :%d command and paste there code below into it then save and exit

```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');
 
router.get('/todos', (req, res, next) => {
 
//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});
 
router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});
 
router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})
 
module.exports = router;
```
The next piece of our application will be the MongoDB Database.

## MongoDB Database
We need a database where we will store our data. For this, we will make use of mLab. 
mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, we will need to sign up for shared clusters free account(https://www.mongodb.com/atlas-signup-from-mlab), which is ideal for our use case. We will sign up and also select AWS as the cloud provider, and choose a region near near us.
