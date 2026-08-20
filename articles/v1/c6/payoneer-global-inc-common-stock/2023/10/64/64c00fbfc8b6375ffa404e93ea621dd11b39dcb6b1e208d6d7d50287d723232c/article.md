---
schema_version: "1.0.0"
document_id: "64c00fbfc8b6375ffa404e93ea621dd11b39dcb6b1e208d6d7d50287d723232c"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/how-to-use-asynclocal-for-cross-service-logging-5a6d75dfeb81"
published_at: "2023-10-03T09:42:29+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:de7642cc661b39dd26085ea11c58ea03ed93894ef639b530456826cad0fcffea"
---

# How to use AsyncLocal for cross-service logging

# How to use AsyncLocal for cross-service logging


[Michael Berezin](https://medium.com/@mbearz?source=post_page---byline--5a6d75dfeb81---------------------------------------)


7 min read


·


Oct 3, 2023


--


Press enter or click to view image in full size


In our modern world, more and more companies are adopting a microservice architecture for their product.


While microservices offer better ways to develop, deploy and scale, they introduce complexity in other areas, such as in diagnostics, monitoring, and troubleshooting in a distributed system.


In this article, we will explore a solution for dealing with logging in the microservices world.


Let's assume that we have 3 services:


*Users Service,* that calls *Roles Service,* that calls *Permissions Service.*


In this scenario, when we call *Users Service* we get a general server error, and we need to find out what is causing the issue. The first thing to do is look at the logs, but which logs? The error could come from *Users* , *Roles,* or *Permissions* .


All 3 services are separate applications that write their own logs. We can look at the logs for *Roles Service* and see errors, but how do we correlate these errors to the call we just made in *Users Service?*


We need a way to correlate log entries across multiple services and to do it in a simple way that will not make our fellow developers do extra work.


## **How to pass data “behind the** scenes” **?**


Conceptually, adding some kind of correlation ID is not hard: the first service generates some random number and sends it to the second service, that in turn sends it to the third, and so on, and each service adds this correlation ID to the logs that it writes. But in practice, this is not so simple as it requires each method in our service to be able to get this correlation ID as a parameter to pass on. It’s a lot of repetitive extra work, and this method is cluttering our code and method signatures.


**AsyncLocal to the rescue**


The older .NET framework actually has something that can do exactly this:[CallContext](https://learn.microsoft.com/en-us/dotnet/api/system.runtime.remoting.messaging.callcontext?view=netframework-4.8.1) . It “Provides a set of properties that are carried with the execution code path.” This is what we want to do, but it is not supported in .NET Core.


Luckily for us, some kind souls on the internet have[figured out a way to replicate this](https://www.cazzulino.com/callcontext-netstandard-netcore.html) *by using AsyncLocal.*


**What is AsyncLocal?**


According to the Microsoft[documentation](https://learn.microsoft.com/en-us/dotnet/api/system.threading.asynclocal-1) , AsyncLocal:


> Represents ambient data that is local to a given asynchronous control flow, such as an asynchronous method.


Let's see how we can use it:


```text
public static class CallContext  {      static ConcurrentDictionary<string, AsyncLocal<object>> state =                 new ConcurrentDictionary<string, AsyncLocal<object>>();       public static void SetData(string name, object data) =>          state.GetOrAdd(name, _ => new AsyncLocal<object>()).Value = data;       public static object GetData(string name) =>          state.TryGetValue(name, out AsyncLocal<object> data) ?                                                 data.Value : null;  }
```


Now we *CallContext* class that we can use in .NET Framework and .NET Core.


**How does that help us get cross-service logs?**


Press enter or click to view image in full size


Before getting into the technical stuff of how to implement logging across multiple services, let’s describe the solution's general idea:


**The general idea**


1. **Middleware**


The middleware (aka *ContextMiddleware* ) job will be to retrieve the logging data from the request and store it using AsyncLocal so we can use it later on.


## Get Michael Berezin’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


In the case of the first service, no information is being passed, so we will create our own logging information and store it.


**2. Logger**


Now that we have the logging information stored at CallContext, every call to our logging mechanism will **extract** it from CallContext and **add** it to our logs.


**3. HttpClient wrapper**


The HttpClient wrapper’s job is to get all of the logging information from the *CallContext* and add it as request headers to all the outgoing requests to other services. In turn, this facilitates the Http Target service middleware with the logging information.


**Note** : For every HttpClient request, we will generate a new *SpanId.*


Now that we have a grasp of what we want to accomplish, let’s dive into the technical stuff.


**The technical stuff**


First, we need some correlation ID that identifies the whole flow so we can correlate the log entries, and I will call it *FlowId.* When our system grows to more than 3 services we will have more complex flows, so we will need a bit more data to understand how our flow works.


We can borrow some ideas from *OpenTelemetry* and use span and parent IDs.


**Note:** If you think about the flow as a call graph, span ID identifies the current level in the graph, and parent ID identifies the previous level.)


So now we need to create a class that will manage the flow, parent, and span IDs and store them in the CallContext.


```text
public static class Flow  {      public const string FlowIdName = "flowId";      public const string SpanIdName = "spanId";      public const string ParentIdName = "parentId";         public static string CreateFlowId()      {          return Guid.NewGuid().ToString("N");      }         public static string CreateSpanId()      {          return Guid.NewGuid().ToString("N").Substring(0,16);      }         public static (string flowId, string? parentId, string spanId) SetContext(string? flowId = null, string? parentId = null)      {          flowId ??= CreateFlowId();          var spanId = CreateSpanId();          CallContext.SetData(FlowIdName, flowId);          CallContext.SetData(SpanIdName, spanId);             if (parentId != null)          {              CallContext.SetData(ParentIdName, parentId);          }          return (flowId, parentId, spanId);      }         public static (string? flowId, string? parentId, string? spanId) GetContext()      {          return (              CallContext.GetData(FlowIdName)?.ToString(),              CallContext.GetData(ParentIdName)?.ToString(),              CallContext.GetData(SpanIdName)?.ToString()              );      }         public static Dictionary<string, string> GetContextAsDictionary()      {          var (contextFlowId, contextParentId, contextSpanId) = GetContext();          return new Dictionary<string, string>          {              {FlowIdName,contextFlowId},              {ParentIdName,contextParentId},              {SpanIdName,contextSpanId}          }.Where(d => d.Value != null)          .ToDictionary(k => k.Key, v => v.Value);      }  }
```


For that to make more sense, let's see how we can use this class in a middleware:


```text
public class ContextMiddleware  {      private readonly RequestDelegate _next;         public ContextMiddleware(RequestDelegate next)      {          _next = next;      }         public async Task InvokeAsync(HttpContext context)      {          context.Request.Headers.TryGetValue(Flow.FlowIdName, out var flowId);          context.Request.Headers.TryGetValue(Flow.SpanIdName, out var spanId);          //the span id of the previues service is now the parent id           Flow.SetContext(flowId, parentId: spanId);             //Adds a delegate to be invoked just before response          //headers will be sent to the client.          context.Response.OnStarting(state =>          {              foreach (var item in Flow.GetContextAsDictionary())              {                  context.Response.Headers.Add(item.Key, item.Value);              }                 return Task.CompletedTask;          }, context);             // Call the next delegate/middleware in the pipeline.          await _next(context);      }  }
```


Using middleware, we make sure for that every request we get the flow and *spanId* from the previous request and save them to the flow context (if this is the first request, then the flow context will generate the IDs).


We also make sure that we will return the IDs in the response headers. This is useful for the first service so it can return the *flowId* to the client.


Having the IDs in the flow context means that we can use them in 2 places:


1. Add the IDs to the logs to make sure that all of our logs have the flow ID (in a real-world implementation we would add the flow ID as a **tag** and not as part of the message).


```text
public interface ICustomLogger  {     public void Log(string message);  }   public class CustomLogger : ICustomLogger  {     public void Log(string message)     {         var (contextFlowId, contextParentId, contextSpanId) = Flow.GetContext();         //in the real world this would write to real logs         Trace.WriteLine($"[{contextFlowId}_{contextParentId}_{contextSpanId}] {message}");     }  }
```


2. Send the IDs to the next service in line (look at the *ExecuteAsync* method).


```text
public static class HttpUtils  {    public static async Task<TOut> Get<TOut>(string url, Dictionary<string, string>? headers = null) =>        await ExecuteAsync<TOut>(url, null, HttpMethod.Get, headers);     public static Task<TOut> Post<TIn, TOut>(string url, TIn obj, Dictionary<string, string>? headers = null) =>        ExecuteAsync<TOut>(url, JsonConvert.SerializeObject(obj), HttpMethod.Post, headers);     public static Task<TOut> Put<TIn, TOut>(string url, TIn obj, Dictionary<string, string>? headers = null) =>        ExecuteAsync<TOut>(url, JsonConvert.SerializeObject(obj), HttpMethod.Put, headers);     public static Task<JObject> Delete(string url, Dictionary<string, string>? headers = null) =>        ExecuteAsync<JObject>(url, null, HttpMethod.Put, headers);     public static async Task<TOut> ExecuteAsync<TOut>(string url, string? json,        HttpMethod method, Dictionary<string, string>? headers = null, string contentType = "application/json")    {        //add the flow context to the request headers        headers ??= new Dictionary<string, string>();        foreach (var item in Flow.GetContextAsDictionary())        {            headers.Add(item.Key, item.Value);        }         var request = CreateRequestToBeSent(url, json, method, headers, contentType);         var response = await HttpClientFactory.Create().SendAsync(request);         if (response.IsSuccessStatusCode)        {            return await response.Content.ReadAsAsync<TOut>();        }         throw new Exception($"Status code: {response.StatusCode}, reason: {response.ReasonPhrase}");    }     private static HttpRequestMessage CreateRequestToBeSent(string endpoint, string? json,        HttpMethod method, Dictionary<string, string> headers, string contentType = "application/json")    {        var request = new HttpRequestMessage        {            RequestUri = new Uri(endpoint),            Method = method,            Content = json != null ? new StringContent(json, Encoding.UTF8, contentType) : null        };         foreach (var item in headers)        {            request.Headers.Add(item.Key, item.Value);        }         return request;    }  }
```


Now we can be sure that all our services will get the *flowId* and add them to the logs without us having to manually pass the flow and span IDs.


We can also be sure that all our services will get the same IDs and have a good correlation between our logs.


**Note** : If you are using Kafka or RabbitMQ to pass messages between your services instead of Http, you can use the same concept. Both Kafka and RabbitMQ support message headers that we can use to pass the flow ID.


You can see the full code here:[https://github.com/mberaz/FlowContext](https://github.com/mberaz/FlowContext)
