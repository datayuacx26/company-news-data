---
schema_version: "1.0.0"
document_id: "16e9fec5feed9220525151ffd430b447faf70e85c0b651e65b21aeb19f82ab80"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/python-rebase-library/"
published_at: "2018-08-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:5f19a787cff03c29372f6748194eb8f1784f5a602c7632d480945c6d49ac9bfe"
---

# Improving Your Data Layer with Rebase on Python

## Introduction


Technology keeps getting better and better which, at some point, makes us think “Should I migrate to the latest version/technology or not?” Well when you decide to use a better technology for your application, you have to also consider rewriting the code that your application runs on. The business logic remains the same in most of the cases but the data model would definitely change if you are switching from SQL to some NoSQL Technology for example.


At trivago, we use several technologies to store, process and analyze our data. Ensuring the quality and accuracy of content is our topmost priority and adapting data from one structure to another is very challenging.


## Our Challenges


A scenario that we come across very often is moving data from one storage into another on the cloud and providing different read replicas (view of a dataset in a different structure or format) for independent stakeholders. For instance, transporting data from legacy MySQL tables to DynamoDB tables on AWS using Kinesis and Kafka streams. In this scenario, the data undergoes a lot of structural changes when traversing the different technologies before being stored as an object into DynamoDB up to being delivered to stakeholders in Read Replicas.


### Let’s break it down


Yes, we are migrating to the Cloud and these technologies have their own way of storing data. Taking the records from MySQL and transitioning them into different structures is indeed demanding. Working with the cloud is great since you have more than one technology in your stack that you can use to ease your life. One good practice would be to stop thinking about relational data structures since you probably would use more than one technology for your data storage such as some primary NoSQL storage with streaming, caching and other read replicas.


So in the above scenario, the steps are:


- MySQL tables are streamed to Kafka Topics
- Kafka Topics are then sent to Kinesis Streams
- Lambda Function listens to Kinesis Events and performs the gluing and validation to create objects
- Objects are stored in DynamoDB tables
- Another Lambda Function listens to DynamoDB Events and maps object data into different views and stores them in different read replicas.


The main problem with this scenario is when you have to write code that follows the same process but with some minor difference at specific level such as a different validation rule. Firstly it is time-consuming and secondly when you have to modify one little thing (e.g. a validation) you have to do it multiple times.


#### Example


Imagine you have the following MySQL datasets:


##### User


```text
{
"id"  :   123  ,
"name"  :   "John Doe"  ,
"location"  :   "Germany"  ,
"age"  :   32
}
```


##### Item


```text
{
"id"  :   1  ,
"name"  :   "Dark Chocolate 80%"  ,
"price"  :   5.75
}
```


##### Transaction


```text
{
"id"  :   654  ,
"user_id"  :   123  ,
"item_id"  :   1  ,
"amount"  :   5  ,
"date"  :   "2018-06-17"
}
```


In the end, you want to achieve something like this:


##### User


```text
{
"id"  :   123  ,
"name"  :   "John Doe"  ,
"location"  :   "Germany"  ,
"age"  :   32  ,
"transactions"  : [
{
"id"  :   654  ,
"date"  :   "2018-06-17"  ,
"items"  : [
{
"id"  :   1  ,
"name"  :   "Dark Chocolate 80%"  ,
"price"  :   5.75  ,
"amount"  :   5
}
]
}
]
}
```


And if your storage does not support nested structures, maybe something like this as a flattened structure:


##### User


```text
{
"id"  :   123  ,
"name"  :   "John Doe"  ,
"location"  :   "Germany"  ,
"age"  :   32  ,
"transaction_0_id"  :   654  ,
"transaction_0_date"  :   "2018-06-17"  ,
"transaction_0_item_0_id"  :   1  ,
"transaction_0_item_0_name"  :   "Dark Chocolate 80%"  ,
"transaction_0_item_0_price"  :   5.75  ,
"transaction_0_item_0_amount"  :   5
}
```


The different mappings to different views require different logic and thus take time to implement. Renaming an entity would require the modification of several lines of code. For example, renaming` item` to` product` .


Also depending on the context or scenario, you might want to add layers of validation or project only some attributes to the view. There is more to it on how you can manipulate your data and get different views, but this is just an overview.


## What is rebase, and how will it help?


Well, to try to solve the above challenges and minimize workload while increasing productivity, I wrote a set of reusable components that form up rebase. Python, like any other programming and scripting language, offers us the basics that we use to build applications on. It’s not just magic that automatically creates your application by the click of a button.


But while working with Python, there were some constraints or lack of features that we needed in our daily work. It provides the core but we always want more. Thus I wrote rebase by extending the base Python` object` to add more features and functionalities so that it can provide what we’re missing.


In short, rebase is a little library that consists of some dynamic components to improve your application focusing mostly on the data layer.


### How is rebase implemented?


The base object that rebase provide has most of the magic functions (e.g.` __get__` ,` __set__` ,` __debug__` etc) implemented in a better way to provide more functionalities.


The base object supports a basic way of applying “types” to instance variables which also acts as a first layer of validation. The base object can be used by just creating an instance and passing arguments to the constructor and any arguments passed would become a property of the object.


But to make the most of the features, the object can be extended and override the methods such as` properties()` to specifically assign properties to the class.


### Improving your data layer


Rebase provides a base model class that is specifically made to represent a database entity. It consists of` scenarios` and` rules` that are used to validate and project attributes. You can have a model with different rules applied to specific scenarios.


#### Example


In this example, we’ll take a basic user profile registration.


```text
from   rebase.core   import   Model


from   myapp   import   NameValidator, EmailValidator, PasswordValidator


class   User  (  Model  ):
def   properties  (self):
return   {
'name'  : (  'name'  ,   str  ),
'email'  : (  'email'  ,   str  ),
'password'  : (  'password'  ,   str  ),
}


def   scenarios  (self):
return   {
'create'  : [  'name'  ,   'email'  ,   'password'  ],
'view'  : [  'name'  ,   'email'  ],
'update'  : [  'email'  ,   'password'  ]
}


def   rules  (self):
return   {
'name'  : [NameValidator()]
'email'  : [EmailValidator()]
'password'  : [PasswordValidator(  min_length  =  15  )]
}
```


The properties above means that it will have three properties; name, email, and password which will be set from constructor arguments; name, email, and password respectively and all of them will be cast to string.


Assuming that you have rules already implemented as Validation instances, this class has three different scenarios; create, view and update.


When you set the context of the object to any of the scenarios, only the specified properties will be validated and returned in the` attributes` property.


```text
user: User(
context  =  'create'  ,
name  =  'John Doe'  ,
email  =  'john.doe@gmail.com'  ,
password  =  'someverydifficulthashedpassword'  ,
)


print  (user.attributes)   # will return name, email and password


user.set_context(  'view'  )
print  (user.attributes)   # will return name and email


user.set_context(  'update'  )
print  (user.attributes)   # will return email and password
```


As you can see the attributes that you want to be projected can be specified through context scenarios. That also means when the` validate()` function is called, only the attributes for the specified scenario will be applied.


This makes it easier to create a different projection for your read replicas and apply different validation where needed.


### Future of rebase


Rebase is still very young and has a lot of improvements to be made and features to be added. With the help of the open source community, I hope we can make it a great package which is easy and fun to use.


If you feel motivated to improve the library, find us at[https://github.com/trivago/rebase](https://github.com/trivago/rebase) . Issues and pull requests are always welcome.
