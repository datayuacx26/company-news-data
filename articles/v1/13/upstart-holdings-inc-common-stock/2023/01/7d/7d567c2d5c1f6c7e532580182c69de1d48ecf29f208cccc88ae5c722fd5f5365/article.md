---
schema_version: "1.0.0"
document_id: "7d567c2d5c1f6c7e532580182c69de1d48ecf29f208cccc88ae5c722fd5f5365"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/self-service-oauth-with-okta-and-terraform-ede1555465ab"
published_at: "2023-01-18T22:37:21+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-08-20T00:26:23.139930+00:00"
content_hash: "sha256:f831352f36a8de8336a90c740a6c0d2ae6c98c11654f621354322ad46a7094cf"
---

# Self-Service OAuth with Okta and Terraform

Here at Upstart, our engineering architecture is growing to be ever more complex, both on the product side and internally. This includes both internal application communication as well as integrating external applications. With software, everyone is chomping at the bit to write that amazing tool to solve problems X, Y, and Z as the opportunity for solving these problems grows.


For example, it feels great to finally finish that code snippet to automate a mindless, time-consuming manual task that’s been eating away at you for a while. But before you share that tool, there’s a feeling in your stomach that seems impossible to shake. You have tested locally, operating under implicit trust from one system to another, but how is trust handled when your pipeline is performed by a computer hosted in the cloud? Authorization, that was the pit in your stomach! Fortunately, authorization (“authz”) can be both easy and self-service, while keeping security in mind, using a few simple tools: Okta, Terraform, and a secrets vault (AWS Secrets Manager in this case).


### The Current State and Its Problems


We have all seen it; that super-privileged (hopefully encrypted) API key that sits waiting to be accessed and used in Authorization headers for your API calls. In most cases, when interacting with external services, you have the option to paste a generated API key into a form-field to be used for HTTP requests. This is super convenient and easy, but here are a couple issues that may come to light from using this option:


1. You just pasted a secret key into a field that you have no control over, exposing that key to external databases or services owned by others.
2. There is nothing programmatically telling you what constraints govern what this key is able to access and do. Once you create this key, you have no control on what requests can be made under its privileges. If this key is compromised, your attacker only has to try a handful of authorization tactics that are common amongst most static credentials.
3. Which brings me to my next point: In most cases, this key is static. The longer a static key exists, the more likely it is to be compromised. This may motivate key rotation, which is an entirely separate, fruitfully fault-prone process.


### Welcome, OAuth


OAuth, or Open Authorization, is an authz protocol that can address most limitations of a static API key. Rather than a service inherently trusting an HTTP header, there is a series of handshake transactions to gain access to resources, prior to accessing any secure servers. The transactional flow sequence looks like this:


1. The service requests an access token restricted to certain scopes from the authorization server (in this case, with a client credentials flow).
2. The authorization server checks the token request against its defined access policies for the OAuth client. If the client passes the checks, an access token is returned, scoped to the necessary resources.
3. The service uses the token (typically as a Bearer) to make subsequent requests to the secured resources.
4. Each request received by the server is tested. In this case, a token introspection endpoint is used by the server to determine if the token is valid (not expired, correct scopes for resources and operations requested). If the token is valid, the HTTP request is successful, otherwise the request should throw an error.


This flow may seem very confusing and a lot of extra work compared to a static key, and the benefits may not seem explicitly clear.


### Benefits of OAuth


Now that we have an understanding of the general OAuth flow, let us outline its benefits as compared to static API credentials. From the above explanation, it may seem like we are just creating another static credential, and calling it something other than “API key.” Very literally, yes, we are! Even though there is a higher up-front cost to set OAuth up, we still have a much better security posture for authorization based on the following non-exhaustive list of reasons:


- While client credentials are static, the access token you are provided via successful OAuth transaction is not. Typically, the OAuth provider will configure access tokens minted for their servers to be dynamic and expirable. Therefore, if a Bearer token is compromised, your attacker will only have access to your resources for the amount of time your token is configured to be active. After that, your service should return HTTP errors (since the service has ongoing token introspection per request).
- You may now be wondering about the scenario of a compromised client credential pair. Let us go through that exercise and imagine that an attacker gained access to your application’s client credentials. Oh no! Had they gotten your API key, they could try a few different ways to pass in Authorization headers to your server, and likely figure out pretty quickly how to make things go poorly for your team. This is where OAuth excels! If a hacker gets your client credentials, they have to figure out what kind of access policies you have configured in your OAuth provider to gain an access token. Typically, when making a token request to an OAuth server, you must provide a scope(s) to the authorization server, so the authorization server can check to see whether your client credentials have the ability to get an access token with that scope. The good thing about these scopes is that they are totally arbitrary! You define them as a developer in your application, and configure them in your OAuth provider. A hacker will have a harder time figuring out what client can access custom scopes that you defined, versus figuring out whether to pass a static API key in an Authorization header by way of Single Sign-On for Web Systems (SSWS) or Bearer.


Here are a couple very common, well-known ways to use an API key, that are sure to be known by your attacker. You’ll notice that the credential never has to change and that the methods for authorization below are probably some that you have used before.


```text
# Bearer authorization  curl https://my-very-secure-service.com/databases/123/delete \    -H “Authorization: Bearer your-hacked-key-xyz”   # Basic authorization  curl https://my-very-secure-service.com/databases/123/delete \    -H “Authorization: Basic your-hacked-key-xyz”   # SSWS Authorization (Okta’s API token authorization method  curl https://my-very-secure-service.com/databases/123/delete \    -H “Authorization: SSWS your-hacked-key-xyz”   # custom api key header, can be found in AWS API gateways, for example  curl https://my-very-secure-service.com/databases/123/delete \    -H “x-api-key: your-hacked-key-xyz”
```


Comparatively, here is what would happen if a client credential pair was compromised


```text
# Token request without scopes parameter  curl \  https://my-very-secure-auth-server/token?grant_type=client_credentials&client_id=your-hacked-id&client_secret=your-hacked-secret
```


In the above scenario, it is quite possible that you are able to retrieve a token without requesting scopes, but if you configure your application to only perform operations under a scoped token, the token generated here is useless.


```text
# Token request with invalid scopes  curl \  https://my-very-secure-auth-server/token?grant_type=client_credentials&client_id=your-hacked-id&client_secret=your-hacked-secret&scopes=fake.scope.guess
```


In this scenario, you’d get an error return value, as fake.scope.guess is an invalid scope that is not defined in your authorization server. This is how OAuth adds another layer of security that is totally custom to each individual implementation of it.


- Access tokens are dynamic, both in expiration and scope. Another benefit of OAuth is flexibility for least-privileged access on a per-request basis! For example, given an application with read and write operations, it is likely that you will separate read and write privileges to minimize risk, assigning each privilege to a different set of accessors. This is where OAuth flexibility shines. All you have to do is request read scope with your client credentials, and then you don’t have to worry about accidental (or threat actor) write operations. If someone were to compromise this token, it would not have write access, which reduces the surface area of the threat. Conversely, if someone were to compromise your static API key that has all the permissions baked into it, they could do whatever they want.


### Okta OAuth Provider with Terraform


If I haven’t convinced you that OAuth is better than an API key, or you think the overhead of setting this up is too great, let’s move on to self-service OAuth.


Now understanding the benefits of OAuth, we must weigh the overhead cost against the security gain. There definitely is a high overhead, thinking in terms of standing up your own authorization server to mint access tokens, scopes, access policies, etc. Fortunately, at Upstart we have a great set of resources to handle this with. In the simplest case, as few as five lines of code is all it takes to provision OAuth resources, along with bringing your own credentials for your applications.


If you are lucky enough to be working with Okta, AWS, and Terraform, this is a breeze! We have recently gone through the exercise of configuring OAuth applications, scopes, access policies, and access policy rules in Okta. While this is a relatively easy task for a seasoned Okta developer, why should the Okta developer be a bottleneck for product development? I believe there is no reason to be the bottleneck, so I decided to write a self-service module for OAuth, using Okta as the OAuth Provider. Terraform acts as the self-service piece and AWS Secrets Manager stores client credentials.


All a developer needs to do is create a secret containing credentials for both client and server side applications, and then utilize[this Terraform module](https://github.com/lukasindre/terraform-okta-oauth-generator/tree/v2.0.0) to provision the Okta resources needed. With a couple lines of code, and a little bit of knowledge surrounding Okta’s OAuth API endpoints, developers can be up and running with OAuth in just a couple of minutes! Wrapped in this module are plenty of configurable things, namely your service’s name, scopes, and token lifetime. In comparison, the end-to-end workflow for this without self-service would be:


- Developer submits request to Okta team to provision OAuth resources
- Back and forth questionnaire between Okta team and developer
- Okta team clicks what seems to be a million buttons to provision resources
- Okta team shares client credentials with developer


With change management, and time zone considerations for workers, the above workflow could take days, if not weeks, to complete. Below we can see a couple declarations of this module from the examples directory:


This is the most simple declaration of the module, if you do not require that much customization.


```text
module "example_app" {    source = "github.com/lukasindre/terraform-okta-oauth-generator.git?ref=v2.0.0"    label  = "Test App"    client_credentials = {      client_client_id     = local.creds["client_client_id"]      client_client_secret = local.creds["client_client_secret"]      server_client_id     = local.creds["server_client_id"]      server_client_secret = local.creds["server_client_secret"]    }  }   data "aws_secretsmanager_secret_version" "creds" {    secret_id = local.secret_id  }   locals {    secret_id          = "my-secret-id"    credentials_object = jsondecode(data.aws_secretsmanager_secret_version.creds.secret_string)  }
```


On the other hand, if you want to configure your own scopes, add token lifetime, and policy descriptions, you can see that in the following declaration:


```text
module "easy_bake_service" {    source = "github.com/lukasindre/terraform-okta-oauth-generator.git?ref=v2.0.0"    label  = "Easy Bake Oven"    scopes = [      {        "name" : "dough.cookie.mix",        "description" : "grant permission to mix cookie dough"      },      {        "name" : "dough.cookie.bake",        "description" : "grant permission to bake the cookie dough"      },      {        "name" : "cupcake.batter.mix",        "description" : "grant permission to mix cupcake batter"      }    ]    access_token_lifetime_minutes = 60    client_credentials = {      client_client_id     = local.creds["client_client_id"]      client_client_secret = local.creds["client_client_secret"]      server_client_id     = local.creds["server_client_id"]      server_client_secret = local.creds["server_client_secret"]    }    policy_description = "This policy restricts what the OAuth app is allowed to do with the easy bake oven service."  }   data "aws_secretsmanager_secret_version" "creds" {    secret_id = local.secret_id  }   locals {    secret_id          = "my-secret-id"    credentials_object = jsondecode(data.aws_secretsmanager_secret_version.creds.secret_string)  }
```


With this proposed self-service model, all the developer has to do is provide client credentials, know a little bit of Terraform and they can be on their way to OAuth as quickly as the Terraform build happens in their CICD pipeline.


### The Module


This Terraform module takes care of all the nitty-gritty, Okta-nuanced details imposed by the Okta API, so developers don’t have to learn an entirely new framework to interact with to provision their resources. For the developer’s interest and needs, it creates the OAuth clients and developer-defined scopes and lets them self-service changes, speeding up development. For Security’s interest, it defines access policy and rules so that only the clients provisioned per module are able to access the defined scopes, versus an “All Clients” trust scenario, minimizing risk. Additionally, using this module frees resources from the Okta development team to improve our corporate identity posture in other great ways. Going forward, the use and evolution of this module will ensure that both Okta developers and software developers are spending their resources and time on developing, while keeping our Information Security team happy by both using OAuth and a templated authorization framework.


---


[Self-Service OAuth with Okta and Terraform](https://medium.com/upstart-tech/self-service-oauth-with-okta-and-terraform-ede1555465ab) was originally published in[Upstart Tech](https://medium.com/upstart-tech) on Medium, where people are continuing the conversation by highlighting and responding to this story.
