---
schema_version: "1.0.0"
document_id: "20167793e0e3059aadd66be4be1875a60c7875109d87fdb827ccc99781b2f90e"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/detecting-cloud-ransomware-in-azure-with-tenable-ones-cloud-detection-and-response"
published_at: "2026-08-17T15:15:00+00:00"
first_seen_at: "2026-08-17T18:45:47.758260+00:00"
fetched_at: "2026-08-17T18:45:50.173056+00:00"
content_hash: "sha256:12a779ecd74ea3dd296dabb5291a989d57682e33a3f464c4f27be85af2f403ed"
---

# Detecting cloud ransomware in Azure with Tenable One’s cloud detection and response capabilities

Learn how Tenable One Cloud Exposure helps you unmask the sophisticated tactics of cybercrime group Storm-0501, which carries out Azure-based cloud ransomware campaigns. Tenable One Cloud Exposure uses AI-powered threat stories to expose Storm-0501 TTPs, backed by precision-engineered threat detection alerts.


## Key takeaways


1. Storm-0501 demonstrates that cloud-first ransomware groups have shifted from simple endpoint encryption to the total hijacking of cloud tenants.
2. Storm-0501 systematically neutralizes resource locks, immutability policies, and backups, making the detection of these configuration changes critical for early intervention.
3. Detecting modern campaigns requires moving beyond static rules to a unified threat story that contextually connects the dots across the attack chain.


## From ransomware to cloud ransomware


Historically, ransomware functioned as a localized threat: malicious software infected a workstation or server to encrypt local drives and hold specific host systems hostage.


Today, sophisticated ransomware actors like[Storm-0501](https://attack.mitre.org/groups/G1053/) have fundamentally changed the battleground. Instead of relying on local malware execution, they target the cloud control plane itself. They hijack high-privilege administrative identities, weaponize native cloud tools, systematically dismantle defensive barriers, and compromise entire cloud tenants from the inside out.


Storm-0501, a financially motivated cybercrime group, exemplifies this tactical shift and has repeatedly demonstrated its proficiency in bridging on-premises Active Directory systems with cloud-native Microsoft Entra ID and Azure environments.


In 2024,[Microsoft observed](https://www.microsoft.com/en-us/security/blog/2024/09/26/storm-0501-ransomware-attacks-expanding-to-hybrid-cloud-environments/) how Storm-0501 began expanding its on-premises ransomware tactics to the cloud, using cloud-native capabilities to evade detection, exfiltrate data, destroy data backups, and demand ransom payments.


This new reality of cloud ransomware demands more than endpoint monitoring; it requires[cloud detection and response](https://www.tenable.com/cloud-security/solutions/cloud-detection-and-response) (CDR). CDR provides full visibility into the attack chain and identifies the surgical techniques employed by adversaries like Storm-0501.


Driven by deep threat intelligence on[Storm-0501's evolving tactics, techniques, and procedures (TTPs](https://www.microsoft.com/en-us/security/blog/2025/08/27/storm-0501s-evolving-techniques-lead-to-cloud-based-ransomware/) ),[Tenable One Cloud Exposure](https://www.tenable.com/cloud-security/products/cnapp) maps these sophisticated maneuvers to ensure robust protection across the entire attack chain and to extend preemptive[exposure management](https://www.tenable.com/exposure-management) into post-compromise incident response.


Even in scenarios where initial breach access slips past existing security controls, Tenable One's contextual detections empower your defenders to maintain control, trace lateral movement, and neutralize fast-moving attacks before threat actors can seize, encrypt, exfiltrate, and destroy your organization’s critical data.


## Unmasking Storm-0501 TTPs: A guided walkthrough


In the following video, we demonstrate the CDR capabilities of Tenable One and how it aggregates Azure activity logs into a cohesive threat story, mapping Storm-0501 capabilities directly to the[MITRE ATT&CK framework](https://attack.mitre.org/) . You will also see the specific detections required to expose and intercept these tactics.


## From detection to action: Rapid triage and containment


Defenders can immediately use Tenable One’s CDR capabilities, with[AI-powered threat stories](https://connect.tenable.com/discussions/product-announcements/transform-disparate-isolated-alerts-to-one-threat-story-with-tenable%E2%80%99s-cloud-det/112070) , to guide surgical containment of a Storm-0501 cloud ransomware campaign. By consolidating fragmented Azure activity logs into a clear chronological timeline, Tenable One eliminates hours of manual log parsing and enables security teams to execute the following containment actions immediately:


- **Scope and revoke identities** : Use the timeline to identify the initial breach point of an[Entra ID Global Administrator role](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/permissions-reference) . Immediately terminate all active sessions, revoke refresh tokens, and rotate credentials for the compromised accounts.
- **Revert rogue access** : Trace role-assignment events in the events explorer dashboard in Tenable One to strip attacker-assigned owner privileges across affected subscriptions and delete any unauthorized persistence accounts or guest users.
- **Analyze the blast radius** : Investigate additional resources associated with the attacker using the events explorer page.
- **Restore defenses** : If the alert trail indicates deleted[Azure Resource Locks](https://techcommunity.microsoft.com/blog/coreinfrastructureandsecurityblog/using-resource-locks-to-prevent-accidental-changes-in-azure/3842402) , immutability policies, or[Azure Recovery Services](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview) vaults, immediately re-apply these defensive barriers to all surviving cloud infrastructure.
- **Recover adversary-created keys** : **** If the adversary created an unauthorized[Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault) or encryption scope to lock your storage accounts, revoke adversary access first, then restore the soft-deleted keys, take ownership of the vault, and re-encrypt data under your own keys before the soft-delete window expires.


## Azure cloud security: How to protect infrastructure against cloud ransomware


The campaign orchestrated by Storm-0501 underscores that modern defenders can no longer rely on disparate alerts. They need a unified view that connects the dots. Tenable One’s CDR capabilities provide that clarity, context, and insight, turning attackers’ complex cloud maneuvers into a clear, actionable threat story that empowers organizations to intercept ransomware at the earliest stage possible.


*Note: Tenable continuously monitors attacker campaigns and the threat landscape; therefore, additional detection rules will be released to provide an even more comprehensive coverage against this threat actor and others.*


[Learn more about Tenable One’s CDR capabilities.](https://www.tenable.com/data-sheets/tenable-one-cloud-exposure-cloud-detection-and-response-cdr)
