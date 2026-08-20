---
schema_version: "1.0.0"
document_id: "8b4c6cfc825438face939b3e0865d2aa26d920d3322aa0c1606951dc0ab5964b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/fail-open"
published_at: "2021-01-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:dd4e56732b1876e6017df80c0c1c30b7e47a317cae84e3f1fd54f73348ae96a6"
---

# Understanding "Failed Open" and "Fail Closed" in Software Engineering

## Understanding "Failed Open" and "Fail Closed"


These concepts are intriguing because they apply not only within code but also in broader system designs, offering insights into error handling and security practices across different layers of software engineering.


### Definitions and Context


- Fail Open: A system that fails open defaults to an operational or open state in the event of a failure. This can be critical in systems where continuous operation is necessary, but it may compromise security if not properly managed.
- Fail Closed: Conversely, a system that fails closed defaults to a closed or secure state. This is often preferred in security-critical applications where preventing unauthorized access is paramount.


### Application in Control Systems


In control systems, the choice between fail open and fail closed is driven by safety considerations. For example, in an automated cooling system, failing open might be safer to prevent overheating, whereas in other systems, failing closed might be necessary to prevent hazardous conditions.


### Security Implications


In security contexts, such as firewall configurations, the fail state can significantly impact network security. A firewall that fails open could expose the network to unauthorized access, while one that fails closed would block all traffic, maintaining security but potentially disrupting service.


### Broader Applicability


These concepts are not limited to software or code but are integral to various layers of system design. Understanding fail open and fail closed is essential for ensuring comprehensive system reliability and security across hardware, network configurations, and software engineering.


## Code Examples: Fail-Open vs. Fail-Closed


Let's take a look at some code to understand the difference between fail-open and fail-closed scenarios.


#### Example A (Fail-Open)


py


1


2


3


4


5


```text
# Example A
if    not   user.is_allowed():
raise   NotAllowedError()
do_more_work_here()
return   response


```


py


1


2


3


4


5


```text
# Example A
if    not   user.is_allowed():
raise   NotAllowedError()
do_more_work_here()
return   response


```


In this example, if` user.is_allowed()` encounters an unexpected issue and doesn't handle all failure scenarios, the code proceeds with` do_more_work_here()` , potentially allowing unauthorized actions. This is because the error handling does not prevent the execution of subsequent code, which is characteristic of a fail-open approach.


#### Example B (Fail-Closed)


py


1


2


3


4


5


```text
# Example B
if   user.is_allowed():
do_more_work_here()
return   response
raise   NotAllowedError()


```


py


1


2


3


4


5


```text
# Example B
if   user.is_allowed():
do_more_work_here()
return   response
raise   NotAllowedError()


```


In this example, unless` user.is_allowed()` explicitly returns True, the code raises` NotAllowedError()` , preventing further execution. This ensures that unauthorized actions are blocked, aligning with the fail-closed approach.


### Implications of each approach


- Fail-Open: This approach can lead to security vulnerabilities if not properly managed, as it allows access or continues operation even in the event of a failure. It is typically used where availability is more critical than security.
- Fail-Closed: This approach prioritizes security by denying access or stopping operation in the event of a failure. It is essential in scenarios where security is paramount, such as in financial transactions or sensitive data handling.


By clearly labeling and explaining each example, developers can better understand the implications of each approach and make informed decisions based on their system's specific needs.


### The Implication of Unhandled Scenarios


Consider the scenario when the` is_allowed()` method doesn't support every failure scenario. What would happen when the unhandled failure scenario occurs? In Example B, execution would continue and I would get an error. However, in Example A, execution would continue and I would ultimately get a response!


In this small example, you might be wondering why anyone would be tempted to write code in the fail-open style, but imagine that you have hundreds of lines of code where` do_more_work_here()` is called. Each failure scenario makes the successful execution path indent one level further:


py


1


2


3


4


5


6


7


```text
if    not   request.is_valid():
work = do_more_work_here()
if   work.was_successful():
user = lookup_user()
if   user.is_allowed():
return   response
raise   NotAllowedError()


```


py


1


2


3


4


5


6


7


```text
if    not   request.is_valid():
work = do_more_work_here()
if   work.was_successful():
user = lookup_user()
if   user.is_allowed():
return   response
raise   NotAllowedError()


```


You can combine checks like so:


py


1


2


3


4


5


6


```text
if   request.is_valid():
work = do_more_work_here()
user = lookup_user()
if   work.was_successful  and   user.is_allowed():
return   response
raise   NotAllowed()


```


py


1


2


3


4


5


6


```text
if   request.is_valid():
work = do_more_work_here()
user = lookup_user()
if   work.was_successful  and   user.is_allowed():
return   response
raise   NotAllowed()


```


However, if functions like` do_more_work_here()` or` lookup_user()` are expensive or contain side-effects, you'd want to run the function only once you filtered all the requests where the work wasn't successful.


Fail-closed code can end up awful to read: sometimes so awful that you might be more likely to write bugs because it's too hard to read. This is why you must decide for yourself where to risk writing something fail-open or fail-closed.


The next time you're left wondering why a senior developer's code is nested deeply in something critical, consider that they were trying to have the code fail-closed.


## Importance of Fail States in Authorization Systems


In authorization systems like those managed by AuthZed's SpiceDB, understanding fail-open and fail-closed behaviors is crucial for maintaining security integrity.


### Fail-Open State


A fail-open state can inadvertently grant access to unauthorized users during unexpected failures, posing significant security risks. This behavior is contrary to security best practices, as it compromises the security of the system by allowing malicious actors to exploit vulnerabilities.


### Fail-Closed State


Conversely, a fail-closed state ensures that, in the event of an error, access is denied, maintaining the integrity of your application's security. Implementing fail-closed logic in your authorization checks helps prevent unauthorized access even when unforeseen issues occur. This practice aligns with security best practices and underscores the importance of robust error handling in software development.


### Security Best Practices


To ensure the security of authorization systems, it is essential to follow best practices such as continuous monitoring and testing of system controls. Regular security audits and ongoing authorization processes help in spotting and mitigating potential security risks. A layered security approach, including multiple security controls like firewalls, intrusion detection systems, encryption, and user authentication mechanisms, further enhances the security of the system.


### Real-World Implications


System failures can have significant financial and reputational consequences. Ensuring that authorization systems default to a fail-closed state during failures is critical in preventing unauthorized access and maintaining the security of the application.


## Conclusion


Understanding the distinction between fail-open and fail-closed behaviors is critical for developing secure and reliable software systems. As discussed, failing closed ensures that applications deny access during unexpected failures, thereby maintaining security integrity. This principle is particularly relevant in scenarios where security is a top priority, such as in financial transactions and sensitive data handling.


At AuthZed, we emphasize the importance of robust authorization solutions like SpiceDB to empower developers in implementing these best practices effectively. By adopting fail-closed principles, developers can significantly enhance the security posture of their applications.


In conclusion, the distinction between fail-open and fail-closed is not just a technical nuance but a fundamental aspect of secure software development. By prioritizing fail-closed behaviors, developers can ensure their applications are more secure and resilient against potential failures.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


Originally published January 16, 2021


On this page


- Understanding "Failed Open" and "Fail Closed"
- Definitions and Context
- Application in Control Systems
- Security Implications
- Broader Applicability
- Code Examples: Fail-Open vs. Fail-Closed
- Example A (Fail-Open)
- Example B (Fail-Closed)
- Implications of each approach
- The Implication of Unhandled Scenarios
- Importance of Fail States in Authorization Systems
- Fail-Open State
- Fail-Closed State
- Security Best Practices
- Real-World Implications
- Conclusion
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
