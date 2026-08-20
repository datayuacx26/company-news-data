---
schema_version: "1.0.0"
document_id: "cf02a63cadfd5fb9afa9c36f07918adad4237126f4755c0f4daf28fb2866d0fc"
company_key: "zoominfo-technologies-inc-common-stock"
company: "ZoomInfo Technologies Inc"
source_id: "zoominfo-technologies-inc-common-stock-rss-ef9b8d7bcd4f"
canonical_url: "https://engineering.zoominfo.com/testing-workflows-as-a-multi-service-our-journey-to-automated-testing-at-zoominfo"
published_at: "2025-01-21T18:54:27+00:00"
first_seen_at: "2026-07-20T23:24:42.992125+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:ed89180a41199563d22798a798aecefd0195df6acd7c16de7000e1141ee2a354"
---

# Testing Workflows as a multi-service: Our Journey to Automated Testing at ZoomInfo

## **Background**


At ZoomInfo, we process and analyze massive volumes of data through various pipelines within our Workflows product.[Google Cloud Platform’s Dataflow](https://engineering.zoominfo.com/gcp-dataflow-advanced-topics-part-i) serves as a crucial infrastructure component in this process, enabling us to handle these complex data operations efficiently.


## **A few words about Workflows**


Workflows is a sales and marketing automation product within the ZoomInfo platform. It allows users to automate various prospecting and lead management tasks. The key functionalities include:


- Automated trigger-based actions based on specific criteria or events (like when a company gets new funding, changes leadership, or shows buying signals)
- Lead routing and distribution among sales teams
- Automated enrichment of contact and company data
- Integration with CRM systems (like Salesforce) and other sales tools
- Automated email sequences and engagement tracking
- List building and management based on specified criteria


From a quality assurance perspective, Workflows testing encompasses three critical dimensions:


1. Workflow creation – creating automated triggers based on events and different permutations to bring data.
2. Workflow execution – running created automated trigger to collect actual data from Zoominfo data sources
3. Workflow data integration – enrich external CRM systems with Zoominfo data collected from workflow execution.


## **The Testing Challenge**


### **Multiple Processing Frameworks**


Our data processing infrastructure spans across multiple technologies:


- **GCP Dataflow Pipelines** : Handle batch and streaming data processing
- **Nest.js Microservices** : Process data through REST and gRPC endpoints
- **Kafka Message Queues** : Manage asynchronous communication between services


### **Key Challenges**


### **1. Lack of Standardization**


The distributed nature of our system creates several standardization issues:


- Different input/output formats across processing frameworks
- Varied logging and monitoring approaches
- Missing crucial data in input/output logs
- Absence of comprehensive workflow execution summaries


### **2. Data Visibility Issues**


Tracking data flow through our system presents significant challenges:


- No unified view of data movement across different systems
- Complex tracing of records across Kafka topics and services
- Limited visibility into transformation steps within Dataflow
- Difficulty correlating logs across multiple systems


### **3. Testing Complexity**


End-to-end testing requires complex setup and dependencies:


- Challenging cross-system testing across hybrid architectures
- Extensive setup requirements for end-to-end testing:


- User provisioning with specific roles and permissions
- CRM system account creation and configuration
- Dependency on real data sources:


- Intent data
- Websights traffic


### **Manual Testing Limitations**


These challenges forced our QA process to rely heavily on manual testing:


- QA testers needed to:


- Parse through Datadog logs for workflow execution analysis
- Manually generate trigger messages for streaming workflow testing
- Analyze results stored in designated buckets
- Verify data exports in third-party CRM systems


### **The Mock Data Dilemma**


Mock data proved insufficient for our testing needs because:


- Workflows use complex filters and triggers
- Real data is essential for validating business logic
- Multiple data source dependencies
- Complex integration points with external systems


This combination of factors made our testing process:


- Extremely time-consuming
- Resistant to automation
- Prone to human error
- Difficult to scale


The lack of built-in solutions for programmatic access to logs and bucket files further complicated our ability to automate these processes effectively.


## **Finding a Better Way**


Through extensive collaboration within our automation team, we developed a novel approach to testing. The key insight was that we needed to create a suite of internal APIs that would give us programmatic access to each step of workflow creation and execution while taking into account that workflows built as a multi-service which led us to struggle with data extraction, analysis and verification.


This realization led us to:


1. Conduct a deep dive into our multi-service implementation
2. Understanding service interactions
3. Identify critical checkpoints throughout the workflow process
4. Create monitoring points to track execution progress and results


## **The Implementation Journey**


The development process was truly collaborative:


- Our QA automation team worked to define the required internal APIs
- We maintained continuous communication with the R&D team
- Requirements evolved through iterative feedback and refinement


## **Technical Implementation**


Our API implementation followed a strategic approach to capture and validate each crucial step of the Dataflow pipeline. We used five core APIs that provide comprehensive visibility into workflow execution and data processing.


A key innovation in our implementation is a new polling mechanism that continuously invokes the API until reaching the expected result, ensuring reliable data validation throughout the suite execution.


### **Core API Components**


### **1. Get GCP Status API**


This API serves as our initial checkpoint for workflow creation:


- Returns real-time status updates from Google Cloud Platform
- Confirms successful workflow registration in the system
- Validates workflow configuration before execution


### **2. Execution Details API**


Provides comprehensive workflow execution monitoring:


- Tracks the current state of workflow execution
- Reports completion status and execution time
- Identifies any execution bottlenecks or failures
- Enables real-time monitoring of workflow progress


### **3. Trigger Streaming Workflow API**


Enables the initiation of streaming workflows through data injection via Pub/Sub messages. This API facilitates real-time data processing by allowing automated workflows to be triggered based on incoming message streams from the Pub/Sub system. The streaming capability ensures efficient handling of continuous data flows for workflow execution.


- Integration with Google Cloud Pub/Sub messaging system
- Support for real-time data processing triggers
- Automated workflow initiation based on message content
- Scalable message handling for high-volume data streams


### **4. Step Details API**


Offers granular visibility into individual workflow steps:


- Returns detailed output data from each workflow stage
- Enables data validation at each transformation point
- Provides timing metrics for each step


### **5. Execution Record Details API**


Implements detailed record-level tracking:


- Maps the complete journey of individual records through the workflow
- Shows all transformations applied to specific records
- Helps identify where and why records might fail


### **6. Execution Summary Report API**


Generates comprehensive execution analytics:


- Aggregates key performance metrics
- Reports total records processed and failure rates
- Tracks credit consumption


## **Code Examples**


Here are key examples of how to interact with our workflow testing APIs:


## **Wins**


### **Breakthrough in End-to-End Testing**


- Successfully implemented the first automated end-to-end tests covering our entire multi-service architecture
- Achieved comprehensive testing across different services, data formats, and processing frameworks
- Established reliable automated verification of complex workflow scenarios


### **Significant Performance Improvements**


- Reduced manual regression testing time by more than 50%
- Decreased the effort required for testing complex workflows
- Minimized time spent on repetitive testing tasks
- Enabled QA teams to focus on more strategic testing activities


### **Enhanced Platform Stability**


- Improved early detection of issues through automated testing
- Increased confidence in workflow deployments
- Reduced production incidents related to workflow execution
- Established more consistent and reliable testing processes


### **Measurable Impact**


- 50%+ reduction in manual testing effort
- Increased test coverage across the platform
- Faster feedback cycles for development teams
- More reliable and consistent test results


## **Next Steps**


Our immediate priorities for the next quarter are focused on expanding our testing capabilities and adapting to product evolution:


### **1. Increase Automation Coverage**


- Expand test coverage to include edge cases and error scenarios
- Develop reusable test components for common workflow patterns
- Implement automated regression testing suite
- Build comprehensive end-to-end test scenarios for critical business flows


### **2. Support Complex Workflows Scenarios**


- Develop testing frameworks for multi-stage workflow orchestration
- Create validation mechanisms for conditional branching logic
- Add support for testing data transformation accuracy in complex scenarios


### **3. Support the New Workflow Product Design**


- Adapt testing infrastructure to accommodate new architecture changes
- Build a generic automation framework that enables other teams to easily test their custom workflow steps
- Create reusable testing components that reduce development time for new workflow testing
- Create testing tools for new workflow components
- Ensure backward compatibility with existing test suites


These priorities reflect our commitment to maintaining robust testing practices while supporting product evolution and increasing test coverage across our platform.
