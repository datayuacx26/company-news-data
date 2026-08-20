---
schema_version: "1.0.0"
document_id: "1408923b50aca0af65bd9db9529e03fce811100d91c68943f25b116fdff0ad16"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/backstage-and-kubernetes-everything-you-need-to-know/"
published_at: "2023-08-22T08:23:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:c8e7c7935a4b26f701379349d35a45f3e1491cf6980734f2612f4fa3dadbe109"
---

# Kubernetes and Backstage: Everything You Need to Know

‍


Backstage is a powerful tool for presenting a clean and consistent developer experience within an organization. As Signadot is fundamentally a tool for improving the developer experience, I wanted to give a brief outline of Backstage, and how it can improve engineer’s lives, especially when using Kubernetes.


## What is Backstage?


Backstage is an open-source platform developed by Spotify that serves as a developer portal for all the tooling, services, and documentation within an organization. It’s designed to be a unifying interface for both developers and operations engineers, providing a centralized location for managing various aspects of software development and infrastructure.


### Key Features and Concepts


1. **Service Catalog** : Backstage includes a service catalog that allows developers to explore and manage all the services within an organization. This catalog provides insights into ownership, lifecycle status, and other critical metadata.
2. **Plugins** : The platform is highly extensible, with a plugin architecture that allows developers to add custom functionality. This includes integrations with various tools like Kubernetes, CI/CD pipelines, monitoring systems, and more.
3. **Standardized Documentation** : Backstage encourages the co-location of documentation with code, promoting a standardized approach to documenting services, APIs, and other components.
4. **Developer Experience** : By providing a unified interface for various tools and systems, Backstage enhances the developer experience, making it easier to navigate the complex landscape of modern software development.
5. **Tech Stack Insights** : It offers insights into the technology stack, dependencies, and the overall health of services, aiding in decision-making and governance.


Backstage represents a significant step towards consolidating the fragmented tooling landscape that many organizations face. By offering a single access point for all development-related activities, it aims to improve efficiency, collaboration, and understanding across teams.


The success of Backstage within an organization would likely depend on thoughtful implementation and alignment with existing processes and culture. The extensibility through plugins is a strong point, but it also requires careful management to ensure that the platform remains coherent and user-friendly.


In essence, Backstage is a comprehensive platform that seeks to unify various aspects of software development and operations within a single interface. Its open-source nature aligns well with the growing trend towards transparency and collaboration in the tech industry. While promising, its effectiveness would be contingent on the specific needs, culture, and practices of the organization implementing it. It’s a tool that warrants consideration, especially for organizations looking to streamline their development processes and foster a more cohesive and efficient development environment.


## Kubernetes in Backstage


Kubernetes in Backstage is designed to enable developers to effortlessly monitor the health of their services, irrespective of the deployment environment. This could range from a local host for testing to production environments spread across multiple clusters globally.


The salient features of this integration include:


1. **Enhanced Visibility** : It accentuates the identification of errors and provides detailed insights into deployments, pods, and other related objects for a service.
2. **Modular Design** : The feature is composed of two plugins: **@backstage/plugin-kubernetes** (frontend) and **@backstage/plugin-kubernetes-backend** (backend). The frontend plugin is responsible for presenting information in an accessible manner, while the backend plugin manages the mechanics of connecting to Kubernetes clusters to gather relevant data.


**a quick insight into your running clusters**


‍


## Should you adopt Backstage?


The decision to integrate Backstage within your organization’s technological ecosystem must be approached with a nuanced understanding of both its capabilities and potential challenges. While the deployment of Backstage for a small number of developers may be relatively straightforward, scaling it to an enterprise level introduces complexities that warrant careful consideration:


1. **Enforcement of Approved Deployment Patterns** : The flexibility of Backstage allows for various installation methods, such as utilizing Helm. However, the responsibility lies with the platform administrator to enforce and validate a deployment pattern that aligns with the organization’s IT compliance standards. This requires a robust framework to ensure that the chosen method of deployment adheres to the specific regulatory and operational requirements of your enterprise.
2. **Maintenance** : The dynamic nature of open-source projects like Backstage means that staying abreast of the latest versions is essential. As a platform administrator, the ability to efficiently apply updates across the cluster fleet is a critical consideration. This involves not only the technical aspects of version management but also an understanding of how updates align with the broader strategic goals and operational stability of the organization.
3. **Adaptability to Diverse Cluster Environments** : Many enterprises operate within a hybrid or multi-cloud strategy, necessitating a solution that can be seamlessly deployed across various cluster types. The evaluation of Backstage must include an assessment of its compatibility with your organization’s existing infrastructure and the potential need for customization to ensure smooth deployment across different cloud environments.


## How to use Kubernetes with Backstage


Here’s a brief guide on how to use Kubernetes with Backstage, tailored for an enterprise setting:


1.


**Install the Frontend Plugin** : Begin by adding the Kubernetes frontend plugin to your Backstage application. This can be done from the Backstage root directory using the following command:


2.


yarn add —cwd packages/app @backstage/plugin-kubernetes


3.


**Import the Plugin** : Next, import the plugin in your app by adding the “Kubernetes” tab to the respective catalog pages. You can set an optional refresh interval for the content, with a default of 10 seconds.


4.


**Install the Backend Plugin** : Navigate to the packages/backend of your Backstage app and install the @backstage/plugin-kubernetes-backend package:


5.


yarn add —cwd packages/backend @backstage/plugin-kubernetes-backend


6.


**Create a Backend File** : Create a file called kubernetes.ts inside packages/backend/src/plugins/ and add the necessary code to build and return the router.


7.


**Import the Backend Plugin** : Import the plugin into packages/backend/src/index.ts and add the required lines of code to integrate it with your existing Backstage backend.


8.


**Custom Cluster Configuration (Optional)** : If the existing cluster locators don’t suit your needs, you can implement a custom KubernetesClustersSupplier. This allows for custom refresh intervals and cluster details fetching.


9.


**Configure the Plugins** : After installing the plugins in the code, you’ll need to configure them according to your specific requirements.


10.


**Troubleshoot if Necessary** : If the Kubernetes information is not displaying as expected, you may need to troubleshoot the issue.


This integration is designed to enhance visibility and control for developers and administrators alike, but it should be implemented with consideration of your organization’s specific needs and compliance requirements.


## Developer Experience Matters


Developer Experience (DX) tools, such as Backstage, can significantly improve a team’s development velocity by addressing the unique needs of service owners, engineers, and platform teams. Backstage, an open platform for building developer portals, provides a centralized hub for managing Kubernetes resources, including workloads, pods, and containers. With its Kubernetes plugin, Backstage enables seamless interaction with the Kubernetes cluster, simplifying the complexity of managing various objects and functions within the infrastructure.


By reducing the manual lines of code and providing a clear path for managing various aspects of the application, from development to production, Backstage serves as a valuable tool for organizations looking to enhance their development velocity. Its integration with Kubernetes, coupled with the flexibility to adapt to the specific needs and preferences of service owners and engineers, makes it a robust solution for modern development environments.


‍
