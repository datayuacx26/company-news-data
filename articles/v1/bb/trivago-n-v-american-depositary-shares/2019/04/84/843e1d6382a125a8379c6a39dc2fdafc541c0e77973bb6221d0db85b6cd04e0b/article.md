---
schema_version: "1.0.0"
document_id: "843e1d6382a125a8379c6a39dc2fdafc541c0e77973bb6221d0db85b6cd04e0b"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2019-04-09-circuitbreakerwithawsstepfunctions/"
published_at: "2019-04-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:512c33f5ce34f99fc5711f2c8337713831ffc47642cee937deb1ce3cd1f59e8f"
---

# Circuit Breaker with AWS Step Functions

At trivago, we have several workflows which interact with external services. The health and availability of external services can have an impact on keeping our workflows alive and responsive. Think of an API call made to an external service which is down. Our workflows have to be prepared to expect these errors and adapt to it.


We have to be prepared to handle three specific anomalies in the behaviour of the external service.


- Unresponsiveness. This could mean the service is down and all calls to it fail. At this point, we wish to stop making calls to avoid errors piling up
- Throttling. We could only make a limited number of calls in a timeframe, ex: 100k calls in an hour. Here, we would like to stop making calls and resume in the next hour
- Error. This is when the service throws an unexpected error. This could be because of wrong API parameters. In this case, we would like to retry a few times and log an error


## Circuit Breaker


When building distributed systems, we have to anticipate services going down. We also have to consider the general availability and responsiveness of these external services. A common and useful pattern to handle this is the[Circuit Breaker](https://martinfowler.com/bliki/CircuitBreaker.html) . The idea is to put a component between your workflow and the external service call. When we detect the external service to have gone down, the component opens the circuit and all subsequent calls are paused. For every call to the service, a check is done against the component to see if the circuit is open or closed.


Think of the gatekeeper component as very similar to the electrical circuit breakers in buildings. However, in buildings, when the circuit breaker goes down, it needs an external intervention to reset things. But for software circuit breakers, we can have the breaker itself detect if the underlying calls are working again. We can do this by configuring a reset behaviour by an attempt to make the call the external service after a suitable interval.


## Step Functions as state machines


We use AWS lambdas heavily as components in our pipelines. Natively lambdas are meant to be stateless, which makes building complex lambda to lambda workflows difficult. But with step functions, you can model and orchestrate workflows with lambda functions. Step functions enable you to define your workflow as a state machine.


In most cases, a single state in the state machine invokes a lambda function. But you can also incorporate branching logic, error handling, wait-states or even invoke multiple states in parallel. The state machine can be configured as a JSON document and deployed on AWS.


## Handling multiple errors


A feature we use a lot in step function is a combination of a lambda function state and error handling. With error handling, we specify which state can get executed next for a specific type of Error.


Think of a simple call to an external service which can throw several exceptions, and different control flows for each of them. As we progress further, we can refine the state machine below to add more capabilities.


When we are capable of distinguishing between errors, we gain the capability of handling them separately. Some of them can be retried directly. But some of them need action before a retry, like opening the circuit to prevent further calls.


## Enabling wait and retries


If you have exhausted your quota of calls for a time period, the service can throw a throttling error. The best action here is to wait for a predefined interval and try again. You can do this using a wait state and looping back to the original state once the wait interval is complete.


You can directly specify the time to wait or it can infer this from the incoming message from the previous state.


If the service throws an unexpected error, it’s best to retry a few more time before giving up.


If no action needs to be taken other than a retry, this can be configured directly on a state retry block.


```text
"Retry": [
{
"ErrorEquals": ["RetriableError"],
"IntervalSeconds": 1,
"MaxAttempts": 2,
"BackoffRate": 2.0
},
{
"ErrorEquals": ["NotAvailableError"],
"IntervalSeconds": 30,
"MaxAttempts": 2,
"BackoffRate": 2.0
}
]
```


## Storing the state and opening the circuit


Especially in the case of a throttling error, you are absolutely sure no further calls will succeed for a while. Before the request enters the retry phase, the circuit has to be opened to avoid further calls. The` HandleThrottledException` state can open the circuit. We use an external state store to persist the state of the system. Before reaching the` ServiceEntrypoint` state which talks to the external service, we check the state store to see if the circuit is closed or open. If it’s open, the request is moved into a waiting queue. We use DynamoDB as the state store. AWS Elasticache or even an external Redis cluster can be used for this.


When the original request which keeps on retrying until it succeeds, it resets the flag in DynamoDB state store, thereby closing the circuit and allowing all the further calls to the service.


## Dead letter queues


For calls which resulted in errors from the external service, even after multiple retries, it’s a good practice to move them into a[Dead Letter Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) . We think it’s better than throwing away those requests as this allows us to have a look at those requests and try them again in the future. This also makes sure, if we end up having many errors due to a bug in the pipeline. We use SQS for maintaining the Dead Letter Queue, and periodically, this is inspected and evaluated and even retried if some conditions are met.


## Next Steps


We feel it’s really helpful to be in total control of how your system interacts with external services. Especially, in the case of throttling errors, not handling it properly can lead to your pipelines throwing repeated errors. We continually improve our pipelines to make it more resilient and stable.
