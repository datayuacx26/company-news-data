---
schema_version: "1.0.0"
document_id: "57b0481963888aa02f3ca0f80a9f641bd60a325fa2408c97a2db42bcda48ea4e"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/infisical-honey-tokens"
published_at: "2026-05-06T22:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:44:45.479848+00:00"
content_hash: "sha256:79003199af5fd8c2899feaf88d8204d7196ad50be989c8190203e3dc5380c4f0"
---

# Infisical Honey Tokens: Bait Credentials That Catch Breaches

In March of 2025, Rippling revealed[one of the more dramatic cases of alleged corporate espionage](https://techcrunch.com/2025/04/02/the-affidavit-of-a-rippling-employee-caught-spying-for-deel-reads-like-a-movie/) in HR Tech. A Rippling employee had searched for their competitor, Deel, an average of 23 times a day in Slack, raising suspicions of being, ironically, on Deel’s payroll.


So Rippling's security team created an empty Slack channel called #d-defectors. Deel's leadership was tipped off by Rippling that the channel held damaging information they wouldn’t want public. Within hours, the spy searched for it. The trap had worked.


The #d-defectors channel was a honeypot: bait that looks like what a bad actor is after, but is a trap that detects a security compromise. Rippling used a Slack channel, but the same principle works with anything an attacker could want. In most breaches, that is a credential.


A fake credential planted among real ones is a honey token. When the honey token is used, you know a breach has happened. This requires an irresistible credential that an attacker can get a lot of mileage from, like an AWS IAM token.


And that’s why today, we’re launching[honey tokens in Infisical](https://infisical.com/docs/documentation/platform/honey-tokens/aws/setup) . They’re an additional layer of security that acts *after* someone has gained access to your secrets. Think of it the same way you might install a motion-activated alarm, even if you have good locks on your doors.


Credential exfiltration is one of the most common patterns in breaches today, and AI is making it easier than ever to find and exploit weaknesses. Attackers are using AI to weaponize leaked credentials at a pace that you can’t catch manually. The hard part is detection, and that's where honey tokens come in to help close the gap.


## Detecting vulnerabilities keeps getting harder


Certainty doesn’t exist, no matter your resources. Enterprises have too many credentials, environments, vaults, and teams to monitor every detail, while startups usually lack the dedicated security team to do so.


After a breach, leaked credentials often linger for weeks or months before they’re rotated. But attackers don’t wait weeks or months to exploit them. It’s hard to escape the dread that something, somewhere, has gone wrong.


A honey token works quietly in the background and sends a real-time alert the moment it's triggered. That removes the guesswork and lets you move directly to response: rotating the compromised secret, for example, and limiting the blast radius by acting before the attacker does.


## How honey tokens help you respond faster


In Infisical, a honey token is an AWS IAM access key pair (real, albeit with zero permissions). You mint it in Infisical and drop it into the same folder as your real credentials. Trying to call` sts:GetCallerIdentity` (or any other action an IAM credential can perform, such as listing S3 buckets or checking for secrets in parameter storage) creates a CloudTrail log, which propagates back to Infisical and fires an alert.


Infisical then sends you an email (Slack and PagerDuty alerts coming soon), which tells you that adjacent secrets are likely also compromised, and you should rotate secrets. A legitimate workload would never touch a honey token, so you shouldn’t see false positives.


Honey tokens work because of simple psychology: an AWS IAM credential is too tempting for an attacker to ignore. We started here because AWS IAM is the most enticing bait and the most reliable trap. AWS logs every request against a key, authorized or not, so any attempt to use the token is caught immediately. Other honey token types are on the roadmap.


## How honey tokens work in Infisical


Honey tokens live inside projects and folders among your actual secrets. There are two parts of the process. We’ll briefly walk through them here, but you can[get the full details in our documentation](https://infisical.com/docs/documentation/platform/honey-tokens/aws/setup) .


### Provisioning honey token infrastructure


First, you need to enable honey tokens at the organization level. In the organization settings under "Product settings," choose an AWS App Connection and save. Infisical will then display an AWS CLI command. Run it in a terminal with credentials for the target AWS account to deploy the CloudFormation stack. Once AWS provisions the infrastructure, click "Verify connection" to confirm the setup is active.


Once AWS provisions the infrastructure, you’ll be able to verify the connection is active:


### Using honey tokens in Infisical projects


After you run the AWS CLI CloudFormation command to spin up the needed honey token infrastructure in your AWS account, you're ready to start using honey tokens. Simply add one to an environment in Infisical.


Details such as name, description, environment, and the two secret key names are customizable.


The token now waits for bad actors to trigger it, with a green label that says “active.” If used, the badge will switch to red and say “triggered” and the all organization admins receives an email. We recommend rotating everything in the blast radius if this happens. The honey token confirms the worst case. Everything adjacent to it should be treated as exposed.


### Why Infisical’s honey tokens are harder to detect


Other honey token offerings are easier to detect for hackers because they mint the honey tokens on vendor-controlled infrastructure. This minimizes setup friction by avoiding deployments or CLI interactions.


But AWS access keys embed the issuing account ID, and it can be extracted offline without ever using the key to authenticate. This means attackers can identify honey tokens as such once security vendors' account IDs are known, which greatly negates the honey token's power and potentially creates an illusion of security (though honey token inactivity is no guarantee of safety).


We consciously added a bit of extra friction to make honey tokens indistinguishable from real secrets by minting them in your AWS account, where all of your other secrets also live.


## Give it a spin


Honey tokens are available today on Infisical Pro and Enterprise. The feature is rolled out across cloud and self-hosted deployments.[Read the honey token documentation here](https://infisical.com/docs/documentation/platform/honey-tokens/aws/setup) .


If you're on a security team and have spent any time wishing for a detection signal you could actually trust, give honey tokens a try.


If your organization is evaluating credential security at scale and you'd like to talk through what enterprise-grade detection looks like,[reach out, and we'll walk you through it](https://infisical.com/talk-to-us) .
