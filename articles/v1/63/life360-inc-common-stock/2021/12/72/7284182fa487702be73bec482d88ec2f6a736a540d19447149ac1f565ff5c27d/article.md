---
schema_version: "1.0.0"
document_id: "7284182fa487702be73bec482d88ec2f6a736a540d19447149ac1f565ff5c27d"
company_key: "life360-inc-common-stock"
company: "Life360 Inc."
source_id: "life360-inc-common-stock-rss-25c02f0e1eb8"
canonical_url: "https://medium.com/life360-engineering/jenkins-mysteries-phantom-aws-roles-2d3afcb78d9"
published_at: "2021-12-24T21:30:46+00:00"
first_seen_at: "2026-07-20T04:36:48.728062+00:00"
fetched_at: "2026-07-28T22:26:34.525443+00:00"
content_hash: "sha256:07a36d426de1f4e3a450f85c8291ba836977ba0f0d1a803ce8b15a423f8512df"
---

# Jenkins Mysteries: Phantom AWS Roles

# Jenkins Mysteries: Phantom AWS Roles


[Benjamin Galperin](https://medium.com/@bgalperin?source=post_page---byline--2d3afcb78d9---------------------------------------)


9 min read


·


Dec 24, 2021


--


Press enter or click to view image in full size


The Mysterious Case Of The Phantom AWS Roles


Hello and welcome to my little corner of Life360, please step into my office and make yourselves comfortable as I regale you with a tale of mystery, frustration, and automation: The case of the phantom AWS roles.


It was a damp and foggy night, not much different from any other night in San Francisco. There I sat, a dimly lit room with nothing but the glow of my laptop’s screen illuminating my face and the nearby trays that held small piles of paper. Sitting in the tray labeled “In Progress” sat a particularly troublesome ticket, one that had the audacity to travel back from the “In Testing” tray to its current resting spot. The ticket’s task was supposed to be simple: Configure the mobile pipeline to retrieve its secrets from AWS Secrets Manager using a bespoke AWS IAM Role, but it was proving to be anything but; When assuming this tailored AWS IAM Role we would sometimes come across a specter of itself, that in fact we had somehow already assumed the role, that some ghost in the machine was providing credentials where there should be none. But before diving deeper into this programming poltergeist, I should give a little background on our systems.


A thorough overview of our mobile pipelines would need its own dedicated post, so in the immortal words of Inigo Montoya, “Let me explain. No, there is too much. Let me sum up.”


This particular pipeline is run on Jenkins, in which we use multi branch pipelines and split everything into distinct Build, Test and Deploy stages. The paradigm can be summed up along these lines: Determine what needs to be built first, go ahead and build it, then stash it somewhere so it can be passed along to the parts of the Test or Deploy stage that need it. We accomplish this using Jenkins Shared Libraries that take in parameters from a project’s Jenkinsfile, then use that information to determine what needs to be built, which tests to run, and whether or not something needs to be deployed once tests pass. Each of these stages can be split up to run various tasks on their own nodes in parallel with one another. Combine this with a steady use of caching dependencies and propagating artifacts to downstream tasks, we are able to cut down on end to end pipeline execution times. Lots of details are left out of this overview, but as Mr. Montoya mentioned, there is too much for now.


Secrets management is important


However, one detail worth mentioning is how we handle our secrets. Typically keys, tokens, secrets, etc don’t belong in your source code, so you need to store them somewhere secure while also having the ability to safely retrieve them. We use AWS Secrets Manager and set up dedicated AWS IAM roles that have access to a small scope of secrets. For example, our android mobile pipelines only have access to android related secrets, while our iOS mobile pipelines only have access to iOS related secrets; Roles should only have access to what they need.


With that information out of the way, we can get back to the aforementioned task which lay on my desk. We had recently consolidated our mobile pipeline secrets into one AWS account and we needed to update the Jenkins pipeline to use the new AWS roles that were dedicated in the fashion previously mentioned. I swapped in the new AWS roles into our existing secrets fetching methods, all the tests passed, the changes were merged into our Jenkins shared library repository and all was well. Until it wasn’t. Builds started sporadically failing, different stages failing at different times in different locations, passing here, failing there, nothing made sense, it was the chaos of an intermittent issue.


After a hasty revert it was time to comb through the logs and see if we could determine the pipeline’s cause of death. The autopsy report beckoned:


> Office of Build & Release Engineering
> Autopsy Report
> Case No: 42–069
> Deceased: Pipeline, Android
> Type of Death: Pipeline Error Signal
>
>
> Autopsy: Jenkins pipeline containing Build, Test and Deploy stages, with each stage running various stages on their own nodes in parallel. Nodes appear to be running in Linux VMs with various resource sizes, scaling up and down via AWS EKS. No anomalies found in the Build stage. Sporadic failures across all testing stages that require fetching secrets. UI tests, unit tests, and analyzers all experiencing intermittent failures upon attempting to assume a role to fetch AWS SM secrets. Deploy stage never reached due to Test stage failures.
>
>
> Cause of Death: Pipeline error signal upon receiving the following message
>
>
> *An error occurred (AccessDenied) when calling the AssumeRole operation: User: arn:aws:sts::<ACCOUNT-REDACTED>:assumed-role/<ROLE-REDACTED> is not authorized to perform: sts:AssumeRole on resource: arn:aws:iam::<ACCOUNT-REDACTED>:role/<ROLE-REDACTED>*
>
>
> Notes:
> It is in my professional opinion that the cause of death is a terminal existential crisis in which the pipeline attempted to assume the role of itself.


Press enter or click to view image in full size


The crime scene


After reading the autopsy report I began to troubleshoot the issue. The first thing I did was just a simple sanity check to ensure the AWS roles and resources had the required permissions. Everything looked fine: The base role our Jenkins pipeline used had the permissions to assume the new android mobile pipeline role, and that mobile role allowed itself to be assumed by the base Jenkins role. Similarly, the android mobile role had the correct read permissions to access the AWS SM entries we wanted to retrieve. With that out of the way I moved on to see how we were setting out AWS role credentials.


The report had pointed me in the right direction, so I created my own Jenkins shared library repository branch to start testing. While the failing stages were intermittent, the type of error was always the same: An AccessDenied error when attempting to assume the required role because it had already assumed it. Navigating to the common method we used for fetching secrets it was clear that it had logic in it to stash any existing AWS credentials, assume the requested role and retrieve the required secret entry, then revert back to the stashed credentials. What followed was an extreme saturation of debug print statements such that I could see what AWS credentials were in use at any given time by running the “aws sts get-caller-identity” command everywhere in which secrets were retrieved in this particular pipeline. The results would be equal parts surprising and confusing.


Two Jenkins “sh” steps running one immediately after the other would sometimes return different values for the “aws sts get-caller-identity” command.


What?


I thought to myself, how is such a thing possible? Those two “sh” steps were running on the same node, and I could see nothing in those steps that would change the AWS credentials. I decided to explicitly clear any AWS credentials prior to assuming a requested role by following AWS documentation and using the “unset” command within those debug “sh” steps. The result was frustratingly the same, the “aws sts get-caller-identity” command would still intermittently return a different value than the step immediately before it.


In hindsight, these clues should have been enough to point me in the culprit’s direction, but I didn’t come to realize this until I took a step back and looked at the pipeline results as a whole. The Build stage never seemed to have this problem, so what was it doing differently than the Test stage? In this case, my testing branch Build stage was running one stage in one node, while the Test stage was running a number of stages in parallel, each on their own node. If the way we are setting AWS credentials is writing to a single location, this would lead to the precise asynchronous issue that killed this pipeline.


We were close, the culprit nearly in our grasp. Following this new set of clues brought us to the methods we used to set, get, and stash AWS credentials. One look at this code and it was clear what had happened: We were setting our AWS credentials to environment variables, but we were doing it by writing to the “env” map, the same map that’s accessible by all of the nodes that were running in parallel in our pipeline! Any time the nodes started a new “sh” step to begin the assume-role process they would default to the credentials stored in this global env map leading to the precise issue we were facing: Attempting to assume a new role while using an unexpected role’s credentials.


Press enter or click to view image in full size


The first fix that came to mind was to clear out any AWS credentials before assuming a new role, however that was not an option as there were other pipelines in the company that depended on nested role assumptions. Given the nature of Jenkins shared libraries it’s always beneficial to check how other pipelines may be using the methods you intend on changing in your shared library. I quickly ditched that proposal and moved on to the next, quite simpler one.


The simple fix for our pipeline would take advantage of our mobile secrets already being compartmentalized, and dedicated roles for each mobile pipeline. As mentioned earlier, our android mobile pipeline has a specific AWS role that caters to just the android mobile pipeline. There’s simply no need to constantly assume roles when we instead can assume the role once at the beginning of the pipeline and keep that role the entire way through. This assured us that multiple nodes were not writing to the global environment variable map and overriding the AWS credentials stored there.


A longer term fix would be to change the way we store AWS credentials to allow for parallel nodes to assume whatever roles they want in parallel with each other, but that’s a larger refactor, and a topic for another time.


And that’s where this detective story comes to an end. We analyzed the cause of failure, followed the clues and implemented a fix to keep the developer’s pipeline moving. Sometimes there’s no killer, it’s just an accident. No smoking gun, no bloody knife, nothing complicated, elaborate or malicious, just a simple case of asynchronous writing to a global object. Held in my hand was a circular rubber stamp that read “DONE” in large bold letters surrounded by the phrase “semper vigilantes, semper fluit”. The once elusive ticket now lay silent in the “In Testing” tray, awaiting the rubber stamp. I was eager to punch this ticket, but it would have to wait until more of my clients had triggered the pipeline and all looked well.


It’s late, and the night is still damp and foggy, but as I ponder the words on the stamp I take solace in successfully ensuring that the mobile pipeline must flow, and some days that’s just good enough.


Press enter or click to view image in full size


Semper vigilantes, semper fluit


## Come join us


Life360 is the first-ever family safety membership, offering protection on the go, on the road, and online. We are on a mission to redefine how safety is delivered to families worldwide — and there is so much more to do! We’re looking for talented people to join the team: check out our[jobs page](http://www.life360.com/jobs) .
