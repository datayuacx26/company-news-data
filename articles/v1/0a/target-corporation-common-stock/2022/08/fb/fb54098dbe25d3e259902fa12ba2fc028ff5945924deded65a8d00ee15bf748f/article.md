---
schema_version: "1.0.0"
document_id: "fb54098dbe25d3e259902fa12ba2fc028ff5945924deded65a8d00ee15bf748f"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/strelka"
published_at: "2022-08-24T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:1a78848d587742c74275a4c97d9400dcc475fd00fac1baaad019e88500562894"
---

# Strelka: Real-Time Threat Hunting Scanner

[Strelka](https://github.com/target/strelka) is a real-time, container-based, file scanning system used for threat hunting, threat detection, and incident response, built by our Target cybersecurity team. A fork of Lockheed Martin's


[Laika BOSS](https://github.com/lmco/laikaboss) project, Strelka's purpose is to perform file extraction and metadata collection at an enterprise scale. For example, if a JavaScript file is identified by Strelka, metadata such as hyperlinks, functions, and forms are extracted for analysis, storage, and aggregation. In this post, we’ll examine how Strelka works and the benefits of deploying Strelka to your network.


Too Much Data


Millions of files are transferred through networks every day ranging from documents to executables to compressed files and countless more. Data traverses the network without providing much awareness or context for analysts - eventually requiring incident responders to connect to hosts or capture network traffic to dig into file content. Assuming files are still available at the time of request, this activity requires analysts to perform data collection activities and potentially basic file or malware analysis.


Enter Strelka


Strelka is a modular active data collection platform, allowing analysts to observe file content for any defined file types without ever needing to perform active file collection. Data is submitted by an endpoint (e.g., network sensor, host) to a Strelka cluster, analyzed, normalized into


[(JSON)](https://www.json.org/json-en.html) , and passed on to another endpoint. Coupled with a security information and event management system


[(SIEM)](https://en.wikipedia.org/wiki/Security_information_and_event_management) , Strelka can aggregate, alert, and provide analysts with the capability to better understand their environment without having to perform initial data gathering or file analysis.


Active File Analysis


The easiest way to understand Strelka is by observing its foundation: scanners. Scanners are Python scripts whose role is to perform data analysis per filetype. These scanners are provided files by Strelka based on the taste, or signature, of an identified file. For example, if Strelka has a signature for docx file identification, it executes the Strelka docx scanner. This specific scanner handles the extraction of data such as: author, comments, last modified timestamps, and more before passing it back the results back to Strelka. Here are a few more predefined scanners and some of the data they collect:


HTML hta_file, text/html, html_file Hyperlinks, Title, Forms, Frames, Scripts


JavaScript text/html, html_file Identifiers, Strings, Tokens


ZIP application/zip, application/vnd.openxmlformats-officedocument, ooxml_file In addition to collecting the quantity and size of the compressed file, Strelka will decompress the file and pass the contents back into the Strelka pipeline.


PDF application/pdf, pdf_file Annotated URIs, Objects


YARA All Scans all files for any provided YARA files and returns matches.


Table 1: Scanner Examples


At a high level, Strelka is a mostly simple process.


Figure 1: Strelka Overview


visualizes the data flow for files in the Strelka pipeline. A


[go application](https://github.com/target/strelka/blob/f87d2ca15c38d54a4b4401e0a56d6e6d46212eb9/docs/README.md#client-apps) is used to upload or stream files into the Strelka cluster where a backend server performs file tasting, or type identification. After that, relevant scanners, like those defined above, are executed against the files. Execution of analysis is defined in a configuration file on the server that allows administrators to set size and timeout requirements. Upon completion, the data is packaged into a JSON file and shipped to an additional endpoint or back to the submitter. For more detailed information on the design of Strelka and component-to-component details, please see the


[Readme.](https://github.com/target/strelka/blob/f87d2ca15c38d54a4b4401e0a56d6e6d46212eb9/docs/README.md#design)


Figure 1: Strelka Overview


Response example


The following, also available in the


[Readme](https://github.com/target/strelka/blob/f87d2ca15c38d54a4b4401e0a56d6e6d46212eb9/docs/README.md#identifying-suspicious-text?scl=1) , is a response example from Strelka showing how a typical response object is generated. This example shows a scan result for a text file that appears to be a shell script containing an IP address. The IP address is redacted to prevent accidental navigation.


```text
{     "file": {       "filename": "VirusShare_1860271b6d530f8e120637f8248e8c88",       "depth": 0,       "flavors": {         "mime": [           "text/plain"         ]       },       "scanner_list": [         "ScanYara",         "ScanHash",         "ScanEntropy",         "ScanHeader",         "ScanUrl"       ],       "size": 1856     },     "tree": {       "node": "c65e5d0a-3a7d-4747-93bd-7d02cb68e164",       "root": "c65e5d0a-3a7d-4747-93bd-7d02cb68e164"     },     "hash": {       "md5": "1860271b6d530f8e120637f8248e8c88",       "sha1": "ca5aaae089a21dea271a4a5f436589492615eac9",       "sha256": "779e4ae1ac987b1be582b8f33a300564f6b3a3410641e27752d35f61055bbc4f",       "ssdeep": "24:cCEDx8CPP9C7graWH0CdCBrCkxcCLlACCyzECDxHCfCqyCM:g9LPnPWesnV"     },     "entropy": {       "entropy": 4.563745722228093     },     "header": {       "header": "cd /tmp || cd /var/run || cd /mnt || cd /root || c"     },     "url": {       "urls": [         "[redacted]"       ]     }   }
```


Contribute


Since Strelka is an open source tool available on GitHub, the community can utilize this high-speed, modular, file analysis platform either in response, investigations, or hunting. A


[Readme](https://target.github.io/strelka/#/) is available to provide users with a detailed understanding of getting started, installation, architecture, modules, how to contribute, and more. The best way to get started with


[contributing](https://github.com/target/strelka/blob/master/CONTRIBUTING.md) to the Strelka project is through scanner development. These scanners are written in Python 3 and if you have any ideas on new types of scanners (or want to have a go at writing your own) we encourage you to submit those as an Issue or Pull Request.


This post has only highlighted a foundational overview of Strelka and we recommend you give it a test with the


[quickstart](https://github.com/target/strelka/blob/f87d2ca15c38d54a4b4401e0a56d6e6d46212eb9/docs/README.md#quickstart) . While we only touched on a few concepts and scanners, Strelka isn’t defined only by filetypes and metadata. Scanners can be developed to perform low level analysis as well as pass files to other tools (e.g., sandboxes). If you have any questions, concerns, or feedback about Strelka,


[contact us via GitHub](https://github.com/target/strelka) .
