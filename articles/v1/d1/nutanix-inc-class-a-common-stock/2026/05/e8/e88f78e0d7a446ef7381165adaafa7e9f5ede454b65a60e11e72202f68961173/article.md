---
schema_version: "1.0.0"
document_id: "e88f78e0d7a446ef7381165adaafa7e9f5ede454b65a60e11e72202f68961173"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/05/04/nutanix-objects-storage-for-developers-nutanix-python-sdk-operations-part-1/"
published_at: "2026-05-04T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:46034e167a6947e322af1b215a0978e56393a57dc07de1e6dfe99a08b32e66e8"
---

# Nutanix Objects Storage for Developers: Nutanix Python SDK Operations (Part 1)

## Introduction


The Nutanix v4 APIs and SDKs can provide developers and automation teams with a significant productivity boost. This boost comes directly from the ability to easily manage a number of the Nutanix Prism UI operations from scripts and custom applications.


This 2-part series focuses on the programmatic productivity boosts found when automating common Nutanix Objects Storage operations with the Nutanix v4 Python SDK and AWS` boto3` . Part 1 creates a new Object Store, a new Bucket and configured appropriate credentials for bucket access.


In part 2, we’ll configure a simple Bucket policy that allows users to upload and browse objects via HTTP. This is a common requirement for web applications accessing files via object storage.


## Scenario


For today’s demo and code sample, the following requirements must be met.


- Use READ operations to check for and verify the configuration of existing Object Stores and Buckets
- Use READ operations to check for the existence of potential user account “conflicts”
- Creation of Nutanix Objects Storage Object Stores and Buckets; part 2 in the series will cover the creation of an S3-compliant Bucket policy
- Use READ operations to check for existing user accounts
- Create a new user account of type Service Account
- Create a new API key of type` OBJECT_KEY` for use with the specified Service Account


### Detailed Script Workflow


The demo script distributed with part 1 in this series will step through the following detailed process.


- Collect Prism Central account password
- List all AOS clusters registered to the specified Prism Central instance
- List all subnets configured in the selected Prism Element cluster
- Collect the destination cluster and subnet name for use with the new Object Store and Bucket
- Collect new Object Store and Bucket names from the user
- Collect the name for the new user of type Service Account
- Collect network details for use during Object Store creation
- Check for existing Object Stores with the same name as provided
- Create the new Object Store
- Check for existing user accounts with the same name as provided


- If a matching user account is found, ask if the user wants to continue;


- “Yes” will create an API key of type` OBJECT_KEY` for use with the Nutanix Objects Browser


- The new key will be assigned to the **existing** user of type Service Account


- “No” will exit the demo


- If no matching user account is found, create a new user of type Service Account


- Create an API key of type` OBJECT_KEY` for use with the Nutanix Objects Browser.
- The new key will be assigned to the **new** user of type Service Account


- Check for existing buckets with the same name as provided


- If a matching bucket is found, exit the demo;
- If no matching bucket is found, create the bucket


## Demo Environment


This demo and the accompanying demo script were developed using the following component versions:


- Nutanix Prism Central **v7.5.0.5**
- Nutanix AOS **v7.5.0.5**
- Nutanix Objects Service and Objects Manager **v5.3.0.1**
- ` ntnx_objects_py_client` SDK **v4.0.2**
- AWS` boto3` **1.42.59**


## Assumptions


This article and the accompanying demo script are not intended as a “learn Python” guide and, as such, some software development or scripting knowledge is beneficial.


For info on getting started with the Nutanix v4 APIs as a general topic, see the[Nutanix v4 API User Guide](https://www.nutanix.dev/nutanix-api-user-guide/) .


## The Demo & Script


The workflow has been outlined in detail above so we can keep this article as short and to-the-point as possible.


Each small code block represents a single action carried out during the script workflow.


### Step 1: Collect registered cluster and subnet details


#### Step 1.1: Cluster


First, we must know the` extId` for the destination cluster. Nutanix v4 API Odata filters allow this query to include only AOS clusters and will explicitly exclude Prism Central clusters.


```text
# use an Odata filter to retrieve a list of clusters, filtered by AOS clusters ONLY
# in the context of this query, the valid cluster function values are 'AOS' and 'PRISM_CENTRAL'
print("Retrieving cluster list ...")
cluster_list = cluster_instance.list_clusters(
async_req=False,
_filter="config/clusterFunction/any(a:a eq Clustermgmt.Config.ClusterFunctionRef'AOS')",
)


# request the user to enter the name of the destination cluster
# based on the response, get the extId of the cluster


cluster_ext_id = matches[0]["ext_id"]
```


#### Step 1.2: Subnet


This request does not require a filter and simply requests a list of all subnets.


Important note: In large environments with a large number of subnets, a query limit can be used.


##### Subnet List: Without filter


```text
print("\nRetrieving subnet list ...")
subnets_list = networking_instance.list_subnets(
async_req=False
)


# request the user to enter the subnet name
# based on the response, get the extId of the subnet


# get the subnet ext_id
subnet_ext_id = matches[0]["ext_id"]
```


##### Subnet List: With filter


The same query but with a query limit filter (not required for this demo):


```text
print("\nRetrieving subnet list ...")
# list subnets with query limit filter
# the filter value can be in the range 1-100
subnets_list = networking_instance.list_subnets(
async_req=False,
_limit=100
)
```


### Step 2: Collect names for the new Object Store and Bucket


Because Object Store and Bucket names must follow specific naming conventions, use regular expression searches to verify compliance.


#### Object Store Naming Conventions


- Be unique across all existing object store names in Nutanix Objects.
- Begin with a letter, and end with a letter or number.
- Can contain alphanumeric or hyphen characters.
- Not contain any special character other than a hyphen.
- Minimum of 1 and a maximum of 16 characters long.


#### Bucket Naming Conventions


- Be a unique DNS compliant name within a deployed bucket instance.
- Can contain alphanumeric, dot, or hyphen characters.
- Begin with a lowercase letter or a number.
- Cannot contain uppercase and special characters.
- Minimum of 3 and a maximum of 64 characters long.


```text
# collect details for use in upcoming steps
store_pattern = r"^(?=.{1,16}$)[A-Za-z](?:[A-Za-z0-9-]*[A-Za-z0-9])?$"
store_name = ""
print("Requesting compliant Object Store name ...")
while not re.search(store_pattern, store_name):
store_name = input("\nEnter the name of the new Object Store: ")
print("Object Store name meets requirements, continuing ...")


bucket_pattern = r"^[a-z][a-z0-9.-]{1,62}[a-z0-9]$"
bucket_name = ""
print("Requesting compliant Bucket name ...")
while not re.search(bucket_pattern, bucket_name):
bucket_name = input("\nEnter a name for the new bucket: ")
print("Bucket name meets requirements, continuing ...")
```


### Step 3: Check for existing Object Store names


To avoid conflicts in Object Store names, run a quick search to see if the entered name is already used.


Similar to the cluster list request, this request uses an Odata filter to limit the query to Object Stores matching the entered name only.


```text
store_list = objects_instance.list_objectstores(
async_req=False,
_filter=f"name eq '{store_name}'"
)


if not store_list.metadata.total_available_results == 0:
print("(1) matching Object Store found, retrieving configuration ...")


# get the first public IP of the existing store
# this will be used as the endpoint for the upcoming boto3 operations
public_ip = store_list.data[0].public_network_ips[0].ipv4.value
print(f"Object Store's first public IP: {public_ip}")
else:
# no matching Object Stores found
print(
"(0) matching Object Stores found, continuing with Object Store creation ..."
)
```


### Step 4: Request network details and display Object Store summary


The last configuration details required from the user are the IP addresses used for the Object Store:


- External IP
- DNS IP
- Storage network IP


Once all details are collected and as a final step, display a summary for the user to verify, then, if they continue, create the Object Store.


Note: This section is only relevant when a matching Object Store has **not** been found. During the Object Store creation process, some small default values have been used.Critically, to run this script in your own environment **ensure these settings match your configuration** e.g. Prism Central domain name, set to` msp.cluster.local` in this demo.


```text
storage_vip = input(
"Enter the storage network's VIP (must be outside the DHCP range): "
)
dns_ip = input(
"Enter the storage network's DNS IP (must be outside the DHCP range): "
)
public_ip = input(
"Enter the public network IP (must be outside the DHCP range): "
)


worker_nodes = 1
domain_name = "msp.cluster.local"
capacity = 100


print("\nSummary of new Object Store:")
print(f"    Name: {store_name}")
print(f"    Cluster name: {expected_cluster_name}")
print(f"    Subnet name: {expected_subnet_name}")
print(f"    Storage VIP: {storage_vip}")
print(f"    DNS IP: {dns_ip}")
print(f"    Public IP: {public_ip}")
print(f"    Domain: {domain_name}")
print("    Number of Worker Nodes: 1")
print("    Object Store size: 100GiB")
```


### Step 5: Create Object Store


If a matching Object Store was not found, the new Object Store can now be created.


This process requires building the new Object Store request payload and inserting the values collected earlier in the script.


Note: This block uses SDK models such as` ObjectStore` . This has been aliased in the demo script as follows:


```text
# alias the ObjectStore import
from ntnx_objects_py_client.models.objects.v4.config.ObjectStore import ObjectStore
```


This example uses constructor initialization; the same results can be achieved with post-instantiation attribute assignment, if that is your preferred approach.


```text
if create_store:
# create a new objects store
# build the payload that will create the new Object Store
new_store = ObjectStore(
name=store_name,
description="Object Store created with the Nutanix v4 Python SDK",
domain=domain_name,
num_worker_nodes=worker_nodes,
cluster_ext_id=cluster_ext_id,
storage_network_reference=subnet_ext_id,
storage_network_vip=IPAddress(
ipv4=IPv4Address(
prefix_length=32,
value=storage_vip,
)
),
storage_network_dns_ip=IPAddress(
ipv4=IPv4Address(
prefix_length=32,
value=dns_ip
)
),
public_network_reference=subnet_ext_id,
public_network_ips=[
IPAddress(
ipv4=IPv4Address(
prefix_length=32,
value=public_ip
)
)
],
total_capacity_gi_b=capacity,
)
```


### Step 6: Create new user of type Service Account


For programmatic use of the Nutanix v4 APIs and SDKs, it can be considered good practice to use dedicated service accounts and API keys. This approach avoids the use of standard user accounts that may be affected by situations such as password expiry, unexpected password change (etc).


This is particularly relevant when working with Nutanix Objects Storage as bucket, object and file operations are not supported with standard user accounts.


For a dedicated article on using Nutanix IAM service accounts and API keys, see[Nutanix v4 APIs: Using API Key Authentication (Part 1: Authenticating using Python SDK)](https://www.nutanix.dev/2025/02/05/nutanix-v4-apis-using-api-key-authentication/) .


This demo creates a dedicated Nutanix Objects user of type service account that will be used for Nutanix Objects Storage only. The username has been requested from the user and will now be verified to make sure an existing user account with the same name does not exist.


Whilst SDK requests have been completed using the Objects SDK up to this point, user account management is completed using the IAM (Identity and Access Management) SDK. An Odata filter is used to explicitly request a list of users matching the entered name only.


```text
# do some quick validation to ensure there are no conflicts in upcoming steps
user_validation = iam_instance.list_users(
async_req=False,
_filter=f"username eq '{service_account_username}' and userType eq Iam.Authn.UserType'SERVICE_ACCOUNT'",
)


if user_validation.metadata.total_available_results == 0:
# create a new user of type service account
# first, build the payload
print("Building new Service Account user payload ...")
service_account = User(
username=service_account_username,
email="no-reply@acme.com",
display_name="Objects User",
description="Objects user of type Service Account",
creation_type=CreationType.USERDEFINED,
status=UserStatusType.ACTIVE,
user_type=UserType.SERVICE_ACCOUNT,
)
# now, create the user
print("Creating new user of type Service Account ...")
create_user = iam_instance.create_user(
async_req=False, body=service_account
)


# user extid is required for creating the new user's Objects key
user_extid = create_user.data.ext_id
```


### Step 7: Generate and assign API key for new service account


Whilst standard user accounts can be used to manage **Object Stores** , a user of type Service Account must be used during **Bucket** operations. This demo uses a user of type service account and an associated API key of type` OBJECT_KEY` .


With the new user account created or an existing user account verified, we can now move on with generating an API key for the new user account.


In this demo, the process involves building the request payload, creating the API key and associating it with the user of type Service Account.


Important note: The access key and secret key are available ONLY immediately after the request is successful. This demo displays the keys on screen; this should not be done in a production environment.


If the request fails, the demo script waits for 5 seconds then displays the complete exception details. This is done purely so the user is not overwhelmed with unexpected JSON in the event of an exception.


```text
# build the Objects key payload
print("Building new Object key payload ...")
object_key_id = uuid.uuid1()
key = Key(
name=f"objects_key_{object_key_id}",
description="Objects Storage Key",
key_type=KeyKind.OBJECT_KEY,
)


# create the new Objects key
print(
f"Creating new user key of type OBJECT_KEY named objects_key_{object_key_id} ..."
)
try:
objects_key = iam_instance.create_user_key(user_extid, key)
except IamException as ex:
print("An exception occurred while creating the user's Object key:\n")
print("Displaying exception details in 5 seconds ...")
time.sleep(5)
print(ex)
sys.exit()


# make sure the Object key was created
if objects_key:
access_key = objects_key.data.key_details.access_key
secret_key = objects_key.data.key_details.secret_key
print("Objects Key created:")
print(f"    Access key: {access_key}")
print(f"    Secret key: {secret_key}\n")
```


### Step 8: Create Bucket


The final step in the demo is to create a Bucket in the new Object Store.


#### » AWS S3 Compliance Note


Because Nutanix Objects Storage is AWS S3-compliant, bucket management operations in Python can be carried out using the popular` boto3` library.


```text
# create a new bucket on the new Object Store
print(f"Creating bucket named {bucket_name} ...")


# this section uses the AWS boto3 library
session = boto3.session.Session()


# setup the s3 session
# this uses the AWS access and secret keys obtained when
# the Objects Key was created in previous steps
s3c = session.client(
aws_access_key_id=access_key,
aws_secret_access_key=secret_key,
endpoint_url=f"http://{public_ip}",
service_name="s3",
use_ssl=False,
)


# attempt to retrieve a bucket matching the entered name
try:
s3c.head_bucket(Bucket=bucket_name)
print(f"A bucket named {bucket_name} already exists, exiting ...")
sys.exit()
except ClientError:
# a bucket matching the supplied name does not exist
try:
print("(0) matching buckets found, attempting to create bucket ...")
s3c.create_bucket(Bucket=bucket_name)
print(f"Bucket named {bucket_name} created successfully.")
except Exception as ex:
# unhandled exception
print("Exception occurred while creating bucket:")
print(f"{ex}")
```


## Script Demo


With the main parts of the demo script discussed in detail, let’s take a look at how the demo looks during execution.


In the following screenshot, each step has been highlighted with each part matching the steps listed above.


Screenshot of complete demo script running


## Downloadable Demo


The entire demo script used in this article can be[downloaded from the NutanixDev GitHub](https://github.com/nutanixdev/code-samples/blob/master/python/v4api_sdk/objects_storage.py) . When downloading this script, **make sure you git clone the entire repository** ; this will ensure your download includes the` tme` library that provides Prism Central endpoint connection functions.


## Summary


After running the demo script, the specified Prism Central instance will have Nutanix Objects Storage configured as follows:


1. A new Object Store named cr-store


- The public IP address will be used for Nutanix Objects Browser access


2. A new Bucket named cr-bucket
3. A new user of type Service Account named objects_user
4. Access key and secret key as shown


- Remember these details are only available after creation and cannot be retrieved later


## Conclusion


This demo has covered programmatic management of Nutanix Objects Storage to work with Object Stores, Buckets (via` boto3` ), user accounts of type Service Account and API keys of type` OBJECT_KEY` .


All operations shown in this demo can be completed using the Nutanix v4 SDKs or with their associated REST API endpoints.In part 2, coming soon, we’ll complete the configuration by creating a bucket policy for` cr-bucket` .
