---
schema_version: "1.0.0"
document_id: "109e0f3196aeb50332996ec62ad9ad020d504962e4fdbecf8c2654409f961a1d"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/05/18/automation-with-nutanix-central-2-0-v4-apis-part-1-rest-apis/"
published_at: "2026-05-18T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:3123467af847db88c739d6f431ace2bb955ba831c6a38b2d81e7d57dd9711638"
---

# Automation with Nutanix Central 2.0 v4 APIs: Part 1, REST APIs

## Introduction


In March 2026, Nutanix announced the general availability of Nutanix Central On-Premises 2.0. For detailed information, see the[Nutanix Central 2.0 release notes](https://www.google.com/url?q=https://portal.nutanix.com/page/documents/details?targetId%3DRelease-Notes-Nutanix-Central-Onprem-v2_0:onp-nc-onprem-whats-new-r.html&sa=D&source=docs&ust=1776728484749362&usg=AOvVaw2aXBhlBXClwMYrsysWmptR) .


The Nutanix Central 2.0 release makes the v4 APIs generally available and improves operational efficiency by enabling environment configuration of Nutanix Central using automated approaches.


The domain registration process in Nutanix Central on-premises involves providing the Prism Central domain details, then generating API credentials in Nutanix Central and adding/uploading the API credentials in Prism Central to complete the registration. After the domain registration process succeeds, an encrypted, mutually trusted connection is established for Nutanix Central on-premises to communicate to the registered Prism Central domains.


In today’s article, we’ll walk through how environments needing to register multiple Prism Central instances to Nutanix Central can achieve their goal in an automated provisioning workflow.


For detailed assumptions, abbreviations, and environment configuration, expand the relevant section below.


## Environment


**


**


This article was developed using the following software versions. All versions are GA (Generally Available) at the time of publication.


- Nutanix Central 2.0


- Prism Central 7.5.1


- AOS 7.5.1


This simplified diagram shows the relative position of these components.


PC to NC APIs - Environment Components


## Assumptions


**


**


This article is not intended to serve as a how-to for deploying Nutanix Central. For this reason, the following assumptions apply:


- Prism Central 7.5.1 is deployed and updated using Nutanix Lifecycle Manager (LCM)


- This includes configuration of the Prism Central fully-qualified domain name (FQDN) and appropriately trusted SSL certificates for that FQDN


- Nutanix Central 2.0 is deployed and operational


- This includes the configuration of Nutanix Identity and Access Management (IAM) to handle session authentication and operational authorization


## Abbreviations


**


**


Throughout this article, the following abbreviations will be used.


- **PC** : Prism Central


- **NC** : Nutanix Central


- **` extId`** : An entity’s unique identifier e.g. PC` extId`


## Important: Nutanix Central v4 API Endpoints


Consumers of current Nutanix v4 APIs will likely be familiar with URLs using the following construct:


pc_apis.sh


```text
https://{{pc_ip}}:9440/api/ ...
```


The Nutanix Central APIs are new and must be accessed through the Nutanix Central endpoint. In our example, the Nutanix Central endpoint is` https://nc .dev.demolab.net`


; this endpoint will be used for a number of the API requests in this article.


Request URLs will now begin as shown below.


**Note the use of HTTPS with a default port 443** . This is not port` 9440` .


nc_apis.sh


```text
https://nc.dev.demolab.net/api/ ...
```


All requests in this article will be shown with the appropriate endpoint, as required:


- ` {{pc_ip}}` or
- ` {{nc_fqdn}}`


## Part 1: Collect Initial Information


Here are the steps required to register a Prism Central domain with Nutanix Central 2.0.


1. Using Prism APIs, collect the required information from Prism Central that is needed for Nutanix Central registration. This includes the Prism Central unique ID, Identity Provider (IdP) unique IQ and the PC domain’s location ID as listed below.


2. Create a Prism Central domain registration in Nutanix Central to generate trust certificate and API keys
3. Complete the registration of the Prism Central domain with Nutanix Central through the registration request


Importantly, in the context of a Prism Central registration, a “domain” is synonymous with a Prism Central instance and should not be confused with a DNS domain.


### List of required information


For this process to succeed, the following information is required. All items listed here are available via API, each of which will be outlined in upcoming sections.


1. **PC domain extId** : The unique identifier for the PC instance being registered to NC


2. **Location extId** : In the context of a PC to NC registration, the “location” is the geographical representation of this domain on the NC Global Overview map


3. **Identity Provider (IdP) extId** : the common identity provider that is configured on both PC and NC


4. **Target URL** : The NC FQDN to which PC will create the connection


5. **Credentials** : The API key and API key ID for authenticating the connection between PC and NC


6. **Tenant extId** : Unique identifier for the NC instance


#### Information provided by Prism Central


This diagram shows the required information provided by Prism Central.


PC to NC APIs - PC Components


#### Information provided by Nutanix Central


This diagram shows the required information provided by Nutanix Central.


PC to NC APIs - NC Components


### Collect Prism Central Domain unique ID


Every Prism Central instance, or domain, is identified by an` extId`


i.e. a unique identifier. The PC ID is a UUID-formatted string and, in this instance, will associate the domain with the requested PC registration.


The PC domain’s unique ID can be obtained with a


**PC API request** as shown below.


This specific request uses an Enum filter and lambda expression to limit responses to those with a cluster function of


**PRISM_CENTRAL** . Prism Element (


**AOS)** clusters are excluded.


pc_id.sh


```text
https://{{pc_ip}}:9440/api/clustermgmt/v4.2/config/clusters?$filter=config/clusterFunction/any(a:a eq Clustermgmt.Config.ClusterFunctionRef'PRISM_CENTRAL')
```


The response from this request includes a list of matching objects. Because we limited this request to only those matching the


**PRISM_CENTRAL** cluster function, and made the request to PC itself, the list contains a single object with an` extId`


of


**27191464-8be0-404e-9387-24278596629b** .


partial_response.json


```text
{
"data": [
{
"$reserved": {
"$fv": "v4.r2"
},
"$objectType": "clustermgmt.v4.config.Cluster",
"extId": "27191464-8be0-404e-9387-24278596629b",
"name": "pc",
"nodes": {
...
}
...
]
...
}
```


### Collect Location extId


All supported locations are identified by a unique` extId`


. The list of supported geographical locations can be obtained using the


multidomain


namespace APIs. In this example, we’ll be registering a PC domain located in New York City, New York. This location’s` extId`


can be obtained by making an


**NC API request** as follows. In this example we’re also using Odata filters to limit the results to only contain New York City locations.


list_locations.sh


```text
GET https://{{nc_fqdn}}/api/multidomain/v4.3/config/locations?$filter=contains(name, 'New York City')
```


This request must be authenticated with an NC credential e.g. an Identity Provider account with appropriate permissions.


The response from this request includes a list of matching objects. Because we limited this request to only those matching New York City, the list contains a single object with an` extId`


of


**753c39f1-e31c-48d9-82b2-5e2eb5ceff4b** .


locations_response.json


```text
{
"$objectType": "multidomain.v4.config.Location",
"$reserved": {
"$fv": "v4.r2"
},
"coordinates": {
"$objectType": "multidomain.v4.config.GeographicCoordinates",
"$reserved": {
"$fv": "v4.r2"
},
"latitude": 40.7127281,
"longitude": -74.0060152
},
"country": "United States",
"extId": "753c39f1-e31c-48d9-82b2-5e2eb5ceff4b",
"name": "New York City",
"state": "New York"
}
```


### Collect Identity Provider (IdP) extId


The Identity Provider (IdP) is a critical component of a successful PC to NC registration. In our demo environment, both PC and NC have an Active Directory IdP configured with a name of **nutanixdemo** .


We can obtain a list of the configured directory services with a


**PC API request** as shown below.


list_idp.sh


```text
GET https://{{pc_ip}}:9440/api/iam/v4.0/authn/directory-services
```


In our demo environment’s response, the list contains a single object with an` extId` of **a7ee9399-4afc-5946-bfd5-4cee6a45fb87** .


idp_response.json


```text
{
"$objectType": "iam.v4.authn.DirectoryService",
"$reserved": {
"$fv": "v4.r1.b2"
},
"createdBy": "00000000-0000-0000-0000-000000000000",
"createdTime": "2026-03-05T04:45:01.203354Z",
"directoryType": "ACTIVE_DIRECTORY",
"domainName": "nutanixdemo.com",
"extId": "a7ee9399-4afc-5946-bfd5-4cee6a45fb87",
"groupSearchType": "NON_RECURSIVE",
"lastUpdatedTime": "2026-03-05T04:45:01.203354Z",
"name": "nutanixdemo",
"serviceAccount": {
"$objectType": "iam.v4.authn.DsServiceAccount",
"$reserved": {
"$fv": "v4.r1.b2"
},
"password": "****",
"username": "administrator@nutanixdemo.com"
},
"tenantId": "59d5de78-a964-5746-8c6e-677c4c7a79df",
"url": "[ldap_url]"
}
```


## Part 2: Create Registered Domain


With the initial information collected, we can now build the payload for creating the registered domain.


This step generates the PC to NC trust certificate and API keys. It prepares NC to accept a registration request from PC in the upcoming steps.


### Build Payload


The registered domain creation request is handled by an


**NC API request** as shown below.


create_registered_domain.sh


```text
POST https://{{nc_fqdn}}/api/multidomain/v4.3/config/registered-domains
```


This request is accompanied with a payload that uses the information collected in previous steps:


create_registered_domain.json


```text
{
"name": "registered_domain_name",
"locationExtId": "753c39f1-e31c-48d9-82b2-5e2eb5ceff4b",
"domainExtId": "27191464-8be0-404e-9387-24278596629b"
}
```


#### Nutanix v4 API Headers


Most Nutanix v4 API POST requests require a header named


Ntnx-Request-Id


. This header uniquely identifies the specific request and ensures request idempotency in the event an identical request is sent later.


The` Ntnx-Request-Id`


header is a UUID-formatted string, for example:


**4f56ff55-4903-433a-a040-a77534be98f6**


Similar to many Nutanix v4 API requests, this specific request’s response includes details about the associated task. The task’s` extId`


can be used via the` prism`


namespace to get task details such as task result, progress and status.


In the context of a wider automation workflow, task details are critical in order for the script or app to know when next steps can proceed.


An example of the task-related response is shown here; for the remainder of this article this will be referred to as “task response”. In this example, note the


**extId** property.


**Important note** : This release does not include Task APIs for retrieving operation status.[Prism Central 7.5.1 v4 API Release Notes.](https://portal.nutanix.com/page/documents/details?targetId=Release-Notes-v4-API:top-release-notes-v4-pc-7.5.1-r.html)


task_response.html


```text
{
"$dataItemDiscriminator": "prism.v4.config.TaskReference",
"$objectType": "multidomain.v4.config.CreateRegisteredDomainApiResponse",
"$reserved": {
"$fv": "v4.r2"
},
"data": {
"$objectType": "prism.v4.config.TaskReference",
"$reserved": {
"$fv": "v4.r0"
},
"extId": "32e5fb13-5bd0-42da-ae8e-05c49bd0af4e"
}
}
```


## Part 3: Collect Additional Information


Nutanix Central is ready to accept the Prism Central registration request once the registered domain is successfully created.


This request uses the details already obtained, but also requires additional details.


### Collect Target URL, Credentials and Tenant extId


Replacing` {{extId}}`


with the PC domain ID obtained in Part 1 of this article, the response from our demo environment contains the details of the registered domain. The response contains a single object with the following details:


get_registered_domain.sh


```text
GET https://{{nc_fqdn}}/api/multidomain/v4.3/config/registered-domains/{{extId}}
```


Replacing` {{extId}}` with the PC domain ID obtained earlier, the response from our demo environment contains the details of the registered domain. The response contains a single object with the following details:


- ` targetUrl:` **https://nc.dev.demolab.net**
- ` tenantExtId` : **59d5de78-a964-5746-8c6e-677c4c7a79df**


#### Credentials


In addition to the` targetUrl` and` tenantExtId` properties, we can also see an object named` credentials` :


credentials.json


```text
"credentials": {
"$objectType": "multidomain.v4.common.RegistrationCredentials",
"$reserved": {
"$fv": "v4.r2"
},
"apiKey": "ODlk ... ZTM=",
"keyId": "03e0  ... f4ac"
},
```


Build the PC registration payload using these 4 properties.


### Build PC Registration Payload


The final step in this process is to send the


**PC API request** for PC to NC registration. Being a POST request, this will be accompanied with a JSON payload, starting with a URL as shown below. As with other POST requests, a unique` Ntnx-Request-Id`


header value is also required.


pc_registration.sh


```text
POST https://{{pc_ip}}:9440/api/multidomain/v4.3/management/local-domain/$actions/register
```


Payload:


pc_registration_payload.json


```text
{
"domainExtId": "27191464-8be0-404e-9387-24278596629b",
"identityProviderExtId": "a7ee9399-4afc-5946-bfd5-4cee6a45fb87",
"targetUrl": "nc.dev.demolab.net",
"credentials": {
"apiKey": "ODlk ... ZTM=",
"keyId": "03e0 ... f4ac"
},
"tenantExtId": "59d5de78-a964-5746-8c6e-677c4c7a79df"
}
```


When sent and successful, the response will be a task response, similar to the example shown earlier. Use the task` extId`


to monitor the registration process for status, progress and completion details.


#### Workflow Summary


This diagram shows the complete workflow from collecting information through to registering PC to NC.


PC to NC APIs - End-to-end Request Workflow


### Successful Response


A successful PC to NC registration task contains all the details necessary to understand the registration status. The example shown below is the` data`


object of a PC to NC registration task.


Note the following properties:


- operatationDescription


:


**Registration with Nutanix Central**
- progressPercentage


:


**100**
- subSteps


- ` API_KEY_VERIFY`


- status: **SUCCEEDED**


## Nutanix v4 SDKs


The Nutanix v4 language-specific SDKs are backed by the same v4 APIs used in this article. All supported SDK languages i.e. Python, Java, JavaScript and Go, can follow an identical workflow to achieve the same result.


Part 2 in this series will provide a demonstration of this workflow using the Python SDK that can then be translated into similar workflows for the other supported SDK languages.


## Conclusion


In environments registering Prism Central to Nutanix Central at scale, the end-to-end registration process can be achieved using a combination of existing and new Prism Central and Nutanix Central APIs. You can see how the Nutanix v4 APIs simplify often complex processes and how automation of those processes can be integrated into larger automation workflows.


## Related Resources


- [Nutanix Central](https://www.nutanix.com/products/nutanixcentral)
- [Nutanix Cloud Manager](https://www.nutanix.com/products/cloud-manager)
- [Nutanix v4 API Resources](https://nutanix.dev/api-reference-v4) (including User Guide)
