---
schema_version: "1.0.0"
document_id: "1fe329d9f4ae0fda158cf0bbd228edc8247a4d13dd3da2c3dd5e2c52ac78c776"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/cyber-fusion-center-open-source"
published_at: "2024-02-27T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:7028a04a45dc110013dd7e55d8a4b6c48db6d145bddba04fc51d69b1ed12595b"
---

# Target's Cyber Fusion Center Highlights Open Source Projects

In the arena of cybersecurity, the Cyber Fusion Center (CFC) at Target continuously evolves to rise to the challenges of threat analysis and detection. We are not only responders but also creators, equipping both our internal teams and the global community with tactics and tools designed to counteract digital threats. Our commitment to fortifying collective cyber defenses is reflected in our proactive contributions to the open source community. This blog post highlights a selection of our assets—Merry Maker, LUCkyRegex, Strelka, and the Strelka UI—complemented by insights on vital topics like threat hunting. Each tool is crafted to address the unique challenges presented in cybersecurity.


Contact, Feedback, and Assistance


Each of the following tools is part of our open source initiative available on GitHub, developed to empower the security community. While this post will highlight key features and updates, we encourage hands-on experience through the quick start guides provided. For inquiries, feedback, or assistance regarding any of these tools, we welcome you to reach out through their respective GitHub repositories. Your engagement helps us drive innovation and fortify cybersecurity defenses.


Merry Maker


[Merry Maker](https://github.com/target/mmk-ui-api) is a tool developed by the Target Cyber Fusion Center to specifically combat digital skimming threats. It simulates typical online shopping behaviors, monitoring and analyzing JavaScript and network activity to detect malicious code and unauthorized data exfiltration. Utilizing Puppeteer and Headless Chrome, Merry Maker effectively emulates real user interactions with websites, capturing detailed insights into potentially harmful activities. This tool stands out for its ability to alert security teams to digital skimming attacks in real time, and its framework has been open sourced by Target, contributing to the wider cybersecurity community's efforts to safeguard digital transactions.


GitHub Repository:


[Merry Maker](https://github.com/target/mmk-ui-api)


Learn More:


[Meet Merry Maker: How Target Protects Against Digital Skimming](https://tech.target.com/blog/meet-merry-maker)


Merry Maker Dashboard


LUCkyRegex


[LUCkyRegex](https://target.github.io/LUCkyRegex/) allows users to test their Lucene regular expressions through their browser. While Lucene regular expressions are common, there are no other Lucene regular expression testers available online. Lucene regular expressions can be drastically different from Perl Compatible Regular Expressions (PCRE), which means tools such as regex101 do not work for testing. This tool is notably helpful for anyone using Elasticsearch or Kibana, as both products utilize Lucene regular expressions. You can give LUCkyRegex a try by visiting our


[GitHub repository](https://target.github.io/LUCkyRegex/) .


GitHub Repository:


[LUCkyRegex](https://target.github.io/LUCkyRegex/)


The LUCkyRegex UI shows a Lucene query


Strelka


Developed by Target's cybersecurity team,


[Strelka](https://github.com/target/strelka) is a sophisticated file scanning system designed for comprehensive threat hunting, detection, and incident response. It excels in extracting detailed metadata and performing deep file analysis across a wide range of file types, including executables, password protected archives, emails, PDFs, and more. Whether analyzing scripts, identifying embedded hyperlinks, or extracting critical data from various file formats, Strelka’s capabilities ensure a thorough evaluation of potential threats. Its flexible architecture and adaptability make it a vital component in modern cybersecurity strategies, empowering professionals with extensive and automated file analysis tools for robust network security.


GitHub Repository:


[Strelka](https://github.com/target/strelka)


Learn More:


[Strelka: Real-Time Threat Hunting Scanner](https://tech.target.com/blog/strelka)


Sample Executable Data Extracted from Strelka


Strelka UI


The


[Strelka UI](https://github.com/target/strelka-ui) serves as a user-friendly interface for the Strelka file analysis system, enabling easy submission of files and hashes for detailed examination. One of its key features is a visual map that provides analysts with clear insights into file relationships, highlighting crucial details like YARA matches and extracted file content. Analysts can interactively delve into each file, uncovering detailed information, highlights, and filetype-specific insights. This interface significantly bolsters a cybersecurity analyst's ability to decode file content, making it an invaluable tool in the realm of file analysis.


GitHub Repository:


[Strelka UI](https://github.com/target/strelka-ui)


Strelka UI Showing File Analysis Details


Threat-Hunting


Finding abnormality in complex environments is a significant challenge. Threat hunters need new and novel ways of doing this to help keep their organizations safe. The Target Threat Hunting team has released several Python notebooks that can be used to find suspicious occurrences that could be consistent with malicious activity. We published notebooks that facilitate hunting for beacon malware via statistical methods, for obfuscated commands used by adversaries to hide their activities, and for abnormalities in directed graph analysis. These are useful tools for anyone charged with identifying suspicious activity that would have otherwise gone undetected.


GitHub Repository:


[Threat-Hunting](https://github.com/target/Threat-Hunting)


Louvain Community Analyzer


## RELATED POSTS


### Strelka: Real-Time Threat Hunting Scanner


By Paul Hutelmyer, August 24, 2022


Strelka is a real-time, container-based, file scanning system used for threat hunting, threat detection, and incident response, built by our Target cybersecurity team.


### Synthetics: Continuous Assurance of Detection Components


By Paul Hutelmyer, December 13, 2022


This post provides a solution for utilizing synthetic events for the purpose of validating signature integrity and functionality, with the goal of achieving continuous assurance of a system’s detection signatures.
