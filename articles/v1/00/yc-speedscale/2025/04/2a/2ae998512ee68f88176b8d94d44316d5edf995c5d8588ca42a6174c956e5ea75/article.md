---
schema_version: "1.0.0"
document_id: "2ae998512ee68f88176b8d94d44316d5edf995c5d8588ca42a6174c956e5ea75"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/using-proxymock-with-gcp-services/"
published_at: "2025-04-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:8eabf703457b302c978cc48783b247ca28ae6eb7362c6852af77f0018c29f0be"
---

# Using Proxymock with GCP Services

Google Cloud Platform, or GCP, is a cloud resources collection offered by Google for enterprise and standard users. GCP offers a wide range of cloud services, including compute, storage, networking, security, analytics, and even machine learning models.


Google Cloud products are the backbone of many cloud applications - the relative flexibility of Google Cloud and the scalable and predictable cost management on offer provide a decent way to get an application bootstrapped and deployed, regardless of the service or customer localization.


## **Limitations of GCP Service Mocking**


While Google Cloud Platform offers a wide variety of services, there are some specific challenges that come with trying to mock a service hosted on it:


1. **Authentication Complexity** – many Google Cloud services require authentication via OAuth, service accounts, or IAM roles, making it difficult to create fully isolated mock environments without setting up fake credentials or disabling auth checks in test scenarios. Google APIs are often quite strict, requiring authentication across a variety of API calls. This can in turn lead to overly complex attempts at local mocking at best, especially when you’re trying to meet the high demands of the library or client documentation method.
2. **Limited Official Mocks** – while Google does provide some emulators (e.g., Firestore, Pub/Sub, Cloud Spanner), not all services have official local emulators - and where these emulators exist, they nonetheless each have their own significant pitfalls, limitations, and issues.
3. **Feature Parity Gaps** – using static mocking can introduce issues with feature parity. The reality is that Google’s offerings are very specific, and as such, anything that drifts from the real traffic and use case will introduce significant gaps in parity coverage.
4. **IAM & Permission Issues** – many GCP services enforce strict IAM policies, which are not always reflected in mock environments, leading to potential authorization issues when moving from local testing to production. This strict policy enforcement can also make building the mocks themselves overly difficult.
5. **Network & Latency Simulation** – GCP services like Cloud Storage, BigQuery, and Pub/Sub involve network calls, and mimicking their behavior - including latency, retries, and failures - can be difficult in a local environment. Poorly mocking these functions can result in a mock that works great in testing, but fails to reflect the actual build out - which undermines the entire purpose of the mock in the first place.
6. **Billing & Quota Limitations** – while local mocks avoid actual costs, they don’t reflect real-world constraints like API rate limits, request quotas, or billing-related errors, which can cause unexpected failures when deployed.
7. **Event-Driven Behavior** – services like Cloud Functions, Cloud Tasks, and Pub/Sub rely on asynchronous event-driven architectures, making it tricky to accurately reproduce event sequencing and timing in a typical mock setup.


## **How Speedscale Helps Mock GCP Services**


Luckily, Speedscale can help mitigate many of these issues. It does this by offering one huge advantage - traffic capture and replay. Speedscale captures actual interactions between your application and GCP services - e.g., BigQuery, Pub/Sub, Firestore, Cloud Functions - replaying them for testing to ensure that your mocks behave just like real GCP APIs. Ultimately, this makes for a mock that is closer to production use cases and is more accurate.


Adopting this strategy has some huge additional benefits:


1. **Stateful & Event-Driven Testing** - since many GCP services are asynchronous (e.g., Pub/Sub, Cloud Tasks, Eventarc), Speedscale can enable stateful mocking based on actual data flow and utilization, ensuring that event sequencing, message processing, and retries behave as expected. This bypasses a lot of the stateful code requirements in traditional mocking, allowing you to define and test each unit or function test specifically rather than trying to navigate complex class or object storage.
2. **Latency & Fault Injection** - unlike static mocks, Speedscale can introduce a variety of conditions for more accurate functionality. By introducing network delays, API timeouts, and other simulated failures, Speedscale can help developers test how their applications would handle everything from GCP service disruptions to network failures.
3. **Quota & Rate Limiting Simulation** - GCP services impose API rate limits and quotas that many traditional mocking tools don’t support, and even if they do, you’ll often incur some pretty significant costs during the mocking stage. Speedscale allows developers to test under real-world constraints, preventing unexpected failures in production and combining the cost of building the mock into the operational costs that already exist.
4. **Load & Performance Testing** - Speedscale allows developers to test how their applications perform under realistic conditions utilizing real interactions. Traditional mocks tend to validate the core functions and then simulate traffic based on complex models - Speedscale uses real data for real insights!
5. **CI/CD Integration** - Speedscale seamlessly integrates into CI/CD pipelines, making it easy to test cloud service interactions without needing to provision actual GCP resources. This makes for much easier development and deployment, and ultimate saves significant time and costs.


### **Key Benefits Over Traditional Mocking**


Using Speedscale unlocks some pretty significant benefits for adoptees:


- **Less manual effort** - there’s no need to manually create extensive mock APIs, as Proxymock automatically generates mocks based on observed traffic, saving time and effort.
- **Higher accuracy** - since Speedscale and Proxymock use real-world GCP interactions instead of static approximations, your mock will more accurately reflect the reality of the product on the ground.
- **Better resilience testing** - Speedscale allows you a substantial amount of flexibility in simulating real-world service outages, throttling, and failures, making for better and more accurate resilience testing.


## **Getting Started with Proxymock**


Speedscale as an offering has 3 core processes - observe, analyze, and replay. The idea is that in each stage, a critical part of the mock supportive infrastructure is built, allowing for flexible creation and maintenance.


In the Observe stage, Speedscale runs a proxy called go proxy which brings in traffic. The inbound and outbound traffic that is being recorded is being stored as a snapshot, which can be manipulated in the Analyze stage. In this stage, traffic can be filtered and transformed to reflect a variety of test cases and system configurations, allowing for highly flexible and dynamic testing. Next, in the Replay step, this traffic can then be replayed as recorded traffic against the running application.


Proxymock is a unique solution as it automatically builds the mock during the initial observation recording phase. This allows you to dynamically record traffic while also building a mock of the system handling that traffic.


### **Installing Proxymock**


Getting started with Proxymock and GCP services is super easy! Firstly, assuming you have Speedscale installed and initialized, you’ll need to install Proxymock itself. To do this, you can install via a variety of systems - for the purposes of this article we’ll use[Homebrew](https://brew.sh/) via the following command:


```text
brew install speedscale/tap/proxymock
```


From here, you’ll want to initialize Proxymock using the run command as follows:


```text
proxymock run
```


This will generate an output similar to the below:


```text
...
export   http_proxy  =  http  :  //127.0.0.1:4140
export   https_proxy  =  http  :  //127.0.0.1:4140
...
proxymock is capturing and mocking snapshot   <  snapshot  -  id  >
...
```


It’s important to notice here that Proxymock is capturing and mocking at the specific snapshot location as referenced in the section above. This ID can be used to replay this particular traffic snapshot in the future, either in its current state or remixed and transformed into another use case.


Once Proxymock has built the snapshot, you can look at it using the inspect command:


```text
proxymock inspect snapshot <snapshot-id>
```


This will open a terminal UI that will show you all of your traffic:


When you want to expand this mock - say, building in new GPC services as you implement them - you can analyze this snapshot with new requests and responses using the analyze command as follows:


```text
proxymock analyze <snapshot-id>
```


It’s that simple! Since Speedscale leverages traffic capture and replay, you don’t need to implement complex GPC functions or systems - largely speaking, they will be mockable simply through the observed traffic and interactions as captured by Speedscale’s Proxymock!


## **Summary**


Your local environment no longer requires the IP Stack API key or AWS DynamoDB. You can run the demo app simply by opening the Command Palette and run proxymock: Start Debugging.


The app will run normally - except that it will use the mock server you created in the previous step. If you need to update your mocks, just re-record the application’s traffic.


That’s it! You command the superpower of running your app without it’s dependent APIs and microservices.
