---
schema_version: "1.0.0"
document_id: "65b737e639270e6f7e5354792b236667129f9843a75380d1e5e82a7b327b1d96"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/dont-just-shift-left-with-aws-cloud-security-you-might-miss-something-8487f999c07"
published_at: "2022-07-07T17:52:55+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:26:31.203349+00:00"
content_hash: "sha256:d29762f882b7b6f46a3f988d38293bbd0722420c58d1163dc0a7215583f4eafc"
---

# Don’t just shift left with AWS cloud security, you might miss something!

# Don’t just shift left with AWS cloud security, you might miss something!


[Alex Farhadi](https://medium.com/@farhadialex?source=post_page---byline--8487f999c07---------------------------------------)


8 min read


·


Jul 7, 2022


--


Press enter or click to view image in full size


Life360 uses AWS to host its application and related services. AWS presents new security challenges as the DevOps paradigm has shifted how and who controls various facets of a technology stack. The DevOps philosophy has enabled teams to become more independent and efficient by breaking down barriers and silos. In the days before DevOps, there was a centralized team who managed networks, deployments, servers, or other components of the technology stack; now that line is a bit more blurred. There are now more engineers, with a variety of different skills and expertise all changing things at lightning speed vs what may have taken a specialized team days or weeks to complete. The key benefits of the DevOps philosophy such as speed and agility are likely here to stay. The impact of high-speed releases is not all rainbows and unicorns since now security issues can make it to production with the same speed. In this article, I’ll go into how Life360 balances enabling high-speed DevOps style workflows and maintaining a high security bar by not always just shifting left.


Press enter or click to view image in full size


Credit: Pete Cheslock for the meme.


AWS security issues can come in all shapes and sizes; this article is focused on AWS service and component configuration. Some of the most notable issues that people may be familiar with from the news are things like an open S3 bucket that contained customer data, or a database that was left wide open to the world that contained other sensitive information. These are all things that without proper scanning and auditing, can happen to your organization.


Many times, companies prefer the shift left strategy when it comes to identifying vulnerabilities. Shift left is a strategy in which you try to identify and stop vulnerabilities as early on in the development process as possible. This is a great approach that reduces the costs associated with remediating security issues as it is typically less expensive to change something within a code review vs changing something in production. Shift left is a great strategy for things that are defined in code, but what about everything else that was created before IaC (infrastructure as code) was started within the company? Or that one account that was supposed to be used purely for development purposes where IaC is not enforced? This is where a combination of shifting left and right comes into play, or scanning things across the entire SDLC spectrum. In terms security practitioners would recognize, this is a modern version of “defense in depth” applied to the cloud native model.


Press enter or click to view image in full size


Life360 uses a continuous spectrum approach for AWS security issues, meaning we want to detect and prevent new issues as quickly as possible within the SDLC, but we also want to find things that already exist within production, have been created without code, or somehow made it past earlier scans. We have a company preference toward both open source and AWS native tooling due to the wide community knowledge base and support, as well as their products being budget friendly. Commercial products are great, but at a certain scale, they become cost prohibitive.


As you can see in the diagram below, we have incorporated various tooling and other processes into the SDLC process. Starting at the left hand side we will overview and describe the functionality and integration of each of these items.


Press enter or click to view image in full size


**Education**


From my time and experience working with developers who use AWS, one common source of security misconfigurations comes from a lack of knowledge about what a setting or change would do. This is why education is especially important when working with AWS. At Life360 we ensure all developers who have access to the AWS cloud have the training resources required to perform their duties. We also have internal documentation about the preferred configuration states of AWS resources and when exceptions can be allowed.


**Preventative controls**


What is better than fixing something that has made it all the way to production? Making it impossible for that to ever happen! Life360 creates guardrails via technologies like[AWS Service Control Policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) and global configuration items like “S3 account level block public access” to ensure undesired configurations are not allowed in the first place. We use SCPs for things like denying certain regions, preventing the disabling of security controls, and creating configurations that expose resources to the internet. Guardrails can be an impediment to progress, so it is important to only restrict where you are sure it will not block normal development.


**Design Reviews and Threat Modeling**


It is possible to identify security misconfiguration in the design and tech spec processes. This is why it is important that all designs and implementation plans are threat modeled and reviewed before engineering work commences. This way, security or other stakeholders can raise concerns earlier on in the design process, reducing the likelihood that a misconfiguration could happen. This is a multistage process that involves reviews at the high level design phase and the technical implementation phase. This is not including pull requests, which happens closer to the deployment phase.


**TFSec**


[TFSec](https://aquasecurity.github.io/tfsec) is an open source static analyzer for your terraform code. It can detect misconfigurations such as open security groups, S3 buckets as well as lack of encryption. We encourage all contributors of terraform at Life360 to run TFSec locally while they are developing infrastructure code. Additionally, TFSec is run on all pull requests as a Github action and within the terraform plan deployment step which uses Atlantis (a CI/CD tool for terraform deployments). If high criticality TFSec findings are introduced, TFSec automatically blocks the PR, preventing developers from applying terraform code at all and therefore introducing a security misconfiguration.


**Security Hub**


Security Hub is provided by AWS. It scans AWS resources and looks for configuration vulnerabilities. When new issues occur, it can trigger different workflows on how that alert should be handled. Those workflows can include automated remediation scripts, Slack notifications, or integration into a SOAR product for more complex analysis and remediations. Security Hub is a great cost effective solution, but lacks some of the reporting and automation capabilities that commercial products offer such as Prisma Cloud. We utilize Security Hub for real time notification of high criticality alerts, and to prioritize and plan future security remediation work and controls such as making Cloud Custodian remediation rules.


**Cloud Custodian**


Cloud Custodian is a great open source solution to write complex detection and remediation capabilities with little effort. With native Security Hub, we would often have to create custom complex lambdas to perform the remediations. With Cloud Custodian, it is often as simple as writing a few lines of code. We primarily reserve Cloud Custodian for alerts that are too noisy (just fix it for us please!) and where we think the risk of remediation efforts impacting production is minimal/acceptable. For example, we certainly would not enable a rule that would terminate an EC2 instance within production, but we might enable a rule like that within development. Our Cloud Custodian runs within a centralized security AWS account, and uses cross account IAM roles to query and remediate issues.


**Attack Surface Management**


Attack surface is one of the most underlooked items within AWS security. Attack surface in this context is EC2, or other AWS IP based resources that are reachable from the internet. Existing tooling provided by AWS, mainly the AWS Network Inspector, provides some level of scanning for attack surface but also relies on having a relatively cruft free environment and really good security group hygiene. If anyone has worked in a non greenfield environment, they know that security groups often grow into a mess that is difficult to untangle. At Life360, we opted for a custom solution that uses NMap, as well as an internal database to periodically list all EIPs across all AWS accounts, and port scan them to check to see if they are actually open and will accept a connection request, not simply if they have a security group that would allow it, which is how AWS Network Inspector works. This system alerts when an EIP and port is reachable from the internet in near real time, so that the security engineer can lock down that exposure. This system is not yet open sourced but we are considering publishing it in the future. Commercial tooling was also an option for this, like Censys, but with Life360’s scale it was cost prohibitive.


**CloudTrail Alerts**


We utilize organizational level Cloudtrail configuration to send all events to a log management cluster. From there, we create alerts for actions we would like notifications on and send it to security for review and next steps. One of these items could be creating an IAM user, since we have a preference for using IAM roles where possible.


**AWS Macie**


This is a relatively new tool for Life360. We use this to look for sensitive information only on public buckets, as we estimated if we scanned against all our buckets it would cost somewhere in the hundreds of thousands to millions per year. We have Macie configured to look for all the normal PII, PCI information but also utilize custom identifiers to look for Life360 specific sensitive information like location data, user and circle IDs.


**Lambda and Boto3 Script**


The last tools within our AWS security arsenal are lambdas and boto3 scripts. Where automation, security hub or cloud custodian might not fit well, we will often write custom python boto3 scripts and run them periodically across all our AWS accounts on a cron based schedule. This solution is not very elegant, but when time and engineering resources are constrained, it can be a very cost effective solution to just get things done.


**Conclusion**


When it comes to AWS security, we believe it’s important to shift left and right and not just focus on one or the other. Shifting left is great for preventing security misconfigurations and failing faster, but will not always catch everything. As you start, or continue your AWS security journey, here’s what’s worked for us in building a layered approach to security that supports rapid DevOps style releases while maintaining security.


**Detection** is key to starting any successful AWS cloud security journey but the volume quickly becomes unmanageable with the amount of findings that need to be triaged. At a minimum, you should **enable AWS Security Hub** and review the findings starting with the most critical ones first. Once you complete the first review, keep an eye on new findings that occur as AWS is always adding new rules to their Security Hub product. **Automation and preventative controls** are the key to scaling your AWS security program and to allow for your development team to continue to be agile, but also secure!


We hope you enjoyed this article. If you have any questions, feel free to reach out to me. Also be on the lookout for our next security blog about container security at Life360!


## Come join us


Life360 is the first-ever family safety membership, offering protection on the go, on the road, and online. We are on a mission to redefine how safety is delivered to families worldwide — and there is so much more to do! We’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .
