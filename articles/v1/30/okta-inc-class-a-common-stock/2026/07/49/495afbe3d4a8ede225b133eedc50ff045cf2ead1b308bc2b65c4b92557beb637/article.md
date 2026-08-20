---
schema_version: "1.0.0"
document_id: "495afbe3d4a8ede225b133eedc50ff045cf2ead1b308bc2b65c4b92557beb637"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/threat-intelligence/behind-the-scenes-of-a-vishing-operation/"
published_at: "2026-07-27T16:00:00+00:00"
first_seen_at: "2026-07-28T15:25:44.666639+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:0a666c2b1e6ed1d30b412a60b00d4b7ebfdcfe69abd781d3c92f9fc9b6a6f7d5"
---

# Behind the scenes of a vishing operation

## Executive Summary


Okta Threat Intelligence recently obtained material from a sensitive source that provides an inside view of *Work Panel* , an operator console used by threat actors running vishing campaigns that target customers of multiple identity providers, including Okta.


*Work Panel* is a multi-tenant platform that packages everything an operator needs to run a vishing-driven account takeover operation. Registering a new phishing domain, cloning a target brand, and standing up a new isolated phishing site are each one-button operations. New campaigns can be launched in minutes.


The platform is designed to be sold to multiple operators. Each operator brings their own integrations (registrar account, content-delivery keys, B2B contact-data API key, Telegram bot, SIP provider) and their own callers. Multiple intrusion clusters can run their own deployments in parallel.


Our analysis of *Work Panel* offers unique insights into the day-to-day operations of vishing actors. Infrastructure owners, campaign managers and outsourced callers each sign in to Work Panel with distinct roles, exposing in the process a cybercrime ecosystem in which the social engineer on the call is increasingly treated as interchangeable labor. By design, the human callers conducting vishing campaigns cannot view or access the credentials they help their ‘manager’ to capture. That insider-risk control gives operators exclusive access to the spoils of a campaign and significant leverage over the individuals that agree to make the calls on their behalf.


The only persistent components are the platform, the infrastructure pipelines, and the recruitment networks that feed it.


Figure 1: Work Panel login page


Fill out the form to access this content.


## Threat Analysis


This advisory walks through what a single vishing operation looks like from the operator side using *Work Panel* , an active operator console used by at least one of the intrusion clusters Okta has been tracking as **O-UNC-045** aka **CORDIAL SPIDER** .


*Work Panel* is not a single phishing page or a script. It is a full web application designed to run a vishing-driven account takeover business. The console organizes the work into multiple sections covering target recon, voice-call routing, brand cloning, infrastructure provisioning, live session management, captured-credential review, and an audit log. Different roles see different tabs, and the access boundary is enforced on the server rather than hidden in the client.


This is a meaningful step away from the phishing kit pattern that dominated earlier credential-theft operations. A traditional phishing kit ships as a folder of HTML and PHP files that a single operator deploys, watches, and manages alone. Work Panel ships as a service. Operators deploy it once and then onboard a workforce.


Figure 2. The Work Panel user registration page


Fill out the form to access this content.


## IAM: Three roles, three workspaces


The operator workforce in Work Panel is structured around three server-enforced roles: caller, manager, and admin.


Each role uses a different part of the panel. The platform’s author is explicit in their own help guide that the split is enforced server-side and not just hidden in the UI.


### The caller role


The caller, who is hired by someone who wants to run a campaign with *Work Panel* , is the person who actually dials the targeted user.


The caller’s workspace is the most restricted of the three roles. The caller has access to a target contact lookup too, a SIP credentials tab and a limited mailer for sending pretexting emails. The SIP credentials tab displays the username, password, server, and domain assigned to them by the admin for use in an external softphone.


Figure 3. The caller workspace


Fill out the form to access this content.


The caller does not see the live session queue nor the credentials they help capture. They also have no visibility into the operational infrastructure or the targets assigned to any other caller.


### The manager role


The manager supervises a team of callers.


The manager workspace includes additional features including a live target queue, a captured-credential review panel and a workforce administration tab. That admin tab lets them onboard or remove callers, generate one-time registration codes for new hires, assign callers to specific phishing sites, and monitor the activity log.


A manager can only manage the callers they personally onboarded or that the admin explicitly assigned to them.


Figure 4. The manager workspace


Fill out the form to access this content.


### The admin role


The admin owns the platform and has likely purchased it from the platform’s author.


In addition to everything the manager sees, the admin manages the infrastructure pipeline (domain registration, content delivery, DNS), rotates the platform's API keys, holds a kill switch, and configures their personal Telegram notification channel.


There is typically one admin per deployment.


Figure 5. The admin workspace


Fill out the form to access this content.


## Caller operations


The caller workspace is a tightly scoped tool for one job: get a targeted user on the phone and keep them on the page until the manager confirms the credentials are captured.


A caller starts in the Company Lookup tab. The tab is a thin wrapper around RocketReach, a commercial B2B contact-data service. The caller types a target company's email domain and the tool returns a list of employees with their corporate email addresses, direct phone numbers, job titles, and links to their LinkedIn profiles. The caller picks a target by title, typically someone whose role makes a helpdesk-themed pretext plausible.


Figure 6. Callers use the Company Lookup, a thin wrapper around RocketReach, a commercial B2B contact-data service, to find targets.


Fill out the form to access this content.


This recon step is the bridge between the technical infrastructure and the human social engineering that follows. It is also why these calls feel personalized. By the time the phone rings, the caller already knows the target's job title, their direct phone number, and often their manager's name. The data is not stolen. It is purchased through legitimate channels, then weaponized inside *Work Panel* .


The caller opens the SIP tab to retrieve their assigned SIP credentials, username, password, server and domain, and loads them into an external softphone application to place the call. *Work Panel* itself has no calling capability. The SIP tab is only used to pass credentials to the caller for a SIP service. Each caller's credentials are unique, assigned by the admin and visible only to that caller.


User credentials captured by the phishing kit are not displayed to the caller's screen. When the target lands on the phishing page and submits their password and MFA approval, those values flow to the manager's Sessions tab and are not provided to the caller role. The caller's job, in the kit author's own framing, is "Company Lookup and SIP." Page pushes and credential handling are explicitly described as manager-only work.


## Manager operations


While the caller is on the phone with a targeted user, the manager is watching a live target queue.


Each row in the queue represents a target's browser that has landed on a phishing page. The row shows a status (live, idle, or stalled, based on how recently the browser pinged the phishing page), a country flag, an identifier (the first email or username the target typed), and the page the browser is currently displaying.


Figure 7. The manager's session queue


Fill out the form to access this content.


The manager's primary control is a "Push" button on each row. Push opens a small menu of phishing-flow pages the manager can send the target's browser to. The list of pages is fixed by the kit, so the manager cannot send a target to an arbitrary URL. The fixed menu includes the typical authentication challenge pages (push notification, number matching, authenticator app code, a support-ticket completion screen) for each cloned brand. The manager advances the target through the flow page by page, in real time, while the caller talks the target through each step on the phone.


When the target submits their credentials, the captured values appear in a session detail panel next to the queue. The panel shows the username, password, and any MFA codes intercepted. Each value has a copy button, and every copy is logged. If the manager's deployment has a Telegram bot configured, a notification with the captured material lands in their personal Telegram channel within seconds.


The session detail panel also shows a timeline of every page the manager pushed and a tap-code field for the number-matching MFA flow. The mechanics of the number-matching bypass are covered in the threat advisory “[Vishing Operators Synchronize Phishing Sites to their Script for Hybrid Social Engineering Attacks](https://security.okta.com/product/oktathreatintelligence/vishing-operators-synchronize-phishing-sites-to-their-script-for-hybrid-social-engineering-attacks) ”.


## Admin operations


The admin's workspace is essentially a self-service infrastructure console. Where a traditional phishing kit operator would manually register a domain, configure DNS, set up a web server, deploy the kit, and configure TLS, *Work Panel* does all of that from buttons in the admin tab.


The flow is automated end to end. The admin types a domain name into the 'Buy Domain' form; the platform checks availability against NiceNIC, charges the operator's registrar account, registers the domain, and spins up a Cloudflare DNS zone. On the server side, the platform provides a dedicated Caddy reverse-proxy block per phishing site. Caddy is a lightweight web server that handles HTTPS termination and routes incoming traffic to the appropriate backend.


Figure 8. Admin domain configuration


Fill out the form to access this content.


Each site also receives a PM2 process and a configuration file; PM2 is a Node.js process manager that runs the site as a supervised background service, restarting it automatically if it crashes.


Bunny CDN handles the click-redirect hosts used in the email channel. All three sets of API credentials, NiceNIC, Cloudflare, and Bunny CDN, are stored server-side and never exposed in the client. A new phishing site for a new target is ready within minutes of domain registration. Operators can also import a Cloudflare zone for a domain registered elsewhere.


With the domain live, the admin clicks 'Add Panel', selects a sign-in flow template (Okta, Microsoft 365, or Salesforce) and supplies a clone source. The template selection is permanent: it determines which paths the phishing site renders (/sign-in/* for Okta, /common/oauth2/v2.0/* for Microsoft, /community/* for Salesforce), and cannot be changed after creation.


When targeting Okta, pasting any sign-in URL automatically retrieves the target organization's logo, favicon, primary color, company name, and tenant domain. When targeting Microsoft, pasting any corporate email address pulls the tenant's logo, header background, welcome text, and brand colors via Microsoft's own tenant-discovery endpoints. Salesforce panels use the /community/* paths but have no equivalent automated extraction step.


Figure 9. Adding a phishing panel


Fill out the form to access this content.


Each panel runs on its own subdomain with its own process, configuration, and web server block, entirely independent of any other active panel.


The admin can also restrict which device operating systems are permitted at the gate; traffic from disallowed platforms receives an error page rather than the sign-in flow.


The admin tab also exposes the operational controls that matter most for an operator who expects to be tracked. There is a button to rotate the platform's content-delivery API key without redeploying. There is a self-destruct endpoint that tears down every phishing site, every supporting process, and every DNS record in a single action. There is an activity log that captures every action every other operator on the platform has taken, so the admin can audit the work of their hired labor.


There is also a Telegram-notification configuration that the admin populates with their own bot token and chat ID. No operator credentials of any kind are baked into the platform.


The platform also supports a second attack channel alongside the vishing calls. Operators can compose branded phishing emails directly in the panel using a built-in template editor. Templates are written with {{variable}} placeholders, such as company name, Okta domain, or primary color, that are automatically filled in from the brand profile the operator cloned when setting up the phishing site.


Figure 10. A Work Panel phishing email template.


Fill out the form to access this content.


An operator who has already cloned a target's sign-in page can send a phishing email to the target's employees with no further setup. Outbound mail is sent through operator-configured SMTP servers, each tied to a sender domain provisioned through the same infrastructure pipeline. All links in outbound email are wrapped through Bunny CDN redirect hosts, providing click-tracking and a layer of separation between the email channel and the phishing panel.


## Commentary


Work Panel is a complete, cybercrime-as-a-service software package. It accommodates the key requirements to execute successful phishing attacks: research, infrastructure, and operational security.


- **Insider risk is engineered out.** The separation of duties is not incidental. It addresses a problem intrinsic to running a criminal operation with hired labor. Callers are sourced from underground channels, paid per successful capture, and carry no institutional loyalty. Without access to the credential archive, a caller who wants to act for personal gain by selling captured material to a competing operator, for example, has nothing to sell. The most valuable output of the operation is locked in exclusively with those at the top of the hierarchy.
- **Voice phishing callers are easily replaced** . Callers do not have access to the operation's most valuable asset (stolen credentials), so they do not need to be trusted. They can be recruited through public underground channels with a brief test, paid per successful capture, and replaced easily. Recruitment ads for fluent English-speaking callers offering large financial returns have been observed on Telegram and tied to operators using this kit (see “[Vishing Operators Synchronize Phishing Sites to their Script for Hybrid Social Engineering Attacks](https://security.okta.com/product/oktathreatintelligence/vishing-operators-synchronize-phishing-sites-to-their-script-for-hybrid-social-engineering-attacks) ”).
- **Operational resilience is built in** . Every phishing site runs in its own process under its own configuration. The compromise of one site does not cascade to the others. API keys for every external service are rotatable from the panel. The self-destruct endpoint provides a one-click exit if the operator suspects their hosting environment has been seized.
- **The platform is multi-tenant by construction** . Every operator brings their own integrations (their own registrar account, their own content-delivery keys, their own B2B contact-data API key, their own Telegram bot, their own SIP provider, their own callers). This enables the kit author to sell access to many independent operators at once without holding the credentials for any of those operators.
- **Operator oversight is built in.** From the Live View tab, an admin can select any online caller and watch a real-time stream of their console, exactly as it appears on the caller's own screen, embedded directly in the panel. The caller is never told they are being watched: the kit author's states plainly that the window can simply be closed to stop. The only data withheld from the admin's view is anything the caller types into a password field. Every other input, message, and screen the caller sees while working is visible. Every action by every operator, credential copies, bans, role changes, code issuances, settings edits, is recorded in an activity log, auditable and exportable at any time.
- **The kit treats its own workforce as an interchangeable and monitored labor pool** . The platform supports that labor model by stripping callers of access to anything sensitive. The result is an operation that is harder to disrupt by removing any single person, harder to follow by tracking any single domain, and easier to rebuild after a takedown than the phishing operations that came before.


Figure 11. Admin configuration settings


Fill out the form to access this content.


### What Work Panel teaches us about modern vishing operations


We assess it is likely that Work Panel was “vibe-coded” - built with substantial AI assistance. The console's design system, which includes pre-built cards, toasts, motion effects on nearly every transition, is consistent with the default output of AI coding tools when they are asked to scaffold a dashboard. This would lower the bar for one person to ship and maintain a SaaS-grade platform for criminal customers.


Treating *Work Panel* as just another phishing platform understates the development. The features it ships with are the features a SaaS product team would build if they were building a CRM for vishing crews. Workflow specialization, role-based access control, infrastructure automation, third-party integrations, audit logging, rotation of secrets, and a tested exit procedure are all standard SaaS engineering practices. They are now standard on the offensive side too.


The intrusion clusters using these platforms are not single individuals. They are small teams, organised around clear roles, with at least one role (the caller) treated as commodity labor. Recruitment for that role is happening in public underground channels.


This split between operators who own the infrastructure and callers who supply only their voice is not unique to Work Panel. It has become a repeating pattern in organized vishing: multiple independent actors have adopted the same model, advertising openly for hired callers while retaining the platform, the target data, and the credential capture for themselves. Work Panel formalizes what was previously an informal arrangement, but the underlying labor model was already consolidating across the market.


The most important practical implication of this organizational maturity is that the social engineering the targeted user experiences has been engineered to be as convincing and as resilient as possible. The credential-capture pipeline that supports that experience is increasingly automated.


The defensive controls that work against this kind of operation (see table below) are the same ones that worked against earlier credential-phishing campaigns. What changes with Work Panel is the timeline. New campaigns can be stood up faster, new brands cloned in minutes, and disrupted infrastructure rebuilt in hours.


## Recommendations


ATT&CK Tactic


Control Recommendation


T1566


Phishing


Enroll users in strong authenticators such as Okta FastPass, passkeys or[smart cards and enforce phishing resistance in policy.](https://help.okta.com/oie/en-us/content/topics/identity-engine/authenticators/smart-card-authenticator.htm)


Establish, communicate and evangelise methods of verifying the identity of helpdesk personnel when they contact users.


T1078


Phishing


Deny requests from locations where your organization does not offer services. Okta[network zones](https://help.okta.com/oie/en-us/content/topics/security/network/about-enhanced-dynamic-zones.htm) allow administrators to set policies that deny access to Okta-protected applications by geolocation (country), ASN, IP, or other criteria.


T1078


Valid Accounts
(Initial Access)


Okta authentication policies can be used to restrict access to user accounts based on a range of customer-configurable prerequisites. We recommend administrators restrict access to sensitive applications to devices that are[managed by Endpoint Management tools](https://help.okta.com/oie/en-us/Content/Topics/identity-engine/devices/managed-main.htm) and[protected by endpoint security tools.](https://help.okta.com/oie/en-us/Content/Topics/identity-engine/devices/edr-integration-main.htm)


T1078


Valid Accounts
(Initial Access)


Notify users of every authenticator (factor) lifecycle event using[end user notifications.](https://help.okta.com/en-us/content/topics/security/healthinsight/notifications-factor-enroll.htm)


T1098


Account Manipulation (Device Registration)


Apply[Okta Account Management Policies](https://help.okta.com/oie/en-us/content/topics/identity-engine/policies/oamp.htm) that constrain the ability to add or modify authenticators based on network context, device management status and enrolled authenticators.


Specific guidance is provided in the following blog post:
[https://www.okta.com/en-au/blog/threat-intelligence/intrusion-actors-self-serve-their-way-into-accounts/](https://www.okta.com/en-au/blog/threat-intelligence/intrusion-actors-self-serve-their-way-into-accounts/)
