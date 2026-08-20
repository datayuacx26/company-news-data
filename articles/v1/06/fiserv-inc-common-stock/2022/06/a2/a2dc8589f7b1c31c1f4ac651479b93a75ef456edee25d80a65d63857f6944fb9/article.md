---
schema_version: "1.0.0"
document_id: "a2dc8589f7b1c31c1f4ac651479b93a75ef456edee25d80a65d63857f6944fb9"
company_key: "fiserv-inc-common-stock"
company: "Fiserv Inc."
source_id: "fiserv-inc-common-stock-rss-0ed4f0fc109c"
canonical_url: "https://medium.com/fiserv-tech-blog/on-premise-connectivity-from-azure-monitor-a-serverless-approach-bcad06dcb339"
published_at: "2022-06-28T21:00:22+00:00"
first_seen_at: "2026-07-20T04:35:14.828685+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:1c96a83a027519a262e9ea6f803584d2c5e18d97a04eb8bc445918ce053e0387"
---

# On-premise connectivity from Azure Monitor: a serverless approach

Serverless


Azure


Cloudops


DevOps


# On-premise connectivity from Azure Monitor: a serverless approach


[Ganesh Venkataraman](https://medium.com/@vganeshkumar?source=post_page---byline--bcad06dcb339---------------------------------------)


5 min read


·


Jun 27, 2022


--


Press enter or click to view image in full size


[developer.fiserv.com](https://developer.fiserv.com/)


## Summary


One of the obvious benefits of open, cloud-based technologies is the ability to integrate a wide range of global services with on-premise resources. If you’ve developed for financial services for any length of time, you know this openness adds another level of complexity to development. When money movement is involved, you’re going to encounter a higher level of risk and compliance considerations.


This blog highlights just such an issue encountered by the DevStudio engineering team here at Fiserv, and how we solved it. I hope this blog helps you understand not only how our team works, but gives you useful context for implementing your own solutions that prioritize security and compliance without sacrificing speed or functionality.


## Challenge


The API portal (DevStudio) from Fiserv runs on Azure RedHat OpenShift and leverages many of Azure’s managed services, such as Cosmos DB, Redis, ACR and others. All resources run in a secure, private VNET subnets with multiple layers of WAF providing OWASP/DDOS protection.


DevStudio infrastructure and managed services are monitored by Azure Monitor using Azure Alert Manager which triggers alerts requiring manual intervention. Hosted in the Fiserv data center, Moogsoft’s observability telemetry is used for correlating and routing alerts. Moogsoft’s Azure webhook is typically used for routing alerts from Azure Alert Manager. However, connectivity to this webhook API endpoint from all public IP addresses, including Azure Alert Manager, is blocked by network policies from Fiserv.


We needed a way to securely connect Azure alert manager with on-premises Moogsoft endpoint.


## Current State


Press enter or click to view image in full size


Current State- restricted connectivity from Azure global services


Azure Monitor is a globally managed service that provides many features, including monitoring, log analytics and alerting. Azure alerts triggered from Azure Monitor have public IP addresses currently blocked by Fiserv network policies. One solution to address this connectivity issue is to route all metrics and logs to the on-premise Splunk server and leverage existing Splunk/Moogsoft integration to manage alerts.


## Anti-Pattern


Treating metrics as logs is an anti-pattern and should be avoided for the following reasons:


Press enter or click to view image in full size


Route metrics as log streams


· On-premise Splunk cluster could crash , which will result not only in losing metrics and logs, but also all alerts


· Latency involved in shipping the metric(s) to Splunk prevents responding to critical metrics in real- or near-real time


· Offloading the metric(s) to Splunk will make the entire **observability** engineering process disconnected. A better design pattern is to leverage cloud-provided services to self-heal and proactively recover at the source and at the time when an incident happens


· There are costs and effort involved in routing and storing metrics and logs in Splunk


## Solution


A simple Java Azure function can be created to process the alert from Azure and to route to the Moogsoft Internal API (Fiserv) server.


Press enter or click to view image in full size


Azure function deployed in Azure private VNET with on-premise connectivity


The Azure function can be configured to only invoke through a HTTPS trigger from Azure Alert Manager, preventing any unauthorized access to the Azure function. Azure function source code can be found here:


```text
@FunctionName("MoogSoftClient")  public HttpResponseMessage run(  @HttpTrigger(  name = "req",  methods = {HttpMethod.POST},  authLevel = AuthorizationLevel.FUNCTION)  HttpRequestMessage<Optional<String>> request,  final ExecutionContext context) {  String response = “”;  try {  //Retrieve moogsoft from Azure function configuration settngs  String moogsoftURL =  System.getenv(MOOGSOFT_URL);  //Map azure alert payload to moogsoft request schema  String moogsoftRequestStr = MoogSoftUtil.transformRequest(request.getBody().get());  //Post alert to moogsoft API  response = DevStudioHttpClient.sendPOST(moogsoftURL,moogsoftRequestStr, context);  } catch (Exception e) {  response = " { \"Status\": \"Error processing request: " + e.getMessage() + "\"}";  }  // return response document to the browser or calling client.  return request.createResponseBuilder(HttpStatus.OK)  .body(response)  .build();  }
```


· **VNET integration:** To connect to Fiserv, the function was integrated with a spoke VNET where the rest of the Azure managed services for DevStudio are currently deployed. Azure functions require a dedicated subnet for such VNET integration, and a new subnet was created for this purpose. VNET integration provides the function access to resources in the VNET, which also includes routes to on-premises infrastructure


Press enter or click to view image in full size


Azure function- VNET configuration


To access the VNET resource from the function, the following configuration setting must be added to Azure function:


· **Access restrictions** : While VNET integration provides secure access to resources in VNET, it does not provide private access to Azure function. To restrict access to only the Azure Alert Manager action group, add the following access restriction in the networking section of VNET:


Press enter or click to view image in full size


Restrict azure function access to Azure Alert manager


**TLS connection** : Connectivity to MoogSoft API requires adding the Moogsoft public certificate to Azure TLS/SSL configuration


Press enter or click to view image in full size


To access the certificate from the Azure function, add the following to Azure function configuration:


Press enter or click to view image in full size


Lastly, configure the Moogsoft URL as an application setting:


Press enter or click to view image in full size


## Conclusion


The problem could have been addressed by deploying the code sample provided in a docker container in ARO cluster. However, it adds dependency on ARO resources that could crash, potentially affecting alerting as well. Deploying the code in a separate set of load balanced VM’s was also considered, but that entails doing the heavy lifting of creating, maintaining and monitoring another component, which further increases the cost of ownership.


Azure functions are fully managed, can auto scale, support perpetual warm instances and priced based on core usage. Using Azure functions eliminates the need to maintain any infrastructure and provides a cost-effective and elegant solution to integrate with on-premises resources.
