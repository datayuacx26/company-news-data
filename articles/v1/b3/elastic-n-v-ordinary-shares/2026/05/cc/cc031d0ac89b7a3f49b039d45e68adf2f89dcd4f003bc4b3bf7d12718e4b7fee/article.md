---
schema_version: "1.0.0"
document_id: "cc031d0ac89b7a3f49b039d45e68adf2f89dcd4f003bc4b3bf7d12718e4b7fee"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:6c7326a42dc9f3b4d65a4b2d5f4e4d747d631358b7439f3862d3ccf9824ca4f2"
---

# Detecting Tycoon 2FA AiTM attacks across Entra ID and Google Workspace

26 May 2026 •


[Samir Bousseaden](https://www.elastic.co/security-labs/author/samir-bousseaden) •


[Terrance DeJesus](https://www.elastic.co/security-labs/author/terrance-dejesus)


# Detecting Tycoon 2FA AiTM attacks across Entra ID and Google Workspace


Tycoon 2FA bypasses MFA on Entra ID and Google Workspace. We map telemetry fingerprints across both platforms, ship detection rules for both tiers, and contain incidents in under 10 seconds with Elastic Workflows.


14 min read


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering) ,


[Threat Intelligence](https://www.elastic.co/security-labs/category/threat-intelligence)


Tycoon 2FA is currently the most prolific Phishing-as-a-Service (PhaaS) platform among AiTM phishing kits. First observed in August 2023 and attributed to[Storm-1747](https://malpedia.caad.fkie.fraunhofer.de/actor/storm-1747) (per Microsoft Threat Intelligence), the kit provides turnkey adversary-in-the-middle (AiTM) capabilities that bypass multi-factor authentication and steal authenticated session tokens from Microsoft 365 and Google Workspace accounts. At its peak, Tycoon 2FA[accounted](https://blogs.microsoft.com/on-the-issues/2026/03/04/how-a-global-coalition-disrupted-tycoon/) for roughly 62% of phishing attempts blocked by Microsoft, reaching over 500,000 organizations monthly.


Despite a coordinated[takedown](https://blogs.microsoft.com/on-the-issues/2026/03/04/how-a-global-coalition-disrupted-tycoon/) in March 2026 led by Microsoft and Europol, with support from Cloudflare, SpyCloud, eSentire, and other partners that seized over 300 domains, operators adapted within weeks. By late April 2026,[eSentire](https://www.esentire.com/blog/tycoon-2fa-infrastructure-update-threat-actors-adapt-following-global-coalition-takedown) documented campaigns combining Tycoon tradecraft with OAuth Device Code phishing flows, and the kit remains the #1 entry on[ANY.RUN](https://any.run/malware-trends/tycoon/) 's malware trends tracker.


##


How Tycoon 2FA works


###


The AiTM mechanism


Tycoon 2FA operates as a reverse proxy between the victim and the legitimate identity provider (Entra ID or Google). It is not a static credential harvester. It proxies the real login flow in real time:


1. The victim receives a phishing email containing a link or QR code embedded in a PDF, SVG, HTML, or PPTX attachment.
2. The link routes through a multi-layer redirect chain. The kit performs browser fingerprinting, CAPTCHA challenges, and anti-analysis checks before presenting the login page.
3. The victim sees a pixel-perfect replica of the Microsoft or Google login page, often including the target organization's branding dynamically fetched from the real service.
4. Credentials are relayed in real time to the legitimate identity provider. The real MFA challenge is triggered and proxied back to the victim.
5. The victim completes MFA normally. The identity provider issues a session token. The proxy intercepts this token before it reaches the victim's browser.
6. The attacker now holds a fully authenticated access token.


The session cookie is the value the operator monetizes. Once captured, MFA is moot because the operator replays minted tokens post-MFA.


###


Two structural variants in current rotation


Two distinct kit variants we analyzed were in active use:


WebSocket AiTM (the "classic" Tycoon 2FA flow): The victim authenticates through a kit-hosted proxy that forwards traffic to Microsoft or Google over WebSocket (Socket.IO) and captures the post-MFA session cookie. The kit's JavaScript client controller maintains a real-time bidirectional channel to the C2 server, relaying credentials and authentication responses as the victim types. These responses include minted access and refresh tokens for use.


Device-code-grant abuse (Microsoft only): The kit relay obtains a device code from Microsoft's oauth2/devicecode endpoint with Microsoft Authentication Broker (` 29d9ed98-a469-4536-ade2-f981bc1d605e` ) as the client, displays it to the victim through a "verification code" lure, and exchanges the code for access/refresh tokens after the victim signs in at the legitimate microsoft.com/devicelogin endpoint.


###


Evasion techniques


The kit employs layered anti-analysis mechanisms confirmed through JavaScript decompilation:


- IP-based researcher filtering: Before any content is shown, the kit calls *api.ipapi.is* (or equivalent service) to check the visitor's IP against a blocklist of cloud/hosting providers (Leaseweb, M247, DigitalOcean, Linode, Amazon, OVH, Hetzner, Google, Microsoft, Cloudflare, Akamai, Fastly, stored as reversed strings to evade static scanning). Visitors on cloud infrastructure are redirected to a benign decoy site.
- Bot/tool detection: Checks for *navigator.webdriver* (Selenium), *window.callPhantom* /` window._phantom` (PhantomJS), and "Burp" in the user-agent string. Detection triggers a redirect to *about:blank* .
- DevTools blocking: Intercepts keyboard shortcuts for developer tools (F12, Ctrl+Shift+I/J/C, Ctrl+U, macOS equivalents) and disables right-click context menus.
- Debugger trap: A *setInterval* loop running every 100ms inserts a debugger statement and measures execution time. If *DevTools* are open (execution pauses >100ms), the victim is redirected to a decoy site.
- DOM vanishing: Malicious JavaScript removes itself from the *DOM* after execution, leaving no trace for static inspection.
- Per-victim encryption: The payload uses a custom two-stage cipher (Caesar shift + XOR with a PRNG-generated keystream) seeded with per-session values. The seed, key, and encrypted blob are generated server-side for each victim, making static signature detection impossible.
- Platform targeting: On Linux desktops, it writes an empty string to blank the page: likely assuming Linux users are more likely to be security researchers.
- Fake CAPTCHA: A custom image-grid CAPTCHA replaces Cloudflare Turnstile in the current variant. Unsplash-sourced images in a 3×3 grid provide human verification before the phishing page loads.


For Google-targeted campaigns, the first-hop lure is frequently staged on legitimate Google infrastructure, such as Google Storage or Google Sites, though operator-controlled or compromised domains are also observed. When Google's own hosting is used, the` storage.googleapis.com` or` sites.google.com` origin provides built-in reputation cover before the victim reaches the AiTM relay.


In other instances the victim's email is auto-filled and the "Next" button is auto-clicked: the victim lands directly on the password page, making it look like they're already partially authenticated (increasing trust) :


```text


var emailcheck = "victim@email.corp";
// ...
function tryfindingele(email) {
emailinputcheck.value = email;
emailsectionelecheck.querySelector(".btn-blue-next-btn").click();
}
if (emailcheck !== "0") { tryfindingele(emailcheck); }
```


##


Microsoft 365 / Entra ID


###


A two-tier operational architecture


Tycoon 2FA's current operational model splits across two distinct infrastructure tiers, each with its own ASN, role, and behavioral signature. Defenders looking for a single pattern will catch one tier and miss the other.


Tier 1 - Kit Relay


The automated backend that handles token acquisition and renewal. Characteristics:


- Cloud-VPS egress IPs from hosting providers (Alibaba Cloud, similar cheap-VPS ASNs), rotating across multiple IPs in different /16 blocks during a single engagement.
- Node.js HTTP client user agents: node (bare, default Node.js UA), axios/1.15.2, node-fetch/1.0, undici.
- Client app: Microsoft Authentication Broker (29d9ed98-a469-4536-ade2-f981bc1d605e), later used with[Device Registration Service (DRS)](https://learn.microsoft.com/en-us/entra/identity/devices/device-registration-how-it-works) to mint a primaryRefreshToken (PRT).
- Token-type progression: incomingTokenType: none (initial victim auth) > refreshToken (kit relay renewal loop, repeated across rotating IPs) > Rogue Device Registration > primaryRefreshToken (PRT replay, broader scope).
- Non-interactive sign-ins: After the initial interactive device-code completion, subsequent token operations are server-to-server refreshes.


Tier 2 - Operator Console


The human (or human-simulating tool) that performs post-compromise reconnaissance. Characteristics:


- Residential-shaped ISP or proxy egress, typically a small ASN not present in common hosting-provider threat feeds. Multiple IPs in a single /24, all acting in coordination.
- Single browser user agent (e.g., Firefox on Windows) fixed across all IPs in the cluster. A configured tool, not independent users.
- Browser-based interactive sign-ins to Microsoft web apps: My Profile, My Signins, Microsoft Approval Management, Outlook Web and OfficeHome.
- Single c_sid (client session ID in Graph Activity Logs) shared across all IPs, confirming a single session distributed across the pool.
- Operational tempo: Typically appears 10-20 minutes after the kit relay's first successful token issuance. The gap represents the kit-to-operator handoff window.


The durable cross-tier detection signal: Two distinct ASNs (one cloud-VPS, one residential-shaped) authenticating as the same user principal within minutes. Single-ASN rules catch one tier; the cross-tier pivot is the high-confidence indicator.


###


Post-compromise Graph API enumeration


Once the operator console has a valid token, a rapid burst of Microsoft Graph API calls follows, typically dozens of requests within 30-60 seconds, hitting high-value reconnaissance endpoints:


Recon Category Example Endpoints Purpose


Role Discovery transitiveRoleAssignments, memberOf/directoryRole, roleManagement/directory/roleAssignments Check what Entra ID roles the compromised identity holds


Cross-Tenant Recon tenantRelationships/getResourceTenants Enumerate trusted cross-tenant relationships for lateral movement


Mailbox Recon me/mailboxSettings Read forwarding rules, auto-replies, timezone


Contact Harvesting me/contactFolders/contacts ($top=1000) Dump contact list for next-wave phishing targets


Org & Licensing subscribedSkus, organization, appRoleAssignedResources ($top=999) Map tenant licensing, org structure, app landscape


Key telemetry indicators of automated post-compromise recon:


- Volume and speed: 20-30+ calls within a 30-60 second window, each hitting a different endpoint.
- Mixed HTTP methods: GET for most endpoints, POST for actions like *getResourceTenants* .
- Structured query parameters: *$select* , *$top=999* , *$count=true* - optimized for maximum data extraction per call.
- /beta/ API usage: Disproportionately used by offensive tooling versus normal portal navigation.
- Mixed success/failure: Some endpoints return 400 or 403 (the kit probes everything regardless), while most return 200. Failed recon attempts are still recon.
- Empty C_DeviceId: The token was issued to an unmanaged, unregistered device.
- First-party apps with broad pre-consented scopes: Tokens for My Profile carry scopes including *RoleManagement.ReadWrite.Directory* , *MailboxSettings.ReadWrite* , *UserAuthenticationMethod.ReadWrite* , and *User.RevokeSessions.All* - all pre-consented, requiring no OAuth consent prompt.


###


Device-PRT persistence


As stated earlier, the kit can establish device-registration persistence that survives standard session-revocation playbooks. The mechanism:


1. MAB refresh token is resource-swapped at *oauth2/token* for an access token whose *aud* is *urn:ms-drs:enterpriseregistration.windows.net* (same client ID, new audience, no consent prompt).
2. The kit uses the *urn:ms-drs:enterpriseregistration.windows.net* access token to POST endpoint *EnrollmentServer/device* with a locally-generated PKCS#10 CSR, synthetic device metadata and transport key blob. DRS creates a device object, assigns a device ID, signs and returns a device certificate.
3. The kit builds a JWT containing the user’s refresh token, signs it RS256 with the device private key, and embeds the device certificate in the JWT header. It POSTs this to *login.microsoftonline.com/common/oauth2/token* as a JWT bearer grant. Entra validates the signature against the cert and returns the PRT plus a session key encrypted (JWE).
4. When a defender fires *revokeSignInSessions* (which invalidates all user-level tokens and refresh tokens), the device PRT remains valid because the device is a separate principal in Entra ID.
5. From the new relay IPs, the kit uses the PRT plus session key to sign per-request *HMAC-SHA256* assertions to */oauth2/token* , brokering access tokens for any first-party` client_id` it names (Teams, Outlook, OneDrive, Office, Intune).


###


Why doesn't revoking sessions stop Tycoon 2FA?


This means the standard incident response sequence of "revoke sessions > reset password" is insufficient. Defenders must enumerate and delete registered devices before revoking sessions to break the device-PRT chain atomically.


###


Detection nuances - Microsoft


**Identity Protection may not flag kit infrastructure.** Tycoon 2FA's current egress IPs rotate aggressively and may not be in Microsoft's risk corpus. Defenders relying solely on Entra ID risk signals for AiTM detection will see nothing.


**c_sid in Graph Activity Logs is NOT the user object ID.** It's a session/security-context identifier. Analysts filtering Graph Activity Logs by` c_sid == user_object_id` will get empty results and conclude the attacker didn't use Graph tokens. The correct hunt pivot is source IP + appId, cross-referenced with sign-in logs to map IP to user.


**Geolocation is unreliable for cloud-provider IPs.** The same kit relay IP can geolocate to different cities within the same sign-in session. ASN is the only reliable enrichment for detection rules.


**Token minting visibility.** Token minting or issuance is not logged; authentication events leveraging these tokens propose a more reactive hunting signal.


**Entra ID Protection Risky User Status.** Entra ID protection analyzes sign-in events, sessions, tokens and more to apply a risk level and status to users. *aiConfirmedSafe* was observed during tier 2 relay, marking the user with no risk. Then User Risk anomalies were identified based on *anomalousToken* which then placed the user back into a medium risk. Simply excluding events where *aiConfirmedSafe* can blind organizations to false-negatives from Microsoft’s labeling.


##


Google Workspace


###


Single-tier kit relay


The Google variant operates as a single-tier kit relay without the distinct operator console tier seen on the Microsoft side. Multiple kit relay IPs (typically from cheap hosting ASNs like Clouvider, Host Telecom, or similar) authenticate the same user within minutes, each performing the same four-event sequence:


1. ` login_success` password validated (T+0.000s)
2. ` login_verification` with` is_second_factor: true` - kit relays the TOTP/SMS/push code in real time, completing 2SV (T+0.000s)
3. token: authorize for Google's Chrome OAuth client (77185425430) (T+0.4 to 0.6s)
4. ` DEVICE_REGISTER_UNREGISTER_EVENT` (new device is registered by Google due to profile authentication) (T+0.6 to 1.2s)


That ~1-second compression is a signal of automated logins.


The kit consistently authorizes the same OAuth client across every relay session:


Field Value


google_workspace.token.client.id 77185425430.apps.googleusercontent.com


google_workspace.token.app_name Google Chrome


google_workspace.token.client.type NATIVE_DESKTOP


google_workspace.device.type WINDOWS


google_workspace.token.scope.value[https://www.google.com/accounts/OAuthLogin](https://www.google.com/accounts/OAuthLogin)


google_workspace.token.method_name authorize


The *OAuthLogin* scope is Chrome's internal bootstrap sign-in scope. It is not a data-plane scope (it does not by itself grant Gmail, Drive, or Calendar access). The kit's blast radius from this single scope is bound to a long-lived sign-in capable of becoming a Chrome Sync session, not direct mailbox or file access without further token-exchange calls.


What the *token.authorize* event from a VPS ASN confirms is that the authorization happens server-side during the relay, not from the victim's device, making it suspicious regardless of operator intent.


###


Kit JavaScript architecture (Google variant)


Decompilation of the Google-targeting WebSocket variant reveals a 5-layer architecture:


Layer Function


1. Anti-Analysis IP filtering via api.ipapi.is (cloud provider blocklist with reversed strings), bot/debugger detection, DOM vanishing


2. Phishing HTML ~747KB base64-decoded Google sign-in clone with 15 input fields covering every Google auth method


3. WebSocket C2 Socket.IO 4.6.0 real-time relay (send_to_browser / response_from_browser events)


4. Encrypted Payload Per-victim Caesar+XOR cipher (LCG PRNG, unique seed per session), eval()'d at runtime


5. Libraries CryptoJS 4.2.0 for AES-CBC credential encryption (hardcoded key 1234567890123456 to encrypt collected credentials), list.js


The 15 input fields capture every Google 2FA method: password, TOTP, SMS, voice call, backup codes, recovery email, phone verification, security key fallback, mobile prompt, and forced password change. The “recieveid” Socket.IO event name (note the typo) is a consistent kit fingerprint.


###


Detection nuances - Google


**Google Alert Center may stay silent.** Even when multiple sign-ins from multiple ASNs hit the same user within minutes, Alert Center records may not flow to the Alert API. Google's victim-mailbox security alert emails are not a substitute, since they go to the compromised user's inbox, not the admin surface.


**is_suspicious may not fire.** Kit relay IPs from cheap hosting ASNs may not be in Google's risk corpus. Defenders relying on this field as a primary signal will have blind spots. In the canary engagement,` is_suspicious` was false on every` login_success` from all four kit IPs across both Clouvider and Host Telecom.


**No user-agent on login events:** The Reports API login events do not include user-agent or device-fingerprint data. The UA-based detections that work on the Entra side (node / axios / undici) have no direct Google equivalent.


**OAuth workflow visibility is shallow:** Google's *token.authorize* event surfaces *client.id* , *app_name* , *client.type* , and *scope.value* , and that's the full set. There is no *resource_id* distinct from scope, no grant-type field, and no incoming-token-type field.


**Most auxiliary streams stay quiet:** no *google_workspace.context_aware_access* events fired (despite five new device records on the user) and no Alert Center records reached the Alert API. The kit footprint lives in three streams only: login, token, and device. Hunts that depend on any other stream will not detect this kit.


##


Tycoon 2FA across Entra ID and Google Workspace


Dimension Microsoft 365 (Entra ID) Google Workspace


Kit relay infrastructure Cloud-VPS hosting ASNs, rotating IPs Cloud-VPS hosting ASNs, rotating IPs


Kit relay user agents` node` ,` axios` ,` undici` ,` node-fetch` Not exposed (Reports API lacks UA)


Auth flow targeted Auth Broker device-code grant Google Chrome OAuth sign-in


Persistence scope Device registration leading to primaryRefreshToken (PRT) Not observed


Persistence durability High - device-PRT can survive session revocation Low - single OAuth revoke sufficient


Operator console tier Yes - residential-proxy IPs, browser-based M365 web app recon Not observed


Risk engine flagged kit egress Yes - User Risk detection for *anomalousToken* No (` is_suspicious` silent)


SOC log latency <5 minutes (sign-in logs near-real-time) Up to ~3 hours (Reports API lag)


CA / policy defense available Block device-code-flow CA > clean 53003 rejection No equivalent policy


Kill-switch complexity Must delete registered devices before revoking sessions Single OAuth revoke sufficient


The M365 variant is operationally heavier, and logging provides extensive detail before and after identity compromise. The Google Workspace variant is lighter (only sign-ins were observed), but default logging lacks important context.


##


Tycoon 2FA behavior detection rules


We shipped detection rules across Microsoft and Google telemetry sources covering the full attack chain: initial AiTM phish, token relay, operator console recon and device persistence.


###


Microsoft - Kit relay detection


[Entra ID Potential AiTM Sign-In via OfficeHome (Tycoon2FA)](https://github.com/elastic/detection-rules/blob/9bd94c62a54c6b3d6054e4f1d57f014538be70bf/rules/integrations/azure/initial_access_tycoon_entra_id.toml#L27) - This is a high signal detection that triggers on Auth Broker or OfficeHome sign-ins to Graph/Exchange with Node.js-style user agents (` node` ,` axios` ,` undici` ). Catches the kit relay tier's server-side token operations.


[M365 Potential AiTM UserLoggedIn via Office App (Tycoon2FA)](https://github.com/elastic/detection-rules/blob/9bd94c62a54c6b3d6054e4f1d57f014538be70bf/rules/integrations/o365/initial_access_tycoon_o365.toml#L28) - Same detection logic as the Entra sign-in rule but against the M365 Unified Audit Log for tenants ingesting` o365.audit` instead of (or in addition to) Entra sign-in logs.


[Entra ID OAuth Device Code Phishing via AiTM](https://github.com/elastic/detection-rules/blob/9bd94c62a54c6b3d6054e4f1d57f014538be70bf/rules/integrations/azure/initial_access_entra_id_oauth_device_code_phishing_tycoon_aitm.toml#L27) : Detects successful interactive device-code-flow sign-ins through the Auth Broker targeting Exchange, Graph, or SharePoint. Catches the device-code-grant abuse variant specifically.


[Entra ID Microsoft Authentication Broker Sign-In to Unusual Resource](https://github.com/elastic/detection-rules/blob/9bd94c62a54c6b3d6054e4f1d57f014538be70bf/rules/integrations/azure/initial_access_entra_id_microsoft_auth_broker_unusual_resource.toml#L28) : Detects successful Auth Broker sign-ins where the target resource is outside the commonly-observed first-party set. Catches FOCI token exchange to unexpected APIs or enterprise applications.


###


Microsoft - Persistence detection


[Entra ID Register Device with Unusual User Agent (Azure AD Join)](https://github.com/elastic/detection-rules/blob/8089df918f81850b28dfc3e691495f55bf30c94a/rules/integrations/azure/persistence_entra_id_register_device_unusual_user_agent.toml#L26) : Detects successful device registration events where the user agent is not one of the known native registration clients *(` Dsreg` ,` DeviceRegistrationClient` ,` Dalvik` )* . Catches the kit's device-PRT persistence play also originating from the axios user agent:


###


Post-compromise Graph API enumeration (ES|QL)


For the operator console tier's post-compromise recon, we built an ES|QL[rule](https://github.com/elastic/detection-rules/blob/b87d2f58938f67c3b74bf5ae334fad06371d4dac/rules/integrations/azure/discovery_graph_activity_delegated_user_multi_category_recon.toml) that tags each Microsoft Graph API request into one of five reconnaissance categories and fires when 4 or more distinct categories are hit within the aggregation window (<= 60s):


[Microsoft Graph Multi-Category Reconnaissance Burst](https://github.com/elastic/detection-rules/blob/d623721ec303064d1f47bcea15ae5bc3062f2b54/rules/integrations/azure/discovery_graph_activity_delegated_user_multi_category_recon.toml#L26) catches systematic post-compromise enumeration while filtering out organic portal usage. Normal user activity might touch one or two of these categories, hitting 4 or more distinct recon categories from a single session within a short window (33 seconds) is the automated-tooling fingerprint.


###


Google - Kit relay and persistence detection


[Google Workspace Impossible Travel Login](https://github.com/elastic/detection-rules/blob/3ad97fe42d0862ef3c7a93cd54afebfcee4eaac0/rules/integrations/google_workspace/initial_access_google_workspace_login_impossible_travel.toml) - ES|QL rule using[* st_distance()](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/spatial-functions/st_distance) * geospatial functions to detect successful sign-ins from locations implying travel faster than 800 km/h with at least 500 km separation. Catches the multi-ASN kit relay pattern where multiple IPs in different geolocations authenticate the same user within minutes:


[Google Workspace User Login from Atypical ASN](https://github.com/elastic/detection-rules/blob/696b276e628b5663a78e35c02be3d2798a318479/rules/integrations/google_workspace/initial_access_google_workspace_login_from_atypical_asn.toml) -[new term](https://www.elastic.co/docs/solutions/security/detect-and-alert/new-terms) rule that detects the first time a Google Workspace user successfully signs in from a given source ASN within a 14-day historical window.


[Google Workspace Device Registration After OAuth from Suspicious ASN](https://github.com/elastic/detection-rules/blob/8089df918f81850b28dfc3e691495f55bf30c94a/rules/integrations/google_workspace/persistence_google_workspace_device_registered_after_oauth_from_suspicious_asn.toml#L27) : EQL sequence rule detecting OAuth authorization for the Chrome client (` 77185425430.apps.googleusercontent.com` ) from cheap hosting ASN, followed within 30 seconds by a device registration with account state` REGISTERED` .


[Google Workspace Device Registration Burst for Single User](https://github.com/elastic/detection-rules/blob/0236027f5a3a675608dbec29d15dc994916e2f13/rules/integrations/google_workspace/persistence_google_workspace_device_registration_burst.toml) - Detects bursts of Google Workspace device registration events for the same user, where three or more distinct
` google_workspace.device.id` values are emitted in a one-minute window :


##


Automating containment with Elastic Workflows


Once the detection content is in place, the next gap is the time between alert and action. The Tycoon 2FA M365 kit-to-operator handoff window we documented earlier is 10-20 minutes, the time between the kit relay's first successful token issuance and the operator-tier session beginning its post-compromise Graph recon.


A manual SOC response routinely takes longer than that window, which is why the operator gets recon work done before containment lands. Closing that gap is what makes detection actionable.


[Elastic Workflows](https://www.elastic.co/docs/explore-analyze/workflows) , shipped with the stack (9.4+), lets detection rules invoke custom[workflows](https://github.com/elastic/workflows) with a YAML-defined pipeline of steps on every alert.


As a PoC, we built a custom workflow wired to an[Entra ID Potential AiTM Sign-In via OfficeHome (Tycoon2FA)](https://github.com/elastic/detection-rules/blob/9bd94c62a54c6b3d6054e4f1d57f014538be70bf/rules/integrations/azure/initial_access_tycoon_entra_id.toml#L27) detection rule that mirrors the required response actions.


On every alert the workflow:


1. Acquires a Graph bearer via` client_credentials` (one-time per execution).
2. PATCHes the compromised UPN with` accountEnabled: false` to halt new authentications.
3. Enumerates *registeredDevices* and *ownedDevices* on the user.
4. DELETEs each device principal, which is what actually invalidates a device-bound PRT.
5. POSTs to *revokeSignInSessions* to invalidate user-level refresh tokens and session cookies.
6. Opens a Kibana case populated with the alert context for post-IR audit (password reset, auth method review, OAuth grant audit).


The chain executes in less than 10 seconds end-to-end against Microsoft Graph, well inside the 10-20 minute Tycoon 2FA handoff window. The operator-tier session never gets a chance to begin recon.


The pattern scales beyond this one rule. The same workflow shape works for any cloud-identity detection that benefits from immediate containment: AiTM sign-ins, impossible travel, illicit OAuth consent grants, role escalation, MFA fatigue, anomalous device registration. Wire the rule to a workflow that calls the relevant cloud API and the SOC gets seconds-level containment.


##


Defending against Tycoon 2FA AiTM attacks


- Deploy phishing-resistant MFA: FIDO2 security keys and passkeys are the only methods immune to AiTM session theft. TOTP, SMS, and push-based MFA can all be proxied.
- Enforce device compliance via Conditional Access: Require managed, compliant devices for token issuance. This is the single most effective control against AiTM token theft.
- Block device code flows: The` Block device code flow` Conditional Access policy cleanly rejects the kit relay at the grant phase (error 53003). Enable it for all users except explicitly approved kiosk/headless scenarios.
- Enable token protection (token binding):[Binds](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-token-protection) tokens to the device they were issued to. A stolen token replayed from a different device is rejected.
- Enable Continuous Access Evaluation (CAE): Near real-time token revocation when risk conditions change.
- Enable Security Defaults in Entra ID (only for tenants without custom Conditional Access): rejects legacy authentication such as ROPC and blocks device code flow by default. Enabling Security Defaults disables custom CA policies, so this is not applicable to tenants already running granular CA.


##


MITRE ATT&CK Mapping


Technique ID Observable


Phishing: Spearphishing Link T1566.002 Lure emails with embedded links, QR codes, PDF/SVG/HTML attachments


Steal Web Session Cookie T1539 AiTM proxy captures post-MFA session tokens


Valid Accounts: Cloud Accounts T1078.004 Stolen tokens used for Graph API access and M365 web app browsing


Account Manipulation: Device Registration T1098.005 Kit registers device for PRT persistence


Use Alternate Authentication Material: Application Access Token T1550.001 FOCI token exchange across Auth Broker app family


Account Discovery: Cloud Account T1087.004 Graph enumeration of user profile, role memberships, contacts


Permission Groups Discovery: Cloud T1069.003 Enumerating directory roles and transitive role assignments


Cloud Service Discovery T1526 Listing subscribedSkus, organization metadata, app inventory


##


References


- [Microsoft Security Blog - Inside Tycoon2FA: How a leading AiTM phishing kit operated at scale](https://www.microsoft.com/en-us/security/blog/2026/03/04/inside-tycoon2fa-how-a-leading-aitm-phishing-kit-operated-at-scale/) (March 2026)
- [Tycoon 2FA Malware Analysis, Overview by ANY.RUN](https://any.run/malware-trends/tycoon/)
- [Cloudflare - Tycoon 2FA Takedown](https://www.cloudflare.com/threat-intelligence/research/report/tycoon-2fa-takedown/) (March 2026)
- [SpyCloud - Tycoon 2FA Takedown](https://spycloud.com/blog/tycoon-2fa-takedown-inside-the-global-phishing-infrastructure-disruption/) *: Inside the Global Phishing Infrastructure Disruption* (March 2026)
- [eSentire - Tycoon 2FA Operators Adopt OAuth Device Code Phishing](https://www.esentire.com/blog/tycoon-2fa-operators-adopt-oauth-device-code-phishing) (May 2026)
- [Sekoia - Tycoon 2FA: an in-depth analysis of the latest version of the AiTM phishing kit](https://blog.sekoia.io/tycoon-2fa-an-in-depth-analysis-of-the-latest-version-of-the-aitm-phishing-kit/) (March 2024)


#### Jump to section


- [How Tycoon 2FA works](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#how-tycoon-2fa-works)
- [The AiTM mechanism](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#the-aitm-mechanism)
- [Two structural variants in current rotation](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#two-structural-variants-in-current-rotation)
- [Evasion techniques](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#evasion-techniques)
- [Microsoft 365 / Entra ID](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#microsoft-365--entra-id)
- [A two-tier operational architecture](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#a-two-tier-operational-architecture)
- [Post-compromise Graph API enumeration](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#post-compromise-graph-api-enumeration)
- [Device-PRT persistence](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#device-prt-persistence)
- [Why doesn't revoking sessions stop Tycoon 2FA?](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#why-doesnt-revoking-sessions-stop-tycoon-2fa)
- [Detection nuances - Microsoft](https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering#detection-nuances---microsoft)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Detecting%20Tycoon%202FA%20AiTM%20attacks%20across%20Entra%20ID%20and%20Google%20Workspace&url=https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering&title=Detecting%20Tycoon%202FA%20AiTM%20attacks%20across%20Entra%20ID%20and%20Google%20Workspace)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/tycoon-2fa-aitm-detection-engineering&title=Detecting%20Tycoon%202FA%20AiTM%20attacks%20across%20Entra%20ID%20and%20Google%20Workspace)
