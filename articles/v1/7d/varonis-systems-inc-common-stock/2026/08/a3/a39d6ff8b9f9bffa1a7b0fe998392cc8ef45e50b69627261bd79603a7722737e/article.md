---
schema_version: "1.0.0"
document_id: "a39d6ff8b9f9bffa1a7b0fe998392cc8ef45e50b69627261bd79603a7722737e"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/ws-trust-autologon-endpoint"
published_at: "2026-08-12T13:00:00+00:00"
first_seen_at: "2026-08-12T13:19:55.074676+00:00"
fetched_at: "2026-08-12T13:19:56.135971+00:00"
content_hash: "sha256:55ef7ce805882c2876c36b6208919d1cf2adb16941dd76d11018e4e16b2769c0"
---

# WS-Trust Autologon Endpoint: Password Spray Without Smart Lockout Blocking

A legacy Entra ID endpoint kept alive for Office 2013 clients lets attackers spray passwords past Smart Lockout, confirm valid credentials on MFA-protected accounts, and leave only partial logs behind.


In 2018, Microsoft introduced[Smart Lockout](https://learn.microsoft.com/en-us/entra/identity/authentication/howto-password-smart-lockout) into Azure AD, now Entra ID, to make password spraying harder.


The control was meant to stop attackers from testing passwords indefinitely by locking accounts after repeated failed attempts from unfamiliar locations, while defenders watched the pattern through sign-in logs and relied on MFA or Conditional Access to block access if a password was guessed.


The problem is that Entra ID still has older authentication paths that were built for a different era of Microsoft identity. One of them is the WS-Trust autologon endpoint used by[Entra Seamless SSO](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-sso) , which exists to help domain-joined machines sign users into Microsoft 365 in the background.


Direct username and password requests sent to that endpoint from the internet bypass Smart Lockout, stay out of the standard sign-in logs, and return enough information to confirm valid passwords even when MFA or Conditional Access stopped the final sign-in.


## **Finding the autologon path**


The endpoint sits on top of WS-Trust, an old SOAP-based authentication protocol originally built for username and password authentication against federation services. Microsoft keeps a dedicated WS-Trust path for Entra Seamless SSO at:


https://autologon.microsoftazuread-sso.com/{tenant}/winauth/trust/2005/usernamemixed


The usernamemixed endpoint was designed for a legacy sign-in flow. A domain-joined machine can use it as part of the background process that signs a user into Microsoft 365 without asking for credentials again.


The problem comes from direct reachability. A request can send a username and password in a SOAP envelope, and the endpoint will return a structured response describing what happened.


The important part of the request is the UsernameToken element in the SOAP security header.


```text
<wsse:UsernameToken>
<wsse:Username>user@contoso.com</wsse:Username>
<wsse:Password>{password}</wsse:Password>
</wsse:UsernameToken>
```


If authentication succeeds, the response contains a DesktopSsoToken.


```text
<wst:RequestSecurityTokenResponse>
<wst:RequestedSecurityToken>
<DesktopSsoToken>eyJhbGciOi...{JWT}</DesktopSsoToken>
</wst:RequestedSecurityToken>
</wst:RequestSecurityTokenResponse>
```


If authentication fails, the response returns a SOAP Fault containing an AADSTS error code.


```text
<s:Fault>
<s:Reason>
<s:Text xml:lang="en-US">
AADSTS50126: Error validating credentials due to invalid username or password.
</s:Text>
</s:Reason>
</s:Fault>
```


That error code is what turns a failed sign-in attempt into useful information.


## **Where the response leaks too much**


The endpoint returns different AADSTS codes for different account states.


AADSTS50126


Meaning


What it tells the attacker


(no error)


Authentication succeded


Password is correct; sign-on token returned


AADSTS50126


Invalid credentials


Password is wrong


AADSTS50034


User does not exist


Account does not exist in the tenant


AADSTS50053


Account is locked


Account exists; currently locked out


AADSTS50057


Account is disabled


Account exists; disabled by an administrator


AADSTS50055


Password has expired


**Password is correct; account requires reset**


AADSTS50076


MFA required


**Password is correct; MFA blocks completion**


AADSTS53003


Blocked by Conditional Access


**Password is correct; Conditional Access blocks completion**


The important cases are **AADSTS50055** , **AADSTS50076** , and **AADSTS53003** .


Those responses mean the password was accepted before another control stopped the flow. MFA or Conditional Access may still prevent the attacker from completing the sign-in, but the endpoint has already confirmed that the password itself is valid.


That changes the value of a failed login.


In a normal MFA-protected sign-in path, an attacker wants to know whether the password worked. A generic failure gives them very little. With this endpoint, the response separates a wrong password from a correct password blocked later in the flow.


## **Where Smart Lockout falls away**


Smart Lockout is Microsoft’s main control for slowing password spray and brute-force attempts against Entra ID accounts. It tracks failed authentication attempts per account, using separate familiar and unfamiliar location counters. Once the threshold is crossed, the account should temporarily lock, and later failed attempts should extend the lockout period further.


Through the WS-Trust autologon endpoint, I saw a different result.


A password spray against username mixed produced more than 1,000 failed attempts against the same account from a single unfamiliar IP, with no AADSTS50053 locked-account response. There was no obvious exponential backoff or response-time throttling, and a later legitimate sign-in to the same account succeeded immediately.


The reason this matters is that Entra ID handles interactive and non-interactive authentication differently. Interactive logons are the normal user-driven sign-ins most defenders already monitor through standard sign-in logs. Non-interactive logons are background authentication flows, including legacy protocol access through paths such as WS-Trust.


Autologon sits in that second category.


The request still carries a username and password, but Entra ID processes it through a legacy background path built for older Office clients rather than the normal browser or application sign-in flow. In testing, that difference showed up in the exact places defenders care about: Smart Lockout never returned the expected lockout response, and the failed attempts stayed out of the standard sign-in logs.


Most security teams start with Entra ID sign-in logs when they investigate password spraying. Through this endpoint, much of that signal is missing.


## **How this differs from the other paths**


Every legacy protocol deserves attention, but this endpoint sits in a particularly awkward gap. It’s reachable from the internet, partially visible in standard logs, outside the expected Smart Lockout behavior, and detailed enough to confirm passwords. The behavior was specific to the autologon path in my testing.


Protocol


Endpoint


Standard sign-in logs


Smart Lockout


MFA gate


WS-Trust autologon


autologon.microsoftazuread-sso.com /usernamemixed


Partial visibility only


Not observed during testing


Yes


ActiveSync


outlook.office365.com


Yes


Enforced


Yes


ROPC


login.microsoftonline.com /oauth2/token


Yes


Enforced


No


## **What the spray looks like in practice**


At a high level, the end-to-end attack chain runs in five steps.


### **Step 1: Enumerate users**


Microsoft 365 exposes an[API](http://login.microsoftonline.com/common/GetCredentialType) that returns whether a given email address belongs to a real user in the directory. The IfExistsResult field reveals whether the user exists, with 0 meaning the user exists and 1 meaning the user was not found. The autologon endpoint itself works as a fallback when the API is rate-limited: AADSTS50034 means the user does not exist, while most other responses mean they do.


### **Step 2: Filter the list**


Keep only the accounts that exist and discard the rest. Spraying non-existent accounts wastes requests, creates noise, and increases the chance of rate-limiting on the GetCredentialType API.


### **Step 3: Send the spray**


Pick one common password or a company-themed guess and send it against every account in the filtered list. Smart Lockout does not fire. Standard sign-in events do not appear in the tenant’s logs. The defender sees little while every account in the list is being tested.


### **Step 4: Triage the responses**


Classify each response by AADSTS code. VALID accounts have confirmed credentials and immediate access. MFA_REQUIRED accounts have a confirmed valid password where MFA blocks interactive sign-in, which is still valuable for token-theft chains. EXPIRED_PW accounts confirm a correct-but-expired password. CA_BLOCKED accounts confirm a valid password where Conditional Access denies the flow.


### **Step 5: Post-exploitation**


For VALID accounts without MFA, the attacker has direct access through OAuth2 or Microsoft Graph. For MFA_REQUIRED accounts, downstream techniques such as adversary-in-the-middle phishing, device code flow abuse, or primary refresh token theft can bypass the MFA requirement and complete the sign-in.


## **The overall security implications**


The practical result is a quieter password spray path with higher-quality output.


An attacker can test passwords without creating the standard lockout and logging signals defenders expect from Entra ID authentication attempts. They can also confirm valid passwords on MFA-protected accounts, which is the part that changes the value of the attack.


Many organizations treat MFA as the point where password exposure becomes less urgent, but a confirmed password still has value. It can support[adversary-in-the-middle phishing](https://www.varonis.com/blog/sessionshark?hsLang=en) , device-code social engineering, session theft workflows, primary refresh token theft, helpdesk pretexting, password reuse attacks against other systems, and more targeted follow-on access attempts.


Instead of only finding accounts with no MFA, the attacker can identify accounts where the password is correct but another policy stops the final sign-in.


Explore more research from **Varonis Threat Labs.**


[Learn more](https://www.varonis.com/varonis-threat-labs?hsLang=en)


## **How defenders can close the gap**


There are four ways to close the gap, ordered by how much they help.


The simplest is to **disable the usernamemixed endpoint** at the tenant level, which turns off WS-Trust authentication entirely. The only reason to keep it on is Office 2013 clients running versions older than the May 2015 update, so for most tenants this is a one-policy fix that closes the entire vector.


The highest-leverage move available is to **block legacy authentication** through Conditional Access, because Microsoft's standard policy template covers WS-Trust along with ROPC, ActiveSync, and the rest of the legacy surface. That kills this attack and several related ones at the same time.


Defenders who cannot close the endpoint immediately should at least be able to see attacks against it. Standard Entra ID sign-in logs do not capture failed authentications against autologon, but the Unified Audit Log does, so **enabling UAL alerts** for requests to autologon.microsoftazuread-sso.com makes the spray visible.


The long-term fix is to **move to passwordless** . FIDO2 keys and Windows Hello remove the password from the equation entirely; there is no password to spray and nothing for the endpoint to confirm. Rollout is gradual in most tenants, but every account moved off passwords removes a target for this attack and dozens of other password-based attacks against Entra ID.


## **What “by design” actually costs**


This research builds on earlier work by other identity security researchers.


Dr. Nestori Syynimaa[documented](https://aadinternals.com/post/desktopsso/) the Seamless SSO user-enumeration angle in 2019. Secureworks Counter Threat Unit later[documented](https://www.secureworks.com/research/undetected-azure-active-directory-brute-force-attacks) the autologon logging gap in 2021, and Microsoft classified the behavior as “by design.” Tools such as[AADInternals](https://github.com/Gerenios/AADInternals) and[MSOLSpray](https://github.com/dafthack/MSOLSpray) have also automated pieces of the broader Microsoft cloud enumeration and spraying workflow.


Those references gave me the starting point. What I wanted to understand was what still worked, how the endpoint behaved against Smart Lockout, and whether the response leaked anything useful after MFA or Conditional Access entered the flow. The answer was worse than a visibility gap.


During testing, the endpoint still behaved differently from normal sign-in paths, failed sprays stayed out of the standard sign-in logs, Smart Lockout never produced the expected lockout response, and the AADSTS responses could still confirm valid passwords even where MFA or Conditional Access stopped the final sign-in.


That is the real cost of “by design.” The trade-off may protect a small set of legacy clients, but it leaves defenders with a password spray path that is quieter than the controls suggest and more informative than a failed login should be.


Smart Lockout was meant to make password spraying expensive. On this one Microsoft endpoint, it’s still free.
