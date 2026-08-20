---
schema_version: "1.0.0"
document_id: "e9875eafc98392c0505e4ddc3a4812aa7d2afe5f533aa06f941388de6224212b"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:bf260ccabcf1b8bbb16523e9154be79603dd784ee37f76c3b843a8b4ec3a3fcb"
---

# Azure AD Graph Activity Logs: Ingestion and threat detection to close the visibility gap

19 June 2026 •


[Terrance DeJesus](https://www.elastic.co/security-labs/author/terrance-dejesus)


# Azure AD Graph Activity Logs: Ingestion and threat detection to close the visibility gap


Azure AD Graph Activity Logs land in Elastic with full ECS parsing. Detect ROADrecon and AADInternals enumeration with ready-to-use detection rules.


11 min read


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering)


AAD Graph Activity Logs are now ingestible into Elastic and usable for threat detection within the[SIEM/XDR solution](https://www.elastic.co/security/xdr) . That sentence shouldn't be exciting, but it is. For most of the past decade, this slice of telemetry simply didn't exist as a customer-accessible log stream. Microsoft Graph Activity Logs (the modern *graph.microsoft.com* surface) went GA in April 2024. The legacy graph.windows.net surface, the one adversary tooling actually hits, stayed dark until early 2026.


This post walks the loop end-to-end. Why visibility matters, how to ingest the logs into Elastic, how to generate realistic recon manually and with ROADrecon, and how to hunt the result in ES|QL. Everything below was validated against a live tenant.


##


Key takeaways


-


AAD Graph Activity Logs ride into Elastic through the[Azure integration](https://www.elastic.co/docs/reference/integrations/azure) and land in` logs-azure.aadgraphactivitylogs-*` with full ECS extraction.


-


ROADtools, AADInternals, and friends have been operating in a visibility gap for years. Defenders weren't capturing the calls.


-


AAD Graph is "deprecated" but still queryable in most tenants. The 1.61-internal API version still returns data that Microsoft Graph won't.


-


ECS fields land typed (` event.action` ,` event.outcome` ,` http.request.method` ,` source.ip` ,` user.id` ,` user_agent.original` ). Dataset extras stay queryable under` azure.aadgraphactivitylogs.properties.*` .


-


Five hunts reliably catch the activity: tooling user-agents, endpoint breadth,` *-internal` API misuse, FOCI client-ID mismatches, and 4xx surges.


##


A short history of defender visibility


Defenders have spent years on sign-ins, conditional access, role assignments, and OAuth consent grants. Very little content covers the *underlying* directory APIs that adversary tooling actually hits. The reason is structural: customer-accessible logs for those APIs didn't exist. Microsoft Graph Activity Logs landed first (preview October 2023, GA April 2024). AzureADGraphActivityLogs finally showed up in early 2026.


For most of the past decade, AAD Graph enumeration was invisible to SOCs, not because the telemetry was hidden, but because it didn't exist. ROADtools, AADInternals, MSOLSpray, Microburst. None of them produced data that anyone could capture, even with a perfect logging configuration.


That changes the day AzureADGraphActivityLogs start landing in your platform-logs index.


##


AAD Graph is “deprecated” but still very much alive


Quick refresher. Azure AD Graph is the legacy REST API for Entra ID directory objects, hosted at` https://graph.windows.net/{tenantId}/{objecttype}` with API versions like *1.5* , *1.6* , and *1.61-internal* . Microsoft has been telling everyone to migrate to Microsoft Graph since 2019, and the retirement date has slipped several times.


Deprecation isn’t gone. In 2026, AAD Graph can still answer requests in environments where legacy access paths remain available or where applications have not been explicitly blocked from using it. A few reasons it sticks around as an attacker target:


-


Adversary tooling hasn't been ported. ROADrecon still uses it for` gather` . AADInternals has dozens of cmdlets wrapping it.


-


The` *-internal` API versions return more data.` 1.61-internal` exposes` strongAuthenticationDetail` inline on the user object during a normal directory walk. The Microsoft Graph equivalent lives behind a separate /authentication/methods endpoint gated by` UserAuthenticationMethod.Read.All` . That asymmetry is exactly what bulk enumeration tooling exploits.


-


The block isn't a single toggle. The` blockAzureADGraphAccess` control lives per-app on` application.authenticationBehaviors` , so blocking tenant-wide means iterating every app registration. Most environments haven't done that because some legacy automation still depends on the API. Microsoft's phased retirement enforcement does the work on Microsoft's timeline, not the defender's.


-


Visibility did not exist, thus red teamers and adversaries could hammer the API endpoints for relevant information.


Legitimate AAD Graph traffic is dominated by a handful of first-party Microsoft callers. In our test tenant, the order, by volume, was` Microsoft.OData.Client` ,` Microsoft Azure Graph Client Library` , an empty-UA tail from first-party AppIds,` Microsoft ADO.NET Data Services` , and the Azure portal (Chrome UAs against the portal app ID). Anything outside that recognisable set is either internal tooling or unauthorized activity. That makes it a solid threat hunting/detection dataset. If you're capturing it.


##


Setting up the ingestion pipeline


If you're already running the Elastic Azure[integration](https://www.elastic.co/docs/reference/integrations/azure) with diagnostic settings forwarding to an event hub, skim this section. You probably just need to enable one extra log category. From scratch, it's about a 20-minute path.


####


Step 1: A stack to receive the logs


Any Elastic deployment works. An Elastic Cloud trial is the lowest-friction option for prototyping. Another option is the[Elastic Container Project](https://github.com/peasead/elastic-container) for getting started. The Azure integration already handles AzureADGraphActivityLogs once it's enabled.


####


Step 2: Add the Azure integration


In Kibana, Integrations > Azure Logs > Add Azure Logs. Plug in your Event Hub connection string, the Event Hub name, and a Storage account for offset checkpointing, all on an event hub in the same subscription as your tenant.


Enable the Azure logs v2 data stream specifically. That's the entry point for AAD Graph Activity Logs. The events router matches` category == "AzureADGraphActivityLogs"` and reroutes documents to` logs-azure.aadgraphactivitylogs-*` , where the dataset pipeline applies full ECS extraction.


We've also broken Azure AD Graph Activity Logs out into its own integration item, so you can search for "Azure AD Graph Activity Logs" and install via the policy template directly.


####


Step 3: Enable diagnostic settings on Entra ID


This is the step most defenders miss. AzureADGraphActivityLogs as a diagnostic-settings category is newer. Even if your Entra ID diagnostic settings have been configured for a while now, the new category needs a fresh tick. Otherwise, the data lives and dies in Microsoft's tenant boundary.


In the Azure portal:


1. Entra ID > Monitoring > Diagnostic settings > + Add diagnostic setting.
2. Name it.
3. Under Logs, check AzureADGraphActivityLogs. While you're there, MicrosoftGraphActivityLogs, SignInLogs, and AuditLogs are worth turning on if they aren't already. The integration handles all of them.
4. Under Destination details, Stream to an event hub (the same one from step 2).
5. Save.


####


Step 4: Verify data is flowing


Within a few minutes, you should start seeing events. Fastest sanity check:


```text
FROM logs-azure.aadgraphactivitylogs-*
| LIMIT 20
```


If documents come back with a populated` event.action` ,` http.request.method` , and` zure.aadgraphactivitylogs.properties.*` fields, you're good. If nothing shows up, the usual suspects are a forgotten event hub permission, a typo in the connection string, or the AAD Graph category just not being ticked.


To force a few events, sign in to the Azure portal and click around Users or Applications. The portal still calls AAD Graph internally for some object details. If that doesn't generate anything, this curl loop will:


```text
TOKEN=$(az account get-access-token --resource https://graph.windows.net --query accessToken -o tsv)
TID=$(az account show --query tenantId -o tsv)
for obj in users groups servicePrincipals applications tenantDetails; do
curl -sS -o /dev/null -H "Authorization: Bearer $TOKEN" \
"https://graph.windows.net/$TID/$obj?api-version=1.6&\$top=5"
done
```


##


Field shape


Once data is flowing, properties land as typed, top-level fields. The ones that matter for hunting:


- [ECS](https://www.elastic.co/docs/reference/ecs) , populated directly:` event.action` (a semantic verb derived from method + collection, e.g.,` users-read, batch-execute` ),` event.outcome` ,` event.duration` ,` http.request.method` ,` http.response.status_code, source.ip` , and` source.geo.*, user.id, user_agent.original` (plus parsed sub-fields),` url.path, azure.tenant_id, cloud.service.name = "Azure AD Graph"` .
- Dataset-specific under` zure.aadgraphactivitylogs.properties.*: app_id,` ,` app_id` ,` api_version` ,` actor_type` ,` roles` ,` scopes` ,` wids` ,` identity_provider` ,` client_auth_method` ,` sign_in_activity_id` ,` token_issued_at` .
- ` related.user` gets both` user.id` and` properties.app_id` , so pivots on the OAuth-client dimension work alongside the user pivot.


Raw JSON stays in *event.original* for forensic replay. You shouldn't need to reach into it for normal hunting. If you do, ES|QL's[JSON_EXTRACT()](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/string-functions/json_extract) is the lever.


##


AAD Graph enumeration with ROADrecon


To know what to hunt, you need to know what the activity looks like. The two toolkits below are the most common sources of AAD Graph traffic in red team and security research workflows. I ran both against our testing tenant.


Note: I take no responsibility for misuse of this code. Run these tools only against tenants you own or have explicit written authorization to test.


###


ROADrecon: Bulk enumeration test


ROADrecon is the data-collection module of[ROADtools](https://github.com/dirkjanm/ROADtools) , Dirk-jan Mollema's Entra ID research framework. Highly recommended if you haven't used it. *gather* walks every interesting object type in the directory (users, groups, service principals, applications, devices, directory roles, role assignments, eligible role assignments, OAuth2 permission grants, administrative units) and writes the result to SQLite.


Setup is the standard workflow:


```text
pip install roadrecon
roadrecon auth --device-code -c 04b07795-8ddb-461a-bbee-02f9e1bf7b46 -r https://graph.windows.net
```


The device-code flow hands you a URL and a code. We use the Microsoft Azure CLI as the default (` 1b730954-1685-4b74-9bfd-dac224a7b894` - AAD PowerShell), which returned 403s in our tenant. After signing in:


```text
roadrecon gather
```


Running *roadrecon gather* with the resulting token completed cleanly. From the tenant's perspective, the run produced just over ~2,000 AAD Graph calls and logs in roughly 1 minute. Bulk enumeration across every object type ROADrecon knows.


From this, we can form some initial detections to start flagging these anomalies.


##


Key fields for AAD Graph threat detection


Before the hunts, here are some solid starting fields for detecting anomalies.


Field Description What you can find


` event.action` Semantic verb (HTTP method + collection, e.g.,\`users-read, batch-execute) A cheap filter to isolate AAD Graph activity by intent


` http.request.method` GET, POST, PATCH\`, DELETE Reads (recon) vs writes (modification, credential injection, persistence)


` http.response.status_code` HTTP status returned Successful vs blocked recon; bursts of 4xx indicate permission-probing or brute-forcing


` user.id` Calling user's directory object ID Identity attribution; pivot to that user's other activity in SignInLogs / AuditLogs


` user_agent.original` Full UA string of the caller Whether the caller is a first-party Microsoft library, a developer tool (curl, Python aiohttp), or known offensive tooling


` url.path` Resource path (/users, /policies, /servicePrincipals, ...) Which directory object types are being touched; breadth across distinct paths indicates bulk enumeration


` azure.aadgraphactivitylogs.properties.app_id` OAuth client ID that issued the token Whether traffic comes from a legitimate first-party client or from a FOCI-swap-style abuse path


` azure.aadgraphactivitylogs.properties.api_version` 1.5, 1.6, 1.61-internal, etc. Whether the caller is asking for internal-only fields (strongAuthenticationDetail, full CAP set) that adversary tooling specifically targets


` azure.aadgraphactivitylogs.properties.actor_type` User, Application, ServicePrincipal Human caller vs service-principal / app-only flow


` azure.aadgraphactivitylogs.properties.roles` /` wids` Directory role display names and well-known role template GUIDs held by the caller Whether a privileged role (Global Admin, Application Administrator, etc.) is being exercised at the moment of the call


` azure.aadgraphactivitylogs.properties.scopes` OAuth scopes on the calling token Which directory permissions the token actually grants the caller


` azure.aadgraphactivitylogs.properties.client_auth_method` How the client authenticated (PRT, certificate, secret, ...) Fingerprints for PRT abuse, device-PRT exploitation, or stolen client-credential use


` azure.aadgraphactivitylogs.properties.sign_in_activity_id` Correlation ID to the originating sign-in Pivot from an AAD Graph call back to the sign-in event that produced the calling token


` azure.aadgraphactivitylogs.properties.token_issued_at` Timestamp the token was minted Token-age analysis; calls riding on a token issued days ago can indicate stale-token / refresh-token abuse


##


Detection and prevention


###


Detection


The prerequisite for any AAD Graph detection is having the logs in the first place. The *AzureADGraphActivityLogs* diagnostic category needs to be enabled in Entra ID and routed to a destination you can query (at minimum a Log Analytics workspace, ideally also forwarded to an event hub for Elastic ingestion as described in the setup section above). Until that's done, the calls described in this post happen entirely off-camera, and none of the hunts below will fire.


If you can't ingest into Elastic right now, enable the diagnostic setting anyway and send to Log Analytics. The KQL equivalents of the hunts below are straightforward, and the data accumulates with retention even without further processing.


###


Prevention


There's no single tenant-wide AAD Graph kill-switch in the portal. The actual application-layer control is:


- ` application.authenticationBehaviors.blockAzureADGraphAccess`


A per-app Boolean on the application resource (Microsoft Graph beta,[docs](https://learn.microsoft.com/en-us/graph/api/resources/authenticationbehaviors) ). Blocking at scale means walking through every app registration and flipping it manually or programmatically. Microsoft's own phased retirement is doing this on their timeline regardless. The further along that gets, the less surface there is to defend.


Defenders can move on the same axes in the meantime:


-


Audit applications in your tenant that still hold tokens for *graph.windows.net* . Set` blockAzureADGraphAccess = true` on the ones that don't need it. Anything still depending on AAD Graph breaks loudly, which surfaces legacy automation you didn't know you had.


-


Apply Conditional Access with Azure AD Graph as a target resource. The Azure AD Graph service principal (` 00000002-0000-0000-c000-000000000000` ) doesn't show in the standard CA app picker, but it's covered by *All resources* policies and is individually targetable via the[custom security attribute filter approach](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-cloud-apps#protect-directory-information) . Microsoft's[March 2026 enforcement change](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-cloud-apps#new-conditional-access-behavior-when-an-all-resources-policy-has-a-resource-exclusion) makes this more practical: low-privilege scopes ( *User.Read* , *People.Read* , etc.) that used to be auto-excluded from CA enforcement are now treated as AAD Graph access, so *all resource* policies actually gate them. CA evaluates at token issuance, so already-valid tokens keep working until expiry.


-


Apply CA to the FOCI clients adversary tooling rides on (Microsoft Teams, Microsoft Office, OneDrive, Azure PowerShell, etc.). Require managed and compliant devices. The swap path collapses if the underlying client can't sign in.


-


For service-principal callers,[Workload Identities Premium](https://learn.microsoft.com/en-us/entra/identity/conditional-access/workload-identity) adds CA scoped to service principals. Conditions are limited to location, Identity Protection risk, and authentication context; the only grant control is Block. Useful for collapsing external- and risky-context paths, not for scoping an SP to specific cloud apps the way user CA does.


-


Disable device-code flow for users who don't need it.` roadrecon auth --device-code` is the path of least resistance into the entire pipeline above and is extremely common in OAuth phishing.


###


Behavior detection


We shipped detection rules covering the AAD Graph recon shapes documented above. Each lives in the[Elastic detection-rules](https://github.com/elastic/detection-rules) repository and runs natively against the parsed` logs-azure.aadgraphactivitylogs-*` data stream.


[Azure AD Graph Access with Suspicious User-Agent](https://github.com/elastic/detection-rules/blob/31d1fa31152c208dfde4feeb6737ca06e030ae53/rules/integrations/azure/discovery_aad_graph_suspicious_user_agent.toml) - KQL match rule. Triggers when AAD Graph receives traffic from user-agent strings matching offensive tooling families (Python, aiohttp, curl, Go-http-client, axios, AzureHound, BloodHound, AADIntenals, etc.). Solid baseline signal because no first-party Microsoft component identifies as any of these, while default tooling does.


[Azure AD Graph High 4xx Error Ratio from User](https://github.com/elastic/detection-rules/blob/df0396ea4a3b0fb25529444166c36c430941d3a6/rules/integrations/azure/discovery_aad_graph_high_4xx_ratio_by_user.toml) - ES|QL aggregation. Triggers when a single caller produces an unusually high ratio of 4xx responses against AAD Graph in a short window. Recon and brute-force token usage leave a tail of 403s and 404s as tools walk endpoints they don't have permission for, ask for object IDs they don't have, or use a client ID unauthorized for AAD Graph.


[Azure AD Graph Access with Unusual Client and User](https://github.com/elastic/detection-rules/blob/0c15c6c581780d962b33972a8a83263b75fb2d79/rules/integrations/azure/discovery_aad_graph_unusual_client_for_user.toml) - KQL new_terms rule, medium severity. Fires when a (calling OAuth client, signed-in user) pair appears on AAD Graph for the first time in the prior 14 days. Catches FOCI swaps, phished refresh tokens redeemed for clients the user doesn't normally use, and stolen tokens used under unfamiliar clients. Ignores known first-party applications that were commonly observed interacting with Azure AD that are backend owned by Microsoft.


[Azure AD Graph Access with Unusual User and ASN](https://github.com/elastic/detection-rules/blob/6f933e04ddbe720898c9ea9c4bae234700a822fe/rules/integrations/azure/initial_access_aad_graph_unusual_asn.toml) - KQL match rule. Excludes the common Microsoft / AWS / GCP / Akamai / Cloudflare ASN organisations and flags AAD Graph traffic originating outside that set. Adversary tooling typically rides on residential ISPs, VPS providers, or anonymising networks that produce a different ASN distribution than legitimate first-party callers. Tunable per tenant by adjusting the excluded ASN list.


[Azure AD Graph Potential Enumeration (ROADrecon)](https://github.com/elastic/detection-rules/blob/4256409e8925a8da016448d1378bc669c2333961/rules/integrations/azure/discovery_aad_graph_roadrecon_aiohttp_enumeration.toml) - ES|QL aggregation, **high severity** . Requires both an *aiohttp* user-agent and a burst of 500+ AAD Graph requests from a single identity. ROADrecon's *gather* command uses aiohttp by default and walks every directory object type, so the combination is essentially a tool fingerprint. Higher severity than the generic non-Microsoft UA rule because the additional burst requirement removes the developer-prototype false-positive class.


[Entra ID OAuth Device Code Sign-in to Azure AD Graph Enumeration](https://github.com/elastic/detection-rules/blob/6a946179e66c7a952e55a33741c160d6bd83f3b8/rules/integrations/azure/credential_access_device_code_signin_aad_graph_enum.toml) - EQL sequence, **high severity** . Joins a successful device-code sign-in to the legacy AAD Graph audience (` 00000002-0000-0000-c000-000000000000` ) on an unmanaged device with directory enumeration against *graph.windows.net* by the same user within five minutes. Device-code phishing lands an OAuth token without touching the user's password or MFA, so immediate Graph reads of users, service principals, applications, role assignments, policies, or tenant details under that token are the compromised identity being driven by the attacker. Cross-data-stream sequence removes the single-event false-positive class that the other AAD Graph rules carry.


##


AAD Graph visibility: what comes next


For most of the past decade, AAD Graph activity was the telemetry equivalent of dark matter. We knew it was there because adversary tooling kept pointing at it, but customers had no diagnostic stream to subscribe to and no logs to query. Microsoft Graph Activity Logs closed half the gap when they went GA in April 2024. *AzureADGraphActivityLogs* finally closed the other half in early 2026.


Now that the data exists, the rest is on us. Add the new diagnostic setting, point it at an event hub, ingest into your stack, turn detections on (or create your own) and get to monitoring.


The detections in this post are a starting point. Once you have AAD Graph traffic landing in your stack and a baseline of what normal looks like in your tenant, the same patterns generalize. Legitimate first-party Microsoft callers form a small, recognisable set, and anything outside that set deserves a closer look.


The activity was always there. The visibility finally is too.


Happy hunting!


##


References


The following were referenced throughout the above research:


- [Elastic Azure Integration](https://www.elastic.co/docs/reference/integrations/azure)
- [ROADtools GitHub](https://github.com/dirkjanm/ROADtools)
- [ROADtools wiki](https://github.com/dirkjanm/ROADtools/wiki)
- [BloodHound with Azure AD capabilities](https://github.com/dirkjanm/BloodHound-AzureAD)
- [AADInternals](https://github.com/Gerenios/AADInternals)
- [AADInternals documentation](https://aadinternals.com/aadinternals/)
- [Migrate your apps from Azure AD Graph to Microsoft Graph](https://learn.microsoft.com/en-us/graph/migrate-azure-ad-graph-overview)
- [Configure Microsoft Entra diagnostic settings for activity logs](https://learn.microsoft.com/en-us/entra/identity/monitoring-health/howto-configure-diagnostic-settings)
- [Access Microsoft Graph activity logs](https://learn.microsoft.com/en-us/graph/microsoft-graph-activity-logs-overview)
- [Microsoft Graph activity logs is now generally available](https://techcommunity.microsoft.com/blog/microsoft-entra-blog/microsoft-graph-activity-logs-is-now-generally-available/4094535)
- [The Missing Link: AADGraphActivityLogs Finally Arrives](https://www.invictus-ir.com/news/the-missing-link-aadgraphactivitylogs-finally-arrives)
- [Azure AD privilege escalation - Taking over default application permissions as Application Admin](https://dirkjanm.io/azure-ad-privilege-escalation-application-admin/)
- [Abusing Azure AD SSO with the Primary Refresh Token](https://dirkjanm.io/abusing-azure-ad-sso-with-the-primary-refresh-token/)


##


About Elastic Security Labs


Elastic Security Labs is the threat intelligence branch of Elastic Security dedicated to creating positive change in the threat landscape. Elastic Security Labs provides publicly available research on emerging threats with an analysis of strategic, operational, and tactical adversary objectives, then integrates that research with the built-in detection and response capabilities of Elastic Security.Follow Elastic Security Labs on Twitter[@elasticseclabs](https://twitter.com/elasticseclabs?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor) and check out our research at[www.elastic.co/security-labs/](https://www.elastic.co/security-labs/) .


#### Jump to section


- [Key takeaways](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#key-takeaways)
- [A short history of defender visibility](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#a-short-history-of-defender-visibility)
- [AAD Graph is “deprecated” but still very much alive](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#aad-graph-is-deprecated-but-still-very-much-alive)
- [Setting up the ingestion pipeline](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#setting-up-the-ingestion-pipeline)
- [Step 1: A stack to receive the logs](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#step-1-a-stack-to-receive-the-logs)
- [Step 2: Add the Azure integration](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#step-2-add-the-azure-integration)
- [Step 3: Enable diagnostic settings on Entra ID](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#step-3-enable-diagnostic-settings-on-entra-id)
- [Step 4: Verify data is flowing](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#step-4-verify-data-is-flowing)
- [Field shape](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#field-shape)
- [AAD Graph enumeration with ROADrecon](https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection#aad-graph-enumeration-with-roadrecon)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Azure%20AD%20Graph%20Activity%20Logs:%20Ingestion%20and%20threat%20detection%20to%20close%20the%20visibility%20gap&url=https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection&title=Azure%20AD%20Graph%20Activity%20Logs:%20Ingestion%20and%20threat%20detection%20to%20close%20the%20visibility%20gap)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/aad-graph-activity-logs-threat-detection&title=Azure%20AD%20Graph%20Activity%20Logs:%20Ingestion%20and%20threat%20detection%20to%20close%20the%20visibility%20gap)
