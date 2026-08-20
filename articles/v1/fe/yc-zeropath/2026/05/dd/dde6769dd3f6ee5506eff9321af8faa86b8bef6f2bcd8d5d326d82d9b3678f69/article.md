---
schema_version: "1.0.0"
document_id: "dde6769dd3f6ee5506eff9321af8faa86b8bef6f2bcd8d5d326d82d9b3678f69"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/nifi-cve-2026-39816-privesc-rce"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:12:44.576485+00:00"
content_hash: "sha256:5c417d93c5a8a16a4dee9fdb9717963c80e1e9546ffd21ba0f8229679e8e4445"
---

# CVE-2026-39816 Allows Privesc And Code Execution In Apache NiFi

## Summary


ZeroPath Research discovered a vulnerability in[Apache NiFi](https://nifi.apache.org/) that allows authenticated users without the EXECUTE_CODE privilege to execute arbitrary code on the NiFi server if administrators have installed the optional nifi-other-graph-services-nar NAR.


The issue was tracked in[NIFI-15800](https://issues.apache.org/jira/browse/NIFI-15800) and patched in version 2.9.0. MITRE has assigned it[CVE-2026-39816](https://www.cve.org/CVERecord?id=CVE-2026-39816) , and scored the severity as "high."


Using our POC to get a shell on a NiFi server.


## Impacted Software


Vulnerable Versions Patched Versions


- >= 2.0.0-M1, < 2.9.0


- >= 2.9.0


## Timeline


- 2026-04-03 — Issue reported to Apache NiFi maintainers
- 2026-04-07 —[NIFI-15800](https://issues.apache.org/jira/browse/NIFI-15800) created to track the issue
- 2026-04-10 — NiFi 2.9.0 ships with a patch


## POC


[Full working POC](https://github.com/ZeroPathAI/nifi-CVE-2026-39816-poc) with Docker setup to produce a vulnerable environment to test against.


## Apache NiFi


### Background


Apache NiFi is an open source dataflow management system that allows users to create, execute and manage data pipelines. It's similar to Airflow or Dagster, but instead of orchestrating batch jobs that other systems execute, NiFi sits in the data path itself, transforming and routing data as it passes from one place to another, typically in real-time continuous flows.


The project has 6.1k stars on Github, and is widely-used across the industry.


6.1k stars as of April 28th, 2026.


Typically, NiFi is not exposed to the public internet, but 302 publicly-accessible instances do show up in Shodan.


302 publicly-accessible NiFi instances as of April 28th, 2026.


Users primarily interact with NiFi via a web UI, where they can create and edit dataflows by drag and dropping components on a canvas.


Screenshot of NiFi canvas from its[user guide](https://nifi.apache.org/docs/nifi-docs/html/user-guide.html) .


### Privilege Model


Given that NiFi exists to author and manage data flows, its users have a fair amount of privilege by default compared to something like a word processor. However, executing arbitrary code is gated behind a specific permission. By design, there are intended to be flow designers who can't run whatever they want on the server.


NiFi does not have pre-defined roles. Instead, user privilege is constructed by assigning one or more granular permissions. The full list (especially the EXECUTE_CODE permission) will become relevant later:


Permission Effect


EXECUTE_CODE Processors that compile/run user-supplied code (ExecuteScript, ExecuteGroovyScript, ...)


READ_FILESYSTEM Reading from the local filesystem (GetFile, FetchFile)


WRITE_FILESYSTEM Writing to the local filesystem (PutFile)


READ_DISTRIBUTED_FILESYSTEM Reading HDFS-style remote stores


WRITE_DISTRIBUTED_FILESYSTEM Writing HDFS-style remote stores


ACCESS_KEYTAB Kerberos keytab access


ACCESS_TICKET_CACHE Kerberos ticket cache access


ACCESS_ENVIRONMENT_CREDENTIALS Reading credentials from env vars / credential providers


EXPORT_NIFI_DETAILS Exporting NiFi internals (e.g. flow definition with secrets)


REFERENCE_REMOTE_RESOURCES Fetching arbitrary URLs (InvokeHTTP, etc.)


## CVE-2026-39816: Running code without EXECUTE_CODE


### Overview


NiFi contains two processors for executing traversals against graph databases: ExecuteGraphQuery and ExecuteGraphQueryRecord. Neither require EXECUTE_CODE to instantiate or run... and for good reason at first glance. Executing a traversal, like executing a SQL query, intuitively feels like an operation that shouldn't involve arbitrary code execution.


However, if the traversal target is a Tinkerpop-compatible service, this common sense assumption fails: under some circumstances the user can specify a query using arbitrary Groovy code, which is then run on the NiFi server.


### NiFi Graph Extensions


NiFi has a large ecosystem of first and third party extensions that add functionality. The core functionality is kept fairly lean by design. Most of these extensions are actually part of the NiFi project, and live in the[main NiFi repo](https://github.com/apache/nifi/tree/main/nifi-extension-bundles) . It's normal to add extensions to support access to particular data sources for example (e.g. Airtable or Dropbox).


If users want to run graph traversals against TinkerPop-compatible graph databases, they must enable a number of first party extensions from the NiFi project.


To be vulnerable to CVE-2026-39816, those must include:


- nifi-graph-nar


- adds the ExecuteGraphQuery and ExecuteGraphQueryRecord processors for making graph queries


- nifi-graph-client-service-api-nar


- adds GraphClientService, the abstraction that different graph db clients implement


- nifi-other-graph-services


- Adds TinkerpopClientService, the specific implementation of GraphClientService for TinkerPop-compatible graph dbs


### TinkerPop Query Conventions


ExecuteGraphQuery and ExecuteGraphQueryRecord both allow the user to specify a graph client, and a query to run. TinkerpopClientService is one possible graph client.


TinkerpopClientService is interesting though, because TinkerPop has an unusual approach to traversals: while users can specify them as strings, the preferred technique is to implement the query directly in the language you're using with[Gremlin Language Variants](https://tinkerpop.apache.org/docs/3.8.0/reference/#gremlin-drivers-variants) like this:


```text
Cluster   cluster   =     Cluster  .  open  (  "conf/remote.yaml"  )  ;
GraphTraversalSource   g   =     AnonymousTraversalSource
.  traversal  (  )
.  with  (  DriverRemoteConnection  .  using  (  cluster  )  )  ;


g                                    // GraphTraversalSource - the entry point
.  V  (  )                                // GraphTraversal<Vertex,Vertex> (start step)
.  has  (  'name'  ,     'alice'  )               // GraphTraversal<Vertex,Vertex> (filter step)
.  out  (  'knows'  )                       // GraphTraversal<Vertex,Vertex> (nav step)
.  toList  (  )                           // terminal: triggers execution, returns List
```


Here the traversal is the bit after "g," but each step of the path is expressed using a native Groovy method. Per[the TinkerPop documentation](https://tinkerpop.apache.org/docs/3.8.0/reference/#gremlin-drivers-variants) , this approach is preferred for a number of reasons, including:


- Queries are compile-time verified
- Queries can be composed just like any other functions
- Development-time linting can flag errors and so on


### The Flaw: TinkerPop's Unorthodox Query Approach Breaks Assumptions


TinkerpopClientService, NiFi's abstraction for executing TinkerPop traversals against a target, supports both the string-based expression of traversals AND the preferred native language approach.


String-based submission is the default, but the service can be instantiated with native language submission like this:


```text
"properties"  :     {
"Script Submission Type"  :     "bytecode-submission"  ,
"Settings Specification"  :      "service-settings"  ,
"Contact Points"  :            args.gremlin_host  ,
"Port"  :                      args.gremlin_port  ,
"Path"  :                        "/gremlin"
}
```


It's important to note here, that "bytecode" does not refer to JVM-bytecode... TinkerPop has its own bytecode that traversals in supported languages get compiled down to. It is much less full featured than something like JVM bytecode... potentially abusable because it supports arbitrary lambdas, but getting a shell is a little bit harder than instantiating ProcessBuilder.


However, if we look at how TinkerPopClient actually handles these bytecode submissions, something interesting jumps out:


```text
protected     Map  <  String  ,     String  >     bytecodeSubmission  (
String   s  ,     Map  <  String  ,     Object  >   map  ,     GraphQueryResultCallback   graphQueryResultCallback  )     {
// ...
Script   compiled  ;
// ...


// ZP !!! compile groovy code that will ultimately become TinkerPop bytecode
compiled   =   groovyShell  .  parse  (  s  )  ;
```


` groovyShell` here has the type` groovy.lang.GroovyShell` . The real input to the bytecode submission vector is Groovy code, which is compiled and run on the NiFi server to produce the bytecode to send to TinkerPop.


This makes executing arbitrary code easy. We just need to create an ExecuteGraphQuery processor and set the query to valid Groovy code that returns a HashMap, e.g.:


```text
GROOVY_PAYLOAD   =     (
'def idOut = "id".execute().text.trim()\n'
'def hostnameOut = "hostname".execute().text.trim()\n'
'def proof = "RCE_PROOF\\nid: " + idOut + "\\nhostname: " + hostnameOut\n'
f'new File("  {  MARKER_FILE  }  ").text = proof\n'
'def result = new HashMap()\n'
'result.put("rce_proof", "CONFIRMED")\n'
'result'
)
```


This is only a security issue ultimately because ExecuteGraphQuery and related components can be instantiated by a user without EXECUTE_CODE permissions, potentially because executing graph queries under most circumstances doesn't involve arbitrary code execution. This TinkerPop edge case invalidates some core intuitive security assumptions likely made by the NiFi developers.


## Mitigation


- Upgrade Apache NiFi to 2.9.0 or later
- If upgrade is not possible, ensure the following optional extensions are not installed:


- ` nifi-graph-nar`
- ` nifi-graph-client-service-api-nar`
- ` nifi-other-graph-services`


## Takeaways


There's no buffer overflow here... no SQL query that interpolates unsanitized input, no insecure deserialization. The issue exists purely at the semantic level: the ExecuteGraphQuery and related components were gated by one set of required privileges, but in actual practice supported behavior typically only available under another.


As a result, this is a hard vulnerability to find with a traditional SAST tool in an application that is filled with by-design code execution vectors. LLMs working within a structured scaffold made it much easier to pick out the one code execution path that was surprising given the meaning of the permissions required to access it.
