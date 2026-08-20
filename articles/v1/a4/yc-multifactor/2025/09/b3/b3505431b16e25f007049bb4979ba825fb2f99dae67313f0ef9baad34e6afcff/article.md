---
schema_version: "1.0.0"
document_id: "b3505431b16e25f007049bb4979ba825fb2f99dae67313f0ef9baad34e6afcff"
company_key: "yc-multifactor"
company: "Multifactor"
source_id: "yc-multifactor-news-import-864f66d5ce56"
canonical_url: "https://multifactor.com/blog/aaa-threat-agentic-solutions"
published_at: "2025-09-08T00:00:00+00:00"
first_seen_at: "2026-07-22T05:04:58.540173+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:d85025d7b846a7f78a03b014ee4c8711a6c31f0dadabfedaf455b4bb4b449bba"
---

# Addressing the AAA Threat: AuthN, AuthZ, and Auditing for Agentic Applications

# Addressing the AAA Threat: AuthN, AuthZ, and Auditing for Agentic Applications


2025-09-08


[Company](https://multifactor.com/blog/tags/company)[Research](https://multifactor.com/blog/tags/research)[Agentic](https://multifactor.com/blog/tags/agentic)


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


In our[previous post](https://multifactor.com/blog/aaa-threat-agentic-apps) , we introduced the “AAA threat” for agentic applications: Authentication, Authorization, and Auditing of AI agents on behalf of an end user. While applications that use AI agents promise to deliver high value to organizations and individuals, the lack of robust security measures today makes these utilities fundamentally insecure and virtually unusable in sensitive enterprise contexts. As a result, Multifactor’s agentic security platform is not just about safety, it’s also about enabling entirely new capabilities.


As detailed in our previous post, attacks against AI agents have already been disastrous, and the means of executing them are often trivial. For instance, the recent[“Invitation is All You Need”](https://www.wired.com/story/google-gemini-calendar-invite-hijack-smart-home/) article showed that a calendar invite can cause Google’s Gemini to trigger IoT devices, download malicious files, delete emails and files, and exfiltrate sensitive data. There are vulnerabilities like this discovered daily (such as this[zero-click Cursor vulnerability](https://www.lakera.ai/blog/zero-click-remote-code-execution-exploiting-mcp-agentic-ides) ), and we routinely monitor for such attacks and post about them on our[@AgenticSec](https://x.com/AgenticSec) account.


To contend with these issues, Multifactor offers solutions that completely address the AAA threat using underlying technologies of[MFKDF2](https://multifactor.com/blog/introducing-mfkdf2-mfchf2-mfdpg2) ,[Switchboard](https://multifactor.com/blog/introducing-switchboard) , and[Checkpoint](https://multifactor.com/blog/introducing-checkpoint) . The products built atop these primitives will enable agentic applications to be production ready for millions of users, allowing them to take advantage of a whole new way of interacting with the web.


## Authentication


Recently,[Cloudflare and Browserbase](https://www.browserbase.com/website/blog/cloudflare-browserbase-pioneering-identity) proposed a new standard for authentication of agents and bots called[Web Both Auth](https://developers.cloudflare.com/bots/reference/bot-verification/web-bot-auth/) . This is a great start for allowing agents to identify themselves as a known agent (as opposed to malicious bot traffic), but addresses a very different threat than the one we’re concerned about here. Fundamentally, Web Bot Auth is a spam prevention technique, not a security feature for an end user. More concerning is the lack of mechanisms for securely authenticating agents into legacy online accounts.


What is the most secure way to have an agent (or another person, for that matter) login to your online account? For one, you cannot let an agent consume your account’s credentials into its context window, just as you shouldn’t just give your password itself to another person. As we saw in the “Invitation is All You Need” article, credentials stored in context could then be coerced from an agent and exfiltrated elsewhere, thereby compromising the entirety of your account. And, as always, model context can be seen in the clear by the model providers, but it is best to avoid giving these credentials to another organization. Even if you’re not worried about OpenAI using these credentials themselves, a compromise of their platform would leave your sensitive credentials in the hands of an attacker.


This is where the combination of[MFKDF2](https://multifactor.com/blog/introducing-mfkdf2-mfchf2-mfdpg2) and[Checkpoint](https://multifactor.com/blog/introducing-checkpoint) come to the rescue. MFKDF2 allows for the safe storage of all of your credentials in a familiar password management system that does not weaken the security associated with these individual accounts since MFKDF2 cryptographically binds multiple authentication factors together to generate stronger keys. Fortunately, MFKDF2 also provides familiar UX, and boasts additional features such as native social recovery. Hence, you can comfortably secure your authentication credentials behind an MFKDF2 key or derive them or passkeys on the fly using MFDPG2.


With all of your online accounts at one’s fingertips, users can then securely share accounts with anyone else, including agents, using Checkpoint. The Checkpoint system requires that an agent gets logged into your account through a proxy that securely injects credentials on requests and strips away sensitive information on response. This means that the agent itself never sees any private information, but they can still access the account to perform their intended actions. Since this is all built on top of authentication protocols commonly used on the web today, Checkpoint works for humans just as well as it does for agents (unlike many other “agentic gateways” that are being offered today).


## Authorization


Once the agent is logged in, how do we make sure it only uses specific account features? Various recent attacks have shown that once authenticated, there is no guarantee that agents will then use accounts for their intended purpose, meaning that users also need to specify and enforce their authorization policies in a robust way. For example, an accountant may want to give an agent access to a collection of accounts (which could be bank accounts shared by customers), but the agent should not be able to change any account settings or make any transfers. Rather, the agent gets effective “read only” access so that they can collect data, but not modify anything. If certain items shouldn’t be read, fine grained control can limit this even further.


Multifactor has developed[Switchboard](https://multifactor.com/blog/introducing-switchboard) for this exact purpose. As applications become more complex and agentic use becomes prolific, simple software-based security doesn’t cut it anymore. Fortunately, Switchboard provides a purely cryptographic solution to access control which doesn’t fall victim to vulnerabilities of the current software-based solutions. This comes at no cost to the user and, in fact, brings other noteworthy benefits such as instant revocation. You may still share collections of accounts with groups of agents or people and create your own fine-tuned policy arrangements to suit your needs. Just see the[demo video](https://multifactor.com/videos/switchboard-demo) !


Combined with Checkpoint, Switchboard gives you the ability to share accounts as if you were sharing docs on Google Drive.


You can create links to share outside of an organization so that anyone can gain access to this account – be it an agent, your colleague, or your friends and family. Also, this authorization policy enforcement makes sharing entirely non-repudiable unlike traditional password sharing. If you provide your account to an agent, there’s no way for that agent to share it once again unless you explicitly allow it. In other words, this prevents the “sticky note” attack where a person or agent can write down a shared credential and use it without your knowledge. This is only possible because of the combination with Checkpoint.


Though web applications are all unique, it’s simple to set up Switchboard and Checkpoint to allow for a collection of permissions. If an automation is one that will be repeated often, it’s quick to do it once by hand and provide access to only that sequence of instructions. Or if a more general approach is desirable, automatically crawling a website’s API is also a route to set the permission table. And of course, it can always be specified manually through a configuration file for the cases in which you want direct control of your own set of permissions within an organization.


## Auditing


Finally, how do we know exactly what happened with the agents using the accounts that were shared? This is where Checkpoint and its durable logging feature called “Carbon” shines. For browser or computer use agents, robust and interpretable logs are not something that you get for free, so this problem is nuanced.


As agents browse the web on your behalf, the typical stream of actions they take is rather unintelligible. Typically, you will see something like this:


```text
[2025-09-07 16:28:17] ACTION: Find element by CSS selector: 'input[name="username"]'.
[2025-09-07 16:28:17] OBSERVE: Element found: <input type="email" id="user-email-field" name="username">.
[2025-09-07 16:28:18] ACTION: Type " [email protected]  " into element 'input[name="username"]'. [2025-09-07 16:28:18] ACTION: Find element by CSS selector: 'input[type="password"]'.
[2025-09-07 16:28:19] ACTION: Type “hunter2” into element 'input[type="password"]'.
[2025-09-07 16:28:19] ACTION: Find element by XPath: '//button[@id="login-submit-btn"]'.
[2025-09-07 16:28:19] OBSERVE: Element found: <button id="login-submit-btn" class="btn btn-primary">Log In</button>.
[2025-09-07 16:28:20] ACTION: Click element '//button[@id="login-submit-btn"]'.


```


Evidently, it’s hard to track what types of interaction has happened; the logging system here is not designed for human interpretability (you may have also noticed that this agent had to be provided with the plaintext password in its context). Worse yet, some agents instead just log the mouse positions at which actions were performed, something that doesn’t even carry over to a different display or maintain relevance as a website layout changes.


Instead, by using Checkpoint and Carbon together, you get logs that are much more clear and retain their semantic context:


This is possible because we are not interfacing the logger with the agent, but are instead capturing the requests and responses between the agent and the target site through Checkpoint. Furthermore, with access policies laid out by Switchboard, we are able to map these underlying HTTP messages to the policy seen on your interface. Another virtue of this system is that these logs are cryptographically secured by both Checkpoint and Carbon, since they are captured by the proxy system itself.


## Conclusion


The solutions we’ve presented here comprehensively address the problems of sharing accounts with semi-trusted third parties such as AI agents. For any agentic app, the AAA threats must be solved simultaneously and through the combination of MFKDF2, Switchboard, and Checkpoint into Multifactor’s platform we are able to create a product that does exactly this.


Today, the deployment of agentic applications is stunted by the lack of security options; it takes new security mechanisms to enable completely new classes of products. Our product launch is on the horizon and we will have more details of the launch in the next few weeks, so stay tuned!


If any of these matters are pressing for you or your organization, please reach out to us.


# We're redefining zero-trust — so you can protect your accounts with confidence.


Identity is your first and last line of defense, and the root cause of most application security breaches. Multifactor's provably secure zero-trust solutions cryptographically guarantee that only authorized users can access sensitive data, turning identity into your greatest asset in the fight against cyber threats. Learn more about our research, or reach out to explore working together.


[Get Started](https://multifactor.com/get-started)[Learn More](https://multifactor.com/company/sales)


## Related Posts


[We're Hiring](https://multifactor.com/blog/we-are-hiring)


2026-03-02


Multifactor is hiring. If you are a talented engineering leader or individual contributor who cares deeply about forging the future of authentication, authorization, and auditing for the agentic era, you should probably check out our open positions.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[Multifactor Versus Password Managers: Securing Capabilities, Not Credentials](https://multifactor.com/blog/multifactor-versus-password-manager)


2026-01-31


Say goodbye to password managers and hello to Multifactor, the world’s first true Account Manager!


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[Meet Multifactor: Putting Your Digital Life on Autopilot](https://multifactor.com/blog/meet-multifactor)


2025-10-31


A closer look at the platform designed to unify and secure your entire digital identity.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)
