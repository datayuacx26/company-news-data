---
schema_version: "1.0.0"
document_id: "ca0ab69dd715be08f290abed0463f268575dc890a81a5e1f35c01b875627242e"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/microsoft-credential-leak"
published_at: "2024-04-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:1a87b638ec6ee2e50182bbb0e47f4090c0b6e9d10dbf3c401654fdd1fe21ba3f"
---

# Learning from Microsoft’s Credential Leak

In this article, we discuss what transpired and how the situation could have been avoided from an engineering standpoint.


Recently, news surfaced that Microsoft employees exposed internal passwords made headlines on[TechCrunch](https://techcrunch.com/2024/04/09/microsoft-employees-exposed-internal-passwords-security-lapse/) and[Vice](https://www.vice.com/en/article/m7gb43/microsoft-employees-exposed-login-credentials-azure-github) .


## What happened?


On February 6th, security researchers at[SOCRadar](https://socradar.io/) notified Microsoft about a publicly exposed Azure storage server containing passwords, keys, and credentials used by Microsoft employees to access internal databases and systems. The storage server was not password-protected which meant that anyone on the internet could’ve accessed it until March 5th, when Microsoft secured the leakage.


The latest incident comes amid a slew of similar incidents in recent years including when[Microsoft employees exposed their corporate network logins to GitHub](https://www.vice.com/en/article/m7gb43/microsoft-employees-exposed-login-credentials-azure-github) and[lost an internal email signing key to China-backed hackers](https://techcrunch.com/2023/09/08/microsoft-hacker-china-government-storm-0558/) .


## What’s problematic here?


Unfortunately, leaking passwords, keys, and credentials (called secrets) is something that happens to many companies, not just Microsoft, and happens sporadically across the development cycle.


From first glance, the case at hand illustrates two big issues associated with poor secrets hygiene:


- Poor secrets storage practice: Storing secrets in a storage server/bucket is an anti-pattern, especially considering that buckets can easily be misconfigured (e.g. to have no authentication/authorization controls, encryption at rest, etc.). While images, documents, and videos can be stored in buckets, secrets belong elsewhere with more stringent security measures and access protocols in place since having access to a secret means potentially having access to a whole trove of data.
- Poor response: Based on the reported timeline, it took Microsoft a month to react to the leakage. This means that bad actors who knew about the issue had a full month to exploit it and use any exposed credentials to access internal data. Moreover, when reached by email, Microsoft did not comment on if they had rotated any of the exposed credentials.


## How can situations like this be avoided?


With good secrets management practices implemented throughout your development cycle, you can avoid leaking secrets entirely and, in the unlikely case that it does happen, respond swiftly to disable access and rotate any leaked credential(s). From using a secure solution like[Infisical](https://github.com/Infisical/infisical) to store and manage secrets to employing tools to detect credentials before they make it anywhere public, there’re plenty of strategies and solutions to prevent leaking secrets.


Let’s discuss a few strategies:


- Using a secrets manager to store secrets: Foremost, the best thing you can do across your development cycle is use a dedicated tool to store your database credentials (also known as a secrets manager). Unlike a public S3 bucket or GitHub repository that’s built with generic security in mind, secrets managers layer rigorous security controls around the data contained within, be it encryption at rest or authentication/authorization controls, and often provide secure methods to manage and fetch back secrets to engineers, CI/CD pipelines, and production environments. In an increasingly complex infrastructure world where secrets lay sprawled across a development cycle, employing a secrets manager is a must-have.
- Using a secrets manager to rotate secrets: Beyond storing secrets, a secrets manager can be used to rotate secrets and respond timely in the event of a leak; this is not to mention[dynamic secrets](https://infisical.com/docs/documentation/platform/dynamic-secrets/overview) functionality involving issuing ephemeral database credentials in place of static database credentials. The premise behind both rotating secrets and issuing dynamic secrets is to keep your secrets as moving targets. By, for example, rotating secrets on an interval basis or on-demand, you can ensure that a secret, if ever leaked, is defunct or unusable.
- Using secrets scanning pre-commit hooks: When developing features locally, it’s possible for engineers to accidentally hardcode secrets into codebases, accidentally hit commit, and push out the secrets for everyone to see in version control. It’s also possible for engineers to forget to` .gitignore` configuration file(s) and leak those to GitHub as well. With secrets scanning pre-commit hooks, however, it’s possible to configure local development workflows with a few lines of code to detect secrets prior to committing them into version control. By implementing this control, you add a preventative measure to guarantee that any incidents resulting from hardcoding secrets do not arise in the first place.
- Using a secrets scanning with continuous monitoring solution: In the event that a secret does get leaked to source control, a monitoring solution can help scan every new commit in the codebase for the presence of secrets and report findings to you and your team to respond as fast as possible. By implementing this control, you add a recovery measure to guarantee that you get notified before anyone else, should an incident transpire.


By employing a few strategies, it’s possible to mitigate, if not entirely prevent secrets from leaking across the development cycle. Had the engineers at Microsoft used a secrets management platform, they would’ve been required to configure tight authentication/authorization policies around the credentials such that they could not be accessible by anyone on the public internet. They could’ve also prevented leaking employee access credentials to GitHub in the earlier incident last year if they had configured secrets scanning capabilities across their workforce to guarantee that secrets did not make it to version control.


Overall, there’re a myriad of practices in place when it comes to good secrets hygiene that any company would benefit from implementing into their development cycle.


## Conclusion


In recent years, Microsoft has incurred a host of security issues with the latest being leaking passwords, keys, and credentials for database and internal systems as of an incident reported February 6th. The issue points to the larger fact that poor secrets hygiene exists amongst small and large companies with significant room for improvement. With some modest changes, a team, including Microsoft, can instill a strong secrets management foundation and, in doing so, mitigate and prevent secrets from leaking across their development cycle.
