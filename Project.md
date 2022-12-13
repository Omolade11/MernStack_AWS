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
mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, we will need to sign up for shared clusters free account(https://www.mongodb.com/atlas-signup-from-mlab), which is ideal for our use case. 
1. We will sign up and also select AWS as the cloud provider, and choose a region near near us.
2. We will complete the get started quick guide Then we allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing). We will also change the time of deleting the entry from 6 Hours to 1 Week. The end result should look like this
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.40.19.png)
3. Create a MongoDB database and collection inside mLab by clicking on "add my own data" like in the image below
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2008.37.11.png)
4. In the ```index.js``` file, we specified ```process.env``` to access environment variables, but we have not yet created this file. So we need to do that now.
Create a file in your Todo directory and name it .env. 
```
touch .env
vi .env
```
5. Add the connection string to access the database in the .env file created above, just as below:
```
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```
Ensure to update ```<username>, <password>, <network-address> and <database>``` according to your setup.
Here is how to get your connection string 
![](https://github.com/TobiOlajumoke/DevOps-Projects/blob/main/Project_3/Image/DB%20connect.png)
![](https://github.com/TobiOlajumoke/DevOps-Projects/blob/main/Project_3/Image/MongoDB_connect.png)
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2008.54.53.png)
6. Now we need to update the index.js to reflect the use of ```.env``` so that Node.js can connect to the database.
Simply delete existing content in the file, and update it with the entire code below.
To do that using vim, we will open the file with ```vim index.js```, Press esc, Type :, Type %d and Hit ‘Enter’.
The entire content will be deleted, then,
Press i to enter the insert mode in vim
Now, paste the entire code below in the file.
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();
 
const app = express();
 
const port = process.env.PORT || 5000;
 
//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));
 
//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;
 
app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});
 
app.use(bodyParser.json());
 
app.use('/api', routes);
 
app.use((err, req, res, next) => {
console.log(err);
next();
});
 
app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

7. Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.
Start our server using the command:
```node index.js```
We will see a message ‘Database connected successfully’ 
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2009.12.41.png)
This means we have our backend configured. Now we are going to test it.

So far we have written the backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTful API. Therefore, we will need to make use of some API development client to test our code.

we will use [Postman](https://www.getpostman.com/) to test our API.
Click [Install Postman](https://www.getpostman.com/downloads/) to download and install postman on your machine.
Click [HERE](https://www.youtube.com/watch?v=FjgYtQK_zLE) to learn how perform [CRUD operartions](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) on Postman.
We should test all the API endpoints and make sure they are working. For the endpoints that require body, we should send JSON back with the necessary fields since it’s what we setup in our code.
1. Now we will open our Postman, create a POST request to the API ```http://<PublicIP-or-PublicDNS>:5000/api/todos```. This request sends a new task to our To-Do list so the application could store it in the database.
Note: make sure your set header key Content-Type as application/json
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2010.31.26.png)
We can add this value as the body
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2010.31.50.png)
The image below shows the result of our POST request
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2010.32.07.png)
2. We will create a GET request to our API on ```http://<PublicIP-or-PublicDNS>:5000/api/todos```. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request). The image below shows our GET request and the result we derived.
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2010.42.04.png)

## Frontend Creation
Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.
In the same root directory as our backend code, which is the Todo directory, run:
 ``` npx create-react-app client ```
This will create a new folder in your Todo directory called client, where we will add all the react code.

### Running a React App
Before testing the react app, there are some dependencies that need to be installed.
1. Install concurrently. It is used to run more than one command simultaneously from the same terminal window.
``` npm install concurrently --save-dev ```
2. Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
``` npm install nodemon --save-dev ```

In Todo folder, we will open the package.json file and thereafter change the highlighted part of the below screenshot and replace with the code below.
```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.02.13.png)
The end result should look like the image below:
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.04.51.png)

### Configure Proxy in ```package.json```
1. Change directory to ‘client’
```cd client```
2. Open the package.json file
```vi package.json```
3. Add the key value pair in the package.json file ```"proxy": "http://localhost:5000"```. Like in the image below
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.12.33.png)
The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos
4. Now, we have to ensure we are inside the Todo directory, and simply do:
```npm run dev```
Our app should open and start running on localhost:3000
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.16.47.png)
Important note: In order to be able to access the application from the Internet we have to open TCP port 3000 on EC2 by adding a new Security Group rule. 
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.15.15.png)

### Creating our React Components
One of the advantages of react is that it makes use of components, which are reusable and also make code modular. 
For our Todo app, there will be two stateful components 
One of the advantages of react is that it makes use of components, which are reusable and also make code modular. For our Todo app, there will be two stateful components and one stateless component.
1. From our Todo directory we will run
```cd client```
move to the src directory
```cd src```
Inside our src folder create another folder called components
```mkdir components```
Move into the components directory with
```cd components```
Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.
```touch Input.js ListTodo.js Todo.js```
Open Input.js file
```vi Input.js```
Copy and paste the following  
```
import React, { Component } from 'react';
import axios from 'axios';
 
class Input extends Component {
 
state = {
action: ""
}
 
addTodo = () => {
const task = {action: this.state.action}
 
    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }
 
}
 
handleChange = (e) => {
this.setState({
action: e.target.value
})
}
 
render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}
 
export default Input
```
2. To make use of Axios, which is a Promise based HTTP client for the browser and node.js, we need to cd into your client from our terminal and run yarn add axios or npm install axios.
Move to the src folder
```cd ..```
Move to clients folder
```cd ..```
Install Axios
```npm install axios```

## FRONTEND CREATION (CONTINUED)
1. Go to the ‘components’ directory
```cd src/components```
2. After that, we will open our ListTodo.js
```vi ListTodo.js```
in the ListTodo.js we will copy and paste the following code
```
import React from 'react';
 
const ListTodo = ({ todos, deleteTodo }) => {
 
return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}
 
export default ListTodo
```
3. Then in our Todo.js file you write the following code:
```
import React, {Component} from 'react';
import axios from 'axios';
 
import Input from './Input';
import ListTodo from './ListTodo';
 
class Todo extends Component {
 
state = {
todos: []
}
 
componentDidMount(){
this.getTodos();
}
 
getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}
 
deleteTodo = (id) => {
 
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))
 
}
 
render() {
let { todos } = this.state;
 
    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )
 
}
}
 
export default Todo;
```
4. We need to make little adjustment to our react code. We willl Delete the logo and adjust our App.js to look like this.
Move to the src folder
```cd ..```
5. Make sure that we are in the src folder and run
``` vi App.js ```
Copy and paste the code below into it
```
import React from 'react';
 
import Todo from './components/Todo';
import './App.css';
 
const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}
 
export default App;
```
After pasting, exit the editor.
6. In the src directory open the App.css
```vi App.css```
Then paste the following code into App.css:
```
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}
 
input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}
 
input:focus {
outline: none;
}
 
button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}
 
button:focus {
outline: none;
}
 
ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}
 
li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}
 
@media only screen and (min-width: 300px) {
.App {
width: 80%;
}
 
input {
width: 100%
}
 
button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}
 
@media only screen and (min-width: 640px) {
.App {
width: 60%;
}
 
input {
width: 50%;
}
 
button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
```
Exit
7. In the src directory open the index.css
```vim index.css```
Copy and paste the code below:
```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}
 
code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```
8. Go to the Todo directory
```cd ../..```
When we are in the Todo directory, we will run:
```npm run dev```
Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all our tasks.
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.30.37.png)
![](https://github.com/Omolade11/MernStack_AWS/blob/main/Images/Screenshot%202022-12-13%20at%2011.31.07.png)





