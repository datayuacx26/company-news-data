---
schema_version: "1.0.0"
document_id: "da20082410b4a48d992b9bec68d2960cf085ad68efc7ac351009f0b1a8c3153d"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/oauth-app-workload-identity-droplets"
published_at: "2025-10-22T18:51:32.229+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:57bc9a5d7cafd12fc4083528cefa4900fc30f16fc752242398a4b91da6394282"
---

# OAuth App Based Workload Identity for Droplets

This post is the first entry in a three part series on[workload identity federation](https://www.ietf.org/archive/id/draft-ietf-wimse-workload-identity-practices-03.html) :


-


Part 1: Architecture (this post)


-


[Part 2: Deployment and Configuration](https://www.digitalocean.com/community/tutorials/oauth-workload-identity-federation-digitalocean-droplets-part2)


-


[Part 3: Usage from Droplets and GitHub Actions](https://www.digitalocean.com/community/tutorials/oauth-workload-identity-federation-digitalocean-droplets-part3)


This entry will cover what workload identity federation is and how it can be implemented leveraging DigitalOcean’s OAuth API. In the following entries in this series, we’ll deploy an[open source Proof of Concept (PoC)](https://github.com/digitalocean-labs/droplet-oidc-poc) , configure roles and policies for workload identity access control, spin up a Droplet, write a GitHub Actions workflow, and access databases and Spaces keys from them using their respective workload identity tokens.


## What is workload identity federation?


Workload identity is used to reduce the amount of secrets involved in deploying and administrating software systems. Instead of authentication being done based on something a workload knows, for example passwords or API tokens, authentication is done based on what the workload *is* . The heart of workload identity federation is asymmetric cryptography. By leveraging public / private key pairs, tokens can be issued to workloads, such as Droplets, and used for authentication and authorization to APIs exposed by resource servers. Workload identity tokens are exchanged for domain specific access tokens, or grant access to resources directly.


This series showcases how we can use DigitalOcean’s OAuth API and fine grained permission scopes to implement and leverage workload identity federation using OpenID Connect (OIDC) protocol tokens. We’ll enable secretless access to DigitalOcean hosted databases and Spaces buckets from Droplets and GitHub Actions workflows. Eliminating the need to provision static, long-lived credentials for databases and Spaces buckets for those environments.


Workload identity RBAC


## Security properties


Authentication based on what the workload *is* requires that the infrastructure orchestrating the workload be able to make verifiable claims about a workload’s properties. To do this, the infrastructure responsible for running the workload enables issuance of workload-specific tokens containing these claims. The security of workload identity hinges on proper validation of these tokens per the OIDC protocol specification, the claims defined by each token, and the RBAC configuration used to validate tokens based on their claims.


OIDC tokens are JSON Web Tokens (JWTs). These tokens are cryptographicly signed using a private key. The private key’s corresponding public key is made available via the JSON Web Key (JWK) format at a RFC 8615 “well known URI”. The URI of the public key is then referenced by a JSON format OpenID Configuration which is hosted at another well known URI.


#### /.well-known/openid-configuration


```text
{


"issuer"  :    "https://<deployment-name>.ondigitalocean.app"  ,


"jwks_uri"  :    "https://<deployment-name>.ondigitalocean.app/.well-known/jwks"  ,


"response_types_supported"  :    [  "id_token"  ]  ,


"claims_supported"  :    [  "sub"  ,    "aud"  ,    "exp"  ,    "iat"  ,    "iss"  ]  ,


"id_token_signing_alg_values_supported"  :    [  "RS256"  ]  ,


"scopes_supported"  :    [  "openid"  ]  ,


}


```


#### /.well-known/jwks


```text
{


"keys"  :    [


{


"kty"  :    "RSA"  ,


"n"  :    "jplR2Q2_hJeA0tqAMqRppJxu16H8i8nrSgX..."  ,


"e"  :    "AQAB"  ,


"use"  :    "sig"  ,


"kid"  :    "g1uTyq-nvRAVGYg6doHZ7LJVuNznJ1QX6OxVebUX6eE"


}


]


}


```


These well known URIs are what make workload identity federation possible. Federation in this context means distinct services are interoperable for the purposes of confirming the identity of a workload by services other than the issuer of the token. Later in our blog post, we’ll leverage this interoperability to verify validity of OIDC tokens issued to workflows run by GitHub Actions, in that case, the workflow is the workload. GitHub Action’s relevant well known URIs are as follows:


-


[https://token.actions.githubusercontent.com/.well-known/openid-configuration](https://token.actions.githubusercontent.com/.well-known/openid-configuration)


-


[https://token.actions.githubusercontent.com/.well-known/jwks](https://token.actions.githubusercontent.com/.well-known/jwks)


The proof of concept we’ll be deploying will have the same well known paths, just with a different issuer domain name.


JWTs are issued with a set of “claims”. When a token is issued with a certain claim, the issuer is asserting that the token lays claim to some value, and by signing the token using its private key, the issuer is making an attestation to the validity of that claim. Therefore, if we trust a given issuer and an OIDC token passes cryptographic signature verification, we know we can trust the claims within that token. The set of claims we’re interested in for our PoC and that our service will issue tokens with are as follows:


```text
{


"iss"  :    "https://<deployment-name>.ondigitalocean.app"  ,


"aud"  :    "api://DigitalOcean?actx=f81d4fae-7dec-11d0-a765-00a0c91e6bf6"  ,


"sub"  :    "actx:f81d4fae-7dec-11d0-a765-00a0c91e6bf6:role:data-readwrite"  ,


"droplet_id"  :    514729608


}


```


Token claims


The issuer here is within the` ”iss”` claim, we append` ”/.well-known/openid-configuration”` to find the OpenID Configuration JSON, decode it, and subsequently find the public JSON Web Keys we can use to perform cryptographic signature verification of the token.


Also of critical importance to the security of workload identity federation are the audience (` ”aud”` ) and subject (` ”sub”` ) claims. The audience is used by the resource server to determine if the token is valid for its resources. Our audience is the API we’re authenticating to (` ”DigitalOcean”` ). The subject varies depending on the workload environment we’re authenticating from. The subject identifies the workload itself, what is it?


In our PoC, setting the subject via Droplet tags is a critical piece of our trust flow. Creation of a Droplet with certain tags in combination with our RBAC roles will define what that Droplet has access to. We’re allowing any team member / role with permission to create a Droplet to set its subject. As such, one must take this into consideration when considering[RBAC definitions and assignments](https://docs.digitalocean.com/platform/teams/roles/custom/) . When we access resources from GitHub Actions, we know the answer to “what is it?” based on how the GitHub Actions orchestrator formats the subject:` ”org/repo/.github/workflows/name.yml”` .


## Workload identity solution architecture


The goal for our PoC is to allow users to easily provision Droplet access to team resources such as Spaces Keys and Databases, removing the need for out-of-band secret provisioning or hard-coding sensitive values into cloud-init. In our example, we will use GitHub Actions workloads to deploy DigitalOcean Droplets configured with access to a Managed Database and a Spaces bucket without hard-coding any secret token.


Our PoC has the following aspects:


-


An OAuth Client Application following the[DigitalOcean OAuth Web Application Flow](https://docs.digitalocean.com/reference/api/oauth/)


-


A policy-based evaluation to determine access control (supporting policy upload)


-


Provisioning and issuing workload identity tokens


-


.well-known URIs for OIDC token validation


-


Handlers to intercept and wrap routes provided by the DigitalOcean API:


-


[Create a New Droplet](https://docs.digitalocean.com/reference/api/digitalocean/#tag/Droplets/operation/droplets_create)


-


[Retrieve an Existing Database Cluster](https://docs.digitalocean.com/reference/api/digitalocean/#tag/Databases/operation/databases_get_cluster)


-


[Create a New Spaces Access Key](https://docs.digitalocean.com/reference/api/digitalocean/#tag/Spaces-Keys/operation/spacesKey_create)


To tie these aspects together and make them accessible to both our end-users and services such as Droplets and GitHub Actions which will access them, the PoC leverages[caddy](https://github.com/caddyserver/caddy) as a reverse proxy of DigitalOcean’s API. We can then write a Caddyfile configuring caddy to expose the OAuth route to the user, passthroughs of the DigitalOcean API to the user and workloads, and our wrappers around routes we’ll be modifying to enable workload identity.


Solution High Level Flow


Caddy enables us to easily define routes we want to handle and routes we want to wrap. Our app-specific auth code is the[callback.py](https://github.com/digitalocean-labs/droplet-oidc-poc/blob/main/src/workload_identity_oauth_reverse_proxy/cgi/auth/digitalocean/v1/callback.py) file which redirects users when they land at our servers root to the DigitalOcean OAuth team selection and approval page. Once approved,[callback.py](http://callback.py/) handles secure storage of the users token for the selected team.


Part 1: Initial OAuth Web Flow for Team Level Token


When the user issues a Droplet create request, the proxy application intercepts it and calls our wrapper. The wrapper creates a provisioning token which is a JWT with the subject containing a nonce value and the audience containing the team UUID associated with the DigitalOcean Personal Access Token sent to the Droplet create call endpoint. The wrapper then injects the provisioning token into the Droplet via modification of the` user_data` cloud-init field. The modified create request and PAT are then passed through to the upstream API which creates the Droplet.


Part 2: Workload Identity Token Provisioning for Droplet


The application adds the workload identity token to the Droplet after successful completion of signing and validation of a provisioning token done via[cloud-init](https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup) . On first boot cloud-init executes the` user_data` which binds the provisioning token to the Droplet by signing it with the Droplet’s SSHD private key.


The provisioning token and signature are then exchanged for a workload identity token. Our service looks up the Droplet’s IP address using the team OAuth token received from the OAuth flow. It connects to the Droplets listening SSHD port and retrieves its public SSH key to verify the signature over the provisioning token, and upon successful verification it returns the Droplet’s workload identity token for storage and later use on the Droplet. The workload identity token is an OIDC protocol compliant RSA JWT with the subject containing information from the Droplet’s tags as well as the team UUID within the audience. By including the team UUID in the audience we enable mapping workload identity tokens to the team token provided by the initial web OAuth flow.


Workload ID Token Provisioning Flow


From the Droplet, the workload identity token can be sent to our proxy application where wrappers around Spaces Keys creation and Database information retrieval can lookup the associated OAuth token for the team.


Before the proxy makes a request to the upstream API, the associated policy referenced by the workload identity token’s subject is used to validate the request data from the workload. The scoped team OAuth token is then used to make the request to the upstream API.


This approach allows us to effectively act as the DigitalOcean API to properly configured clients, such as[doctl](https://github.com/digitalocean/doctl?tab=readme-ov-file#configuring-default-values) . Users set the doctl API URL in their doctl config to the fully qualified domain name (FQDN) at which they deployed the API proxy application.


Any doctl commands will then either be intercepted and wrapped by the proxy logic or passed through unaltered. POST requests to Droplet Create and Spaces Keys Create as well as GET requests to Database endpoints are all intercepted and handled by scripts inside the proxy application which wrap requests and responses with the app’s custom logic. All other requests are passed through to the upstream DigitalOcean API.


Part 3: Use of Workload Identity Token by Droplet to Access Resources


## Next steps


Now that we’ve reviewed how the[open source Proof of Concept (PoC)](https://github.com/digitalocean-labs/droplet-oidc-poc) works, we’ll deploy the application to DigitalOcean App Platform. Once deployed, we’ll write custom HCL to configure roles and policies enabling workload identity tokens to be exchanged for API specific access tokens. Token exchange is a best practice pattern,[PyPI Docs: Internals and Technical Details](https://docs.pypi.org/trusted-publishers/internals/) has more information on token exchange in another context which may be helpful to aid understanding. Finally, we’ll use the Droplet’s workload identity token to create Spaces keys and retrieve Database connection URIs. We’ll also configure roles and policies to enable the same from GitHub Actions workflows via workload identity federation.


-


-
