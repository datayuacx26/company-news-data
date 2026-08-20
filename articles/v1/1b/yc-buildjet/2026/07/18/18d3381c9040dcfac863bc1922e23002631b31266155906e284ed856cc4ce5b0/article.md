---
schema_version: "1.0.0"
document_id: "18d3381c9040dcfac863bc1922e23002631b31266155906e284ed856cc4ce5b0"
company_key: "yc-buildjet"
company: "BuildJet"
source_id: "yc-buildjet-news-import-6b2f4e2d5236"
canonical_url: "https://buildjet.com/for-github-actions/blog/hosted-arm-runners-on-github-actions"
published_at: null
first_seen_at: "2026-07-24T23:34:21.347694+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:a36479f2b2922e58ff8431e57ed51bf7abb5e4397fd70e7feff4d3abe5e8bb80"
---

# Hosted ARM runners on GitHub Actions

Today, we’re happy to release the **first-ever hosted ARM runners for GitHub Actions** . This will enable many thousands of developers to finally get native support for their ARM workflows. **BuildJet for GitHub Actions ARM runner makes your workflows 10x faster** than what is currently possible on GitHub Actions.


With Apple's recent switch to ARM-based M1 processors, we see a growing requirement for ARM-compatible tooling and applications. This has emphasized the need for developers to test and build applications for ARM. However, building and testing your ARM applications is extremely slow as GitHub Actions does not natively support ARM.


To work around this on GitHub Actions, developers fall back to emulating ARM through QEMU. This is extremely slow and can become costly, and in numerous instances it runs for hours.


# 10x faster and 2x cheaper


In our testing, we found that BuildJet for GitHub Actions finished 10x faster than GitHub Actions. In addition to the incredible gains in speed, GitHub Actions is 2 times as expensive as BuildJet for GitHub Actions.


Our 2vCPU ARM runner costs $0.004/min, compared to GitHub Actions runner $0.008/min for their 2vCPU x86 runner.


To benchmark the runner, we used sentry’s docker container snuba. Snuba builds to a docker container and supports the arm architecture. We ran the build on both GitHub Actions and on the new BuildJet ARM runners. The results are below:


Check out the benchmark workflow run[here](https://github.com/BuildJet/snuba/actions/runs/2749694524) for more details.


If this sounds interesting, and you'd like to use BuildJet for GitHub Actions, simply authorize BuildJet on the repositories you want to use and then update the` runs-on` property in your` workflow.yml` .


```text
yaml  1   runs-on  :   buildjet  -  4vcpu  -  ubuntu  -  2204  -  arm


```


For more information, please have a look at[our documentation](https://buildjet.com/for-github-actions/docs) .


Happy building!
