---
schema_version: "1.0.0"
document_id: "51e017d87d27235bf943757b3529a17a2405226dcb49ec3abe2207bf2c2b375e"
company_key: "yc-buildjet"
company: "BuildJet"
source_id: "yc-buildjet-news-import-6b2f4e2d5236"
canonical_url: "https://buildjet.com/for-github-actions/blog/why-github-actions-is-so-slow"
published_at: null
first_seen_at: "2026-07-24T23:34:21.347694+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:3d0b259b6415c45a460e4a16e269e251ca6af6cbd1a9c4c74923e2157dbbc59c"
---

# Why GitHub Actions is so slow

In this breakdown we will take a close look at why GitHub Actions default runners are so slow. Firstly, we'll compare GitHub Actions runners with a MacBook Pro 2015 running the official self-hosted software. We'll then detail what actual hardware is powering the GitHub Actions runners.


# Hardware


For the comparison, we will go with 2 different hardware setups:


- **GitHub Actions** - the only runner provided by GitHub Actions, a 2vCPU machine.
- **MacBook Pro 2015** - i7 4 cores/16GB, which runs on GitHub's official self-hosted runner software,[actions-runner](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners) . We virtualize Ubuntu 20.04 on it, mainly due to the dependency requirements of most CI workflows.


# Software


We have chosen three codebases that we think cast a wide net. Each project runs mainly in a different language, representing a different type of codebase, with different types of characteristics.


- **Facebook's Folly** - Core C++14 library components used extensively at Facebook
- **Nextcloud's bookmarks** - Web app written in PHP and javascript
- **Rust’s regex library** - A high-performance regex lib in rust


# Facebook’s Folly


Main takeaways:


- A MacBook Pro from 2015 beats the GitHub Actions hardware
- This is a clear example of a project that can utilize more available cores


For more details check out the GitHub Actions[CI run page](https://github.com/BuildJet/folly/actions/runs/1371433714) .


# NextCloud Bookmarks


Main takeaways:


- GitHub Actions only runs ~5% faster than a MacBook Pro 2015.
- The Selenium tests are single-threaded and only gain performance due to the single core speed improvement.
- Selenium tests are not parallelized, and only run sequentially. Thus, you don't see any speedups from more cores.


For more details check out the GitHub Actions[CI run page](https://github.com/BuildJet/bookmarks/actions/runs/1371843769) .


# Rust’s regex library


Rust build times are notoriously known to be slow, however,[there are ways to minimize the slowdowns](https://matklad.github.io/2021/09/04/fast-rust-builds.html) . Like many other languages, a proper amount of speedups requires CI speed is a dev team goal.


Main takeaways:


- A MacBook Pro from 2015 beats the GitHub Actions hardware
- Another clear example of a project that can utilize more available cores


For more details check out the GitHub Actions[CI run page](https://github.com/BuildJet/regex/actions/runs/1367320277) .


# Too big for speed


We decided to split the CI run into three parts: *downloading* , *building* , and *testing* . We did this mainly to highlight which part of the CI workflows can be addressed with faster hardware.


As seen in the charts above, time spent *downloading* does not decrease substantially with better hardware, mostly because it is network & IO-bound. The expensive parts of a CI workflow are during the build and test phase, where the workload mostly is CPU-bound and this is where good hardware counts.


# What hardware is actually GitHub Actions using?


It is common practice for CI's to simply state the amount of vCPUs at the user's disposal. We do the same and find it generally to be a good abstraction. Cross-service comparison gets easier and the users don't need to dabble with the details. As this post is meant to explore the performance of GitHub Actions, we decided to dabble with the details. To do this we ran lscpu 50 times on the GitHub Actions hardware ([source](https://github.com/BuildJet/lscpu/blob/master/.github/workflows/ci.yml) ):


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


# Athletes in a contest of performance?


One might think we don't like GitHub Actions, but we do. We enjoy the close integration with GitHub and the free CI for public repositories. Nevertheless, we also see the value in offering newer and faster hardware to users that need it.


Our best theory in the case of GitHub Actions is that the slow hardware seems to be self-inflicted due to the close marriage to the Microsoft Azure cloud. Painting with a larger brush, we also think the odd choice of hardware is a widespread problem in the entire CI ecosystem and not limited to GitHub Actions.
