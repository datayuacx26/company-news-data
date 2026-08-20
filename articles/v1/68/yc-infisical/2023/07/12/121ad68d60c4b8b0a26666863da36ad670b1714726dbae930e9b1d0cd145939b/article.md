---
schema_version: "1.0.0"
document_id: "121ad68d60c4b8b0a26666863da36ad670b1714726dbae930e9b1d0cd145939b"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-update-june-2023"
published_at: "2023-07-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:38583a0cb9afa2da5ec667a6d4ad72a3ebce012c1a9bc5b8eab38aa200fec890"
---

# Infisical Update – June 2023

June has been another month with many amazing updates. You can read more about all of those below. ✨


## 1: Ability to query and fetch back secrets in plaintext.


First up is the new addition of (optional) REST API endpoints that can be used to fetch secrets back from[Infisical](https://github.com/Infisical/infisical) in plaintext.


Previously, in order for applications to consume secrets stored with[Infisical](https://github.com/Infisical/infisical) via REST API, developers needed to manually fetch back encrypted secrets and perform cumbersome, client-side decryption operations to maintain the platform's end-to-end encryption (E2EE) property. After much discussion with users, however, we found E2EE to be perceived more as a nice-to-have rather than a must-have feature; in fact, companies self-hosting[Infisical](https://github.com/Infisical/infisical) cared little about E2EE, so long as secrets remained encrypted at rest and secured in transit with TLS. Seeing that other solutions in the market don't impose E2EE and maintain good security posture, we decided to allow users to opt their projects out of E2EE in order to be able to query and fetch their secrets back in plaintext from[Infisical](https://github.com/Infisical/infisical) .


This topic deserves a separate blog post but, in any case, we felt it important to include some of the key considerations above.


## 2: Ability to organize and nest secrets in folders.


Next up, folders enable secrets, of different types belonging to distinct services, to be stored under one project within unique paths. Here, we present a versatile but common structure that lets you better organize secrets for more advanced use-cases. To get a better understanding, check out the nested folder structure below:


```text
|-- microservice1
|---- environment-variables
|---- users
|------ oauth-tokens
|------ api-keys
|-- microservice2
|---- environment-variables


```


In this example for some hypothetical infrastructure with two micro-services, the structure allows you to store the environment variables for each micro-service under the respective` /environment-variables` folder for it. It also allows you to store user-specific secrets for micro-service 1 under the` /users` folder. Given this structure, when a client wishes to fetch the environment variables for micro-service 1, it can specify the path to the secrets it wishes to fetch from such as` /microservice1/environment-variables` . By extending this example, you can see how we may use folders to accommodate for advanced use-cases.


Check out the documentation for the folders feature[here](https://infisical.com/docs/documentation/platform/folder) .


To note, we've built the folders feature with integrations in mind. So, when using an integration such as Infisical-Vercel, you can indicate which path you want to sync secrets from (default is` /` ).


## 3: Support for multi-line secrets.


After hearing your feedback, we've added support for the storage of multi-line secrets such as certificates and RSA keys etc.


For example, this type of secret is now supported:


```text
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDGxULSIlhBJAl/JIyn4dbdc1xJ
HbgZrkd84sPapEYHOKKuKD/kVCM1nk9dNWbbWXldM++kgcj7SFV2aQi+/lrS9ZeY
b7Ke+eAGeyCjmvmXkQj3seKvpPOIGBzFAMby/D+Hr1fa7K3A8MI7v5yHsxg2sDrJ
xgXSiJJaCAHWlA0dOQIDAQAB
-----END PUBLIC KEY-----


```


## 4: Infisical completed a full-coverage, gray-box penetration test (pentest).


As part of our security program, we commissioned cybersecurity firm Oneleet to perform a full-coverage, gray box pentest against the application’s entire attack surface to identify vulnerabilities, according to industry standards (such as OWASP ASVS, WSTG, TOP-10). The process lasted from May-June throughout which several minor vulnerabilities were discovered and remediated. Following remediation, Oneleet provided a remediation report and letter of attestation for the conducted pentest.


You can read more about the pentest[here](https://dev.to/dangtony98/guide-to-pentesting-what-why-and-how-it-works-2b92) and request a copy of either remediation report / letter of attestation by contacting our team.


## 5: Native integrations for Checkly and HashiCorp Vault and a new Terraform provider.


Every month, we add more native integrations to[Infisical](https://github.com/Infisical/infisical) so that secrets can be synced to other platforms like Vercel, Netlify, Github, GitLab, etc. This month, upon request, we added integrations for Checkly, an API & E2E monitoring platform, HashiCorp Vault, a tool for securely accessing secrets, and Terraform.


The[Checkly integration](https://infisical.com/docs/integrations/cloud/checkly) allows developers to sync secrets from Infisical to Checkly at the global level. Meanwhile, the[Vault integration](https://infisical.com/docs/integrations/cloud/hashicorp-vault) allows developers to specify the Vault Cluster URL, namespace, and engine/secrets paths to sync secrets to; the integration uses Vault's AppRole mode of authentication. We also released a Terraform provider for users of Terraform which you can read more about[here](https://infisical.com/docs/integrations/frameworks/terraform) .


## 6: Revised usage and billing experience in Infisical Cloud.


[Infisical Cloud](https://app.infisical.com/) now comes with a revised usage and billing page which organizations can use to monitor and update their existing plan, usage, and billing information. Amongst many improvements to the previous experience, here are a few that stand out:


- Ability to update invoice details like organization name, email recipient, tax identification numbers.
- Ability to view current usage information in the context of plan limits.
- Ability to compare plan offerings against each other via one unified view.


## 7. Restyling & performance improvements


As part of making the platform experience feel more uniform, cohesive, and fast, we standardized the look and feel of the billing and usage, organization settings, user settings, and project settings pages. This meant reducing the variation in button types to primary and secondary, sticking to a smaller selection of font sizes, and introducing tabs to better organize content within pages, as opposed to having really long pages.


Beyond styling, we improved the performance and reliability of the integrations page. We also made a few DX improvements such as setting up ESLint for more consistent styling across the codebase.
