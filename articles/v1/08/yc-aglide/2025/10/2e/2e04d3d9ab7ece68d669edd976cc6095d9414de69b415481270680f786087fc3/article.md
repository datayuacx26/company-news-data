---
schema_version: "1.0.0"
document_id: "2e04d3d9ab7ece68d669edd976cc6095d9414de69b415481270680f786087fc3"
company_key: "yc-aglide"
company: "Aglide"
source_id: "yc-aglide-rss-a73e409223ac"
canonical_url: "https://blog.aglide.com/guides/add-sso-saml-scim-to-citibank-with-aglide/"
published_at: "2025-10-13T17:23:30+00:00"
first_seen_at: "2026-07-24T14:45:00.121943+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:d8ddf2d034361355308614633fabde7edcda9ce247f79b13effdb9c41d807b6c"
---

# Setup SSO (SAML, SCIM) for Citibank

### **Does Citibank support SSO?**


If you’ve tried to connect **Citibank’s corporate portal** to your **identity provider (Okta, Entra, or Ping)** , you’ll know there’s no built-in **SSO (SAML/OIDC)** option.


**Aglide** changes that.


With **Aglide** , you can add **enterprise SSO and SCIM automation** to **Citibank** and any other financial platform your business uses.


---


### **How Aglide connects Citibank**


**Aglide** provides dedicated **SAML and SCIM details** for **Citibank** , allowing a seamless connection to:


- **Okta**
- **Microsoft Entra ID (Azure AD)**
- **Ping Identity**
- **Other IdPs supporting SAML**


Users authenticate via **SSO** , and admins can manage account creation and deprovisioning automatically.


Image of Citibank in Okta, and the Aglide login widget appearing on the CitiDirect login page.


---


### **Benefits of connecting Citibank via Aglide**


Aglide makes Citibank a natively supported application in the eyes of your IdP, leading to:


- **Strong, phishing-resistant SSO** - leverage your IdP’s MFA and conditional access when logging into Citibank.
- **Automated provisioning and deprovisioning (SCIM)** - full lifecycle control. Remove access as soon as someone leaves.
- **Access audits at the IdP level** - visibility across all financial apps in one place. Both what accounts people have and when they're accessed.


Using Aglide to achieve this means you also get:


- **Support for shared accounts (e.g. developer API access)** - managed securely within Okta/Entra/Ping in the same way.
- **More secure than SWA or password managers** - with Aglide credentials never touch the browser or are revealed to the user, so can't be stored or used in an insecure location.
- **Unsanctioned user detection** - Aglide directly detects which users have access to Citibank, and notifies you of issues.


Learn more about[Aglide](https://aglide.com/?ref=blog.aglide.com) .


Explainer of Aglide key features: Native SAML Login, automated provisioning and de-provisioning, automated password rotation, shared accounts behind sso, unsanctioned user and privilege detection, and session recording / playback.


---


### **Setup overview**


1. Create a new **Aglide connector** called “Citibank”.
2. Run the **Show & Tell setup** wizard.
3. Configure your **IdP** with **Aglide’s SAML and SCIM endpoints** .
4. Validate **login** and **provisioning** .
5. Assign users or groups via your **IdP** .


Setting up Aglide **for any application** takes just a few minutes, and can be done independently by a service admin at any time.


---


### **Why security chooses Aglide**


**Aglide** integrates apps directly into your IdP, and therefore with your **IGA stack** (SailPoint, Omada, Opal, ConductorOne) - **Citibank access reviews and RBAC policies** become part of your normal enterprise identity flow.


All secrets are **locally encrypted with zero-trust keys** , therefore your accounts are never accessible by **Aglide's servers** . Automations run on device. The full[security whitepaper](https://aglide.com/security?ref=blog.aglide.com) outlines how this is done.


Aglide is SOC 2 compliant - check out the[trust portal here](https://trust.aglide.com/?ref=blog.aglide.com) .


---


### **Get started**


You can connect **Citibank** , or any other banking platform (see our guides for[J.P. Morgan](https://blog.aglide.com/enable-enterprise-sso-for-jp-morgan-with-aglide/) and[Customers Bank / CUBIX](https://blog.aglide.com/enable-sso-saml-scim-for-cubix-with-aglide/) ), to your **Okta** , **Entra** , or **Ping** environment in minutes.


Book a demo to see how


****Aglide**** turns disconnected apps into secure, managed


****SSO integrations**** .


[Book Demo](https://aglide.com/book-demo?ref=blog.aglide.com)
