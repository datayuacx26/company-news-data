---
schema_version: "1.0.0"
document_id: "c5c0c182597dd4fe15fa999b7a7f610ad7147bae33273e850be72c2b631a12dc"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/what-is-platform-as-a-service"
published_at: "2023-05-26T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:ab7ea138e28a5877ee75f84b716178291b29bb7a1b418c9aa4d7f822ba234e47"
---

# Understanding Platform-as-a-Service (PaaS): what it is and how it works

Platform-as-a-Service (PaaS) is a semi-novel technology pioneered by[Heroku](https://www.porter.run/blog/what-is-heroku) . Over the past decade, multiple platforms have emerged offering a wide range of improvements to the archetypal PaaS. A PaaS product enables companies to host, build, and deploy web and mobile applications for their customers. Instead of having to manage their own infrastructure, development teams are provided with a framework that they can use for web application development. PaaS adoption became commonplace among app development teams that didn't want to manage their own DevOps or be responsible for the maintenance of their app infrastructure; PaaS offerings take care of many parts of the software development process such as Continuous integration and Continuous delivery (CI/CD), enabling software testing and deployment through automation. Furthermore, a wide range of web applications and APIs can be developed on a cloud platform, allowing for greater innovation and collaboration without devoting additional bandwidth towards DevOps. If you want to read up on the advantages and disadvantages of PaaS, check out[this article](http://www.porter.run/blog/advantages-of-paas) .


## **How Does Platform-as-a-Service Work?**


To explain how PaaS technology works requires a bit of background regarding cloud infrastructure, software, and user interface (UI). There are several PaaS options, but they all involve these components.


### Cloud infrastructure


Cloud infrastructure is an umbrella term that covers all of the tools necessary in building a cloud; all aspects of the cloud computing model. This includes the hardware (such as servers, networking components, and storage) and software, like virtualization software, that allow access to cloud system resources. A cloud service provider like Amazon Web Services (AWS) uses a data center to store this hardware (a data center does not exist just for data storage). Generally, each availability zone has more than one data center, and these availability zones are spread across different regions around the world, allowing for lower latency and greater application availability. As[Red Hat](https://www.redhat.com/en/topics/cloud-computing/what-is-cloud-infrastructure) explains, virtualization (through hypervisor software) is used to parse out resources (storage, memory, and compute; generally CPU but for companies with Artificial Intelligence and Machine Learning workloads, GPU as well) from hardware into virtual machines on the cloud. Cloud infrastructure acts as the foundation for a Platform-as-a-Service, with virtual machines being the building blocks allowing software development in a cloud environment to occur.


### Software


What allows for applications to be developed within this environment is software. Since software provides instructions for the hardware to operate, in the case of a Platform-as-a-Service, the software is what allows for the building, deploying, and managing of these applications; software in the form of operating systems (O/S) manage resources and establishes a user interface. Operating systems also provide services to the applications, but the middleware executes any additional services; furthermore, the runtime environment is comprised of the O/S and any code libraries which are linked.


### User Interface (UI)


In order for developers to actually utilize the software, they must access it, and that’s where the UI comes into play. The UI allows software engineers to interact with the software through visual and text-based icons. This UI should cover the entire software development life cycle and provide a frictionless user experience for engineers and be accessible through a web browser by anyone with an internet connection (that has access to the development platform for their specific project).


##### PaaS Definition


A Platform-as-a-Service, in essence, manages the cloud infrastructure that allows for software application development to occur, providing a convenient interface for software engineers to work. A PaaS sits on top of the infrastructure, managing operating systems, middleware, and runtimes.


**The Porter dashboard is a UI that allows developers to interact with the underlying software that manages their infrastructure.**


## **Platform-as-a-Service versus other cloud services**


**Overview of the wide range of cloud computing service models and what a PaaS product manages.**


### Software-as-a-Service


Software-as-a-Service (SaaS) is the most well-known cloud service model, which entails the distribution of software whereby users purchase subscriptions (generally monthly or yearly) to use the software, which is ready-to-use and hosted on the cloud, meaning users don’t have to store the software locally. SaaS can come in many forms, from communication tools like Slack to video streaming applications including Netflix or Max, to project management solutions such as Asana. When a corporation or an individual uses a SaaS product, they don’t have to worry about any of the underlying technological aspects; they simply use the product for its intended purpose, whether that be sending messages to coworkers and clients, watching movies and TV shows, or organizing and managing their work. SaaS applications can be hosted, built, and deployed on a PaaS like any other application; many of Porter’s users provide SaaS products, such as[WriteSonic](https://porter.run/case-study/how-writesonic-runs-a-1-6-tb-kubernetes-cluster-with-no-devops-engineers) ,[Govly](https://porter.run/case-study/govly-moves-from-gcp-to-aws-in-a-day-using-porter) , and[Memberstack](https://porter.run/case-study/how-memberstack-uses-porter-to-serve-30-million-requests) .


### Infrastructure-as-a-Service


Infrastructure-as-a-Service (IaaS) is another cloud service mode. For IaaS, cloud service providers own and operate the hardware (servers, storage, and networking) and perform virtualization of those resources. Some examples of IaaS vendors (IaaS is generally an offering by a cloud service provider), include Amazon Web Services, Google Cloud, and Microsoft Azure. However, in order to host, build, and deploy applications on this infrastructure, it must be configured properly. A PaaS product sits on top of IaaS, acting as another layer of abstraction away from the infrastructure for the end user, managing it for them. For a more detailed description of IaaS vs PaaS vs Saas, read[here](https://www.porter.run/blog/iaas-vs-paas-vs-saas) .


### On Premises


Most companies utilizing cloud services do not need to maintain physical hardware where they are located; this hardware is what is referred to as on-premises cloud infrastructure. There may be compliance, security, or reliability-related reasons necessitating the use of on-premises infrastructure, but most companies utilize IaaS, which takes the hardware resources and contains them in virtual machines so they are available as cloud resources.


### Serverless


Another, newer type of cloud service is serverless computing; despite its name, serverless computing does not mean that servers aren’t used in the process of hosting applications; servers are still used for code execution, but cloud providers manage the servers themselves and allot computing resources on-demand.


## Public, Private, and Hybrid


### Public


Platform-as-a-Service products were initially meant to host applications on the public cloud; public PaaS products are hosted in the public cloud, which is accessible through the public internet.


### Private


PaaS providers have expanded their offerings to include private cloud hosting, involving a cloud that is only accessible by the organization using it, necessitating the emergence of private PaaS solutions. This allows for greater security, especially in the case of a company processing sensitive data which should not be accessible via the public internet. Data security is a priority for many companies utilizing a Private PaaS.


### Hybrid


A hybrid PaaS is a mix of public and private cloud deployments.


## **What Types of Platform-as-a-Service are Available?**


### Types of PaaS solutions


A PaaS provider may offer other types of PaaS models, but these classifications are generally trivial and open to interpretation:


‍


#### Database-as-a-Service (DBaaS)


‍


DBaaS allows users to access cloud-based databases without being responsible for managing the underlying infrastructure. Data management is taken care of by the DBaaS.


‍


#### Mobile Platform-as-a-Service (MPaaS)


‍


MPaaS has a specific use case: the configuration of mobile applications. It features no-code methods to implement push notifications and other mobile-specific features.


‍


#### Communication Platform-as-Service (CPaaS)


‍


CPaaS is configured to help developers implement instant communication into their applications. This includes video, voice, and chat messaging.


‍


#### Integration Platform-as-a-Service (iPaaS)


‍


iPaaS is focused on the development of software integration solutions.


## **Porter, the Platform-as-a-Service that runs in your own cloud**


Porter is a Platform-as-a-Service that runs in your own private cloud. However, there are no concerns about vendor lock-in since users can select a cloud provider of their choice. Users can create web and mobile applications or APIs in a wide range of programming languages without worrying about the underlying cloud infrastructure or DevOps work, allowing them to focus simply on building great products.
