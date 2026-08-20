---
schema_version: "1.0.0"
document_id: "b1249a94fd199b9f3aaebe293ad87fd64a58ecebc0c09d0bfcafee8f14c17a56"
company_key: "yc-riot"
company: "Riot"
source_id: "yc-riot-news-import-992df223e3e3"
canonical_url: "https://tryriot.com/blog/phishing-false-positives/"
published_at: null
first_seen_at: "2026-07-22T11:58:46.553977+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:0b5d31789034bc3afc325a8b86d168a801c19237d06419f48f289e80398636d3"
---

# No More False Positives – Our 4-Step Method for Great Phishing Simulation Data

We run phishing simulations to protect our teams from real scammers. But to truly keep us safe, we need these simulations to give us dependable data on who is detecting attacks safely, and who is still vulnerable. Unfortunately, false positives can make this a real challenge.


**In this article, we explore the tricky problem of false positives in phishing simulations, and look at why users of platforms like KnowBe4 seem to experience so many of them. We’ll show you our four-step process for solving false positives at Riot – and how it helps our users.**


But first, let’s look at a basic question: what are false positives, and why do some phishing simulation platforms produce so many of them?


## What are false positives in phishing simulations?


**In phishing simulations, false positives occur when legitimate user actions in response to test emails (or messages) are flagged as failures by mistake.**


For example, let’s say a user has enabled antivirus software which scans all emails received and ‘clicks’ links in advance. In this case, your phishing simulation tool may register these clicks as failures in response to a test, when in reality the user hasn’t interacted with the test email at all.


When running simulations, there are a few signs you might have a problem with false positives. For example, if you send a phishing test and start getting fail notifications in just a few seconds, it’s highly likely these are false positives. The same goes for campaigns with 100% fail rates.


False positives might sound like just an occasional inaccuracy – but the problem is much bigger than that. In fact, some studies have found[a fifth of all cybersecurity alerts are false positives](https://www.securitymagazine.com/articles/97260-one-fifth-of-cybersecurity-alerts-are-false-positives) .


If you’re running phishing simulations to help you identify the members of your team who are most vulnerable to real attacks, false positives are more than just a bug in the system. In some cases, they can actually increase your risk of getting phished for real.


***Learn more:[How to Run a World-Class Phishing Simulation in 6 Steps](https://tryriot.com/blog/phishing-simulation/)***


## The 5 biggest risks of relying on false positives


False positives might sound like just a minor nuisance, but they’re actually a big deal. Here’s why.


### #1: False positives make it harder to optimise phishing awareness training


Running consecutive phishing simulations allows you to set a baseline of vulnerability, identify who on your team needs more training to detect attacks safely, and[reduce your risks to cyber threats](https://tryriot.com/blog/biggest-cyber-threats/) over time. But to do this, you need to be able to trust your results.


If not, it may seem like your teams are more susceptible to phishing attacks than they actually are. You might end up sending subsequent phishing tests or training courses to the wrong cohorts, wasting everyone’s time, energy, and money. And speaking of money…


### #2: False positives make it impossible to allocate resources correctly


False positives can make it impossible to allocate the right training resources, creating the risk that some of your learners end up completing their phishing awareness training over and over without really needing to. Even worse, those that *do* need the training might end up missing out.


When our[cybersecurity budgets are under the microscope](https://www.techtarget.com/searchsecurity/feature/Cybersecurity-budget-trends) , false positives are a mistake we simply can’t afford to make.


### #3: False positives prevent clear compliance and reporting


Every successful organization runs on good data – and phishing simulations are no exception. Simply put, if leadership can’t trust your simulation results and track the risks of real attacks, they won’t know whether you’re moving the needle in the right direction.


Even worse, inaccurate data can also make it much more challenging to confirm your compliance with cybersecurity regulations and report to your stakeholders. In the worst case scenario, this could even have legal implications for your organization.


***Need some help staying on top of your cybersecurity metrics? Check out our[free checklist](https://tryriot.com/toolbox/checklist-12-cybersecurity-metrics/) .***


12 Essential Cybersecurity Metrics


### #4: False positives can be a huge drain on employee morale


Failing a phishing simulation is a stressful experience for anyone. Even worse? Being flagged for a mistake you didn’t even make. In a worst-case scenario, false positives can erode trust in your[cybersecurity awareness training](https://tryriot.com/blog/cybersecurity-awareness-culture/) , leading to lower engagement and fewer completions.


Not to mention, false positives are a big pain for your security team to have to keep chasing up. In extreme cases, they might even[create the risk of burnout](https://www.intelligentciso.com/2024/03/07/fighting-back-against-cybersecurity-burnout/) for you or your staff.


And last but not least…


### #5: False positives can make your phishing simulations pointless


Running phishing simulations involves an investment of time, energy, and relationship capital. Not only do you have to invest in the platform itself, but you have to convince your teams to get on board with being tricked in a safe environment.


So, if at the end of all that, your simulations aren’t even telling you who is still vulnerable, then why should you run them in the first place?


Now, let’s look at another big question: Why do some phishing simulation tools produce so many false positives? And more importantly, why are so many users still putting up with it?


## Why are users of platforms like KnowBe4 getting so many false positives?


**At Riot, we’re always curious about what’s happening with the competition. And lately, we’ve noticed an uptick in discussion about false positives with other simulation tools, with[KnowBe4](https://www.reddit.com/r/sysadmin/comments/1cgv5h2/recent_false_positives_with_knowbe4_campaign/) ,[Proofpoint](https://www.reddit.com/r/proofpoint/comments/1ertc3a/proofpoint_false_positive_block_ip_and_no/) , and even[Microsoft](https://www.channele2e.com/brief/microsoft-resolves-widespread-false-positive-malware-email-issue) users flagging issues.**


There are a few reasons as to why this might be happening:


- Every great phishing platform needs great user onboarding. If user organizations aren’t getting the right guidance on how to avoid false positives safely and efficiently, it’s no wonder they’re running into trouble.
- Some phishing platforms may not be paying close enough attention to the latest developments in automated security solutions, meaning their simulations aren’t capable of detecting legitimate responses to phishing tests and sorting these from the real fails.
- Some platforms may not give users the ability to perform manual reviews of failures in response to phishing tests, making it harder for system admins to investigate and resolve false positives.


Fortunately for our users, we’re fighting the good fight against false positives.


***Learn more:[KnowBe4 vs. Riot: Which Platform is Right for Your Team?](https://tryriot.com/blog/knowbe4-vs-riot/)***


## How Riot fights false positives in 4 steps


We know how important it is for our users to have clear, dependable data when it comes to their phishing simulations. After all, you’re putting in the time and effort to craft something sneaky enough to test your team – the least we can do is make sure you only count the true fails!


**Here’s how we fight false positives in four steps at Riot.**


### Step 1: Build a detailed profile of how real users behave


Most false positives start when security systems automatically scan or check phishing simulation emails and the links they contain. To solve for this, we’ve built a detailed profile of how our *real* users behave, which we use to filter out behavior that *doesn’t* originate from real users.


For example, if a user clicks on a test email a hundredth of a second after it was sent, this tells us it’s likely just a headless browser or Chrome extension. Similarly, real users have plugins and browser language preferences – so we screen for users with no plugins or language settings.


Of course, security systems are evolving every day, becoming more nuanced and sophisticated. That’s why we’re constantly updating our user profile to keep up.


### Step 2: Detect and block thousands (and thousands!) of bots


Another critical step is to detect and block bots from interfering with our users’ phishing simulation results. And we’re not just talking about a couple of bots here and there – each month, we block between 4,000 and 5,000 bots (and counting). But to do this, we need the right tools.


At Riot, we use[Fingerprint](https://fingerprint.com/) to filter bots from creating inaccurate data. This tool provides accurate insights into user touch points, sorting through unique identifiers for each machine to establish who are the real users.


We use a variety of backend techniques to prevent false positives, including IP address checks and checking for banned IPs. For example, in our most recent update, we’ve started filtering for users running[Chrome DevTools Protocol](https://developer.chrome.com/blog/cdp-command-editor) (CDP), as this tells us a user is probably a bot.


### Step 3: Work closely with our users to configure simulations correctly


To avoid false positives in phishing simulations, we also work closely with our users to make sure their campaigns are correctly configured. In practice, this means:


- ‘Whitelisting’ (approving in advance) domains used in phishing simulations, meaning user security systems such as firewalls won’t automatically block the test.
- Encouraging users to be aware of how the wider set of tools they use could interfere with their simulations. For example, if users are running[Selenium](https://www.selenium.dev/) to automate their browsers, this could result in a spike in false positives.
- Helping system admins to define the user actions that will be considered as failures in response to phishing tests – and communicating these clearly to their teams.


### Step 4: Rinse and repeat!


Preventing false positives in phishing is an ongoing project. That’s why we’re constantly repeating these steps to keep phishing simulation data as clear and accurate as possible. And while it’s impossible to prevent 100% of false positives, our goal is to get as close as we can.


## Don’t settle for bad phishing simulation data


Running great phishing simulations takes time and energy. You need to craft the right hooks, track your campaigns, and follow up with awareness training to improve your security posture.


One thing you really don’t need? False positives. That’s why we’re working so hard to stop false positives and ensure our users have dependable, accurate, and actionable simulation data.


**To find out how Riot can help you and your team stay on top of the latest cyber threats with fantastic phishing simulations and clear data,[get in touch with one of our experts today](https://tryriot.com/demo/?utm_source=direct_traffic&utm_medium=blog) .**


[Spear Phishing in 2026: How AI-Powered Attacks Bypass Traditional Defenses Benjamin Netter Founder](https://tryriot.com/blog/what-is-spear-phishing/)[3,000 Years Later, What Can The Odyssey Tell Us About the Birth of Social Engineering? Tom Baragwanath Head of Content](https://tryriot.com/blog/odyssey-social-engineering-interview/)[Introducing Slash: Email Security for the New Generation of Phishing Bettina Sarafis Product Manager](https://tryriot.com/blog/slash-email-security/)[Phishing Training for Employees: The Complete 2026 Guide (That Actually Changes Behavior) Tom Baragwanath Head of Content](https://tryriot.com/blog/phishing-training-for-employees/)[Shadow IT: Why It Happens, Why It's Risky, and How DLP Can Help Thibault Bernard Product Manager](https://tryriot.com/blog/shadow-it/)[Ransomware Training: How to Reduce Risk Without Wasting Budget Benjamin Netter Founder](https://tryriot.com/blog/ransomware-training/)
