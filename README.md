# Notes on Back-End Technologies
My notes on back-end technologies. The following topics are covered:
 - [Apache](#apache)
 - [Node.JS](#node.js)
 - [ExpressJS](#expressjs)

## Apache
Apache is a software that serves as a web server to deliver or serve websites on the internet. Now remember, it is a web server, not a physical server, however it does runs on the physical server to act as the middle man between a client (browser) and a server. That is, a client will request a file from the server, the request will first go to the apache server to process and it will pull what it needs from the server to and send it back to the client.

As a web server software, Apache can have modules added to increase its functionality. This includes a module for server side languages such as PHP, Perl or Lua to send files to a client that is modified based on certain settings/parameters sent to the server. So, essentially, what happens when you request a .php file from the apache server is that the apache server will access the php interpreter from the php module and run the php file so that it creates an HTML file to serve.

To configure Apache settings, there is a text file you need open up and edit. You need to configure this file for the domain name, module use, etc.

Nginx is another web server simiiar to Apache, however it was made with more heavy traffic in mind.

See https://www.hostinger.com/tutorials/what-is-apache or https://kinsta.com/knowledgebase/what-is-apache/ or https://www.quora.com/How-does-PHP-work-with-Apachefor more details 

## Node.js
### What is Node.js and why do we use it?
Node.js is a JavaScript Runtime Environment based off the same JavaScript Engine that Google Chrome uses (V8), that allows us to run JavaScript outside of the browser and instead run it directly on a computer or server OS. Ultimately, the benefit of this is that we can use Node.js to then create a web server on a computer or server, and then allow it to handle HTTP requests. So, when there is a HTTP request for a certain file, developers will use Node.js to grab the right file and serve it to a client. Additionally, Node.js can also be used to modify the file (HTML) before serving it, essentially it serves the same role as PHP and Apache in a LAMP.XAMP,MAMP stack where PHP dynamically modifies a webpage and Apache handles the HTTP requests. 

### Node Modules 
Node.js also has the the Node Pack Manager (NPM) which allows us to download what are considered node modules which are used for many different purpose of the web development process, from developing the material for webpages to actually hosting the web server.

All node modules are just a single or a series of JavaScript files. To use these modules, we need to "require" them into a variable. To require them in we need to use the path of file or, if we installed it from npm we just need the name. **It's important to note that any variables initialized in these modules are only local to that module, they do not pollute the global scope**

It's important to note that when we "require" a module in, it's not like we are just calling that file and running it. This [link](https://www.freecodecamp.org/news/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8/) here does a great job of explaining it but essentially, when we use "require" we wrap the module in a function, making the variables local to that module, since all variables are function scoped. 

For us to use any of the functions or data from this module, the JavaSript file must have a module.exports object that contains all the functions/data. When we wrap the module in a function, the function returns this module.export object. **It technically doesn't have to be an object, it can just be one function, or an array, or string, etc. but an object allows us to export more**

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

- controller folder \- Now this doesn't exist naturally in a new express app but it does seem to be good practice. The controller folder will hold modules that have functions tell us how to interact with data from the models folder based on the HTTP request. 

Notice how in almost all of these modules, anytime ".use" is used and a function is passed in, there are always three parameters, **a req, res and next**. What's happening is that when a HTTP request gets to this ".use" it will actually pass in the HTTP request, response, and the next function that is to be called out if we don't send back a response. This allows the function to interact with the HTTP and ultimately send back a HTML response as well.
