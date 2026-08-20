---
schema_version: "1.0.0"
document_id: "597c54341cd874667ea557f51e4c185728aa7f19012ac4e4f3a32a9824b9d2dc"
company_key: "yc-blueshoe"
company: "Blueshoe"
source_id: "yc-blueshoe-news-import-28975b8749e8"
canonical_url: "https://www.blueshoe.io/blog/django-o365-a-modern-mail-backend-for-django-and-exchange-online/"
published_at: "2025-10-24T10:00:00+00:00"
first_seen_at: "2026-07-21T10:56:59.304644+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:ed6bce7f06e92eacf47000f6598f68de5c548b7f9392b95369bf3a4d646af37e"
---

# django-o365: A Modern Mail Backend for Django and Exchange Online | BLUESHOE

## Table of Contents


- Modern Email Authentication for Django Applications
- The Starting Point: Mail Dispatch with django-exchange
- Technical Foundation: python-o365
- Architecture & How It Works
- Usage in Django
- Open Source & Outlook


---


## Meet the Author


[Robert Gutschale](https://www.blueshoe.io/team/robert-gutschale/) With django-o365, sending emails in Django via Exchange Online with OAuth 2.0 is that simple.


Last updated:


2026-02-04


---


Python


Django


Development


---


2026-02-04


# Open Source, OAuth 2.0 and Microsoft 365


django-o365: A Modern Mail Backend for Django and Exchange Online


[django-o365](https://github.com/Blueshoe/django-o365) is an open-source mail backend for Django that enables sending emails via Exchange Online using OAuth 2.0. Born from real-world project requirements, this package is aimed at teams using Microsoft 365 who want to implement their email dispatch securely, maintainably, and without workarounds. This article provides an overview of the motivation, features, and usage.


## Modern Email Authentication for Django Applications


Emails are a central component of many web applications – and for that very reason, they are particularly critical when they don't function reliably. Many companies rely on Microsoft 365 and Exchange Online, while classic SMTP authentication is increasingly being restricted or completely disabled. OAuth 2.0 has become the recommended standard.


With[django-o365](https://github.com/Blueshoe/django-o365) , we have developed a mail backend for[Django](https://www.blueshoe.io/technologies/python-django-agency/) that allows sending emails via Exchange Online with OAuth 2.0 authentication and integrates seamlessly into the existing Django mail workflow. In this post, we will show you what components it is built on and how you can use it in your own project.


## The Starting Point: Mail Dispatch with django-exchange


In a legacy project for one of our clients, email dispatch was previously handled with a local mailbox via Microsoft Exchange. For this purpose, we developed the package[django-exchange](https://github.com/Blueshoe/django-exchange) back then. The mailbox has now been migrated to Exchange Online with OAuth 2.0 authentication. Since there was no suitable package for Django yet, we developed django-o365. Our main motivation was to achieve a clean Django integration so that we would only need to replace the email backend in our project.


## Technical Foundation: python-o365


As its technical foundation, django-o365 relies on the established Python package[python-o365](https://github.com/O365/python-o365) . This provides a complete abstraction of the Microsoft Graph API and handles, among other things, authentication via OAuth 2.0 and the actual sending of emails through Exchange Online.


django-o365 specifically uses python-o365 as a transport layer but completely encapsulates its functionality behind the familiar Django mail interface. This allows us to benefit from a stable and actively maintained base without having to use it directly in the application code.


We deliberately decided against implementing our own version of the Microsoft Graph API: python-o365 already covers the required functionality reliably and allows us to focus on a clean Django integration, maintainability, and ease of use.


## Architecture & How It Works


django-o365 is intentionally built to be lean and follows the principle of seamlessly integrating into existing Django structures. The core of the package is a custom Django mail backend that implements the standardized Django mail API. As soon as Django sends an email, django-o365 hands it over to python-o365, which in turn handles the dispatch via the Microsoft Graph API.


Authentication is handled entirely via OAuth 2.0. Access and refresh tokens are managed and renewed by python-o365 without the application code needing to be aware of it. Thus, django-o365 acts as a bridge between Django and Exchange Online: it translates Django's mail dispatch into Graph API calls without forcing additional logic into the application.


This clear separation has two advantages: firstly, the Django code remains unchanged and independent of Microsoft-specific details, and secondly, we can rely on a proven library for the complex OAuth and API logic.


## Usage in Django


Since django-o365 is not yet published on PyPI, it is installed directly from the Git repository, for example, via the requirements.txt file. Other dependencies, especially python-o365, are installed automatically.


On the Microsoft side, an app registration in Microsoft Entra ID (formerly Azure AD) is required. An application is created there that receives the permission to send emails via Microsoft Graph (e.g.,` Mail.Send` ). For OAuth authentication, a Client ID, a Client Secret, and the Tenant ID are needed. Additionally, it must be specified whether the dispatch should occur via a user account or a shared mailbox.


In Django, the configuration is limited to the settings. Only the mail backend is switched to django-o365, and the required credentials are provided:


```text
EMAIL_O365_TENANT_ID   =   'my-tenant-id'
EMAIL_O365_CLIENT_ID   =   'my-client-id'
EMAIL_O365_CLIENT_SECRET   =   'my-super-secret'
EMAIL_O365_SENDER   =   'mysender@example.com'


# set backend
EMAIL_BACKEND   =   'django_o365.backend.O365EmailBackend'


```


No changes are needed in the actual application code. Existing calls to` send_mail` ,` EmailMessage` , or` EmailMultiAlternatives` continue to work as before. The switch to Exchange Online with OAuth 2.0 is done exclusively through configuration – which was one of the central goals in the development of django-o365.


## Open Source & Outlook


We at Blueshoe are firm believers in the open source approach. Therefore, django-o365 is, of course, designed as an open-source project and is developed openly on GitHub. The current focus is deliberately on a clear, stable core: reliable mail dispatch via Exchange Online with OAuth 2.0 authentication using the credentials workflow. Sending emails, including attachments, is already fully supported and covers the common use cases in web projects.


For the future, other authentication workflows are conceivable, such as more user-centric scenarios. The aspiration remains the same: a clean Django integration without unnecessary complexity in the application code. Feedback, suggestions, and contributions from the community are expressly welcome – they help to further develop the package in a targeted and practical manner.


---


## Do you have questions or an opinion? With your GitHub account you can let us know...


---


### Here are a few articles that you might also find interesting:


[Act, Don't Isolate: The Blueshoe Approach to Kubernetes Development Develop locally, test in the cluster - without CI/CD wait times. Gefyra promises the holy grail of Kubernetes development: full-speed coding with real cluster context. In our case study, you’ll learn why this tool is a game-changer for your daily operations and how to efficiently integrate it into your workflow by Carl-Christian Neundorf](https://www.blueshoe.io/blog/gefyra-hands-on/)


[Mastering Django Caching: The Ultimate Guide from Cachalot to the Low-Level API Is your Django application slower than it should be? Long loading times not only frustrate your users but are also penalized by search engines. The solution often lies in a smart caching strategy. But where do you start? by Jonathan Kölbl](https://www.blueshoe.io/blog/django-caching-cachalot-performance-guide/)


[Handoff from Designer to Developer: What We Need for Frontend Implementation The transition from design to development is a critical moment in any frontend project. A well-thought-out handoff can save weeks of development time, while an incomplete handoff leads to endless questions, delays, and compromises. by Lukas Rüsche](https://www.blueshoe.io/blog/designer-to-developer-handoff/)


[Nuxt 4 is here: A Deep Dive into the New Features and What They Mean for Developers After months of speculation and intensive beta testing, the Nuxt team has officially released Nuxt 4. by Lukas Rüsche](https://www.blueshoe.io/blog/nuxt4-new-features/)


[Alternatives to Celery for Django on Kubernetes Are you planning background jobs in your cluster and wondering if there are lighter options than Celery? by Jonathan Kölbl](https://www.blueshoe.io/blog/alternatives-to-django-celery-in-kubernetes/)


[More Blog Articles Discover exciting insights and practical tips in our latest blog posts. From Python and Rust to Kubernetes, Django security, and frontend topics – find valuable knowledge for your projects here. Click to explore all articles!](https://www.blueshoe.io/blog/)


## These companies trust Team Blueshoe


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
