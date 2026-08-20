---
schema_version: "1.0.0"
document_id: "b75246711bba7d837a9071e6df93ab10c2cbd48bf4392f736e26bdfd09695fa0"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/0day-discoveries"
published_at: "2024-10-29T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:4a945161577eaba9591f179ea3baa9291a094399b686028dc407c64f9026a30e"
---

# Autonomous Discovery of Critical Zero-Days

AI-driven 0-day detection is here. AI-assisted security research has been quietly advancing since early 2023, when AIxCC researchers demonstrated the first practical applications of LLM-powered vulnerability detection in AI systems. Modern LLMs have been used to improve the accuracy of detections of existing classes of web issues (XSS, SQLi, CSRF) and find business logic and authentication problems that were previously undetectable by SAST.


So what does this mean in terms of AIs' ability to do unsupervised security research? Since July 2024, ZeroPath is taking a novel approach combining deep program analysis with adversarial AI agents for validation. Our methodology has uncovered numerous critical vulnerabilities in production systems, including several that traditional Static Application Security Testing (SAST) tools were ill-equipped to find. This post provides a technical deep-dive into our research methodology and a living summary of the bugs found in popular open-source tools.


---


## Summary of Vulnerabilities Discovered


**Note:** This list represents only a portion of our findings. Many vulnerabilities remain undisclosed due to ongoing remediation efforts or pending responsible disclosure processes. We will update this list as new issues are disclosed over the next few months.


Date Project Vulnerability Technical Impact Root Cause CVE/Reference


Jul 21, 2024 **Fonoster Voice Server** Local File Inclusion Access to system files via voice file paths Incomplete path validation CVE-2024-43035


Jul 22, 2024 **Uptrain** Remote Code Execution Arbitrary code execution via eval RCE during project creation Pending Assignment


Aug 22, 2024 **LibrePhotos** File Upload + Path Traversal Arbitrary file write via photo upload Insufficient path sanitization Pending Assignment


Aug 22, 2024 **Clone-Voice** Command Injection System command execution via voice file metadata Unescaped input in ffmpeg command Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized Conversation Deletion Complete deletion of other users' chat history Missing object-level authorization checks Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized Canvas Deletion Deletion of other users' visualization canvases Insufficient IDOR protection on API endpoint Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized Knowledge Base Access Read access to other users' private knowledge bases Missing tenant isolation in KB queries Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized File Movement Moving/deleting other users' uploaded files Missing ACL checks in file operations Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized Conversation Access Reading other users' private conversations Broken access control in chat retrieval Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized API Key Removal Removal of other users' API keys IDOR in key management endpoint Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized Knowledge Base Enumeration Enumeration of all private knowledge bases Missing authentication in list endpoint Pending Assignment


Sep 2, 2024 **RAGFlow** Unauthorized Dialog Deletion Mass deletion of other users' dialogs Race condition in deletion endpoint Pending Assignment


Sep 3, 2024 **E2nest (Netflix)** Local File Inclusion Arbitrary file read via path traversal in model loading Insufficient path normalization in config loading CVE-2024-9301


Sep 5, 2024 **LibrePhotos** Unauthorized Access to User Jobs Access to other users' processing jobs Missing authorization in job queue Pending Assignment


Sep 5, 2024 **LibrePhotos** Token Refresh Authentication Bypass Complete authentication bypass Improper token validation Pending Assignment


Sep 20, 2024 **Monaco (Hulu)** Remote Code Execution Code execution via deserialization Unsanitized data being passed into pickle.loads CVE-2024-48946


Sep 20, 2024 **Monaco (Hulu)** Unauthorized Redis Access Access to all Redis clusters administered by Monaco Missing authentication in app_redis_api endpoint Pending Assignment


Oct 1, 2024 **LogAI (Salesforce)** Directory Traversal Access to sensitive files via log paths Broken path traversal protection Pending Assignment


Oct 24, 2024 **DB-GPT** Directory Traversal Access to database files via backup paths Missing path normalization Pending Assignment


For comprehensive technical analyses of some these vulnerabilities, please refer to our write-ups:


- **Uptrain RCE:**[Project Creation to RCE](https://zeropath.com/blog/uptrain-rce-vulnerability-analysis)
- **Clone-Voice Command Injection:**[From Voice Processing to Shell Access](https://zeropath.com/blog/command-injection-vulnerability-clone-voice)
- **Fonoster Voice Server LFI:**[Breaking Audio File Boundaries](https://zeropath.com/blog/fonoster-voiceserver-lfi-vulnerability)
- **LibrePhotos Arbitrary File Upload:**[From Upload to RCE](https://zeropath.com/blog/librephotos-arbitrary-file-upload-vulnerability)


---


## Vulnerability Distribution


```text


```


#### 1. Authorization Flaws


-


**Prevalence:** 53% of the vulnerabilities (10 instances)


-


**Common Issues:**


- Missing object-level access controls
- Insufficient tenant isolation
- Broken access control in API endpoints
- IDOR vulnerabilities in resource management
- Unauthorized Redis access and configuration exposure


-


**Impact:** Unauthorized access, data leakage, and resource manipulation across tenant boundaries.


-


**Examples:**


- RAGFlow's multiple IDOR vulnerabilities allow manipulation of conversations, canvases, knowledge bases, and API keys belonging to other users
- Unauthorized access to Redis instances due to missing access controls


#### 2. File Operation Issues


-


**Prevalence:** 26% of the vulnerabilities (5 instances)


-


**Common Issues:**


- Directory traversal in configuration loading
- Local file inclusion via path manipulation
- Unsafe file handling in upload features
- Insufficient path validation and normalization


-


**Impact:** Unauthorized file access, sensitive data exposure, and potential system compromise.


-


**Examples:**


- E2nest's LFI via model path traversal (CVE-2024-9301)
- DB-GPT's directory traversal in backup paths
- LogAI's broken path traversal protection


#### 3. Code Execution Vulnerabilities


-


**Prevalence:** 16% of the vulnerabilities (3 instances)


-


**Common Issues:**


- Unsafe pickle deserialization
- Command injection in file processing
- Unsanitized input in system commands


-


**Impact:** Remote code execution, system command execution, and potential full system compromise.


-


**Examples:**


- Monaco's RCE via pickle deserialization (CVE-2024-48946)
- Clone-Voice's command injection via ffmpeg metadata
- Uptrain's RCE via project creation


---


## Our Technical Methodology


TL;DR - most of these bugs are simple and could have been found with a code review from a security researcher or, in some cases, scanners. The historical issue, however, with automating the discovery of these bugs is that traditional SAST tools often rely on pattern matching and predefined rules, which can miss complex vulnerabilities that do not fit known patterns (i.e. business logic / broken authentication flaws or non-traditional sinks such as from dependencies). They also tend to generate a high rate of false positives, overwhelming security teams and reducing efficiency.


The beauty of LLMs is that they can reduce ambiguity in most of the situations that previously caused scanners to be either unusable or produce few findings when mass-scanning open source repositories like this. For instance, you can prevent:


- Alerting on test code
- Alerting on CLI only administrators would have access to
- Alerting on injection bugs whose "sinks" (i.e. injection sources) aren't able to be controlled by attackers in practice
- Alerting on injection bugs that include controls that make an attack possible (for instance, limiting the input to valid UUIDs)


To do this well, we combine deep program analysis with an adversarial agents that test the plausibility of vulnerabilties at each step. The solution ends up mirroring the traditional phases of a pentest - recon, analysis, exploitation (and remediation which is not mentioned in this post).


*Note: Most sections have been adopted from our[how it works](https://zeropath.com/blog/how-zeropath-works) post, for more information about patching and remediation please refer to it.*


### Stage 1: Application Identification


ZeroPath starts by using AI agents to investigate what applications are inside a repository and gather some basic data about how they work. This step is crucial when dealing with mono-repositories or repositories containing multiple services, as often happens with microservice architectures. Specifically, we:


1. Identify directory boundaries for each application
2. Generate application descriptions and metadata, noting details like the auth procedure and tech stack
3. Collect additional contextual information helpful for subsequent analysis stages


This process helps ensure that ZeroPath has enough information about the apps to discriminate between relevant and irrelevant security issues.


### Stage 2: AST Generation and Indexing


To illustrate the following steps, we will be using a[basic Django application](https://gist.github.com/r0path/dfb9c5657fc8b8d7c25b33d9cfcc6a26) that provides fundamental functionality for:


1. User management (creating and listing users)
2. Content management (creating and listing posts)
3. User authentication (login and logout capabilities)


Below is an example of the method to retrieve users from the application:


```text
class     UserViewSet  (  View  )  :
def     get  (  self  ,   request  )  :
users   =   User  .  objects  .  all  (  )
return   render  (  request  ,     'user_list.html'  ,     {  'users'  :   users  }  )
```


That's how it's represented as plain text, but as with most languages this is broken down into an intermediate representation before compilation. Using tree-sitter we can convert the method definition into an AST that has standard names for things like "function_definition", "body", and etc.:


```text
(  function_definition
name:   (  identifier  )      ; get
parameters:   (  parameters
(  identifier  )      ; self
(  identifier  )  )      ; request
body:   (  block
(  expression_statement
(  assignment
left:   (  identifier  )      ; users
right:   (  call
function:   (  attribute
object:   (  attribute
object:   (  identifier  )      ; User
attribute:   (  identifier  )  )      ; objects
attribute:   (  identifier  )  )      ; all
arguments:   (  argument_list  )  )  )  )
(  return_statement
(  call
function:   (  identifier  )      ; render
arguments:   (  argument_list
(  identifier  )      ; request
(  string  )      ; 'user_list.html'
(  dictionary
(  pair
key:   (  string  )      ; 'users'
value:   (  identifier  )  )  )  )  )  )  )  )      ; users
```


This AST representation breaks down the` get_users` function, showing its structure, parameters, and the operations it performs. Each node in the tree is represented by parentheses, with the node type followed by its children. Leaf nodes (like identifiers and strings) are represented directly. Comments after semicolons provide additional information or clarification about the nodes.


This format allows for a detailed, hierarchical view of the code structure, making it easier to analyze. In particular, from the AST we create a **call graph** , which is a map of the program's function invocations. The call graph facilitates navigation through the codebase during vulnerability analysis, and also provides a comprehensive summary of the application's structure and behavior. This holistic understanding is key to our tool's ability to detect complex, context-dependent vulnerabilities, and it looks something like this:


```text


```


### Stage 3: Graph Enrichment


After generating the AST, we enrich the graph with contextual information by identifying features like endpoints (exposed functions or URLs that can be accessed externally) and assigning attributes to each node. These attributes can be details such as request paths, HTTP methods, and authentication and authorization mechanisms. For example, a node representing a login function might be enriched with attributes indicating it accepts POST requests, and implements rate limiting. A key aspect of this enrichment is recognizing how middleware and other security controls are implemented across the application. This process transforms the basic AST into a more comprehensive representation of the application's structure and behavior. While the initial AST shows the structure of individual functions, this enriched call graph demonstrates how these functions interact and what security measures are in place throughout the application flow.


```text


```


### Stage 4: Vulnerability Discovery and Validation


Finally we get to the most important part, using the call graph to discover vulnerabilities. In our application security analysis, vulnerabilities are bucketed into three main types:


1.


**Conventional Vulnerabilities** : These encompass implementation-specific security flaws such as SQL Injection (SQLI), XML external entity (XXE) injection, Cross-site Scripting (XSS), Cross-site Request Forgery (CSRF), Leaking of secrets, and Server-side Request Forgery (SSRF).


2.


**Business Logic Flaws** : These vulnerabilities arise from flaws in the application's logic. Examples include:


- Price manipulation in e-commerce systems
- Exploitation of coupon systems leading to incorrect pricing
- Bypassing intended workflow sequences
- Lack of rate limiting especially when interacting with external APIs (leading to excessive charges from providers)


3.


**Authentication/Authorization Issues** : These stem from improper implementation of user authentication or access control mechanisms. Common subtypes include:


- Insecure Direct Object Reference (IDOR)
- Missing Function Level Access Control
- Broken Session Management


Each category requires distinct analysis techniques. Conventional vulnerabilities often involve pattern matching and taint analysis, business logic flaws require understanding of intended application behavior, and authentication/authorization issues necessitate comprehensive flow analysis of user sessions and permissions. Some ways we find these bugs:


- Static Rules: ZeroPath has a large set of static rules that detail vulnerable code patterns, which are then used to semantically search if a given codebase uses them. Using this we are able to detect many existing classes of issues.
- Threat Modeling: Having ZeroPath comes up with attack scenario and verifies them by performing rigorous investigations of the code.
- Software Composition Analysis (SCA): ZeroPath actively monitors dependencies used within the application for known vulnerabilities, and see if these dependencies' problems are exploitable from the outside.
- Secret Scanning and Validation: ZeroPath also scans for secrets and performs validation on the secrets to ensure that they're valid and provides information about each discovered secret to help quickly rotate and enforce best practices.


Our methodology for investigating business logic flaws and broken authentication vulnerabilities combines two AI techniques:[Tree-of-Thoughts (ToT)](https://arxiv.org/abs/2305.10601) and an adaptation of the[ReAct framework](https://arxiv.org/abs/2210.03629) .


ToT enables multi-path reasoning, intermediate step evaluation, and outcome ranking. This improves our ability to explore complex vulnerability scenarios. The ReAct-inspired component enforces structured tool usage with explicit action justification, enhancing the rigor of our investigative process.


By integrating these techniques, we've developed a framework that allows for comprehensive vulnerability assessment. ToT facilitates thorough scenario exploration, while the ReAct adaptation ensures methodical tool application. This approach has proven particularly effective in addressing the nuanced challenges presented by business logic and authentication vulnerabilities.


To further enhance our validation process and ensure the exploitability of identified vulnerabilities, ZeroPath employs a Monte Carlo Tree Self-refine (MCTSr) algorithm. This approach, inspired by recent advancements in AI problem-solving, allows us to efficiently explore and verify complex attack vectors.


#### Monte Carlo Tree Self-Refine (MCTSr)


Our MCTSr implementation builds upon research from Shanghai Artificial Intelligence Laboratory, Fudan University, and collaborating institutions. Their[work](https://arxiv.org/abs/2406.07394) on solving International Mathematical Olympiad problems using Monte Carlo Tree Search and self-refinement techniques provided a foundation that we've adapted for cybersecurity applications. We've modified this approach to navigate the decision trees involved in verifying security vulnerabilities, allowing both for more efficient exploration of potential attack vectors and fewer false positives.


The most important part of using MCTSr is defining a "win function". As a static analysis tool, our win function is implemented by an LLM that determines the chances that a hypothesized attack could work, and the severity of the problem.


The particular verification agent we use is different for problems like SSTI, SQLi, XSS, and business logic issues. Generally, an agent is designed to pull in relevant information from previous stages, and consider all of the controls that could make an attack impractical. If the LLM investigator determines that a given attack is above a given practicality threshold, it's sent for the next stage, which is patch generation and tweaking.


## Conclusion


AI-driven vulnerability detection is moving fast. While some are just now jumping into this field, it's been developing for a while. Since July 2024, we've been exploring how deep program analysis combined with adversarial AI agents can uncover critical bugs that might be overlooked by traditional tools.


What's intriguing is that many of these vulnerabilities are pretty straightforward—they could've been spotted with a solid code review or standard scanning tools. But conventional methods often miss them because they don't fit neatly into known patterns. That's where AI comes in, helping us catch issues that might slip through the cracks.
