Source : https://www.playframework.com/documentation/3.0.x/Anatomy

### Application Layout

This is the typical layout of a PLAY application. You will find some similarities with SpryPoint's SpryEngage and SpryCIS source code.

app                      → Application sources
 └ assets                → Compiled asset sources
    └ stylesheets        → Typically LESS CSS sources
    └ javascripts        → Typically CoffeeScript sources
 └ controllers           → Application controllers
 └ models                → Application business layer
 └ views                 → Templates
build.sbt                → Application build script
conf                     → Configurations files and other non-compiled resources (on classpath)
 └ application.conf      → Main configuration file
 └ routes                → Routes definition
dist                     → Arbitrary files to be included in your projects distribution
public                   → Public assets
 └ stylesheets           → CSS files
 └ javascripts           → Javascript files
 └ images                → Image files
project                  → sbt configuration files
 └ build.properties      → Marker for sbt project
 └ plugins.sbt           → sbt plugins including the declaration for Play itself
lib                      → Unmanaged libraries dependencies
logs                     → Logs folder
 └ application.log       → Default log file
target                   → Generated stuff
 └ resolution-cache      → Info about dependencies
 └ scala-2.13
    └ api                → Generated API docs
    └ classes            → Compiled class files
    └ routes             → Sources generated from routes
    └ twirl              → Sources generated from templates
 └ universal             → Application packaging
 └ web                   → Compiled web assets
test


### Explanations / Unknown terms

Terms:
Model View Controller (MVC) : It's a software architectural pattern. Model manages the data, business logic and rules. View handles what the user sees. Controller connects the model and view - handles requests and returns responses. [Resource] : https://www.youtube.com/watch?v=DUg2SWWK18I&pp=ygUVbW9kZWwgdmlldyBjb250cm9sbGVy

Q/A's:

Q: You will find .less files inside the app/assets folder in the SpryEngage repo. What are these? Why should we use these?
A: .less file contains enhanced CSS syntax. It gets compiled into standard .css before it's used in a browser. It provides reusable code via mixins and allows for logic like conditions and loops if needed. 

Q: What are application controllers?
A: Scala / Java classes that handle incoming HTTP requests and return responses. Controllers communicate with Model and View, Model and View never have to communicate within themselves.

Q: What are application business layers / models?
A: Core logic and data management lives here. Example of a business logic would be a function like : isEmailValid() that checks if an entered email address is valid. 

Q: What are templates / views?
A: This is what the user sees. This could be HTML / css or something like that.

Q: What is the conf/routes file?
A: This defines the URL routing table for the web application. It contains fields in the following format:
Method    Path    Controller Method
For example : GET    /hello    controllers.HomeController.sayHello

Now, let's say your controller has this method : 
public Result greet(String name){
    return ok("Hello, " + name);
}

You can use the route:
GET    /greet/:name    controllers.HomeController.greet(name)

So a request to /greet/Alex will return Hello, Alex

You can also return a view! Let's say you have a file named hello.scala.html, then in the controllers, you can say:
public Result hello(String name){
    return ok(views.html.hello.render());
}

In routes, you will have:
GET    /hello    controllers.HomeController.hello(name)

And in your hello.scala.html, you will have:
@(name: String)
@main("Hello"){ // I will explain what this main does later!
<h1> Hello, @name </h1>
}

Now, you can do : localhost:9000/hello?name=Adithya and you should get a heading saying "Hello Adithya"

Q: What are templates?
A:
