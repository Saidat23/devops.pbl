# MERN STACK IMPLEMENTATION

In this project, we would be implementing a web solution based on MERN stack in AWS Cloud.

MERN Web stack consists of:

MongoDB: is a document-based, No-SQL database, used to store application data in form of documents.

ExpressJS: is a server side Web Application framework for Node.js.


ReactJS: is a frontend framework developed by Facebook. It is based on JavaScript and used to build User Interface (UI) components.


Node.js: is a JavaScript runtime environment. It is used to run JavaScript on a machine.


The diagram below illustrate how the MERN Stack works.
<img width="638" alt="MERN-stack" src="https://github.com/Saidat23/devops.pbl/assets/138054715/5cde0948-b568-4b85-80ef-62dbfe64a652">


When a user interacts with the ReactJS UI components at the application front-end in the browser. The application backend in the server respond to the frontend through ExpressJS running on top of NodeJS.

Any interaction that causes a data change request is sent to the NodeJS based Express server, which request for data from the MongoDB database. The data received is sent back to the frontend application, and presented to the user.

## DEPLOYING A SIMPLE APPLICATION THAT CREATES A TO-DO LIST.
### Step 1. BACKEND CONFIGURATION
Update Ubuntu by runing the command " sudo apt update "

Upgrade Ubuntu by runing the command " sudo apt upgrade "

To check the location of the Node.js software run the command " curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - "


#### Installing Node.js on the server.


Install Node.js and npm with the command " sudo apt-get install -y nodejs " .
NPM is a Node package manager used to install Node modules and packages and also to manage dependency conflicts.

Verify the node installation by runing the command " node -v ".

Verify the npm installation by runing the command " npm -v ".

#### Application Code Setup.

Create a new directory for the To-Do Project by running the command " mkdir Todo "

Verify the Todo directory by runing the command " ls ".

Change the current directory to the new directory by runing the command " cd Todo ".

When we initialise the project by runing the command " npm init " , a new file named package.json will be created. This file contain the information about the application and the dependencies it needs to run. Follow the prompts after runing the command. You might have to press Enter several times to accept default values, then type yes to accept to write out the package.json file. 

Run the " ls " command to verify that you have the package.json file created.


#### Installing ExpressJS
Express is a framework for Node.js, it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of the application based on HTTP methods and URLs.

Install ExpressJS with the command " npm install express "

Create a file named index.js with the commamnd " touch index.js " 

Run " ls " command to confirm that the file is created.

Install the dotenv module by running the command " npm instal dotenv " 

Open the index.js file with the command " vim index.js ". 

Type in letter "I" to switch to insert mode and then type in the code below and save.

" const express = require('express');
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
  });  "

  Click esc button and type in :w to save the code in Vim and :qa to exit Vim.

  To test if the server works, open the terminal in the same directory as the index.js file and type " node index.js "
  If everything goes well, it should read " Server running on port 5000 "
  
![Screenshot 2023-07-16 203916](https://github.com/Saidat23/devops.pbl/assets/138054715/cc1a86e3-d1b4-4721-aebf-74fba21a6000)


  Open the Security Group in your EC2 to edit the Inbound rules by creating an inbound rule to open TCP port 5000. 
  
![Screenshot 2023-08-24 174853](https://github.com/Saidat23/devops.pbl/assets/138054715/f5df2ad7-036a-4117-8c7c-835eb02dd501)

  Open the browser and try to access the server's public IP followed by port 5000. 
  i.e http://< Public IP >:5000 You should get " Welcome to Express " on your browser. 
  

![Screenshot 2023-07-17 211657](https://github.com/Saidat23/devops.pbl/assets/138054715/875a9f61-0924-4c64-8700-e57c631fc817)

This To-Do application should be able to create a new task, display list of all tasks and delete a completed task. Each task will be associated with endpoint using different http request methods: POST, GET and DELETE. For each task, we have to create route that will define various endpoints that the To-do application will depend on.

Create the routes directory using the " mkdir routes " command.

Change the directory to routes folder using "cd routes " command

Create a api.js file with the " touch api.js " command

Open the file with the command " vim api.js " then copy and paste the code below in the file. Save and exit the file.

" const express = require ('express');
  const router = express.Router();

 router.get('/todos', (req, res, next) => {

 });

 router.post('/todos', (req, res, next) => {

 });

 router.delete('/todos/:id', (req, res, next) => {

 })

 module.exports = router; ".

 #### MODELS 

 We have to create a model since we would be using Mongodb: a NoSQL database. A model makes JavaScript interactive and defines the database schema which is a blueprint of how the database would be constructed. 

 To create a Schema and a model, we would install mongoose which is a Node.js package that makes it easy to work with mongodb. 
 
 Change the directory back to To-Do folder with " cd .. " and install Mongoose with " npm install mongoose "

 Create a new folder named models by running the command " mkdir models ".

 Change directory into the newly created 'models' folder with " cd models "
 
 Inside the models folder, create a file and name it todo.js using " touch todo.js " command.

 Open the file created with the command " vim todo.js " and type in the codes below.

" const mongoose = require('mongoose');
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

  module.exports = Todo; ".

  With this done, we have to update the routes from the file " api.js " in the routes directory to make use of the new models.

Change into the routes directory, open the api.js with " vim api.js ". Delete the code inside with the " :%d " command, paste the code below, save and exit.

" const express = require ('express');
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

 module.exports = router; ".

 #### Mongodb Database
 We would need a database to store our data in. For this, we would use mLab which provides Mongodb database as a service solution (DBaaS).Sign up for a shared cluster free account on MongoDB Atlas, follow the sign up process, select AWS as the cloud provider and choose a region near you. Allow access to the MongoDB database from anywhere and change the time of deleting the entry from 6 Hours to 1 Week.

Create a file in the Todo directory and name it .env by running the command "touch .env"

vi into.env using the command "Vi.env" and add the connection string to access the database in it using the command 

< B = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority' >

Making sure to update the < username >, < password >, < network-address > and < database > according to your setup.

Next, we need to update the index.js to reflect the use of .env to enable Node.js connection to the database.

Delete existing content in the file, than paste in the code below.

To do that using vim, follow below steps

Open the file with "vim index.js"

Press esc

Type :

Type %d

Click on ‘Enter’

The entire content will be deleted, then,

Press i to enter the insert mode in vim

Then, paste the entire code below in the file.

"const express = require('express');

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

});"

Start the server using the command " node index.js " and you will see a message that says "Database connected successfully" which indicate that the backend has been configured. Next thing is to test it.


![screenshot mongoDB connection string](https://github.com/Saidat23/devops.pbl/assets/138054715/49851b37-ef2b-4418-b106-fdc9aaff3f97)



#### Testing Backend Code without Frontend using RESTful API

Presently, we have written the backend part of our To-Do application, and configured a database, but we do not have a frontend User Interface yet. We need a ReactJS code to achieve this. But during development, we will need a way to test our code using RESTfull API. Therefore, we will need to make use of some API development client to test our code.
We would make use of Postman to test our API.

Download and install postman on your machine.

Test all the API endpoints and make sure they are working. For the endpoints that require body, send JSON back with the necessary fields since it’s what we setup in our code.

Open your Postman, create a POST request to the API "http://<PublicIP-or-PublicDNS>:5000/api/todos". This request sends a new task to the To-Do list, then the application stores it in the database.

Make sure that your Headers key Content-Type has a value set as application/json.


Create a GET request to your API on "http://<PublicIP-or-PublicDNS>:5000/api/todos". This request will retrieve all existing records from the To-do application (backend requests these records from the database and sends it us back as a response to GET request).

We have tested the backend part of our To-Do application and made sure that it supports all three operations we wanted:

1.Display a list of tasks – HTTP GET request

2.Add a new task to the list – HTTP POST request

3.Delete an existing task from the list – HTTP DELETE request

With this, We have successfully created our Backend and the next is to create the Frontend.


### FRONTEND CREATION



