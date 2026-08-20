---
schema_version: "1.0.0"
document_id: "484d04909d5931ffd95cf100e026be14f19c3ed5619696c7aa6568769e1e1065"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/console-container-updates/"
published_at: "2024-11-25T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:15b5fa66784d74958b5c2e1fa846c774d46e51ec3140e302076382238e088fc4"
---

# Console container updates

We are rolling out some updates to the[Console](https://sst.dev/docs/console) to better support container applications.


### Logs


You can now see **logs from your containers** in the Console. In addition to the logs from your functions. Head over to your **App** > **Stage** > **Logs** .


As a bonus, you can see logs from any CloudWatch log group that your app creates.


### Autodeploy


The Console can auto-deploy your apps when you *git push* to your GitHub repo. We added the following updates recently.


- **VPC** : You can now configure Autodeploy to run in your VPC using[autodeploy.runner.vpc](https://sst.dev/docs/reference/config#vpc) . This is useful if your builds need to access private resources.
- **Caching** : You can now configure files or directories to be cached as a part of your build process with[autodeploy.runner.cache](https://sst.dev/docs/reference/config#cache) . This can help speed up builds.
- **Extended timeouts** : Autodeploys now have a maximum timeout of 36 hours. This is useful for apps that take a long time to build.
- **New runner sizes** : There are new` xlarge` and` 2xlarge` sizes with around 70GB and 145GB of memory; helpful for large applications.


Check out the updated[console.autodeploy](https://sst.dev/docs/reference/config#console-autodeploy) config.


### Other updates


While not strictly related to containers we also added support for:


- **Manual deploys** : You can now manually trigger a deployment from **App** > **Autodeploy** > hitting the **Deploy** button. You can pass in a Git ref and the stage you want to deploy to.
- **Redeploys** : You can now redeploy any past deploy through the Console.
- **Cancel deploys** : You can also cancel a deploy that’s in progress through the Console.


---


### About Autodeploy


If you haven’t used Autodeploy before, we designed it to be a better fit for SST apps when compared to alternatives like GitHub Actions or CircleCI.


1. **Easy to get started** , supports branch and PR workflow out of the box.
2. **Configurable** , customize your workflow through your` sst.config.ts` .
3. **Runs in your AWS account** , builds are run in your AWS account and can use your VPC.


And it integrates with the rest of the SST Console.[Learn more about Autodeploy](https://sst.dev/docs/console#autodeploy) to get started.
