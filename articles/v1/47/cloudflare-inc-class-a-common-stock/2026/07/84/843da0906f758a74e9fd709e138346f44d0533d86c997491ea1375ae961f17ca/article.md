---
schema_version: "1.0.0"
document_id: "843da0906f758a74e9fd709e138346f44d0533d86c997491ea1375ae961f17ca"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/post-quantum-authentication-to-origins/"
published_at: "2026-07-29T13:00:00+00:00"
first_seen_at: "2026-07-29T15:25:06.315081+00:00"
fetched_at: "2026-07-29T15:25:07.276396+00:00"
content_hash: "sha256:049ede442b929c17a2f903414a7b9ccc47a4875a983d2c9d3553349143bb516e"
---

# Post-quantum authentication to origins is now supported

Cloudflare's Authenticated Origin Pulls and Custom Origin Trust Store now support post-quantum authentication.


Here we’ll explain how you can configure fully post-quantum secure mutually authenticated TLS connections to your origin server, dive into the engineering details of how we built it, make a shameful confession, and finally explain how this work fits into our overall post-quantum migration roadmap.


## Reaching a major milestone


Our focus for the past several years has been in deploying post-quantum[encryption](https://radar.cloudflare.com/post-quantum) to protect against[harvest-now/decrypt-later](https://en.wikipedia.org/wiki/Harvest_now,_decrypt_later) attacks, where an attacker quietly stockpiles your encrypted data with the hope of decrypting it in the future with a quantum computer.


However, recent breakthroughs in quantum computing and cryptanalysis pulled the timelines for upgrading to post-quantum cryptography forward[across](https://www.microsoft.com/en-us/security/blog/2026/06/30/microsoft-advances-quantum-safe-security-as-the-risk-timeline-shifts/)[industry](https://blog.google/innovation-and-ai/technology/safety-security/cryptography-migration-timeline/) and[government](https://blog.cloudflare.com/post-quantum-eo-2026/) and have caused us to shift our attention to deploying post-quantum *authentication* , to protect against attackers who will soon be able to use quantum computers to break classical credentials and carry out impersonation attacks.


In a previous post, we announced that Cloudflare is[targeting 2029](https://blog.cloudflare.com/post-quantum-roadmap/#cloudflares-roadmap-to-full-post-quantum-security) for full post-quantum security, and laid out several milestones to hit along the way. We have reached the first of those milestones: our[Authenticated Origin Pulls](https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/) and[Custom Origin Trust Store](https://developers.cloudflare.com/ssl/origin-configuration/custom-origin-trust-store/) products[now support post-quantum (PQ) authentication](https://developers.cloudflare.com/changelog/post/2026-06-17-pqc-mldsa-aop-cots/) via Module-Lattice-Based Digital Signature Algorithm (ML-DSA) signatures to protect connections between Cloudflare and customer origin servers.


## The origin connection is different


When a client visits a website proxied by Cloudflare, there are typically two connections involved. The first connection is from the visitor (e.g., a browser) to Cloudflare. If the request can be served from Cloudflare’s cache or triggers any blocking rules, Cloudflare might respond directly. Otherwise, Cloudflare establishes a second connection to the customer’s origin server to fetch the requested content, so it can respond to the original request.


Two connections: visitor-to-Cloudflare and Cloudflare-to-origin.


Protecting sensitive visitor data requires both of these connections to be secure against quantum attacks. We enabled post-quantum encryption support for both the visitor-to-Cloudflare (Connection 1) and Cloudflare-to-origin (Connection 2) connections in[2022](https://blog.cloudflare.com/post-quantum-for-all/) and[2023](https://blog.cloudflare.com/post-quantum-to-origins/) , respectively, and already see[significant usage](https://radar.cloudflare.com/post-quantum) .


We are actively working on completing the picture with post-quantum authentication. For the visitor-to-Cloudflare connection, we are collaborating with[Google](https://blog.google/security/cultivating-a-robust-and-efficient-quantum-safe-https/) and others at the Internet Engineering Task Force ([IETF](https://datatracker.ietf.org/wg/plants/about/) ) to develop and[experiment](https://blog.cloudflare.com/bootstrap-mtc) with Merkle Tree Certificates[(MTC)](https://datatracker.ietf.org/doc/draft-ietf-plants-merkle-tree-certs/) , a design for fast, post-quantum certificates for the web, with initial deployments targeting 2027. The topic of this post, however, is the Cloudflare-to-origin connection, where the requirements for authentication differ from that of the visitor-to-Cloudflare connection in several important ways.


For this connection, Cloudflare is the client. This gives us the control to employ techniques such as connection pooling to fan in requests from all over our network to a smaller set of connections to origin servers, amortizing the overhead of connection setup over many requests. This makes the cost of “drop-in” post-quantum signatures more palatable, and the performance benefits of MTC less necessary.


And with a pre-existing trust relationship between Cloudflare and customers (i.e., a Cloudflare account), we need not tie ourselves to the constraints and timelines of the public key infrastructure (PKI) for the public Internet ([WebPKI](https://cabforum.org/working-groups/server/baseline-requirements/requirements/) ) and can instead use custom PKIs tailored to the use case, without overhead from intermediate certificates and[Certificate Transparency](https://datatracker.ietf.org/doc/html/rfc6962) that may not be applicable. Solutions like[Cloudflare Tunnel](https://developers.cloudflare.com/tunnel/) can also be used to protect the Cloudflare-to-origin connection without upgrading legacy origin systems, by forwarding traffic over a tunnel secured with post-quantum encryption (and post-quantum authentication in the works).


All this to say, the unique requirements of the Cloudflare-to-origin connection have allowed us to deploy post-quantum authentication via ML-DSA authentication ahead of support landing in the WebPKI for the public Internet. (For customers who stick with the WebPKI, don’t worry: we’ll add MTC support on the Cloudflare-to-origin connection in the future.)


So how do you turn this on? Let’s dive into the configuration.


## Configuring fully PQ-secure origin connections


We have added ML-DSA support (for all[FIPS 204](https://csrc.nist.gov/pubs/fips/204/final) parameter sets: ML-DSA-44, ML-DSA-65, and ML-DSA-87) to the Custom Origin Trust Store and Authenticated Origin Pulls products. ML-DSA-44 is our recommendation for most applications as it is the most performant option and attains a comfortable NIST[category 2](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.204.pdf#page=25) security strength.


### Custom Origin Trust Store


When Cloudflare makes a connection to a customer origin server configured with[Full (strict)](https://developers.cloudflare.com/ssl/origin-configuration/ssl-modes/full-strict/) SSL mode, we authenticate the origin certificate against a default trust store consisting of all[commonly trusted](https://ccadb.org/) Certificate Authorities (CAs) as well as Cloudflare’s[origin CA](https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/) . The[Custom Origin Trust Store (COTS)](https://developers.cloudflare.com/ssl/origin-configuration/custom-origin-trust-store/) product (which requires[Advanced Certificate Manager](https://developers.cloudflare.com/ssl/edge-certificates/advanced-certificate-manager/) to be enabled) allows customers to replace this default trust store with a set of CAs they control. COTS now allows customers to upload ML-DSA CAs, such that Cloudflare will trust any origin server certificate chaining to that CA when connecting to the origin.


### Authenticated Origin Pulls


To limit abuse and resource consumption on their origin servers, customers may want to only serve requests coming from Cloudflare’s servers.[Authenticated Origin Pulls (AOP)](https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/) can be used to configure Cloudflare to present a client certificate to the origin server in order to establish a[mutual TLS (mTLS)](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/) connection, in which communication between the parties is bidirectionally secure and trusted. AOP is available for free on all Cloudflare plan levels.


AOP supports three[configuration levels](https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/#configuration-levels) : global, per-zone, and per-hostname. The per-zone and per-hostname configuration levels now allow customers to upload ML-DSA certificates and private keys (in the FIPS 204 seed format), so that Cloudflare’s TLS client will present this certificate to authenticate itself when connecting to the origin server. (Don’t worry, we haven’t forgotten about the global configuration level — it just happens to be a more involved change that will be prioritized at a later date.)


### Avoiding downgrades


Adding post-quantum encryption and authentication support to both the authenticating and verifying parties is necessary but *not* sufficient for full post-quantum security. The pesky issue of downgrades remains. If the verifying party supports any quantum-vulnerable authentication mechanisms, they remain open to attack from an[on-path attacker](https://www.cloudflare.com/learning/security/threats/on-path-attack/) capable of forging classical credentials.


The fix: the verifying party must remove trust in quantum-vulnerable authentication mechanisms. (This is more nuanced in complex PKIs. For example, see the Chromium Security team’s[four-stage plan](https://www.chromium.org/Home/chromium-security/post-quantum-auth-roadmap/) for transitioning the Web.) See the[configuration guide](https://developers.cloudflare.com/ssl/post-quantum-cryptography/pqc-to-origin/#avoid-downgrades) for AOP and COTS for details on how to ensure your origin is secure against downgrade attacks.


### Quick start


The walkthrough below shows how to generate an ML-DSA certificate chain and configure both products via the Cloudflare API. For dashboard instructions and additional context, refer to the[developer docs](https://developers.cloudflare.com/ssl/post-quantum-cryptography/pqc-to-origin/) .


1. Generate certificates


You will need OpenSSL 3.5.0 or later. The private key must be generated in the FIPS 204 seed-only encoding, which is the only format Cloudflare currently accepts on upload.


**Origin server certificate chain for COTS:**


```text
# Create a private ML-DSA-44 CA for the origin server
openssl   genpkey   -algorithm   mldsa44   \
-provparam   ml-dsa.output_formats=seed-only   \
-out   origin-ca.key


openssl   req   -new   -x509   -key   origin-ca.key   \
-out   origin-ca.crt   -days   10950   \
-subj   "/CN=Origin Server CA"


# Create the origin server certificate (signed by the origin CA)
openssl   genpkey   -algorithm   mldsa44   \
-provparam   ml-dsa.output_formats=seed-only   \
-out   origin-server.key


openssl   req   -new   -key   origin-server.key   \
-out   origin-server.csr   \
-subj   "/CN=origin.example.com"


openssl   x509   -req   -in   origin-server.csr   \
-CA   origin-ca.crt   -CAkey   origin-ca.key   -CAcreateserial   \
-out   origin-server.crt   -days   5475   \
-extfile   <(  printf   "basicConstraints=CA:FALSE\nkeyUsage=digitalSignature\nsubjectAltName=DNS:origin.example.com\n")


```


**Cloudflare client certificate chain for AOP:**


```text
# Create a private ML-DSA-44 CA for Authenticated Origin Pulls
openssl   genpkey   -algorithm   mldsa44   \
-provparam   ml-dsa.output_formats=seed-only   \
-out   aop-ca.key


openssl   req   -new   -x509   -key   aop-ca.key   \
-out   aop-ca.crt   -days   10950   \
-subj   "/CN=Authenticated Origin Pull CA"


# Create the client certificate Cloudflare will present (signed by the AOP CA)
openssl   genpkey   -algorithm   mldsa44   \
-provparam   ml-dsa.output_formats=seed-only   \
-out   aop-client.key


openssl   req   -new   -key   aop-client.key   \
-out   aop-client.csr   \
-subj   "/CN=cloudflare-aop-client"


openssl   x509   -req   -in   aop-client.csr   \
-CA   aop-ca.crt   -CAkey   aop-ca.key   -CAcreateserial   \
-out   aop-client.crt   -days   5475   \
-extfile   <(  printf   "basicConstraints=CA:FALSE\nkeyUsage=digitalSignature\n")


```


2. Upload the origin CA to Custom Origin Trust Store


```text
CA_CERT  =  $(  jq   -Rs   .   <   origin-ca.crt  )


curl   "https://api.cloudflare.com/client/v4/zones/  $ZONE_ID  /acm/custom_trust_store"   \
--header   "Authorization: Bearer   $CLOUDFLARE_API_TOKEN  "   \
--header   "Content-Type: application/json"   \
--json   "{  \"  certificate  \"  :   $CA_CERT  }"


```


*Uploading a COTS CA replaces the default publicly-trusted CAs for the zone. Make sure you only upload post-quantum CAs if you want to avoid downgrade attacks.*


3. Upload the client certificate for Authenticated Origin Pulls


The example below uses zone-level AOP. If you prefer per-hostname AOP, use the` /origin_tls_client_auth/hostnames/certificates` endpoint instead.


```text
CERT  =  $(  jq   -Rs   .   <   aop-client.crt  )
KEY  =  $(  jq   -Rs   .   <   aop-client.key  )


# Upload the ML-DSA client certificate
curl   "https://api.cloudflare.com/client/v4/zones/  $ZONE_ID  /origin_tls_client_auth"   \
--header   "Authorization: Bearer   $CLOUDFLARE_API_TOKEN  "   \
--header   "Content-Type: application/json"   \
--json   "{  \"  certificate  \"  :   $CERT  ,   \"  private_key  \"  :   $KEY  }"


# Enable zone-level Authenticated Origin Pulls
curl   -X   PUT   "https://api.cloudflare.com/client/v4/zones/  $ZONE_ID  /origin_tls_client_auth/settings"   \
--header   "Authorization: Bearer   $CLOUDFLARE_API_TOKEN  "   \
--header   "Content-Type: application/json"   \
--json   '{"enabled": true}'


```


4. Set your SSL/TLS mode to Full (strict)


Custom Origin Trust Store is only active when your zone is using **Full (strict)** mode. If you are using AOP without COTS, **Full** or higher is sufficient.


```text
curl   -X   PATCH   "https://api.cloudflare.com/client/v4/zones/  $ZONE_ID  /settings/ssl"   \
--header   "Authorization: Bearer   $CLOUDFLARE_API_TOKEN  "   \
--header   "Content-Type: application/json"   \
--json   '{"value": "strict"}'


```


5. Configure your origin server (on NGINX)


If you are using COTS (your origin presents the ML-DSA server certificate):


```text
server {
listen 443 ssl;
ssl_certificate     /etc/ssl/origin-server.crt;
ssl_certificate_key /etc/ssl/origin-server.key;
ssl_protocols       TLSv1.3;
}


```


If you are using AOP (your origin verifies Cloudflare's client certificate):


```text
server   {
listen   443   ssl  ;
ssl_client_certificate   /etc/ssl/aop-ca.crt  ;
ssl_verify_client        on  ;
}
```


If you are using both together (recommended for full post-quantum mutual TLS):


```text
server   {
listen   443   ssl  ;
ssl_certificate          /etc/ssl/origin-server.crt  ;
ssl_certificate_key      /etc/ssl/origin-server.key  ;
ssl_client_certificate   /etc/ssl/aop-ca.crt  ;
ssl_verify_client        on  ;
ssl_protocols            TLSv1.3  ;
}
```


6. Verify the post-quantum handshake


The TLS handshake between Cloudflare and your origin happens behind the scenes, so you cannot observe it directly by connecting to your proxied hostname from the outside. Instead, verify each side separately.


**Verify COTS (origin presents an ML-DSA certificate):**


If your origin IP is directly reachable (for example, during testing before enabling the Cloudflare proxy), connect to the origin IP directly and validate the certificate:


```text
openssl   s_client   -connect   <  ORIGIN_I  P  >  :443   \
-servername   origin.example.com   \
-CAfile   origin-ca.crt   \
-brief
```


Look for` Signature type: mldsa44` in the output.


If your origin is firewalled to only accept Cloudflare IPs, check your origin server's TLS logs or use a packet capture tool such as` ssldump` or` tcpdump` on the origin to confirm that Cloudflare negotiated TLS 1.3 with the ML-DSA certificate.


**Verify AOP (Cloudflare presents a client certificate):**


Confirm that direct connections to the origin (without a valid client certificate) are rejected:


```text
openssl   s_client   -connect   <  ORIGIN_I  P  >  :443   \
-servername   origin.example.com   \
-brief
```


With` ssl_verify_client` on enforced, this should fail with an SSL alert.


**Verify the full Cloudflare-to-origin path:**


Because the mTLS handshake happens server-to-server, the most reliable way to confirm that Cloudflare is presenting the ML-DSA client certificate is to inspect your origin server logs. For example, in NGINX you can log the client certificate serial number or subject:


```text
log_format   pq_verify   '$remote_addr - $ssl_client_serial $ssl_client_s_dn'  ;
access_log   /var/log/nginx/pq-verify.log   pq_verify  ;
```


After sending a request through Cloudflare, check the log. You should see the serial number of the` aop-client.crt` certificate you uploaded.


For the key agreement, ensure that your origin's TLS library supports` X25519MLKEM768` and that it is preferred in your configuration. The post-quantum key agreement will be visible in origin server logs or packet captures as the negotiated group.


## The boring details


Implementing this feature involved two primary systems: our control plane service that allows customers to manage their TLS settings and upload certificates, and the data plane service responsible for establishing TLS connections to origin servers based on customer configurations.


### Control plane


As with many other services that power Cloudflare’s APIs and Dashboard, the service that powers the configuration for Cloudflare’s[SSL/TLS products](https://developers.cloudflare.com/ssl/) runs in a[highly available setup](https://blog.cloudflare.com/major-data-center-power-failure-again-cloudflare-code-orange-tested/) across a set of critical data centers. The service is responsible for handling SSL/TLS settings updates and pushing them out to our[globally-distributed key-value store](https://blog.cloudflare.com/quicksilver-v2-evolution-of-a-globally-distributed-key-value-store-part-1/) so that they are available to data plane services when handling live requests.


Enabling ML-DSA support for AOP and COTS required updating this service to support parsing and validating ML-DSA certificates. This sounds simple on paper, but there’s a catch: the service is written in Go, but Go’s standard X.509 and TLS libraries did not yet support ML-DSA. We instead implemented the[necessary functionality](https://github.com/cloudflare/circl/pull/559) in Cloudflare’s[CIRCL](https://blog.cloudflare.com/introducing-circl/)[library](https://github.com/cloudflare/circl) to patch in support. This was a relatively simple change, but repeating this for every service that needs post-quantum authentication support would be a major chore.


Fortunately,[Go 1.27](https://go.dev/doc/go1.27) (expected August 2026) will include native ML-DSA support, and will allow us to drop the CIRCL dependency. Other Go-based services will then be able to seamlessly pull in ML-DSA support with a simple version update.


### Data plane


With the control plane changes in place, customers could then upload ML-DSA certificates for the AOP and COTS products. The next step was to update our data plane service responsible for interacting with customer origins to actually *use* those certificates.


We have talked in previous blog posts about our open-source proxy framework[Pingora](https://blog.cloudflare.com/pingora-open-source/) and specifically how we have a Pingora-based service that handles all the connections to those origins. That service is unimaginatively named[Pingora Origin](https://blog.cloudflare.com/pingora-saving-compute-1-percent-at-a-time/#motivation) , and it is responsible for ensuring millions of requests per second worth of origin-bound requests make it safely and securely to their final destination.


The task of ensuring the request’s security typically falls to the TLS provider, and it may surprise you to know that post-quantum security (or in this case authenticity) is no different. It also might come as a letdown that defending against quantum attacks[does not require](https://blog.cloudflare.com/you-dont-need-quantum-hardware) exotic states of matter with lasers and superconducting[Josephson junctions](https://en.wikipedia.org/wiki/Josephson_junction) ; all you need is an update to[BoringSSL](https://github.com/google/boringssl) . Now, BoringSSL lives up to its name: over the past several years, there have been no CVEs or major changes. In fact, we relied on that stability so heavily that we have an admission to make: we snoozed Pingora Origin’s update to BoringSSL for four years, instead maintaining an internal fork to patch in additional functionality as needed. That has worked well, but when post-quantum authentication support[landed in BoringSSL](https://github.com/google/boringssl/commit/4a3cda40b965bbda7cebf86e35c1ed6890ebcc34) in April 2026, we decided that this update was worth the inconvenience.


This is where we wish we could say, “This update went perfectly. No notes!” but naturally there were some hiccups. Within the four years’ worth of code changes was[this commit](https://github.com/google/boringssl/commit/cee2dbb08cb3daf56995875207b5e947310697de#diff-fab9b7680cab08ae895d70161c48cb7fde3af78110c4b5641a7c6a3e0b1f441aL706) that enables enforcement of rules related to[KeyUsage](https://datatracker.ietf.org/doc/html/rfc5280#section-4.2.1.3) in TLS certificates. This change is in line with the specifications, but as we have seen before, the Internet is not known for being[RFC compliant](https://github.com/cloudflare/boring/pull/495) . The result was that even after testing the changes for weeks and a very slow release rollout looking for just this sort of regression, a small number of customers’ certificates were deemed invalid after the change, leading to an[incident on June 10, 2026](https://www.cloudflarestatus.com/incidents/mwj6mb927p3g) . We quickly rolled back the change and after a[patch](https://github.com/cloudflare/boring/pull/512) to retain support for RSA certificates with technically invalid[KeyUsage](https://datatracker.ietf.org/doc/html/rfc5280) , fully post-quantum secure TLS to origins is now live and ready to use.


## We are only getting started


ML-DSA support is increasingly ubiquitous across[TLS libraries](https://developers.cloudflare.com/ssl/post-quantum-cryptography/pqc-support/#libraries) , and routine software updates will bring post-quantum authentication support to many applications. (Please keep your libraries updated!) The highly-anticipated[Go 1.27](https://go.dev/doc/go1.27) (August 2026) will come with native ML-DSA support, allowing Go-based services to add post-quantum authentication with a simple version update.


As these changes propagate across the ecosystem, we will be upgrading our systems as well. See[PQC in Cloudflare Products](https://developers.cloudflare.com/ssl/post-quantum-cryptography/pqc-cloudflare-products/) for an up-to-date tracker of post-quantum encryption and authentication support in Cloudflare products and services.
