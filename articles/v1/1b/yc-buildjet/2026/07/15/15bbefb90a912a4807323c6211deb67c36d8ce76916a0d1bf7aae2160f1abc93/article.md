---
schema_version: "1.0.0"
document_id: "15bbefb90a912a4807323c6211deb67c36d8ce76916a0d1bf7aae2160f1abc93"
company_key: "yc-buildjet"
company: "BuildJet"
source_id: "yc-buildjet-news-import-6b2f4e2d5236"
canonical_url: "https://buildjet.com/for-github-actions/blog/what-hardware-powers-github-actions"
published_at: null
first_seen_at: "2026-07-24T23:34:21.347694+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:254ccfdc1eb7913d11004b346581b8e6ac777413a4728f113c628856be79cb0e"
---

# What hardware powers GitHub Actions?

It is common practice for CI's to simply state the amount of vCPUs at the user's disposal. We do the same and find it generally to be a good abstraction. Cross-service comparison gets easier and the users don't need to dabble with the details. As this post is meant to explore the hardware of GitHub Actions, we decided to dabble with the details. To do this we ran lscpu 50 times on the GitHub Actions hardware ([source](https://github.com/BuildJet/lscpu/blob/master/.github/workflows/ci.yml) ):


```text
yml  1   name  :   Display information about GitHub Actions CPU architecture
2    on  :   push
3    jobs  :
4       build  :
5         strategy  :
6           matrix  :
7             runs-on  :     [  ubuntu  -  latest  ]
8             name  :     [  1  ...  50  ]
9         name  :   Display lscpu   -   $  {  {  matrix.name  }  }
10         runs-on  :   $  {  {  matrix.runs  -  on  }  }
11         steps  :
12           -     name  :   Run lscpu
13             run  :   lscpu


```


After running the above workflow and cross-pollinating it with[cpubenchmark](https://www.cpubenchmark.net/) data, we got the following results: Looking at the GitHub hardware table we can extract a few things:


- If you're unlucky, **GitHub Actions runs your workflow with old hardware from 2015.**
- One of the processors, the 8272CL (C = Cloud, L = Large memory) is only sold through OEM channels and we were not able to find a normalized benchmark for it, hence the lack of a single-thread score. Although we did find a benchmark for the newer and better[827 5 CL](https://www.cpubenchmark.net/cpu.php?cpu=Intel+Xeon+Platinum+8275CL+%40+3.00GHz&id=3624&cpuCount=2) . It lands on an underwhelming 2375 single-thread score, so the 8272CL would likely score less than that.
- Low clock speed becomes a problem, especially in single-threaded workloads. However, the lack of a single-thread score could be mitigated by only offering it to projects that can and want to utilize many cores.
