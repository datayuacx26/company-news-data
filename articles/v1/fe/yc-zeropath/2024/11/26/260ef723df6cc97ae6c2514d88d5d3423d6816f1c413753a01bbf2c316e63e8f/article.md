---
schema_version: "1.0.0"
document_id: "260ef723df6cc97ae6c2514d88d5d3423d6816f1c413753a01bbf2c316e63e8f"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/how-zeropath-works"
published_at: "2024-11-01T00:00:00+00:00"
first_seen_at: "2026-07-22T21:04:50.046038+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:dd7cd3070f2411f66b8047ed9432a63db28239c2f21c70d29add25d1bcb0b986"
---

# How ZeroPath Works

> **Update:** This "How ZeroPath Works" post is kept for historical reference. Current architecture and capabilities have evolved over time.


---


> **TL;DR: This post explains how ZeroPath's unified AppSec platform (SAST, SCA, secrets, IaC, and EOL checks) works, focusing on its AI-powered SAST engine.**
>
>
> - **Unified vulnerability coverage:** finds business-logic flaws, authentication/authorization bypasses, and classic vulnerabilities such as SQLi, XSS, and SSRF.
> - **Continuously adaptive detection:** the tool automatically updates to recognize new vulnerability classes, such as prompt-injection attacks.
> - **End-to-end workflow:** trigger → AST → enriched graph → vulnerability discovery → vulnerability validation → patch generation.
> - **Fast developer experience:** pull-request scans finish in under 60s on GitHub, GitLab, Bitbucket, and Azure DevOps across 16+ languages.
> - **Signal over noise:** reachability-aware data-flow analysis surfaces only exploitable paths, cutting false positives.
> - **Fixes, not just findings:** the platform opens PRs with validated patches; developers can refine them using natural-language prompts.
> - **Enterprise-ready deployment:** available as SaaS or on-premises via private cloud, with SSO/SAML support, SOC 2 and GDPR compliance (ISO 27001 underway).
> - **Proven in the wild:** more than 125,000 scans each month for 750+ organizations, with critical CVEs disclosed in Netflix, Hulu, and Salesforce code.
>
>
> **→[Schedule a live demo or set up a PoC](https://zeropath.com/demo)**


---


ZeroPath is an Application Security (AppSec) platform that provides a way to manage all of your code security at once. This post focuses specifically on ZeroPath's Static Application Security Testing (SAST) engine, the AI-driven core that analyzes source code to detect security vulnerabilities. But this engine is also how we drive SCA and Custom Policy results.


ZeroPath finds vulnerabilities without relying on rules, rule-based engines, or long lists of language specific "sinks" and "sources". This is unusual in AppSec, where most vendors are quietly relying on Semgrep, OpenGrep, or other open-source policy engines. But building our tooling from the ground up with LLMs allows the system to automatically adapt and find new types of issues without requiring developer specification. Additionally, it allows teams to define custom security rules using natural language, making it easier to enforce organization-specific security policies without writing complex regex patterns.


Developed by security engineers, our custom-built SAST tool identifies complex issues often missed by conventional methods, including business logic flaws, authentication vulnerabilities, and emerging threats like AI-specific vulnerabilities such as prompt injection attacks. Traditional SAST tools struggle to detect business logic and authentication vulnerabilities because they rely on pattern matching rather than understanding application behavior. ZeroPath has been used to uncover vulnerabilities in major open-source repositories owned by companies like Netflix, Hulu, and Salesforce. By understanding code context and functionality through our proprietary AI engine, ZeroPath's SAST capabilities provide accurate detection with fewer false positives.


The tool performs pre-merge scanning, analyzing code before it's merged into the main codebase. Pull-request scans typically complete in under 60s, enabling early detection of security issues without impeding development workflows.


ZeroPath currently provides first-class support for C, C++, PHP, Go, Ruby, Perl, Rust, Python, Scala, Kotlin, Swift, JavaScript/TypeScript, Dart, Elixir, Java, and C#, including templating engines like Svelte and Embedded Ruby. It uses the tree-sitter library for Abstract Syntax Tree (AST) generation, allowing rapid integration of additional or custom language grammars.


Given the prevalence of AI in security tools, we prioritize transparency. The following sections provide an overview of ZeroPath's functionality and demonstrate how our approach has led to the discovery of critical vulnerabilities in production systems used by major companies.


## Why AI-Driven SAST Matters


Traditional SAST tools face fundamental limitations in modern application security. While they excel at finding certain pattern-based vulnerabilities, they struggle with context-dependent issues that require understanding how code actually behaves. This includes business logic flaws, authentication bypasses, and complex data flow vulnerabilities that don't match predefined patterns.


ZeroPath's AI-driven approach represents a different paradigm. Instead of relying solely on rules and patterns, our engine understands code semantics and can reason about application behavior. This enables detection of vulnerability classes that rule-based systems simply cannot identify.


For teams interested in detailed performance comparisons, we've published our[benchmarking methodology and results](https://zeropath.com/blog/benchmarking-zeropath) . The key takeaway is that AI-driven analysis opens up entirely new categories of vulnerability detection, particularly for business logic and authentication issues that have traditionally required manual security reviews.


## ZeroPath Architecture and Workflow


ZeroPath's vulnerability detection process consists of six key stages, each designed to contribute to the accuracy of its findings. The following section breaks down these stages, providing a technical overview of how the system operates to identify security issues in code bases.


```text


```


### Triggering a Scan


ZeroPath scans can be initiated through multiple methods:


1.


**Scheduled/Manual Scans**


These are initiated through the dashboard, CLI, or our[fully documented API](https://zeropath.com/docs/api-reference/organizations/list-organizations) (with TypeScript SDK support), or occur at predefined intervals. They perform a more complete analysis of the entire codebase, designed both to catch vulnerabilities in code that may have bypassed PR checks (e.g., direct pushes to the main branch) and refresh the intensive parts of analysis. This document describes what happens when you do a manual scan.


2.


**Pull-Request Scans**


PR scans are triggered automatically when a pull request is created on your repository. They are implemented as a GitHub check (and similarly for GitLab, Bitbucket, and Azure Pipelines), which avoids using CI/CD minutes, and focus on analyzing only the modified code. To optimize performance, the AST from previous scans is cached, and PR scans complete in around 30 seconds on average. Only the nodes corresponding to updated code are re-parsed and integrated into the existing AST. This incremental updating approach significantly reduces processing time while maintaining high accuracy, resulting in efficient and precise scans.


3.


**Auto AppSec Mode**


For teams wanting continuous security coverage, repositories can be configured to scan automatically on a schedule with high severity issues automatically generating pull requests for remediation.


### Application Identification


ZeroPath starts by using AI agents to investigate what applications are inside your repository and gather some basic data about how they work. This step is crucial when dealing with mono-repositories or repositories containing multiple services, as often happens with microservice architectures. Specifically, we:


1. Identify directory boundaries for each application
2. Generate application descriptions and metadata, noting details like the auth procedure and tech stack
3. Collect additional contextual information helpful for subsequent analysis stages


This process helps ensure that ZeroPath has enough information about your apps to discriminate between relevant and irrelevant security issues.


### AST Generation and Indexing


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


That's how it's represented as plain text, but as with most languages this is broken down into an intermediate representation before compilation. Using tree-sitter we can convert the method definition into an AST that has standard names for things like` function_definition` ,` body` , and etc.:


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


This format allows for a detailed, hierarchical view of the code structure, making it easier to analyze. In particular, from the AST we create a **call graph** , which is a map of the program's function invocations. The call graph facilitates navigation through the codebase during vulnerability analysis, and also provides a complete summary of the application's structure and behavior. This holistic understanding is key to our tool's ability to detect complex, context-dependent vulnerabilities, and it looks something like this:


```text


```


### Graph Enrichment


After generating the AST, we enrich the graph with contextual information by identifying features like endpoints (exposed functions or URLs that can be accessed externally) and assigning attributes to each node. These attributes can be details such as request paths, HTTP methods, and authentication and authorization mechanisms. For example, a node representing a login function might be enriched with attributes indicating it accepts POST requests, and implements rate limiting. A key aspect of this enrichment is recognizing how middleware and other security controls are implemented across the application. This process transforms the basic AST into a more comprehensive representation of the application's structure and behavior. While the initial AST shows the structure of individual functions, this enriched call graph demonstrates how these functions interact and what security measures are in place throughout the application flow.


```text


```


### Vulnerability Discovery and Validation


Finally we get to the most important part, using the call graph to discover vulnerabilities. In our application security analysis, vulnerabilities are bucketed into three main types:


1.


**Conventional Vulnerabilities** : These encompass implementation-specific security flaws such as SQL Injection (SQLi), XML External Entity (XXE) injection, Cross-Site Scripting (XSS), Cross-Site Request Forgery (CSRF), leaking of secrets, and Server-Side Request Forgery (SSRF).


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


```text


```


- AI-driven Sink and Source Analysis: ZeroPath uses AI to identify potential vulnerability sinks (dangerous functions) and sources (user inputs) throughout the codebase, then traces data flow between them to detect exploitable paths.
- Natural Language Rules: Teams can define custom security policies using plain English, which the system converts into security checks.
- Threat Modeling: ZeroPath generates attack scenarios and verifies them by performing rigorous investigations of the code.
- Integrated SCA with Reachability Analysis: While SCA is a separate component of our platform, it feeds vulnerability data directly into our SAST engine's source-to-sink analysis. The SCA component identifies vulnerable dependencies and maps CVEs to specific functions within those dependencies, leveraging OSV.dev's comprehensive vulnerability database that aggregates GitHub advisories, ecosystem-specific security bulletins, and other trusted sources. These vulnerable functions are treated as sinks in our AST-based analysis.


```text


```


The SAST engine then determines if these vulnerabilities are exploitable by tracing whether user-controllable inputs can reach them through the call graph, accounting for:


- Sanitization functions in the execution path
- Authentication and authorization checks
- The specific context of how the dependency is used


This approach eliminates many false positives since we only flag dependencies when there's an actual exploitable path from user input to the vulnerable code, rather than simply reporting all CVEs in your dependency tree.


While this post focuses on our SAST capabilities, it's worth noting that ZeroPath's platform also includes other complementary security tools like secret scanning and validation, and Infrastructure as Code (IaC) scanning. These tools work together to provide application security coverage.


Our methodology for investigating business logic flaws and broken authentication vulnerabilities combines two AI techniques:[Tree-of-Thoughts (ToT)](https://arxiv.org/abs/2305.10601) and an adaptation of the[ReAct framework](https://arxiv.org/abs/2210.03629) .


ToT enables multi-path reasoning, intermediate step evaluation, and outcome ranking. This improves our ability to explore complex vulnerability scenarios. The ReAct-inspired component enforces structured tool usage with explicit action justification, enhancing the rigor of our investigative process.


By integrating these techniques, we've developed a framework that allows for thorough vulnerability assessment. ToT facilitates thorough scenario exploration, while the ReAct adaptation ensures methodical tool application. This approach has proven particularly effective in addressing the nuanced challenges presented by business logic and authentication vulnerabilities.


To further enhance our validation process and ensure the exploitability of identified vulnerabilities, ZeroPath relies on a multi-agent validation pipeline. The system enumerates candidate attack paths surfaced during investigation, scores their plausibility and impact using CVSS 4.0 heuristics, and requests additional evidence whenever confidence falls below an acceptable threshold.


Each vulnerability class—such as SSTI, SQLi, XSS, or business logic flaws—routes to a specialized verification agent. These agents pull relevant context from earlier stages, analyze available controls that might block the attack, and weigh the practicality of exploiting the issue in production. When the agent's assessment clears the predefined practicality threshold, the finding advances to patch generation and tweaking.


### Patch Generation, Tweaking, and Integration


After discovering your vulnerability, we do a first pass check to see if we can fix it safely. Some vulnerabilities, like hardcoded secrets and SSRF, often require redesign and are marked unpatchable.


For issues deemed patchable, we leverage the data collected from earlier stages, leaning towards existing sanitization functions and minimal code modifications. Our validation process verifies vulnerability remediation, ensures syntactic correctness and functional preservation, and rescans to prevent new vulnerabilities. Patches failing validation undergo refinement.


Deployment methods vary: manual scans allow direct user approval and application via pull requests, while PR scans automatically create pull requests for fixes. Teams can customize PR formatting including titles, descriptions, branch names, and commit messages to match their conventions.


```text


```


This code generation system supports multi-file, codebase-wide changes, incorporating techniques from[AutoCodeRover](https://arxiv.org/abs/2404.05427v2) . It integrates with PRs, allowing developers to refine patches using natural language commands. The system can implement complex modifications across multiple files without requiring manual coding for each adjustment.


### Security Insights and Team Collaboration


Our tool also provides an (optional) centralized dashboard for managing application security across your organizations. It supports multi-user repository sharing with team and organization-based access controls, and provides visualization tools for security posture assessment and trend analysis. For enterprise authentication, ZeroPath integrates with your existing SSO providers including SAML 2.0 and OIDC-compliant systems (Okta, Azure AD, Google Workspace, OneLogin, etc.), enabling seamless user provisioning and centralized access management. The platform includes reporting capabilities that track metrics like mean time to remediation, most common vulnerability classes by language and framework, and vulnerability attribution by teams or authors.


Users can access repository-specific views with customizable filtering options. This feature facilitates the prioritization of high-severity issues, enabling teams to focus on the most critical security concerns within their codebase. The platform integrates with GitHub, GitLab, Bitbucket, and Azure Pipelines for version control interaction, and also supports direct code base uploads via zip files for projects using other version control systems or none at all.


For broader workflow integration, ZeroPath supports Linear and Jira for issue tracking, along with Slack and email notifications to keep teams informed of security findings. Teams can also interact with vulnerabilities through our Q&A feature, which provides full codebase context for remediation advice and exploitation understanding. The platform offers complete source-to-sink visibility, showing full call graph relationships from the vulnerability source to the exploitable code snippet.


Additional capabilities include SBOM (Software Bill of Materials) generation and export, audit logs for compliance, and CWE mapping for standardized vulnerability classification. For organizations requiring multi-tenancy, ZeroPath provides MSP (Managed Service Provider) support with control over multiple organizations. Developers can also leverage our[fully documented API](https://zeropath.com/docs/api-reference/organizations/list-organizations) with TypeScript SDK for custom integrations.


## Public Vulnerabilities and Research


The ZeroPath team regularly uses the tool to find new vulnerabilities in open-source projects. We've disclosed numerous significant vulnerabilities in production systems, including:


- Local file inclusion in[e2nest](https://github.com/Netflix/e2nest) , a Netflix application used internally (CVE-2024-9301)
- Unauthorized Redis access in[Monaco](https://github.com/hulu/monaco) , a Hulu application allowing access to all Redis clusters (CVE-2024-48946, currently reserved)
- Directory traversal in[LogAI](https://github.com/salesforce/logai) , a Salesforce tool exposing sensitive files (CVE-2024-48945, currently reserved)
- Broken and missing authentication flaws in[LibrePhotos](https://github.com/LibrePhotos/librephotos)
- Arbitrary file upload in[LibrePhotos](https://zeropath.com/blog/librephotos-arbitrary-file-upload-vulnerability)
- Remote code execution in[Uptrain](https://zeropath.com/blog/uptrain-rce-vulnerability-analysis)
- Command injection in[Clone-Voice](https://zeropath.com/blog/command-injection-vulnerability-clone-voice)
- Local file inclusion in[Fonoster Voice Server](https://zeropath.com/blog/fonoster-voiceserver-lfi-vulnerability)


Note: Some CVEs listed are in reserved status pending full disclosure coordination with vendors.


For a complete and up-to-date list of all our vulnerability discoveries, visit our[0-day discoveries blog post](https://zeropath.com/blog/0day-discoveries) and our[Security Wall of Fame](https://zeropath.com/wall) . Our team is actively working on responsibly disclosing its current batch of vulnerabilities, reinforcing our commitment to improving security across the open-source ecosystem.


## Try ZeroPath


ZeroPath is now publicly available, with 750+ companies of various sizes already using our solution across finance, healthcare, technology, and other sectors, performing 125,000+ scans monthly. The platform is SOC 2 Type II and GDPR compliant, with ISO 27001 certification currently in progress, ensuring enterprise-grade security and data protection standards.


**Ready to see ZeroPath in action?[Schedule a personalized demo](https://zeropath.com/demo) to:**


- Explore key features and capabilities.
- Discuss your specific use cases and deployment options (SaaS or on-premises).
- Set up a proof-of-concept for your organization.
- Get implementation and scaling insights.
