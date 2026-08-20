---
schema_version: "1.0.0"
document_id: "6d45e2c5c6b7ccb55ac02c6349b9cba12da177afebb15b97b1c40192fab9c34d"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/05/29/nutanix-objects-storage-for-developers-nutanix-python-sdk-operations-part-2/"
published_at: "2026-05-29T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:c817bf234c85b21a744d861008b70e3b4055701cded47ac78494550655f7b725"
---

# Nutanix Objects Storage for Developers: Nutanix Python SDK Operations (Part 2)

## Introduction


In the first part of this series,[Nutanix Objects Storage for Developers: Nutanix Python SDK Operations (Part 1)](https://www.nutanix.dev/2026/05/04/nutanix-objects-storage-for-developers-nutanix-python-sdk-operations-part-1/) , we introduced the the Nutanix Objects Storage solution’s v4 APIs and SDKs. In that article, the focus was specifically on a “ground-up” configuration:


- Creation of a new Object Store
- Creation of a new Bucket
- Creation of a new User of type Service Account
- Creation of a new API Key for use with the new Service Account
- At each step, verify the existence of the info collected by the user, ensuring duplicate stores, buckets and account creations are avoided


In part 2 of this series, we’ll extend what we already know and make a small adjustment to the process: **create and apply a new bucket policy to the new or existing bucket.**


This is an important step for production environments as it ensures only the required access is granted to specific users.


## Scenario


For today’s demo and code sample, the following requirements must be met.


- Create a new bucket policy using the ubiquitous` boto3` Python library
- Specify the policy the grant the following permissions to all users:


- GetObject
- ListBucket


### Permissions Disclaimer


The permissions used in this article are for **demo purposes only** . All Nutanix Objects Storage customers must individually verify site-specific permissions, ensuring only the required permissions are applied.


## Demo Environment


This demo and the accompanying demo script were developed using the following component versions:


- Nutanix Prism Central **v7.5.0.5**
- Nutanix AOS **v7.5.0.5**
- Nutanix Objects Service and Objects Manager **v5.3.0.1**
- ` ntnx_objects_py_client` SDK **v4.0.2**
- AWS` boto3` **1.42.59**


All software used in this demo is GA (generally available).


## Assumptions


This article assumes readers have already read part 1 in this series:[Nutanix Objects Storage for Developers: Nutanix Python SDK Operations (Part 1)](https://www.nutanix.dev/2026/05/04/nutanix-objects-storage-for-developers-nutanix-python-sdk-operations-part-1/) .


For info on getting started with the Nutanix v4 APIs as a general topic, see the[Nutanix v4 API User Guide](https://www.nutanix.dev/nutanix-api-user-guide/) .


## The Demo & Script


Before modify the demo script from part 1, examine the following code snippet:


```text
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
```


This snippet checks for the existence of a bucket matching the entered name. If a matching bucket is found, the script exits.


In part 2 of the series, the script must continue if an existing bucket is found, otherwise the bucket policy will not be applied.


If you are following this article and building the same script, make sure to remove the` sys.exit()` line before continuing. Continue adding the bucket policy code underneath the exception handler that is thrown when a bucket cannot be created.


### Step 9: Apply Bucket Policy


#### Step 9a: Build JSON Bucket Policy


Our demo bucket policy can be created as follows:


```text
# attempt to create the bucket policy
try:
print(f"Building bucket policy payload for bucket {bucket_name}:")
print("    ListBucket: list bucket contents")
print("    GetObject: read a file or object from the bucket")
# allow anyone ("*") with file links:
#   ListBucket i.e. list bucket contents
#   GetObject i.e. read a file or object from the bucket
# requires URL to be resolvable
# e.g. in /etc/hosts:
# <public-ip> <bucket-name>.<object-store>.msp.cluster.local
new_policy=json.dumps({
"Version": "2.0",
"Statement": [
{
"Sid": "AddPerm",
"Effect": "Allow",
"Principal": "*",
"Action": [
"s3:GetObject",
"s3:ListBucket"
],
"Resource": f"arn:aws:s3:::{bucket_name}/*"
}
]
})
...
```


This policy will grant all users permission to list bucket contents (` ListBucket` ) and get the contents of an object within that bucket (` GetObject` ).


#### Step 9b: Apply Bucket Policy


With the policy built, we can now apply the policy to the specified bucket. Remember, the name of the bucket was specified by the user in part 1 of this series.


```text
....
print(f"Creating bucket policy for {bucket_name} ...")


s3c.put_bucket_policy(
Bucket=f"{bucket_name}",
Policy=new_policy
)


print(f"Bucket policy creation for {bucket_name}.")
print(f"\nIf you have configured name resolution, your bucket files will be available at http://{bucket_name}.")
print("\nDone.\n")
except ClientError as ex:
print("Exception occurred while applying bucket policy:")
print(ex)
```


## Script Demo


With the main parts of the demo script discussed in detail in part 1, let’s take a look at how final part the demo.


In the following screenshot, the final step, bucket policy creation is shown.


Using` boto3` to apply a new bucket policy


Finally, the following screenshot shows the entire end-to-end process.


Note for part 2: Because this is the second time we’ve run the script, the following entities already exist:


- ` cr-store` object store
- ` cr-bucket` bucket
- ` cr-objects-users` user of type Service Account


Screenshot of part 2 demo script running


## Downloadable Demo


The entire demo script used in this article can be[downloaded from the NutanixDev GitHub](https://github.com/nutanixdev/code-samples/blob/master/python/v4api_sdk/objects_storage.py) . When downloading this script, **make sure you git clone the entire repository** ; this will ensure your download includes the` tme` library that provides Prism Central endpoint connection functions.


## Summary


After running the demo script, the specified Prism Central instance will have Nutanix Objects Storage configured as follows:


1. A new Object Store named` cr-store`


- The public IP address will be used for Nutanix Objects Browser access


2. A new Bucket named` cr-bucket`
3. A new user of type Service Account named` cr-objects-users`
4. Access key and secret key as shown


- Remember these details are only available after creation and cannot be retrieved later


## Conclusion


This demo has covered programmatic management of Nutanix Objects Storage to work with Object Stores, Buckets (via` boto3` ), user accounts of type Service Account and API keys of type` OBJECT_KEY` .


All operations shown in this demo can be completed using the Nutanix v4 SDKs or with their associated REST API endpoints.
