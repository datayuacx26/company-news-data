---
schema_version: "1.0.0"
document_id: "c63acf3b11a731f8510ea6598b58d1f33c753e8786cf5698aa20c2912b02a0d9"
company_key: "schrodinger-inc-common-stock"
company: "Schrodinger Inc."
source_id: "schrodinger-inc-common-stock-rss-d7bf526e9e3e"
canonical_url: "https://extrapolations.com/accelerating-science-how-gitops-is-transforming-drug-discovery-infrastructure/"
published_at: "2026-07-08T20:20:51+00:00"
first_seen_at: "2026-07-20T23:18:31.059214+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:d5ec9bd46ca0bf1f9cc96bbc173fbd662a281fea013e4ca063286f2ff0243ca9"
---

# Accelerating Science: How GitOps is Transforming Drug Discovery Infrastructure

[Life Science](https://extrapolations.com/category/life-science/)


# Accelerating Science: How GitOps is Transforming Drug Discovery Infrastructure


BY Aleksandr Ivenin


Jul 8, 2026


### **Summary**


When drug discovery scientists need custom computational tools, manual deployment shouldn’t be the bottleneck. Discover how Schrödinger’s engineering team built an automated GitOps pipeline that reduced deployment time from days to minutes, enabling seamless collaboration with customers like AstraZeneca.


Schrödinger’s LiveDesign enterprise informatics platform serves as the digital “central station” for collaborative drug design. Our customers often build their own custom “gadgets” tailored to their specific workflows. For years, getting these custom tools from a developer’s desk into a scientist’s hands was a manual marathon. It involved coordinated handoffs, configuration tweaks, and time-zone-defying emails. On a good day, it took hours. On a bad day, it took days to weeks.


When we began working with


**AstraZeneca** to deploy their custom tools, we knew we had to move beyond “manual.” We needed a system that was as agile as the science it supports.


---


###


### **The Friction of Manual Handoffs**


LiveDesign brings together computational chemists, biologists, and data scientists to optimize therapeutic candidates. Our customers often develop specialized tools for specific tasks – molecule generation, property prediction, or custom analytics- that integrate directly into their LiveDesign workflows. These tools run as containerized applications in the cloud, but the “last mile” of deployment was a persistent bottleneck.


The old workflow was rigid:


1. The customer packages their tool as a Docker image and pushes it to their container registry.


2. They notify our team.


3. We manually update configuration files, verify the changes, and deploy the new version to their Kubernetes cluster.


For teams iterating rapidly, this manual process and associated delays weren’t just an inconvenience—it was friction that slowed down the search for new medicines. We needed automation that could react to new tools immediately, deploy them reliably, and keep everything traceable.


### **Solving for Speed: The GitOps Approach**


We turned to


**GitOps** —a paradigm where the entire infrastructure configuration lives in a Git repository, and automated systems ensure the live environment always matches that “source of truth.”


To make this a reality for AstraZeneca, we integrated three core components:


- **Argo CD:** A Kubernetes-native tool that watches the Git repository. The moment a configuration change is detected, Argo CD synchronizes the application configuration automatically.


- **Argo CD Image Updater:** This monitors the Amazon ECR (Elastic Container Registry) where AstraZeneca stores their Docker images. It uses semantic versioning to detect new tags and automatically commits the updated tag back to GitHub.


- **IAM Roles for Service Accounts (IRSA):** Security is never optional. We used IRSA to provide the Image Updater with temporary, secure credentials for ECR, eliminating the need for long-lived secrets which follows best practices.


The result is a closed-loop pipeline: AstraZeneca pushes a new Docker image, and within minutes, the updated tool is live. No tickets, no emails, no waiting. The entire cycle takes minutes instead of days.


### **Engineering for Production: Beyond the Basics**


Building a system that “just works” requires obsessing over the details. One specific challenge was AWS ECR’s 12-hour token expiration. To prevent the pipeline from stalling mid-workflow, I developed a authorization script that generates fresh tokens on demand, refreshing every 10 hours to ensure 24/7 readiness.


Security was another pillar of the design. While our configurations are transparent and auditable in Git, sensitive data like API keys are never stored there. Instead, our Helm charts render secrets directly from


**AWS Secrets Manager** at runtime. It’s GitOps with industrial-grade guardrails.


Another decision was whether to use Argo CD’s annotation-based configuration or custom ImageUpdater resources. Annotations are simpler to start with, but as we planned to support multiple customers with different update strategies, custom resources gave us more flexibility and better version control.


The architecture diagram tells the story visually: a clean flow from ECR through GitHub to Kubernetes, with each component responsible for one thing. It’s the kind of system that you can explain to a non-engineer in two minutes, which usually means it’s well-designed.


### **Why This Matters: Impact Beyond the Code**


The most satisfying part of this project isn’t the automation itself—it’s what that automation enables.


By removing the “deployment tax,” we’ve empowered AstraZeneca scientists to iterate independently. They can test a new hypothesis, update their tool, and see the results in minutes. We’ve turned a week-long waiting room into a real-time laboratory.


For our engineering team, this has eliminated a whole category of manual toil, allowing us to focus on higher-order architectural challenges. We can now onboard new tools in about an hour, leveraging the reusable patterns we’ve established.


At the end of the day, this project sits at the intersection of infrastructure and impact. These aren’t just web apps; they are the tools that help researchers design better medicines. By making the infrastructure disappear into the background, we let the science take center stage.


Aleksandr Ivenin


Aleksandr Ivenin, M.Sc., is a Principal Solutions Architect at Schrödinger, where he works directly with top-tier customers, scientists, and developers to deploy, configure, and tune the company’s high-end molecular modeling and informatics applications. He earned his M.Sc. from the Faculty of Physics at Lomonosov Moscow State University. Aleksandr specializes in building sustainable, scalable, and cloud-native architectures, focusing on implementing optimal deployment strategies across high-performance computing and cloud environments. Throughout his career, he has successfully directed cross-functional engineering teams, automated infrastructure for high-load systems managing thousands of servers, and designed microservice platforms supporting millions of users.


### Sign Up Today


Sign up to receive quarterly updates with the latest from Extrapolations.


### Up Next


[Life Science](https://extrapolations.com/category/life-science/)[Physics Meets Agents: The Next Chapter of Our Work with NVIDIA For years, computationally driven drug discovery has faced two bottlenecks at once: accuracy and scale. It’s not enough to run one good calculation. Scientists need results they can stake real decisions on, and they need them across a chemical space far too vast for any wet lab to explore by hand. Physics-based approaches deliver the BY Schrödinger Editorial Team](https://extrapolations.com/physics-meets-agents-the-next-chapter-of-our-work-with-nvidia/)
