---
schema_version: "1.0.0"
document_id: "a2f3359ec3f2c1f3aea4489b980175153881c623568bac1f79850dd014d8a59a"
company_key: "yc-tensorfuse"
company: "Tensorfuse"
source_id: "yc-tensorfuse-news-import-97ab2b67de47"
canonical_url: "https://tensorfuse.io/blog/increase-gpu-quota-on-aws-with-python-script"
published_at: "2024-06-03T00:00:00+00:00"
first_seen_at: "2026-07-24T04:04:56.878556+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:b5f9c65e6b5485b38825e06be814352cf2542a4bbe1d67b6619ca7985eea8f24"
---

# Increase GPU Quota on AWS: A Comprehensive Guide

Tutorial


# Increase GPU Quota on AWS: A Comprehensive Guide


Agam Jain


Jun 3, 2024


10 mins


This guide provides a comprehensive tutorial on increasing GPU quota limits on an AWS account for scaling machine learning workloads, including a Python script for automating the process.


## Introduction


In this blog, we will walk through the process of increasing GPU quota limits on your AWS account, essential for scaling machine learning (ML) workloads. By the end of this post, you’ll have a script that allows you to programmatically apply for service quota increases for all types of GPU instances in all regions. This will save you time compared to manually applying via the AWS console.


## **Types of EC2 Instances and Ideal Instances for ML Workloads**


AWS offers a variety of EC2 instances tailored for different types of workloads. Here are the primary types of EC2 instances and the tasks they are optimized for:


1. **General Purpose** : Balanced CPU, memory, and network resources. Examples: M5, M6i.
2. **Compute Optimized** : High-performance processors for compute-bound applications. Examples: C5, C6i.
3. **Memory Optimized** : High memory size for memory-intensive applications. Examples: R5, X1.
4. **Accelerated Computing Instances** : Instances with hardware accelerators like GPUs and FPGAs. Examples: P3, P4, G4.


For ML workloads, particularly deep learning tasks, **Accelerated Computing Instances** are the best choice due to their GPU capabilities. Here are the most common EC2 instances ideal for ML workloads along with the types of GPU they support:


- **P5 Instances** : NVIDIA H100 GPUs
- **P4 Instances** : NVIDIA A100 GPUs
- **P3 Instances** : NVIDIA V100 GPUs
- **G5 Instances** : NVIDIA A10G GPUs
- **G4 Instances** : NVIDIA T4 GPUs
- **G3 Instances** : NVIDIA M60 GPUs


## Estimating the Service Quota Limit (Using DBRX Example)


In this section, we will calculate the service quota increase limit for some of the most common types of ML instances like` p4d.24xlarge` ,` p3.16xlarge` ,` g5.4xlarge` , and` g3.8xlarge` using the example of DBRX inference.


It's crucial to apply for a quota increase for various instance types in multiple regions, even if you already possess the quotas. This strategy can help address potential availability issues that may occur in different regions.


To infer the DBRX model in` int8` quantization, we need approximately 121.98 GB of VRAM. Below is a table that outlines the number of instances required for each instance type to meet the GPU memory requirement for inferring DBRX, along with the total vCPU quota required:


Instance Type GPU Memory per Instance (GB) Number of Instances Required vCPUs per Instance Total vCPUs Required


p4d.24xlarge 320 1 96 96


p3.16xlarge 128 1 64 64


g5.4xlarge 24 6 16 96


g3.8xlarge 16 8 32 256


Now that we are aware of the minimum vCPU quota limit required for each instance type, the next step is to apply for these quota limits in all possible regions to prevent availability issues.


## **Applying for Quota Limit Increase (Script Included)**


To increase your GPU quota limits, there are a couple of ways to apply:


1. **AWS Management Console** : Navigate to the Service Quotas dashboard, select the service (e.g., EC2), and request a quota increase for the desired resource. Do this for all the regions.
2. **AWS CLI** : Use the AWS Command Line Interface to request quota increases by running specific commands for each region and instance type.


While these methods are effective, they are manual and time-consuming, especially when you need to apply for multiple regions and instance types.


To streamline this process, you can use the following Python script, which automates the application for service quota increases across different regions and instance types using the AWS SDK (Boto3).


**Important Warning:** Avoid applying the script to all regions and instance types at once as this could trigger security issues on your account. Begin by applying to the 1-2 most essential instance types in 1-2 regions. Once those are approved, proceed with more. Remember, there's a cap on open service quota requests in EC2. If you've hit that cap, wait for current tickets to close before rerunning the script once the limit is lifted.


```text


import   boto3


##Important Warning: Avoid applying the script to all regions and instance types at once as this could trigger security issues on your account. Begin by applying to the 1-2 most essential instance types in 1-2 regions. Once those are approved, proceed with more. Remember, there's a cap on open service quota requests in EC2. If you've hit that cap, wait for current tickets to close before rerunning the script once the limit is lifted.


# Define the regions and GPU instance types
regions   =     [  'us-east-1'  ,     'eu-west-1'  ]     #['us-east-1', 'eu-west-1', 'us-west-2', 'us-east-2', 'ap-south-1', 'eu-west-2', 'eu-west-3', 'eu-north-1', 'eu-central-1', 'ca-central-1']  # Add all desired regions
gpu_instance_types   =     {
'All P4, P3 and P2 Spot Instance Requests'  :     'L-7212CCBC'  ,
'All G and VT Spot Instance Requests'  :     'L-3819A6DF'
#'All P5 Spot Instance Requests': 'L-C4BD4855'
#'All Inf Spot Instance Requests': 'L-B5D1601B'
#'All Trn Spot Instance Requests': 'L-6B0D517C'
}


# Desired quota value
desired_value   =     700


# Initialize the boto3 client
def     request_quota_increase  (  service_code  ,   quota_code  ,   region  ,   desired_value  )  :
try  :
# Check for open quota increase requests
open_statuses   =     [  'PENDING'  ,     'CASE_OPENED'  ,     'INVALID_REQUEST'  ]
response   =   client  .  list_requested_service_quota_change_history_by_quota  (
ServiceCode  =  service_code  ,
QuotaCode  =  quota_code
)
for   quota_request   in   response  [  'RequestedQuotas'  ]  :
if   quota_request  [  'Status'  ]     in   open_statuses  :
print  (  f"Open quota increase request already exists for   {  quota_code  }   in   {  region  }   with status   {  quota_request  [  'Status'  ]  }  "  )
return


# Check current quota
response   =   client  .  get_service_quota  (
ServiceCode  =  service_code  ,
QuotaCode  =  quota_code
)
current_value   =   response  [  'Quota'  ]  [  'Value'  ]
print  (  f"Current quota for   {  quota_code  }   in   {  region  }  :   {  current_value  }  "  )


# Request quota increase if current value is less than desired value
if   current_value   <   desired_value  :
response   =   client  .  request_service_quota_increase  (
ServiceCode  =  service_code  ,
QuotaCode  =  quota_code  ,
DesiredValue  =  desired_value
)
print  (  f"Requested quota increase for   {  quota_code  }   in   {  region  }   to   {  desired_value  }  "  )
else  :
print  (  f"No increase needed for   {  quota_code  }   in   {  region  }  "  )
except   Exception   as   e  :
print  (  f"Error requesting quota increase for   {  quota_code  }   in   {  region  }  :   {  e  }  "  )


# Iterate over all regions and GPU instance types
for   region   in   regions  :
for   instance_type  ,   quota_code   in   gpu_instance_types  .  items  (  )  :
client   =   boto3  .  client  (  'service-quotas'  ,   region_name  =  region  )
request_quota_increase  (  'ec2'  ,   quota_code  ,   region  ,   desired_value  )


```


**Explanation of the Script** :


1. **Regions and GPU Instance Types** : Defines a list of regions and a dictionary of GPU instance types with their corresponding quota codes.
2. **Desired Quota Value** : Sets the desired quota limit (number of vCPUs).
3. **Quota Increase Function** :


1. **Checks for Open Requests** : Ensures there are no pending quota increase requests for the specified quota code in the region.
2. **Current Quota Check** : Retrieves and prints the current quota value.
3. **Request Quota Increase** : If the current value is less than the desired value, it requests a quota increase.


4. **Iterate Over Regions and Instance Types** : The script iterates over all specified regions and GPU instance types, applying the quota increase where needed.


**How to run the script**


1. Install Boto3 and Configure AWS Credentials.
2. Modify the regions, gpu_instance_types, and desired_value as needed, then run the script. It will apply for quota increases across the specified regions and instance types.


## **Conclusion**


Managing GPU quotas is crucial for ML workloads, especially as projects scale. Understanding the types of EC2 instances and their GPU capabilities allows you to choose the right instance for your needs. With the provided script, you can streamline the quota increase process, ensuring your projects run smoothly without manual intervention.


Happy computing!
