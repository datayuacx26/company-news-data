---
schema_version: "1.0.0"
document_id: "809afc91226029c1e6511e886241105da07ce62c99a39aed47f03bf17698813a"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/blog/opentelemetry-ecs"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:88f48d4d633da0e337bc4d730205ba60f7f1bff1faf8ed467f2b138c5c3d78b4"
---

# OpenTelemetry ECS Tutorial - Monitor AWS ECS metrics [Step-By-Step Guide]

# OpenTelemetry ECS Tutorial - Monitor AWS ECS metrics \[Step-By-Step Guide\]


Published on: December 28, 2023


Last Updated: June 17, 2026


12 min read


OpenTelemetry can be used to monitor ECS clusters. In this tutorial, you will install OpenTelemetry Collector to collect ECS metrics and then send the collected data to SigNoz for monitoring and visualization.


In this tutorial, we cover:


- A brief overview of AWS ECS
- What is OpenTelemetry Collector?
- How does OpenTelemetry Collector collect data?
- Pre-requisites
- Setting up SigNoz
- Setting up OpenTelemetry Collector
- Reference: Metrics collected by OpenTelemetry ECS Container insights receiver for ECS
- Conclusion


If you want to jump straight into implementation, start with thisPre-requisites section.


Containerization is a popular deployment mechanism in today’s ever-changing technology landscape. It has allowed us to easily migrate applications from one platform to another without having to worry about a bunch of OS-specific dependencies. With containerization and a cloud-native approach picking up pace, every cloud provider has come up with solutions to deploy containers in a more native cloud fashion. On AWS, the Elastic Container Service was introduced with the same goal.


## A brief overview of AWS ECS


AWS ECS is the container orchestration platform proprietory to AWS. It simplifies the provisioning of Docker containers using EC2 or Fargate-based computing. With AWS ECS, you can easily deploy containers, autoscale them as well as load balance them using AWS ELB. Thus, it provides a fully integrated solution for deploying, managing, and scaling containers. Although AWS provides a decent amount of metrics related to the service itself, it still does not provide sufficient insights into the actual container metrics themselves.


In this article, we try to get deeper into monitoring the services and tasks on AWS ECS using an OpenTelemetry Collector. If you wish to first understand the basics, such as which signals matter on ECS and the failure modes to be aware of, check out our[overview of AWS ECS monitoring](https://signoz.io/blog/aws-ecs-monitoring/) blog.


## What is OpenTelemetry Collector?


OpenTelemetry Collector is a stand-alone service provided by OpenTelemetry. It can be used as a telemetry-processing system with a lot of flexible configurations to collect and manage telemetry data.


It can understand different data formats and send it to different backends, making it a versatile tool for building observability solutions.


[Read our complete guide on OpenTelemetry Collector](https://signoz.io/blog/opentelemetry-collector-complete-guide/)


## How does OpenTelemetry Collector collect data?


A receiver is how data gets into the OpenTelemetry Collector. Receivers are configured via YAML under the top-level` receivers` tag. There must be at least one enabled receiver for a configuration to be considered valid.


Here’s an example of an` otlp` receiver:


```text
receivers  :
otlp  :
protocols  :
grpc  :
http  :


```


An OTLP receiver can receive data via gRPC or HTTP using the[OTLP](https://github.com/open-telemetry/opentelemetry-proto/blob/main/docs/specification.md) format. There are advanced configurations that you can enable via the YAML file.


Here’s a sample configuration for an otlp receiver.


```text
receivers  :
otlp  :
protocols  :
http  :
endpoint  :    "localhost:4318"
cors  :
allowed_origins  :
-   http :  //test.com
# Origins can have wildcards with *, use * by itself to match any origin.
-   https :  // *.example.com
allowed_headers  :
-   Example -  Header
max_age  :    7200


```


You can find more details on advanced configurations[here](https://github.com/open-telemetry/opentelemetry-collector/blob/main/receiver/otlpreceiver/README.md) .


After configuring a receiver, **you must enable it** . Receivers are enabled via pipelines within the service section. A pipeline consists of a set of receivers, processors, and exporters.


The following is an example pipeline configuration:


```text
service  :
pipelines  :
metrics  :
receivers  :    [  otlp ,   prometheus ]
exporters  :    [  otlp ,   prometheus ]
traces  :
receivers  :    [  otlp ,   jaeger ]
processors  :    [  batch ]
exporters  :    [  otlp ,   zipkin ]


```


Now that you understand how OpenTelemetry collector collects data let’s see how to collect AWS ECS metrics with it.


## Pre-requisites


For the purpose of this tutorial, you are expected to have at least one working ECS service based on EC2 compute that can be monitored using the OpenTelemetry. You will also need a SigNoz cloud account.


## Setting up SigNoz


You need a backend to which you can send the collected data for monitoring and visualization.[SigNoz](https://signoz.io/) is an OpenTelemetry-native APM that is well-suited for visualizing OpenTelemetry data.


SigNoz cloud is the easiest way to run SigNoz. You can sign up[here](https://signoz.io/teams/) for a free account and get 30 days of unlimited access to all features.


You can also install and self-host SigNoz yourself. Check out the[docs](https://signoz.io/docs/install/) for installing self-host SigNoz.


### Setting up ECS Service


If you do not have a service up and running, you can easily set up a simple NGINX task to run on ECS. To run an NGINX image as an ECS service, follow the below steps:


#### Create Cluster


1. Login to your AWS console
2. Head to the AWS ECS service console
3. Click “Create cluster” and fill out the below details


1. Cluster name -` MetricsTestCluster`
2. Infrastructure - Select Amazon EC2 instances
3. Autoscaling group - Create new
4. Instance type -` t3.medium` (to allow enough space for the collector and NGINX to run)
5. Minimum 1 - Maximum 2
6. Leave the rest of it as default


4. Create cluster


This will create a cluster and register instances, and the cluster will be ready to deploy tasks.


#### Create Task Definition


Next, head to the Task Definitions screen to create a new task definition. Follow the below steps:


1. Create a new task definition
2. Enter task family name:` nginx`
3. Launch type: Amazon EC2 instances
4. Task size: 0.25vCPU and 0.5GB memory
5. Leave the roles empty/defaults
6. Container-1 Name:` nginx` & Image URL:` nginx:latest`
7. Leave the rest as defaults
8. Create Task definition


#### Create Service


Finally, go to the cluster you just created and click “Create Service” in the services tab. Select the task definition you just created - Leave the rest of the options default and create the service.


### Installing AWS CLI


Next, we will require your system to be configured to deploy resources on AWS using AWS CLI. To install AWS CLI on your system, you can follow the instructions provided on[this link](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions) . It is assumed that you have` AdministratorAccess` to the account to create the required resources. Once you have the above things ready to go, we can proceed to set up the OpenTelemetry collector in ECS.


## Setting up OpenTelemetry Collector


Unlike our previous[tutorials](https://signoz.io/blog/opentelemetry-mysql-metrics-monitoring/) , we wouldn’t be running OpenTelemetry Collector as a local service on your laptop anymore. In order to monitor ECS containers, we need to run the OpenTelemetry collector on every EC2 instance that is providing compute for ECS. This implies that we need to run the collector as a Daemon service, which creates one task per node.


You can follow[this tutorial](https://signoz.io/docs/opentelemetry-collection-agents/ecs/ec2/overview/) to install the OpenTelemetry collector as a Daemon service on your ECS. The Cloud Formation template in Step 3 of the linked tutorial creates the below resources:


- Task Definition as Daemon for Opentelemetry Collector
- Service as a Daemon for the OpenTelemetry Collector
- IAM roles required for calling the container insights API


The OpenTelemetry collector config would be as shown below:


```text
extensions  :
health_check  :


receivers  :
awscontainerinsightreceiver  :
collection_interval  :   30s
container_orchestrator  :   ecs
filelog  :
start_at  :   beginning
include_file_path  :    true
include  :
-   /rootfs/var/log/dmesg
-   /rootfs/var/log/messages
-   /rootfs/var/log/ *.log
-   /rootfs/var/lib/docker/containers/ */*.log
hostmetrics  :
root_path  :   /rootfs
collection_interval  :   30s
scrapers  :
cpu  :    {  }
load  :    {  }
memory  :    {  }
disk  :    {  }
filesystem  :    {  }
otlp  :
protocols  :
grpc  :
endpoint  :   0.0.0.0 :  4317
http  :
endpoint  :   0.0.0.0 :  4318


processors  :
batch  :
timeout  :   10s
batch/aws  :
timeout  :   60s
resource/aws  :
attributes  :
-    key  :   Timestamp
action  :   delete
resourcedetection  :
detectors  :
-   env
-   ec2
-   ecs
-   docker
-   system
timeout  :   2s
override  :    false


exporters  :
otlp  :
endpoint  :    "ingest.{region}.signoz.cloud:443"
tls  :
insecure  :    false
headers  :
signoz-ingestion-key  :    "<SIGNOZ_INGESTION_KEY>"
debug  :
verbosity  :   normal


service  :
pipelines  :
metrics/aws  :
receivers  :    [  awscontainerinsightreceiver ]
processors  :    [  resource/aws ,   batch/aws ]
exporters  :    [  otlp ]
metrics/host  :
receivers  :    [  hostmetrics ]
processors  :    [  resourcedetection ,   batch ]
exporters  :    [  otlp ]
logs/host  :
receivers  :    [  filelog ]
processors  :    [  resourcedetection ,   batch ]
exporters  :    [  otlp ]
traces  :
receivers  :    [  otlp ]
processors  :    [  batch ]
exporters  :    [  otlp ,   logging ]
metrics  :
receivers  :    [  otlp ]
processors  :    [  batch ]
exporters  :    [  otlp ,   logging ]
logs  :
receivers  :    [  otlp ]
processors  :    [  batch ]
exporters  :    [  otlp ,   logging ]
extensions  :    [  health_check ]


```


This` config` does multiple metrics collection which includes collection of ECS Container insights. The receiver contacts ECS metadata endpoints and internal docker sockets to gather statistics.


To verify that the data is being sent to SigNoz Cloud, you can go to the SigNoz dashboard page and import the dashboards below:


- [instance-metrics.json](https://github.com/SigNoz/dashboards/blob/main/ecs-infra-metrics/instance-metrics.json)
- [hostmetrics.json](https://github.com/SigNoz/dashboards/blob/main/hostmetrics/hostmetrics.json)


Once imported, select the variables - Cluster name, Autoscaling group, and instance name respectively. You will be able to see the metrics as shown below:


*AWS ECS metrics were collected by OpenTelemetry Collector and visualized with SigNoz.*


You can additionally check logs of your service as well using the same configuration as shown below:


*Logs of AWS ECS collected with OpenTelemetry Collector shown in SigNoz*


The` resourcedetection` processor takes care of identifying key attributes for the container automatically and allows you to view logs by container name, task defintion arn, service name etc.


## Reference: Metrics collected by OpenTelemetry ECS Container insights receiver for ECS


The below list is specific to AWS ECS. The[documentation](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/awscontainerinsightreceiver) for this receiver contains a broader list which includes Kubernetes-specific metrics, too.


Metric Unit Type Attributes


cluster_failed_node_count Count Count ClusterName, NodeName, Type, Timestamp, Version, Sources


cluster_node_count Count Count ClusterName, NodeName, Type, Timestamp, Version, Sources


container_cpu_limit Millicore Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_cpu_request Millicore Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_cpu_usage_system Millicore Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_cpu_usage_total Millicore Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_cpu_usage_user Millicore Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_cpu_utilization Percent Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_cache Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_failcnt Count Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_hierarchical_pgfault Count/Second Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_hierarchical_pgmajfault Count/Second Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_limit Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_mapped_file Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_max_usage Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_pgfault Count/Second Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_pgmajfault Count/Second Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_request Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_rss Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_swap Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_usage Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_utilization Percent Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


container_memory_working_set Bytes Gauge AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


number_of_container_restarts Count Count AutoScalingGroupName,ClusterName,ContainerId,ContainerName,InstanceId,InstanceType,Namespace,Timestamp,Type,Version,Sources,container_status,container_status_reason,container_last_termination_reason


instance_cpu_limit Millicore Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_cpu_reserved_capacity Percent Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_cpu_usage_system Millicore Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_cpu_usage_total Millicore Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_cpu_usage_user Millicore Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_cpu_utilization Percent Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_cache Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_failcnt Count Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_hierarchical_pgfault Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_hierarchical_pgmajfault Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_limit Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_mapped_file Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_max_usage Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_pgfault Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_pgmajfault Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_reserved_capacity Percent Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_rss Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_swap Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_usage Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_utilization Percent Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_memory_working_set Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_rx_bytes Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_rx_dropped Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_rx_errors Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_rx_packets Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_total_bytes Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_tx_bytes Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_tx_dropped Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_tx_errors Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_network_tx_packets Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_number_of_running_tasks Count Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_serviced_async Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_serviced_read Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_serviced_sync Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_serviced_total Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_serviced_write Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_service_bytes_async Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_service_bytes_read Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_service_bytes_sync Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_service_bytes_total Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_diskio_io_service_bytes_write Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_filesystem_available Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_filesystem_capacity Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_filesystem_inodes Count Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_filesystem_inodes_free Count Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_filesystem_usage Bytes Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_filesystem_utilization Percent Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_rx_bytes Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_rx_dropped Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_rx_errors Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_rx_packets Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_total_bytes Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_tx_bytes Bytes/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_tx_dropped Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_tx_errors Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


instance_interface_network_tx_packets Count/Second Gauge ClusterName,InstanceType,AutoScalingGroupName,Timestamp,Type,Version,Sources,ContainerInstanceId,InstanceId,


## Conclusion


In this tutorial, you installed an[OpenTelemetry Collector](https://signoz.io/guides/opentelemetry-collector-vs-exporter/) to collect AWS ECS metrics and logs and sent the collected data to SigNoz for monitoring and alerts.


Visit our complete guide on OpenTelemetry Collector to learn more about it. OpenTelemetry is quietly becoming the world standard for open-source observability, and by using it, you can have advantages like a single standard for all telemetry signals, no vendor lock-in, etc.


SigNoz is an open-source[OpenTelemetry-native APM](https://signoz.io/blog/opentelemetry-apm/) that can be used as a single backend for all your observability needs.


---


**Further Reading**


-


[Complete Guide on OpenTelemetry Collector](https://signoz.io/blog/opentelemetry-collector-complete-guide/)


-


[Amazon EKS monitoring with SigNoz](https://signoz.io/blog/eks-monitoring-with-opentelemetry/)
