---
schema_version: "1.0.0"
document_id: "03ea5d3ad85a4fc43a47235820df0ae5a4870520fa2935dc2e4b90bb376ddfeb"
company_key: "yc-cloudanix"
company: "Cloudanix"
source_id: "yc-cloudanix-rss-5201a91d9952"
canonical_url: "https://www.cloudanix.com/blog/aws-networking-fundamentals-crash-course-for-a-busy-developer/"
published_at: "2020-07-17T00:00:00+00:00"
first_seen_at: "2026-07-27T01:01:37.873246+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:a3ed0df7bb6ba423e5bd996d43a7699671edf6baf93d0298e220937f7b8a06d2"
---

# AWS Networking Fundamentals Crash Course For A Busy Developer

## Introduction


Many developers use AWS as a virtual data center to deploy various services like EC2 instances, RDS instances, etc. These instances or machines need to be stored somewhere in the data center and also need to be connected to the Internet. This is where VPC comes into the picture. Instances can also be referred to as the deployment of service and can also be called a machine throughout this blog.


VPC is an isolated network in a region that spans all the availability zones in that region. So, when you create an AWS account, it provides you with default VPC. Default VPC provides you with few things, these are:


-


IP address ranges/CIDR block


-


Subnets in availability zones


-


Routers to route out to the Internet


-


Security groups and network access control lists (NACL’s) for security


#### I will provide you with a brief idea about:


1. IP addressing
2. Creating Subnets
3. Routing
4. Security


## IP Addressing


The IP address stands for Internet Protocol address. It is a numerical label assigned to each device connected to a computer network either via the Internet or via the intranet. The functions of an IP address are:


- Host or Network Interface Identification
- Local Addressing


AWS provides a range of IP addresses with its default VPC so that anything that will be deployed in that VPC will get a specific IP address from that range of IP addresses. IP address ranges are denoted with the CIDR block. Most IP addresses fall under some RFC ranges. RFC ranges are reserved IP address ranges for private networks. Hence they don’t exist publicly on the Internet, and if you create something in a VPC, it will not show up on the Internet.


So, when you create a VPC in AWS, you specify a CIDR block saying that all the machines that are connected to the network will be deployed in this VPC and will receive an IP address from some RFC.


## What is Subnet?


Another thing that networking developers must be aware of is our subnets. Subnets or subnet masking is a logical division of an IP Network. We call the practice of dividing a network into two or more networks subnetting. IPv4 addresses on the network may be categorized by their Subnet Mask or Netmask. This is also the bitmask when applied by a bitwise AND operator to any IP address on the network, yields the routing prefix.


We also express subnet masks in dotted-decimal notation, like an address. Each deployment scenario in an existing network varies the benefits of subnetting. In the address location architecture of the Internet using CIDR blocks and large enterprises, it is necessary to allocate address spaces efficiently. Subnetting may also enhance routing efficiency.


#### Creating Subnets


We know VPCs are specific to a region and subnets specific to availability zones. Subnets are subnetworks inside a VPC. Since machines are launched in a specific subnet, they receive IP addresses from the address range of Subnets, and subnets receive address ranges from the address range of the VPC. Hence, each subnet belongs to a VPC. By default, IPv4 addresses are assigned, and developers can also configure the machines to receive IPv6 addresses.


## What is Netmask?


Netmask is a 32-bit “mask” used to divide an IP address into subnets and is also used to specify the network’s available hosts. In a Netmask, 2 bits are always automatically assigned. E.g., “255.255” in “255.255.255.0”.


## What is Route Table?


Fundamentals for networking in the routing table are essential for developers using AWS. The route table is responsible for deciding how the traffic flows between subnets. Since it is heavily related to a subnet, if you want to move traffic from a public network to a private network, you need to set up routing tables to define where services can access. Without route tables, we will have chunks of networks with no rules about which devices can communicate among themselves and how they would communicate.


## What is an Internet Gateway?


For a networking developer, the internet Gateway is a fundamental part of networking. Internet Gateway is a component in AWS that, when attached to a VPC, gives it public internet access. To create a public subnet, we need to create a regular subnet first, and we need to update its route table to the Internet. We also need to ensure that a VPC is set up with an internet gateway.


The Internet gateway is confusing when we think about internet access because some services need a network coming in, like a static website. In contrast, other services need traffic that flows out and not in, such as internal private micro-service that needs to pull in dependencies from other internet-based services. To set up an internet gateway, developers need to create a resource and attach it to a VPC.


## What is a Virtual Private Cloud (VPC)?


When you’re working with AWS Networking, the only logical way to separate resources is through a VPC or virtual private cloud. A VPC is a way to lay claim on the machine that belongs to you to prevent them from being accessed by anyone else. When a developer owns a VPC, its resources can only communicate with other VPC resources unless they do some special tricks to connect the VPCs. A developer creates a VPC when they define a range of private IP addresses to allocate to it.


- Routing in a VPC


To communicate between the machines’ addresses, we need to route, and routing tables tell the system how the packets should move around and where to put the next packet. The routing table contains routes that contain the destination or target mapping. Destination here refers to the destination of CIDR, where you want traffic from your subnet to go. The target here refers to the target through which destination traffic needs to be sent.


A default main route table is provided to developers by AWS for every VPC. Developers can even create custom route tables and associate them with the subnet. AWS also creates a default route from communication with the VPC. Every VPC has a default route table, and developers can find out everything inside their CIDR range via that.


#### To connect machines in the specific VPC to the Internet, you need to have


-


Some form of connection to the Internet


-


Some routes to the Internet


-


Some public address


Some machines can only connect to the Internet if it has a public subnet present.


## Security


Security is critical for developers to learn in an AWS networking environment. 3 things will be discussed here:


-


Security groups


-


Network access control lists (NACL)


-


Flow logs (for monitoring, analyzing network trafficking, and debugging network)


#### Security Groups


A security group is a set of networking rules that apply to a resource. It is responsible for defining what traffic enters or leaves certain resources. A single resource can reference many security groups and aggregate unique access types. Also, developers might have a security group that allows HTTP and HTTPS traffic in for a website, though they might not want SSH access for their service. They will ensure that SSH access is limited by IP for any instance and not for the whole Internet. Hence, security groups help developers to limit such instances. Security groups can be defined as distributed firewalls, and they are specific to an instance or a machine.


The security group is also stateful, so if a request comes from one direction, it automatically sets permission to respond to the request that arrives from any other direction. Still, it does not mean that, if you have created an inbound access rule, then you don’t need outbound access.


#### Network Access Control List (NACL)


To explain the Network Access Control List or what we know as NACL in short, in very basic language, we can say that it permits or restricts the set of traffic rules. According to the arrival or departure of the restrictions imposed upon the respective subset, these traffic rules find a place to or from any subset. The fact that NACL is often termed as stateless can be justified by the condition that you have to permit the flow of traffic rules in both the possible directions without any interference.


Since the use of NACL is at the subset level of a VPC, there can be situations where a single NACL can be petitioned with more than one subnet. But the vice versa cannot be attained because each subset has to be associated with only a single NACL. To better understand the use of NACL and security groups, you may glance at the AWS documentation.


For instance, we can consider a scenario involving controlling traffic from any specific network or a range containing a few IP addresses. Such a situation, which is comparatively short in length, is suitable for the use of NACLs. But if the same scenario would consist of complex rules concerning the ports and IP addresses involved, then in that case use of security groups would be a better idea.


#### Flow Logs


Specifically, the[VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html?ref=cloudanix.com) flow logs are responsible for allowing the users to apprehend detailed information regarding the traffic concerned with the IP. This traffic finds its path between the network interferences of the VPC of the user. The working of the flow logs spread up to the areas of VPC, subset, and the instance level. The flow logs feature enables us to shack our data like the network calls to S3 or even the CloudWatch Logs. Once shacked, we can keep a note about the traffic flow. Once you have created your flow log, you can easily retrieve your required data from the desired destination.


Suffice it to say; flow logs consist of your network interfaces, the information regarding the source IP address, the ports both initial and final, and the destination IP address. The flow logs also contain the bytes and the information about the acceptances and rejections.


## Conclusion


No doubt, in the coming years, there will be various entrepreneurs who would thank AWS or Amazon Web Services for the immense growing popularity of their enterprises. So, it’s always a need to have a basic idea about the fundamentals of AWS Networking. The main aim of this blog is to acknowledge the aspiring business developers with the ideologies related to AWS Networking.


At[Cloudanix.com](https://www.cloudanix.com/) , we are building a simple to use AWS audit and real-time monitoring system which will alert you about any misconfiguration or suspected behavior related to your cloud networking. Give our free trial a spin now!


## People Also Read


- [The Different Types of Protection your Application Needs If Hosted in AWS](https://www.cloudanix.com/blog/the-different-types-of-protection-your-application-needs-if-hosted-in-aws)
- [A Big List Of Mistakes AWS Cloud Users Have Done And Spent Millions](https://www.cloudanix.com/blog/a-big-list-of-mistakes-aws-cloud-users-have-done-and-spent-millions)
- [A Practical Step by Step Guide To Prevent Security Breach Of Your AWS Account](https://www.cloudanix.com/blog/a-practical-step-by-step-guide-to-prevent-security-breach-of-your-aws-account)
- [How To Reduce Your AWS Cloud Cost?](https://www.cloudanix.com/blog/how-to-reduce-your-aws-cloud-cost)
- [All You Need To Know About AWS CloudTrail](https://www.cloudanix.com/blog/all-you-need-to-know-about-aws-cloudtrail)
- [Top 13 AWS EC2 Misconfigurations To Avoid in 2022](https://www.cloudanix.com/blog/top-13-aws-ec2-misconfigurations-to-avoid-in-2022)
- [Understanding AWS Cloud Compliance](https://www.cloudanix.com/blog/understanding-aws-cloud-compliance)
- [Top 18 Challenges of Cloud Security in 2026](https://www.cloudanix.com/blog/top-18-challenges-of-cloud-security)
