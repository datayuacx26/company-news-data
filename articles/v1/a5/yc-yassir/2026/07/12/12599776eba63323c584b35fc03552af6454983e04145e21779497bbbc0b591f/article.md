---
schema_version: "1.0.0"
document_id: "12599776eba63323c584b35fc03552af6454983e04145e21779497bbbc0b591f"
company_key: "yc-yassir"
company: "Yassir"
source_id: "yc-yassir-news-import-6914a7494ed8"
canonical_url: "https://yassir.com/en/blog/how-platform-engineering-is-transforming-the-use-of-modern-day-cloud-native-technologies"
published_at: null
first_seen_at: "2026-07-26T06:10:41.130229+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:b9bbedc31bd618857b4e134b90eda889b3bcb601625cb27053cf4ce3b20f19c3"
---

# 03/03/2024 How Platform Engineering is transforming the use of Modern-day Cloud Native Technologies In this article, we discuss how Platform Engineering is used at Yassir to transform the use of...

03/03/2024


How Platform Engineering is transforming the use of Modern-day Cloud Native Technologies


by Michael Mekuleyi - Senior DevOps Engineer


Cloud Technologies


Platform Engineering


Innovation


In this article, we discuss how Platform Engineering is used at Yassir to transform the use of Cloud-native tooling with a focus on the introduction of the principles of self-service, Internal Development Platforms (IDPs), and automated infrastructure. We also delve into how Platform Engineering benefits such services by encouraging automated infrastructure provisioning, enhanced observability and monitoring, enforced security practices, and finally scalability and resilience.


**Platform Engineering; A fresh air**


Platform Engineering is a discipline that focuses on building tools that combine automated infrastructure and self-service capabilities to provide developers the ability to create apps using standardized frameworks, instead of starting from scratch. Platform engineering aims to create Internal Developer Platforms (IDPs) that abstract the underlying complexities of infrastructure, enabling developers to focus on delivering value, without being experts in infrastructure thereby improving software delivery, reducing toil, and increasing operational efficiency.


According to the State of Platform Engineering Vol 2, when developers focus on writing code completely, organizations can slash Time To Market (TTM) by 30%, achieve 4x higher deployment frequency and accelerate lead time by 30%.


**How we transform Developer Experience and Cloud Operations with Platform Engineering at Yassir**


At Yassir, we are committed to leveraging cutting-edge tools and practices to deliver innovative services and solutions. We rely on tenets of Platform Engineering to bridge the gap between DevOps practice and the Software Development Cycle, enabling our engineers to deploy faster, easier, and without much friction. One of the cornerstones of platform engineering at Yassir is the use of the CDKTF library for designing infrastructure. This library allows our developers to review software in a relatable programming language without the hassle of having to learn Terraform or understand its syntax.


At Yassir, we are focused on the right developer tooling to allow us ship products faster and deploy new releases seamlessly. We make use of an open-source platform called backstage as our Internal Developer Platform(IDP) to encourage collaboration between teams, squads and domains. In addition to backstage, we use another open-source tool called Apache Devlake for our core DORAmetrics. We use Devlake to measure deployment frequency, change failure rate and other necessary metrics that enhance observability.


Security is a major concern at Yassir, we employ the use of standard security practices in the use of HashiCorp Vault for secrets management and SonarQube for continuous vulnerability scanning and static code analysis. We integrate these security tools into our CI/CD pipelines, automatically scanning for vulnerabilities and bugs. We also utilize PagerDuty, to ensure the highest reliability and availability of our services. We use pager duty for incident response to automate escalations and manage our incident response systems.


With the strategic implementation of the afore-mentioned services and tools, Yassir has completely transformed its developer experience and Cloud Operations, making it easier to abstract complexity, automate process and build software securely from start to finish.


**Introducing Platform Engineering to your organization**


Introducing platform engineering to your organization should be an iterative process that requires careful planning and continuous improvement. The goal is to build a self-service platform, encourage its adoption then initiate a culture switch to the self-service orientation. The following steps are quick steps ways to achieve this:


- Assess the current state and define the objectives of your migration to Platform engineering.
- Gain Executive support and align stakeholders.
- Establish your first Platform Team.
- Develop a Platform Strategy.
- Implement a pilot project.
- Document your findings and iterate.


**Conclusion**


Platform engineering is leveraging cloud-native technologies to redefine how software is deployed and improve the collaboration between Software engineering and Infrastructure. Also, by abstracting layers of infrastructure into modules and APIs, Platform engineering is improving developer autonomy and embedding best practices into the software deployment process. Adopt Platform Engineering in your organization today.


#####
