---
schema_version: "1.0.0"
document_id: "1554b5fdf712c3cfd6e4aa9f8b20d03299cfe3af73fbefca80b48b80574d7d5a"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-25a2b11929a5"
canonical_url: "https://zeropath.com/blog/toward-actual-benchmarks"
published_at: "2024-11-13T00:00:00+00:00"
first_seen_at: "2026-07-24T06:49:27.067531+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:c1497143f5aba8354dbc318ceb6592b685391c8390743092c5760d5f69f48ba0"
---

# Towards Actual SAST Benchmarks

In theory, code scanning tools and dynamic testing tools should have eliminated much of the OWASP top 10 years ago. In practice, static analysis of arbitrary applications are difficult, there are a lot of vendors of widely varying quality and many of these vendors gatekeep their product behind a 100k contract sizes.


The best way to prove that a scanner works well is by finding[active vulnerabilities in popular software](https://zeropath.com/blog/0day-discoveries) . After all, if your "AI" isn't effective or reliable enough to find problems in current applications, why should the security community care? That said, benchmarks are still useful, because with numbers, you can quantify differences between vendors and track their improvement or regression over time.


There have been several attempts at benchmarking bugfinding programs. However, before the advent of LLMs, projects like[the OWASP benchmark](https://github.com/OWASP-Benchmark/BenchmarkJava) were aimed towards testing specific abilities like correct control flow analysis instead of a broader capability to find security problems in realistic applications. It was simply not expected in 2015 that security tools would be able to differentiate test code from production code read comments and identifiers, understand auth, or infer deployment details of an application from things like Dockerfiles.


Yet nowadays, they can, which is why the ZeroPath team was excited to see that XBOW open-sourced their DAST benchmarks[here](https://github.com/xbow-engineering/validation-benchmarks) . As the README claims, "Several external contractors were engaged in \[their\] development... with the intention of mirroring the variety of vulnerability classes typically encountered by our security team during their routine pen testing/bug bounty engagements."[We were so excited we decided to fork it so that we could test major SAST vendors](https://github.com/ZeroPathAI/validation-benchmarks) !


The good news is that we did very well, for a breakdown on the stats check out our[blog post](https://zeropath.com/blog/benchmarking-zeropath) .


## First: What was the XBOW Benchmark?


The original XBOW benchmark is a set of 104 toy applications, generally written in Python and PHP. Each of these apps includes at least one security problem. The applications are written in such a way that exploiting them presents a flag, like a CTF challenge; to confirm that the AI has exploited the challenge, it passes that flag back to the overseer, and the overseer marks that challenge as completed if the flag is correct.


XBOW also apparently compared their tool's performance on the benchmark with[real-world](https://xbow.com/blog/xbow-vs-humans/) performance by humans, and matched or exceeded the performance of real people. So there's no doubt that XBow has created a remarkable tool. Security problems in the XBOW benchmark even include standard issues like command injection and XSS, as well as business logic problems like IDOR. The last part is nice because auth and business logic issues are common finds in actual security assessments but haven't been built into any benchmarks until now.


## Adding False Positives


So why fork it? Well, first, the XBOW benchmark only reports one summary statistic: the percentage of challenges finished. As anyone who's used a SAST offering knows, the primary problem with most application security scanners is not that they report nothing, it's that they report lots of incorrect findings or findings of no actual significance.


If you're not a CISO or an Appsec engineer, this problem might sound trivial. But most code does not contain security flaws, and so much like a test for a rare disease, a tool that invalidates 99% of potential issues will still mostly report garbage when run against a new, large codebase. So, in a sense, designing security benchmarks the way XBOW did - such that the findings it reports are unambiguous - sidesteps a key part of the problem, especially when you consider how difficult it is to get very high reliability out of LLMs.


A better benchmark would measure whether the tool is also reporting bugs that are not there. To fix this, instead of having the static analysis tool retrieve a flag, we took the same benchmarks for each of the 25 challenges and added a counterpart that provides a tricky patch. The tool is run on both the patched and unpatched challenges; if it reports a bug on the latter, its score is noted in the false positive section.


## Removing Hints


On top of that, since Xbow develops a black-box security testing tool, they naturally don't expect it to have access to the code. This means the benchmark developers were free to include variable names and comments that referred to the problem the AI was supposed to find. Since we wanted to test white box scanners, we had to do our best to remove those 'hints' in order to present a fair comparison with the scanners that don't use LLMS. We also had to modify configuration files like Dockerfiles that a static analysis tool could read in order to understand the problem it was expected to find.


XBOW also includes 'challenge hints' that are designed to be consumed by humans or tools hacking the application. For example, in challenge '6', the description says:


> A simple SQLi example with an allowlist filter of words to try to prevent exploitation.


On the one hand, these directions are common features of CTFs. On the other hand, what these benchmarks are supposed to measure is whether or not your tool can find bugs in the wild, without prompting. Giving 'tips' like this makes a test strictly less representative, and for ZeroPath's purposes, it also provides an unfair advantage to LLMS over legacy tools. So, we removed them entirely from the test; instead the static analysis tool is simply presented with the


## Hints to Dynamic Testers


One thing we didn't expect to find when starting to do that was that the hints also leaked into the output of the toy web applications themselves.[For example, XBEN-020-24, an SSRF challenge gives the following index to the bot](https://github.com/xbow-engineering/validation-benchmarks/blob/a0fe6cf5e01196def7e8bd788d6c5dffa325a8c4/benchmarks/XBEN-020-24/ssrf-demo-app/index.php) , which contains the class of vulnerability in the title:


```text
<!  DOCTYPE     html  >
<  html     lang  =  "  en  "  >
<  head  >
<  meta     charset  =  "  UTF-8  "  >
<  meta     name  =  "  viewport  "     content  =  "  width=device-width, initial-scale=1.0  "  >
<  title  >  SSRF Demo  </  title  >
</  head  >


...Removed for Brevity...


</  html  >
```


This also extended past conventional technical vulnerabilities and into broken authentication vulnerabilities.[XBEN-069-24](https://github.com/xbow-engineering/validation-benchmarks/tree/a0fe6cf5e01196def7e8bd788d6c5dffa325a8c4/benchmarks/XBEN-069-24) , a PHP application, labeled as a medium challenge containing an IDOR (and a privilege escalation),[has the word IDOR plastered on every page accessible to the bot](https://github.com/xbow-engineering/validation-benchmarks/blob/a0fe6cf5e01196def7e8bd788d6c5dffa325a8c4/benchmarks/XBEN-069-24/app/website/profile.php#L22) :


```text
<  title  >  User Profile | IDOR Eats  </  title  >
```


Even in the context of dynamic testing, this gives an AI unfair advantage over traditional tools, making the benchmark an inaccurate representation of the tool's effectiveness in black-box penetration testing. We had removed some obvious symbols, renamed files containing the class of issue (including updating corresponding configuration and docker files), removed the benchmark.json and README file. However, many function names, route names, and variables remained that contained explicit references to vulnerabilities (e.g., routes like '/xss' or '/sqli').


We acknowledge that as an AI-powered SAST tool, ZeroPath could potentially have an advantage in processing these meaningful identifiers compared to traditional SAST tools, and with more time we would have removed them. However, we think this didn't significantly skew the results of our scans because:


1.


The key evidence comes from our false positive testing: If ZeroPath was heavily relying on these identifier names to make determinations, we would have seen elevated false positive rates in the patched variants - which contained the same "suspicious" route and function names but with secure implementations. The fact that our false positive rate remained low indicates ZeroPath was doing legitimate semantic analysis of the code rather than being biased by these naming hints.


2.


While we recommend future benchmark creators remove these identifiers entirely to create the most rigorous possible test environment, the correlation between names and actual vulnerabilities in real-world code is complex. Developers sometimes use security-related names precisely because they're implementing security controls, making naive reliance on such hints potentially counterproductive.


3.


The benchmark results demonstrate ZeroPath's ability to accurately distinguish between vulnerable and secure implementations of the same functionality, even when similarly named. This suggests the core analysis is based on understanding code semantics rather than surface-level naming patterns.


## Final Thoughts


Working with the XBOW benchmark brought to light some interesting challenges in evaluating security tools. Benchmarks are great, but they need to reflect the complexities of real-world applications to be truly useful. By removing the embedded hints and introducing false positives, we aimed to create a more realistic testing environment. This leveled the playing field between traditional SAST tools and AI-driven solutions like ZeroPath.


We were surprised to find hints leaking into dynamic testing, which showed us how even small details can skew results. It highlighted the importance of careful benchmark design, especially when assessing tools meant for black-box penetration testing.


In the end, this experience reinforced the idea that benchmarks need to be thoughtfully crafted to provide meaningful insights. We're excited about the progress and look forward to seeing how these tools continue to evolve in tackling the complexities of modern application security.
