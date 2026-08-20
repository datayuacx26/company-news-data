---
schema_version: "1.0.0"
document_id: "8dc9644fd524af3256a4178b721ca70366c1df257b0c16989d7c579f355b4d51"
company_key: "yc-glasskube"
company: "Glasskube"
source_id: "yc-glasskube-news-import-f40b83a58804"
canonical_url: "https://distr.sh/blog/distr-dashboard/"
published_at: "2025-05-21T00:00:00+00:00"
first_seen_at: "2026-07-21T21:44:16.826534+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:57afbc134d62a12bdfa08a995b52f50d3a0d64694f222139564e6e733b76344e"
---

# Distr Launch Week: The new Dashboard

I am Philip, an engineer working at Glasskube, which helps[software and AI companies](https://distr.sh/glossary/isv-meaning/) distribute their applications to[self-hosted environments](https://distr.sh/glossary/self-hosted-software/) . We build an Open Source Software Distribution platform called Distr ([github.com/distr-sh/distr](https://github.com/distr-sh/distr) ).


Adding support for multiple deployments per target and introducing metrics forced us to rethink our current dashboard and led to redesigning it from scratch.


---


## Previous version of the Distr Dashboard


Initially we thought that AI companies and software vendors would deploy with Distr around the globe, annotating every deployment with the respective geolocation. Additionally, we added some graphs on the dashboard to visualize the uptime of deployment targets over time. To be quite frank, our users didn’t mind having these components on our old dashboard, but they also didn’t find them useful.


*Previous version of the Distr Dashboard*


With the recent[introduction of our container registry](https://distr.sh/docs/registry/) we are now also supporting[fully self-managed](https://distr.sh/docs/use-cases/fully-self-managed/) environments in addition to agent-based[assisted self-managed](https://distr.sh/docs/use-cases/assisted-self-managed/) environments.


## Current version of the Distr Dashboard


Our new dashboard is a unified view for software vendors to check all their customers on one page.


*Current version of the Distr Dashboard*


### Moving from a deployment table to cards


The main reason for the switch to cards was the introduction of multiple deployments per deployment target in[Distr v1.6](https://github.com/distr-sh/distr/releases/tag/1.6.0) . Often software vendors receive a pretty beefy VM or quite large namespace to deploy their software to. Modern applications consist of multiple application components which might need to be deployed independently of each other, so the docker compose and[Helm Charts](https://distr.sh/glossary/helm-chart/) stay in a manageable size.


With the introduction of cards we are now able to display a list of all deployed applications on a deployment target.


### Adding CPU & Memory Utilization


Deployment target metrics have been introduced in[Distr v1.9](https://github.com/distr-sh/distr/releases/tag/1.9.0) . We extended our agent to not only reconcile the application deployment but also collect metrics from the target when configured.


Read more about[Metrics Collection](https://distr.sh/docs/agents/logs-and-metrics/#metrics) in our documentation.


These newly introduced metrics allow us to calculate the CPU and memory utilization of the deployment targets. These metrics allow you to spot any resource constraints directly on the Dashboard.


### Avatars


The introduction of images was a rather small addition in[Distr v1.6](https://github.com/distr-sh/distr/releases/tag/1.6.0) , but allows users to associate customers and deployments with images to further customize their Distr experience.


### Performance


Removing globe.js and our charting library allowed us to reduce the bundle size for our application from around 4.3MB to 1.9MB. 😎


- Current Distr Dashboard
- Previous Distr Dashboard


Terminal window


```text
# @distr-sh/distr@1.9.1 build    ng     build     --configuration=production     --source-map=true
❯     Building...    ✔     Building...    Initial     chunk     files       |     Names                    |      Raw     size     |     Estimated     transfer     size    main-QFVDKA7M.js          |     main                     |     326.00     kB     |                    91.94     kB    chunk-63CHSFAT.js         |     -                        |     300.78     kB     |                    76.87     kB    chunk-BHIO7JRK.js         |     -                        |     166.59     kB     |                    49.35     kB    styles-QI4PTBSE.css       |     styles                   |     138.72     kB     |                    14.71     kB    chunk-THV5RBBS.js         |     -                        |     107.63     kB     |                    25.78     kB    polyfills-Q763KACN.js     |     polyfills                |      34.57     kB     |                    11.36     kB    chunk-LFJUVAQA.js         |     -                        |       1.03     kB     |                   471     bytes
Initial     total            |       1.08     MB     |                   270.49     kB
Lazy     chunk     files          |     Names                    |      Raw     size     |     Estimated     transfer     size    chunk-RMHGQFCR.js         |     app-logged-in-routes     |     704.54     kB     |                   154.39     kB    chunk-5L57ZTDX.js         |     browser                  |      63.98     kB     |                    17.13     kB    chunk-YLK7DU7V.js         |     nav-shell-component      |      23.30     kB     |                     5.43     kB    chunk-JISBMPPF.js         |     -                        |       4.30     kB     |                     1.48     kB
```


Terminal window


```text
# @distr-sh/distr@1.4.5 build    ng     build     --configuration=production
❯     Building...    ✔     Building...    Initial     chunk     files       |     Names                    |      Raw     size     |     Estimated     transfer     size    chunk-ZPNOAYI5.js         |     -                        |     635.51     kB     |                   159.45     kB    scripts-PGJSDJ25.js       |     scripts                  |     572.84     kB     |                   128.09     kB    main-LYFHJHR3.js          |     main                     |     475.40     kB     |                   109.60     kB    chunk-5RL27WWB.js         |     -                        |     197.69     kB     |                    57.67     kB    chunk-NA52VAIS.js         |     -                        |     130.22     kB     |                    34.99     kB    styles-MX7U44S5.css       |     styles                   |      59.89     kB     |                     9.31     kB    chunk-XGBYAQK4.js         |     -                        |      47.58     kB     |                    13.69     kB    polyfills-FFHMD2TL.js     |     polyfills                |      34.52     kB     |                    11.28     kB    chunk-7BXBZ5JU.js         |     -                        |       1.91     kB     |                   840     bytes
Initial     total            |       2.16     MB     |                   524.93     kB
Lazy     chunk     files          |     Names                    |      Raw     size     |     Estimated     transfer     size    chunk-3HCZJOWX.js         |     dashboard--component     |       1.76     MB     |                   360.09     kB    chunk-3OFHP4XL.js         |     apexcharts-esm           |     571.10     kB     |                   127.67     kB    chunk-FYY72LAW.js         |     browser                  |      63.97     kB     |                    17.13     kB    chunk-GCIIXV6Y.js         |     home-component           |       1.25     kB     |                   649     bytes
```


## Conclusion


**Is anything missing?** You can try out our new dashboard yourself by signing up for a free Distr account at[Get Started](https://distr.sh/get-started/) .


We are very excited about this new dashboard and hope you also like what we built. If you have any other feedback, please let us know!


P.S.: Read all our announcements of this week’s launch week here:[#distr-launch-week](https://distr.sh/blog/)
