---
schema_version: "1.0.0"
document_id: "ad6ebcd809e6be349cb0c13125079d178918b52c8eeb42f137f04fa1ad1bde3a"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/using-proxymock-with-aws-services/"
published_at: "2025-04-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:28e69b0137c93a707f66e481f9650d3e3ed1064979840c5f7b99e7e9a17b328a"
---

# Using Proxymock with AWS Services

Amazon Web Services, or AWS, offers a variety of cloud services ranging from AWS resources such as CDNs and data lakes to cloud computing and transformation services such as compute resources, virtual servers, and dynamic availability zones. For this reason, AWS cloud is one of the most broadly adopted cloud solutions, offering a global network of solutions at generally lower costs compared to on-premises solutions.


Because of the wide range of services on offer at AWS, many providers have taken to adopting the solution in order to get better access management, more dynamic operating systems through virtualization, and cost effective access to data centers. New feature sets have unlocked generative AI, machine learning, and complex analytics at scale and with low latency, offering better and more efficient solutions at scale.


## **Limitations of AWS Service Mocking**


However, despite AWS’s many features, some aspects present challenges when implementing mocks. These include:


1. **Authentication & IAM Issues** - since many AWS services are either part of a fully managed service package or locked behind complex authentication, IAM roles, policies, and permission structures typically stand in the way of easy mocking. These are quite difficult to accurately replicate in a mock environment, and while many providers have attempted to find a halfway solution, they typically start to drift further away from real-world data, undermining the utility of the mock approach in totality.
2. **Service Parity Issues** - AWS is well known for its frequent and significant updates, especially those which offer new features, API updates, and cutting edge security implementations. For this reason, static mocks can almost immediately fall out of sync with the AWS service it is mocking, making it difficult to keep up service parity.
3. **Lack of Network-Level Simulation** - AWS is a leading provider of IaaS and PaaS solutions, and as such, many AWS solutions require specific networking configurations. These introduce significant complexity into a standard mocking approach, which can make it exceedingly difficult to test variable network-related issues and scenarios such as latency spikes, AWS regions constraints, and other issues across the AWS partner network.
4. **No Real Quotas or Throttling** – while AWS services do enforce rate limits, API quotas, and throttling policies, these are hard to mock in a static format. Dynamic mocks that implement local quotes and throttling introduce substantial complexity, creating a mock surprise when applications hit real-world limits in production with actual data and request flows.
5. **Event-Driven Behavior Challenges** – many AWS services, including SQS, SNS, Lambda, EventBridge, and Step Functions, rely on asynchronous, event-driven processing which can be difficult to replicate with simple mocking libraries and approaches. While some tools get close to mocking these, they can often lack full capture of execution delays, retried, failures, and so forth.
6. **Data Consistency & State Management** – there are quite a few AWS services like DynamoDB, S3, and RDS that operate at massive scale and offer consistency models that are difficult to replicate with in-memory mocks. For example, in AWS, an S3 object might take time to propagate globally, but in a mock, changes are usually instantaneous. This can result in significant issues regarding the applicability of findings and the consistency of built solutions for complex problems.
7. **Limited Cloud-Native Features** - features like Auto Scaling, Spot Instances, AWS Marketplace AMIs, and Global Accelerator instances are highly dependent on AWS’s infrastructure, and as such, are not easy to mock effectively.
8. **Integration with External AWS Services** - since many applications rely on cross-service interactions (e.g., Lambda triggered by S3 events, API Gateway calling a Step Function, etc.), static mocks or basic mocking libraries might miss the underlying complexity of the service being tested. Mocks often support individual services, but they often struggle with accurately replicating interactions across multiple services, especially in a highly complex environment like AWS.


## **How Speedscale Helps Mock GCP Services**


In response to these challenges, Speedscale leverages a powerful solution-[capture and replay](https://speedscale.com/blog/traffic-replay-tutorial/) . By capturing real interactions between your applications and the AWS services they leverage, Speedscale can replay these interactions. This helps create a flow that is based upon actual data and actual observed interaction. Proxymock uses these interactions to build accurate, production-aligned mocks. They are dynamic and based on real, observable traffic—not simulated flows.


This approach unlocks some pretty huge benefits:


1. **Better Variable Injection and Testing** - static mocks often struggle with mirroring uncertain conditions, and this is especially important with something like AWS where the complexity is in the solution itself. Speedscale can introduce variables and inject faults to better mimic uncertain variables within the production environment. Simulating network delays, timeouts, or even faults and data overflows, Speedscale can help you isolate the worst case scenario and attack it in the safety of the mock environment!
2. **More Accurate Performance Testing** - a mock is only as good as it is aligned to real world data. When managing load and performance testing, this is doubly important, as you’re building against an eventual condition that is hard to identify and scale. Using actual data and scaling this data based on potential iterations, Speedscale can help you test application performance in an A-B testing manner that is second to none.
3. **Accurate Rate Limiting and Quota Simulation** - AWS is a business, and because of that, a lot of the limitations developers encounter are going to come from rate limiting and quotas. Static mocks can bypass this restriction entirely, making it hard to project actual costs and scalability. Speedscale allows you to impose these restrictions and requirements on test data, allowing for more dynamic and real-world aligned test cases.
4. **CI/CD Integration** - Speedscale integrates into a variety of[CI/CD solutions](https://docs.speedscale.com/integration/cicd/) , offering a stronger integration in the development and testing pipeline. This makes testing a breeze, abstracting away the resource provisioning and credentialing that often is a hallmark of AWS mocking.


## **Key Benefits Over Traditional Mocking**


Speedscale and Proxymock is a match made in heaven, offering some significant benefits:


- **Easy to Get Started and Cost Efficient** - with Speedscale, you don’t need to mock AWS services using extensive and confusing AWS SDK mock guidelines or complex scenarios based on local machine and server mirroring - you simply observe and build, creating a testable and producible product that you can configure, validate, and verify against a wide range of scenarios and endpoint resolutions.
- **More Accurate and Specific** - since you are in full control over what traffic gets replayed and in what form, you can test everything from the limiting of advanced features to new error handling and everything in between. Want to test a new handler method? Maybe you want to pipe a Lambda function to a different node. Maybe you have a customer differentiation and revenue model depending on database needs and freemium account restrictions. ALL of that can be mocked accurately and tested in variable conditions using Speedscale!
- **Highly Flexible Testing** - AWS is a complex system, and the services and applications which utilize it often introduce their own quirks and complexities. Speedscale allows you a substantial amount of flexibility in the mock stage, allowing you to mimc real-world externalities, internal code issues, new service development, and so forth. This ultimately makes your testing more flexible and thus more useful.


## **Getting Started with Proxymock**


Speedscale leverages[a three stage process](https://docs.speedscale.com/) . In the first stage, Observe, Speedscale uses a proxy called go proxy to capture incoming and outgoing traffic. In the analysis stage, Speedscale then looks at the traffic stored as a snapshot and filters or transforms it for specific purposes. In Replay, this traffic is then replayed, allowing you to mirror traffic as if it is routing and being fed into the service.


Where Speedscale changes the game is Proxymock. Proxymock allows you to use that initial Observe stage as a building phase, creating a mock from the observed traffic and endpoint interactions. This eliminates the entire need to create a manual mock, allowing for dynamic mock creation and iteration across complex use cases and scenarios.


### **Installing Proxymock**


Getting started with Proxymock and AWS is easy, and takes just a few minutes. If you can set up AWS Lambda functions, good news - Speedscale is nowhere near as complicated to get going!


First, install Speedscale via[Homebrew](https://brew.sh/) using the following command:


```text
brew install speedscale/tap/proxymock
```


Now initialize Proxymock using the run command:


```text
proxymock run
```


This command will generate a snapshot. Snapshots are how Speedscale stores observed traffic - each snapshot is given an ID that can be referenced for future replay:


```text
...
export   http_proxy  =  http  :  //127.0.0.1:4140
export   https_proxy  =  http  :  //127.0.0.1:4140
...
proxymock is capturing and mocking snapshot   <  snapshot  -  id  >
...
```


Once you have the snapshot, you can inspect it using the inspect command:


```text
proxymock inspect snapshot <snapshot-id>
```


This will create a terminal UI showing you all of your traffic:


From here, you now have a mock! This mock has built not only the endpoints and systems observed during the data observation phase, it has also stored the data that was observed for future replay. You can now select any of these data points and transform them across a variety of factors, allowing for very dynamic and controlled testing. You can iterate your tests around usage and error scenarios, missing region services, complex module or message variables, ,failed API call handling, or even AWS account validation issues.


When you want to expand your mock for further unit testing or to ingest new systems into the log, you can simply run the analyze command as follows, inputting the snapshot ID you collected above to alter the snapshot:


```text
proxymock analyze <snapshot-id>
```


From here, the sky is the limit! AWS can operate in a variety of different ways with variable setup params, import constraints, traffic operation restrictions, and more. You can make your mock do anything and everything you want it to do, introducing new constraints and variables as you iterate on the codebase!


## **AWS Services Considerations**


It should be noted that AWS services often use quite different formats and protocols depending on what service you’re using. The AWS SDK services can even vary from version to version, so you’ll need to keep in mind some factors as you’re building out your mock and iterating on traffic.


###


## Headers


- **X-Amz-Target** - this header contains the API version and command, such as is used with a DynamoDB table. You may have to specifically note this as a resolution parameter in your mock if the service does not properly define it as a service routing target - in this case, you should treat it the same way you would treat an API gateway or ingest redirection.
- **Authorization** - these headers contain tokens that are used to authenticate the request. Because your mock is not pinging live services, you will have to keep in mind that authorized requests may need additional routing in a live production environment since your token is implicitly accepted in the mock service.
- **X-Amz-Date** - this head contains data and request timing in ISO 8601 formatting. This might be required in certain AWS services, so again, you should plan for this to be handled in production with a bit more finesse than in the mock.
- **X-Amz-Security-Token** - the same applies here as the authorization section above - consider that this might be more complicated in production compared to the mock service due to the nature of implicit flows.


Generally, you can change these variables inside the mock without any issues - Proxymock will often deduce the target information seamlessly. Just keep in mind that you’ll need to validate handling for external processing.


## Conclusion


Getting started with Proxymock and Speedscale is ludicrously easy, and can help you start building full code mirroring mocks with very little overhead. As long you follow some basic best practice driven coding and generate ample documentation, you can get started with highly effective mocks in minutes.
