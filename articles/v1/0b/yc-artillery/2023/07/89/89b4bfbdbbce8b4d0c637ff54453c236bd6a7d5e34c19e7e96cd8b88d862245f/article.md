---
schema_version: "1.0.0"
document_id: "89b4bfbdbbce8b4d0c637ff54453c236bd6a7d5e34c19e7e96cd8b88d862245f"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/announcing-the-artillery-github-action"
published_at: "2023-07-20T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:c4d339c4a4955cf4d313adfee9c6c44f36f28ea1eb96f26c7dbeea3c8223ff3b"
---

# Announcing the Artillery GitHub Action

July 20th, 2023[Announcement](https://www.artillery.io/blog/tag/announcement)


# Announcing the Artillery GitHub Action


Artem Zakharchenko


You can use the` artilleryio/action-cli` to set up load testing faster in your new or existing GitHub workflows. We designed the action to mirror our CLI so you get a consistent experience no matter how you decide to use Artillery.


Start small with tests running directly in your GitHub workflows, or scale out on your[AWS Lambda](https://www.artillery.io/docs/load-testing-at-scale/aws-lambda) and[AWS Fargate](https://www.artillery.io/docs/load-testing-at-scale/aws-fargate) .


## Usage example


The best way to illustrate our new GitHub action is to see it in practice.


Let’s write a simple workflow that makes sure all load tests pass on the staging environment before promoting it to production.


```text
# .github/workflows/prod-deploy.yml
name  :   prod-deploy
on  :
push  :
branches  : [  main  ]
jobs  :
deploy  :
runs-on  :   ubuntu-latest
steps  :
-   name  :   Checkout
uses  :   actions/checkout@v3


-   name  :   Load tests
id  :   load-test
uses  :   artilleryio/action-cli@v1
with  :
# Run load tests against the current staging environment
# as the candidate for the production deployment.
command  :   run ./staging.test.yml


-   name  :   Deploy
# Require the load tests to pass before deploying to production.
needs  : [  load-test  ]
run  :   ./deploy.sh
```


The GitHub action pairs perfectly with[Artillery Cloud](https://www.artillery.io/cloud) , where you can preview all your test runs, see the visualized metrics, and share them with your team.


> **[Join the waitlist for Artillery Cloud](https://www.artillery.io/cloud?tf=1) to get an early access to the platform!**


## Documentation


Learn more about[Using Artillery on GitHub Actions](https://www.artillery.io/docs/cicd/github-actions) , and see other examples, such as scheduled runs that make sure your application is always ready for the next traffic spike.


And don’t hesitate to reach out or[ask a question](https://github.com/artilleryio/artillery/discussions/new/choose) whenever you have one. We are here to help you test better.


### Useful links


- [GitHub repository](https://github.com/artilleryio/action-cli)
- [Using Artillery on GitHub Actions](https://www.artillery.io/docs/cicd/github-actions)
- [Artillery GitHub Action on GitHub Marketplace](https://github.com/marketplace/actions/artillery-load-testing-action)
