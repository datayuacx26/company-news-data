---
schema_version: "1.0.0"
document_id: "ad69a42720ce74d0412c8cf8f775afef5b7ef80134a5518f35fe2e13edf3b337"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/corgea-vs-snyk-security-benchmark"
published_at: "2026-07-02T20:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:4d641323530c992af854965b2cff1ab3aa4984d33fe45100227cad709e2277ff"
---

# Corgea vs. Snyk: We benchmarked SAST on a deliberately vulnerable repo

We benchmarked Corgea and Snyk against[latiotech/insecure-kubernetes-deployments](https://github.com/latiotech/insecure-kubernetes-deployments) , a deliberately vulnerable repository with application, API, JavaScript, Java, Kubernetes, and configuration weaknesses. We used the same fixed benchmark basis as our[Corgea vs. Aikido comparison](https://corgea.com/blog/corgea-vs-aikido-security-benchmark) so the results are directly comparable.


Corgea performed best overall. Out of 47 source-confirmed issues, Corgea found 42 and Snyk found 26. Corgea reached **89.36% recall** and **85.71% F1** , and it also edged out Snyk on precision at **82.35%** versus **78.79%** . Snyk reached **55.32% recall** and **65.00% F1** .


SAST benchmark


### Corgea found 42 of 47; Snyk found 26


Same deliberately vulnerable repository, same fixed benchmark basis as the Aikido comparison, 47 source-confirmed issues. Corgea found more confirmed vulnerabilities and led on precision, recall, and F1.


Winner by F1 and recall


**Corgea**


1.6x Snyk's recall, 1.3x its F1, 16 more confirmed vulnerabilities found, and higher precision on the same benchmark basis.


47


source-confirmed issues


Confirmed findings 42 vs 26


False negatives 5 vs 21


F1 score 85.71% vs 65.00%


**Recall** Confirmed vulnerabilities caught


Corgea


89.36%


Snyk


55.32%


**F1 score** Balance of precision and recall


Corgea


85.71%


Snyk


65.00%


**Precision** Reported findings that were confirmed


Corgea


82.35%


Snyk


78.79%


Tool Findings reviewed TP FP FN Precision Recall F1


**Corgea** 51 42 9 5 82.35% 89.36% 85.71%


**Snyk** 33 26 7 21 78.79% 55.32% 65.00%


Scoring set: 47 source-confirmed issues reviewed on July 2, 2026. Precision = TP / (TP + FP), recall = TP / (TP + FN), F1 = harmonic mean of precision and recall.


Precision vs. recall


### Corgea led on both precision and recall


Bubble position shows precision and recall. Bubble size shows confirmed true positives. The upper-right corner is the goal: high confidence and broad vulnerability discovery.


**Corgea** **82.35%** precision


**89.36%** recall


**42** true positives


**Snyk** **78.79%** precision


**55.32%** recall


**26** true positives


**Bubble area:** confirmed true positives. The shaded upper-right zone represents 75%+ precision and 75%+ recall.


**Takeaway:** Snyk stayed under 80% precision and missed almost half the confirmed issues; Corgea moved into the high-recall zone while keeping precision above 80%.


Precision and recall use the same July 2, 2026 scoring set of 47 source-confirmed issues.


[See this benchmark on your repo](https://corgea.com/demo?utm_source=corgea.com&utm_medium=cta&utm_campaign=corgea-vs-snyk-security-benchmark&utm_content=mid-benchmark-demo&ref=corgea-vs-snyk-security-benchmark:mid-benchmark-demo&source=corgea-vs-snyk-security-benchmark)


Run the same head-to-head evaluation on a security-sensitive repository from your portfolio and compare confirmed findings, recall, and fix workflow.


[Book a benchmark walkthrough](https://corgea.com/demo?utm_source=corgea.com&utm_medium=cta&utm_campaign=corgea-vs-snyk-security-benchmark&utm_content=mid-benchmark-demo-button&ref=corgea-vs-snyk-security-benchmark:mid-benchmark-demo-button&source=corgea-vs-snyk-security-benchmark)


Snyk found several meaningful issues Corgea missed, but it missed more of the fixed source-confirmed benchmark set. The operational story is coverage: a vulnerability a scanner never reports does not enter the backlog, does not get assigned, and does not get fixed.


## The benchmark setup


We compared the Corgea export` corgea_682cea3f-1429-450e-9449-1f2469ce8778.csv


`


with the Snyk findings provided for this repository, both from July 2, 2026.


Each finding was reviewed against the local repository source and classified as:


Classification Meaning


**True positive** The reported vulnerability, or a directly equivalent weakness, is present in source.


**False positive** The report is unsupported, materially misclassified, duplicated under a second finding class without adding coverage, or points to code where the stated risk is not applicable.


**False negative** A source-confirmed benchmark issue the tool did not report.


The scoring set contained the same **47 source-confirmed application and configuration security issues** used in the clean Corgea vs. Aikido report. To keep the comparison apples-to-apples, Snyk-only supplemental findings were not added to the benchmark denominator. Snyk provided 43 rows: 33 are reflected in the fixed-scope metric table, while 10 source-confirmed Snyk-only rows are discussed below as supplemental observations and are not counted as false positives. Precision, recall, and F1 used standard definitions:


- **Precision:** TP / (TP + FP)
- **Recall:** TP / (TP + FN)
- **F1:** 2 × Precision × Recall / (Precision + Recall)


## Why Corgea came out ahead


Unlike the Aikido run, where the two tools traded a precision edge for a recall gap, Corgea led Snyk on every headline metric here. The decisive factor was still coverage: Corgea found 42 of the 47 confirmed issues; Snyk found 26.


Coverage delta


### Corgea led on precision and covered 16 more confirmed issues


Snyk reported strong taint-flow findings, but its scan left 21 confirmed issues behind. Corgea covered more vulnerability classes across more parts of the repository.


**Corgea** 42 found · 5 missed


42


5


**Snyk** 26 found · 21 missed


26


21


**16** more true positives


**16** fewer false negatives


**1.6x** recall


Examples Corgea found that were absent from the Snyk results:


- Missing authorization on data-modifying FastAPI routes
- XML external entity processing in the Flask app
- Prototype pollution risk around JSON5 parsing
- Sensitive API data exposure without authorization
- Credential exposure in deployment and test configuration


Each stacked bar totals 47 source-confirmed issues. Found percentages: Corgea 89%, Snyk 55%.


Corgea performed better because it covered more vulnerability classes across more of the repository:


- Access control issues in FastAPI routes, including missing authorization on data modification and user/token exposure.
- JavaScript issues beyond SQL injection, including prototype pollution and template code injection.
- XML parser misconfiguration in the Flask application.
- Credential exposure in deployment and test configuration.
- Open redirect, SSRF, command injection, SQL injection, and XSS across multiple services.


Snyk’s strongest areas were taint-flow findings in request handlers, hardcoded credentials, Java deserialization, unsafe upload path construction, disabled TLS certificate verification, and client-side DOM XSS sinks.


## Where both tools agreed


Both tools found important vulnerabilities in the benchmark repo.


For SQL injection in the FastAPI search endpoint, request-controlled input reached a query string directly:


```text
# insecure-api/main.py
sql_query   =   f  "SELECT * FROM video_games WHERE title = '  {  query  }  '"
cursor.execute(sql_query)
```


For OS command injection in the Flask application, request input reached a shell command with` shell=True


`


:


```text
# insecure-app/app.py
cmd   =   request.form[  "command"  ]
process   =   subprocess.Popen(
cmd,   shell  =  True  ,   stdout  =  subprocess.  PIPE  ,   stderr  =  subprocess.  PIPE
)
```


For SSRF, an endpoint fetched a caller-controlled URL server-side without host or scheme validation:


```text
# insecure-api/main.py
@app.get  (  "/fetch_url"  )
def   retrieve_content  (url:   str  ):
response   =   requests.get(url)
return   {  "content"  : response.text}
```


And both identified the open redirect in the FastAPI service:


```text
# insecure-api/main.py
@app.get  (  "/redirect"  )
def   navigate_to  (next:   str  ):
return   RedirectResponse(  url  =  next  )
```


These are real findings, and Snyk deserves credit for catching them.


## What Corgea found that Snyk missed


The meaningful gap was coverage across the rest of the repository.


Corgea identified a data-modifying FastAPI endpoint with no authentication or authorization:


```text
# insecure-api/main.py
@app.put  (  "/games/  {game_id}  "  )
def   modify_game  (game_id:   int  , updated_game: VideoGame):
for   i, game   in   enumerate  (video_games):
if   game.id   ==   game_id:
video_games[i]   =   updated_game
return   {  "message"  :   "Game updated"  }
```


It flagged an unsafe XML parser configuration on request-controlled XML, with DTD loading and entity resolution enabled:


```text
# insecure-app/app.py
xml_data   =   request.form[  "xml"  ]
parser   =   etree.XMLParser(  load_dtd  =  True  ,   resolve_entities  =  True  )
tree   =   etree.fromstring(xml_data.encode(), parser)
```


In JavaScript, Corgea identified prototype pollution risk around user-controlled JSON5 parsing:


```text
// insecure-js/server.js
const   parsedObject   =   JSON5  .  parse  (postData.json5data);
```


Corgea also reported sensitive API data exposure, where a route returns user records without authorization:


```text
# insecure-api/main.py
@app.get  (  "/users"  )
def   list_users  ():
return   users
```


And it found credential exposure in deployment and test configuration, outside the main application source:


```text
# insecure-chart/templates/insecure-app.yaml
-   name  :   AWS_ACCESS_KEY_ID
value  :   "<redacted>"
-   name  :   AWS_SECRET_ACCESS_KEY
value  :   "<redacted>"
```


Those areas were not represented in the Snyk findings provided.


## What Snyk found that Corgea missed


This was not a shutout. Snyk correctly reported several meaningful findings that were missing or less specific in the Corgea results.


It identified request-controlled Java deserialization:


```text
// insecure-java/src/main/java/com/example/insecurejava/UnsafeDeserializationController.java
ObjectInputStream ois   =   new   ObjectInputStream  (  new   ByteArrayInputStream  (data));
Object deserializedObject   =   ois.  readObject  ();
```


It reported disabled TLS certificate verification on outbound requests:


```text
# insecure-app/app.py
response   =   requests.post(url,   headers  =  headers,   data  =  data,   verify  =  False  )
```


It flagged unsafe upload path construction, where a request-controlled filename is joined into a filesystem path before saving:


```text
# insecure-app/app.py
uploaded_file   =   request.files[  "file"  ]
uploaded_file.save(os.path.join(  UPLOADS_DIR  , uploaded_file.filename))
```


It identified client-side DOM XSS sinks, with server-returned content assigned directly into` innerHTML


`


:


```text
// insecure-ai/templates/index.html
if   (data.status   ===   "error"  ) {
resultContent.innerHTML   =   `<div class="text-red-600">Error: ${  data  .  result  }</div>`  ;
}   else   {
resultContent.innerHTML   =   marked.  parse  (data.result);
}
```


It found user-facing error detail disclosure, where exception details and stack traces are returned to callers:


```text
# insecure-ai/app.py
error_msg   =   f  "Error processing code:   {str  (e)  }\n{  traceback.format_exc()  }  "
return   jsonify({  "error"  : error_msg}),   500
```


And it found hardcoded seed and database credentials that were not represented in the Corgea export:


```text
// insecure-java/src/main/java/com/example/catapp/config/DataInitializer.java
createUser  (  "admin"  ,   "<redacted>"  )
```


```text
// insecure-js/server.js
const   connection   =   mysql.  createConnection  ({
host:   "localhost"  ,
user:   "root"  ,
password:   "<redacted>"  ,
});
```


These are real misses for Corgea. The difference is scale: Snyk missed 21 confirmed issues; Corgea missed 5.


## The false positive profile


Snyk had seven false positives in this review. The main patterns were:


- An XSS finding on the Java deserialization response, where the response is a string body and the primary supported issue is unsafe deserialization, not HTML rendering.
- A finding for binding to` 0.0.0.0


`


; in a containerized service this is often required for routing and is not, by itself, a vulnerability.
- A resource-allocation finding on the generic HTTP server entry point that did not point to a clearly exploitable unbounded expensive operation.
- Three lower-severity hardcoded-credential findings that duplicated medium-severity findings on the same Java seed credentials.
- A CSRF finding on a REST endpoint without evidence of stateful browser authentication, which makes the stated CSRF risk unsupported.


Corgea’s nine false positives were mostly imprecise classification, duplicate framing, or weak line pointers:


- A regular-expression complexity finding pointed at a sample value in an HTML form, with no matching execution path.
- A protection-mechanism finding pointed at` X-XSS-Protection: 0


`


; this legacy browser header is deprecated and is not a strong standalone vulnerability.
- Authentication-bypass classifications that duplicated SSRF or open-redirect issues under the wrong class.
- A referer-based authentication finding pointed at an SSRF endpoint that did not use the` Referer


`


header for authentication.
- A CSRF finding pointed near a status route rather than a state-changing action.
- A source-code sensitive-information finding pointed at a static user-facing note string, not a secret.
- A deserialization finding pointed at a standalone helper file rather than the request-handling controller.


## Recommendation


For this benchmark, Corgea is the better fit when the goal is broad vulnerability discovery and remediation coverage. It found more confirmed issues and produced the strongest F1 score on the same benchmark basis used in the[Aikido comparison](https://corgea.com/blog/corgea-vs-aikido-security-benchmark) .


Snyk is still valuable as a complementary signal. It found several important issues Corgea missed, especially Java deserialization, disabled TLS verification, client-side DOM XSS, unsafe upload path construction, and hardcoded application and database credentials.


The limitation of relying on traditional taint-flow SAST as the primary layer is coverage: it is good at recognizable source-to-sink patterns, but weaker when a vulnerability depends on application context, framework conventions, authorization boundaries, or multi-file reasoning.[Corgea AI SAST](https://corgea.com/products/ai-sast) combines static analysis with code context, reachability, and AI-native reasoning. The architecture is covered in the[BLAST AI-powered SAST whitepaper](https://corgea.com/blog/whitepaper-blast-ai-powered-sast-scanner) , and the broader shift from rules to AI-native analysis is explained in[The Three Waves of SAST](https://corgea.com/blog/the-three-waves-of-sast-from-rules-to-ai-native-analysis) . If you are building an evaluation rubric, pair this benchmark with[how to evaluate AI-native SAST tools](https://corgea.com/learn/how-to-evaluate-ai-native-sast-tools) and[how to reduce false positives in SAST](https://corgea.com/learn/how-to-reduce-false-positives-in-sast) .


On this repository, **Corgea won** , based on higher recall, higher precision, and the highest F1 score.


[See this benchmark on your repo](https://corgea.com/demo?utm_source=corgea.com&utm_medium=cta&utm_campaign=corgea-vs-snyk-security-benchmark&utm_content=closing-benchmark-demo&ref=corgea-vs-snyk-security-benchmark:closing-benchmark-demo&source=corgea-vs-snyk-security-benchmark)


Run a pilot on your own code and measure confirmed findings, missed issues, fix quality, and total cost.


[Book a Corgea demo](https://corgea.com/demo?utm_source=corgea.com&utm_medium=cta&utm_campaign=corgea-vs-snyk-security-benchmark&utm_content=closing-benchmark-demo-button&ref=corgea-vs-snyk-security-benchmark:closing-benchmark-demo-button&source=corgea-vs-snyk-security-benchmark)[Read the Snyk alternatives guide](https://corgea.com/learn/snyk-alternatives)


For a broader vendor comparison, see[best Snyk alternatives](https://corgea.com/learn/snyk-alternatives) and the[Corgea vs Snyk comparison page](https://corgea.com/compare/snyk-alternative) .


*Corgea is not affiliated with Snyk. This benchmark reflects the repository, export, and findings reviewed on July 2, 2026.*
