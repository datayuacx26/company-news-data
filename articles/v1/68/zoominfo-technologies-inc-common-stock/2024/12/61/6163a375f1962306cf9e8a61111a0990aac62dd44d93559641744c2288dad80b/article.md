---
schema_version: "1.0.0"
document_id: "6163a375f1962306cf9e8a61111a0990aac62dd44d93559641744c2288dad80b"
company_key: "zoominfo-technologies-inc-common-stock"
company: "ZoomInfo Technologies Inc"
source_id: "zoominfo-technologies-inc-common-stock-rss-ef9b8d7bcd4f"
canonical_url: "https://engineering.zoominfo.com/python-match-client"
published_at: "2024-12-18T02:44:06+00:00"
first_seen_at: "2026-07-20T23:24:42.992125+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:9a3b5796184d4492364a3efe3bed6e85250bf6fcaa5ec8b850cb64a8fc6742c2"
---

# Python Match Client

## **Problem statement**


Along with search service, ZoomInfo’s match service is a fundamental building block for our applications. It functions by working with the search service to filter data and get candidate profiles, which are then ranked to intelligently return a match profile for a person or a company. For many engineers and analysts at ZoomInfo, the ability to interact with match service, which is often facilitated with the help of a client, is vital.


However, in the past, only the Java match service client has been consistently maintained. Though there is an existing Python client, it has become difficult to maintain and understand in the process of being used and edited by different teams over the years. Because of the difficulty of manually updating the code, the Python client does not have immediate access to new features when match service is updated like the Java client does. The task, then, is to keep the Python and Java clients in sync more easily, which we will accomplish using gRPC. Developing a new Python client using gRPC will not only make it easier to maintain (which will simplify the process of rolling out new features), but it will also make it easier to use. We hope that this new client will allow developers who work mainly with Python to have the freedom and flexibility to work with match service without having to rely on other developers.


## **Solution**


### **Why gRPC?**


Before discussing how I integrated it into my own code, I will provide a brief overview of gRPC. gRPC is a remote procedure call framework that is intended to connect microservices (often written in different languages) together. This means that I can call a method that is implemented by a server application on a different machine as if it were a local object.


gRPC uses HTTP/2 for transport, which should reduce latency, and it uses protocol buffers as its data format. To use protocol buffers, one can take the following steps:


1. Define services (parameter and return types for methods) and messages (outlines of data structures) in a proto file
2. Generate the proto file into class definitions in Python, Java, or another language
3. Use class definitions in server or client code


As the same original proto file can be used to create code in a multitude of different languages, gRPC has the benefit of being language-agnostic.


Incidentally, the Java match service client already uses gRPC to run match requests, meaning that the proto files containing the service and message definitions already exist. With the goal of keeping the Python client in sync with the Java client in mind, it makes sense to integrate gRPC into our Python client. This way, we can update both of the clients when the data model is changed simply by editing the proto file and recompiling it into Java and Python code.


### **Quality of life fixes**


Despite the many advantages described above, protocol buffers can be inconvenient to use. One reason for this is that the code for Python classes that is generated from a proto file is very compressed and difficult to read for a human. Not only that, but IDEs don’t recognize the class and method definitions in the generated files, meaning that they can’t provide code completion and type checking. For a developer who is not already familiar with these data structures, trying to construct a match request object using the generated Python class would involve guessing what fields need to be filled in or finding the original proto files to reference.


To solve this problem, it is necessary to install another plugin that, when used with the protocol buffer compiler, will produce Python classes that can be read by a human and recognized by an IDE. The developer can thus benefit from code completion and type checking, as well as the ability to directly access the Python class definitions and view the fields, methods, and relationships between data structures.


### **Implementation**


Overall, the implementation of the client involved several steps.


1. Install the protobuf compiler plugin: The first plugin that I experimented with was python-betterproto, which offered clearer generated code. However, after attempting to integrate it into existing code, it became apparent that it was not compatible with some of the packages that were already present. I then tried mypy-protobuf, which was much easier to work with.
2. Generate the Python classes: I created a script to combine this step and the next. The script pulled the proto files from existing repositories and compiled them using the plugin.


1. Package classes and upload to Jfrog: The aforementioned script also placed the generated code into a directory, which was then packaged up. Once this code was pushed and merged, it was uploaded to Jfrog.
2. Create client code: Much of the original Python client code was used to define classes for match requests and responses. Since this is taken care of by the generated code, the rest of this step was much more straightforward. The code simply needed to create an instance of a match request class using fields passed in by a user via a JSON or CSV file.


## **Summary**


By creating an accessible Python client, we have enabled a broader set of users with different skill sets to work with match service, removing a limitation on our ability to make progress towards a better product. For Python engineers needing to make matches, we offer more independence and less reliance on Java teams, and for Java engineers, we offer more freed up time.


The new client can easily be kept up-to-date with new features of match service, and it will produce results that are more consistent with the Java client. With about 150 lines of code as opposed to 1500 in the original code, the new Python client is also much simpler, making it easier to understand and more ready for change. We are looking forward to integrating this tool into our workflows, and we hope that it will be an asset in boosting productivity for teams working with match service.


**References** : FreeCodeCamp (2020)[https://www.freecodecamp.org/news/googles-protocol-buffers-in-python/](https://www.freecodecamp.org/news/googles-protocol-buffers-in-python/)
