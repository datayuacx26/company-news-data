---
schema_version: "1.0.0"
document_id: "378aa732bfdd1d196ff37e675e98664ffc3e60e725bc69f4ca353177b6b30c1d"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-development-with-docker-and-docker-compose/"
published_at: "2021-04-08T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:df0176359679c6371867193b342516e7f5894ad36608b1540c6f6baf5c6a00d3"
---

# Sails development with Docker and Docker Compose

## Whats and whys: Docker & Docker Compose


The goal of this article is to make setting up a Docker workflow for Sails as simple as possible. Docker makes it easy to set up and run a development environment and also simplifies your deployment workflow.


First, we’ll get into how Docker and Docker Compose work and its benefits. Then we will take a step by step guide to make our beloved Sails application run inside a Docker container. Let’s get started!


## Containerization


Before talking about Docker and Sails we need to understand what is containerization and why we want it.


Containerization is a form of virtualization, through which applications are run in isolated user spaces called containers. We can think of a container as a lightweight virtual machine (but to be clear, it is not).


## But, why do we want it?


With containerization, everything an application needs to run, let’s say, its binaries, libraries, configuration files and dependencies, are encapsulated and isolated in its container.


This makes it easier to set up and run a development environment, and also simplifies the deployment by reducing the chances of missing or different versions of libraries, files or dependencies errors.


The container itself is abstracted away from the host OS, with only limited access to underlying resources. Isolation has its advantages, for example portability, stability and security.


So, standard, lightweight, secure… **yes as developers we want containerization!**


## Docker and Docker Compose


The word “Docker” refers to several things including an open source project, tools and a company (Docker Inc., the company that primarily supports the project and tools).


Talking about the tools, Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Container orchestration automates the deployment, management, scaling, and networking of containers.


It can help you deploy the same application across different environments without needing to redesign it. As an example, microservices in containers make it easy to orchestrate services, including storage, networking, and security. Docker Compose is an orchestration tool for Docker containers. \[2\]


## Installing Docker


Mac:[https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install/)


Windows:[https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/)


Ubuntu:[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)


Others:[https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)


## Sails containerization


Now, how do we make Sails, our beloved Node framework, run inside a container?


## Sails CLI image


Our container needs an image to run. Let’s create one. To do so, create docker-sails-cli directory and a file named Dockerfile inside it with this content:


```text
FROM   node:lts
RUN   npm install -g  [email protected]
```


P.S: As of the time of this article, v1.4.2 was the latest release of Sails. You can always update your own image to use the latest version of Sails if it has changed when you are reading this article.


The **FROM** statement declares we are using node latest lts version as our base image. The syntax used is` imageName:tag` , if no tag is specified, the default is used. \[3\]


At the same time Node lts image is based on Debian Stretch. So this is our project environment: Sails at version 1.4.2, Node at versioin 14.15.4 (as of the time of writing this article) running on a Debian Stretch.


Now we need to build our image.


```text
docker   build   .   -t   sails-cli:1.4.2
```


Simple, right?


Let’s see what we did.


```text
docker   build   [context] -t [tag]
```


-


**context** : The context where we are building. As we are in the same directory as the Dockerfile we use` .` . And also, there’s no need to specify the file name as Dockerfile is the default one.


-


**tag** : It is optional, but we want to tag the image we just built as we are probably going to have many different images with different versions for different projects.


## Creating a new Sails project


Once we have our Sails CLI image built we are going to use it to create a new Sails project.


```text
docker   run   --rm   -v   `  pwd  `  :/app   -w   /app   sails-cli:1.4.2   sails   new   sails-docker-example   --no-frontend
```


- **docker run** : Create and run a new container
- **—rm** : Remove container after process finishes as we don’t need the container after that
- **-v** : Mount current working directory (in our host computer) in /app path inside container. As a result we are sharing current directory with the container
- **-w** : Set container current working directory to /app
- **sails-cli:1.4.2** : Our previously built Sails image
- **sails new sails-docker-example —no-frontend** : Sails CLI command we want to run in the container.


As a result, after running` sails new` in the container, we are going to find a new directory created with our new Sails project. Now, let’s run our project:


```text
cd   sails-docker-example
docker   run   --rm   -it   -v   `  pwd  `  :/app   -w   /app   -p   1337:1337   sails-cli:1.4.2   sails   lift
```


- **-i** : Keep STDIN open even if not attached
- **-t** : Allocate a pseudo-TTY
- **-p** : Maps container 1337 to host computer so we can reach our app through localhost:1337


It’s alive!


## Running and debugging


As you may notice, although our project is running, if you modify a file you will need to stop the container and launch it again to get the changes applied.


Nodemon to the rescue!


```text
docker   run   --rm   -v   `  pwd  `  :/app   -w   /app   sails-cli:1.4.2   npm   install   nodemon   --save-dev
```


```text
"scripts"  : {
"debug"  :   "nodemon --inspect=0.0.0.0 app.js"  ,
"start"  :   "NODE_ENV=production node app.js"  ,
...
```


And run again, but this time instead of sails lift we are going to run with npm:


```text
docker   run   --rm   -v   `  pwd  `  :/app   -w   /app   -p   1337:1337   -p   9229:9229   sails-cli:1.4.2   npm   run   debug
```


Now, modify a file, save it and verify that the project is restarted automatically.


Finally, as we are running inside a container we need a way to get the debugger connected to the process, so we are mapping the 9559 port.


If you use VS Code you can use the following configuration in your launch.json file


```text
{
"version"  :   "0.2.0"  ,
"configurations"  : [
{
"address"  :   "localhost"  ,
"localRoot"  :   "${workspaceFolder}"  ,
"name"  :   "Attach to Remote"  ,
"port"  :   9229  ,
"remoteRoot"  :   "/app"  ,
"request"  :   "attach"  ,
"skipFiles"  : [
"<node_internals>/**"
],
"type"  :   "node"  ,
"restart"  :   true
}
]
}
```


## Docker Compose


As we mentioned before Docker Compose is an orchestration tool. Docker Compose allows us to define our infrastructure, let’s say, services, networks, volumes in a YAML file.


As a first step we will create a very simple file defining our Sails service in it. For this service we will set the image, command, ports mapping, volume mounts, working directory, etc, as we did with Docker command line tool.


Our docker-compose.yml:


```text
version  :   '3'


services  :
api  :
image  :   sails-cli:1.4.2   # same as with docker cli
# Run npm install before starting sails to keep our dependencies installed and updated
command  :   sh -c "npm install && npm run debug"
working_dir  :   /app   # same as -w with docker cli
ports  :   # same as -p with docker cli
-   '1337:1337'
-   '9229:9229'
volumes  :   # same as -v with docker cli
-   .:/app
```


Then, we will use docker-compose command to lift our service.


```text
docker-compose   up
```


The result is the same as running with Docker CLI.


In case we want to remove the created infrastructure, once stopped, run:


```text
docker-compose   down
```


This will remove all the containers, networks, volumes created with` docker-compose up` .


## Database containerization


In most cases we will require a database for our project. In this example we will be using MongoDB but it’s possible to do the same with other db engine supported by Sails \[4\].


For those who don’t know, MongoDB is a free and open-source cross-platform document-oriented database program. \[5\]


Fortunately MongoDB has an official image that we will be using to add a database service to our project.


First, we need to add Sails MongoDB adapter to our project. As we did before, we can add and install a dependency to our project with Docker like this:


```text
docker   run   --rm   -v   `  pwd  `  :/app   -w   /app   sails-cli:1.4.2   npm   install   sails-mongo
```


Then, modify our project config and set mongo as our default adapter.


In the config/datastore.js file:


```text
.
.
.


default  : {
adapter:   'sails-mongo'  ,
url: process.env.  DB_URL
},


```


Next we need to add MongoDB service to our docker-compose.yml and provision Sails service with needed config.


```text
version  :   '3'


services  :
api  :
image  :   sails-cli:1.4.2
command  :   sh -c "npm install && npm run debug"
working_dir  :   /app
ports  :
-   '1337:1337'
-   '9229:9229'
volumes  :
-   .:/app
env_file  :
-   ./sails.env   # set env vars using a file


db  :
container_name  :   mongodb   # set name to the container
image  :   mongo:4.4   # as of this writing, the latest version is 4.4.3
volumes  :
-   ./mongo:/data/db
```


We added a db service to our compose file, and also set an env file to Sails service. We will use that file to config our database connection.


sails.env


```text
DB_URL=mongodb://root:@mongodb:27017/sails
```


Compose allows us to connect to db using the container name as network address, that is why we can use mongodb as the address in our connection url.


Important note: we are not enabling MongoDB authentication for this example but it is a good idea to do so for production environments. \[6\]


Finally, use` docker-compose up` to run your application, you will see MongoDB output along with Sails.


And we are done, a fully Sails development environment running on Docker!


Thank you so much for reading my post,[Tweet](https://twitter.com/vidueirof) at me if you have any questions!


Here is the[link](https://github.com/sailscasts/sails-docker-example) to the demo Sails application on GitHub


**\[1\]**[https://www.docker.com/resources/what-container](https://www.docker.com/resources/what-container)


**\[2\]**[https://www.docker.com/why-docker](https://www.docker.com/why-docker)


**\[3\]**[https://hub.docker.com/_/node](https://hub.docker.com/_/node)


**\[4\]**[Available Sails adapters](https://sailsjs.com/documentation/concepts/extending-sails/adapters/available-adapters)


**\[5\]**[https://www.mongodb.com](https://www.mongodb.com/)


**\[6\]**[https://hub.docker.com/_/mongo](https://hub.docker.com/_/mongo)
