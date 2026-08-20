---
schema_version: "1.0.0"
document_id: "9c4b17cfd179e1667ccac9dc8c15e7d337eb144162727486df9b7fa3cfba0938"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/using-grpc-with-python/"
published_at: "2026-02-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:21:38.029889+00:00"
content_hash: "sha256:54cc57b78b6043f348485b2c729417dacebd3bd6250fdf664ada65b7ff1154eb"
---

# Using GRPC with Python Best Practices Guide

Microservices are now the architecture of choice for many developers when crafting cloud-native applications. A *microservices application* is a collection of loosely coupled services that communicate with each other, enhancing collaboration, maintainability, scalability, and deployment. There are several options for enabling this communication between microservices. When it comes to Python, gRPC and REST are two extremely popular directions to go.[REST](https://restfulapi.net/) is the most popular among developers, and it is sometimes used synonymously with the term “API”. However,[gRPC](https://grpc.io/) can be a better alternative to REST. Like REST, gRPC facilitates communication between services across data centers, providing load balancing and authentication features that are critical in microservice development. In this tutorial, you will learn the basics of gRPC and how to start using Python.


## What Is gRPC?


*gRPC* is a high-performance remote procedure call framework built on top of HTTP/2. In gRPC communications, gRPC clients play a crucial role by making requests to gRPC servers, allowing for a clear distinction between client and server responsibilities. A client application can directly call a method on a server application on a different machine as if it were a local object, ensuring a smooth client-server integration. gRPC abstracts the process of directly calling a certain endpoint on the server, as you would typically do with REST. It uses[protocol buffers](https://developers.google.com/protocol-buffers/docs/pythontutorial) as its interface definition language (IDL) and for data serialization as it handles client-server communications. Proto buffers will be discussed in more detail below, but you can also check the[documentation](https://developers.google.com/protocol-buffers/docs/pythontutorial) for more information.


### Why Use gRPC?


“Why gRPC?” you ask. It’s true that REST can seem like the best option in many cases. REST offers a mature ecosystem with extensive support and tools for multiple languages. However, it has some trade-offs, including low consistency when integrating an API across different languages and more difficult, higher-latency streaming. This is where gRPC comes into play.


Here are some of the benefits that gRPC offers:


### Language Agnostic


The framework is designed to work with multiple languages. The server will always receive the same kind of request, regardless of the client’s language. This also applies to the client, thus enforcing consistency in API communication.


### Better Streaming


gRPC has strong support for streaming client streaming, server streaming, and bi-directional streaming, and it’s easier to implement compared to REST. Beyond just being able to efficiently connect services synchronously, bidirectional streaming RPC combines both request-streaming and response-streaming functionalities.


### Better Speed


gRPC offers better speeds than REST, GraphQL, and SOAP because it uses protocol buffers for data serialization instead of JSON or XML. This makes the payload smaller and faster.


### Code Generation


Using the **protoc** or protocol buffers compiler, you can easily compile the .proto files into language-specific code, simplifying building your API service and client libraries.


## Getting Started with gRPC in Python


Now that you know more about gRPC, let’s look at it in action. This tutorial will show you how to create a simple API service and client that fetches real-time cryptocurrency prices.


### Prerequisites


To follow this tutorial, you’ll need the following dependencies:


- Python 3.5+
- [pip](https://pypi.org/project/pip)
- [virtualenv](https://pypi.org/project/virtualenv) (Optional)
- [grpcio-tools](https://pypi.org/project/grpcio-tools)
- [grpcio](https://pypi.org/project/grpcio)


### Installing Dependencies


Use the following commands to install the dependencies on your machine:


```text
# Create a directory for the project
mkdir   crypto-service
cd   crypto-service
# Create a virtual environment
virtualenv   crypto
# For those in Windows
crypto/Scripts/activate
pip   install   grpcio   grpcio-tools
# For those in Linux and MacOS
source   crypto/bin/activate
pip3   install   grpcio   grpcio-tools
```


### Creating the Workflow


When you’re creating API services and clients with gRPC, you will usually follow this workflow:


- Define services and messages in` .proto` file(s).
- Compile the` .proto` file(s) to code artifacts.
- Use generated code artifacts to implement the server code and client code.


### Protocol Buffer Basics


In gRPC, the structure of the incoming request, outgoing response, and handling method must be predefined in the .proto file(s). The incoming request and outgoing response are usually defined as *messages* . The handling method is defined as an RPC method with input and output messages. These RPC methods are organized into groups known as *services* . One .proto file can consist of multiple services, each with multiple methods. For instance, say you want to create a simple API service that receives a request consisting of only the cryptocurrency’s name. The service will then return the minimum, maximum, and average price of that cryptocurrency over a day and return none if the cryptocurrency is not found on the record. Such a service in gRPC should look something like this:


messages (requests and responses) methods service


cryptocurrency ( name) get_price GExchange


market_price ( min, max, avg)


To create this in gRPC, create a new` .proto` file as shown below:


```text
touch   crypto_service.proto
```


Here is how a protocol buffer definition will look. Copy and paste the below code to the` crypto_service.proto` file you created:


```text
syntax   =   "proto3"  ;
// Incoming request from client
message   cryptocurrency  {
optional   string   name   =   1  ;
}
// Response to be returned by API service
message   market_price  {
optional   float   max_price   =   1  ;
optional   float   min_price   =   2  ;
optional   float   avg_price   =   3  ;
}
// Service definition for GExchange
service   GExchange   {
// get_price method definition
rpc   get_price   (  cryptocurrency  )   returns   (  market_price  ) {};
}
```


### Compiling with Protoc


Now you have a proto buffer definition for the crypto service. The next step is using the **protoc** compiler to compile the **crypto_service.proto** file to code artifacts. You can use a single command to do this through your terminal (or command prompt for Windows users), as shown below:


```text
python3   -m   grpc_tools.protoc   -I.   --python_out=.   --grpc_python_out=.   crypto_service.proto
```


When you run the above command, it will generate two Python files from` crypto_service.proto` named:


- ` crypto_service_pb2.py`
- ` crypto_service_pb2_grpc.py`


You can look at the contents of the files, but don’t worry about them because you won’t edit them. If you check` crypto_service_pb2_grpc.py` , you will notice there are three classes generated, two of which are` GExchangeServicer` and` GExchangeStub` . You’re going to use them to implement server-side code and client code, respectively.


### Implementing the gRPC Server


Next, you’re going to implement the gRPC server from generated artifacts and create a new Python file within the same directory titled` crypto_server.py` .


```text
touch   crypto_server.py
```


You’ll need to first import all the necessary requirements, then create a class that will inherit` GExchangeServicer` . Its methods will have the same name as defined in the` crypto_service.proto` file. For this tutorial, the name is` get_price` as shown below:


```text
import   grpc
from   concurrent   import   futures
import   crypto_service_pb2   as   pb2
import   crypto_service_pb2_grpc   as   pb2_grpc
# A class for handling GExchange service
class   GExchange  (  pb2_grpc  .  GExchangeServicer  ):
def   get_price  (self, request, context):
return   pb2.market_price(  **  data.get(request.name, {}))
```


The only thing your gRPC server needs to be complete is the instruction for your service and the initial dummy data to query from. You’re going to use Python’s built-in feature` futures, gRPC library` with some components from generated artifacts to instruct your Python code on how to run the` GExchange` service. Once you add them, your code should look like this:


```text
import   grpc
from   concurrent   import   futures
import   crypto_service_pb2   as   pb2
import   crypto_service_pb2_grpc   as   pb2_grpc
class   GExchange  (  pb2_grpc  .  GExchangeServicer  ):
def   get_price  (self, request, context):
return   pb2.market_price(  **  data.get(request.name, {}))
def   serve  ():
server   =   grpc.server(futures.ThreadPoolExecutor(  max_workers  =  10  ))
pb2_grpc.add_GExchangeServicer_to_server(GExchange(), server)
server.add_insecure_port(  "[::]:50051"  )
server.start()
server.wait_for_termination()
data   =   {
"Ethereum"  : {  "max_price"  :   4000.0  ,   "min_price"  :   3590.0  ,   "avg_price"  :   3800.0  },
"Bitcoin"  : {  "max_price"  :   50000.0  ,   "min_price"  :   48539.0  ,   "avg_price"  :   49072.0  },
"Cardano"  : {  "max_price"  :   3.3  ,   "min_price"  :   2.9  ,   "avg_price"  :   3.12  },
}
if   __name__   ==   "__main__"  :
print  (  "running the gRPC server"  )
serve()
```


### Running the gRPC Service


You can now run the gRPC service as you would typically run the Python script, as shown below:


```text
python3   crypto_server.py
.....
running   the   gRPC   server
```


## Implementing the gRPC Client


In this section, you will learn how to implement a gRPC client in Python. First, create a new Python file,` fetch_prices.py` .


```text
touch   fetch_prices.py
```


Import all the necessary libraries and then create a class for your client. In the class constructor, create a stub or client that will connect to a gRPC. Here’s how to do that:


```text
import   grpc
import   crypto_service_pb2   as   pb2
import   crypto_service_pb2_grpc   as   pb2_grpc
class   fetchPrices  (  object  ):
def   __init__  (self):
# creates a gRPC channel to connect to a server
self  .channel   =   grpc.insecure_channel(  "localhost:50051"  )
# creates a gRPC stub(client) to communicate to server
self  .stub   =   pb2_grpc.GExchangeStub(  self  .channel)
```


Finally, create a method that will receive a cryptocurrency name as string, transform it into a message object, use the stub to send a request to the gRPC server, then return the response. Your code should look like this:


```text
import   grpc
import   crypto_service_pb2   as   pb2
import   crypto_service_pb2_grpc   as   pb2_grpc
class   fetchPrices  (  object  ):
def   __init__  (self):
self  .channel   =   grpc.insecure_channel(  "localhost:50051"  )
self  .stub   =   pb2_grpc.GExchangeStub(  self  .channel)
def   get_price  (self, name):
request   =   pb2.cryptocurrency(  name  =  name)
response   =   self  .stub.get_price(request)
return   response
if   __name__   ==   "__main__"  :
client   =   fetchPrices()
print  (client.get_price(  "Bitcoin"  ))
print  (client.get_price(  "Ethereum"  ))
print  (client.get_price(  "Cardano"  ))
```


As you can see in the script near the bottom, using the gRPC service through the client is similar to a normal function call within Python.


### Running the gRPC Client


Your gRPC server should already be running. If not, run it first; otherwise, your client scripts will raise an exception. Once you do that, you’re ready to go. Now run your` fetch_prices.py` script:


```text
python3   fetch_prices.py
........
max_price:   50000.0
min_price:   48539.0
avg_price:   49072.0
max_price:   4000.0
min_price:   3590.0
avg_price:   3800.0
max_price:   3.299999952316284
min_price:   2.9000000953674316
avg_price:   3.119999885559082
```


After running, your script should be able to effectively communicate with the gRPC server, and the response should look like what’s shown above since you used the same example for the data.


### Testing the Service Through the Command Line


To test the gRPC service without a script, you can use the **grpc** command-line tool. This tool allows you to send requests to your gRPC server and receive responses, making it easier to verify that your service is working as expected. Here are the steps to follow:


1.


**Install the grpc Command-Line Tool** : If you haven’t already installed the grpc tool, you can do so by running the following command:


```text
pip   install   grpcio-tools
```


2.


**Start the Server** : Ensure your gRPC server is running by executing the command:


```text
python3   crypto_server.py
```


3.


**Send a Test Request** : Open a new terminal window and use the grpc tool to send a request to the server. For example, to test the get_price method, run the following command:


```text
grpcurl -plaintext -d '{"name": "Bitcoin"}' localhost:50051 GExchange/get_price
```


The server will respond with the result of the request, displaying the maximum, minimum, and average Bitcoin prices. Using the **grpc** command-line tool, you can quickly test your gRPC service and ensure it functions correctly.


## Best Practices for gRPC in Python


Here are some best practices to follow when using gRPC in Python to ensure your code is maintainable, efficient, and scalable:


### Protobuf Organization


Keeping your protobuf definitions separate from your Python code is a good practice. You can store your .proto files in a separate directory or repository. This separation makes managing and maintaining your protobuf definitions easier, especially as your project grows. For example, you might have a directory structure like this:


```text
project/
├── proto/
└── crypto_service.proto
└── src/
├── crypto_server.py
└── fetch_prices.py
```


This organization helps keep your source code clean and modular.


### Protobuf Versioning


When making changes to your protobuf definitions, following a versioning strategy is essential. You can use a version number in your protobuf package to indicate changes to the API. This helps manage backward compatibility and ensures that your clients and servers remain compatible. For example, you can include a version number in your **.proto** file like this:


```text
syntax   =   "proto3"  ;
package   crypto_service.v1  ;
```


By versioning your protobuf files, you can make updates without breaking existing clients.


### Type Checking Protobuf-Generated Code


When using protobuf-generated code, it’s a good practice to use type checking to ensure your code is correct. Tools like **mypy** can help you check the types of your protobuf-generated code, catching errors early and ensuring that your code is maintainable. To use **mypy** with protobuf, you can install the necessary packages and run type checks on your code: <pre` pip install mypy-protobuf mypy --strict src/` By incorporating type-checking into your development process, you can improve the reliability and maintainability of your gRPC services.


## Conclusion


Now, you should better understand gRPC and how to use it as a Python developer. This tutorial demonstrated unary RPCs, in which a client sends a single request to a server and gets a single response back. There are multiple types of gRPC implementation, and today’s exercise is just the beginning of what you can learn. Using gRPC can offer better performance and more communication flexibility between your microservices. For more examples of using gRPC in Python, check its[GitHub site](https://github.com/grpc/grpc/tree/master/examples/python) .


### gRPC and Speedscale


When building out full-fledged systems with gRPC, testing your services for functionality and scalability will be crucial. Certain APIs may not be ready for integration, but you don’t want to postpone the development cycles waiting for completion. This is where Speedscale comes into play, helping to implement traffic replay for gRPC services to help with integration, mocking, and other angles related to testing your services for functionality and scalability using traffic replay. For more on how to mock gRPC APIs, check out our blog[here](https://speedscale.com/blog/mock-grpc-apis/) and sign up for a free trial of Speedscale to get started!
