---
schema_version: "1.0.0"
document_id: "d6d91a37c023666b64895fc3677cb6c5cedb3191f40961163b071fc6e5e1e4ec"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/service-bridge-remote-record-producer-getting-the-display-value/ba-p/3018717"
published_at: "2024-08-22T14:37:50+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:25:54.866601+00:00"
content_hash: "sha256:4133698f8e0fb43753528e2102b8a9997fdc008f8a4373f88d43fbeedaa1ec53"
---

# Service Bridge - Remote Record Producer: Getting the display value of a reference variable

Remote Record Producers in Service Bridge for Providers application are service requests published in consumer instances. They enable consumers to request provider services through their service catalog via Service Bridge. Remote Record Producers use questions also known as variables to gather information that gets submitted with a request. One of the variable types used is a reference type. It contains a reference to a record in the instance. The value of the record selected in the reference variable is the SysId of the record. When a Remote Record Producer is submitted it creates a Provider Task and the answers to the variables in the consumer instance. The Provider Task and answers are then synced to the provider instance. In the consumer instance the record selected in the reference variable exists. In the provider instance the record selected in the reference variable may not exist.


Let’s walk through an example. In this example a Remote Record Producer has a reference variable called “Select Computer” that is linked to the computer table \[cmdb_ci_computer\]. The consumer selects a computer and submits the Remote Record Producer.


Submitting the Remote Record Producer creates a provider task and the associated answers in the consumer instance. Because the computer exists in the consumer instance, the “Select Computer” reference variable has a connection to the record and has access to the attributes of the computer such as name, serial number, asset tag, etc.


The provider task and answers are synced to the provider instance. In the provider instance, the computer in the “Select Computer” reference variable does not exist therefore it does not have a connection to the computer record or access to the record attributes. The “Select Computer” variable appears to be empty but it contains the SysId of the computer from the consumer instance. If the computer did exist in the provider instance, it would be connected and attributes available.


To get the display value of the computer, the provider can add a string variable to the Remote Record Producer and use Auto-Populate feature of catalog variables to get the value from the reference variable.


When the consumer selects a computer in the “Select Computer” reference variable the “Consumer Computer Name” string variable will be auto populated with the computer name. The Consumer Computer Name variable could also be hidden or set to read-only.


After submitting the Remote Record Producer with the additional variable, a Provider task and associated variables are created in the consumer instance.


The provider task is synced to the Provider instance and the Consumer Computer Name is available.


On a side note: Sometimes the required data is in the Provider instance. In this case the Remote Record Producer reference variable could be turned into a Remote Choice that allows selection of provider data in real-time from the consumer instance during runtime. See[Remote Choice](https://docs.servicenow.com/csh?topicname=service-bridge-v2-create-remote-choice-fld-defs.html&version=latest) for more information.
