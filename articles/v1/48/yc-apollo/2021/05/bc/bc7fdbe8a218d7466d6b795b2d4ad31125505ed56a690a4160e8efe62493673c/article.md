---
schema_version: "1.0.0"
document_id: "bc7fdbe8a218d7466d6b795b2d4ad31125505ed56a690a4160e8efe62493673c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/complete-api-guide"
published_at: "2021-05-11T14:10:54+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:73c695a1651f82d9a854e9248e3115ce86ab1fcb048c9e0ecb9be686c28b2d0a"
---

# Using GraphQL with Python – A Complete Guide

*We recently released an updated Python tutorial post using Strawberry* *and GraphOS* . *You can check it out[here](https://www.apollographql.com/blog/backend/federation/add-python-to-your-graphql-api-with-graphos-and-strawberry-graphql?referrer=python-strawberry-subgraph) !*


Known for its ease of use and simplicity, Python is one of the most beloved general-purpose programming languages. And GraphQL, a declarative query language for APIs and server runtimes, pairs quite nicely with Python. Unfortunately, there are very few comprehensive learning materials out there that give you a step-by-step breakdown of how to use GraphQL with Python. This article will go over everything you need to know to get up and running with GraphQL API using Python, Flask, and Ariadne.


You can find the complete code for this post[on GitHub](https://github.com/Shadid12/flask-graphql) .


## Learning objectives


By the end of the article, you should know how to:


- Set up a Python web server with Flask
- Use the Ariadne library to implement GraphQL
- Compose a GraphQL Schema
- Perform queries and mutations against a Python GraphQL API


**GraphQL vs REST: What problem does GraphQL solve?** If you are completely new to GraphQL and want to know how it differs from a traditional REST API, I recommend reading “[What is GraphQL? GraphQL introduction](https://www.apollographql.com/blog/what-is-graphql-graphql-introduction/) “.


## Setting up GraphQL with Python (Flask)


​​Let’s dive into creating our very own GraphQL API with Python. For this demo, we will be using the Flask​ web server. If you are more accustomed to other frameworks such as Django,​ you can adapt this codebase to your framework. The basic concepts of GraphQL and Python are more or less the same across various frameworks.


### **Creating a new python virtual environment**


​​First of all, let’s create a new project and change the directory to the project folder.


```text
mkdir graphql-python-api
cd graphql-python-api
```


In Python, best practices are to use a *virtual environment* . We can create a new virtual environment by running the following command.


```text
python3 -m venv myapp
```


Next, we have to activate the virtual environment. If you are on a Linux or Mac machine you can run the` source` command with the path of the activate script like shown below.


```text
source myapp/bin/activate
```


And if you’re on a windows machine, you can run the following command to activate the virtual environment.


```text
myapp/bin/activate.bat
```


### Installing dependencies


Our application relies on the following dependencies:


- Flask — this is the web server that we’ll use
- Flask-SQLAlchemy — an ORM that makes it easier for us to communicate with our SQL database
- Ariadne — a library for GraphQL python integration
- Flask-Cors — an extension for Cross Origin Resource Sharing


You can install them all using a single command:


```text
pip install flask ariadne flask-sqlalchemy flask-cors
```


## Up and running with a simple Flask app


We will make the following directory structure. The first file we’ll start working with is the` api/__init__.py` file, which will hold all the API-related configuration code.


For now, let’s populate the` api/__init__.py` with the following code.


```text
from flask import Flask
from flask_cors import CORS


app = Flask(__name__)
CORS(app)


@app.route('/')
def hello():
return 'My First API !!'
```


Our` app.py` file is what’s responsible for actually starting the flask app. Let’s import the flask API instance using the following code:


```text
from api import app
```


Next, we tell Flask to start the application by looking at our` app.py` file. In the command line, we can accomplish this by setting the` FLASK_APP` environment variable.


```text
export FLASK_APP=app.py
```


Finally, we run the app by running the` flask run` command.


Great! We can see our Flask app up and running. Before we enable it to use GraphQL, lets hook up a database and define some tables.


### Adding a database


For this example, we are going to be using a Postgres DB instance. I like[ElephantSQL](https://www.elephantsql.com/) , a hosted SQL database, but you can use any SQL database you like.


In ElephantSQL, once the instance is provisioned on the cloud, we can see the database server information. If you’re using ElephantSQL, copy the DB URL as shown below, otherwise, copy the URL to where your SQL database is – whether it’s running locally on your machine or with another hosted SQL database service.


We can now add this database url to the __ **init__** .py as shown below.


```text


from flask import Flask
from flask_cors import CORS
from flask_sqlalchemy import SQLAlchemy


app = Flask(__name__)
CORS(app)


app.config["SQLALCHEMY_DATABASE_URI"] = "postgres://mycreds.db.elephantsql.com:5432/ngimluxm"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False
db = SQLAlchemy(app)


@app.route('/')
def hello():
return 'My First API !!'
```


Restart the server and make sure everything is working as usual.


### Creating a model


Next, let’s create our first model.


In our database, we are going to have a` Post` table. A` Post` will have a unique` id` , a` title` ,` description` , and the` date` it was created.


Create an` api/models.py` file and a new class called` Post` as shown below.


```text
from app import db


class Post(db.Model):
id = db.Column(db.Integer, primary_key=True)
title = db.Column(db.String)
description = db.Column(db.String)
created_at = db.Column(db.Date)
def to_dict(self):
return {
"id": self.id,
"title": self.title,
"description": self.description,
"created_at": str(self.created_at.strftime('%d-%m-%Y'))
}
```


We can update our` app.py` file to include the current models and database settings.


```text
from api import app, db
from api import models
```


At this point, we can use the Python interactive terminal to create our table and add some records to it. Let’s do that.


First, let’s open the Python terminal by running the following command.


```text
python
```


Once inside the Python terminal, run the following command to create our table.


```text
>>> from app import db
>>> db.create_all()
```


On the first line, we import the database instance and on the second, we run the` create_all()` method to create related tables based on the model we specified earlier.


Troubleshoot: If you are using Mac for development, you might run into an issue where python can not find` psycopg` . To resolve this, run` pip install psycopg2-binary` within your virtual environment.


To verify whether the table got created or not, hop into the` psql` database console and run the following` SQL` query to get the name of all available tables.


```text
SELECT table_name
FROM information_schema.tables
WHERE table_schema='public'
AND table_type='BASE TABLE';
```


Let’s add a few posts to the` Post` table directly from Python command prompt.


```text
>>> from datetime import datetime
>>> from api.models import Post
>>> current_date = datetime.today().date()
>>> new_post = Post(title="A new morning", description="A new morning details", created_at=current_date)
>>> db.session.add(new_post)
>>> db.session.commit()
```


With a working web API connected to the database, we’re ready to integrate GraphQL into the server.


## Writing the GraphQL Schema


A schema in GraphQL describes the shape of our data graph. It is the core of any GraphQL server implementation. It defines the functionality available to the client applications that consumes the API. GraphQL has its own language (GraphQL Schema Definition Language) that is used to write the schema. The schema determines what resources the clients can query and update.


[Learn more about GraphQL schemas in the “Schema basics” section from the docs.](https://www.apollographql.com/docs/apollo-server/schema/schema/)


Let’s go ahead and create a new file called` schema.graphql` in our root directory. Copy and paste the following code in the file.


```text
schema {
query: Query
}


type Post {
id: ID!
title: String!
description: String!
created_at: String!
}


type PostResult {
success: Boolean!
errors: [String]
post: Post
}


type PostsResult {
success: Boolean!
errors: [String]
post: [Post]
}


type Query {
listPosts: PostsResult!
getPost(id: ID!): PostResult!
}
```


First of all, we have a schema type defined in the top. This determines what type of operations clients can perform. For now, clients can only perform` Query` operations.


Next, observe the` Post` type. You will notice that the structure of the` Post` type is identical to our` Post` model that we defined earlier. The` PostsResult` type defines the structure of the response object when we query for all the posts in the database.


Similarly,` PostResult` represents the response when we query for one post in the database.


Finally, we have the type` Query.` This type defines the query operations that our clients can perform. Currently, we have two queries: a` listPosts` query to grab all the posts from the database, and a` getPost` query to get a particular post by its` id` .


## Wiring up Flask server and GraphQL with Ariadne library


Thus far, we have our Flask server up and running, we connected to a database, and we’ve created our first GraphQL schema. Next, we need to wire up our server with GraphQL, so that we can start using the queries/mutations defined in the schema. We will be using the[Ariadne](https://ariadnegraphql.org/) library to do this.


Ariadne is a lightweight Python library that lets you get up and running with GraphQL quickly. Ariadne is framework agnostic (which means you can use it with Flask, Django, or any other framework of your choice) and it uses a ***schema first*** approach to GraphQL API development. In this approach, we define our schema first (as we did for this demo app) and write the business logic based on our schema.


Another popular pattern is to use a ***code first approach*** while designing GraphQL APIs ([Graphene](https://graphene-python.org/) is a popular library that does this). If you’re interested in learning more about this approach, I recommend you give[this article](https://blog.logrocket.com/code-first-vs-schema-first-development-graphql/) a read.


Let’s go and make the following changes in our` app.py` file.


```text
from api import app, db
from ariadne import load_schema_from_path, make_executable_schema, \
graphql_sync, snake_case_fallback_resolvers, ObjectType
from ariadne.constants import PLAYGROUND_HTML
from flask import request, jsonify


type_defs = load_schema_from_path("schema.graphql")
schema = make_executable_schema(
type_defs, snake_case_fallback_resolvers
)


@app.route("/graphql", methods=["GET"])
def graphql_playground():
return PLAYGROUND_HTML, 200


@app.route("/graphql", methods=["POST"])
def graphql_server():
data = request.get_json()
success, result = graphql_sync(
schema,
data,
context_value=request,
debug=app.debug
)
status_code = 200 if success else 400
return jsonify(result), status_code
```


On lines 2 ~ 4, we import a couple functions from the Ariadne library. On line 7, we import the types from our GraphQL schema. Then, we call the` make_executable_schema` method from Ariadne. We pass the type definitions as the first argument. The second argument` snake_case_fallback_resolvers` is a[Bindable](https://ariadnegraphql.org/docs/0.4.0/bindables) ; these are special types from Ariadne library that is used to bind python methods to GraphQL schema.


[Learn more about Bindables](https://ariadnegraphql.org/docs/0.4.0/bindables) .


Next, we have two methods. The first method will load up the GraphQL user interface for us. The second method is a` POST` method. This endpoint is will be used by our clients to run queries and mutations.


## Testing our server


We can run the application by running` flask run` .


```text
flask run
```


Next, we need a GraphQL IDE to build our queries, explore the schema, and test the API functionality. The[Apollo Explorer](https://www.apollographql.com/docs/studio/explorer/) is a ***free to use*** GraphQL IDE built specifically for GraphQL developers working on GraphQL APIs. It comes with a lot of powerful features like one-click query building, intelligent search, and a multitude of other productivity features.


To get started, head over to[studio.apollographql.com/dev](http://studio.apollographql.com/dev?referrer=blog) and create an account (using either GitHub or your email). Choose a name for our graph, and select the` development` option as the graph type.


We will add our localhost endpoint` http://localhost:5000/graphql` in the endpoint field and click create graph.


Once the setup is done, we will see that the GraphQL IDE will load up in our browser.


### **Query all posts**


We are still not able to run queries. Let’s change that. We will write our first query resolver that will return all the posts in the database.


### Writing our first Resolver


We can create a new file called` api/queries.py` and write the following resolver method as shown below.


```text
from .models import Post
def listPosts_resolver(obj, info):
try:
posts = [post.to_dict() for post in Post.query.all()]
print(posts)
payload = {
"success": True,
"posts": posts
}
except Exception as error:
payload = {
"success": False,
"errors": [str(error)]
}
return payload
```


This resolver method is very self explanatory. We are trying to query all the Posts from the database and return them as a Payload dictionary. We have to reference this resolver in our` app.py` file. Let’s make the following changes to` app.py` file.


```text
from api import app, db
from ariadne import load_schema_from_path, make_executable_schema, \
graphql_sync, snake_case_fallback_resolvers, ObjectType
from ariadne.constants import PLAYGROUND_HTML
from flask import request, jsonify
from api.queries import listPosts_resolver


query = ObjectType("Query")
query.set_field("listPosts", listPosts_resolver)


type_defs = load_schema_from_path("schema.graphql")
schema = make_executable_schema(
type_defs, query, snake_case_fallback_resolvers
)
@app.route("/graphql", methods=["GET"])
def graphql_playground():
return PLAYGROUND_HTML, 200


@app.route("/graphql", methods=["POST"])
def graphql_server():
data = request.get_json()
success, result = graphql_sync(
schema,
data,
context_value=request,
debug=app.debug
)
status_code = 200 if success else 400
return jsonify(result), status_code
```


On line 6, we are importing the resolver. We are then creating a query instance and specifying the query field and the corresponding resolver (line 9). Finally, we are adding the query instance to the make_executable_schema method call as a parameter. Restart the server, go back to the GraphQL playground and you will be able to run the following query.


```text
query AllPosts {
listPosts {
success
errors
posts {
id
title
description
created_at
}
}
}
```


### Querying a single post by id


Next, we will take a look at how we can query a single item by a property. For this example, we will query a` Post` by its id.


We will create a new resolver method inside our` queries.py` file.


```text
from ariadne import convert_kwargs_to_snake_case
...


@convert_kwargs_to_snake_case
def getPost_resolver(obj, info, id):
try:
post = Post.query.get(id)
payload = {
"success": True,
"post": post.to_dict()
}
except AttributeError:  # todo not found
payload = {
"success": False,
"errors": ["Post item matching {id} not found"]
}
return payload
```


We imported a decorator called` convert_kwargs_to_snake_case` from Ariadne. This decorator converts the method arguments from camel case to snake case. Let’s update the` app.py` file to include the latest resolver


```text
...
from api.queries import listPosts_resolver, getPost_resolver
query = ObjectType("Query")
query.set_field("listPosts", listPosts_resolver)
query.set_field("getPost", getPost_resolver)
...
```


We can run the query and verify if everything is working as expected.


```text
query GetPost {
getPost(id: "1") {
post {
id
title
description
}
success
errors
}
}
```


## Mutation


Mutations are used to create, update or delete records from the database. Let’s set up our first mutation.


### Creating a new post


First of all, in our schema, we need to define the type of mutation we are trying to add. In our case, we want to create a new post. Therefore, we will make a mutation called createPost.


```text
// schema.graphql
schema {
query: Query
mutation: Mutation
}


type Mutation {
createPost(title: String!, description: String!, created_at: String): PostResult!
}
...
```


We updated our schema.graphql file accordingly as shown above. We add a new **Mutation** type. We specify the mutation name, required parameters and finally update schema type to include Mutation type. Updating the schema itself will not do much. We need a resolver to correspond to the` createPost` mutation in the schema.


We will create a new file called` api/mutations.py` . All our mutation resolvers will live in this file.


```text


# mutations.py
from datetime import date
from ariadne import convert_kwargs_to_snake_case
from api import db
from api.models import Post


@convert_kwargs_to_snake_case
def create_post_resolver(obj, info, title, description):
try:
today = date.today()
post = Post(
title=title, description=description, created_at=today.strftime("%b-%d-%Y")
)
db.session.add(post)
db.session.commit()
payload = {
"success": True,
"post": post.to_dict()
}
except ValueError:  # date format errors
payload = {
"success": False,
"errors": [f"Incorrect date format provided. Date should be in "
f"the format dd-mm-yyyy"]
}
return payload
```


The resolver method is pretty self-explanatory. We are here trying to create and save a new instance of a` Post` . On success, we return the post. We also need to bind this new mutation resolver in our app.py.


```text
...
from api.queries import listPosts_resolver, getPost_resolver
from api.mutations import create_post_resolver
query = ObjectType("Query")
mutation = ObjectType("Mutation")
query.set_field("listPosts", listPosts_resolver)
query.set_field("getPost", getPost_resolver)
mutation.set_field("createPost", create_post_resolver)


type_defs = load_schema_from_path("schema.graphql")
schema = make_executable_schema(
type_defs, query, mutation, snake_case_fallback_resolvers
)
..
```


As you can see from the code example above, importing and binding the mutation follows the same pattern as importing and binding queries that we have done previously. we can now hop into the GraphQL playground and try to execute this new mutation.


```text
mutation CreateNewPost {
createPost(
title: "New Blog Post",
description:"Some Description") {
post {
id
title
description
created_at
}
success
errors
}
}
```


### Updating a post


Next, we will be looking at updating a post. To do so we will follow the same pattern. First, we will update the schema and add a new mutation called` updatePost` .


```text
type Mutation {
updatePost(id: ID!, title: String, description: String): PostResult!
}
```


` updatePost` takes in a mandatory parameter id and optional parameters title and description. Now we can create a resolver for this mutation.


```text
# mutations.py
...
@convert_kwargs_to_snake_case
def update_post_resolver(obj, info, id, title, description):
try:
post = Post.query.get(id)
if post:
post.title = title
post.description = description
db.session.add(post)
db.session.commit()
payload = {
"success": True,
"post": post.to_dict()
}
except AttributeError:  # todo not found
payload = {
"success": False,
"errors": ["item matching id {id} not found"]
}
return payload
```


In this method, we are querying the post by id and updating the title and description of the post. We can wire this new resolver up in` app.py` like the previous one.


```text
...
from api.mutations import create_post_resolver, update_post_resolver


mutation = ObjectType("Mutation")


mutation.set_field("createPost", create_post_resolver)
mutation.set_field("updatePost", update_post_resolver)
```


That’s it. We can restart the server and run the` updatePost` mutation now.


```text
mutation UpdatePost {
updatePost(id:"2", title:"Hello title", description:"updated description") {
post {
id
title
description
}
success
errors
}
}
```


### Deleting a post


Finally, let’s take a look how we can delete a post. We are going to exactly the same thing as we did with updatePost mutation. We will first create the deletePost mutation in the schema.


```text
type Mutation {
updatePost(id: ID!, title: String, description: String): PostResult!
deletePost(id: ID): PostResult!
}
```


Once that is done we can create a new resolver for it and reference it in the app.py file.


```text
# mutations.py
...
@convert_kwargs_to_snake_case
def delete_post_resolver(obj, info, id):
try:
post = Post.query.get(id)
db.session.delete(post)
db.session.commit()
payload = {"success": True, "post": post.to_dict()}
except AttributeError:
payload = {
"success": False,
"errors": ["Not found"]
}
return payload


```


```text
# app.py
...
from api.mutations import create_post_resolver, update_post_resolver, delete_post_resolver
...
mutation.set_field("deletePost", delete_post_resolver)
```


Let’s test the functionality.


Awesome, we now have our GraphQL and Python API up and running.


## Final thoughts


The main intention of this article was to get you up and running with GraphQL and Python, as well as introduce some widely used patterns and best practices. I hope you found this article informative.


This is just the start. I suggest checking out some of the other posts on the Apollo blog on topics like caching,[GraphQL security](https://www.apollographql.com/blog/why-you-should-disable-graphql-introspection-in-production-graphql-security/) , and if you’re really into Python, checking out the rest of the[Ariadne documentation](https://ariadnegraphql.org/docs/intro) .


That’s a wrap! Happy hacking and see you next time.


Written by


Shadid Haque


[Read more by Shadid Haque](https://www.apollographql.com/blog/author/shadidhaque)
