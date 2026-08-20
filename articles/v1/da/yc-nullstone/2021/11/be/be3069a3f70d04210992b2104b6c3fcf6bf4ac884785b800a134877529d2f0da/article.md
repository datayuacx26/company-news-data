---
schema_version: "1.0.0"
document_id: "be3069a3f70d04210992b2104b6c3fcf6bf4ac884785b800a134877529d2f0da"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/designing-a-data-integration-pipeline"
published_at: "2021-11-30T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:01fd270d72065a6b37f8b5431f7d075b268ed6a99b65e7fe9bae41f7f4156d5a"
---

# Designing a Data Integration Pipeline

## Getting Started


You are faced with the task of taking data from many sources and processing them as transactions into your own software system in a reliable and consistent manner. The data could be coming from spreadsheets, databases, or other software systems. Also, you need to be able to easily add new data sources and scale to thousands or even millions of transactions a day. While the software community has made immense progress in areas such as standardized data formats (JSON, CSV, XML) and APIs (REST, OpenAPI), this is still one of the most challenging systems a team can build. In a[2019 market report by Business Wire](https://www.businesswire.com/news/home/20190122005564/en/Poor-Integration-Costs-Businesses-Half-a-Million-Dollars-Every-Year-According-to-2019-Market-Report) :


- **63 percent** of IT decision-makers say new business onboarding is too complex and takes too long
- **29 percent** report lacking the skilled resources to build and manage integrations between systems, applications, and partner ecosystems


To tackle this problem, let's break it down into it's key components with our goal being a flexible and reusable solution; a data integration pipeline. A data integration pipeline is a system that is used to take data from one system and process it into another using a sequence of steps. With this type of pipeline, we can take input data from many different sources and process the rows as transactions into our system. Typically these transactions include significant business logic, and each row of data received may result in updates or inserts into multiple tables.
‍


We will walk through the process of designing a data integration pipeline and launching it on AWS. By utilizing the serverless technologies AWS Lambda, SQS (Simple Queue Service), and API Gateway; we will be able to simplify our process into smaller steps, optimize for cost, and provide a mechanism that will scale very easily. In addition, we will provision our infrastructure (including environments for dev, staging, and prod) using Nullstone. This post is one of many in a series of[Nullstone Use Cases](https://www.nullstone.io/blog-categories/use-cases) .


‍


## Defining the Problem


Before we start building any infrastructure or cracking open our favorite IDE to begin coding, let's first break down the problem we are trying to solve. To illustrate this process, I will use an example as the target for our integration pipeline. In this example, let's imagine that we are a business that needs to integrate with each of our customers. As we onboard each customer, we will need to ensure that we can take the data they will supply to us and add this data to our system.


While we won't dive into every detail in this post, we will focus on some of the higher-level concepts to help us design our pipeline. In this scenario, we need to satisfy the following:


- We need to accept data from many different data sources and many different companies.
- Each set of data sent represents a day's worth of data. Because of this, we will acknowledge receipt of the data and then be responsible for all the processing that happens after that. If something fails, we will need to recover from it.
- The companies sending us data will do so in some pre-defined data formats such as JSON, CSV (comma separated values), tab-delimited, or even excel. As you can imagine, the customer will mess up the data formatting. We will need to account for malformed data.
- Since we are integrating with many customers, we will need a quick and straightforward way to handle the nuances of each. This is where we want to focus our coding efforts as we bring on additional customers.
- Each row of data will represent a transaction, and the fields being sent will vary from customer to customer. We will need to implement a way to translate or transform the data.
- For each row, we will want to execute a transaction in our system (which may update or insert into multiple tables).
- We need this pipeline to scale and be able to handle hundreds of thousands of transactions per day.


‍


## Scope


I have designed and built a few different integration pipelines throughout my career, but in this post, I will cover one general strategy that works well for an extensive range of scenarios. The goal is to produce a simple design that remains flexible. Keeping the complexity down helps us reduce risk and increase developer understanding of the solution. We want a system that our development teams can reason with, contribute to, and debug easily. We will build using interchangeable parts that follow contracts and only handle a single responsibility to achieve this.


‍


## Simplify Using Single Responsibility


To keep our thinking and solution as simple as possible, we will start by thinking about the problem using the[Single Responsibility Principle](https://deviq.com/principles/single-responsibility-principle) . This principle states that you should write your code in small pieces where each piece only has one responsibility or reason to change. We should apply this line of thinking to our code and our entire system, including the infrastructure. Once we define our building blocks with clear boundaries and contracts, we should compose our system from those blocks and interchange them as we need. So what are the basic building blocks of our pipeline?


### Phase 1: Receive the Data


- one way to solve this is to provide an API endpoint that customers can call
- if they are not tech-savvy enough to call an API, we can provide a user interface that takes the data the user is submitting and makes the API call for them
- through this API, we will store this data in permanent storage so that nothing is lost even if we encounter errors further along in the process
- we will want to send an acknowledgment to the customer - we should respond with the appropriate status code from the API, but we should also consider sending an asynchronous acknowledgment (perhaps by email to their IT staff) in case the API call is from an automated program
- publish this data to a queue for the next phase to handle


### Phase 2: Parse/Validate


- the data received may be a CSV file, JSON, XLS, etc. - parse the data to ensure that a valid file or data format was sent
- parse the data into a common data format to use for the rest of this pipeline
- for this example, we will parse the data into an array of hashes, with each hash representing a transactions
- publish each hash onto a queue for the next phase to handle


### Phase 3: Transform


- this phase is where all of your customer-specific logic will reside
- the goal of this phase is to transform the raw inputs into a standard format that can be acted on in a standard way in phase 4
- this may involve mapping fields with different names, cleansing date formats, combining multiple fields, etc
- before developing this phase, it is best to skip ahead and define the standard format that Phase 4 will require - provide a strong contrast between these two phases to keep the next phase generic


### Phase 4: Final Transaction


- this is typically done through an API, and in many cases can be a generic API that you use throughout the rest of your system
- if you are able to use the same API that is used by the rest of your system and user interface, you will know that all processing is consistent and testing/debugging this API becomes much easier
- sometimes bulk processing is desired to reduce the load on the system and achieve a higher level of efficiency - if you do so, try to keep the API as generic as you can and design it as if you were supporting the same functionality as your UI


By breaking the pipeline down into these parts, many of the individual responsibilities start to become a bit more obvious. Phases 1, 2, and 4 can be developed in a completely generic way. If we need to support a new data format, we can write a new parser and nothing else needs to change. It is also effortless to test each part of the system individually.


‍


## Constructing and Hosting our Pipeline


Now that we have laid out the various stages of our integration pipeline, we need to think about how we will host it. As a software developer, you might initially look to start building all of this using code. It is a good approach and one I have used before.


However, for this example, I'm going to take a little different approach that will allow us to move more quickly, get to market faster. We will use AWS as our hosting platform and utilize AWS Lambda, SQS Queues, and an API Gateway. Using this model, we will be able to keep our costs down, maintain a high degree of resiliency, and scale with ease.


Instead of hosting our system on dedicated machines or even containers, using serverless allows us to only pay for what we use and quickly scale up and down. Serverless helps with scenarios where you have big bursts and then no activity. You won't be charged anything when the serverless functions aren't being executed.


By using SQS, we are getting a queueing mechanism we know to be highly resilient and scalable. We no longer have to worry about developing the queueing mechanism.


### Phase 1: Receive the Data


The pipeline begins with an API Gateway which exposes an HTTPS endpoint that the customers can call. The request is passed off to a Lambda function that will store that data and acknowledge that the data was received successfully. The final step is to publish the data into an SQS queue.


### Phase 2: Parse/Validate


The 2nd phase begins with a Lambda function that is subscribed to an SQS queue. When the Lambda function in phase 1 publishes the data on this queue, this Lambda function is triggered to execute. Our Lambda function validates the data it receives and parses it into an array of hashes. Each hash is then published onto the next SQS queue for the next phase.


### Phase 3: Transform


The 3rd phase is also a Lambda function that receives a single hash from the SQS queue it is subscribed to. It does any mapping and transformation required to get the data into a data structure that follows a standard contract. This phase is where all of the customer-specific logic will reside. Because we have isolated this as a separate phase, we can develop new logic for each customer in this one spot and don't need to change the rest of the pipeline. When we are ready to test and deploy new customer-specific changes, we can deploy the changes to this phase into our QA environment. Once these changes have been tested, we can promote them to our production environment.


## Phase 4: Final Transaction


Finally, the Lambda function from Phase 3 calls our standard API to execute the transaction.


‍


## Building our Environment


To build out this environment in AWS, we could do this in a few different ways.


1. We could use the AWS console or CLI to configure all the infrastructure we need manually. Unless you are well-versed in this, it will take a very long time, and you won't have a way to repeat it when you need QA, production, or other environments.
2. You could use an IaC (infrastructure as code) tool like Terraform. While this will leave you with a repeatable way to stand up more environments, it requires a significant amount of effort to get to a working environment. Also, if you don't have any experience in Terraform, you will need to take a considerable amount of time learning a new tool.


If you are at a point where you want to try out SQS or Lambda functions, it is probably quite an overwhelming task, and it is hard to know where to start.


At Nullstone, we have seen this scenario play out many times. In the following video, we will walk you through how easy it can be to stand up the infrastructure for this scenario. In the end, you will be left with an infrastructure that is working out of the box, can be easily repeated across as many environments as you like, and can be modified easily as you scale.


‍


‍


Now that our pipeline is established, we just need to write the functions to handle each part of our pipeline.


‍


## Additional Environments


In the video above, we launched our solution into our dev environment to begin developing. Once we are ready for QA, staging, or production environments, we will need to duplicate our setup reliably to ensure a smooth rollout. As we add new customers and integrations, these additional environments are vital. The new code to handle each customer should be first rolled out to our QA environments to be tested and validated. If we have any bug fixes that need to be made, we should also roll these out to our QA environments to be verified before going into production.


Using Nullstone, we can add those additional environments and launch the infrastructure. All the configuration is automatically duplicated across environments. Your production environment will need more CPU/memory/etc., so adjust those values as you launch for that environment. To see the process of adding additional environments, please check out the video below.


‍


‍


## Wrap Up


In this post, we took a look at designing and launching an integration pipeline, including multiple environments. While we couldn't dive into all the details, hopefully, this helped you establish an architecture and design for your own integration pipeline.


If you would like help designing your integration pipeline or have any other infrastructure or cloud questions, please contact our team of cloud experts atinfo@nullstone.io .
