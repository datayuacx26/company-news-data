---
schema_version: "1.0.0"
document_id: "986e6d52abdf451fe51949501b87f91e7607230ded34e71caac91d3b141681c9"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/firmware-updates"
published_at: "2023-11-14T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:b4e1e9663024be0b179e4661004ed317f2a7e5375e0ae191f276f79eb7dc60cd"
---

# Firmware Updates: What They Are and Why They Matter

In early 2021, Target’s Cloud & Compute team developed a fully automated firmware update framework for compute hardware used within Target stores. This undertaking was not without its challenges, including the implementation of a new process, scale, maintenance windows, partner communication, testing, and observability. We had to learn and develop a full rollout playbook and automation to accommodate the scale of Target’s platform.


[Photo by Michael Dziedzic on Unsplash](https://unsplash.com/@lazycreekimages?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)


What is Firmware?


Firmware is a type of software that provides low-level control of a device's hardware. It controls basic functions and is responsible for executing the core functions of the device. Firmware is often stored in non-volatile memory devices like ROM, EPROM, EEPROM, and Flash memory. The manufacturers of these devices make continuous improvements to the programs (firmware) responsible for efficiently running the machine.


Why do We Need Firmware Updates?


Firmware updates are important because they can fix defects, improve performance, and protect against security vulnerabilities.


- Unknown defects or bugs can cause devices to malfunction or crash. Firmware updates can fix these problems and make devices more stable. The need for future updates may arise from the interactions between the server and its associated infrastructure, components, applications, and hypervisor.


- Performance can also be improved with firmware updates. Updates can make devices faster and more responsive.


- Security is another important reason to update firmware. Hackers can exploit undiscovered


security vulnerabilities to gain access to devices or steal data. Firmware updates can patch these vulnerabilities and protect devices from attack.


In some cases, device manufacturers may require users to update the firmware before they can receive customer support. This is because firmware updates can sometimes address issues that prevent devices from working properly. By requiring users to update firmware, manufacturers can help ensure that their devices are working properly and that users are receiving the best possible support


Target's Firmware Update Approach at the Edge


Our stores have an


[edge computing](https://tech.target.com/blog/edge-computing) platform that enables the multi-channel retail experience and facilitates store operations. This platform runs applications that support the checkout workflow, in-store product search, team member services, vision services etc.; all of which keep the store running and bring the best possible experience to our guests. Our compute team serves Target technology by providing a productive, reliable, and efficient compute platform. Our infrastructure underpins most of the services behind Target products and a variety of internal infrastructure.


Our team worked hard and developed a comprehensive, fully automated bundle update process that deploys necessary software packages and executables during scheduled maintenance procedures. The implementation of the bundle upgrade is controlled and scheduled through automatic GitHub PR approval process within each store's maintenance window. Once the PR is merged, software executables are downloaded and installed on individual servers, and each firmware upgrade is completed with a reboot.


We set up monitoring and alerting systems, along with automation, to track the progress of each firmware upgrade and verify the servers' health status following the reboot. If any server gets stuck or fails during an upgrade, our support team is immediately available to troubleshoot and fix the problem. If there is a hardware failure that we cannot recover, we work with service partners to replace the hardware.


In addition to tracking the upgrade status, we collaborate with stakeholders and partners to verify and validate their respective workloads and apps. We work with them to schedule the deployment and rollout, taking care to avoid conflicts with other deployments in the store portfolio. To minimize the impact on store operations, we limit the deployment to one node per night per store, ensuring that the application stack continues to run on the other two nodes without affecting the store.


Our entire upgrade process is fully automated, and no manual intervention is required, except in the case of infrequent hardware failures that typically require hardware replacements. We work closely with the original equipment manufacturer (OEM) to identify the required firmware, BIOS, and driver for the quarter and thoroughly test and validate the end-to-end process, including application testing, in a non-production environment before deployment.


The effectiveness of the firmware update on our store's platform is evident in the significant decrease in the number of service calls required per year, even with our older hardware. This implies that the update improved the overall performance and reliability of our platform, resulting in fewer malfunctions and technical issues that would require assistance from our service team. Therefore, the firmware update has proven to be a valuable investment for our company, as it results in improved efficiency and cost savings.


Figure:


Year-over-year service calls.


Challenges


While we recognize the significance of maintaining up-to-date operating systems and applications free from known vulnerabilities, firmware updates often receive a different level of attention and care for the hardware that supports the operating system. Here are some of the challenges associated with firmware updates:


- Time-consuming and risky


:


Updating firmware can be a time-consuming and risky process. This is because the firmware is often stored in non-volatile memory, which means that it cannot be easily changed. If a firmware update is corrupted or fails, it can “brick” the device, rendering it unusable. Users often hesitate to update firmware because of the logistical challenge. The update process typically requires physical access to the device.


- Requires system reboots and downtime


:


Updating firmware often requires system reboots and downtime. This can be disruptive to businesses and users who rely on their devices to be up and running at all times.


- Need for additional tools


:


Additional tools are needed to test and roll out firmware updates. This can add to the cost and complexity of the update process.


- Need to identify the firmware that is currently installed and whether updates are available


:


Automated Services are needed to identify the firmware currently installed on a device and whether updates are available. This can be a time-consuming and error-prone process.


- Need to test and failure scenarios


:


Automated services need to test firmware updates to ensure that they are working properly and have a plan in place for dealing with failure scenarios, such as corrupted or failed updates.


- Need to establish a consistent schedule for regular updates


:


A consistent schedule for frequent firmware updates can take time, requiring coordination with device manufacturers and users.


Overall, there are many challenges associated with firmware updates. These challenges can make it difficult to keep devices up-to-date and secure. However, several tools and resources are available to help overcome these challenges.


Best Practices


Treat firmware updates with the same discipline as software updates


: Firmware updates should be treated with the same level of rigor as software updates. This means establishing standard procedures for testing, deployment, and rollback. It is essential to have a clear understanding of the risks and potential impact of the firmware update before proceeding with the deployment.


Build/invest in test framework and tools for managing rollout and rollback


: It is crucial to have a robust testing framework before deploying firmware updates. This can include tools for automated testing, staging environments for testing, and processes for managing rollbacks in case of unexpected issues.


Build observability and visibility into hardware and firmware:


It is essential to have visibility into the performance and behavior of hardware and firmware. This can include monitoring tools that provide insights into the health and status of firmware and hardware components, as well as logging and alerting mechanisms that can help identify issues early on.


Build in firmware support as a priority for new hardware:


When developing new hardware, it is important to prioritize firmware support from the outset. This can include designing hardware with firmware updates in mind, establishing processes for firmware updates throughout the product lifecycle, and providing tools and resources for firmware developers to ensure quality and reliability.


By following these best practices, we effectively manage firmware updates and minimize risk and potential downtime while improving the performance and reliability of hardware and software systems.


Contributions from VenkataNarayanaswamy M


## RELATED POSTS


### Meet Target’s Stores Deployment Interface that Realizes Distributed Edge Computing at Retail Scale


By Dan Woods, June 20, 2018


At the beginning of 2017... ... our engineering team embarked on a journey to facilitate rapid software delivery to Target stores to better enable innovation and more quickly respond to ever-evolving business needs.


### Chaos Leads to Resilience


By Deepa Kn, November 10, 2022


How Target uses chaos experiments to test the resilience of our architecture.
