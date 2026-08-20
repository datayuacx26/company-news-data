---
schema_version: "1.0.0"
document_id: "7041f960d461be4f2bb3ee9152d8b878ed00e0429b350faac6eec412477cd59d"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/how-to-read-an-audit-report"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:8fed3795bdd5efd99fba69f6e5dbf85036a9ca4c2d896e957da6837661fd084f"
---

# How to Read an Audit Report

This post teaches the reader how to read a Quantstamp audit report and interpret the results of an audit.


## Audit Scope: Understanding What We Audited


Before interpreting the results, audit report readers first need to identify the exact code Quantstamp audited. On the first page, the *repositories* and *commit hashes* listed in the *Source Code* *Section* inform the reader about the exact code that was audited by Quantstamp.


*The section highlighted in green lists the repositories and corresponding commit hashes*[pertaining to a specific audit](https://certificate.quantstamp.com/full/teku) *.*


### What is a code repository?


[Crozdesk](https://crozdesk.com/) provides an excellent definition of what a code repository is: “A code repository is where snippets and patches of source code for software programs are archived in an organized way.” When you look at the *Repository Section* of an audit report, it informs you of the repository we audited.


Is this enough to determine the exact code that Quantstamp audited? Not yet. Most repositories are constantly updated and changed by a team of programmers. When a client asks Quantstamp to audit a code repository, we either 1) audit that repository as it existed at a specific moment in time or 2) we audit a diff (more on this later).


When we audit a code repository as it existed at a specific moment in time (aka not a diff audit), audit report readers can determine the exact code Quantstamp audited by analyzing a repository's *commit hash* .


### What is a commit hash?


A repository’s commit hash can be considered a digital fingerprint of the code within that repository. Anytime updates are made to the code in the repository, it receives an entirely new commit hash: an entirely new digital fingerprint.


If the repository’s commit hash changes after Quantstamp conducts an audit, it contains code that is out-of-scope (aka not reviewed by Quantstamp auditors). Audit readers can click on the link in the *Commit Section* of the report to view the exact lines of code audited by Quantstamp.


*A recent commit for the teku repository is circled in green. This repository is updated often.*


### What is a diff audit?


At the moment, approximately 50% of Quantstamp audits are diff audits. When a client asks Quantstamp to conduct a diff audit, they are asking us to only audit differences between two repositories. Each repository has its respective commit hash. Any code outside of the differences between the two repositories is out-of-scope (aka not audited by Quantstamp).


Clients usually request diff audits when they are copying code belonging to deployed smart contracts from other protocols.


## Understanding Audit Findings


When we conduct an audit and discover an issue, Quantstamp logs the issue within the report in the *Findings Section* and labels the issue as *High* , *Medium* , *Low* , *Informational, or Undetermined* .


*An example finding from*[our audit of Teku](https://certificate.quantstamp.com/full/teku) *, an ETH 2.0 client.*


### Issues Can Be Fixed, Mitigated, or Acknowledged


Clients sometimes choose to not fix issues identified by Quantstamp auditors. When a client decides to not fix an issue, they either *Mitigate* or *Acknowledge* the issue.


#### Mitigation


When a client mitigates an issue, this means that the issue is still present in the codebase but the client developed a workaround in an effort to reduce the likelihood that the issue leads to negative consequences.


What does this look like in practice? Mitigations may include modifying a user interface so that it avoids triggering the issue at the smart contract level and requiring users to use that specific interface. Another mitigation may be some other modification in the smart contract code.


#### Acknowledgment


When a client acknowledges an issue, they are acknowledging that the issue remains in the code: the issue remains unresolved. When an issue is *Acknowledged* , the client usually provides an explanation for their acknowledgment in the *Findings Section* , which is included in the *Update* below the *Recommendation* for that specific finding.


*An example of an acknowledged issue along with a comment from the client in the Update Section.*


After an issue is acknowledged, the client is also supposed to address the issue outside of the code itself. For instance, clients may:


1. Insert comments in the code, create additional documentation, and/or disclose it in the README file within the repository OR
2. Provide an analysis showing that the issue will not lead to negative consequences in practice (e.g., gas analysis, deployment settings)


### Finding Severity Levels


#### High Risk


A high risk issue puts a large number of users’ sensitive information at risk, or is reasonably likely to have a catastrophic impact on the client’s reputation or serious financial implications for the client and users.


#### Medium Risk


Medium risk issues put a subset of users’ sensitive information at risk, would be detrimental for the client’s reputation if exploited, or are reasonably likely to lead to moderate financial impact.


#### Low Risk


Low risk issues are relatively small and could not be exploited on a recurring basis, or is a risk that the client believes is unlikely to have a serious impact considering their business circumstances.


#### Informational


The issue does not post an immediate risk but is relevant to security best practices.


#### Undetermined


The impact of the issue is uncertain.


## Test and Documentation Quality


*Test quality can be found on the first page of an audit report.* This image comes from[our audit of Prysm](https://certificate.quantstamp.com/full/prysm) , another ETH 2.0 client.


This section helps the reader understand the quality of the documentation and testing that pertains to the audited repository. Documentation is important because it helps developers and auditors reason about the codebase and develop quality tests. Quality testing helps developers catch bugs early in the development process and should be a standard practice in smart contract development.


To learn more about why quality documentation and testing is one of the best ways for developers to avoid bugs in smart contracts,[click here](https://quantstamp.com/blog/securing-your-defi-project-starts-with-quality-testing) .


## Public Certificates


After our clients complete their Quantstamp audit, they have the option to make the audit report private or public. If they choose to make their report public, it can be viewed on certificate.quantstamp.com.
