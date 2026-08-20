---
schema_version: "1.0.0"
document_id: "26de16573b1efbcabfa415a12c985b21baad6c5b94ab47fb25942c89cd30328b"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/user-account-security-mfa"
published_at: "2024-09-19T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:b39d3cb48b3333ca6183ccf6e6f68d7de3dc81c2508862c0ddca03c6a9a31edd"
---

# Secure your Elastic Cloud account with multifactor authentication (MFA)

# Secure your Elastic Cloud account with multifactor authentication (MFA)


By


[Alex Chalkias](https://www.elastic.co/blog/author/alex-chalkias)


September 19, 2024


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


In an era where cyber threats are constantly evolving, protecting your identity and data from unauthorized access is more critical than ever. That's why we're excited to bring you the enhanced multifactor authentication (MFA) for Elastic Cloud. This feature significantly strengthens the security of your Elastic Cloud user and deployment data by aligning with industry best practices. You can go to Elastic Cloud and


[complete your MFA setup today](https://cloud.elastic.co/user/settings) .


**Important note:**


At the time of writing this blog, Elastic Cloud MFA enforcement is being gradually rolled out to all Elastic Cloud users. If your experience varies from what is described in this blog, feel free to reach out to your Elastic Account team for questions or


[refer to the FAQ](https://www.elastic.co/guide/en/cloud/current/ec-account-security-mfa.html#ec-account-security-mfa-faq) . If you face challenges with your Elastic Cloud login, please


contact the Elastic Support team .


## Why multifactor authentication matters


MFA adds an essential layer of security to your account by requiring two or more verification factors to gain access. This approach dramatically increases the difficulty for potential malicious actors to compromise your account even if they manage to obtain your password.


Here's why MFA is crucial for your Elastic Cloud account:


1.


**Enhanced security**


**:** By requiring multiple forms of identification, MFA creates a much stronger barrier against unauthorized access. Even if an attacker obtains your password through phishing, keylogging, or a data breach, they would still need additional authentication factors to access your account, such as your fingerprint, your email account or the authenticator app explicitly configured for your account.


2.


**Protection against password-related attacks**


**:** Cybercriminals often use techniques like brute-forcing, credential stuffing, and password spraying to compromise accounts. Automated tools and leaked username and password combinations across various services are often adopted in these attacks. MFA renders these attacks ineffective as the attacker would lack the additional authentication factor.


3.


**Compliance with security standards**


**:** Many industry regulations and security frameworks require or strongly recommend MFA, especially for accessing sensitive data or systems.


4.


**Early detection of unauthorized access attempts**


**:** MFA can alert you to potential security breaches. If you receive an unexpected authentication request, it could indicate that someone is trying to access your account.


By implementing MFA, you're not just adding an extra step to your login process, you're creating a robust defense mechanism that significantly reduces the risk of unauthorized access to your valuable data and systems.


## What's new in this release?


We've made several important improvements to our MFA system:


1.


**MFA by default**


**:** When authenticating to any Elastic service* through


[Elastic Cloud](https://cloud.elastic.co/login) with a username and password, you will now be redirected to an MFA setup screen if you haven't already set up an MFA method. This ensures that every account is protected by at least one additional authentication factor.


2.


**Email MFA support**


**:** You can already


[set up MFA with an Authenticator app](https://www.elastic.co/guide/en/cloud/current/ec-account-security-mfa.html#ec-account-security-mfa-authenticator) in Elastic Cloud. With this release we've also introduced


[email](https://www.elastic.co/guide/en/cloud/current/ec-account-security-mfa.html#ec_configure_email_authentication) as a new authentication method. This provides more flexibility for users who prefer email-based verification. If instead of password-based authentication you use a third-party Identity Provider (IdP), such as Google, Microsoft, or other SAML IdPs, you can configure MFA from your IdP.


3.


**Phasing out SMS MFA**


**:** In line with our internal information security policy and industry best practices, we're moving away from SMS as an authentication option. SMS is not considered a secure MFA method due to a number of weaknesses, and we're committed to providing you with the most secure options available.


**Users currently using SMS MFA will be prompted to set up a different method.**


These changes reflect our ongoing commitment to providing robust security measures that protect your data while maintaining a smooth user experience.


## Take action: Set up MFA today


We strongly encourage all users to set up MFA for their Elastic Cloud accounts. Here's how:


[Log in](https://cloud.elastic.co/login) to your Elastic Cloud account — remember that if you haven’t already set up MFA, you will likely be shown the MFA setup screen right after authentication. Alternatively, navigate to your account settings at


[https://cloud.elastic.co/user/settings](https://cloud.elastic.co/user/settings) . Follow the prompts to set up your preferred MFA method.


By taking this simple step, you'll significantly enhance the security of your Elastic Cloud deployments and protect your valuable data from unauthorized access.


## Learn more


For detailed information about Elastic Cloud MFA, including setup instructions, best practices, and FAQs, please refer to our comprehensive documentation in


[Elastic Cloud account security and MFA guide](https://www.elastic.co/guide/en/cloud/current/ec-account-security-mfa.html) .


At Elastic, we're committed to providing you with the tools and features you need to keep your data secure. Enabling robust and secure MFA is just one more way we're working to ensure that your Elastic Cloud experience is not only powerful and flexible but also in line with industry standards.


Stay safe, stay secure, and as always, happy searching!


*


Elastic Cloud login is used to authenticate the following services or portals provided by Elastic:


-


Elastic Cloud —


[cloud.elastic.co](https://cloud.elastic.co/) (Please note that MFA enforcement applies to all Elastic Cloud trial and commercial organizations.)


-


Support Hub —


[support.elastic.co](https://support.elastic.co/)


-


Learning Portal —


[learn.elastic.co](https://learn.elastic.co/)


- Partner Portal (coming soon) —


[partners.elastic.co](https://partners.elastic.co/)


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
