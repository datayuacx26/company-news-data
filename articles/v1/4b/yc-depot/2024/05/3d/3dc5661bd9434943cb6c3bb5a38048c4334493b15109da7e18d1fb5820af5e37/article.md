---
schema_version: "1.0.0"
document_id: "3dc5661bd9434943cb6c3bb5a38048c4334493b15109da7e18d1fb5820af5e37"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/faster-ec2-boot-time"
published_at: "2024-05-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:62ca43bd09ddf4359abada5e640a10290a5b9f375e3f85a62e1012c6e3ebeac1"
---

# Making EC2 boot time 8x faster

It is possible to increase the speed at which EC2 instances boot! This can be critical for certain types of workloads, where a fresh EC2 instance is needed to process some request or task. At Depot, we accelerate builds, so the EC2 boot time directly equals the lag time for a build to start processing.


It can seem like EC2's boot time is fixed, that the time taken to start an instance depends solely on Amazon. However, this turns out not to be the case, there are several ways to speed up the process, especially if you know the workload that will execute on the machine after boot, like a Docker image build or GitHub Actions runner.


We managed to **reduce EC2 boot time from 40 seconds to 5 seconds** by optimizing each step in the instance launch process.


## What takes so long?


When a new EC2 instance is requested with` RunInstances` , AWS performs several different operations to prepare and boot that instance:


- Creating the root EBS volume from the chosen AMI
- Assigning the instance a private IP address
- Choosing a target host for the instance
- Actually booting the machine


Once the instance hardware is powered on, there's also a bootloader, kernel, and userspace processes that need to start. Each of those steps happens with each instance that you launch and before your application or service starts on it.


## Avoiding the problem


In the past, we've optimized the time to start a build by[running a pool of standby compute](https://depot.dev/blog/infrastructure-provisioner-v3) , where incoming build requests are routed to already-running EC2 instances if available. This works well for container builds, where many incoming builds for the same project are routed to the same pool of machines.


While this hides the EC2 start lag from users, it's not economically viable for all workloads. For[managed GitHub Actions runners](https://depot.dev/blog/depot-github-actions-runners) , each job in a workflow is routed to a dedicated EC2 instance. And with matrix workflows, it's possible to launch many jobs in parallel. It's not possible to keep 50 EC2 instances online just to handle one customer's incoming 50-job matrix workflow.


But, launching 50 concurrent EC2 instances with a 40-second boot time is also not acceptable either. So, we had to look at ways to reduce the time it takes to boot an EC2 instance.


## Faster boot times


One thread became common, **not doing something is always faster than doing something** , if that's an option for the specific workload.


We used this theme by systematically optimizing each step of the EC2 instance creation, boot, and application start, to reduce the time to running application from nearly 40 seconds to 5 seconds.


**tl;dr** — boot the instance once, shut the instance down, then boot it again when needed.


### EBS root volume streaming


Preparing the EBS root volume for use is one of the longest and most impactful aspects of EC2 instance boot time and subsequent application performance once the instance has started. When an EBS volume is created from an AMI, the following happens:


- The EBS volume is created, pointing at the AMI snapshot
- When empty blocks are accessed, they receive the full performance of the volume type
- However when data blocks are accessed for the first time, they must be pulled from S3 and written to the volume before they are returned to the instance


This means that the creating of the EBS root volume is "fast", because it does not need to actually copy the contents of the AMI into the volume. But it also means that every file operation that accesses a data block for the first time will experience a large performance reduction as that data is lazily loaded from S3 in the background.


This can especially affect kernel and application start time. Without optimizations, our GitHub Actions runner AMI's first boot can take 5 seconds for the kernel + 5 seconds just for systemd to *load unit files* , before any processes actually start. Then on restart, since the files are already loaded from S3, that whole process takes *less than 400 ms!*


#### AWS's recommended solution


AWS[documents this behavior](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-initialize.html) and mentions a possible solution: it's possible to preload all data blocks by forcing each of them to be read once. They recommend using[fio](https://fio.readthedocs.io/en/latest/fio_doc.html) to access each block in parallel:


```text
#!/bin/bash
volume  =  /dev/xvdf
sudo   fio   --filename=  $volume   --rw=read   --bs=1M   --iodepth=32   \
--ioengine=libaio   --direct=1   --name=volume-initialize
```


This will cause each data block to be accessed and fully streamed from S3 onto the EBS volume. There are however two potential issues with this approach:


1.


It's slow to access every block - for a 100 GB gp3 volume, which has a default throughput of 125 MB/s, running the` fio` above would take *over 13 minutes* ! (800 seconds)


2.


It's not possible to launch a new EC2 instance with an existing EBS volume. Instead a new EBS volume is always created with the instance is launched.


However, we can combine this solution with the next approach: booting the instance once before it's actually needed.


### Start the instance once


While it's not possible to launch a new EC2 instance with an existing EBS volume, it is possible to stop an EBS-backed instance and then start it again.


From a billing perspective, AWS does not charge for the EC2 instance itself when stopped, as there's no physical hardware being reserved; a stopped instance is just the configuration that will be used when the instance is started next. Note that you do pay for the root EBS volume though, as it's still consuming storage.


Therefore, it's possible to boot an EC2 instance once, let it perform whatever initialization it needs, then stop the EC2 instance, and this creates a "warmed" EBS root volume!


Besides warming the EBS volume, this also caches two other steps of the boot process:


1. The configuration for the instance (tags, launch options, user data, security groups, etc)
2. The private IP address assigned to the instance


This means that when the instance is started again, not only will it start with a pre-warmed EBS root volume, but also with a pre-cached instance and network configuration.


**Booting the EC2 instance is equivalent to an optimized version of` fio` pre-warming!** The first time the instance starts, it will access just the data blocks that are relevant to booting the instance. Then for the next boot, those data blocks will already be loaded on the volume!


This makes warming very fast, rather than spending 11 minutes to read *every* data block, it takes less than 30 seconds to perform the initial boot / warming.


#### Autoscaling warm pools


AWS offers something very similar to this approach called[warm pools for EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-warm-pools.html) . This allows you to define a certain number of EC2 instances inside an autoscaling group that are booted once, perform initialization, then shut down, and the autoscaling group will pull from this pool of compute first when scaling up.


While this sounds like it would serve our needs, autoscaling groups are *very slow* to react to incoming requests to scale up. From experimentation, it appears that autoscaling groups may have a slow poll loop that checks if new instances are needed, so the delay between requesting a scale up and the instance starting can exceed 60 seconds. For us, this negates the benefit of the warm pool.


Since we need the best launch performance, we launch EC2 instances directly with the` LaunchInstances` and` StartInstances` API calls instead.


### Resizing the instance


The final boot time optimization involves changing the instance type of a warmed and stopped instance. Since a stopped EC2 instance is "just" an EBS volume + the instance configuration that will be used whenever the instance is started again, it's possible to[change that instance's type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html) with` UpdateInstance` before starting it again.


This is important for two reasons:


1.


You can use cheaper instance types to perform the warming and avoid consuming AWS capacity for the warming process - we use` t3.large` instance types when first launching the instance.


2.


When AWS has capacity issues and a particular instance type is unavailable, it's possible to update the instance type to another one with availability and still use the pre-warmed EBS root volume.


It's also possible to update the provisioned IOPS and throughput for certain EBS volume types, including gp3, if different performance profiles are needed during the warming or running phases.


## The whole flow


For us, that means that our GitHub Actions runner instances go through the following flow:


1. Created as a` t3.large` instance with a root EBS volume
2. Assigned a private IP address in the target VPC
3. The kernel and userspace processes start once, causing the relevant data blocks to stream from S3 onto the EBS volume
4. The instance is stopped
5. When a GitHub job request arrives, the instance type is updated to a` m7a` instance type, depending on how many CPUs were requested, and the instance is started
6. If AWS responds that there is no current capacity for` m7a` instances, the instance is updated to a backup type (like` m7i` ) and started again


That flow reduces the time the instance for a job **from over 40 seconds to under 5 seconds!**


## Followup work


Besides the above, it's possible to further reduce the time from instance start to application running by optimizing what software actually needs to start — for instance, we removed cloud-init from our GitHub Actions AMI as it was unused and contributed heavily to both boot and reboot time. This depends on the specific application and workload though.


Probably the subject of another post, but we also need to handle specific quirks of the GitHub Actions agent API - while we can boot the Actions runner within 5 seconds of a job starting, it can take GitHub 10+ seconds to actually deliver that job to the runner. We've optimized this process already, but there may be further tricks here.


If any of this is interesting to you, feel free to reach out on[Twitter](https://twitter.com/jacobwgillespie) or[Discord](https://discord.gg/MMPqYSgDCg) — always happy to share what we've learned!


## FAQ


How do you reduce EBS root volume streaming latency on EC2?


Boot the instance once to warm the EBS volume, then stop it. When you create an EBS volume from an AMI, data blocks are lazily loaded from S3 on first access, causing significant performance hits. By booting the instance once, you force the relevant data blocks to stream from S3 onto the volume. Then when you start the instance again, those blocks are already loaded. This reduces kernel + systemd startup from over 10 seconds on first boot to under 400ms on subsequent boots.


Can you launch an EC2 instance with an existing EBS volume?


No, you can't launch a new EC2 instance with an existing EBS volume. A new EBS volume is always created when an instance is launched. However, you can stop an EBS-backed instance and then start it again, which reuses the same EBS volume. This is the key to creating warmed instances: launch once, stop the instance, then start it again when needed. When stopped, you only pay for the EBS volume storage, not the instance itself.


Can you resize a stopped instance to a different instance type without losing the warmed volume?


Yes, you can change the instance type of a stopped instance and keep the warmed EBS volume. A stopped EC2 instance is just an EBS volume plus the instance configuration. You can modify the instance type before starting it again, and the pre-warmed EBS root volume will still be used. This lets you warm instances on cheaper types like t3.large, then resize to the actual instance type needed when starting. You can also switch to backup instance types if AWS has capacity issues.


Why not use EC2 Auto Scaling warm pools instead of this approach?


Autoscaling groups are too slow for workloads that need immediate instance availability. From experimentation, autoscaling groups have a slow poll loop that checks if new instances are needed, so the delay between requesting scale up and the instance starting can exceed 60 seconds. This negates the benefit of having warmed instances. For the best launch performance with sub-5-second boot times, you need to launch and manage EC2 instances directly rather than through autoscaling groups.


## Related posts


- [Accelerating builds: Improving EC2 boot time from 4s to 2.8s](https://depot.dev/blog/accelerating-builds-improve-ec2-boot-time)
- [How we launch GitHub Actions runners in 5 seconds](https://depot.dev/blog/github-actions-breaking-five-second-barrier)
- [Reducing time-to-start](https://depot.dev/blog/infrastructure-provisioner-v3)


Jacob Gillespie


CTO & Co-founder of Depot
