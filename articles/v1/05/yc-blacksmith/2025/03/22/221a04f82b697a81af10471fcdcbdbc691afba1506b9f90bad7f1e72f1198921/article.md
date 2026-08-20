---
schema_version: "1.0.0"
document_id: "221a04f82b697a81af10471fcdcbdbc691afba1506b9f90bad7f1e72f1198921"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/you-have-5-days-before-the-new-dockerhub-limits-f-ck-you-over"
published_at: "2025-03-26T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:fe5013edd19a30dad22c7f356e7223f9512b9a8d6bd2dcfa9cc66f71bad608c5"
---

# You have 5 days before the new DockerHub limits f*ck you over

Docker Hub is about to implement much stricter pull rate limits[starting April 1st, 2025](https://docs.docker.com/docker-hub/usage/) . If you're running CI/CD pipelines—especially on self-hosted runners—this could hurt. GitHub-hosted GitHub Actions runners get a pass here because of their IP whitelisting agreements with Docker. This is a rare situation in which running your GitHub Actions on their[slower, much more expensive machines](https://docs.blacksmith.sh/github-actions-runners/overview#more-on-our-runners) has saved you from some pain! If you've been happily enjoying the use of a free DockerHub account and want to continue, you'll want to keep reading...


## Docker tightening their grip


Docker is tightening their pull limits to the extent that everyone who is not on a Pro, Team or a Business plan is on notice:


As expected, there was public outcry. Yet, I don't think even Docker expected the level of pushback they got because if you check the previous versions of their[announcement](https://docs.docker.com/docker-hub/usage/) , you will see that they quickly reacted to the bad press and pushed back the in-effect date from March 1st to April 1st, and changed various values in this table. Here's a screenshot my cofounder took around the day this was first announced:


As much as I enjoy the discourse, this is not a writeup about whether this is fair or not, for that we have[HackerNews](https://news.ycombinator.com/item?id=43125089) . What frustrated me most was Docker's short notice before implementing this change.


## What I am afraid will break


The immediate impact will hit CI pipelines across organizations of all sizes. Even a modest test suite running a few containers can quickly exceed the new limit of 10 pulls per hour per IP for unauthenticated requests. A small team of 2-3 engineers pushing multiple commits or running parallel jobs might hit this limit in minutes. Miss just one authentication step in your workflow, and you'll face those dreaded 429 (Too Many Requests) errors, grinding your dev cycles to a halt.


Production deployments using unauthenticated Docker Hub access are at risk too. Many production environments—surprisingly—pull public containers without proper authentication. As your CD pipeline pulls images across multiple environments or regions, you can quickly hit rate limits and temporarily break your deployment pipeline.


## Solutions that are doable in the next week


### Option 1: Set up your own pull-through cache mirror


Option 1 requires you to create your own Docker registry mirror that caches pulls of public images:‍


```text
storage:
s3:
accesskey:   {{   minio_access_key   }}
secretkey:   {{   minio_secret_key   }}
region:     us-east-1
bucket:     docker-registry
regionendpoint:     http://{{     minio_endpoint     }}:9000
secure:     false
v4auth:     true
chunksize:     5242880
rootdirectory:     /
cache:
blobdescriptor:     inmemory
maintenance:
uploadpurging:
enabled:     true
age:     168h
interval:     24h
dryrun:     false


proxy:
remoteurl:     https://registry-1.docker.io


http:
addr:     "  {{ registry_address }}  "
relativeurls:     false
draintimeout:     60s
secret:     "  {{ http_secret }}  "
```


This will ensure you reduce your hits on the Docker hub registry, but requires a solid 2-3 days of engineering time to set up properly, plus ongoing maintenance to ensure HA and to scale out your storage cluster. You'll need to navigate storage configuration, network setup across all your runners, and probably a month of back-and-forth with that security-obsessed CIO who keeps asking "but why can't you just pay for Docker?!".


### Option 2: Authenticate with Docker Hub


Option 2 requires adding authentication to every workflow, which raises your limit to 100 pulls per hour:


```text
jobs:
build:
runs-on:     ubuntu-latest
steps:
-     name:     Login     to     Docker     Hub
uses:     docker/login-action@v2
with:
username:     ${{     secrets.DOCKERHUB_USERNAME     }}
password:     ${{     secrets.DOCKERHUB_TOKEN     }}


# Your existing steps...
```


However, this solution only partially works since it’s only a 10x increase on your pull limit and more importantly involves significant organizational overhead. While it may seem straightforward at first, consider the scale: a mid-sized company with 40+ engineers typically manages 50+ repositories containing 250+ workflow files. For larger organizations with 200+ engineers, these numbers grow exponentially—potentially reaching 2,000+ workflows spread across hundreds of repositories.


Without a comprehensive solution, humans will be humans and you'll likely experience weeks of intermittent CI failures as each undiscovered workflow hits rate limits at unpredictable times, creating a lengthy tail of disruption that affects developer productivity and release schedules.


### Option 3: Just use Blacksmith


Option 3 keeps your life simple and stress-free: just use Blacksmith runners. Blacksmith runners have the benefits of option 1—featuring a Docker pull-through cache mirror[enabled by default](https://docs.blacksmith.sh/github-actions-runners/docker-pull-through-mirror) —without the organizational overhead of option 2, thanks to our[migration wizard](https://docs.blacksmith.sh/introduction/quickstart) that automatically creates PRs for each repository needing review.[Try it for free!](https://app.blacksmith.sh/)


As a side effect, you'll not only have dealt with the new Docker Hub usage limits, but you'll have reduced your CI complexity while simultaneously increasing your CI performance. Blacksmith handles all your infrastructure, scaling, and storage needs behind the scenes—while delivering fast CI performance.


```text
jobs:
build:
runs-on:     blacksmith
# The rest of your workflow remains the same
```


### Beyond just solving rate limits


While solving rate limits is the immediate concern, we're taking this opportunity even further. Our[Sticky Disks](https://docs.blacksmith.sh/github-actions-runners/sticky-disks) don't just circumvent Docker's rate limits - it eliminates the entire Docker pull problem for both public AND private images:


- **Near-instant container startup** : Your CI doesn't waste precious minutes waiting for Redis, PostgreSQL and other services to download and initialize
- **Zero extraction overhead** : We eliminate both download AND extraction time completely


With Sticky Disks, we're not just helping you avoid the April 1st Docker rate limits - we're trying to make those pulls in your workflow a no-op.


**You have a week left before Docker's new pull limits take effect on April 1st.** Docker changing the rules in this manner is an infrastructure headache, but I suggest you don't wait until your CI pipelines start breaking and your team loses hours fighting flaky CI. We offer a quick, yet effective solution today that can have your CI pipelines fully protected in under 5 minutes - no auditing hundreds of workflows, no setting up complex mirrors, and no sudden disruptions to your development process.


‍
