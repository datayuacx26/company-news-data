---
schema_version: "1.0.0"
document_id: "cf122481bb6da867ac2a80c1623c168940deff5620a85711c71f9971364d7b00"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/guide-to-testing-sqs-based-microservices-with-signadot-sandboxes/"
published_at: "2025-09-05T17:07:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:ecdb8f4efe7157fd146027ebc8084c95d7eff16b331c49aed88606c3863476a1"
---

# Guide to Testing SQS-Based Microservices with Signadot Sandboxes

## **Introduction**


Welcome to the SQS + Signadot Sandboxes tutorial! In this guide, you’ll learn how to quickly test new versions of SQS-based microservices—either in a pull request or during local development—by leveraging Amazon SQS integration running in a Minikube cluster, with a brief look at the SNS-to-SQS fanout pattern for broader message distribution. This setup enables you to:


- Rapidly iterate on consumer based message processing without redeploying the entire stack
- Use sandboxes to isolate and test changes alongside a stable baseline
- Observe message flow and processing in real time
- Test SQS integration patterns safely in development


**What you’ll learn:**


- Deploy SQS-based microservices and Signadot in Kubernetes
- Run producer and consumer services both baseline and sandboxed versions.
- See how AWS SQS Message distribution works. Meanwhile SNS + SQS fanout integration
- Deploy a sandboxed consumer and route messages to it
- Understand how message routing and selective processing work


## **Prerequisites**


Before you begin, ensure you have the following prerequisites set up:


### **1. Minikube with Docker**


- Install[Docker](https://www.docker.com/) ,[Minikube](https://minikube.sigs.k8s.io/docs/start/) &[Helm](https://helm.sh/) on your local machine
- Start Minikube:` minikube start —-driver=Docker`
- To use Minikube’s Docker daemon instead of the local one run:` eval $(minikube docker-env)`
- Verify cluster is ready:` kubectl cluster-info`


### **2. Active AWS account**


- Create[AWS](https://aws.amazon.com/) account and activate account with card and mobile verification
- Or alternatively you can use[LocalStack](https://docs.localstack.cloud/getting-started/installation/) ( which has 14 days trial version ) for emulate AWS services
- Create[IAM](https://aws.amazon.com/iam/) user to grant the required permission to use aws resource outside the AWS project


### **3. Signadot Account and Operator**


- Sign up for a[Signadot account](https://www.signadot.com/)
- Install the Signadot Operator in your cluster by following the[Signadot Operator installation guide](https://www.signadot.com/docs/installation/signadot-operator) ‍
- Install the[Signadot Cli](https://www.signadot.com/docs/getting-started/installation/signadot-cli) tool on your local machine


## **Project Setup**


- Clone the Project Repository


Here is a more detailed explanation of the project’s folder structure, elaborating on the purpose of each directory and file and how they contribute to the overall application.


- AWS Cloud Setup for SQS and SNS


- Please go to this[URL](https://signin.aws.amazon.com/signup?request_type=register) and complete the required steps to create your AWS account.
- Create a new[IAM](https://aws.amazon.com/iam/) user and grant the necessary permissions.
- Create an access key that you have to store as a k8’s Secret to access aws SNS and SQS services
- Copy & Past Access Key ID and Secret Access Key into k8s/secrets.yaml file in the project folder. ( Both Access Key ID and Secret Access Key should be converted into base64 format )


As you can see, The image below shows sqs-sns-user[IAM](https://aws.amazon.com/iam/) user information and permission policy AmazoneSNSFullAccess, AmazoneSQSFullAccess that allows access to AWS SNS & AWS SQS services.


Hooray! 🎉 We’ve finished all the setup needed to get our project up and running — now it’s time to fire it up and see it in action!


## **Build the demo app**


First, you need to build the Docker image. The image we’re creating, **sqs-signadot** , is a simple demo application that we’ll use to showcase the **Shared SQS** & **SNS + SQS fan-out** pattern.


To build the image, run:


Verify the docker images in your docker repository inside minikube.


Whoo! 🎉 The build is successfull. sqs-signadot image exists in docker image repository.


## **Deploy the demo app**


These simple steps will deploy the demo app that consists of Frontend, Producer & Consumer services to set up baseline flow of the AWS SQS & SNS with Signadot integration.


Following services been deployed :


- **Frontend -** Service that exposes the GUI. And also forward incoming messages ( From GUI ) to a python Fast API app ( producer ) via HTTP.
- **Producer -** Service that can publish messages to AWS SQS queue.
- **Consumer -** Service that implement SQS Subscription to **Selectively Consume** Messages between baseline and sandbox.
- **Redis Server -** Store & Retrieve event logs to show how messages have been distributed.


Lets check everything work as expected :


Next, The tunnel is going to be established by Signadot, so that all in-cluster services are made available locally. To create config.yaml refer[https://www.signadot.com/docs/getting-started/installation/signadot-cli#local-configuration](https://www.signadot.com/docs/getting-started/installation/signadot-cli#local-configuration)


Use the output of the following command as the value for` kubeContext` .


Note: The config.yaml file path should be ~/.signadot/config.yaml


As you can see, Signadot local connect is exposing your services to your host machine, The frontend-service is exposed at[http://frontend-service.aws-sqs-app:8080](http://127.0.0.1:8080/) to interact with web GUI.


### **Initializing both AWS SQS queue and AWS SNS topic**


In the cloud native world the ability to make infrastructure provision automation would be amazing. By compiling these amazing concepts below code shows how the SQS queue and SNS topic can be created by code itself when producer or consumer service startup.


This image shows the SQS queue being created and how it’s showing in the AWS console. Additionally the Access Policy is being created to work with SNS to SQS Fanout pattern.


The image below shows an **AWS SNS topic** with an **SQS queue** subscribed to it. We’ll be using this setup to demonstrate how the **SNS + SQS fan-out pattern** works in practice.


## **Test Baseline Flow without using Signadot’s Sandbox**


The architectural flow of the baseline message processing diagram.


The code below shows how the producer has done his job to publish a message to the AWS SQS queue.


Head over to[http://localhost:8080](http://localhost:8080/) in your browser to use the AWS SQS demo frontend and send a message. With no sandboxes set up yet, the baseline consumer will pick it up, and you can watch it appear in the frontend interface, just like in the screenshot below.


### Producer’s header context propagation


Our next step is to explore consumer sandbox testing. Having demonstrated the basic message flow between producer and consumer services in our demo application, we now need to address a critical development challenge: **how to rapidly test new versions of producer and/or consumer code without disrupting shared testing environments** .


Our goal is to create an isolated testing environment where developers can validate changes before merging. We’ll accomplish this by:


1.


**Using OpenTelemetry auto-instrumentation to propagate request headers** - ensuring context flows seamlessly from producers through the messaging system to consumers


2.


**Implementing selective routing based on header values** - directing traffic to sandboxed versions of services when specific headers are present


3.


**Deploying new service versions using Signadot sandboxes** - creating isolated environments for testing code changes from dev branches or local workstations


This approach will enable you to test new consumer logic, producer modifications, or both simultaneously in a controlled sandbox environment. Let’s walk through each step:


How **OTel auto-instrumentation** propagate headers without modifying application code.


When you build the container image using **docker build,** the **Dockerfile** will install the required packages to implement **OTel auto-instrumentation.**


After that the Fastapi services ( Both Frontend & producer ) are powered up with **Otel** automatic header context propagation through the command below.


## **Test Sandbox flow with using Signadot’s sandboxes**


A Signadot **sandbox** is an isolated, short-lived environment that lets you safely test code changes without impacting test environment traffic.


What does consumer will do in the sandbox:


**Instantiate Dedicated Subscribers for Each Sandbox -** so their consumption offsets remain isolated.


**Filter Out Irrelevant Messages Using the Routes API -** based on routing key evaluation via the Routes API.


**Preserve Context by Propagating the Routing Key Downstream -** by including the routing key when the subscriber communicates with other services or message flows


Let’s take a look at how the router api periodically fetches routing keys in the consumer sandbox.


The thread that is running in the background to keep the router api stay alive. Meanwhile` _periodic_cache_updater` routine fetches the routing keys with 05 sec period manner.


How to extract the OTEL baggage header inside the consumer code


And here’s where selective consumption comes into play. At this stage, the consumer checks the routing key of each message against its own sandbox routing key using the Routes API. If it’s not a match, the message is skipped and immediately released back into the queue, reducing its visibility in this sandbox and making it instantly available for the correct sandbox’s consumer. This guarantees isolation while keeping message delivery fast and efficient.


## Create the sandbox — Signadot’s Signature feature


To create the consumer sandbox lets go to build a sandbox configuration file.


This YAML tells Signadot:


Create me a sandbox called` sqs-counsumer-sandbox` in my cluster, fork the` consumer-deployment` from` aws-sqs-app` namespace, set an environment variable to indicate the sandbox name.


Lets apply the sandbox configuration:


**Note:** To run the following command, you have to install the signadot CLI tool on your local machine.


As you can see the sandbox called` sqs-counsumer-sandbox-dep-consumer-deployment-`


` 7ca2ec39-cc4dfw2p` has been created and the age of the pod shows as 61 seconds.


## Test Sandbox behavior with routing key


Below diagram will illustrate how the routing key works.


Select the sqs-consumer-sandbox after enabling Signadot’s browser extension.


In the next section, building upon the shared SQS pattern, the focus shifts to the SNS-to-SQS Fan-out pattern. For simplicity, instead of creating ephemeral queues, the behavior of this pattern will be demonstrated using an existing queue, with multiple consumers sharing that queue and coordinating message handling in the same way as with plain SQS.


Another option with SNS/SQS is to give each consumer its own queue, where every consumer receives a full copy of the messages. In this case, queues may be created dynamically, and consumers apply selective logic in their code to drop messages not intended for them.
The[https://aws.amazon.com/getting-started/hands-on/send-fanout-event-notifications/](https://aws.amazon.com/getting-started/hands-on/send-fanout-event-notifications/) elaborate the implementation of SNS + SQS Fanout pattern with ephemeral SQS Queue.


## Creating Producer Sandbox to implement SNS to SQS Fanout pattern


To avoid repetition with consumer sandbox, the sandbox boilerplate won’t be revisited here; however, a few key points will be highlighted to ensure clarity and prevent any confusion.


- **Forked workload** — The producer deployment is sandboxed for SNS integration
- **ENV variable** — Introduce new env variable called SNS_FANOUT_PUBLISH to conditionally check SQS message publish from SNS publish. The related code shown bellow


Lets provision the sandbox:


Create **Signadot Router Group** to **control how traffic is routed into sandboxes** .


The **router group** lets you route network traffic from one or more sandboxes to one or more endpoints based on label selectors. It acts like a traffic router or load balancer within your Signadot sandboxes and Kubernetes clusters.


Lets provision the router group:


### Scenario 1 - SNS to SQS baseline consumer


The diagram illustrates how the message will flow in scenario 1.


As you can see the request has been sent through the producer sandbox. Which has published the message to AWS SNS topic and consumed by baseline consumer.


### Scenario 2 - SNS to SQS sandbox consumer


Another diagram that illustrates the message flowing behavior in scenario 2.


As you can see the request has been sent through the producer sandbox. Which has published the message to AWS SNS topic and consumed by sandbox consumers.


## ‍ **Summary** ‍


In this tutorial, you learned how to use a **shared Amazon SQS queue** with **Signadot Sandboxes** to quickly test new message processing logic in an isolated Minikube environment. We demonstrated deploying baseline services, safely routing messages through the shared queue, and using sandboxes to validate changes without disrupting the main processing flow. While the focus was on the shared-queue pattern, we also touched on how the **SNS-to-SQS fanout** pattern can broadcast messages to multiple queues for broader testing scenarios.


This approach enables faster iteration and more reliable integration testing for event-driven microservices architectures, offering:


-


**Realistic message flow simulation** with shared SQS queues.


-


**Safe isolation** for experimental consumers in sandboxes.


-


**Compatibility** with fanout-based testing via SNS-to-SQS.


-


**Reduced risk** when validating new logic alongside live-like traffic.
