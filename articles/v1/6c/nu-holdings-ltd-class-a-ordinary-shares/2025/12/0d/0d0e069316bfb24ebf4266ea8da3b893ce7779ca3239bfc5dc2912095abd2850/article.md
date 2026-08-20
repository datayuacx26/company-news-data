---
schema_version: "1.0.0"
document_id: "0d0e069316bfb24ebf4266ea8da3b893ce7779ca3239bfc5dc2912095abd2850"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-32a3c94cba04"
canonical_url: "https://building.nubank.com/nufilebox-reverse/"
published_at: "2025-12-19T13:03:37+00:00"
first_seen_at: "2026-07-20T04:35:52.604812+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:fcc6cc28ed1bbae352dfa62bf94b38ad5de547a7455a74b9f0b8ae083ed5dc4c"
---

# How NuFilebox Reverse strengthens file security at Nubank

During the second edition of Clojure South, Eric Bispo and Isaac Borges took the stage to present one of the most fascinating projects developed within NuFuturo, the research partnership between Nubank and institutions such as UFBA, IFBA, and UFCG. Their work, the NuFilebox Reverse, emerged from a very concrete challenge faced by Nubank’s legal team: receiving, analyzing, and processing external files securely in a workflow that involves multiple languages, large data volumes, and a fully cloud-based infrastructure.


The session showed how academic research and functional engineering can complement each other to create a robust upload pipeline, placing Clojure at the center of a security architecture built on the zero-knowledge principle.


## The context of the problem


Companies that interact with other companies depend on the constant exchange of documents. At Nubank, many of these files are large, sensitive, and originate from different external sources. Every received file represents a potential risk. High volume creates performance and scalability challenges. The need for security goes beyond simple post-upload analysis, a zero-knowledge architecture imposes a strict constraint: the server cannot, at any point, have access to the decrypted content of these files.


The key question guiding the research was simple in theory but complex in practice: how can third parties send files to Nubank without exposing the system to threats and without creating exceptions that compromise privacy or data integrity.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## Why Clojure became the foundation


Choosing Clojure was not merely pragmatic, even though Nubank is the largest company in the world using the language at scale. Clojure proved ideal for handling sensitive metadata, temporary tokens, and cryptographic keys thanks to its immutability.


Clojure’s ability to handle concurrency made it easier to process extremely large files split into smaller chunks. The functional paradigm made the composition of security steps clearer and more reliable. And interoperability with the Java ecosystem opened the door to stable, battle-tested libraries widely used in the cybersecurity industry.


Over time, even with other technologies surrounding the system, Clojure naturally emerged as the core of the NuFilebox Reverse.


## A security approach that starts on the client side


In the traditional upload model, the client sends the file via HTTPS, the server receives the content in plain text, and only then applies encryption before storing it. This pattern, although common, presents an obvious vulnerability: if the server is compromised, the files are exposed before they are protected.


The team took the opposite approach. Encryption occurs before the file even leaves the client machine. The front end requests only minimal data from the back end, such as the key and the Initialization Vector (IV), which determine where the encryption starts. The server records this information and stores it in DynamoDB without ever touching the file’s content. From there, the front-end itself splits the file into smaller parts, encrypts each piece locally, and uploads them directly to S3 through pre-signed URLs. The server never sees the content. It manages only metadata. Logic stays on one side, data on the other.


It is a truly zero-knowledge architecture, built on the principle of security by design. The entire pipeline was created so that the file is protected from the very first moment of its journey.


## Decryption, reassembly, and the role of Clojure


Once the encrypted file chunks arrive at S3, AWS triggers a chain of events. Lambdas determine which workflow to follow and route the file to the service responsible for merging and decrypting the fragments. This step, implemented in Clojure, consults information stored in DynamoDB such as the key, IV, identifier, and file name, and begins the reconstruction process.


Each part is downloaded from S3, decrypted individually, and reassembled in its original order. This step is essential for identifying malicious patterns, since a malware signature may be split between two file parts. Full reconstruction ensures accuracy in the next phase.


## A second layer of defense with YARA Rules


Once the file is restored to its original format, NuFilebox Reverse performs malware analysis using YARA, one of the most established tools in the industry for threat detection. YARA allows the creation of rules that describe suspicious behaviors, code fragments, encrypted patterns, improper API calls, questionable external connections, or even entire code sections characteristic of specific malware families.


The team combined manually created rules, rules from trusted repositories, and the generation of an indexed file to streamline scanning.


## How the system handled increasingly large files


The group also shared practical results. Files up to five hundred megabytes completed the process in just a few seconds. One-gigabyte files took about four minutes. Ten-gigabyte files finished in around forty-five minutes. In the most extreme scenario, thirty-gigabyte files concluded in roughly two hours.


Even in these cases, performance was better than what is typically seen in widely used services adopted by large companies. And unlike many of these platforms, NuFilebox Reverse integrates malware detection before any data enters Nubank’s internal ecosystem.


## Applied research with real-world impact


One of the most compelling aspects of the presentation was seeing how NuFuturo operates in practice. The project brings together students, professors, researchers, and engineers in continuous cycles of collaboration. Eric and Isaac’s team received direct guidance from Nubank professionals starting in April 2024. The result is a system that solves a real problem, creates opportunities for new talent, and expands the use of Clojure into areas that go beyond typical product workflows.


The final session brought questions about immutability, possible uses of Datomic, implementation decisions, and future challenges. The presenters emphasized that there is still plenty of room to evolve, from exploring more performant ways to execute YARA rules to experimenting with new approaches that could reduce processing time for extremely large files.


NuFilebox Reverse is a concrete example of how partnerships between industry and academia can produce solutions that combine deep research, functional engineering, and direct impact on a real-world problem. It demonstrates how long-term vision, technical curiosity, and institutional collaboration can transform into technology that protects millions of people every single day.
