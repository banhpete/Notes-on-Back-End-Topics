# Notes on Back-End Technologies
My notes on back-end technologies. This is by no mean a comprehsive coverage of all the back-end technologies. The following topics are covered:
 - [Notes on HTTP Request](#http-request)
 - [Apache](#apache)
 - [Node.JS](#nodejs)
 - [ExpressJS](#expressjs)
 - [Mongoose](#mongoose)
 - [Cross Origin Resource Sharing - CORS](#cors)
 - [Python](#python)

## HTTP Request
To talk about the back end technologies, it's important to make a note of how HTTP Requests work first. HTTP stands for Hypertext Transfer Protocol, it's the network protocol that powers the communications across the Web. Essentially, anytime a user accesses a website, HTTP is used to deliver the goods from the server back to the browser. **So what are the steps?**

- The browser will send what is called the HTTP Request to the webserver of the URL
  - In this request there will be Request Method: GET, POST, DELETE, etc
  - Thwere will also be the request target which is usually a URL
  - Potentially Cookies
  - Parameters 
- The webserver handles the HTTP Request
  - How the webserver handles the request depends on the info in the request mentioned above.
  - It's a back-end developer's job to develop the server side code to handle how a web server handles the request.
- Once the HTTP Request is proceessd by a server side, the webserver will send back a HTTP Response
  - HTTP Response will always contain a status code indicating to the client how the request/resposne went. It always a three-digit number that falls within the following categories:
    - 1xx Informational
    - 2xx Success
    - 3xx Redriection
    - 4xx Client Error
    - 5xx Server Error
  - The HTTP response may also have a file to server in the browser, or CSS to style HTML, or JavaScript to run in the HTML.

## Apache
Apache is a software that serves as a web server to deliver or serve websites on the internet. Now remember, it is a web server, not a physical server, however it does runs on the physical server to act as the middle man between a client (browser) and a server. That is, a client will request a file from the server, the request will first go to the apache server to process and it will pull what it needs from the server to and send it back to the client.

As a web server software, Apache can have modules added to increase its functionality. This includes a module for server side languages such as PHP, Perl or Lua to send files to a client that is modified based on certain settings/parameters sent to the server. So, essentially, what happens when you request a .php file from the apache server is that the apache server will access the php interpreter from the php module and run the php file so that it creates an HTML file to serve.

To configure Apache settings, there is a text file you need open up and edit. You need to configure this file for the domain name, module use, etc.

Nginx is another web server simiiar to Apache, however it was made with more heavy traffic in mind.

See https://www.hostinger.com/tutorials/what-is-apache or https://kinsta.com/knowledgebase/what-is-apache/ or https://www.quora.com/How-does-PHP-work-with-Apachefor more details 

## NodeJs
### What is Node.js and why do we use it?
Node.js is a JavaScript Runtime Environment based off the same JavaScript Engine that Google Chrome uses (V8), that allows us to run JavaScript outside of the browser and instead run it directly on a computer or server OS. Ultimately, the benefit of this is that we can use Node.js to then create a web server on a computer or server, and then allow it to handle HTTP requests. So, when there is a HTTP request for a certain file, developers will use Node.js to grab the right file and serve it to a client. Additionally, Node.js can also be used to modify the file (HTML) before serving it, essentially it serves the same role as PHP and Apache in a LAMP.XAMP,MAMP stack where PHP dynamically modifies a webpage and Apache handles the HTTP requests. 

### Node Modules 
Node.js also has the the Node Pack Manager (NPM) which allows us to download what are considered node modules which are used for many different purpose of the web development process, from developing the material for webpages to actually hosting the web server.

All node modules are just a single or a series of JavaScript files. To use these modules, we need to "require" them into a variable. To require them in we need to use the path of file or, if we installed it from npm we just need the name. **It's important to note that any variables initialized in these modules are only local to that module, they do not pollute the global scope**

It's important to note that when we "require" a module in, it's not like we are just calling that file and running it. This [link](https://www.freecodecamp.org/news/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8/) here does a great job of explaining it but essentially, when we use "require" we wrap the module in a function, making the variables local to that module, since all variables are function scoped. 

For us to use any of the functions or data from this module, the JavaSript file must have a module.exports object that contains all the functions/data (Remember, it's a module, hence "Module" and it needs to have exports, hence "exports". It's our job to specify what the module will export though!). When we wrap the module in a function, the function returns this module.export object. **It technically doesn't have to be an object, it can just be one function, or an array, or string, etc. but an object allows us to export more**

**To summarize:**
 -Node modules are JavaScript Files
 -They are "required" in from other files
 -The "require" function wraps the module in a function and returns the module.exports object
 -Variables inside the module are local to that file

## ExpressJS
### What is ExpressJS and why do we use it?
ExpressJS is a node module, which serves a web framework whiich helps us use Node.Js to set up a web server and program it to handle HTTP request. This can be installed from NPM.

### How do we use ExpressJS?
First we need to have ExpressJS installed through NPM, and then to be able to use express in the CLI, we need to have express-genetaor installed globally. ** If we wanted to only have it installed locally, we can use the key word 'npx' before 'express' to use it in the CLI.

Then we can create an Express App Skeleton by using the command "express <name of app>. This will create a folder with the following structure:
 ```
 	├── app.js
	├── bin
	│   └── www
	├── package.json
	├── public
	│   ├── images
	│   ├── javascripts
	│   └── stylesheets
	│       └── style.css
	├── routes
	│   ├── index.js
	│   └── users.js
	└── views
	    ├── error.js
	    └── index.js
 ```
 
Now, express is non-opinionated, meaning, there isn't a right way to have things structured to use express. However for the interest of learning, we will follow some best practices set out by some people but this is by no mean a definite "MUST DO" - the structure above is just what express recommends. 

So when the folder is created we technically now have used ExpressJS to create and host a webserver. At this point we just need to use node to run the "www" file OR we can use **nodemon**, a really helpful node module that helps us refresh the web server whenever we make changes, **it's recommended we use this.**

Let's go through this structure:
 - bin/www \- This is the file that creates the server requires the express object from the app.js file and creates a server with the following line:
 ```
 var app = require('../app');
 var server = http.createServer(app);
 ```
This essentially says, create a server, and whenever a request is made, run the functions from the app module. **Keep in mind that the object returned from "/app" is not just any object, it's an express object. This express object may technically a function itself which calls all the other functions that we define in it.** Regardless of how it works under the hood, the important thing to know here is that a request will go through all the functions specified in the app module '/app'.

- app.js \- This is the file that specifies what will happen to a request when it comes through. Looking at the file you will see that we first require all the modules that we need, and then we create the express object that we will export. Everything after creating the express object are the actions that will be done to to a HTTP Request. We will see the following:
  - app.set() which sets express settings such as what engine we want to use for rendering templates.
  - app.use() which tells us a request what to do. This can be taking from the data from a request and parsing it, running middlewares (functions that does work on the HTTP request), or even redirecting the HTTP Request to other files depending on the path that is included in the HTTP request. To add to the last action, we will see the app.use redirect an HTTP request to another module, these modules are called routes.
  
- routes folder \- This folders contain the routes module that app.js will redirect to. In them are an router object that is exported, essentially these are like mini express object. So these routers take the HTTP requests and then it goes it's own .set and .use like above, but instead of using app.use, or app.set we use router.use or router.get. So the purpose of this folder/these files or to further route an HTTP request to different files, it's a type of further organization in the code - we can decide to not include these files and just keep everything in app.js but then app.js could end up becoming super long.

- views folder \- This folder holds the templates that we can have rendered and sent to the client. Generally these files may be a pug type or ejs, these are view engines that will go through the template and create an HTML file.

- models folder \- This folder will hold modules again, but this seems to be used to hold data and provide functions to interact with the data.

- controller folder \- Now this doesn't exist naturally in a new express app but it does seem to be good practice. The controller folder will hold modules that have functions tell us how to interact with data from the models folder based on the HTTP request and it will also be the one to send back a HTTP response. Essentially these are modules that control the flow of the response.

Notice how in almost all of these modules, anytime ".use" is used and a function is passed in, there are always three parameters, **a req, res and next**. What's happening is that when a HTTP request gets to this ".use" it will actually pass in the HTTP request, response, and the next function that is to be called out if we don't send back a response. This allows the function to interact with the HTTP and ultimately send back a HTML response as well. If no response is sent back in the function, then we need it to call "next()" otherwise the client may never receive a response back as their request essentially gets stuck.

To send back a response, some of the options are:
 - res.render()
 - res.redirect()
 - res.send();

### Summary of the Express Flow
To get a better idea of the flow of express, we take the points above and create sort a list of the steps:
 - Create a webserver object using http.createServer, the creation of this server should take in a function that you want to run when the server recieves a request. Because it's a function to respond to the a request to the webserver, the function needs to take in two arguments, the request (req) and the response (res), these are the HTTP request and response objects. The request object is to allow us to see what a client is requesting, and the response object allows us to respond to the client, it contains methods for us to use.
 - Instead of passing in a function we pass in the express object that is exported from our "server.js" file, which itself is most likely a function that runs the functions in the express object.
 - Then we tell the server to listen a specific port using "server.listen(<port number>)
 - Now when the server recieves a request the express object takes that request and runs in through several functions in the "server.js" file.
 - The express object uses "app.use('/', <route-name>)" to route the request to specific functions in a router object, these are essentially mini express objects. Essentially, we're sending the request around to see what web server function (a function that receives a req and res) to run. 
	
### In relation to MVC Design Pattern
Model-View-Controller (MVC) is a software design pattern for developing user interfaces diving the trelated program logic into three interconnected elements, Model, View, and Controller.
  - **Views** Is what the user will see and interact with. Depending on how a user interacts with the Views, the controller component will do different things.
  - **Controller** This is the component that accepts input from the view, and these inputs are translated to actions for the model. It also receives input from the model and translates it to actions for the view (like how things should be rendered).
  - **Model** This is the component that handles the data of the application.
  
When using express, and the express generator, the general design of the express app follows a MVC pattern, where you have the "Views" folder, "Controller" folde rand the "Models" Folder.
	
## Mongoose
Mongoose is an JavaScript module that allows us to interact with a MongoDB in a more intuitive way, instead of writing MongoDB commands everytime we want to pull data from it, Mongoose provides us with easier syntax/functions. It also allows some structure to our data in the MongoDB non-schema database. Mongoose is what is known as a Object Document Mapper (ODM) meaning it basically provides us with objects that allows us to interact with our database in our code. For example, when we have a mongoDB collection we want to work in our application, and we wanto edits document or add documents, Mongoose, makes it into an object and we can work with it easier rather than writing several lines of MongoDB commands. 

Some important notes on how we use Mongoose are:
- To allow us to work with mongodb we first need to connect to it. to do so we with mongoose, it's a simple line
```
mongoose.connect(<local host path to db name>, {connection options})
```
- The mongoose has an object in it called "connection" which we can use to check that status of the connection. "mongoose.connection.on('expected status', callback)" On whatever connection status of the connection to the db, run this callback function.
- Once we have connection established, **how do we send data?** First we need to create a scheme, the structure of the data that we want to send to the database. To do so, we export the class object, Schema, from mongoose, and we create these schema objects by sending in the data structure as an object. 
```
const movieSchema = new Schema({
  title: {
    type: String,
    required: true
  },
  releaseYear: {
    type: Number,
    default: function () {
      return new Date().getFullYear();
    }
  }, mpaaRating: String,
  cast: [String],
  nowShowing: { type: Boolean, default: false }
}, {
  timestamps: true
});
```
- Once we have a Schema, we turn it into a model. The model is another class which accepts a string variable and a schema object into the constructor. When we create an instance of this model, we send in data that is aligned with structure defined in schema. Then we can use methods in the model object to send data to the database. This instance will send data to the collection with a name the same as the string variable we defined in the model class earlier. 
- After we create the instance we can save it to the database. 
- The model class also has a static method which is used to find documents in the Mongodb in the collection named afted the string passed into the mongoose model.
- All methods called on the instance or the class requires a callback function as they are async functions.
- **Just to re-iterate the steps of saving data to a database:**
  - Create schema for data structure
  - Make schema into a model (A class that stores the methdos to communicate with MongoDB
  - To send data create instance of the model and send in an object with the same structure as the schema so that the class constructor can construct the object.
  - Call the method save and incude a callback function

## CORS
This is a topic that's technically not just server related, the knowledge of CORS is releavent to all web developers, especially when it comes to using APIS or developing APIS. As a result, the topic of CORS, Cross-Origin Resource Sharing, is in this README for backend technologies.

Cross-Origin is in reference to a cross-origin request which is when a domain makes an HTTP request to another domain. For example, http://domain-a.com makes a request to https://domain-b. However, for secruity reasons, all browsers prevent JavaScript from making request cross origins. Obivously, if we could not make any cross-origin request, web applications today would not be near anywhere to what we are used to, modern browsers do allow cross-origin requests as long as cross-origin resource sharing is followed.

Essentially, APIs need to specify which origins (whcich websites) are allowed to make requests, in express, we use the CORS middleware to do this.  If we look at the options for this [CORS Middleware](https://expressjs.com/en/resources/middleware/cors.html) we can see that there different ways we can restrict access. 

## Python
Python is another programming language, and similiar to JavaScript, it is a high-level interpreted programming language and is also used for web development, that being said, Python is used in far more applications.

Unlike JavaScript, Python exists outside of a browser naturally, and we can call the interpreter in our shell and use it as REPL. 

Somethings to note about the Python Language:
 - Cannot declare a variable wihtout setting it
 - Naming convention is to use snake_case
 - Every piece of data in python is an objec, there are no primitive data
 - No undefined or null, just "None". 
 - Uses acutual float types
 - There are no type coercion, you can't just add a string or number together
 - Indentation is important, no curly brackets are used in python, it uses identation to separate code
 - Ternary expressions are wrriten like "This value if<condition> else this value
 - Falsey values in python are: False, None, 0, or empty sequences or collections
 - Logical operators are just the words "and" and "or"
 	- Like JavaScript, "or" returns an operand as follows: "If the first operand is truthy, return it, otherwise return the second operand."
	- "and" returns an operand as follows: "If the first operand is falsey, return it, otherwise return the second operand."
 - Like JavaScript, python has container data types too, such as objects and arrays, but even more!
	- Dictionaries are like JavaScript Objects, however:
		- All strings used as keys need to be quoted
		- Square brackets need to be used
	- List are pythons arrays
	- List comprehensions are ways to create a new list with another list. We assentially write code in square brackets, to iternate over another list, and it will create a new for us, the basic sytanx is as follows: # [<expression> for <item> in <list>] meaning: I want <expression> for each <item> in <list>.
	- List is considered a sequence, and there exists more, such as the tuple which is an immunutable sequence, and a string, a sequence of charts.
	- Sequences (List, tupbles, and strings) can be unpacked meaning we can assigning multiple values innside  the sequence to differnet variables all in one line of code. For example colors = ('red', 'green', 'blue'), r, g, b = colors.
	- Seququences can also be sliced like the slice method, but we use the following syntax: a_sequence[m:n].
 - With python, functions:
	- Cannot edit global variables without using the global keyword and naming the functions inside the function! Offers protection to global variables.
	- Are not hoisted
	- Can not exist without anything inside, hence we use the "pass" statement
	- Cannot be assigned to a variable
	- Uses what are called lamda functions as anonymouse inline functions. These functions are essentially the one lined arrow functions we see in JavaScript, they cannot contain a code block, just one expression. Lamnda functions are written as follow: lambda arguments : expression. 
	- Python uses * to intake multiple arguments similiar to using "...". Python can also accept multiple key-value pair arguments using **.
 - Python has what are called decorater functions which are higher-order functions (accepts and returns an argument)
	- To use a decorater on a function, before we define a function we write @<decorater name> right over top.
	- Decorators essentially add more logic to a function by wrapping it another functions and returning it. We have to be careful when doing this though, as it changes the functions name, and this will confuse things, especially if there's a bug in the function. This is why inside the decorator, another decorator is used to allow a function to maintain its name. 
	-As you can see below, the decorator function, "do_twice", must also have the wrapper function return the function if we want the function to maintain its return value AND the wrapper function must accept parameters if the original function has arguments.
	
```
from functools import wraps

def do_twice(func):
  @wraps(func) # use the wraps decorator from Python's functools module
  def wrapper(*args, **kwargs):
    func(*args, **kwargs)
    return func(*args, **kwargs)

  return wrapper
```

- Python also has classes.
	- To view the attributes or methods of an object or any instance of a class we can use the dir() function.
	- Like classes in JavaScript, python uses a constructor function to create an instance of a class, this function is called __init__. This function should accept all the properties expected when creating an instance of this class but the very first argument must be "self".
	- Unlike JavaScript, python uses 'self' instead of 'this'. All methods inside a class should accept the arguemnt self to provide it context to the instance. 
	- Classes in python can have class members (methods or attributes) which are intended to be accesed/invoked by the class and not the instance. Any class methods need to be passed to the classmethod decorator, and it needs the parameter cls.
	- When extending a class, we just pass a super class into the subclass and then call the __int__ off the super class.
- Python objects also have what are called magic methods which are meant to be overloaded by the coder. These methods have some default codes already and are executed in certain steps of a process, like __init__ 
