---
schema_version: "1.0.0"
document_id: "f401444b1731f1cfa3f983b9d4d5b75604cf029c3ca8130cfa91f3cafe7c4f4d"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/elastic-google-cloud-security-data-ingestion-incident-response"
published_at: "2024-07-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:a425d5fe74b55a81a2297d84a23ddc91273dc8dbac01cd1bb542aad8ea2ee5c3"
---

# Elastic and Google Cloud: Enhancing security analytics from data ingestion to incident response

# Elastic and Google Cloud: Enhancing security analytics from data ingestion to incident response


By


[Valerio Arvizzigno](https://www.elastic.co/blog/author/valerio-arvizzigno)[Yang Li](https://www.elastic.co/blog/author/yang-li)


July 9, 2024


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


Elastic and Google Cloud are pioneering a comprehensive security solution that leverages our distinct capabilities to offer an unparalleled security analytics experience. This collaboration integrates Elastic's Search AI Platform with Google Cloud's scalable and secure infrastructure services to provide a comprehensive security platform designed to secure hybrid workloads efficiently. Together, we deliver enhanced protection capabilities and superior security analytics, covering everything from threat intelligence to incident response. Discover how Elastic and Google Cloud are better together, offering a unique approach to safeguarding digital environments.


## Comprehensive security for hybrid workloads


Our combined offerings bring a unified solution that simplifies the security management of hybrid environments. By seamlessly integrating threat intelligence, compliance measures, endpoint protection, SIEM capabilities, response strategies, and data ingestion, organizations are equipped to swiftly respond to — and prevent — threats at scale, ensuring continuous security posture improvement.


## Advanced threat intelligence with Elastic and Mandiant


We start our security journey from


[threat intelligence](https://www.elastic.co/security/tip) , a crucial component in the cybersecurity landscape for building our knowledge foundation for tackling cyber threats.


The first stop for every security analyst is the extensive content made available daily on Elastic's


[Security Labs](https://www.elastic.co/security-labs) . Security research and malware analysis are critical, especially when the intelligence we have is limited in context. Our goal is to ensure your team comprehends the situations we're observing just as thoroughly as we do. We aim to provide you with resources that align with the threats you face. This enables you to make informed decisions about your environment based on the threat’s past actions, its potential capabilities, and your own risk tolerance.


To achieve this, we connect reports with a range of resources, including:


-


Detailed malware analysis reports covering implants and tools


-


Custom utilities we've developed for our use that could be valuable to you


-


Summaries of the techniques we've encountered


-


Artifacts such as rules and signatures that you may find useful to implement


We continually monitor Elastic telemetry and various third-party sources to enhance our analyses and share these insights with you.


Give it a try exploring


[guidance](https://www.elastic.co/blog/address-llm-adoption-security-risks) and reports on how to securely adopt LLMs in your enterprise environment, one of the latest hot topics.


In addition to this domain, two prominent Google Cloud services complement our offering: Mandiant and VirusTotal. While VirusTotal specializes in malware signature detection, our primary focus in this post will be


[Mandiant](https://cloud.google.com/security/products/mandiant-threat-intelligence?hl=en) .


The Mandiant platform serves as a robust repository, aggregating and meticulously analyzing copious amounts of data from diverse sources. Leveraging cutting-edge machine learning techniques, Mandiant discerns patterns and anomalies within extensive data sets.


In essence, Mandiant provides real-time intelligence feeds regarding threat actors, methodologies employed, and targeted vulnerabilities, thus furnishing organizations with the latest information in the security space. This allows proactive measures to thwart evolving attack vectors and equips security professionals with the knowledge needed to fortify defense strategies and make informed decisions.


Given the vast volume of IoCs (indicators of compromise) and intelligence collected from Mandiant and other providers, Elastic steps in to streamline the process. Through our built-in


[integration](https://www.elastic.co/docs/current/integrations/ti_mandiant_advantage) , Elastic seamlessly


[ingests these third-party sources](https://www.elastic.co/guide/en/security/current/es-threat-intel-integrations.html) , consolidating them into a unified interface and providing a single pane of glass for all collected intelligence, including Mandiant's insights. Elastic facilitates seamless navigation and deep dives into the knowledge base through advanced search features. This empowers users to not only to find what they’re looking for with speed and scale but also to effortlessly correlate and pinpoint matches of IoCs within their environment, systems, and collected logs.


## Mitigating risks: From misconfiguration to compliance


Moving beyond threat intelligence, our platforms address common security challenges like misconfigurations through tools like Google Cloud's Security Command Center (SCC) and Elastic’s Cloud Security Posture Management (CSPM). These tools not only detect but also help remediate configuration issues, enhancing users’ cloud security posture efficiently.


The Google Cloud SCC is an integrated risk platform that natively collects findings, logs, and configuration information from various Google Cloud services. It also provides comprehensive threat detection for Google Cloud that includes Event Threat Detection, Container Threat Detection, and Virtual Machine Threat Detection as built-in services. Through its extended log monitoring and security features, organizations can ingest and analyze the data collected by SCC with the


[Elastic integration for SCC](https://www.elastic.co/docs/current/integrations/google_scc) — enhancing their ability to proactively prevent breaches and remediate security issues. The connection, driven by Elastic Agent, may use both the SCC REST API or Pub/Sub to retrieve data. Then all users have to do is execute their normal security analyst tasks within Elastic Security, backed by this additional strength.


Elastic complements this risk mitigation journey with our


[Cloud Security Posture Management](https://www.elastic.co/security/cloud-security) solution. This feature scans the entire Google Cloud landscape, identifying misconfigurations and compliance issues, and assigns a risk score to each one — summarizing the cloud infrastructure’s overall performance against


[security guidelines](https://www.elastic.co/guide/en/security/current/cloud-nat-sec-posture-dashboard.html) defined by the Center for Internet Security (CIS). Elastic's CSPM also provides remediation playbooks and best practices to address these issues, helping organizations maintain a strong security posture and adhere to regulatory requirements.


By leveraging Elastic's comprehensive


[suite of security solutions](https://www.elastic.co/security) , organizations can ensure that their cloud environments are protected against threats and vulnerabilities, while also maintaining compliance and minimizing risk.


## Seamless security log ingestion with native integrations


The Elastic and Google Cloud ecosystem offers a wide range of possibilities for moving data from monitored services to security tools, depending on specific needs and architectures.


**Elastic Agent:**


Elastic comes with a multitude of[native integrations](https://www.elastic.co/docs/current/integrations/google-cloud-intro) for Google Cloud and third-party services to ingest, process, search, and visualize security logs. The cornerstone of this capability is


[Elastic Agent](https://www.elastic.co/elastic-agent) , offering unparalleled versatility for data collection. Elastic Agent streamlines everything from collection to parsing, tailored to the specific source, along with prebuilt dashboards in Kibana to highlight key insights — allowing users retain the flexibility to customize these functionalities as needed.


Elastic Agent often emerges as a favorite, not just for the aforementioned skills but also for its role as an endpoint protection tool within hosting environments. Leveraging machine learning algorithms and behavioral analytics, it enriches user environments with malware detection, preventing potential threats in real time while continuously scanning for vulnerabilities to ensure prompt patching and mitigation. Moreover, Elastic Agent empowers organizations to enforce security policies, initiate endpoint isolation or quarantine measures, and maintain compliance with regulatory requirements — all within a centralized management interface.


A standout feature is the centralized management UI,


[Fleet](https://www.elastic.co/guide/en/fleet/current/fleet-overview.html) . This centralized hub enables users to effortlessly oversee their distributed fleet, whether in cloud or hybrid scenarios. From managing updates to rolling out new policies and tweaking configurations, it's all conveniently orchestrated from a single interface, eliminating the need to log into individual machines.


Elastic Agent facilitates data loading from a plethora of Google Cloud services — spanning from Google Cloud Storage and VMs to Kubernetes, from VPC, firewalls, and functions to Drive and Gmail, or Pub/Sub, among others. Of particular note is the Pub/Sub service, which serves as a proxy to enable seamless data publication from various Google services. Through our Pub/Sub integration, users gain access to the entirety of Google's ecosystem, even in cases where direct integrations may not exist.


**Dataflow:**


For those who prefer an agentless, serverless, and no-code approach, Google Cloud's Dataflow service emerges as a compelling option. This fully managed service simplifies the task of transferring data from your Google landscape to an Elastic index with just a few clicks. Dataflow comes with[built-in templates](https://www.elastic.co/guide/en/observability/current/gcp-dataflow.html) for Elasticsearch.


**Logstash:**


However, in scenarios where data requires preprocessing or enrichment before indexing, a more tailored, code-based approach may be necessary. In this case,


[Logstash](https://www.elastic.co/observability/log-monitoring) , our open-source processing tool, will help you manipulate and augment content as needed.


**Confluent Kafka service:**


Another option worth exploring involves leveraging common technology partners’ tools such as Kafka, particularly the managed Confluent Kafka service. This widely used technology seamlessly integrates with both Google services and Elastic, serving as a robust middleware solution for ensuring ingest stability, resiliency, and persistence. Discover the benefits of using Kafka with Elastic with the


[latest released features](https://www.elastic.co/blog/elastic-agent-output-kafka-data-collection-streaming) and through step-by-step


[tutorials](https://www.elastic.co/blog/index-data-elasticsearch-confluent-cloud) .


**Customizable API and SDK:**


Lastly, for developers seeking a fully customizable experience, our


[API](https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html) and


[SDK](https://www.elastic.co/guide/en/elasticsearch/client/index.html) for various programming languages provide unrestricted access to Elasticsearch functionalities within their applications.


## Leveraging GenAI for next-gen security analytics


Looking ahead, our integration with GenAI technologies like Google’s Gemini enhances our capabilities, offering even more guided analytics and automated response solutions. This integration aims to revolutionize how security analytics are performed, embedding advanced AI to streamline security analysts’ daily operations.


Elastic has recently introduced several groundbreaking


[AI-driven solutions](https://www.elastic.co/security/ai) embedded in Elastic Security, substantially streamlining analyst and admin workflows and freeing up hours spent on what would otherwise be manual processes.


[Elastic AI Assistant](https://www.elastic.co/elasticsearch/ai-assistant) can be easily activated with a few clicks within Elastic Security. It provides users with both prebuilt prompts and freedom of conversational interaction, supported by company-specific context — alerts, logs, and monitored sources — tailored for the user’s large language model (LLM) of choice.


This enables and simplifies multiple use cases such as alert summarization, workflow suggestion, query generation, and integration advice — strongly reducing analysts’ workloads while leading to thoughtful decisions. And once users are satisfied with the conversation results, they can add them to a timeline investigation or a case to follow up with the investigation.


Our GenAI capabilities not only support daily routine but change the way we effectively approach security analytics, removing monitoring fatigue by focusing on attacks instead of a list of alerts.


Elastic’s


[Attack Discovery](https://www.elastic.co/guide/en/security/current/attack-discovery.html) , a new groundbreaking available feature, revolutionizes SIEM by


[automating the labor-intensive tasks of alerting](https://www.elastic.co/security-labs/ai-driven-security-analytics) , threat hunting, and context analysis using advanced AI. It utilizes LLMs to examine alerts within a user’s environment and uncover potential threats. Each "discovery" signifies a possible attack, detailing connections between various alerts to identify involved users and hosts, how these alerts align with the MITRE ATT&CK framework, and potential threat actors behind them. This approach maximizes the efficiency of security analysts, combats alert fatigue, and shortens the mean time to respond to threats.


Elastic's generative AI tools lets users choose their favorite LLM to connect with by inserting their select service API key — and Gemini is about to be natively integrated with it as well!


## Next steps: Engage with Elastic and Google Cloud's security innovations


To leverage the full potential of our integrated security solutions, we encourage you to start with a


[free 14-day trial](https://cloud.elastic.co/registration) cluster on Elastic Cloud using your Google Cloud account


or easily subscribe to Elastic Cloud through


[Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/elastic-prod/elastic-cloud?) . This is an excellent first step to transforming your security posture and embracing a more proactive and informed security management approach.


Our partnership extends beyond mere technology integration — it's about creating a cohesive and secure ecosystem that supports your organization's security needs dynamically and efficiently. Don't miss out on the opportunity to enhance your security posture with Elastic and Google Cloud’s pioneering solutions.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, ESRE, Elasticsearch Relevance Engine and associated marks are trademarks, logos or registered trademarks of Elasticsearch N.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
