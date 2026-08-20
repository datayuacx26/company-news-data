---
schema_version: "1.0.0"
document_id: "f37f9c9b71df627855fd5cf36583e23f389b05c1d7dcab273fcec805c16deb2a"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:72d0b9203cd6651ff2d46236d404c9a6064d38d1fe17c914ea85805adf99488f"
---

# From vulnerability report to CVE draft in minutes: how Elastic automated security advisories with AI

23 June 2026 •


[Paul McCann](https://www.elastic.co/security-labs/author/paul-mccann)


# From vulnerability report to CVE draft in minutes: how Elastic automated security advisories with AI


How Elastic's security team built an AI agent with RAG against MITRE's CWE and CAPEC catalogues to draft CVE advisories from raw vulnerability reports, including the full prompt and crawler configs.


11 min read


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai) ,


[Enablement](https://www.elastic.co/security-labs/category/enablement)


Elastic's InfoSec Product Security Team built a generative AI agent using Elastic Agent Builder that drafts complete CVE security advisories (CWE classification, CAPEC methodology, CVSS scoring, and mitigation guidance) directly from raw vulnerability reports. The agent uses RAG against the MITRE CWE and CAPEC catalogues indexed in Elasticsearch, which grounds its output in authoritative data and prevents hallucinated classification IDs. ESA-2026-01 is already in production as an example of output that went through this pipeline. Here's how we built it.


##


How security advisories are drafted manually (and why it's slow)


At Elastic, we manage the lifecycle of product vulnerabilities using the[PSIRT Service Framework](https://www.first.org/standards/frameworks/psirts/psirt_services_framework_v1.1) , which defines four stages: discovery, triage, remediation, and disclosure. Each security advisory starts from a vulnerability report received during the discovery phase, and those reports vary widely in quality — translating them into something customers can consume is time-consuming. We draft the security advisory during the disclosure phase, ahead of a planned product release that contains the fix. The advisory is then published as an[Elastic Security Advisory (ESA)](https://www.elastic.co/product-security) , with an assigned CVE ID, in the[Elastic Security Announcements](https://discuss.elastic.co/c/announcements/security-announcements/31) forum, where anyone can review the disclosed vulnerabilities and the associated mitigations.


Each disclosure also gets published into the[CVE Program](https://www.cve.org/) , from which downstream national and regional databases ingest it automatically, including the US[National Vulnerability Database](https://nvd.nist.gov/) (NIST), the EU's[European Vulnerability Database](https://euvd.enisa.europa.eu/) (ENISA), and Japan's[Japan Vulnerability Notes](https://jvn.jp/en/) (JPCERT/CC).


To keep our output consistent, we follow the standard Common Vulnerabilities and Exposures (CVE) description template:


```text
[PROBLEMTYPE] in [COMPONENT] in [VENDOR] [PRODUCT] [VERSION] on [PLATFORMS] allows [ATTACKER] to [IMPACT] via [VECTOR]
```


The PROBLEMTYPE is identified using a[Common Weakness Enumeration](https://cwe.mitre.org/) (CWE) entry, and the Vector is described using a[Common Attack Pattern Enumeration and Classification](https://capec.mitre.org/) (CAPEC) entry.


Substituting the correct CWE and CAPEC for each vulnerability, the template becomes:


```text
[Common Weakness Enumeration] in [COMPONENT] in [VENDOR] [PRODUCT] [VERSION] on [PLATFORMS] allows [ATTACKER] to [IMPACT] via [Common Attack Pattern Enumeration and Classification]
```


The bulk of the manual effort goes into distilling a long, often technically dense vulnerability report into a concise, accurate advisory with a clear impact assessment for customers. Identifying the correct CWE and CAPEC classifications on top of that makes the process convoluted and drawn-out. This is where automation has the most to offer.


##


Automating security advisory drafts with Elastic Agent Builder and RAG


To streamline this process, our InfoSec Product Security Team developed a solution that uses an LLM to automatically generate the standardized sentence for security advisories. This solution involves two key steps:


1.


**Ingesting vulnerability categorization data:** Hallucination is a well-documented failure mode for LLMs operating without authoritative grounding. The[OWASP Top 10 for LLM Applications](https://genai.owasp.org/llmrisk/llm092025-misinformation/) (LLM09) lists it as a top risk category, and it was the original motivation for Retrieval-Augmented Generation. We saw it directly in our early experiments: when asked to assign CWE and CAPEC IDs unaided, the model frequently produced plausible-looking but non-existent entries. To prevent this, we used the[Elastic Crawler](https://github.com/elastic/crawler) to scrape the CWE and CAPEC websites and ingest the data into two Elasticsearch indices:` web-crawl-mitre-cwe-software` and` web-crawl-mitre-capec-software` .


2.


**Building the generative AI agent:** We used the[Elastic Agent Builder](https://www.elastic.co/guide/en/observability/current/agent-builder-overview.html) to create a custom agent that uses an LLM to generate the advisory text based on the ingested data.


##


Step 1: Indexing MITRE CWE and CAPEC data in Elasticsearch


The first step was to get the CWE and CAPEC data into Elasticsearch. For this, we spun up an[Elastic Serverless](https://www.elastic.co/cloud/serverless) instance, noted the[connection details](https://www.elastic.co/docs/solutions/elasticsearch-solution-project/search-connection-details) , and generated an API key. We then created the configs for[Elastic Crawler](https://github.com/elastic/crawler) to visit the MITRE websites and extract the relevant information. The same crawler configurations are run on a continuous schedule, so the indices stay current as MITRE publishes new CWE and CAPEC entries. That keeps the agent's grounded data fresh without manual intervention.


###


CWE crawler configuration


Here is the configuration for CWE data used to ground our AI agent in authoritative vulnerability data. The crawler seeds from CWE's software weaknesses view, follows links matching the` /data/definitions/` pattern, and extracts structured fields from each weakness page, including descriptions, mitigations, consequences, and observed examples ready for semantic search or RAG pipelines.


```text
# CWE Crawler Configuration (crawl-config-mitre-cwe.yml)


output_sink: elasticsearch
output_index: web-crawl-mitre-cwe-software


elasticsearch:
host: "YOUR_ELASTIC_URL"
port: 443
api_key: "YOUR_API_KEY"
pipeline_enabled: false


domains:
- url: https://cwe.mitre.org
seed_urls:
- https://cwe.mitre.org/data/definitions/699.html


extraction_rulesets:
- url_filters:
- type: "regex"
pattern: "/data/definitions/[0-9]+\\.html"
rules:
# 1. Capture the Full URL in a custom field
- action: "extract"
field_name: "cwe_source_url"
selector: ".*" # Match everything in the URL
join_as: "string"
source: "url"


# 2. Extract Just the ID Number from the URL (e.g., 79)
- action: "extract"
field_name: "cwe_id"
selector: "definitions/([0-9]+)" # Capturing group isolates the digits
join_as: "string"
source: "url"


# 3. Full Title
- action: "extract"
field_name: "cwe_full_title"
selector: "h2"
join_as: "string"
source: "html"


# 4. Description Section
- action: "extract"
field_name: "description"
selector: "#Description .detail"
join_as: "string"
source: "html"


# 5. Extended Description
- action: "extract"
field_name: "extended_description"
selector: "#Extended_Description .detail"
join_as: "string"
source: "html"


# 6. Alternate Terms (Array)
- action: "extract"
field_name: "alternate_terms"
selector: "#Alternate_Terms .detail tr"
join_as: "array"
source: "html"


# 7. Common Consequences (Array)
- action: "extract"
field_name: "common_consequences"
selector: "#Common_Consequences .detail tr"
join_as: "array"
source: "html"


# 8. Potential Mitigations (Array)
- action: "extract"
field_name: "potential_mitigations"
selector: "#Potential_Mitigations .detail tr"
join_as: "array"
source: "html"


# 9. Background Details
- action: "extract"
field_name: "background_details"
selector: "#Background_Details .detail"
join_as: "string"
source: "html"


# 10. Modes of Introduction (Array)
- action: "extract"
field_name: "modes_of_introduction"
selector: "#Modes_Of_Introduction .detail tr"
join_as: "array"
source: "html"


# 11. Applicable Platforms (Array)
- action: "extract"
field_name: "applicable_platforms"
selector: "#Applicable_Platforms .detail tr"
join_as: "array"
source: "html"


# 12. Likelihood of Exploit
- action: "extract"
field_name: "likelihood_of_exploit"
selector: "#Likelihood_Of_Exploit .detail"
join_as: "string"
source: "html"


# 13. Demonstrative Examples
- action: "extract"
field_name: "demonstrative_examples"
selector: "#Demonstrative_Examples .detail"
join_as: "string"
source: "html"


# 14. Observed Examples (Array)
- action: "extract"
field_name: "observed_examples"
selector: "#Observed_Examples .detail tr"
join_as: "array"
source: "html"


# 15. Taxonomy Mappings (Array)
- action: "extract"
field_name: "taxonomy_mappings"
selector: "#Taxonomy_Mappings .detail tr"
join_as: "array"
source: "html"


# 16. Related Attack Patterns (Array)
- action: "extract"
field_name: "related_attack_patterns"
selector: "#Related_Attack_Patterns .detail tr"
join_as: "array"
source: "html"


# 17. References (Array)
- action: "extract"
field_name: "references"
selector: "#References .detail tr"
join_as: "array"
source: "html"


crawl_rules:
- policy: allow
type: begins
pattern: "/data/definitions/"
- policy: deny
type: contains
pattern: "/history"
- policy: deny
type: regex
pattern: .*
```


###


CAPEC crawler configuration


We apply the same approach for the MITRE CAPEC catalog, seeding from the Software attack patterns view and extracting structured fields from each pattern page, including attack descriptions, execution flow, prerequisites, required skills, and mitigations, all indexed into Elasticsearch alongside the CWE data.


```text
# CAPEC Crawler Configuration (crawl-config-mitre-capec-software.yml)
output_sink: elasticsearch
output_index: web-crawl-mitre-capec-software


elasticsearch:
host: "YOUR_ELASTIC_URL"
port: 443
api_key: "YOUR_API_KEY"
pipeline_enabled: false


domains:
- url: https://capec.mitre.org
seed_urls:
- https://capec.mitre.org/data/definitions/513.html # The "Software" Category View


extraction_rulesets:
- url_filters:
- type: "regex"
pattern: "/data/definitions/[0-9]+\\.html"
rules:
# 1. Capture the Full Source URL
- action: "extract"
field_name: "capec_source_url"
selector: ".*"
join_as: "string"
source: "url"


# 2. CAPEC ID (isolates just the number from the URL, e.g., 63)
- action: "extract"
field_name: "capec_id"
selector: "definitions/([0-9]+)"
join_as: "string"
source: "url"


# 3. Full Title (e.g., CAPEC-63: Cross-Site Scripting)
- action: "extract"
field_name: "capec_full_title"
selector: "h2"
join_as: "string"
source: "html"


# 4. Description Section
- action: "extract"
field_name: "description"
selector: "#Description .indent"
join_as: "string"
source: "html"


# 5. Likelihood Of Attack
- action: "extract"
field_name: "likelihood_of_attack"
selector: "#Likelihood_Of_Attack .detail"
join_as: "string"
source: "html"


# 6. Typical Severity
- action: "extract"
field_name: "typical_severity"
selector: "#Typical_Severity .detail"
join_as: "string"
source: "html"


# 7. Relationships (Array of table rows)
- action: "extract"
field_name: "relationships"
selector: "#Relationships .tabledetail tr"
join_as: "array"
source: "html"


# 8. Execution Flow
- action: "extract"
field_name: "execution_flow"
selector: "#Execution_Flow .detail"
join_as: "string"
source: "html"


# 9. Prerequisites
- action: "extract"
field_name: "prerequisites"
selector: "#Prerequisites .detail tr"
join_as: "array"
source: "html"


# 10. Skills Required
- action: "extract"
field_name: "skills_required"
selector: "#Skills_Required .detail tr"
join_as: "array"
source: "html"


# 11. Resources Required
- action: "extract"
field_name: "resources_required"
selector: "#Resources_Required .detail tr"
join_as: "array"
source: "html"


# 12. Mitigations
- action: "extract"
field_name: "mitigations"
selector: "#Mitigations .detail tr"
join_as: "array"
source: "html"


# 13. Example Instances
- action: "extract"
field_name: "example_instances"
selector: "#Example_Instances .detail"
join_as: "string"
source: "html"


# 14. Related Weaknesses (CWE Mappings)
- action: "extract"
field_name: "related_weaknesses"
selector: "#Related_Weaknesses .tabledetail tr"
join_as: "array"
source: "html"


# 15. Taxonomy Mappings
- action: "extract"
field_name: "taxonomy_mappings"
selector: "#Taxonomy_Mappings .tabledetail tr"
join_as: "array"
source: "html"


# 16. References
- action: "extract"
field_name: "references"
selector: "#References .detail tr"
join_as: "array"
source: "html"


crawl_rules:
# Allow the seed page and any pattern definition
- policy: allow
type: begins
pattern: "/data/definitions/"


# Deny navigational noise
- policy: deny
type: contains
pattern: "/history"
- policy: deny
type: regex
pattern: .*
```


With the crawler configurations set up, we then ran the following` docker run` commands to initiate two containers for running the data crawling process on each catalog:


This command launches the CWE crawler.


```text
docker run --rm \
-v "$(pwd)":/config \
-it docker.elastic.co/integrations/crawler:latest jruby \
bin/crawler crawl /config/crawl-config-mitre-cwe.yml
```


This command launches the CAPEC crawler.


```text
docker run --rm \
-v "$(pwd)":/config \
-it docker.elastic.co/integrations/crawler:latest jruby \
bin/crawler crawl /config/crawl-config-mitre-capec-software.yml
```


Now that both containers have successfully run, we can see they have crawled the webpages and indexed both CWE and CAPEC data (example output below). We're ready to proceed to the next step.


```text
---- Crawl Stats ----
- Pages visited: 575
- URLs allowed: 574
- URLs denied
- Already seen: 2817
- Domain filter: 9936
- Crawl duration (seconds): 146
- Crawling time (seconds): 106.245
- Average response time (seconds): 0.18477391304347826


---- Elasticsearch Ingestion Stats ----
- Completed
- Documents upserted: 574
- Volume (bytes): 9626811
- Failed
- Number of documents that failed to index: 0
- Volume (bytes): 0
```


##


Step 2: Building the security advisory AI agent


With the CWE and CAPEC catalogues indexed in Elasticsearch, the next step was to build an agent that could draw on them to draft the security advisory text — CWE for the root cause, CAPEC for the attack methodology. We used the[Elastic Agent Builder](https://www.elastic.co/guide/en/observability/current/agent-builder-overview.html) to create a custom agent using Claude Opus, which consistently produced accurate security advisory text and template adherence.


###


What tools Elastic Agent Builder agent uses


####


Grounding in CWE and CAPEC data


Three tools form the core of the RAG loop, letting the agent find and retrieve authoritative classification data from our indexed catalogues:


-


` platform.core.search` — Elasticsearch full-text and structured search. The primary lookup when the agent is searching for candidate CWE or CAPEC entries that match a given vulnerability.


-


` platform.core.get_document_by_id` — retrieves a full document by index and ID. Once` search` has narrowed candidates, this pulls the complete CWE or CAPEC record so the agent reasons against every structured field — description, mitigations, observed examples, related patterns — and not just a search snippet.


-


` platform.core.execute_esql` — executes an ES|QL query and returns tabular results. Used when the agent needs more precise filtering than full-text search can deliver.


####


Index and schema discovery


Two index and schema discovery tools let the agent figure out what data is available rather than relying on hard-coded names in the prompt:


-


` platform.core.list_indices` — lists the indices, aliases, and data streams the current user can access. Useful when the agent needs to confirm what indices exist before constructing a query.


-


` platform.core.get_index_mapping` — retrieves mappings for a specific index. Lets the agent see the available fields before writing a query against them.


-


` platform.core.index_explorer` — natural-language index discovery. The agent can ask "which index holds the CWE catalogue?" and get back a ranked list with mappings, without that being baked into the prompt.


####


Product-specific context


When the agent populates the *Affected Configurations* and *Solutions and Mitigations* sections of the advisory, it needs to verify feature defaults and deployment-specific behaviour against authoritative sources rather than guessing:


-


` platform.core.product_documentation` — searches Elastic product documentation across the stack.


-


` code.search_kibana_code` ,` code.search_kibana_documentation` , and` code.fetch_kibana_documentation` — Kibana-specific source and documentation access, exercised when the advisory involves Kibana. These give the agent access to the code itself, not just the published docs, which matters for confirming subtle behaviours that the documentation doesn't always spell out.


####


Fallback retrieval capabilities


- ` documentation.tavily_extract` — a defensive backstop that fetches the canonical MITRE page directly. With continuous crawling in place, the indexed catalogues stay current, so this rarely fires; it's there to ensure the agent isn't blocked.


The tools aren't called in a free-form order. The prompt instructs the agent to exhaust the indexed catalogue first —` platform.core.search` to find candidates, then` platform.core.get_document_by_id` to retrieve the full record, before falling back to the external retrieval tool. That ordering matters: it prevents the agent from silently substituting unverified external content for data we've explicitly grounded against.


###


How we tuned the system prompt for accurate advisory generation


The prompt itself is where most of the iteration went. Several behaviours we built in are worth calling out, because each came from something we saw the agent do that we didn't want repeated:


-


**Memory-safety verification.** Early in testing, the agent suggested memory-corruption CWEs (e.g., CWE-119) for vulnerabilities in our Go-based Beats, which don't apply in a memory-safe language. The prompt was tuned so that it now detects the affected component's language and forbids memory-corruption CWEs and CAPECs whenever the language is Go, Rust, Java, TypeScript, or another memory-safe runtime.


-


**Minimum disclosure checklist.** Advisories should describe vulnerabilities without producing a proof-of-concept. A checklist in the prompt scans the draft for function names, file paths, endpoint paths, parameter names, port numbers, and similar implementation details, replacing them with abstract equivalents ("a specific internal component," "a user-supplied input field") before the draft is finalised.


-


**CAPEC is methodology, not consequence.** CAPEC can be wrongly chosen as an impact ("Denial of Service") rather than attack technique ("Resource Exhaustion"). The prompt explicitly forbids that anti-pattern and tells the agent to omit the CAPEC entirely if no entry accurately describes the methodology — accuracy over completeness.


-


**"Never ask, always produce."** The agent is instructed to draft a complete advisory from whatever input it receives, using its own judgment for fields the input doesn't cover, rather than coming back to the operator with clarifying questions. The operator always gets a full draft to review.


-


**CVSS scoring guardrails.** Some scoring patterns don't translate well across products, such as log shippers shouldn't be scored` Attack Vector: Network` unless internet-facing exploitation is explicitly demonstrated. Privilege levels also map directly to Elastic's built-in roles in the prompt: any authenticated role such as` viewer` or` editor` → Privileges Required: Low; admin-level roles such as` superuser` ,` kibana_admin` , or` ingest_admin` → Privileges Required: High.


-


**Serverless.** Elastic Cloud Serverless patches continuously, so advisories for products with a Serverless offering carry a specific block confirming the vulnerability was already remediated there before public disclosure.


###


Handling first-party and third-party vulnerabilities


Not every vulnerability is a first-party Elastic bug. Some are issues in third-party dependencies — language runtimes, libraries, transitive packages. The prompt handles both cases with different templates: a first-party path that maps to a CWE + CAPEC pair, and a dependency path keyed off CWE-1395 ( *Dependency on Vulnerable Third-Party Component* ) that links to the upstream CVE(s) and dependency name. The dependency path also gives the agent access to` documentation.tavily_search` to pull upstream advisory context, while first-party vulnerabilities stay grounded only in our indexed CWE/CAPEC catalogues.


###


What the agent outputs: draft advisory and reasoning


The agent's response is always two parts: the draft security advisory, and a separate Reasoning section. The Reasoning forces the agent to justify each choice — which CWE was selected and why, which CAPEC was selected (or why none applied), what privileges are required to exploit the issue, and a one-sentence justification for every CVSS metric. The Disclosure Checklist that strips implementation details from the public-facing security advisory deliberately does *not* apply to the Reasoning section, so the reviewer sees the agent's full thinking, not the abstracted version. That gives the reviewer what they need: they read the Reasoning, decide whether the analysis is sound, then make the call on the advisory text.


##


Elastic Security Advisory Generator


We built the agent in Agent Builder and named it the **Elastic Security Advisory Generator** , a custom agent with the tools and prompt described above. The screenshot below shows it configured in the Agent Builder UI, with the model, wired-up tools, and system prompt all in place:


The excerpt below is a publish-safe version of the prompt that drives the agent. We've omitted the full production prompt, which contains implementation-specific instructions and internal operational detail not needed to understand the design. To review it, see this[public GitHub repository](https://github.com/elastic/elasticsearch-labs/blob/main/supporting-blog-content/security-labs/security-advisory-automation-rag-elastic-agent-builder/agent-creation-prompt.md) .


```text
## ROLE & OBJECTIVE


You are the Elastic Security Advisory Generator. Your task is to classify Elastic-specific vulnerabilities against approved industry taxonomies and draft a consistent public-facing security advisory.


Your goal is to produce a concise advisory that is accurate, reviewable, and grounded in authoritative source material, while minimizing unnecessary disclosure of exploit-enabling implementation detail.


---


## CORE BEHAVIOR


Produce a complete first draft from the information available in the vulnerability report.


- Use user-provided facts when they are present.
- Use approved internal reference data and product documentation to fill in missing context.
- Use best-effort judgment for non-critical narrative fields that are not explicitly provided.
- Leave placeholders only for identifiers that may genuinely be unavailable at draft time, such as the advisory number, CVE number, or final fixed version.


---


## INTAKE


Extract as many of the following fields as possible from the report:


1. Product name
2. Advisory identifier
3. CVE identifier
4. Fixed version or release
5. Affected versions
6. Deployment or configuration context


Include a Serverless remediation note only when the affected product has a Serverless offering or when applicability has been separately confirmed.


---


## GROUNDING AND SAFETY RULES


1. Use authoritative taxonomy data and product documentation as the primary sources of truth.
2. Prefer grounded retrieval over model inference when choosing weakness and attack-pattern classifications.
3. Keep the public advisory focused on what the issue is, who is affected, and how customers should respond.
4. Remove or generalize details that would make exploitation easier, including specific internal component names, file paths, endpoint paths, parameter names, port numbers, stack traces, and infrastructure identifiers.
5. For dependency vulnerabilities, include upstream dependency and CVE context only when that information is necessary to explain exposure.


---


## CLASSIFICATION GUARDRAILS


Before selecting taxonomy entries, identify the likely implementation language of the affected component.


- If the component is implemented in a memory-safe language, avoid memory-corruption classifications unless the report clearly indicates native-code involvement or low-level memory-corruption behavior.
- Select weakness classifications based on root cause.
- Select attack-pattern classifications based on methodology, not impact.
- If no attack-pattern entry accurately describes the method, omit it rather than forcing a weak match.


---


## MITIGATIONS AND SEVERITY


- Confirm affected configurations, deployment defaults, and workaround viability against product documentation before stating them.
- Distinguish between self-managed and hosted or managed deployment guidance when the mitigations differ.
- Produce only a draft CVSS assessment and justify each metric from the report details rather than from the vulnerability label alone.


---


## OUTPUT FORMAT


Return two clearly separated sections:


1. The Advisory
- Subject line
- One-line summary
- Affected versions
- Affected configurations
- Solutions and mitigations
- Indicators of compromise, when applicable
- Serverless note, when applicable
- Severity, CVE, problem type, and impact


2. Reasoning
- Language assessment and safety guardrails applied
- Rationale for selected taxonomy entries
- Privilege assumptions
- Draft CVSS metric reasoning


The public-facing advisory should stay high level. The Reasoning section may retain the additional context needed for internal validation.
```


##


The result: faster, consistent CVE advisory drafts


To use the agent, we take a security report (typically from our bug bounty programme) and paste the content into the Agent Builder conversation window. That content is usually free-form: a vulnerability description, the affected component and version, reproduction steps, and the researcher's view of impact.


The agent performs RAG against the CWE and CAPEC indices, applies the rules and guardrails in the prompt, and produces the two-part output described above: a draft security advisory, and a Reasoning section explaining its choices.


Before the draft is put forward for publication, the Product Security reviewer works through a short validation pass:


-


**Confirm CWE and CAPEC selections against MITRE.** The Reasoning section names the entries chosen and why. The reviewer verifies that each ID matches the official MITRE entry and that the selection lines up with the actual vulnerability.


-


**Sanity-check the CVSS metric reasoning.** The Reasoning lays out a one-sentence justification per metric. The reviewer challenges anything that doesn't follow from the report.


-


**Scan for any over or under sharing.** The disclosure checklist strips implementation details and replaces them with abstract language. The reviewer scans the Advisory for any specifics that may have slipped through, and equally for anything that should be included but has not been. The paragraph should be enough to understand the issue without being a proof-of-concept.


-


**Verify Affected Configurations and Mitigations sections.** The agent reads from product documentation to formulate the "Affected Configurations" and "Mitigations" sections. These sections go to the engineering team that owns the product for verification before the advisory is published — only they have ground truth on feature defaults and whether a stated workaround actually works on the affected releases.


The CVSS score is explicitly labelled draft in the agent's output — the Engineering team responsible for the product and the InfoSec Product Security team sign off on the final score before publication. In practice most drafts need light editing rather than rewriting; the agent gets the structure right, and the reviewer is checking judgment calls and product-specific behaviour.


The example output below is what the agent produced for[ESA-2026-01](https://discuss.elastic.co/t/metricbeat-8-19-10-9-1-10-9-2-4-security-update-esa-2026-01/384519) , an advisory we published for Metricbeat.


##


What's next: closing the loop with Elastic Workflows


Combining generative AI with RAG against authoritative MITRE catalogues has turned what used to be a manual, time-consuming task into a consistent and faster part of our advisory process. The pipeline is already producing drafts that go to production —[ESA-2026-01](https://discuss.elastic.co/t/metricbeat-8-19-10-9-1-10-9-2-4-security-update-esa-2026-01/384519) , shown above, is one example.


The biggest win is the slowest part of the old process: taking a long vulnerability report — often varying in quality and dense with technical detail — and distilling it into a concise, accurate advisory with a clear impact assessment for customers. That distillation, along with the CVE templating, CWE/CAPEC mapping, and CVSS metric reasoning, is now drafted by the agent. Our team's effort goes into the parts that need human judgment: product-specific behaviour and impact scoring.


The next step is closing the loop end-to-end. Today, the agent is invoked manually; an analyst pastes the vulnerability report into the conversation window. We want to wire this into the triage step itself, using something like[Elastic Workflows](https://www.elastic.co/docs/explore-analyze/workflows) : once a vulnerability is confirmed and accepted for disclosure, the workflow invokes the agent automatically and produces a draft advisory. From there, InfoSec and Engineering collaborate on a single document, replacing the manual hand-offs between triage, drafting, and review.


What made this work was the combination of stable, authoritative data to ground the agent against and a strict review step. Both matter. The same pattern can apply to any structured drafting task with a defined output template and a trustworthy data source. To learn more about building your own generative AI solutions with Elastic, check out the[Elastic Agent Builder documentation](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) .


#### Jump to section


- [How security advisories are drafted manually (and why it's slow)](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#how-security-advisories-are-drafted-manually-and-why-its-slow)
- [Automating security advisory drafts with Elastic Agent Builder and RAG](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#automating-security-advisory-drafts-with-elastic-agent-builder-and-rag)
- [Step 1: Indexing MITRE CWE and CAPEC data in Elasticsearch](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#step-1-indexing-mitre-cwe-and-capec-data-in-elasticsearch)
- [CWE crawler configuration](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#cwe-crawler-configuration)
- [CAPEC crawler configuration](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#capec-crawler-configuration)
- [Step 2: Building the security advisory AI agent](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#step-2-building-the-security-advisory-ai-agent)
- [What tools Elastic Agent Builder agent uses](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#what-tools-elastic-agent-builder-agent-uses)
- [Grounding in CWE and CAPEC data](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#grounding-in-cwe-and-capec-data)
- [Index and schema discovery](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#index-and-schema-discovery)
- [Product-specific context](https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder#product-specific-context)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=From%20vulnerability%20report%20to%20CVE%20draft%20in%20minutes:%20how%20Elastic%20automated%20security%20advisories%20with%20AI&url=https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder&title=From%20vulnerability%20report%20to%20CVE%20draft%20in%20minutes:%20how%20Elastic%20automated%20security%20advisories%20with%20AI)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/security-advisory-automation-rag-elastic-agent-builder&title=From%20vulnerability%20report%20to%20CVE%20draft%20in%20minutes:%20how%20Elastic%20automated%20security%20advisories%20with%20AI)
