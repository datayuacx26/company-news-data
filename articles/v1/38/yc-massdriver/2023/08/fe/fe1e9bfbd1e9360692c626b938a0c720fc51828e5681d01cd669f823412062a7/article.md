---
schema_version: "1.0.0"
document_id: "fe1e9bfbd1e9360692c626b938a0c720fc51828e5681d01cd669f823412062a7"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/the-changing-face-of-open-source"
published_at: "2023-08-16T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:a5ba28b1b2e0c8f6e3c1963b29d7949e979663e494d93656944b15c18c9772cc"
---

# The Changing Face of Open-Source

*Disclaimer: We support the OpenTF initiative, but these opinions do not reflect the views of the initiative. Dave & I speak very plainly.*


In this curiously deleted[YouTube video](https://web.archive.org/web/20211230092711/https://www.youtube.com/watch?v=4BCR7Yx4k4o) , HashiCorp’s co-founders, Armon and Mitchell, passionately championed the virtues of open-source software. Their words resonated with many who believed that open source wasn’t just a coding philosophy but a way of life that fostered collaboration, innovation, and community. However, recent developments have led some to question whether HashiCorp’s path has strayed from its founding principles.


### A Commitment to Ecosystem and Community


Armon and Mitchell’s dedication to nurturing a broad ecosystem and acknowledging the power of community resonates deeply. They recognized the importance of an expansive network transcending company boundaries, enabling developers to solve complex problems collectively. At the core of their philosophy was the belief that integration, collaboration, and shared problem-solving could only thrive in an open-source environment.


*We genuinely believe Armon & Mitchell meant this when they said it, and still probably feel this way.*


### Unintended Consequences and Unforeseen Effects


The universe is tragically governed by entropy. The longer a company lives, the more the image of the founders fades away. HashiCorp’s evolution, driven by the changing dynamics of becoming a public company, has led to actions that have far-reaching consequences. The repercussions are being felt not only by other infrastructure companies and some SaaS enterprises but also by the open-source contributors themselves.


Two years ago, HashiCorp announced a[limitation on community contributions](https://www.theregister.com/2021/09/07/hashicorp_pause/) ([in their words](https://github.com/hashicorp/terraform/blob/ad634f60a5acbaade1eb8c225564e17ad2267f00/.github/CONTRIBUTING.md#:~:text=Due%20to%20current) ) due to a lack of internal resources for the Terraform project. In the meantime, communities of open-source developers and for-profit organizations have begun adding value *around* products like Terraform, diminishing HashiCorp’s ability to realize a profit on their internal efforts.


The decisions that HashiCorp has made, though understandable from a business standpoint, didn’t account for the broader impact on the open-source community.


Yesterday the CNCF posted recommendations for the foundation’s projects to[“switch to an alternative component”](https://github.com/cncf/foundation/blob/main/source-available-recommendations.md#switch-to-an-alternative-component) for dependencies with source available licenses. If CNCF stops using HashiCorp’s tools for development, *much* of the community will disappear. This will impact all people who “don’t care” because they are just an end-user of HashiCorp’s tools.


*NOTE: The CNCF is*[investigating](https://github.com/cncf/foundation/issues/617) *the impact of the BUSL changes. It’s creating a lot of busy work for A LOT of people.*


**One thing is clear: Open source community no longer matters to HashiCorp.**


### An Act of Rebellion


In the blog post in which they announced the[BUSL change](https://www.hashicorp.com/blog/hashicorp-adopts-business-source-license) , they claimed:


> *there are other vendors who take advantage of pure OSS models, and the community work on OSS projects, for their own commercial goals, without providing material contributions back. We don’t believe this is in the spirit of open source*


While HashiCorp’s journey into the realm of open source has been marked by innovation and influence, the dynamics within its community have shifted over time. Take, for instance, the Terraform GitHub project. With thousands of issues and nearly 200 unmerged PRs, the landscape reveals an ecosystem that, while not hostile, lacks the welcoming aura that defines the open-source ethos. What’s evident is a somewhat perplexing situation: cordial rejections, a touch of passive-aggressive behavior, and a sense of inflexibility in decision-making.


Pushing community-contributed ideas aside until HashiCorp’s internal gears are ready for action has led some to step back and wait.


What is evident is that the companies that HashiCorp said don’t provide “material contributions” in fact, really want to based on the traction and camaraderie the OpenTF initiative has gained in the past week.


HashiCorp never asked for help, they never made a call for maintainers.


Their BUSL argument is smoke and mirrors because HashiCorp, for years, has developed in the open but not fostered community. People invested in the idea that HashiCorp created a community, but they never did that. They created an ecosystem shrouded in the idea of community. Their CEO, Dave McJannet, even brags ([in another deleted video](https://web.archive.org/web/20230401054108/https://www.youtube.com/watch?v=59rEiAyYEVk) @ 18m-20m) about how they played communities off of each other in a “malicious user journey.” He then doubles down on how “malicious is the right word.”


*Their open-source* ***commitment*** *is full of shit.*


### Lessons for the Future


The unique thing about software engineers is the blurred line between hobby and profession. It becomes part of who engineers are socially. The desire to create community and work alongside other great engineers is what has allowed software to eat the world.


We can’t let companies profit off our desire to create things and work together. The lesson to carry forward is that the open-source ethos isn’t impervious to the realities of the market. It might be time to seriously consider the sustainability of open-source models outside the protective walls of a foundation or a well-defined structure. The broad repercussions of HashiCorp’s journey serve as a cautionary tale for anyone who believes that open-source can flourish without deliberate care and strategic planning.


### The Fallout


We’re compiling a list of open-source projects impacted by HashiCorp’s actions. If your project has been impacted, please mention[@coryodaniel](https://github.com/coryodaniel) or[@xpositivyx](https://github.com/xpositivityx) on your GitHub issue so we can catalog them here:


- Suggesting that the impact may effect vendors and that Vagrant shouldn’t be used in any CNCF workflows. Suggesting replacing Vagrant for Lima VM[link](https://github.com/cncf/foundation/issues/617#issuecomment-1678134183)
- Kubernetes` kind` may need to remove Vagrant[link](https://github.com/cncf/foundation/issues/617#issuecomment-1675227440)
- Kubernetes` image-builder` uses packer and is an ‘immediate concern’[link](https://cs.k8s.io/?q=packer&i=nope&files=&excludeFiles=&repos=kubernetes-sigs/image-builder)
- Containerd may be moving from Vagrant to Lima VM[link](https://github.com/cncf/foundation/issues/617#issuecomment-1675622002)
- Flux2 is trying to figure out what do do about e2e testing they have invested “tremendous” time in using Terraform and Vault[link](https://github.com/fluxcd/flux2/issues/4156)
- The Argo Project is keeping an eye on a few libraries to make sure they don’t change from MPL -> BUSL[link](https://github.com/argoproj/argoproj/issues/236)
- Jaeger has a proposal to remove the` go-plugin` library owned by @HashiCorp[link](https://github.com/jaegertracing/jaeger/issues/4647)
- Lima VM dropped their Nomad template. Its “no longer free software”[link](https://github.com/lima-vm/lima/pull/1728/files)
- May impact users of Dapr *if they are a competitor*[link](https://github.com/dapr/dapr/issues/6815)
- Thanos has an open an issue to investigate impact[link](https://github.com/thanos-io/thanos/issues/6623)


‍
