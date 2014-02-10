# Getting Started with Node

For the [Node-js-Denver-Boulder Meetup](http://www.meetup.com/Node-js-Denver-Boulder/) <3 

## Part 1

### Node Setup

1. Download [Node](http://nodejs.org/download/) for your specific platform. *This also installs [NPM](https://npmjs.org/)*. More on this later.
2. Create a new folder. Within that folder create a file called *app.js*. and add the following code to the file:
  ```javascript
  // load http module
  var http = require('http');

  // configure http server
  http.createServer(function (request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end('Hello, World!\n');
  }).listen(1137, "127.0.0.1");

  // inform user what is happening
  console.log('Server running at http://127.0.0.1:1137/');
  ```

  Save the file.

3. Navigate to the folder in your terminal and fire up the server:
  ```sh
  $ node app.js
  ```

4. Point your browser to [http://localhost:1137/](http://localhost:1137/).

#### What's going on?

- `var http = require('http')` uses the HTTP server to process requests and send subsequent responses.
- `http.createServer` creates the web server object.
- `function (request, response) {}` handles requests and serves responses.
- `response.writeHead(200, {'Content-Type': 'text/plain'})` sends a response header in the form of a status code along with the exact header message.
- `response.end('Hello, World!\n')` tells the server that all response headers as well as the body have been sent.
- `.listen(1137, "127.0.0.1")` accepts connections on port 1137 on URL [http://127.0.0.1](http://127.0.0.1) (or [http://localhost](http://localhost)).

Please consult the Node API [documentation](http://nodejs.org/api/) for more info/further explanation.

### Extended Example

1. Open *app.js* and save it as *app2.js*, then add the following code:
  ```javascript
  // load http module and use fs to access the file system
  var http = require('http'),
      fs = require('fs');

  // configure http server
  http.createServer(function (request, response) {
    
    // what's going on here?
    fs.readFile('data.txt', function readData(err, data) {
      response.writeHead(200, {'Content-Type': 'text/plain'});
      response.end(data);
    })
    
  }).listen(1137, "127.0.0.1");

  // inform user what is happening
  console.log('Server running at http://127.0.0.1:1137/');
  ```

3. Save and run.

3. Go over this line by line. See if you can figure out what's going on? Need help? Consult the Node [documentation](http://nodejs.org/api/) and/or use the "Google-it-first" algorithm. 

**Next time we'll add [Express](http://expressjs.com/) into the mix!**

## Part 2

(work in progress)

As promised, let's add Express, which is a lightweight framework for Node. Express can also be used as a command line tool to set up a project structure/boilerplate for use with, well, the Express framework.

**We'll be creating an entirely new app for this tutorial.**

Start by installing Express globally:

```sh
$ npm install -g express
```

### Project Setup

1. Use the Express command line tool to create our project structure:
  ```sh
  $ express <new_folder>
  ```

  This creates a new directory with a basic project structure.

2. Before we can beginning developing, navigate into the folder then run the following command to load the Express dependencies from the *package.json* file:
  ```sh
  $ npm install
  ```

  > Please note: The dependencies within *package.json* are generally listed by name and version. In some cases instead of a version, you'll see an `*`, whicg means that npm will retrieve the latest version of the dependency. 

  Your project structure should now look like this:
  ```sh
  ├── app.js
  ├── package.json
  ├── public
  │   ├── images
  │   ├── javascripts
  │   └── stylesheets
  │       └── style.css
  ├── routes
  │   ├── index.js
  │   └── user.js
  └── views
      ├── index.jade
      └── layout.jade
  ```

  #### What's going on?
    - *app.js* includes your app configuration, middleware, and routing.
    - *package.json* holds you app's dependencies configs.
    - The "public" folder contains images, Javascript files, and stylesheets.
    - The file in your "routes" folder define the app's business logic.
    - "views" contain views, templates, and partials.

3. Test out your app to ensure everything is installed:
  ```sh
  $ node app
  ```

  Point your browser to [http://localhost:3000/](http://localhost:3000/), and you should see:

  ![express](https://raw.github.com/mjhea0/node-express-ajax-craigslist/master/img/welcome.png)

  Congrats! You just set up Express. Now, we just need to customize it. In this case we are going to build a form for submitting random strings then displaying them beneath the form upon submission.

### Server Side

1.  Update *app.js* with the following code:

  ```javascript
  // load dependencies
  var express = require('express'),
      routes = require('./routes'),
      http = require('http'),
      path = require('path');

  var app = express();

  // config - all environments
  app.set('port', process.env.PORT || 3000);
  app.set('views', path.join(__dirname, 'views'));
  app.set('view engine', 'jade');
  app.use(express.favicon());
  app.use(express.logger('dev'));
  app.use(express.json());
  app.use(express.urlencoded());
  app.use(express.methodOverride());
  app.use(app.router);
  app.use(express.static(path.join(__dirname, 'public')));

  // config - development only
  if ('development' == app.get('env')) {
    app.use(express.errorHandler());
  }

  // routes
  app.get('/', routes.index);

  // configure http server
  http.createServer(app).listen(app.get('port'), function(){
    console.log('Express server listening on port ' + app.get('port'));
  });
  ```
#### What's going on?

1. First, we load our module dependencies. The app variable is the actual Express server.
2. In the second section, `app.set()` is used to tell Express that we want to use Jade templates and where to find
 our "views" folder. Meanwhile `app.use()` functinons are for middlewares, which you can read more about [here](http://expressjs.com/api.html#middleware).
3. Next, we have routes. The actual endpoint, or path, is defined here as well as the specific HTTP method. The actual callback is handled within the "routes" folder in the *index.js* file. 
4. Finally, we configure the HTTP server like in Part 1.

2. First route
3. Views
4. Form

### Client Side

5. Javascript
6. styles

### Server Side

1. Second route


Boom!




A route consists of a path (string or regexp), callback function, and HTTP method. Our hello world example calls app.get() which represents the HTTP GET method, with the path “/”, representing our “root” page, followed by the callback function.

.....
