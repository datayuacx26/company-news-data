---
schema_version: "1.0.0"
document_id: "7fa346309ce1b9d8b20293c7dc79baf89221b2e351e55b170066862116457f7a"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/openreplay-spots-get-stuck-with-you-re-viewing-the-original-recording-processed-spot-will-be-available-here-shortly/553"
published_at: "2025-12-12T16:37:36+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:4c4948c47b330f19e8def1f8b450f1195b928cbbc97c9c5f50e7394469c480ee"
---

# OpenReplay Spots get stuck with "You’re viewing the original recording. Processed Spot will be available here shortly."

[jayman](https://forum.openreplay.com/u/jayman)


December 12, 2025, 4:37pm 1


I have recently setup a self-hosted OpenReplay server on AWS following the guide on the docs page:[Deploy to AWS - OpenReplay Documentation](https://docs.openreplay.com/en/deployment/deploy-aws/) . I am using this for the OpenReplay Spots feature so we can record sessions for our application without changing the code (future plan). Sometimes when I record a session with the OpenReplay Spot extension (on Brave browser), I get stuck on the following message: “You’re viewing the original recording. Processed Spot will be available here shortly”.


This happens for about 30% of the recordings. So I do get some successful uploads and recordings but several are failing. How do I troubleshoot this issue?


I read on some forums that this may be a problem with version v1.21 but I am on the latest version - v1.23.0.


My server details are below:


> $ openreplay --status
> This machine is AMD (x86_64) architecture.
> \[INFO\] Using KUBECONFIG /etc/rancher/k3s/k3s.yaml
> Checking OpenReplay Components Status
> \[INFO\] OpenReplay Version
> v1.23.0-42551c5
> \[INFO\] Disk
> Filesystem Size Used Avail Use% Mounted on
> /dev/root 97G 16G 82G 17% /
> \[INFO\] Memory
> total used free shared buff/cache available
> Mem: 7.6Gi 3.4Gi 375Mi 27Mi 3.9Gi 3.9Gi
> Swap: 0B 0B 0B
> \[INFO\] CPU
> Linux ip-172-31-89-153 6.8.0-1043-aws #45~22.04.1-Ubuntu SMP Wed Nov 12 16:16:28 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
> PRETTY_NAME=“Ubuntu 22.04.5 LTS”
> CPU Count: 2
> \[INFO\] Kubernetes
> Flag --short has been deprecated, and will be removed in the future. The --short output will become the default.
> Client Version: v1.25.0
> Kustomize Version: v4.5.7
> Server Version: v1.31.5+k3s1
> WARNING: version difference between client (1.25) and server (1.31) exceeds the supported minor version skew of +/-1


[mehdi](https://forum.openreplay.com/u/mehdi)


December 12, 2025, 4:53pm 2


What version of Spot extension are you using? Can you try with latest` v1.0.27` ? Also please try on Chrome (not Brave) and tell us if it’s better.


[jayman](https://forum.openreplay.com/u/jayman)


December 13, 2025, 12:15am 3


I am on v1.0.26. We have seen the problem on both Chrome and Brave - but I can stay on Chrome and continue my tests. I will also upgrade the extension.


Meanwhile I am trying to review the logs from the spot container with the following command.


> $ kubectl logs -n app deploy/spot-openreplay | head -1
> {“level”:“info”,“ts”:“2025-12-12 14:44:02.857”,“caller”:“server/server.go:67”,“msg”:“server successfully started on port 8080”,“level”:“info”}


Any pointers on what to look for and how to enable debug logging would be appreciated.


[mehdi](https://forum.openreplay.com/u/mehdi)


December 18, 2025, 4:02pm 4


Can you update your Spot extension to` v1.0.30` (latest) and also run` openreplay -u` in your OpenReplay instance/backend, then give it another try?
