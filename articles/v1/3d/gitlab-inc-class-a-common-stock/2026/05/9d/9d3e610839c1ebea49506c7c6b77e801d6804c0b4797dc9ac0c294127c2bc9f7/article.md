---
schema_version: "1.0.0"
document_id: "9d3e610839c1ebea49506c7c6b77e801d6804c0b4797dc9ac0c294127c2bc9f7"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-news-import-53e8505a9d97"
canonical_url: "https://about.gitlab.com/blog/gitaly-on-kubernetes-generally-available/"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-25T06:37:24.143169+00:00"
fetched_at: "2026-07-28T21:44:45.479848+00:00"
content_hash: "sha256:01c4e078b9b637b2ded510d20894c53166b536cf6878d1475fbfbe229d156b27"
---

# Consolidate your GitLab stack with Gitaly on Kubernetes

With[GitLab 18.11](https://about.gitlab.com/whats-new/) came good news for teams running GitLab on Kubernetes: Gitaly on Kubernetes is now generally available. Teams hosting GitLab on Kubernetes previously faced the challenge of maintaining a hybrid setup — running most GitLab components in Kubernetes while keeping Gitaly on virtual machines. This hybrid architecture made day-to-day operations more complex for those teams. Those days are over; Gitaly on Kubernetes is now an officially supported deployment option.


### The road to Kubernetes


Gitaly has some hard requirements that don't translate naturally into a Kubernetes environment.


Git operations can be memory-intensive and their usage patterns are difficult to predict. To shield the main Gitaly process from out-of-memory (OOM) events and avoid downtime, Gitaly[can be configured to run each Git process inside a dedicated cgroup](https://docs.gitlab.com/administration/gitaly/cgroups/) . In this setup, the Gitaly process lives in a separate cgroup from those used by Git processes. If a Git process exceeds its cgroup's memory limit and gets terminated, the main Gitaly process remains unaffected.


Making this setup work in a Kubernetes Pod required additional work. Most Kubernetes clusters use` containerd` as their container runtime, and[until recently](https://github.com/containerd/containerd/issues/10924) ,` containerd` only allowed containers to write to` cgroupfs` if they were running in privileged mode. The solution was to mount` /sys/fs/cgroup` via an init container and make the path writable.


Pod restarts also required additional work. On a virtual machine, Omnibus can upgrade the Gitaly binary in place and reload gracefully by keeping the socket open while swapping out the process. On Kubernetes though, when a StatefulSet pod is replaced — whether due to a Helm upgrade, a node drain, or a configuration change — the Gitaly Pods are stopped and restarted. It's a hard stop, not a graceful reload. For Gitaly sharded for example, which does not offer high-availability, that means downtime which might not be acceptable for some customers.


Our solution was to make[client retries configurable](https://docs.gitlab.com/administration/settings/gitaly_timeouts/#gitaly-client-retries) . By configuring Gitaly clients — such as Rails — to retry requests long enough for Gitaly to restart and become available again, users may notice slightly higher latency during that brief window, but requests will ultimately succeed and downtime is avoided.


To confirm that client retries effectively eliminated downtime during upgrades, we ran a series of benchmarks. We executed common Git operations against two GitLab instances — one with Gitaly on VMs, and another on Kubernetes — then triggered an upgrade mid-test and tracked request success rates. The results:


Operation VM Success Rate Kubernetes Success Rate


git clone 100% 100%


git pull 100% 99.16%


git push 99.66% 100%


The numbers are nearly identical across both environments. What makes these results especially encouraging is the nature of Kubernetes itself — a Pod restart means an abrupt process termination and immediate socket closure, yet success rates remained this high. Full 100% success across every operation would require our high-availability solution,[Gitaly Cluster (Praefect)](https://docs.gitlab.com/administration/gitaly/praefect/) , which doesn't yet support Kubernetes — though that's actively being worked on, with general availability status on the horizon.


### What Gitaly on Kubernetes means for you


If you're running GitLab in hybrid mode — with some components on Kubernetes and Gitaly on VMs — you can now consolidate your infrastructure by moving Gitaly into the cluster. This eliminates the need to maintain and monitor a separate VM fleet alongside your Kubernetes nodes, bringing your entire GitLab stack under a single Kubernetes-managed environment.


If you're adopting GitLab for the first time and you already operate software on Kubernetes, you now benefit from a fully Kubernetes-native GitLab deployment provided with our[Helm chart](https://gitlab.com/gitlab-org/charts/gitlab) .


### Installing Gitaly on Kubernetes


The recommended way to deploy Gitaly on Kubernetes is through the[GitLab Helm chart](https://gitlab.com/gitlab-org/charts/gitlab) . Before getting started, it's worth reading through the[Gitaly on Kubernetes documentation](https://docs.gitlab.com/administration/gitaly/kubernetes/) , which covers key configuration guidance and helps you avoid common pitfalls.


Gitaly can be deployed either as part of a full GitLab installation, or as an external component. The[Gitaly on Kubernetes documentation](https://docs.gitlab.com/administration/gitaly/kubernetes/) covers both scenarios.


##


Are you just managing tools or shipping innovation?


[Get your DevOps maturity score](https://about.gitlab.com/assessments/devops-modernization-assessment/)


Quiz will take 5 minutes or less
