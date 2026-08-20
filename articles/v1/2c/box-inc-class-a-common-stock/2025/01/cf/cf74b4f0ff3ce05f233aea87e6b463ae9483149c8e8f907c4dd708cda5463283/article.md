---
schema_version: "1.0.0"
document_id: "cf74b4f0ff3ce05f233aea87e6b463ae9483149c8e8f907c4dd708cda5463283"
company_key: "box-inc-class-a-common-stock"
company: "Box Inc."
source_id: "box-inc-class-a-common-stock-rss-6b6ba587c738"
canonical_url: "https://medium.com/box-tech-blog/box-it-blog-series-how-we-deploy-aws-network-technologies-inside-box-it-eec13fbb8363"
published_at: "2025-01-28T20:46:56+00:00"
first_seen_at: "2026-07-20T04:36:04.074136+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:3de2cba485bf03e060d8528bc168873f73b34d1deedd2de8f4f6ac91a35bf4e4"
---

# Box IT Blog Series: How We Deploy AWS Network Technologies Inside Box IT

# Box IT Blog Series: How We Deploy AWS Network Technologies Inside Box IT


[Zhen Chen](https://medium.com/@zenkchen?source=post_page---byline--eec13fbb8363---------------------------------------)


17 min read


·


Jan 28, 2025


--


Press enter or click to view image in full size


Illustrated by Navied Mahdavian / Art directed by Erin Ruvalcaba Grogan


Welcome to the Box IT Blog Series, where we dive into our journey of transforming from traditional on-premises IT data centers to AWS Cloud technologies. Historically, Box IT relied on datacenter-grade routing, switching, and network security solutions to build and manage robust on-premises networks. Transitioning to AWS introduced a paradigm shift — many of the familiar concepts from traditional network infrastructure no longer applied in the cloud’s virtualized environment. For network engineers accustomed to physical hardware, cable management, and conventional high availability (HA) and resiliency methods, the shift posed unique challenges. In this series, we’ll share how Box IT embraced AWS-native technologies, followed best practices, and redefined our network architecture to meet the demands of the cloud era.


### Traditional Data Center Design Overview


To better understand how Box IT has utilized AWS network technologies, it’s crucial to first revisit our starting point: the traditional data center model. By exploring the key components of this setup and their role within the cloud ecosystem, we can establish a solid foundation for our migration journey and highlight the unique challenges and opportunities it presented.


In traditional network design, the layout of an IT data center can vary significantly depending on organizational needs. However, several foundational components are universally critical for ensuring secure, high-performance, and efficient operations. These components form the backbone of a on-premises network infrastructure:


- Border Network: The gateway to the Internet, this component interfaces directly with the ISP, managing inbound and outbound traffic. As the entry and exit point for external communication, it is crucial for handling and directing network traffic securely and efficiently.
- Security Protection: In today’s threat landscape, robust security measures are non-negotiable. Application firewalls are a standard defense, providing a shield against external threats and controlling the flow of traffic into sensitive parts of the network.
- Intra Data Center Connection: The core of internal data traffic, this component demands high-performance, low-latency, and highly available networks. Modern switching and routing technologies ensure seamless internal communication to meet the demands of contemporary applications.
- Interconnection with Other Data Centers or Office Networks: For organizations with multiple data centers or remote office networks, reliable interconnectivity is essential. These connections, often built with high-capacity links and secure protocols, enable smooth resource sharing and data integrity across all sites.


Each of these elements is designed with principles of high performance, high availability, resiliency, and cost efficiency. As businesses increasingly adopt cloud solutions, it’s worth exploring how cloud-based technologies address these traditional principles, delivering cutting-edge network solutions that align with today’s operational needs. Let’s dive deeper into this evolution!


### Border Network: From Traditional Border Routers to AWS Internet Gateways (IGW) and NAT Gateway (NGW)


In traditional network setups, the border network layer relies on border routers to connect to multiple ISPs using EBGP for redundancy and traffic load sharing. Additionally, due to the global shortage of IPv4 addresses, network teams typically deploy NAT at this layer to translate RFC1918 private IP addresses into public, routable IPs for internet communication.


AWS simplifies the management of internet connectivity with Internet Gateways (IGWs) and NAT Gateways (NGWs). According to AWS, *“An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet.”* Once an IGW is deployed and attached to a VPC, AWS handles all aspects of internet connectivity, including high availability and redundancy — no additional configuration is needed. It’s a hassle-free solution that eliminates much of the manual effort required in traditional setups. More IGW detail can be found[here.](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)


Press enter or click to view image in full size


The IGW in AWS serves a role similar to border routers in on-premises networks. It facilitates internet access for resources within the VPC. Complementing this, the NAT Gateway is critical for enabling internal resources, such as instances in private subnets, to access external services. As AWS explains, *“You can use a NAT gateway so that instances in a private subnet can connect to services outside your VPC, but external services cannot initiate a connection with those instances.”* While not a direct equivalent to an on-premises internet firewall, an NGW performs all outbound NAT functions, ensuring seamless internet access for private subnets.


Unlike IGWs, NGWs are tied to specific Availability Zones. If an AZ experiences an outage, the NGW in that zone becomes unavailable. To mitigate this, AWS strongly recommends deploying an NGW in each AZ to maintain high availability. Setting up an NGW involves specifying a subnet within the desired AZ and assigning an Elastic IP (EIP) — a public IPv4 address reserved in your AWS account. EIPs are essential for creating public-facing services and enabling NAT functionality for NGWs. For more details, refer[here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) . Note that EIPs are a paid resource, so their usage should be planned carefully.


The NGW creation dashboard, illustrated below, emphasizes these two essential components for the setup: the subnet and the Elastic IP. Both are critical for configuring the NGW.


Press enter or click to view image in full size


The diagram below illustrates a typical setup for planning your VPC’s internet-facing layer, incorporating both IGWs and NGWs.


Press enter or click to view image in full size


In summary, AWS offers a robust, scalable, and highly available approach to managing the internet-facing layer of your VPC. IGWs simplify the process of establishing external connectivity, while NGWs provide secure, controlled internet access for private subnets. These tools, when used strategically, modernize and streamline border network management in the cloud. Up next, we’ll explore additional AWS networking solutions!


### Security Protection: AWS Network Firewall for Layer 7 Protection


In contrast to the traditional on-premise model, where security at the network border relies heavily on hardware firewalls and Access Control Lists (ACLs) on routers and switches, AWS reimagines security with a broad suite of native tools designed for the cloud. On-premises setups typically achieve high availability (HA) through firewalls operating in HA mode with Virtual IPs (VIPs) and rely on traffic logging for troubleshooting and maintaining oversight. AWS provides similar functionalities but shifts the focus to cloud-native solutions, offering more flexibility and scalability. Let’s delve into these tools and examine their advantages and trade-offs.


One of the first security tools encountered in AWS is the Security Group, defined by AWS as *“a set of firewall rules that controls the traffic to and from your instance. Inbound rules control the incoming traffic to your instance, and outbound rules control the outgoing traffic from your instance.”* However, from a network security engineer’s perspective, this description oversimplifies their role. Unlike traditional firewalls, Security Groups do not enforce segmentation or protect systems within specific network zones. They also lack the granularity to implement complex permit/deny logic, which is often essential in enterprise environments. If you’re familiar with IPtables, Security Groups function in a similar manner.


Despite their constraints, Security Groups are indispensable in any AWS security architecture. They excel at controlling east-west traffic (internal traffic between resources), a capability not fully addressed by other AWS features. With their default “deny-all” policy, they allow only explicitly defined traffic, ensuring a baseline level of security. Below is a typical Security Group configuration interface, which supports only allow rules but enforces a deny-all policy by default.


Press enter or click to view image in full size


Network ACLs (NACLs) function as stateless filters at the boundary of each network segment, controlling both inbound and outbound traffic. Unlike Security Groups or AWS Network Firewall (NFW), NACLs require explicit rules for traffic in both directions to ensure proper operation ( stateless vs stateful ).


For network engineers, NACLs resemble traditional Access Control Lists (ACLs) on devices like Cisco. However, they come with limitations in granularity, particularly when it comes to protocol-specific rules. For instance, NACLs cannot filter traffic based on specific TCP flags, such as allowing only TCP SYN packets. This makes them less suitable for complex security requirements often encountered in enterprise environments, where advanced firewall technologies are preferred.


Despite these constraints, NACLs offer a cost-effective solution for basic traffic control, as they are provided at no additional charge. While they may not replace full-featured firewalls, they can serve as a useful tool for straightforward access management. For further details on configuring NACLs, refer to the official AWS[documentation](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-rules) . The following is example to setup NACL:


Press enter or click to view image in full size


While Security Groups and NACLs provide basic security controls in AWS, they lack the advanced Layer 7 capabilities required for deep packet inspection and application-level filtering. To address this gap, Box IT adopted AWS Network Firewall (NFW). According to AWS, *“AWS Network Firewall is a stateful, managed, network firewall and intrusion detection and prevention service for your virtual private cloud (VPC) that you create in Amazon Virtual Private Cloud (Amazon VPC).”*


Working with AWS firewall technology can be quite challenging. Why? In most enterprises, there is typically a preferred firewall vendor in place, and the first question to address is whether to stick with the same brand for the AWS setup. Keeping the same brand offers benefits such as simplified management, reuse of existing firewall policies, and easier support from the team already familiar with the solution. However, when you explore deploying third-party firewalls in AWS, replicating the on-premises setup often proves complex and difficult. For instance, on-premises setups typically use a pair of firewalls in HA mode to handle single points of failure. Implementing similar setups in AWS can be challenging, requiring creative solutions to ensure resilience and redundancy.


At Box IT, we prioritized simplicity, efficiency, and cost-effectiveness, which made AWS’s native firewall the ideal choice. AWS NFW is straightforward to set up, requires minimal management, and is highly scalable. However, its operation differs significantly from traditional on-premises setups, introducing a learning curve for network engineers.


One of the primary challenges with AWS NFW lies in maintaining symmetric routing. In traditional environments, HA firewalls typically serve as the default gateway, ensuring that traffic flows symmetrically — following the same path in both directions. This setup minimizes routing complexity and avoids issues with stateful inspection.


In AWS, symmetric routing is essential but requires meticulous planning. Consider the following example, which aligns with AWS’s recommended design principles. To protect customer-built systems that face the Internet directly, AWS strongly recommends using an Application Load Balancer (ALB) in the Public Zone, which serves as the direct interface with the Internet. The customer-built systems can then reside in a DMZ zone behind a well-architected AWS Network Firewall.


Press enter or click to view image in full size


In this setup, both intra-zone traffic and cross-zone traffic come into play. For instance, an ALB deployed in AZ1 can route traffic to backend EC2 instances in both AZ1 DMZ and AZ2 DMZ to achieve optimal load balancing and high availability.


- Intra-zone traffic is straightforward, as it always passes through the NFW within the same Availability Zone.
- Cross-zone traffic, however, introduces more complexity. There are two independent firewalls — AZ1 NFW and AZ2 NFW — which do not share state information.


When traffic flows from the ALB in AZ1 to an EC2 instance in AZ2, it may take one of two possible paths:


1. AZ1 ALB → **AZ1 NFW** → AZ2 EC2
2. AZ1 ALB → **AZ2 NFW** → AZ2 EC2


While both paths may initially function, it is crucial to ensure that return traffic follows the same firewall it originally passed through. If return traffic traverses a different firewall, it will be dropped due to stateful inspection. Managing this subtle but critical routing requirement is essential for maintaining a reliable and secure AWS architecture.


To clarify, there’s no issue with choosing either Option 1 or Option 2; however, it’s vital to enforce a rule to maintain symmetric routing at all times. For instance, you can always prioritize the AZ1 firewall for cross-zone traffic. This approach helps ensure consistent routing behavior and avoids unintended traffic drops.


A quick note on cross-zone traffic, as demonstrated here: AWS typically charges for cross-zone traffic, whereas intra-zone traffic is usually free. However, when a Load Balancer is involved, the cost model changes and differs from standard cross-zone charges. You can find more details[here](https://aws.amazon.com/blogs/networking-and-content-delivery/exploring-data-transfer-costs-for-classic-and-application-load-balancers) . That said, it’s always a good practice to be cautious and mindful of any cross-zone traffic that might be triggered.


A significant learning curve for network security engineers transitioning to AWS Network Firewall (NFW) is mastering Suricata-based firewall policies. Suricata may not be familiar to most network security engineers, but compared to other major firewall vendors that offer a “black box” solution, Suricata is an open-source intrusion detection (IDS) and intrusion prevention (IPS) system. Since late 2020, Suricata IPS rules have been part of the AWS Network Firewall service. For more details, you can find more information[here](https://aws.amazon.com/blogs/opensource/scaling-threat-prevention-on-aws-with-suricata/) .


When setting up NFW policies, you are presented with three options below:


Press enter or click to view image in full size


Compared to standard stateful rule or domain lists, creating Suricata rules is notably more complex. Here are a few examples to illustrate the format and functionality, more details can be found[here.](https://docs.aws.amazon.com/network-firewall/latest/developerguide/stateful-rule-groups-ips.html)


1. **Blocking malicious site traffic** : drop tls any any -> any any (tls.sni; content:”.badsite url”; nocase; endswith; sid:100;)
2. **Allowing DNS traffic** : pass dns $PRIVATE any -> $DNS 53 (msg:”allow DNS”;sid:200;)
3. **Allowing HTTPS traffic to the Internet for internal hosts** : pass tcp 0.0.0.0/0 any -> !$RFC1918 443 (flow:to_server, established; sid: 300;)


Initially, working with these rules may feel daunting, particularly for those accustomed to simpler rule-based interfaces. However, once familiarized with the syntax for pass, reject, and drop actions, the process becomes more intuitive. At Box IT, our network team quickly adapted to Suricata’s structure after overcoming the initial learning curve, leveraging its powerful filtering capabilities effectively.


Another critical component of any effective security stack is the ability to log and categorize events. There is a wide range of both commercial and open-source tools available, and AWS is also making strides in this area. While AWS Network Firewall offers logging capabilities, these are still behind the more advanced tools provided by leading third-party firewall vendors. For effective troubleshooting, it’s crucial to have administrator-friendly logging with real-time analysis and comprehensive debugging tools, which NFW currently lacks.


NFW does offer basic logging, which can be enabled during setup, but its functionality is limited for in-depth troubleshooting or incident response. Enhancements in this area would significantly boost NFW’s usability for enterprise-level network management. Below is an example of NFW logging, more details about logging can be found[here](https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-logging.html)


Press enter or click to view image in full size


Despite these limitations, with the right training and experience, teams can effectively manage and optimize AWS NFW policies, ensuring both security and performance in cloud environments.


By addressing these challenges and leveraging the capabilities of AWS NFW, Box IT has achieved a secure and scalable solution tailored for cloud environments.


### Intra-Data Center Connections in AWS: What Happened to VRRP/HSRP?


Now that we’ve reviewed the overall design and components of an AWS border network and security, let’s turn our attention to intra-data center connectivity. In AWS, this type of connectivity is managed differently compared to traditional on-premises setups, though there are still some familiar elements. In traditional networks, VRRP (Virtual Router Redundancy Protocol) and HSRP (Hot Standby Router Protocol) are essential for ensuring Layer 2 redundancy and gateway failover. These protocols provide seamless failover by assigning a virtual IP to active network devices, minimizing disruptions. How do these configurations translate into AWS?


Before addressing this question, let’s examine the typical network setup for an EC2 instance in AWS. When launching a new EC2 instance, there are three fundamental network settings to configure:


1. Network: Specifies the virtual network (VPC) where the EC2 instance will reside, effectively determining its logical location.
2. Subnet: Assigns the primary network interface of the EC2 instance, ensuring proper connectivity within the selected Availability Zone.
3. Auto-Assign Public IP: An optional setting used if the instance requires internet-facing access.


Also, as a network engineer, you’ve likely come across the term *“multi-homed host”* — a server equipped with multiple network interfaces and IP addresses, allowing it to connect to multiple networks simultaneously. Does AWS support this functionality? Absolutely. AWS enables multi-homing through its EC2 network settings.


To configure multiple network interfaces for an EC2 instance, simply access the advanced network configuration options during setup. For a detailed guide on setting this up, refer to[this](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/scenarios-enis.html) . This capability is particularly useful for scenarios like separating management traffic from application traffic or connecting to different subnets within a VPC.


Press enter or click to view image in full size


By now, you may have realized that AWS does not utilize network-level HSRP or VRRP configurations. So, how does AWS handle failure?


Instead of relying on traditional IP or MAC-level redundancy, AWS advocates implementing failover and redundancy mechanisms at a higher level. The most straightforward approach is to use an Elastic Load Balancer (ELB) and distribute services across multiple Availability Zones (AZs). In the event of an AZ failure, the servers in the remaining AZs automatically take over, ensuring seamless service delivery. This load-balancing strategy is integral to Box IT’s high-availability architecture, enabling services to remain operational even during component failures.


AWS also highlights that the Elastic Load Balancer is inherently designed for high availability, eliminating the need for additional HA mechanisms. For more details, refer to[here](https://aws.amazon.com/elasticloadbalancing/features/) .


If you prefer a system design based on Virtual IPs (VIPs), AWS supports this approach, but it requires more complexity compared to using an ELB. To implement HA failover with multiple instances across AZs using VIPs, refer to the relevant AWS documentation[here](https://aws.amazon.com/blogs/apn/making-application-failover-seamless-by-failing-over-your-private-virtual-ip-across-availability-zones/) .


But what happens if an entire AWS region faces challenges? While rare, such incidents have occurred. The solution is failover to another region. AWS’s global infrastructure spans multiple regions, enabling you to configure failover from one region to another. This ensures uninterrupted service even during a regional failure. Selecting a backup region depends on your performance and cost requirements, and for additional redundancy, you could even consider another cloud provider.


Ref:[AWS Regions and Available Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)


The key takeaway is to think ahead, plan for resilience, and choose a failover strategy that aligns with your business needs. The flexibility of AWS, coupled with thoughtful design, ensures robust and highly available systems.


### Interconnection with Other Data Centers or Office Networks: Achieving Redundancy with Transit Gateway


Managing enterprise networks can be a complex endeavor, especially for organizations with multiple locations and diverse network infrastructures. Box is no exception, operating global offices, on-premises data centers, and now incorporating cloud-based data centers via AWS. While traditional office and on-premises data center networks are fully managed and maintained by the network team, AWS offers a different approach. It simplifies much of the heavy lifting by managing the underlying infrastructure. However, administrators still play a critical role in designing and configuring the interconnections between AWS regions and integrating AWS networks with non-AWS environments. This hybrid setup demands strategic planning and expertise to ensure seamless communication across all components.


AWS introduces several key technologies for enterprises to consider when architecting their network interconnections: Virtual Private Network (VPN), Transit Gateway, and Direct Connect.


VPN is a familiar technology for most network engineers, widely used to establish secure connections between site A and site B. In AWS, VPN connections can be configured between AWS VPCs and other non-AWS networks using Virtual Gateway or Transit Gateway.


- Virtual Gateway: This was the go-to solution for VPN connectivity in AWS before Transit Gateway emerged. While functional, it lacks the advanced features of its successor — Transit Gateway.
- Transit Gateway: Now AWS’s recommended solution, Transit Gateway acts as a “regional central hub” for connecting AWS VPCs with other non-AWS networks. One key advantage of using Transit Gateway is its built-in high availability (HA), eliminating the need for additional HA considerations for this setup.


For enterprises seeking higher network performance and reliability, AWS Direct Connect provides a dedicated physical circuit between on-premises networks and AWS. It supports load sharing and redundancy by enabling multiple circuits, offering benefits such as reduced latency and consistent performance. Configuring Direct Connect requires collaboration with AWS or its certified providers. While setup involves additional effort, the long-term performance gains often justify the investment.


Interconnecting AWS VPCs is another critical element of network architecture. AWS recommends Transit Gateway Peering as a scalable and efficient solution for enabling seamless data exchange between VPCs. This approach simplifies the architecture while maintaining performance and security standards. In addtion, it is a scalable solution considering future VPCs growth.


Each of these technologies addresses specific challenges in enterprise network management, and they are often used in combination to build robust and scalable architectures. For example, a hybrid network might use Direct Connect for on-premises to AWS connectivity, Transit Gateway Peering for regional VPC interconnectivity, and VPN for secure connections with external non-AWS networks.


Below is an example diagram illustrating how these components work together to create an interconnected enterprise network. As always, careful planning and consideration of specific organizational needs are key to success. By leveraging these AWS technologies, enterprises can streamline their network operations and focus on driving business innovation.


Press enter or click to view image in full size


Box IT leverages the full spectrum of AWS networking technologies, tailoring them to meet the unique interconnection requirements of various sites. For our major offices, which house a significant number of engineers and users, we prioritize setting up AWS Direct Connect. This approach ensures high-performance, reliable connectivity to AWS, supporting the demanding workloads and collaboration needs of these locations. On the other hand, for smaller sales offices with lighter network demands, VPN over the Internet has proven to be a cost-effective and reliable solution. This hybrid strategy enables Box IT to balance performance, cost, and scalability, ensuring optimal connectivity for all office types without unnecessary over investment.


**Final Thoughts: Bridging Traditional and Cloud Network Concepts** Transitioning from traditional on-premises network designs to cloud-based architectures like AWS can be both exciting and challenging for network engineers. The concepts of redundancy, failover, security, and performance remain critical, but the implementation methods often differ significantly. Understanding these differences is the key to designing efficient, resilient, and secure cloud-based systems.


AWS eliminates many of the traditional network configurations such as HSRP/VRRP, relying instead on higher-level redundancy mechanisms like Elastic Load Balancers and multi-AZ deployments. While this approach simplifies certain aspects of high availability, it introduces new complexities, such as managing symmetric routing and learning cloud-native tools like AWS Network Firewall.


Security paradigms also shift with cloud environments. Engineers must adapt from traditional firewalls and ACLs to AWS-specific constructs like Security Groups and NACLs, and embrace the advanced capabilities of tools like the AWS Network Firewall. While these tools offer powerful features, they often come with a learning curve, requiring teams to invest time in understanding and implementing them effectively.


The absence of region-wide redundancy in traditional terms pushes engineers to think globally, leveraging AWS’s regional and multi-cloud capabilities for failover and disaster recovery. This forward-thinking approach ensures system resilience even during rare but impactful events like regional outages.


Ultimately, bridging the gap between traditional and cloud networking concepts requires a mindset shift. It’s not just about translating existing designs into the cloud; it’s about embracing the opportunities and flexibility cloud platforms offer to create robust, scalable, and future-ready architectures. By blending the best practices from both worlds and continuously adapting to evolving cloud technologies, network engineers can drive innovation and reliability in modern infrastructure.


Cloud networking is not a replacement for traditional networking — it’s an evolution. And with the right tools, strategies, and mindset, you can confidently navigate this transformation.
