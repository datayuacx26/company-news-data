---
schema_version: "1.0.0"
document_id: "7db30679a4af7326012e0e79e5d9129f23d91f24a50d7f5c89bfa5fe134c296e"
company_key: "yc-propelauth"
company: "PropelAuth"
source_id: "yc-propelauth-news-import-52b3c4ba8ae4"
canonical_url: "https://www.propelauth.com/post/june-2026-updates"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-25T20:01:02.470883+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:bdd7d463eb39d7d29c7b941dd5aab02f3c60a1dafa70a4e50e04901ffa433c44"
---

# June 2026 Updates

June has been all about giving you more control and more visibility, from new identity provider support to sharper audit logging and tighter security controls.


Here are the highlights:


### Ping Identity Support #


We now have first-class support for Ping Identity. Your customers can authenticate via SAML or OIDC, and you can sync users and groups through[SCIM](https://docs.propelauth.com/overview/authentication/scim?ref=propelauth.mymidnight.blog) , all without any extra setup on your end. You can read more about it[here](https://www.propelauth.com/post/ping-identity-saml-oidc-and-scim?ref=propelauth.mymidnight.blog) .


### MFA & Account Security #


- You can now leave a note when resetting MFA through the PropelAuth dashboard. The note is captured in the audit log entry for the reset, so there's always a clear record of who reset MFA and why.
- It's now more obvious in the dashboard when a user has been locked due to too many unsuccessful MFA attempts.
- If a user requires a password update due to their settings on account creation or organization policy, it will now be displayed on their user page.
- User audit logs now record when a user marks a device as remembered during the MFA flow.


### IP Blocking #


You can now block IPs directly from the PropelAuth dashboard. It's a simple way to cut off suspicious or abusive traffic without leaving the platform.


### Audit Logging #


- Organization audit logs now include more detailed information about Enterprise SSO connection configurations.
- Better audit logging has been added for certain actions related to MCP Organization Scopes.


### Enterprise SSO #


- Enterprise SSO connections must now pass a successful test run before they can be set to **Live** , so your customers can confirm their connection is configured correctly before turning it on.
- metadata.xml files can now be uploaded during the SAML set-up process to automatically fill in fields (when supported by the SAML provider).


### Organization Management #


- You can now filter by roles on **Organization Users** lists, making it easier to find exactly the members you're looking for.


### Misc. Changes #


- Private IPs (` 10.*` ) can now use` http` instead of` https` in the test environment, smoothing out local development.
