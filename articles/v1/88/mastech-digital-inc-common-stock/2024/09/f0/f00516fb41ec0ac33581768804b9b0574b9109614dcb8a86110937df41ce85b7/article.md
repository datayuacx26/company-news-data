---
schema_version: "1.0.0"
document_id: "f00516fb41ec0ac33581768804b9b0574b9109614dcb8a86110937df41ce85b7"
company_key: "mastech-digital-inc-common-stock"
company: "Mastech Digital Inc"
source_id: "mastech-digital-inc-common-stock-news-import-435cf6065742"
canonical_url: "https://www.mastechdigital.com/blog/artificial-intelligence-digital-business-transformation/"
published_at: "2024-09-04T07:27:33+00:00"
first_seen_at: "2026-07-24T10:53:16.419034+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:452752352be512bf586e5881218289b5c58f5f6dd9b5d59645fd09e36ba4f298"
---

# Informatica Data Integration | Intelligent Cloud Services – Issues & Best Practices

Informatica Intelligent Cloud Services (IICS) is the[cloud-based data integration](https://www.mastechdigital.com/cloud-crm/) platform that provides various features such as enterprise data integration, application integration, and API management between cloud and on-premise systems.


This Informatica Data Integration platform brings in faster time-to-value (TTV) against your investment in a data integration tool. You don’t need to buy software licenses and servers to install the software, but you only pay for the usage of service. Faster TTV means you get started with building your data integration applications as soon as you have a subscription.


One of the biggest strengths of Informatica Cloud is that it enables business users to create integrations via easy-to-use tools. This makes it easy to create integrations and get faster results.


This blog highlights a few common issues users may encounter while working with the Application Integration module of IICS and the best practices we’ve adopted to overcome those issues. These best practices will help users save time on troubleshooting, increase efficiency and insight quality, and enhance product ROI.


## **Issues & Best Practice**


### **1.** **Salesforce Connection Object Runs on Secure Agents Other than Cloud Server**


When we assign a run-on environment as a secure agent, we are likely to face the following runtime error message if the connection object is used inside the process:


Invoke Create_contact_object: Completed with fault: subLanguageExecutionFault (Error executing expression (hutil:getConnection(string($dataSources/source\[ $counter \]))). Reason: The connection meta-data for SFDC was not found. Please try to re-publish the connection.)


The reason is, the SFDC connection object generally will have hundreds of dependent objects which may not be published completely when you use a run-on environment as one of your secure agents. Hence, when the process gets executed it may not have the full SFDC metadata available from the connection object to add/update/delete the fields.


**Solution:** Use the run-on environment as a cloud server in the connection object and publish again.


### **2. Process/Sub-process Publish Issue**


There may be inconsistency in process and subprocess publishing. In that case, when the parent process tries to invoke a subprocess during execution, we might get the following error:


Invoke P_SUB_PROCESS_NAME: Completed with fault: AeInvokePrepareException (Failed to initialize process invoke! Process ‘urn:screenflow: process: P_MAIN_PROCESS_NAME: P_MAIN_PROCESS_NAME’ (pid: #) in tenant $public.


**Solution:** Publish the parent process as well while publishing the sub-process that has changed. So, the best practice is to publish the parent process because it will invariably publish all its dependent objects.


### **3. Checkbox Field Not Being Populated Properly**


When your target field is of a checkbox type, such as an SFDC checkbox type, and if you give binary values such as 1 for check and 0 for uncheck, the target field won’t be populated.


**Solution:** Always use the checkbox variable to map to checkbox fields. Alternatively, we can consider the checkbox field as a “Boolean type” with the values as true for check and false for uncheck; then a formula field can be used to derive true/false values based on requirements/conditions as shown in the screenshot below:


### **4. Reading/Parsing JSON Messages as Input in a Process**


To parse JSON messages, we can use Process Objects. The process object is a data structure of how the XML/JSON can be stored in each message. It constitutes a group of variables of various types, which will be validated against the input. The order of variables does not matter but the type should match the input type.


The screenshot given below displays the sample process object. We can create input/output/temporary variables (process object type), which we have already defined, inside a process – in this case, Address. Process objects generated inside a service connector are available only within the service connector. Standalone process objects are available for use in any service connectors and/or processes.


When you use “Process object” as a type, you can directly use XQuery/XPath to easily parse the JSON/XML. For example, you can have “Address JSON” as an input.


\[{“ADDRESS”:{“ACTIONTYPE”:”ADD”,”ACTIONDATE”:”2018-05-29T10:19:02.800″,”CONSTITUENTLOOKUPID”:”9995565″,”ADDRESSTYPE”:”Home”,”COUNTRY”:”United States”,”ADDRESS”:”8333 Arnold St”, “CITY”:”Dearborn Heights”,”STATE”:”WY”,”ZIP”: “48127-1218”}}\]


Obtain Zip Code using Xpath as;


temp_Zip = Address\[1\]/ZIP


Note: JSON/XML messages can also be parsed without “Process objects”, but the usage of “Process objects” is advisable.


### **5. Publish Timed Out Error**


When we try to publish any object, be it connection-related objects or even a process, and if it takes more time than the usual, the system will probably throw a publish timeout error. Even when this error message is displayed, Informatica will try to publish the object again and again in the backend. We can see the publish status after 5-10 minutes. If there are no changes in the publish time, it means that the publish activity has probably gone to a death path.


Try to add URN Mapping for the respective secure agent in the console as follows:
1. Go to the Application Integration console.
2. Select the “Deployed Assets” tab and the respective secure agent from the drop-down menu.
3. Add the following URN and URL pair and select the “Add” button.
a. URN: ae:agent-response-mechanism
b. URL: async **URN Mappings**


After adding the URN mapping, if you get an error message while trying to re-publish, even after a long interval (after encountering the above error message), it confirms that your previous publish attempt has gone to a death path. A death path is an issue that we face during publishing an object that never ends the process of publishing it. It can have many reasons such as:
• Need of accurate metadata information while creating a connection object
• Patch related issue that can be fixed only with consultation with Informatica support


However, we cannot view these publish tasks running in the backend from the new UI of Informatica Cloud. Follow the steps below to view the older UI version:


1. Login to IICS and launch the Application Integration console.
2. Copy the URL and paste it in an adjacent browser tab.
3. Do not click on enter yet. Edit the URL to remove everything after the “/activevos”, until end of it, and then click on enter to submit the page.
4. This should open the traditional version (Old UI) of the process console related to your organization
5. Search for “Publish Process” in the Active Processes selection filter, by unchecking the “Hide System” and the “Hide Public” checkboxes.


6. You will find some “Publish” processes, like Publish, PublishProcess in the running state. Click “Publish Process”, and verify the input variable to confirm if that is the connection you initiated from the publish activity.
7. Once you confirm the above and choose to kill the background process, assuming it is hanging for several hours together (which is not ideal), you can terminate the “Publish” processes from the above “Active Processes” listing section, by choosing these processes and choosing the Terminate action from the dropdown.
8. Then, you can try to publish the object again.


### **6. Connections Instances to Queue being Dropped / Event-Driven Process not being Triggered on Message Publish**


If the secure agent machine on which the queue connection object is published underwent a reboot or if there has been a connectivity issue, the connection instance (consumer) to a queue may be lost. This will result in a situation where the attached process doesn’t get triggered even after messages are being published into the Queue.


**Solution:** Try to re-publish the connection object and once you do that, you should be able to see a connection instance (consumer) to the corresponding queue


### **7. Error Handling – Suspend on Faults**


Users can choose to “suspend the process” when there is a fault, by enabling the checkbox in the process Advanced tab. In such cases when there is a fault, the process will suspend with the status as Suspended (faulting) in the console.


The significance of using “suspend on faults” in a process is mentioned below:
• Allows the process to be suspended rather than terminated when something goes wrong
• Suspended processes can be filtered in Application integration console
• Extensive support for fixing process
o Activities within the process can be retried through manual option from the point of failure to
resume the suspended process
o Data in the input fields can also be set/reset before resuming the suspended process


## **Conclusion**


The Application Integration module is used because of its real-time features. The real-time feature is achieved via processes and guides in the integration process. RabbitMQ is used as a source and Salesforce as a target in all the cases discussed above.


As a leading Data & Analytics services provider, Mastech InfoTrellis – a business unit of Mastech Digital – is helping companies around the world in their digital business transformation initiatives. If master data management, enterprise data integration, or big data and analytics are on your agenda, get in touch today for an in-depth consultation.


**Rajendran V**


Technical Consultant
