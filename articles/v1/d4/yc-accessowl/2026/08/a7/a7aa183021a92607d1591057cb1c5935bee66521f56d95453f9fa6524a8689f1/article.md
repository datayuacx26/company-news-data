---
schema_version: "1.0.0"
document_id: "a7aa183021a92607d1591057cb1c5935bee66521f56d95453f9fa6524a8689f1"
company_key: "yc-accessowl"
company: "AccessOwl"
source_id: "yc-accessowl-news-import-49160e0486d6"
canonical_url: "https://www.accessowl.com/blog/saas-discovery-6-methods-to-uncover-shadow-it-in-google-workspace"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T12:25:22.387033+00:00"
fetched_at: "2026-08-13T12:25:23.785076+00:00"
content_hash: "sha256:d67efe3bca2f62a8f8930b9cce5c79acc42b8887b5bff8caf8a88478e43883a4"
---

# SaaS Discovery: 6 Methods to Find Shadow IT in Google Workspace

There’s no denying the impact SaaS services have had on businesses. It’s no surprise that[70% of software](https://devsquad.com/blog/saas-statistics) used by companies today are SaaS applications. For many modern-day startups, that number is 100%.


But, even with all their upsides, the rapid adoption of new apps without a proper[SaaS management](https://www.accessowl.com/blog/what-is-saas-management) strategy can lead to increased security risks, compliance gaps, and unnecessary costs.


SaaS Discovery is the first and most crucial step in SaaS management. Why? You can’t take any decisive action without a complete inventory of all the SaaS apps linked to your business. How else will you know the marketing department has started using a tool to collect the email of website visitors? They don't know anything about GDPR or the other regulations that require user consent.


Follow along as we explore:


-


The key benefits of SaaS discovery


-


6 different ways to discover SaaS apps and the challenges of each method


-


How to leverage automation for effective SaaS discovery


## Why Shadow IT SaaS discovery is important


Every business unit has something to gain from having complete visibility into your SaaS environment. Overall, it helps strengthen the business’s security posture, simplify compliance, save on costs, and improve business efficiency.[Gartner projects that by 2027](https://www.gartner.com/en/cybersecurity/role/chief-information-security-officer) , 75% of employees will acquire or create technology outside IT's visibility, up from 41% in 2022.


With AI-driven tools entering the workplace at an unprecedented pace, organizations risk exposure to compliance failures and data breaches if they fail to maintain oversight. SaaS discovery ensures that both IT and business leaders can anticipate risks before they escalate into larger security or compliance incidents.


More specifically, SaaS discovery will help you:


-


Discover[shadow IT](https://www.accessowl.com/blog/shadow-it-how-to-deal-with-a-problem-you-cant-quantify) : **** You can’t protect what you can’t see. SaaS discovery helps you detect all apps, including those unsanctioned by IT, reducing security risks and compliance violations.


-


Prevent[SaaS sprawl](https://www.accessowl.com/blog/what-is-saas-sprawl) : SaaS purchasing is no longer controlled by IT. This decentralized procurement approach leads to tools with overlapping functionalities, causing inefficiencies and wasted spending.


-


Reduce the risk of missed offboarding: When you don’t know which apps a user is using, you can’t completely offboard them. They might retain access to sensitive company data long after they leave the company.


-


Improve workflow planning for IT managers: **** With complete visibility into the SaaS landscape, IT teams can effectively plan workflows, integrations, and app management strategies that promote efficiency and productivity.


Before implementing a formal[SaaS discovery process for the B2B marketplace Sary](https://www.accessowl.com/customers/sary) , Khalifah Alsadah noticed "in one instance I found out that a former employee was still using a critical internal system 3 months after he left."


## 6 Shadow IT SaaS discovery methods you can use: Step-by-step guide


Here are six ways you can start identifying the SaaS apps linked to your business domain. You’ll see that each method has its limitations, so you will have to combine multiple ways to get complete visibility.


Currently, most organizations no longer rely on a single discovery approach because remote work, flexible procurement, and decentralized teams make SaaS tracking increasingly complex. Instead, businesses combine manual methods with automation to ensure nothing slips through the cracks. That layered approach is now considered a best practice in SaaS governance.


### 1. Survey Employees


This is the simplest and most direct approach. Just ask your employees which apps they’re using. You can send out a survey or request a list of tools employees rely on for their daily tasks.


#### Pros


-


Easy to implement


-


Helps uncover tools that employees acquired without IT knowledge


#### Limitations


-


Employees may forget or overlook certain apps


-


Some may withhold information, especially if they’re using unsanctioned tools


-


Not all employees are aware of what constitutes a SaaS app, leading to incomplete results


This SaaS discovery method is a great starting point, but you’ll still need to implement additional methods to get a complete and accurate picture.


### 2. Audit OAuth logs in Google Workspace


Auditing OAuth logs in Google Workspace will help you discover all apps accessed using the “Sign in with Google” option. You’d be surprised how often this happens.


OAuth log auditing has become increasingly important as employees adopt AI productivity tools and automation services that often integrate through single sign-on. These connections can pose risks because third-party apps may request broad permissions that go unnoticed without regular audits.


#### Steps:


1.


Sign in to the Google Admin Console.


2.


Navigate to **Security > Access and data control > API controls** .


3.


Under **App Access Control** , review the third-party apps that have OAuth access to your domain.


4.


Check app permissions to determine if these apps have access to sensitive data.


#### Pros


-


Offers a straightforward way to discover apps without employee involvement


#### Cons


-


Can’t discover apps if employees didn’t use the “Sign in with Google” option


-


Sifting through the log is cumbersome, requiring extensive cleanup, manual vendor-name matching, and ongoing monitoring


As with the previous option, relying on this method alone will not give you complete SaaS app visibility.


### 3. Scan Email Activity for App Invitations


This is the holy grail when it comes to SaaS discovery in the modern IT environment. Whenever a user registers for a new SaaS tool, they’re typically sent a confirmation or notification email. By scanning email activity, you can identify these account creation notifications and uncover SaaS apps linked to your business domain.


This method is especially valuable in 2026 because most SaaS onboarding flows still rely on email confirmation. With the rise of AI-driven and niche SaaS products entering businesses every month, email activity often provides the earliest and most reliable evidence of adoption. However, scanning requires a balance between thoroughness and respecting user privacy.


However, with this method, you’ll need a third-party tool. We’ll explore how AccessOwl simplifies the process, but another option you can use is the[Google Apps Manager (GAM)](https://www.accessowl.com/blog/gam-beginners-guide-how-to-manage-google-workspace-at-scale) . This is a popular command-line tool that provides an alternative way for Google Workspace admins to manage domain and user settings.


#### Steps:


1.


Download and set up GAM. It’s a multiple-step process.


2.


Create a script to access the email inbox for all the users and search for keywords such as “welcome,” “confirmation,” or specific SaaS app names. Here’s an example of a simple GAM command to search a user's Gmail inbox for emails containing the word “ChatGPT”.


```text
gam user xyz@companyname.com show messages query  "ChatGPT"
```


```text
gam user xyz@companyname.com show messages query  "ChatGPT"
```


```text
gam user xyz@companyname.com show messages query  "ChatGPT"
```


1.


Compile a list of discovered tools based on these email notifications.


#### Pros


-


Provides a nearly foolproof way to detect SaaS app usage


#### Cons


-


Mastering GAM usage takes time


-


Scripting knowledge is required


-


Time and labor-intensive


-


Only works if you know what you are searching for


### 4. Check Expense Reports


Reviewing company expense reports is another excellent way to discover SaaS apps. If you’re specifically tracking apps for SaaS spend management, this method is sufficient in itself. Unfortunately, it doesn't account for free tools, which are still a security and compliance risk.


This method is highly relevant in 2026 because the cost of SaaS subscriptions continues to rise and finance teams are under pressure to optimize spend. While it does not catch free apps, it highlights hidden costs like forgotten subscriptions or duplicate licenses that contribute to SaaS sprawl.


#### Steps:


1.


Work with your finance department to obtain expense reports. Or look into credit card management tools like Ramp or Brex.


2.


Identify recurring payments or subscriptions related to software services.


3.


Create a list of apps in use based on the data you collect


#### Pros


-


Excellent for tracking paid SaaS tools for SaaS spend management


#### Cons


-


Doesn’t account for free tools


-


Requires collaboration with finance teams, which could slow down the process


### 5. Examine Network Activity


This was once a popular method to identify SaaS apps but has become less effective as businesses transition to hybrid and remote working models. This method relies on third-party tools like a Cloud Access Security Broker (CASB) to scan network traffic and identify the URLs and services users visit. You’ll then need to translate the data into specific apps that are in use within the business.


Nowadays, this method is less reliable because employees often work from home or use personal devices that bypass company-controlled networks. Still, for organizations with centralized office environments or regulated industries where CASB remains a requirement, network analysis can provide useful real-time visibility into SaaS activity.


#### Pros


-


Effective in traditional office environments with a central network


-


Gives real-time data on app usage


#### Cons


-


Less effective in remote/hybrid environments


-


Involves filtering through lots of data


### 6. Browser Extension


You can also use a browser extension that monitors the user's browser window to try and identify instances where they’re logging in to a SaaS app.


While this method provides strong visibility, by 2026 employees are increasingly concerned about digital privacy. Many organizations face pushback when rolling out monitoring extensions, especially as regulations around employee tracking tighten. Adoption often depends on creating clear communication about what is being monitored and why.


#### Pros


-


Can help you identify all kinds of SaaS apps regardless of the sign-in method


#### Cons


-


Relies on employee compliance, which can be difficult to enforce


-


Privacy concerns may lead employees to reject the extension


-


Employees can switch to a different browser


## How to simplify SaaS discovery through automation


If you went through the various steps and thought it’s a lot of work, I have good news for you. Automated Shadow IT discovery tools can significantly simplify the process. It eliminates the chances of missing some tools and will save you a lot of time and effort.


Manual discovery cannot keep up with the sheer number of apps employees adopt, nor can it scale with global teams working across multiple devices. An automated joiner-mover-leaver flow closes that gap by making discovery part of offboarding instead of a separate chore.


The fastest way to see your own shadow IT is a[free shadow IT scan](https://www.accessowl.com/scan) from AcessOwl. It leans on Method 2 (Google Workspace or Microsoft OAuth logs) to catch tools in your domain that are set up outside of SSO.


Remember, SaaS discovery is an ongoing process. Even after a thorough manual discovery, new apps are regularly added, requiring continuous effort to keep up with the latest tools employees are adopting.


This is where SaaS management apps like[AccessOwl](https://www.accessowl.com/) come in.


## Why AccessOwl is the best SaaS discovery tool


It’s a lightweight, easy-to-use tool that you add on top of Google Workspace for advanced SaaS app management. AccessOwl simplifies SaaS discovery and management by combining the two most effective methods (continuously auditing OAuth logs and scanning email activity). If you want to compare Shadow It discovery tools side by side see our breakdown of the[best Shadow IT tools.](https://www.accessowl.com/blog/shadow-it-tools-compared-buyers-guide)


Now, IT leaders need solutions that deliver continuous visibility without sacrificing employee experience. AccessOwl does exactly that, providing a seamless way to track app usage across distributed teams, personal devices, and cloud environments. By automating discovery and offboarding, it reduces the burden on IT while improving compliance and data protection.


It doesn’t matter whether employees are using personal devices, working remotely, or connecting through various networks. AccessOwl gives you full visibility into your organization’s SaaS landscape without the need for manual intervention or scripting expertise. Better yet, it works in the background without interfering with employees’ privacy or productivity.


During employee offboarding, AccessOwl will automatically delete a user’s account across all associated SaaS apps (deprovisioning), including those that were unidentified during onboarding. On top of SaaS discovery, AccessOwl helps automate SaaS user access, user account management, and SaaS vendor management, making it the perfect security and compliance partner for IT admins.


In short, AccessOwl serves as your single source of truth for all SaaS information, including:


-


SaaS applications linked to your business


-


SaaS users and their associated permissions


-


Vendor information including data location, type of data processed, and compliance status


It also comes with a renewal calendar and SaaS cost tracking for effective SaaS spend management.


Ready to discover all the SaaS tools used across your organization? Start with our[free shadow IT discovery](https://www.accessowl.com/scan) tool and gain full visibility into your environment.


## FAQ


### How do I detect shadow IT in Google Workspace?


You can start detecting shadow IT by combining four manual methods: survey employees on the tools they use, audit OAuth grants in the Google Admin console, scan mailboxes for signup and confirmation emails, and review expense reports for software charges. Each method has blind spots, use them together. If the manual work gets too time-consuming, a shadow IT discovery tool like AccessOwl automates discovery.


### How do I stop employees from bypassing Google SSO?


You cannot fully prevent it, because any tool can offer email-and-password signup instead. You can reduce it by setting API controls to limit which third-party apps connect, publishing an approved-tools list, and running discovery often so new signups surface quickly.


### How does shadow IT create offboarding risk?


You cannot remove access from a tool you do not know exists. When someone leaves, any app they signed up for outside SSO can keep their account and data active long after their Google account is suspended.


### **How do I detect apps employees signed up for with a username and password**


If the account never used "Sign in with Google" and never touched your directory, auditing OAuth grants will not surface them. Email scanning is a more reliable method: the signup, welcome, or confirmation email still lands in the user's mailbox no matter how the account was created. Email scanning can be set up programatically with Google Apps Manager. Looking at expense reports can also catch paid tools but may not tell you who the account owner is.
