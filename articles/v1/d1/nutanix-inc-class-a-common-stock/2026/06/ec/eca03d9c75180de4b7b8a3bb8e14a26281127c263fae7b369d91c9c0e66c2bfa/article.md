---
schema_version: "1.0.0"
document_id: "eca03d9c75180de4b7b8a3bb8e14a26281127c263fae7b369d91c9c0e66c2bfa"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/06/11/automation-with-nutanix-central-2-0-v4-apis-part-2-python-sdks/"
published_at: "2026-06-11T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:a6c60f68b7a39c373c21e0800bdc2134c6e3fe21a0b9f6f8e5929067bb612eac"
---

# Automation with Nutanix Central 2.0 v4 APIs: Part 2, Python SDKs

## Introduction


In March 2026, Nutanix announced the general availability of Nutanix Central On-Premises 2.0. For detailed information, see the[Nutanix Central 2.0 release notes](https://www.google.com/url?q=https://portal.nutanix.com/page/documents/details?targetId%3DRelease-Notes-Nutanix-Central-Onprem-v2_0:onp-nc-onprem-whats-new-r.html&sa=D&source=docs&ust=1776728484749362&usg=AOvVaw2aXBhlBXClwMYrsysWmptR) .


Following that release, we published the first in a 2-part series covering the use of Nutanix v4 REST APIs to complete the following operations:


- Create a new “registered domain”: Prepare Nutanix Central for registration of a new Prism Central domain
- Complete the Prism Central to Nutanix Central registration process


Part 1 covered the entire process from end-to-end and included the following high-level steps:


- Collection of the Nutanix Central and Prism Central configuration details required for API operations


- Prism Central domain identifier: Uniquely identifies the Prism Central domain being registered
- Location identifier: the registered Prism Central domain’s location on the Global Overview Map
- Identity Provider identifier: The Nutanix IAM Identity Provider that will manage authentication (“authn”) and authorization (“authz”) for all registration steps
- Target URL: the endpoint to which Prism Central will be registered (https://nc.dev.demolab.net in our demo)
- Credential identifier: The API key and API key ID via “User of type Service Account”
- Tenant identifier: the Nutanix Central unique identifier


Part 2 of the series presents the same process but using Nutanix v4 Python SDKS instead of REST APIs. The entire script built throughout this article can be via theRelated Resources section at the end of this article.


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


### Nutanix Central


This article is not intended to serve as a how-to for deploying Nutanix Central. For this reason, the following assumptions apply:


- Prism Central 7.5.1 is deployed and updated using Nutanix Lifecycle Manager (LCM)


- This includes configuration of the Prism Central fully-qualified domain name (FQDN) and appropriately trusted SSL certificates for that FQDN


- Nutanix Central 2.0 is deployed and operational


- This includes the configuration of Nutanix Identity and Access Management (IAM) to handle session authentication and operational authorization


### Python SDKs


This series is not intended as a how-to for learning the Nutanix Python SDKs. To begin using the Nutanix v4 SDKs for the first time, see the[Nutanix API Getting Started Guide](https://www.nutanix.dev/nutanix-api-user-guide/) .


## Abbreviations


**


**


Throughout this article, the following abbreviations will be used.


- **PC** : Prism Central


- **NC** : Nutanix Central


- **` extId`** : An entity’s unique identifier e.g. PC` extId`


## Important: Nutanix Central v4 API Endpoints


Users of current Nutanix v4 Python SDKs will likely be familiar with code blocks similar to the following:


sdk_port9440.py


```text
from ntnx_prism_py_client import Configuration
md_config = Configuration()
md_config.host = "10.10.1.100"
md_config.port = "9440"
md_config.username = "admin"
md_config.password = "nutanix/4u"
md_config.verify_ssl = False
```


Specifically, note the use of **port 9440** .


When required,` multidomain` namespace configuration instances will now constructed as shown below.


**Note the use of HTTPS with default port 443** . This is not port` 9440` .


sdk_port443.py


```text
from ntnx_multidomain_py_client import Configuration
md_config = Configuration()
md_config.host = "10.10.1.100"
md_config.port = "443"
md_config.username = "admin"
md_config.password = "nutanix/4u"
md_config.verify_ssl = False
```


All requests in this article will be shown with the appropriate endpoint, as required:


- ` {{pc_ip}}` or
- ` {{nc_fqdn}}`


## Configuration: Specify Environment Details


To save time during the information collection stage, this demo uses an on-disk file named` register_pc_to_nc.json` . This simple JSON file contains information pertaining to our demo environment:


register_pc_to_nc.json


```text
{
"nc_fqdn": "nc.dev.demolab.net",
"nc_username": "administrator@ntnxlab.local",
"idp_name": "ntnxlab"
}
```


If you have cloned the[entire code samples repository](https://github.com/nutanixdev/code-samples) , this file will be present in the` python/v4api_sdk` directory. If you are following this article and building the script as you go, make sure this file exists before creating or running the` register_pc_to_nc.py` script. Edit all values so they match your environment.


Credentials are not stored in this configuration file and will be requested at runtime.


## Required Namespaces


As with the REST API approach, the SDK approach will make use of various SDK namespaces through the process:


- ` ntnx_iam_py_client` : Nutanix IAM for Identity and Access Management
- ` ntnx_prism_py_client` : Nutanix Prism for task monitoring
- ` ntnx_clustermgmt_py_client` : Cluster management for obtaining lists and details of registered clusters
- ` ntnx_multidomain_py_client` : New SDK features supporting Nutanix Central 2.0 and Nutanix Cloud Manager 2.0


- Important: Depending on the operation and endpoint, **both ports 9440 and 443** will be used by the` ntnx_multidomain_py_client` namespace


All namespaces are imported as aliases, with a simple iterable for configuration:


iterable.py


```text
# create the configuration instances
iam_config = IamConfiguration()
cluster_config = ClusterConfiguration()
prism_config = PrismConfiguration()
md_config = MDConfiguration()


# create configuration instances for IAM and ClusterMgmt
# note these use the PC endpoint and credentials
for config in [iam_config, cluster_config, prism_config]:
config.host = script_config.pc_ip
config.port = "9440"
config.username = script_config.pc_username
config.password = script_config.pc_password
config.verify_ssl = False


# create configuration instances for MultiDomain
# note this uses the NC endpoint and credentials
for config in [md_config]:
config.host = env_config["nc_fqdn"]
config.port = "443"
config.username = env_config["nc_username"]
config.password = nc_password
config.verify_ssl = False
```


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


The PC domain’s unique ID can be obtained with a **PRISM CENTRAL**` clustermgmt` request


as shown below.


This specific request uses an Enum filter and lambda expression to limit responses to those with a cluster function of


**PRISM_CENTRAL** . Prism Element (


**AOS)** clusters are excluded.


pc_id.py


```text
# create the instance of the ClusterMgmt API class
cluster_instance = ClustersApi(api_client=cluster_client)


# list PC clusters
print("Retrieving cluster list ...")
cluster_list = cluster_instance.list_clusters(
async_req=False,
_filter="config/clusterFunction/any(a:a eq Clustermgmt.Config.ClusterFunctionRef'PRISM_CENTRAL')"  # pylint: disable=line-too-long # noqa: E501
)
if cluster_list.metadata.total_available_results:
# get the PC domain's extId
pc_domain_extid = cluster_list.data[0].ext_id
print(f"Cluster of type PRISM_CENTRAL found ({pc_domain_extid}).")
```


The response from this request includes a list of matching objects. Because we limited this request to only those matching the


**PRISM_CENTRAL** cluster function, and made the request to PC itself, the list contains a single object with an` extId`


of


**27191464-8be0-404e-9387-24278596629b** .


partial_response.json


```text
{
'_object_type': 'clustermgmt.v4.config.ListClustersApiResponse',
'_reserved': {
'$fv': 'v4.r2'
},
'_unknown_fields': {},
'data': [
{
'_object_type': 'clustermgmt.v4.config.Cluster',
...
'ext_id': '27191464-8be0-404e-9387-24278596629b',
...
}
]
}


```


### Collect Location extId


All supported locations are identified by a unique` extId`


. The list of supported geographical locations can be obtained using the


multidomain


namespace APIs. In this example, we’ll be registering a PC domain located in New York City, New York.


The location’s unique ID can be obtained with a **NUTANIX CENTRAL**` multidomain` SDK request as shown below.


In this example we’re also using Odata filters to limit the results to only contain New York City locations.


list_locations.py


```text
# build the location list
md_instance = LocationsApi(api_client=md_client)
locations_list = md_instance.list_locations(
_filter="contains(name, 'New York City')"
)
```


This request must be authenticated with an NC credential e.g. an Identity Provider account with appropriate permissions.


The response from this request includes a list of matching objects. Because we limited this request to only those matching New York City, the list contains a single object with an` extId`


of


**753c39f1-e31c-48d9-82b2-5e2eb5ceff4b** .


locations_response.json


```text
{
'$dataItemDiscriminator': 'List<multidomain.v4.config.Location>',
'_object_type': 'multidomain.v4.config.ListLocationsApiResponse',
'_reserved': {
'$fv': 'v4.r2'
},
'_unknown_fields': {},
'data': [
{
'_object_type': 'multidomain.v4.config.Location',
...
'ext_id': '753c39f1-e31c-48d9-82b2-5e2eb5ceff4b',
...
}
],
```


### Collect Identity Provider (IdP) extId


The Identity Provider (IdP) is a critical component of a successful PC to NC registration. In our demo environment, both PC and NC have an Active Directory IdP configured with a name of **nutanixdemo** .


The list of configuration directory services can be obtained with a **PRISM CENTRAL**` clustermgmt` request


as shown below.


list_idp.py


```text
# create the instance of the IAM API class
iam_instance = DirectoryServicesApi(api_client=iam_client)
# list matching Identity Providers
idp_list = iam_instance.list_directory_services(
async_req=False,
_filter=f"name eq '{idp_name_filter}'"
)
```


In our demo environment’s response, the list contains a single object with an` extId` of **a7ee9399-4afc-5946-bfd5-4cee6a45fb87** .


idp_response.json


```text
{
'$dataItemDiscriminator': 'List<iam.v4.authn.DirectoryService>',
'_object_type': 'iam.v4.authn.ListDirectoryServicesApiResponse',
'_reserved': {
'$fv': 'v4.r1.b2'
},
'_unknown_fields': {},
'data': [
{
'_object_type': 'iam.v4.authn.DirectoryService',
...
'ext_id': 'a7ee9399-4afc-5946-bfd5-4cee6a45fb87',
...
}
]
}
```


## Part 2: Create Registered Domain


With the initial information collected, we can now build the payload for creating the registered domain.


This step generates the PC to NC trust certificate and API keys. It prepares NC to accept a registration request from PC in the upcoming steps.


### Build Payload


The registered domain creation payload can be created as follows.


create_registered_domain.py


```text
# build registered domain creation payload
# see the entire demo script for how RegisteredDomain was imported
domain_payload = RegisteredDomain(
name="demo_domain",
location_ext_id=location_extid,
domain_ext_id=pc_domain_extid
)
```


The registered domain creation request is handled by an


**NC API**` multidomain` as shown below.


create_registered_domain.py


```text
# attempt to create the registered domain
md_instance = RegisteredDomainsApi(api_client=md_client)
registered_domain = md_instance.create_registered_domain(
async_req=False,
body=domain_payload
)
if registered_domain.data.ext_id:
# do things here if a task id was returned
# see entire demo script
pass
else:
# do things here if no task id was returned
# this typically indicates the registered task request failed
pass
```


Similar to many Nutanix v4 API requests, this specific request’s response includes details about the associated task. However, unlike other tasks that can be queried by the` prism` namespace, these tasks must be queried differently. Additionally, Nutanix Central tasks are not yet supported by the v4 Python SDKs and must be queried by REST API. This is shown below.


check_task.sh


```text
GET https://{nc_fqdn}/api/nutanix-central/v4.0.a1/config/tasks/{task_extid}/status
```


Within the Python SDK demo, however, the entire task monitoring and checking process is handled as follows.


monitor_task.py


```text
# setup REST API request credentials
# required libraries are imported in the downloadable demo script
encoded_credentials = b64encode(
bytes(
f"{env_config['nc_username']}:{nc_password}",
encoding="ascii"
)
).decode("ascii")
auth_header = f"Basic {encoded_credentials}"


# setup the REST API request headers
headers = {
"Accept": "application/json",
"Content-Type": "application/json",
"Authorization": f"{auth_header}",
"cache-control": "no-cache",
}


# get the task details
# note that this script uses the nutanix-central REST API
# namespace for this request as this action is not yet
# supported by the Nutanix v4 SDKs
task_details_url = f"https://{env_config['nc_fqdn']}\
/api/nutanix-central/v4.0.a1/config/tasks/{task_extid}/status"
task_details = requests.request(
"GET",
task_details_url,
headers=headers,
verify=False,
timeout=10
)
```


We can then check the task status with this simple snippet:


check_task.py


```text
result = task_details.json()["status"]
if result == "FAILED":
result_message = task_details.json()["data"]["subStateInfo"]["errorDetail"]  # pylint: disable=line-too-long # noqa: E501
print("FAILED:")
print(f"    {result_message}")
print("    Exiting ...")
sys.exit()
else:
print("SUCCESSFUL, continuing ...")
```


## Part 3: Collect Additional Information


Nutanix Central is ready to accept the Prism Central registration request once the registered domain is successfully created.


This request uses the details already obtained, but also requires additional details.


### Collect Target URL, Credentials and Tenant extId


Replacing` {{extId}}`


with the PC domain ID obtained in Part 1 of this article, we can now use the` multidomain` namespace via a **NUTANIX CENTRAL** request to get the registered domain details.


get_registered_domain.py


```text
# attempt to get the newly registered domain
# use an Odata filter to restrict the request to only registered domains matching the one we already know about
print("Checking creation of registered domain ...")
new_domain = md_instance.get_registered_domain_by_id(
pc_domain_extid,
_select=f"extId = {pc_domain_extid}"
)
if new_domain.data:
print(f"Registered domain named {new_domain.data.name} found ({pc_domain_extid}).")
else:
print(f"Registered domain {pc_domain_extid} not found. Exiting ...")
```


This request will return our specific registered domain and all its associated details. Specifically, we’re interested in the target URL, the tenant’s unique ID and the associated credentials. A partial response is shown below:


additional_info.json


```text
{
'$dataItemDiscriminator': 'multidomain.v4.config.RegisteredDomain',
'_object_type': 'multidomain.v4.config.GetRegisteredDomainApiResponse,
'_reserved': {
'$fv': 'v4.r2',
'Etag': 'eab7226c471ed0bc0a2490dac35f7b3e'
},
'_unknown_fields': {},
'data': {
'_object_type': 'multidomain.v4.config.RegisteredDomain',
...
'registration_config': {
'_object_type': 'multidomain.v4.config.RegistrationConfig',
...
'credentials': {
'_object_type': 'multidomain.v4.common.RegistrationCredentials',
'_reserved': {
'$fv': 'v4.r2'
},
'_unknown_fields': {},
'api_key': 'ODlk...lZTM=',
'key_id': '03e0995f-d180-55ea-9506-3303e439f4ac'
},
...
'target_url': 'https://nc.dev.demolab.net',
'tenant_ext_id': '59d5de78-a964-5746-8c6e-677c4c7a79df'},
}
...
}
}
```


- ` targetUrl:` **https://nc.dev.demolab.net**
- ` tenantExtId` : **59d5de78-a964-5746-8c6e-677c4c7a79df**
- ` credentials` :


- ` api_key` : **0Dlk…lZTM=**
- ` key_id` : **03e0995f-d180-55ea-9506-3303e439f4ac**


We can now build the PC registration payload using these 4 properties.


### Build PC Registration Payload


The final step in this process is to send the


**PC API request** for PC to NC registration. The required fields can be obtained from the previous response as follows:


pc_registration.py


```text
# the domain registration was successful
target_url = new_domain.data.registration_config.target_url
api_key = new_domain.data.registration_config.credentials.api_key
key_id = new_domain.data.registration_config.credentials.key_id
tenant_extid = new_domain.data.registration_config.tenant_ext_id
```


The last step before PC to NC registration is to build the registration payload:


pc_registration_payload.py


```text
domain_registration_spec = LocalDomainRegistrationSpec(
domain_ext_id=pc_domain_extid,
identity_provider_ext_id=idp_extid,
target_url=target_url.replace("https://", ""),
credentials=RegistrationCredentials(
api_key=api_key,
key_id=key_id
),
tenant_ext_id=tenant_extid
)
```


Now that all required information is available and the payload built, the final PC to NC request can be sent.


Critical note: The final step in the registration process is completed via a` multidomain` namespace operation that is **sent to PRISM CENTRAL over port 9440** . Up until this point, the` md_config` instance has sent requests to the Nutanix Central endpoint over port 443. For this reason, we either need to create or reconfigure the` md_config` and` md_client` instances.


In this demo we’ll do this as follows:


reconfigure_instances.py


```text
# create configuration instances for MultiDomain
# note this step uses the PC endpoint and credentials
for config in [md_config]:
config.host = script_config.pc_ip
config.port = "9440"
config.username = script_config.pc_username
config.password = script_config.pc_password
config.verify_ssl = False
md_client = MDClient(configuration=md_config)
md_instance = LocalDomainApi(api_client=md_client)
```


pc_to_nc_registration.py


```text
# do the actual PC to NC registration
domain_registration = md_instance.register_local_domain(
async_req=False,
body=domain_registration_spec
)
```


#### Workflow Summary


This diagram shows the complete workflow from collecting information through to registering PC to NC.


### Successful Response


A successful PC to NC registration task contains all the details necessary to understand the registration status. Let’s take a look at the most important properties of a successful registration task.


registration_response.json


```text
{
"data": {
"extId": "ZXJnb24=:d312420f-6e11-45f3-4403-dae490397b47",
"operation": "Registration with Nutanix Central",
"operationDescription": "Registration with nutanix central",
...
"progressPercentage": 100,
"subSteps": [
{
"name": "API_KEY_VERIFY",
"$reserved": {
"$fv": "v4.r3"
},
"$objectType": "prism.v4.config.TaskStep"
},
{
"name": "DOMAIN_CERT_GEN"
},
{
"name": "CERT_EXCHANGE"
},
{
"name": "IAM_FEDERATION_SETUP"
},
{
"name": "TUNNEL_SETUP"
}
],
"isCancelable": false,
"lastUpdatedTime": "2026-04-09T00:48:02.894097Z",
"isBackgroundTask": false,
"numberOfSubtasks": 0,
"numberOfEntitiesAffected": 0,
"$reserved": {
"$fv": "v4.r3"
},
"$objectType": "prism.v4.config.Task",
"status": "SUCCEEDED"
},
"$reserved": {
"$fv": "v4.r3"
},
"$objectType": "prism.v4.config.GetTaskApiResponse",
"metadata": {
...
}
}
```


These are the properties that help understand exactly what happened during a successful PC domain registration.


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


Behind the scenes, the Nutanix v4 language-specific SDKs uses the same APIs used in part 1 of this series. All supported SDK languages i.e. Python, Java, JavaScript and Go, can follow an identical workflow to achieve the same result.


## Conclusion


In environments registering Prism Central to Nutanix Central at scale, the end-to-end registration process can be achieved using a combination of existing and new Prism Central and Nutanix Central APIs. You can see how the Nutanix v4 APIs simplify often complex processes and how automation of those processes can be integrated into larger automation workflows.


This concludes this 2-part series covering Prism Central to Nutanix Central registration using the Nutanix v4 Python SDKs. For part 1, covering Nutanix v4 REST APIs, see[Automation with Nutanix Central 2.0 v4 APIs: Part 1, REST APIs](https://www.nutanix.dev/2026/05/18/automation-with-nutanix-central-2-0-v4-apis-part-1-rest-apis/) .


## Related Resources


- Series Part 1:[Automation with Nutanix Central 2.0 v4 APIs: Part 1, REST APIs](https://www.nutanix.dev/2026/05/18/automation-with-nutanix-central-2-0-v4-apis-part-1-rest-apis/)
- Complete demo script on[NutanixDev GitHub](https://github.com/nutanixdev/code-samples) ; clone the repository to help ensure the` register_pc_to_nc.json` file is included in the download, then edit the file to meet your requirements.
- [Nutanix Central](https://www.nutanix.com/products/nutanixcentral)
- [Nutanix Cloud Manager](https://www.nutanix.com/products/cloud-manager)
- [Nutanix v4 API Resources](https://nutanix.dev/api-reference-v4) (including User Guide)
