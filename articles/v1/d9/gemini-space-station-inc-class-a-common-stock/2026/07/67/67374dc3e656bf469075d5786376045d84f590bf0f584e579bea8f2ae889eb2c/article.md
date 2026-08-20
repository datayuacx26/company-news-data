---
schema_version: "1.0.0"
document_id: "67374dc3e656bf469075d5786376045d84f590bf0f584e579bea8f2ae889eb2c"
company_key: "gemini-space-station-inc-class-a-common-stock"
company: "Gemini Space Station Inc."
source_id: "gemini-space-station-inc-class-a-common-stock-news-import-1c1a410de1c5"
canonical_url: "https://www.gemini.com/blog/how-gemini-built-a-malicious-code-detector-to-boost-our-security-apparatus"
published_at: null
first_seen_at: "2026-07-21T21:23:13.509460+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:3a76a4e615ab0dc3a4296bde72d3f5a79393cdf0b179e175e081626f24a527a8"
---

# How Gemini Built A Malicious Code Detector To Boost Our Security Apparatus

A recent incident in the tech space last month highlighted the importance of reviewing new code packages when hackers snuck malicious code into popular developer tools used by millions of programmers. Through a chain of connected tools, they ended up stealing credentials from about 6,000 developer computers. This included passwords, cloud keys, and SSH (Secure shell) keys.


One of those computers belonged to a GitHub employee, which led to 3,800 of GitHub's internal code repositories being stolen. The attack vector originated through a VS Code extension, a coding tool installed by developers. It was ultimately traced to one misconfigured security setting in a third-party package.


At Gemini, we tirelessly review new code packages to avoid this exact situation. The process requires a team of multiple security engineers, but to assist that process, our engineers used Claude to build an MCD audit tool that detects malicious code from third-party vendors.


### The Tool We Built: MCD-Audit


The MCD audit tool outputs a score between one to 10, with higher scores prompting engineers to prioritize the issue immediately. This helps us work faster and more efficiently, freeing the team from going line by line through each section of code and interpreting results. However, final sign off still comes from humans, allowing us to blend a best-in-class security approach with AI.


As a bonus, Claude’s reasoning abilities help review software packages the way a senior security engineer would, with actual understanding rather than just keyword matching.


It works by:


- **Checking the package's background:** Who published it? Is the name suspiciously close to a popular package? Does it even have a public repo?


- **Downloading without running it:** Malicious packages often trigger when installed, so we inspect the files without ever running them.


- **Reading the actual code:** It looks at what the code does, not just what words appear in it. Even binary/compiled files get inspected for clues.


- **Scoring suspicious behaviors in combination:** For example, a package that downloads something from an unknown website and immediately runs it as code scores very high on the danger scale.
- **Checking all dependencies:** Not just the package itself, but everything it depends on, recursively.


- **Inspecting in an isolated environment:** So if something is malicious, it can't harm our systems during inspection.


### What We Found


When we scanned 13,000 brand-new npm packages published over two weeks on the public internet, we found five actively malicious packages, including:


- One that took over any machine that installed it, letting hackers run any command they wanted.
- One disguised as a Claude tool that secretly stole Anthropic API credentials and sent your entire conversation history to an outside server.
- One hiding its malicious behavior behind encoded text (a simple keyword scanner would have missed it entirely).
- One that does nothing until January 31, 2027, specifically to pass security reviews while it quietly spreads.
- One containing a hidden crypto-wallet theft toolkit buried inside a compiled binary file, only detectable by digging into the file's internal symbol names.


### The Bottom Line


Malicious packages are being published constantly. This isn't a rare event. One bad package in your toolchain can cascade into a serious breach. MCD-Audit is an additional step in our multilayered security defense system. It reviews every package intelligently, before it ever touches our systems, setting up Gemini’s team of security engineers for success.
