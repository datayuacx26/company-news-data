---
schema_version: "1.0.0"
document_id: "9f1420a29127dba55ed03c448c1fae57b61298ee0c5d5d984fbda26c90be232e"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/whitepaper-blast-ai-powered-sast-scanner"
published_at: "2024-10-24T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:59:50.514445+00:00"
content_hash: "sha256:f77e64816e17c4e17acf46cc2620ca4e92bbf016c01583148f8702c4aa5aa861"
---

# Whitepaper: BLAST, the AI-powered SAST scanner

I’m Ahmad, the founder of Corgea. We’re building[Corgea AI SAST](https://corgea.com/products/ai-sast) , an application security platform that automatically finds, triage and fix insecure code. We help detect vulnerabilities other scanners can’t find like business logic flaws, reduce 30% of SAST findings with our false positive detection and accelerate remediation by ~80%.


## Executive Summary


Application security has long relied on Static Application Security Testing (SAST) tools to detect and mitigate code vulnerabilities. However, as software architectures grow more complex and security risks evolve, the limitations of traditional SAST tools become increasingly apparent. For a baseline definition, see our guide to[what SAST is](https://corgea.com/learn/what-is-sast) .


If you are deciding how BLAST-style AI-native analysis compares with traditional scanners, use our[best SAST tools](https://corgea.com/learn/best-sast-tools) buyer guide to structure the vendor evaluation.


A study found false negative rates for commercial tools between 56% and 68% \[1\]. This means that AppSec teams have massive blind spots. Additionally, another study found that many commercial tools have a precision between 18% and 36% \[2\]. This has reduced the value of SAST tools in the market and their effectiveness.


Traditional methods, such as **source-sink analysis** and **call-graphs** , are insufficient for identifying business logic vulnerabilities and managing the vast and dynamic nature of modern codebases. Furthermore, more recent techniques like **vector search** introduce new issues by overgeneralizing and retrieving irrelevant data.


**BLAST (Business Logic Application Security Testing)** is a next-generation tool designed to overcome these challenges. By combining **Large Language Models (LLMs)** with **Abstract Syntax Trees (ASTs)** and advanced static analysis, BLAST delivers a deeper **semantic understanding** of code, offering highly accurate vulnerability detection, reduced false positives, and actionable insights. In this white-paper, we explore BLAST’s technical architecture, its capabilities, and how it redefines the landscape of application security.


## Introduction


As the complexity of modern applications increases, security testing tools must adapt. Traditional SAST tools are effective at detecting low-level issues like SQL injection or buffer overflows but fail to address the broader challenges introduced by business logic flaws and sophisticated code interactions. The limitations of existing approaches—such as **source-sink analysis** , **call-graphs** , and **vector search** —make it difficult for security engineers and developers to get accurate, actionable results from security scans.


The shortcomings of traditional methodologies include:


-


**Source-Sink Analysis** : Often generates false positives due to an incomplete understanding of data flow and context-sensitive behavior. This method is not equipped to handle modern frameworks and dynamic language features.


-


**Call-Graphs** : Fail to account for dynamic behaviors like reflection and dependency injection, often overlooking critical vulnerabilities embedded within runtime dependencies.


-


**Vector Search** : Introduces semantic search to codebases, but retrieves irrelevant data, increasing noise and reducing precision.


With the advent of AI and the increased availability of computing power, tools that combine **LLMs** with traditional static analysis now have the potential to overcome these challenges. **BLAST** is designed specifically to address these issues by introducing a fundamentally new approach to code security: one that reasons about code like a human, traverses complex codebases, and handles both business logic vulnerabilities and traditional SAST issues.


## Problem with Traditional SAST


### Methodological Limitations


#### Source-Sink Analysis


**Source-Sink analysis** has been a cornerstone of SAST methodologies. It tracks the flow of untrusted data from its source (e.g., user input) to a vulnerable sink (e.g., a SQL query). While this is useful for identifying injection vulnerabilities, it fails to account for context-sensitive conditions like user authentication or complex validation steps that occur in modern web frameworks. As a result, **false positives** are common, and critical validation points are missed. Additionally, path explosion in large codebases can make the analysis inefficient.


The following is a prime example of a false positive where context matters. This hard-coded password is in a test suite. The SAST scanner did find a hard-coded password, but it doesn’t have the context to determine it’s a false positive.


To demonstrate this, we used a SAST tool to scan this code from PyGoat. It had found that this view ([introduction/apis.py](https://github.com/adeyosemanputra/pygoat/blob/master/introduction/apis.py) ) is vulnerable to CSRF because of the exemption of the CSRF decorator in the beginning. What the tool failed to determine is the context as to why CSRF was exempt.


Corgea detected this was a false positive by understanding the context as reported here:


` The route is not directly exposed to the user and requires authentication. Additionally, the code checks for valid input and performs server-side logic, which mitigates CSRF risks. The csrf_exempt decorator is likely used to enable API functionality in a controlled manner.


`


```text
@csrf_exempt
def ssrf_code_checker(request):
if request.user.is_authenticated:
if request.method == 'POST':
python_code = request.POST['python_code']
html_code = request.POST['html_code']
if not (ssrf_code_converter(python_code)):
return JsonResponse({"status": "error", "message": "Invalid code"})
test_bench1 = ssrf_html_input_extractor(html_code)


if (len(test_bench1) >4):
return JsonResponse({'message':'too many inputs in Html\n Try again'},status = 400)
test_bench2 = ['secret.txt']
correct_output1 = [{"blog": "blog1-passed"}, {"blog": "blog2-passed"}, {"blog": "blog3-passed"}, {"blog": "blog4-passed"}]
outputs = []
for inputs in test_bench1:
outputs.append(main.ssrf_lab(inputs))
if outputs == correct_output1:
outputs = []
else:
return JsonResponse({'message':'Testbench failed, Code is not working\n Try again'},status = 200)
correct_output2 = [{"blog": "No blog found"}]
for inputs in test_bench2:
outputs.append(main.ssrf_lab(inputs))
if outputs == correct_output2:
return JsonResponse({'message':'Congratulation, you have written a secure code.', 'passed':1}, status = 200)


return JsonResponse({'message':'Test bench passed but the code is not secure'}, status = 200,safe = False)
else:
return JsonResponse({'message':'method not allowed'},status = 405)
else:
return JsonResponse({'message':'UnAuthenticated User'},status = 401)
```


#### Call-Graphs


**Call-graphs** trace the execution paths of code by analyzing function calls and their relationships. While this is beneficial for tracking how data flows through an application, call-graphs fail to account for dynamic execution contexts. For example, reflection or dynamic dependency injection is common in modern languages like Java or Python, but traditional call-graphs cannot track these runtime behaviors. This leads to vulnerabilities being overlooked.


#### Vector Search & RAG


**Vector search** and **Retrieval-Augmented Generation (RAG)** have introduced new ways to semantically analyze code by embedding the code in vectors and searching for similar patterns across a large codebase. However, these techniques come with significant downsides: they often retrieve irrelevant files (e.g., migration scripts or tests) due to the high **token density** in large projects. This overgeneralization introduces noise, which leads to **higher false-positive rates** and reduced actionable insights.


For anyone who has used Cursor (I’m a huge fan and a daily user), can often see that RAG queries often balloons context to inappropriate levels. This is[Posthog](https://posthog.com/) , which is a large codebase, and just doing a simple search (“Where does this application process user logins?”) brought in 35 results to inspect. This leads to a lack of the focus and precision needed to perform a proper security scan.


## Gaps in Business Logic Detection


The fundamental limitation of traditional SAST tools is their inability to reason about **business logic vulnerabilities, missing authorization and authentication, etc** . These vulnerabilities arise when the application’s behavior does not align with its intended functionality, allowing attackers to bypass controls or exploit design flaws. Identifying these issues requires understanding the **purpose** of the code, not just its structure. Traditional SAST tools lack this **semantic understanding** , making them inadequate for detecting business logic flaws.


What makes business logic so difficult to detect with current approaches? Semantic context.


### Context is Everything in Security


Semantic context in the realm of software engineering refers to the broader meaning and intent behind a piece of code, rather than just its structure or syntax. It involves understanding how a particular block of code interacts with other parts of the system, including the logic, purpose, and functionality that it is designed to achieve. This goes beyond just analyzing the code at the surface level (e.g., individual functions, variables, or syntax rules) and includes the relationships and dependencies between different components, configurations, and even external libraries.


In security analysis, understanding **semantic context** is critical because it allows tools to:


1.


**Determine Intent** : Identify what the code is supposed to do, which is necessary for detecting business logic vulnerabilities that arise when the system doesn’t behave as intended.


2.


**Highlight Exposure:** Highlight data or function sensitivity exposure is critical when detecting vulnerabilities such as logging usernames and passwords. While logging itself might not be bad, the context of what is being logged is critical.


3.


**Identify Broader Relationships** : Recognize how different components (e.g., functions, classes, middleware, or external APIs) interact with each other to assess security risks arising from these interactions.


4.


**Handle Framework-Specific Behaviors** : Detect how framework-level features (e.g., middleware in Django or dependency injection in Spring) impact the application’s security, even if they aren’t explicitly invoked in the main codebase.


5.


**Reduce False Positives** : By understanding the broader context of the code, tools can avoid flagging harmless code as a vulnerability, a common issue with tools that only analyze code at the syntactic or structural level.


In summary, **semantic context** helps security analysis tools provide more accurate results by understanding what the code is supposed to achieve, not just how it’s written or structured.


### LLMs to the Rescue


**Large Language Models (LLMs)** are particularly effective at understanding **semantic context** because they are trained on vast amounts of data and learn to recognize complex patterns, relationships, and meanings in text. In the context of code analysis, LLMs can capture the broader intent behind the code by leveraging their ability to understand language, syntax, and the structure of code in a similar way they process natural language. This enables them to identify not just how code is written, but what it is designed to do, and how various components interact.


LLMs can grasp the nuances of different programming languages, frameworks, and application structures, making them well-suited to detect vulnerabilities that stem from the logic, flow, or interactions within a codebase. Their capacity to reason, interpret dependencies, and adapt to varying coding styles allows them to provide deeper insights into potential security risks that traditional rule-based static analysis tools often miss.


## Introducing BLAST: AI-Enhanced Static Analysis


**BLAST (Business Logic Application Security Testing)** introduces a new paradigm in application security testing by integrating advanced **LLMs** with **AST-based static analysis** . This combination allows BLAST to reason about the **meaning** of the code, its intended functionality, and the relationships between components within a complex system. Unlike traditional tools that rely solely on static rules and pattern-matching, BLAST understands **context** and **intent** —allowing it to detect vulnerabilities that are otherwise missed by conventional SAST approaches.


### Semantic Understanding of Code


BLAST leverages **Corgea’s CodeIQ engine** to provide a deep **semantic understanding** of the codebase. Instead of analyzing individual functions or variables in isolation, CodeIQ builds a holistic view of the entire system, including how different components—such as services, middleware, and external libraries—interact with one another. This allows it to traverse codebases and detect vulnerabilities that arise from complex, cross-component interactions.


For example, in a **Django** application, BLAST can analyze the impact of middleware like password brute-force protection defined in a` settings.py


`


file and understand its relationship with other parts of the system such as a login function. Traditional SAST tools, which rely on call-graphs, would miss this middleware configuration, as it is not explicitly invoked in the code.


### Detecting Business Logic Vulnerabilities


One of BLAST’s key differentiators is its ability to detect **business logic vulnerabilities (CWE-840)** —vulnerabilities that arise from design flaws rather than code-level mistakes. These vulnerabilities are notoriously difficult to catch because they require an understanding of the **intended behavior** of the system. BLAST excels at detecting issues like:


-


**Transaction tampering** : Manipulating workflows to bypass purchase limits or restrictions.


-


**Authentication flaws (CWE-287)** : Identifying insecure authentication mechanisms or missing multi-factor authentication.


-


**Authorization bypasses (CWE-285)** : Detecting improper access control where users can perform actions they shouldn’t be allowed to.


#### Example


This vulnerability was found in[PyGoat](https://github.com/adeyosemanputra/pygoat) . Corgea multiple serious vulnerabilities this public API endpoint.


**Finding 1 as reported by Corgea: Malicious Code Execution**


` The code allows writing arbitrary code to files, which can be exploited to execute malicious actions, compromising the system's security. - The "log_code" and "api_code" are written directly to files "main.py" and "api.py", allowing execution of harmful code. - An attacker can send malicious code in "POST" requests, which gets saved and potentially executed, leading to unauthorized actions. - The code handles sensitive operations like file writing and HTTP requests, making it a target for malicious exploitation.


`


**Finding 2 as reported by Corgea: Authentication Bypass**


` The code allows bypassing authentication by using different HTTP methods without verifying user identity, potentially exposing sensitive operations. - The function processes requests without checking user authentication, allowing unauthorized access to "log_code" and "api_code". - Multiple HTTP methods ("GET", "POST", "PATCH", "DELETE") are used on a URL without authentication checks, risking unauthorized actions. - Sensitive operations like writing to files and clearing logs are performed without verifying the user's identity, leading to potential misuse.


`


The code:[introduction/apis.py](https://github.com/adeyosemanputra/pygoat/blob/master/introduction/apis.py)


```text
@csrf_exempt
# @authentication_decorator
def log_function_checker(request):
if request.method == 'POST':
csrf_token = request.POST.get("csrfmiddlewaretoken")
log_code = request.POST.get('log_code')
api_code = request.POST.get('api_code')
dirname = os.path.dirname(__file__)
log_filename = os.path.join(dirname, "playground/A9/main.py")
api_filename = os.path.join(dirname, "playground/A9/api.py")
f = open(log_filename,"w")
f.write(log_code)
f.close()
f = open(api_filename,"w")
f.write(api_code)
f.close()
# Clearing the log file before starting the test
f = open('test.log', 'w')
f.write("")
f.close()
url = "http://127.0.0.1:8000/2021/discussion/A9/target"
payload={'csrfmiddlewaretoken': csrf_token }
requests.request("GET", url)
requests.request("POST", url)
requests.request("PATCH", url, data=payload)
requests.request("DELETE", url)
f = open('test.log', 'r')
lines = f.readlines()
f.close()
return JsonResponse({"message":"success", "logs": lines},status = 200)
else:
return JsonResponse({"message":"method not allowed"},status = 405)
```


In the video below, Ahmad showcases how our AI-powered code scanner, BLAST, identifies critical vulnerabilities in code, such as weak password requirements and the use of insecure cryptographic algorithms. He demonstrates how Corgea’s technology goes beyond traditional scanners by understanding semantic context and providing effective fixes. Watch to see how we catch vulnerabilities early in the development process.


### Reducing False Positives


Traditional SAST tools are known for generating a high volume of **false positives** , which leads to alert fatigue and inefficient triaging. BLAST addresses this problem by using its **AI engine** to filter out irrelevant findings based on the **context** of the application. By analyzing the entire system and understanding how components interact, BLAST can distinguish between genuine security risks and benign issues, dramatically reducing false-positive rates.


## Architecture and Workflow Integration


Corgea CodeIQ fundamentally changes how we approach secure code analysis by leveraging Artificial Intelligence (AI), Abstract Syntax Trees (ASTs), and project analysis. Here’s how it works:


1.


Project Parsing: CodeIQ analyzes the entire project, and uses ASTs to break down the structure of the code to understand how different components are related.


2.


AI Contextual Understanding: The AI then steps in to provide contextual insights, which is where the LLM excels. It understands complex code logic, implicit connections, and framework-specific nuances that traditional methods often miss. CodeIQ determines which parts of the application to bring together for context.


3.


Adaptability: CodeIQ adapts to the specific codebase it’s operating in, pulling in just the right amount of context—whether it’s middleware, configurations, or templates—ensuring accurate detection without overwhelming you with irrelevant data.


This combination of project analysis, AI, and ASTs allows CodeIQ to offer both precision and contextual intelligence, going beyond what traditional static analysis can achieve.


### Integration into Developer Workflows


BLAST is designed to integrate seamlessly into existing development workflows, including:


-


**CI/CD Pipelines** : Automated scans at each commit or pull request ensure that new vulnerabilities are identified early in the development process.


-


**IDE Integration** : Developers receive real-time feedback on potential vulnerabilities as they write code, reducing the time spent on security testing.


### Supported Languages and Frameworks


BLAST supports a wide range of languages and frameworks, including:


-


**Languages** : C#, Python, Ruby, Go, JavaScript, TypeScript, Java


-


**Frameworks** : .NET, Django, Ruby on Rails, Node.js, Spring, Flask


This broad support makes BLAST a versatile tool that can be used across a wide variety of application stacks.


## Conclusion


BLAST redefines the landscape of application security testing by combining AI-driven contextual understanding with static analysis. By overcoming the limitations of traditional methods such as **source-sink analysis** , **call-graphs** , and **vector search** , BLAST delivers more accurate vulnerability detection with fewer false positives


————————————————————————-————————————————————————-


Sources:


1.


[https://personal.utdallas.edu/~lxz144130/publications/icst2016.pdf](https://personal.utdallas.edu/~lxz144130/publications/icst2016.pdf)


2.


[https://www.sciencedirect.com/science/article/pii/S0164121222002515](https://www.sciencedirect.com/science/article/pii/S0164121222002515)
