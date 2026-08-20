---
schema_version: "1.0.0"
document_id: "712d2cc00ed186d3d851c166302cb7bef8a9eaa2895f3d5ea5c7d85ce8952df3"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/security-at-svix/"
published_at: "2026-07-29T13:00:00+00:00"
first_seen_at: "2026-07-29T00:11:38.756229+00:00"
fetched_at: "2026-07-29T13:00:02.388435+00:00"
content_hash: "sha256:e30cd0a7243b1cf4014d05fffa654eae682d7a540991d6ac5924dee1e2a39bc8"
---

# Your startup's security policies are concerning

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


Whenever someone joins Svix, without fail, they run into one of our security practices and ask if I'm insane for operating a startup like this. We take security more seriously than any other startup they've previously been a part of, and I'm proud of that.


The reason why it's surprising to people is because most startups optimize for speed above all else, playing fast-and-loose with testing, reliability, and of course security. I feel strongly that this is the wrong approach, and it drives me mad that people do that.


Being a vendor comes with a lot of responsibility to your customers, and also in a way to your peers and the rest of the ecosystem. Your customers pay you to provide a service, but they also pay you for an implied (or often contractual) duty of care. The same way that a restaurant sells you food, but you also assume they use good ingredients, wash their hands, and follow health standards.


I'm writing this post to share a bit about what we do and our experience doing it, with the hopes that more startups take security seriously. It's the right thing to do for your customers, the rest of the ecosystem, and for you as well.


## Some of what we do


We really live by the least-privilege principle. People only get access to things if they need access to them to fulfill their role. If they only need access temporarily, they only get access temporarily. Most importantly, they only get the level of access they absolutely require and not more. This is done by an access request ticket explaining what they need access for and why.


No one from the team has direct access to production servers, networks, or databases, not even using a VPN. All management of servers and production assets is done through infrastructure as code (Terraform), and goes through our normal review and deployment process.


Nobody (other than the owner account) has administrative access to AWS, Github, etc. There is a way to get emergency elevated access that follows an approval path, is time-boxed, audited, and alerts the team, though its access level is also limited.


No one has access to customer data. The only way to get that is through an approval process that only lets you run limited queries, and alerts the whole team when used (to prevent and detect data exfiltration).


We employ division of roles and responsibilities. The team can't land a code-change without it first being reviewed by at least one team member (and when relevant, code-owner), and only a small number of designated employees can trigger a deploy.


Production, staging, and dev environments are completely separated from one another, and different services within the same environment are segmented into different subnets with traffic completely blocked unless previously approved.


We use the Rust type system extensively to make invalid cases non-representable, and manage authentication and authorization in a centralized manner to prevent accidental bypass. We use frameworks that prevent a whole class of security issues (like a SQL-safe library, and React for XSS protection). Customer data is segmented logically between customers (enforced by the type system and Postgres RLS) to reduce the blast-radius of a bug in the server.


With everything we do, including when adopting vendors, open-source components or processes, or when building new functionality, we review the security impact, and consider whether there are ways to limit the blast-radius if something bad does happen. Making sure that a compromise of a vendor won't lead to a compromise of Svix or its customer data. Or more generally: we include risk assessment in everything we do, and think of security first.


This is in addition to the usual things of employing encryption, patching vulnerabilities, dependency scanning, linters, firewalls, doing security training, pentests, audit-logs, etc. Also, this post only covers prevention, but an important part of any security program is reaction (including containment, reporting, and remediation).


## The weakest link


While most movies show hacking as a hooded teenager sitting in a basement and typing very fast, in practice, most hacks happen due to social engineering or laptops getting hacked or stolen.


So one of the most important things you can do is make sure that the team takes security seriously. The way to do that is to overshare about why this matters, how hacks happen, and how we can all prevent them. This is in addition to helping them be secure by employing SSO and using phishing-resistant methods such as passkeys. This should be complemented by ensuring that operating systems and especially browsers stay up to date.


You also want to watch out for security theater, as that's a surefire way to get your team to not follow security practices. For example, we don't mandate password rotation (as per NIST SP 800-63B), and instead focus on the things that matter.


It's important to give people a safe environment to act on their instincts (and training) and push back on suspicious requests. For example, one of the things I always tell the team: there are no repercussions to not following a suspicious request under time pressure. If something smells wrong, post about it publicly on Slack and see what people say.


## Operational impact


There are definitely downsides to being security first. For example, there's friction when onboarding new team members as they are not used to needing to ask for specific access that they need and explaining why they need it. It also sometimes slows us down when we have to wait for an approval in order to do a security-sensitive operation. Though we've grown to consider these frictions as benefits rather than downsides. They force us to really think about security in everything that we do.


However, there are downsides with actual customer impact, for example, when customers come with requests, the team can't just run database queries against customer data and check what's going on. They usually need to guide the user how to check themselves which can sometimes lead to a slower time to resolution. Though our customers usually appreciate the trade-off here, and are happy we take the security of their data seriously.


With that being said, there are also benefits other than security. Namely, resilience, infrastructure maturity, and consistency. Because everything is managed using infrastructure as code, the infrastructure undergoes the same software development process as our codebase, leading to better consistency and infrastructure maturity. Additionally, not being able to accidentally click the wrong thing and deleting a production database ([or have your agent do it](https://www.tomshardware.com/tech-industry/artificial-intelligence/claude-powered-ai-coding-agent-deletes-entire-company-database-in-9-seconds-backups-zapped-after-cursor-tool-powered-by-anthropics-claude-goes-rogue) ) is a huge advantage to the resilience of the service.


## Closing thoughts


One of the biggest challenges of security is that you can't prove the absence of a security issue, so the best approach you can take is a layered approach that makes it so an attacker would need a series of independent flaws in order to gain meaningful access.


I feel like a lot of companies can level up their security without too much pain, and it really annoys me that we don't take security more seriously as an industry. It's probably time we do.


---


For more content like this, make sure to follow us on[X / Twitter](https://x.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
