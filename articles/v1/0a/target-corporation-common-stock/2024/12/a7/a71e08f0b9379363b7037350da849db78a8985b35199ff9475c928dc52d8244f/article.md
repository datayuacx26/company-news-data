---
schema_version: "1.0.0"
document_id: "a71e08f0b9379363b7037350da849db78a8985b35199ff9475c928dc52d8244f"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/implementing_TLSH_based_detection"
published_at: "2024-12-03T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:b4be97486f1a6f4487999292c2b89d26c10a20b30a2893b8d583c64d37affdd7"
---

# Implementing TLSH Based Detection to Identify Malware Variants

In this blog post, we will explore using


[TLSH](https://tlsh.org/index.html)


(TrendMicro Locality Sensitive Hash), a similarity hash, to identify variants of known malicious files. We will also explore how to use


[Strelka](https://tech.target.com/blog/strelka) , a file scanning and analysis system, to generate and compare TLSH similarity scores of extracted files from network traffic or PCAPs.


Malware detection is a challenging task that requires continual vigilance and analysis to keep pace with new and rapidly changing threats. Some traditional methods of malware detection, such as signature detection, may rely on cryptographic hashes (MD5, SHA1, SHA256) that identify files based on their exact content. If you have a fingerprint of a file that you know is malicious, then you can find copies of that file in your environment. However, these methods are not effective against malware variants that change their content slightly to evade detection. Moreover, cryptographic hashes do not provide any information about the similarity or dissimilarity between different files.


What is TLSH?


TLSH is a


[locality-sensitive hashing](https://en.wikipedia.org/wiki/Locality-sensitive_hashing) scheme that is adept at measuring the similarity between files. Traditional cryptographic hashes like MD5 or SHA-256 are extremely sensitive to change. A single altered byte in a file completely changes its hash. This is great for file integrity checks, but attackers can easily modify their malware just enough to fool signature-based detection.


TLSH is different. It's a fuzzy hashing algorithm that focuses on identifying similarities between files. This means that even if malware undergoes minor changes, its TLSH hash will remain relatively similar, allowing you to flag potential variants.


How TLSH Works (and its benefits)


TLSH analyzes a file's byte patterns and creates a fingerprint based on overall structure rather than exact sequences. What’s unique about TLSH is that its hashes can be compared to identify similar files via a comparison score. When two TLSH hashes are compared, a score is provided, which reflects their degree of similarity, starting from a score of 0 for extremely similar files and increasing as differences grow. The maximum score, while not precisely defined, represents a threshold beyond which files are considered significantly dissimilar. The specific interpretation of these scores can be subjective and typically relies on the context of the application:


- Scores near 0 (e.g., less than 30)


suggest a high level of similarity, often indicating only minor variations exist between the files.


- Moderate scores (e.g., between 30 and 100)


imply some level of similarity, with potential for more noticeable differences.


- Scores above 100


are indicative of considerable differences, signaling that the files are dissimilar.


The table below, provided by TrendMicro, provides an empirical view of the relationship between TLSH scores and their corresponding rates of false positives and detection accuracy. These metrics were identified in comparison scans against a population of malicious and benign samples and can guide users in setting appropriate thresholds for their specific needs. To simplify: the lower the score between two TLSH hashes, the closer the two files are in content.


Table 1: False Positive and Detection Rates for TLSH (


[Source](https://documents.trendmicro.com/assets/wp/wp-locality-sensitive-hash.pdf) )


Potential Use Cases of TLSH


TLSH can be used for various applications in cyber security that could benefit from similarity comparisons between files, such as:


- Malware clustering


: Given a collection of malware samples sorted by family, with each family in its own directory, generate the TLSH for each file and store the hash in a YAML file where each family has an entry followed by a list of TLSH hashes. This way, you can group similar malware samples together based on their TLSH hashes.


- Malware identification


: Given an unknown malware sample, generate the TLSH hash and search for similar hashes on VirusTotal or other online databases that support TLSH queries. This way, you can identify the possible malware family or variant of the sample.


- Malware sharing


: Given a malicious file that you want to share with another organization without providing an explicit MD5, SHA1, or SHA256 hash that might reveal its identity or origin, generate the TLSH hash and share it instead. This way, you can still allow other organizations to hunt for similar files without compromising your source or intelligence.


- Malware detection


: Given a set of known malicious TLSH hashes stored in a rule file, scan incoming or existing files using Strelka and compare their TLSH hashes with those in the rule file. This way, you can detect potential malware infections or intrusions based on their similarity with known threats. This is the method that we’ll focus on for the remainder of this post.


Understanding TLSH Through Examples


Let's explore how TLSH works in action. We'll examine two scenarios to see how it reveals similarities and differences between files.


Example 1: Windows Command Prompt Versions


The Windows Command Prompt (cmd.exe) is a staple tool across various Windows versions. The samples in the following table have some differences in file size and code changes yet they all still serve the core purpose of a command-line interface (and more importantly, are both legitimate files from Microsoft). Let's see what TLSH tells us:


When we use a tool like


[py-tlsh](https://pypi.org/project/py-tlsh/) to compare the TLSH hashes of different cmd.exe versions, we get a low comparison score of 2. This low score suggests that despite variations, the files share a similar file. This helps validate they are likely different versions of the same application.


Example 2: Suspicious "cmd.exe" File


Now, imagine two files both labeled "cmd.exe." One is a known, legitimate version from Microsoft. The other? Its origins are unclear. How does TLSH help us investigate?


The drastically high TLSH comparison score of 642 signals a stark difference between these "cmd.exe" files. In this context, a high score often means the files have very little in common. This discrepancy is a major red flag – the suspicious file may be masquerading as the legitimate one, with potentially malicious modifications.


TLSH in Threat Detection


This ability to uncover subtle differences makes TLSH a potent cybersecurity tool. Security teams can build databases of TLSH hashes representing known malicious files. Tools like


[Strelka](https://tech.target.com/blog/strelka) then automate a vital process:


1. Scanning new files and generating their TLSH hashes.


2. Comparing those hashes against the threat database.


High similarity scores may quickly highlight anomalies for further investigation. TLSH streamlines detection, even against disguised or evolved malware variants, bolstering defenses in our ever-changing threat landscape.


Establishing Your TLSH Scanning Environment with Strelka


Implementing your own TLSH-based scanning system is straightforward with


[Strelka](https://tech.target.com/blog/strelka) , which is a scalable file scanning system designed for real-time analysis in cybersecurity applications such as threat hunting and incident response. Within its suite of features, Strelka includes a variety of scanners capable of parsing metadata across numerous file types, including PDF, ZIP, EXE, DOCX, and more. Of particular interest for this use case is Strelka’s ScanTlsh scanner, which compares every file’s TLSH hash against a known list of TLSH hashes to flag similarities.


To get started with Strelka for TLSH hash generation and comparison, you'll require:


- A mechanism to feed files into the system, which could be from endpoints, network traffic, or PCAP files. Tools like Zeek or SecurityOnion are perfect for this task.


- An environment configured to deploy Strelka's containerized system, for which Docker is a prime candidate.


- A set of


[rules](https://github.com/target/strelka/blob/master/configs/python/backend/tlsh/rules.yaml) that includes a list of known TLSH hashes associated with targeted or malicious files.


With Strelka operational and actively processing files, it will continuously compare incoming files against the TLSH hash ruleset and log the lowest score found for each comparison. You can then construct detection rules to monitor these TLSH matches and trigger alerts as necessary.


As an example, let’s look at an example of this in action using the


[Strelka UI](https://github.com/target/strelka-ui) . Imagine a scenario where a file is uploaded and initially classified as "Benign" by external enrichment services. Strelka’s interface, however, highlights a TLSH related match tagged "excavator" - a category from our predefined ruleset of TLSH hashes.


If we scroll down, we can see information pertaining to the TLSH hash. We’re shown details such as: the matched TLSH family, or group defined in the Rules file, the hash it matched closest against, a rating for the match, and visual marker on how similar the file may be.


In this example, a low matching score like 23, would suggest that the uploaded file shares significant similarities with the file represented by the matched TLSH hash, potentially warranting further investigation.


Delving Into Strelka's TLSH Malware Detection Mechanics


Let’s look at an example of how Strelka uses TLSH to detect malware. The process incorporates a pair of scanners—


ScanHash


and


ScanTlsh


—which is detailed in the JSON snippet below from a Strelka file scan.


ScanHash


- This scanner is tasked with generating a suite of hash values for the file, including TLSH, MD5, SHA1, SHA256, and SSDEEP, providing a comprehensive fingerprint of the file in question.


ScanTLSH


- This scanner's role is to take the TLSH hash and cross-reference it against a database of TLSH hashes known to be associated with known files, along with their corresponding groups, families, or identifiers.


- It determines the most similar file in the database by seeking the lowest similarity score, where a score of 0 indicates the closest match.


- The scanner then logs the closest TLSH hash, its associated metadata such as the group, family, or identifier, the similarity score, and the matched TLSH hash.


The output of this scan is a JSON object that contains information about the file and its potential match. Detection engineers can use this output to identify files that have a \`


tlsh.match.score\`


lower than a certain threshold and determine which family or group they likely belong to.


Considerations for TLSH-Based File Similarity Detection


While TLSH offers a distinctive approach to detecting file similarities, several considerations are essential when utilizing this method:


- TLSH requires a minimum byte stream of 50 bytes to generate a hash value. If the file is smaller than that, no hash will be produced.


- A list of TLSH hash values must be maintained and updated regularly to provide effective detection.


- The score threshold for determining similarity can be adjusted based on your preference and data set. A lower threshold means more strict matching but also more false positives, while a higher threshold means more lenient matching but also more false negatives.


- Packed or encrypted malware samples are files that have been compressed or obfuscated to evade detection by antivirus software. These files have very different content and structure than their unpacked or decrypted counterparts, which makes it harder for TLSH to generate meaningful hash values that reflect their similarity. Therefore, it is recommended to maintain a collection of unpacked samples for detection using TLSH.


Contact, feedback, and assistance


Strelka is an open-source project hosted on


[GitHub](https://github.com/target/strelka) , developed to assist the security community in conducting extensive file analyses. This article has provided a snapshot of utilizing TLSH detection with Strelka. Should you have inquiries about the content discussed here, or require guidance in configuring your own setup, we encourage you to connect with us through our


[GitHub](https://github.com/target/strelka) repository.


## RELATED POSTS


### Visualizing File Analysis with the Strelka UI


By Paul Hutelmyer, October 11, 2024


This open-source tool conceived at Target plays a pivotal role in streamlining file analysis for enhanced threat detection.


### Strelka: Real-Time Threat Hunting Scanner


By Paul Hutelmyer, August 24, 2022


Strelka is a real-time, container-based, file scanning system used for threat hunting, threat detection, and incident response, built by our Target cybersecurity team.
