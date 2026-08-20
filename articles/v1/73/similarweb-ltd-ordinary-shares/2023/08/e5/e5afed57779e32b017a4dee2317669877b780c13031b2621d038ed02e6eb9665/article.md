---
schema_version: "1.0.0"
document_id: "e5afed57779e32b017a4dee2317669877b780c13031b2621d038ed02e6eb9665"
company_key: "similarweb-ltd-ordinary-shares"
company: "Similarweb Ltd."
source_id: "similarweb-ltd-ordinary-shares-rss-2bd7dd1f88a6"
canonical_url: "https://medium.com/similarweb-engineering/reduce-nat-gateway-charges-by-identifying-missing-vpc-endpoints-fd3d4fd1d6d8"
published_at: "2023-08-23T12:10:38+00:00"
first_seen_at: "2026-07-20T23:19:16.581763+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:2ce110efb4a7f20057ac77efe09c11c81e0b0c873b30406739a2986117dd1959"
---

# Reduce NAT Gateway Charges By Identifying Missing VPC Endpoints

# Reduce NAT Gateway Charges By Identifying Missing VPC Endpoints


[Liav Shabtai](https://medium.com/@liav.shabtai?source=post_page---byline--fd3d4fd1d6d8---------------------------------------)


8 min read


·


Aug 23, 2023


--


It often happens that, when architecting a network, all traffic is routed via a NAT gateway. This can be due to network architecture habits inherited from the traditional data center, in combination with lack of awareness of the costs involved in using a NAT Gateway, the use of this service can easily accumulate high charges while lacking clear visibility to the traffic that is being traversed through it.


To reduce costs for customers, AWS introduced VPC Endpoints. Enables customers to privately connect to supported AWS services through VPC endpoints powered by AWS PrivateLink, for a much cheaper price compared to the bytes transfer charges through Nat Gateways.


**This blog post will guide you through the essential steps to set up VPC Flow Logs in the most cost-efficient manner, and how to query for missing VPC Endpoints. Providing FinOps and DevOps engineers with crucial visibility into their NAT Gateway traffic. Which may lead to substantial cost reductions.**


**At Similarweb, by adopting this optimization method, we successfully identified and configured missing VPC Endpoints, slashing our NAT traffic expenses by over 35%.**


**An added perk?** if you will Follow the step-by-step guide ([take me there](https://medium.com/similarweb-engineering/reduce-nat-gateway-charges-by-identifying-missing-vpc-endpoints-fd3d4fd1d6d8#ca44) ) **The entire setup can be completed in under 30 minutes** .


## NAT Gateway VS VPC Endpoints Architecture and Pricing


### NAT Gateway


- NAT Gateways allow instances in a Virtual Private Cloud (VPC) to initiate traffic to the internet, and then return the response, without allowing the internet to initiate a connection with the requesting instances.
- Typically used for instances in a private subnet to reach the internet (for updates, patches, etc.) but not for the internet to reach those instances


Press enter or click to view image in full size


**VPC Endpoints**


- **Interface Endpoints:** VPC endpoint enables a private connection to supported AWS services and VPC endpoint services powered by AWS PrivateLink.
- **Gateway Endpoints:** These can be created for Amazon S3 and DynamoDB and route traffic to these services.


**Benefits of VPC Endpoints:**


- **Security:** Your traffic does not traverse the public internet, reduces the exposure to threats such as data breaches and data loss.
- **Performance:** They provide reliable, and often faster, connections to AWS services.
- **Cost-Efficiency:** Data processed through VPC Endpoints is less expensive than the data processed through NAT Gateways. **Specifically, VPC Gateway Endpoints for S3 and DynamoDB incur no additional charges. Therefore, should be a definite inclusion in all network architectures, effectively eliminating current and future bytes transfer charges associated with these services.**


Press enter or click to view image in full size


**Traffic to AWS Services with VPC Endpoints Configured**


### Pricing:


> *The pricing information is accurate for US East (N. Virginia) at the time of this article’s publication.*


Press enter or click to view image in full size


**Routing traffic via VPC Endpoints can be significantly more cost-effective, potentially reducing costs by over 75%, to supporting services, compared to using the default NAT alternative.** Beyond the direct cost savings, NAT Gateways also incur standard data transfer fees, Additional charges for internet outbound and cross-AZ traffic ([See detailed pricing for Data Transfer Charges](https://aws.amazon.com/ec2/pricing/on-demand/) ). VPC Endpoints, remove these additional charges completely.


## Step By Step: How to Identify Missing VPC Endpoints in Your Network Architecture


### Prerequisites


- Delivering Cost and Usage Reports to an Athena-Configured S3 Bucket with Resource ID Cost Allocation.[AWS Guide](https://docs.aws.amazon.com/cur/latest/userguide/cur-query-athena.html)
- S3 Bucket or Prefix to deliver VPC Flow logs into. In case working cross accounts a bucket policy would need to be enabled to deliver the logs ([How to publish VPC Flow logs to a different account](https://dev.to/kasukur/how-to-publish-vpc-flow-logs-to-a-different-account-1ead) **).**


### Step 1: Focus on your top spending NAT Gateways


If you are unsure which NAT Gateways account for the highest Bytes transferred usage, execute the following Athena query. This will help identify the top NAT Gateways based on their Bytes transferred charges, allowing you to prioritize optimization efforts on them.


```text
-- Results display NAT GW ARN costs in descending order.select line_item_resource_id,  -- for the months of June, July, and August in the year 2023.   select line_item_resource_id,                 sum("line_item_unblended_cost") as "unblended_cost"  from <schema>.<table>  where line_item_usage_type like '%NatGateway-Bytes%'     and year like '2023'     and month in ('6','7','8')  group by 1  order by 2 desc;
```


Afterwards, by using the AWS console, find their attached Elastic Network Interface (ENI) and their CIDR Ranges.


### Step 2: Enable VPC Flow Logs on chosen ENIs


The most cost Effective method of delivering VPC Flow logs is by enabling the delivery to S3 and storing the file in a parquet compressed format, the alternative of using Çloudwatch to store and query the VPC Flow logs can accumulate high charges, be warned.


### **Use the following Python Script to Create flow logs automatically**


```text
import boto3  def create_vpc_flow_logs(s3_location, eni_list, region):      # Initialize the EC2 client      ec2 = boto3.client('ec2', region_name=region)      # Create VPC flow logs      create_response = ec2.create_flow_logs(          DryRun=False,          ResourceIds=eni_list,          ResourceType='NetworkInterface',          TrafficType='ALL',          LogDestinationType='s3',          LogDestination=s3_location,          LogFormat='${version} ${account-id} ${interface-id} ${srcaddr} ${dstaddr} ${pkt-srcaddr} ${pkt-dstaddr} ${pkt-src-aws-service} ${pkt-dst-aws-service} ${srcport} ${dstport} ${protocol} ${packets} ${bytes} ${start} ${end} ${action} ${log-status} ${flow-direction}',          TagSpecifications=[              {                  'ResourceType': 'vpc-flow-log'              }          ],          MaxAggregationInterval=600,          DestinationOptions={              'FileFormat': 'parquet',              'HiveCompatiblePartitions': False,              'PerHourPartition': True          }      )  # Return the creation response      return create_response  # Configuration settings  s3_location = 'arn:aws:s3:::<bucket>/<prefix>'  eni_list = ['<eni1>', '<eni2>']    region = '<aws_region>'  # Call the function and print results  response = create_vpc_flow_logs(s3_location, eni_list,region)  print(response)  print(f"Flow Logs successfully created, Flow Log ID: {response['FlowLogIds'][0]}")
```


### **Manual Configuration**


1. Click on the ENI in the AWS Console, choose create Flow Log.
2. Configuration of Flow Logs:


- Destination: Send to an S3 bucket
- Change The Default Log Record Format to the following([VPC Flow Logs Attributes](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) ):


${version} ${account-id} ${interface-id} ${srcaddr} ${dstaddr} ${pkt-srcaddr} ${pkt-dstaddr} ${pkt-src-aws-service} ${pkt-dst-aws-service} ${srcport} ${dstport} ${protocol} ${packets} ${bytes} ${start} ${end} ${action} ${log-status} ${flow-direction}


- Log file format : Parquet
- Partition logs by time: Every 1 hour (60 mins)


**Step 3: Create VPC Flow Logs Table in Athena**


```text
CREATE EXTERNAL TABLE `vpc_flow_logs`(   `version` int,   `account_id` string,   `interface_id` string,   `srcaddr` string,   `dstaddr` string,   `pkt_srcaddr` string,   `pkt_dstaddr` string,   `pkt_src_aws_service` string,   `pkt_dst_aws_service` string,   `srcport` int,   `dstport` int,   `protocol` bigint,   `packets` bigint,   `bytes` bigint,   `start` bigint,   `end` bigint,   `action` string,   `log_status` string,   `flow_direction` string,   `vpc_id` string,   `subnet_id` string,   `instance_id` string,   `tcp_flags` int,   `type` string,   `az_id` string,   `sublocation_type` string,   `sublocation_id` string,   `traffic_path` int)  PARTITIONED BY (   `region` string,   `datehour` string)  ROW FORMAT SERDE   'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'  STORED AS INPUTFORMAT   'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'  OUTPUTFORMAT   'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'  LOCATION   's3://<bucket_name>/<prefix>'  TBLPROPERTIES (   'projection.datehour.format'='yyyy/MM/dd/HH',   'projection.datehour.interval'='1',   'projection.datehour.interval.unit'='HOURS',   'projection.datehour.range'='2021/01/01/00,NOW',   'projection.datehour.type'='date',   'projection.enabled'='true',   'projection.region.type'='enum',   'projection.region.values'='us-east-1,<additional Regions>',   'skip.header.line.count'='1',   'storage.location.template'='s3://<bucket_name>/<prefix>/vpcflowlogs/${region}/${datehour}'   )
```


### Step 4: Query The Traffic to find Missing VPC Endpoints


The` pkt_dst_aws_service, pkt_src_aws_service` column will point to the name of the AWS Service you are trying to communicate with. However most AWS Services are still not mapped and will receive the value “AMAZON”.


Press enter or click to view image in full size


mapped AWS services values for the pkt_src/dst _aws_serice column


### Query for missing Endpoints for Mapped AWS Services


Calculate the total bytes transferred, categorized by the` pkt_dst_aws_service` and` pkt_src_aws_service` columns. This will help identify which mapped AWS Services are not sending data through the VPC Endpoint.


```text
-- Uploads to aws services     -- x.x.x.x.x is the NAT Gateways IP Address    -- y.y.%.% all traffic directed to resources withing the NAT Gateway ip range  select pkt_dst_aws_service,sum(bytes)/(1000*1000) as "MB"  from finops.test_table_vpclogs  where srcaddr = <'x.x.x.x.x '> and dstaddr not like <'y.y.%.% '>  group by 1  order by  2 desc  limit 1000;
```


```text
-- Downloads to aws services     -- x.x.x.x.x is the NAT Gateways IP Address    -- y.y.%.% all traffic directed to resources withing the NAT Gateway ip range  select pkt_dst_aws_service,sum(bytes)/(1000*1000) as "MB"  from finops.test_table_vpclogs  where dstaddr = <'x.x.x.x.x '> and srcaddr not like <'y.y.%.% '>  group by 1  order by  2 desc  limit 1000;
```


### Query for missing Endpoints for Remaining AWS Services


It is very likely you will find that most traffic is for unmapped services which receive the value “AMAZON” for the` pkt_dst_aws_service` column.


Here is how to inspect them:


```text
-- uploads to aws services     -- x.x.x.x.x is the NAT Gateways IP Address    -- y.y.%.% all traffic directed to resources withing the NAT Gateway ip range  select dstaddr,pkt_dstaddr,pkt_dst_aws_service,sum(bytes)/(1024*1024*1024) as "GB Transfered"  from <schema>.vpc_flow_logs  where srcaddr = <'x.x.x.x.x '> and dstaddr not like <'y.y.%.% '>  and "pkt_dst_aws_service" =  'AMAZON'  group by 1,2,3  order by  2 desc  limit 1000;
```


```text
-- downloads to aws services     -- x.x.x.x.x is the NAT Gateways IP Address    -- y.y.%.% all traffic directed to resources withing the NAT Gateway ip range  select dstaddr,pkt_dstaddr,pkt_dst_aws_service,sum(bytes)/(1024*1024*1024) as "GB Transfered"  from <schema>.vpc_flow_logs  where dstaddr = <'x.x.x.x.x '> and srcaddr not like <'y.y.%.% '>  and "pkt_dst_aws_service" =  'AMAZON'  group by 1,2,3  order by  2 desc  limit 1000;
```


The column (representing destination address IPs) will display the IP addresses of unmapped AWS Services that the NAT is attempting to communicate with.


From here we choose to cherry pick specific ip address, starting from those that transferred large amounts of data and did the following:


1. Open up a we-browser search for htttp://<dstaddr>
2. Click Advance


Press enter or click to view image in full size


Click on Advance Press enter or click to view image in full size


The Searched IP address was communication through NAT gateway to ECR not through a VPC Endpoint


In our case, individually reviewing the list of destination IP addresses with the highest traffic was sufficient. Nonetheless, we acknowledge that using third-party tools can enhance and automate this phase of the procedure more efficiently.


### Step 5:[Create the Missing VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html)


### Step 6: Terminate all VPC Flow logs after investigation to not incur further charges for Log Delivery.


## Conclusion:


By identifying and leveraging VPC Endpoints, organizations can not only secure their traffic but also avoid unnecessary expenses associated with NAT Gateways and potentially reduce their NAT Traffic charges drastically. This article has highlighted the foundational steps required for any FinOps or DevOps engineer to gain a clear insight into their NAT Gateway traffic, enabling them to identify and fill the gaps where VPC Endpoints are missing.


We managed to decrease our NAT Gateway traffic costs drastically, by over 35%, How much will you?


### Supporting Sources:


1. [Mastering AWS Cost Optimization](https://www.amazon.com/Mastering-AWS-Cost-Optimization-operational/dp/B09WQBH7VV) : A comprehensive guide on AWS costs, covering services, pricing models, and cost-reduction strategies.
2. [Overview of Data Transfer Costs for Common Architectures:](https://aws.amazon.com/blogs/architecture/overview-of-data-transfer-costs-for-common-architectures/) An article from the AWS Architecture Blog detailing data transfer costs for various AWS architectures and best practices for cost optimization.
