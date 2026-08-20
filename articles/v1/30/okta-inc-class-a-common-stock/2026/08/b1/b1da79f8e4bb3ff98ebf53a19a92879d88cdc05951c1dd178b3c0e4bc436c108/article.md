---
schema_version: "1.0.0"
document_id: "b1da79f8e4bb3ff98ebf53a19a92879d88cdc05951c1dd178b3c0e4bc436c108"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/threat-intelligence/using-advanced-posture-checks-as-a-tooling-watchdog/"
published_at: "2026-08-11T16:00:00+00:00"
first_seen_at: "2026-08-13T00:40:23.090321+00:00"
fetched_at: "2026-08-13T00:40:27.275055+00:00"
content_hash: "sha256:0dc98eab5d233a9258b506c6631ea5caba69311427ac4cf48e51e6888660e4c3"
---

# Using advanced posture checks as a tooling watchdog

### Topics


---


IAM


,


Malware


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


Adversaries love to make life difficult for defenders. One of the best ways for attackers to operate undetected is to disable security tools on a compromised device.


Sometimes users do the hard work for them. When a frustrated user disables a security service that appears to be getting in the way of getting their work done, it creates the same opportunity for attackers. So do unintentional actions, like a failed update.


Unfortunately, many defenders also fail to check that critical security tools are running, and in doing so create a glaring blind spot. Most organizations rely on periodic compliance sweeps that leave these gaps wide open between audits. Too often an organization only discovers whether security tools were running after a compromise, when it’s too late.


That’s why we’re excited to introduce[advanced posture checks](https://help.okta.com/oie/en-us/content/topics/identity-engine/devices/device-assurance-adv-posture-check.htm) to the Okta Verify client for macOS and Windows. This feature transforms every authentication flow into a persistent tooling watchdog. By running an osquery SQL query at the moment an account tries to authenticate, administrators can verify that mandatory security tools are not just deployed, but are also actively running. If the defenses aren’t there, the device doesn’t get in.


Below, we’ll look at how to leverage this feature - but first let’s start with why it’s important.


## The gap that's hiding in plain sight


Most organizations have a well-defined list of tools every managed endpoint is supposed to carry, from EDR agents to MDM devices and beyond. Though the exact set of tools varies from one organization to another, each category of security tool serves a consistent purpose that is crucial to a well-defended environment.


The problem is that just because you deployed something, doesn’t mean it’s still running. Agents crash and services get disabled - sometimes by users who want to speed up their laptop, and sometimes by malware that knows exactly which process to kill first. A VPN client ships an update that breaks the service, and the team doesn’t find out until you notice a pattern in helpdesk tickets weeks later. None of these show up in regular monitoring unless you're specifically looking for process absence, and most teams aren't.


This problem is amplified by the fact that most compliance checks only verify the presence of the security tool once, even if it falls out of compliance later. Advanced posture checks flips this: the check runs at every authentication event, so a tool that stops running at 9am will result in a user being denied at 9:05am.


## Defense impairment in the wild


Adversaries know that if they can blind your security operations, they’ve already achieved a significant goal. These actions are so prevalent that MITRE ATT&CK has an entire tactic for it called[Defense Impairment](https://attack.mitre.org/tactics/TA0112/) , consisting of 18 different techniques. One of the most prevalent techniques is[T1685: Disable or Modify Tools](https://attack.mitre.org/techniques/T1685/) , which is where our tooling watchdog query below focuses.


While adversaries disable many categories of security tools, the use of EDR killers has become commonplace during ransomware intrusions, with operators frequently disabling EDRs prior to encryption. For example,[Gentlemen](https://www.welivesecurity.com/en/eset-research/killing-me-gently-inside-gentlemens-edr-killer-framework/) ransomware operators rely on their "GentleKiller" malware to disable telemetry processes like osqueryd.exe. Similarly,[Reynolds](https://www.security.com/threat-intelligence/black-basta-ransomware-byovd) ransomware uses a Bring Your Own Vulnerable Driver (BYOVD) technique, weaponizing a vulnerable driver to kill local EDR agent processes across a wide range of platforms. By preventing devices with disabled tools from authenticating to your protected resources (apps and data), you can stop these attackers before they successfully encrypt data.


## The watchdog pattern


The detection query we are introducing with this post — which you can find in the[okta/customer-detections](https://github.com/okta/customer-detections/blob/master/sample_osquery_checks/Cross/security_tooling_watchdog_detection.yml) repository — approaches this as a "digital health check" for a user device. Instead of just looking for a single process, the query acts like a scorecard. The query gives the device a point for every mandatory security tool category that is actively running, and the device “passes” only if its total score hits the target. Every security tool category is assigned its own Common Table Expression (CTE) that yields a 1 if a relevant process is currently active, and a 0 if not. The final result aggregates every category, ensuring the check only allows authentication when every mandatory tool is accounted for.


Here is an example of how a device could be evaluated…


Is telemetry being passed? Yes, so the score is **1**
Is the device running EDR? Yes, so the score is **1**
Is the device running MDM? Yes, so the score is **1**
Is the device running DLP? Yes, so the score is **1**
Is the device running VPN/ZTNA? No, so the score is **0**
Is the device running DNS security? Yes, so the score is **1**
Is the device running vulnerability management? Yes, so the score is **1**


Scorecard: 6 out of 7 failed! Therefore, the device would not be granted access.


There are seven categories, each covering the major vendors in that space across macOS and Windows:


```text
WITH telemetry_running AS (


```


```text
-- Fleet Orbit supervisor + osquery daemon


```


```text
SELECT COALESCE(COUNT(*), 0) AS total


```


```text
FROM processes


```


```text
WHERE name IN ('osqueryd', 'orbit', 'osqueryd.exe', 'orbit.exe')


```


```text
),


```


```text
edr_running AS (


```


```text
-- CrowdStrike Falcon | SentinelOne | VMware Carbon Black Cloud |


```


```text
-- Microsoft Defender for Endpoint | Elastic Security | Palo Alto Cortex XDR |


```


```text
-- FireEye/Trellix | Sophos | Trend Micro | ESET


```


```text
SELECT COALESCE(COUNT(*), 0) AS total


```


```text
FROM processes


```


```text
WHERE name IN (


```


```text
'com.crowdstrike.falcon.Agent', 'CSFalconService.exe', 'falcon-sensor',


```


```text
'SentinelAgent', 'SentinelAgent.exe', 'sentineld',


```


```text
'cbagentd', 'CbDefense.exe',


```


```text
'wdavdaemon', 'MsMpEng.exe', 'MsSense.exe',


```


```text
'elastic-agent',


```


```text
'CyveraConsole', 'CyServer.exe',


```


```text
'xagt', 'xagt.exe',


```


```text
'SophosScanD', 'SAVAdminService.exe',


```


```text
'iCoreService', 'Ntrtscan.exe',


```


```text
'esets_daemon', 'ekrn.exe'


```


```text
)


```


```text
),


```


The remaining five CTEs follow the same pattern for MDM, DLP, VPN/ZTNA, DNS security, and vulnerability management. They all feed into a single scoring step:


```text
final_score AS (


```


```text
SELECT


```


```text
CASE WHEN telemetry_running.total > 0 THEN 1 ELSE 0 END


```


```text
+ CASE WHEN edr_running.total > 0 THEN 1 ELSE 0 END


```


```text
+ CASE WHEN mdm_running.total > 0 THEN 1 ELSE 0 END


```


```text
+ CASE WHEN dlp_running.total > 0 THEN 1 ELSE 0 END


```


```text
+ CASE WHEN vpn_ztna_running.total > 0 THEN 1 ELSE 0 END


```


```text
+ CASE WHEN dns_security_running.total > 0 THEN 1 ELSE 0 END


```


```text
+ CASE WHEN vuln_scanner_running.total > 0 THEN 1 ELSE 0 END


```


```text
AS score


```


```text
FROM telemetry_running, edr_running, mdm_running, dlp_running,


```


```text
vpn_ztna_running, dns_security_running, vuln_scanner_running


```


```text
)


```


```text
SELECT


```


```text
CASE WHEN score = 7 THEN 1 ELSE 0


```


```text
END AS security_tools_healthy


```


```text
FROM final_score;


```


A few things to note about how this query is structured:


-


It measures category coverage. The CASE WHEN total > 0 THEN 1 logic around each category is what caps each signal at 1 before it adds them all together. Without it, a single tool that spawns five processes would contribute 5 to the score instead of 1, and a device missing four tool categories could still pass by having one very busy EDR agent. The cap enforces what the check is actually measuring: category coverage, not process count.


-


It normalizes different vendors into a consistent signal. This query uses independent CTEs rather than a simpler WHERE clause to normalize across different tool vendors. Regardless of whether a device runs CrowdStrike or SentinelOne, the EDR category always yields a “1.” A monolithic WHERE clause would return a raw process count, which misses the point: no matter how many processes the EDR spawns, it only gets credit for being one tool.


-


It covers both macOS and Windows. One of the challenges in writing cross-platform posture checks is that macOS tools often have no Windows equivalent, and vice versa.. This check handles that: macOS devices score 1 for MDM via **JamfDaemon** , **kandji-daemon,** **mosyled** , or **addigy-agent** ; Windows devices score 1 for the Intune MDM process, **Microsoft.Management.Services.IntuneWindowsAgent.exe** . The same applies to platform-specific EDR process names. Because every category is designed to cover both platforms within the same CTE, the threshold stays at score = 7 on both operating systems, so there's no need for platform-specific variants of the query.


## Make it work for you


Not every organization deploys all seven tool categories. For example, some use their EDR for vulnerability management and others may handle DNS security at the network layer rather than the endpoint. The process name list in each CTE covers the major vendors in each category to allow the check to work across organizations with little to no modification. However, this query is a starting point, not a final configuration. Before deploying this check in enforcement mode, you need to customize it for your environment and test it.


1.


First, find the specific detection query within the okta/customer-detections repository. We recommend cloning the project.


2.


Next, adjust the query to your unique environment. Go through each CTE and confirm your vendor's process name is in the list - if it isn't, add it. Then lower the threshold to match the number of categories you actually require. (For example, a five-tool stack should use =5, not =7.) We recommend executing the check locally using osqueryi to verify it. Refine the process names and adjust the score threshold until it aligns perfectly with your organization’s required security stack.


3.


Run the query in monitor mode against your fleet for a week. This will tell you which categories are registering zero on real devices, which is often the first sign that a tool has drifted from the expected state.


4.


Run the query in enforcement mode after you’re confident it works as expected.


## Closing the loop


While this tooling watchdog check might not seem glamorous, it is a key foundation worth having in place. This check allows you to verify that the security stack you've paid for and deployed is actually running on every device that's about to authenticate into your environment.


Over the next few days, we will explore what happens when you use advanced posture checks for an entirely different challenge.


About the Author


[Rafa Bono Staff Detection and Response Engineer Rafa Bono is a Staff Detection and Response Engineer on the Cyber Defense Team, where he specializes in identifying sophisticated digital threats within complex data environments. With 10 years of experience in the cybersecurity industry, he has led large-scale security operations for market-leading companies. His expertise focuses on engineering proactive defences against emerging global threats and vulnerabilities. In addition to his corporate role, Rafa serves as a professor at the Universitat Politècnica de Catalunya (UPC), teaching Security Events Monitoring within the Cybersecurity Management Masters program.](https://www.okta.com/blog/author/rafa-bono/)


[Katie Nickels Director, Okta Threat Intelligence Katie Nickels is a Director of Threat Intelligence, where she brings a passion for educating as she provides actionable insights on identity-based threats. With over a decade of experience in the cybersecurity industry, she previously led intelligence, research, and hunting teams at Red Canary, and also served as the Threat Intelligence Lead for the MITRE ATT&CK team. Outside of Okta, Katie teaches FOR578: Cyber Threat Intelligence for the SANS Institute and acts as a Nonresident Senior Fellow at the Atlantic Council.](https://www.okta.com/blog/author/katie-nickels/)
