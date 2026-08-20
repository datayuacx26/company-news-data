---
schema_version: "1.0.0"
document_id: "ddcd59bdc286b12998483ee5b2d4754742a9b2f9a6e8da725baed2f73ef5c960"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/issue-with-openreplay-ingress-nginx-controller-while-installation/569"
published_at: "2026-05-13T05:59:28+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:5c11b79a2ca0523fe057a01fb9bde511708b116c9f119d8c9c16a7db8467e34b"
---

# Issue with openreplay-ingress-nginx-controller while installation

[shlyka](https://forum.openreplay.com/u/shlyka)


May 13, 2026, 5:59am 1


At the moment, I’ve run into another issue with the OpenReplay installation. It has simply stopped installing using the official instructions from the website.
It gets stuck while configuring openreplay-ingress-nginx-controller and cannot assign an external IP to it


sudo wget[https://raw.githubusercontent.com/openreplay/openreplay/main/scripts/helmcharts/openreplay-cli](https://raw.githubusercontent.com/openreplay/openreplay/main/scripts/helmcharts/openreplay-cli) -O /bin/openreplay
sudo chmod +x /bin/openreplay
openreplay -i[ss.mysite.com](http://ss.mysite.com/)


It will hang for a long time at that step,


ready.go:258: \[debug\] Service does not have load balancer ingress IP address: app/openreplay-ingress-nginx-controller


and then it uninstalls itself into some unclear/broken state.


[shlyka](https://forum.openreplay.com/u/shlyka)


May 13, 2026, 7:52am 2


For those who encounter this issue: two CPU cores are not enough for openreplay-ingress-nginx-controller to complete within the timeout. It was insufficient both in our data center, on Azure D-series instances, and on AWS t3 instances. With 4 CPU cores, the installation completes successfully.


[mehdi](https://forum.openreplay.com/u/mehdi)


June 3, 2026, 4:57pm 3


This has been fixed. Please make sure to pull the changes from the repo and try again.
