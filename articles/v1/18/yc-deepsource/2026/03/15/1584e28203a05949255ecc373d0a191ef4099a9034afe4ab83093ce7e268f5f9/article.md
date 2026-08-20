---
schema_version: "1.0.0"
document_id: "1584e28203a05949255ecc373d0a191ef4099a9034afe4ab83093ce7e268f5f9"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/litellm-supply-chain-attack"
published_at: "2026-03-25T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:1aca01fd71f25bf16f4020e6afbacd4db24451a182a7f3669140536f7da95243"
---

# Your AI toolchain is now an attack surface

Last updated on Mar 25, 2026


On March 24, 2026, two poisoned versions of LiteLLM were published to PyPI. Within hours, every machine that pulled version 1.82.7 or 1.82.8 had its SSH keys, cloud credentials, API tokens, Kubernetes secrets, and environment variables encrypted and exfiltrated to an attacker-controlled server. The malware didn't stop at theft. It read every Kubernetes secret in the cluster, deployed privileged pods to every node, and installed a persistent backdoor that phones home every 50 minutes for new instructions.


The developer who caught it didn't even know LiteLLM was in their environment. A Cursor MCP plugin had pulled it in as a transitive dependency.


That last part is the one that keeps nagging at me. Not the sophistication of the attack. Not the credential theft. The fact that a developer got owned by a package they never chose to install.


## What happened


The attack is the work of TeamPCP, a threat actor running a cascading supply chain campaign through March 2026. The chain:


- **March 19** — TeamPCP compromises Trivy (Aqua Security's vulnerability scanner) via a misconfiguration in its GitHub Actions. Credential-stealing payload injected into CI/CD pipelines.
- **March 23** — Stolen credentials from Trivy used to compromise Checkmarx's KICS GitHub Actions.
- **March 24** — LiteLLM's CI/CD pipeline runs Trivy. Attackers harvest LiteLLM's PyPI publishing token. Two backdoored versions pushed to PyPI.


A vulnerability scanner got compromised. Credentials stolen from it were used to backdoor the package that holds every LLM API key in your org. Security tools became the attack vector for compromising AI tools. You can't make this up.


The technical execution got more aggressive between versions. Version 1.82.7 hid malicious code inside` proxy_server.py` , so it only ran when the module was imported. Version 1.82.8 added a` .pth` file to the package. Python's` site` module executes` .pth` files automatically on *every interpreter startup* . You didn't need to` import litellm` . You didn't need to use it. If the package was installed in your environment, the malware ran every time Python started.


The payload itself was a three-stage operation.


**Harvest.** It grabbed everything on the filesystem that looked like a secret: SSH keys,` .env` files, AWS/GCP/Azure credentials, Kubernetes configs, Docker configs, Git credentials, database passwords, shell history, crypto wallets, CI/CD config files, Slack and Discord webhook URLs.


**Encrypt and exfiltrate.** The haul was encrypted with AES-256-CBC (random session key, encrypted with a hardcoded 4096-bit RSA public key) and POSTed to` models.litellm.cloud` , a typosquat of the legitimate` litellm.ai` domain.


**Lateral movement.** If a Kubernetes service account token was present, the malware read all cluster secrets across all namespaces, then tried to deploy a privileged Alpine pod to every node in` kube-system` , mounting the host filesystem and installing a persistent systemd backdoor.


PyPI quarantined the package within roughly three hours. But LiteLLM processes around 3.4 million downloads per day. Three hours was plenty.


## Why this one is different


Most supply chain attacks are opportunistic. Typosquat a popular package, hope someone fat-fingers a` pip install` , grab whatever credentials are lying around. This one was targeted.


LiteLLM is an API key management gateway. It routes requests to OpenAI, Anthropic, Google, Cohere, and dozens of other LLM providers. By design, it holds every API key for every model your organization uses. Compromising LiteLLM is like compromising a password manager.


TeamPCP clearly understood this. Look at their target selection across the campaign: a container vulnerability scanner (Trivy), an infrastructure-as-code security tool (KICS), an LLM API proxy that holds every AI credential in the org (LiteLLM). Each one runs with elevated privileges in automated environments. Each compromise yields credentials that unlock the next target.


But the thing I keep coming back to is how LiteLLM ended up in people's environments in the first place.


Callum McMahon at FutureSearch, the developer who discovered the attack, wasn't using LiteLLM directly. An MCP plugin running inside Cursor pulled it in as a transitive dependency. This is how AI tooling works now: packages flow into your environment through agent frameworks, IDE plugins, MCP servers, and orchestration layers that you didn't explicitly choose to install.


The surface area is bigger than most teams realize. If your developers use Cursor, Claude Code, or Windsurf, those tools have MCP servers and plugins that pull in their own dependency trees. LiteLLM alone has 40+ direct dependencies, each with its own deps. Your CI/CD pipelines run security scanners, linters, test frameworks, all of which can be compromised (as Trivy just demonstrated). And your production services might use LiteLLM or similar proxies to route LLM requests, giving them access to API keys, database credentials, and cloud provider tokens.


The AI toolchain handles credentials with the same sensitivity as your secrets manager. Almost nobody treats it that way.


## What .pth files tell us about the limits of code review


Python's` .pth` files are designed for path configuration. They tell the interpreter where to find packages. But any` .pth` file that starts with` import` executes arbitrary code on every interpreter startup. This is documented behavior. It's a feature, not a bug.


The attacker used a` .pth` file because it's invisible to the most common forms of code review. It doesn't show up in` import` statements. Static analysis of` proxy_server.py` wouldn't catch it. Grepping for` subprocess` in the source tree wouldn't find it. The payload was double base64-encoded, making it opaque to pattern matching.


Here's the part that should bother you:` pip install --require-hashes` would have *passed* . The` .pth` file was properly declared in the wheel's RECORD with a matching SHA-256 hash. The package passed every standard integrity check because it was published using legitimate credentials. No hash mismatch. No misspelled package name. No suspicious domain. The trust chain was intact. The trust was just misplaced.


Source-level code review couldn't have caught this. The malicious code was injected into the packaging pipeline, not the source repository. There was no corresponding git commit, no pull request, no diff to review. The compromise happened between the source code and the distributed artifact.


## What to do about it


**If you're running 1.82.7 or 1.82.8, act now.** Remove the package, check for persistence mechanisms (` ~/.config/sysmon/sysmon.py` , unauthorized Kubernetes pods in` kube-system` ), and rotate every credential that was accessible on that machine. Don't try to scope the rotation. The harvester collected everything.


For the longer term, there are a few things that actually move the needle.


**Know your transitive dependencies.** Most teams can name their direct dependencies. Few can name what those pull in. If your dependency graph grew overnight and you didn't make a change, that's worth investigating. SCA tooling helps here.


**Pin to hashes, but understand what that buys you.** Hash pinning protects against tampering. It does not protect against stolen credentials being used to publish a properly signed malicious release. You still need to review new versions before upgrading. Don't auto-update AI toolchain packages in production or CI/CD without looking at them.


**Treat your AI infrastructure like auth infrastructure.** LLM proxy libraries and agent frameworks have access to credentials at the same level as your secrets manager or OAuth provider. That means SCA scanning, secrets detection in environment variables, and monitoring for unusual outbound network connections from CI/CD runners.


**Monitor what's installed, not just what's committed.** This attack was invisible at the source code level. The backdoor was in the distributed artifact, not the git repository. Security tooling that only operates on source code would have missed it entirely.


**Audit your CI/CD trust chain.** LiteLLM was compromised because it used a compromised security scanner in its build pipeline. How many GitHub Actions, security tools, and CI plugins does your pipeline depend on? Are they pinned to commit SHAs or floating tags? Tags can be force-pushed to point at malicious commits. Commit SHAs can't.


**Watch for` .pth` files.** Legitimate packages rarely need them. A post-install check for` .pth` files containing` import` ,` exec` ,` subprocess` , or` base64` in` site-packages` is a cheap heuristic with very few false positives.


## This campaign isn't over


TeamPCP said on their Telegram channel that they're partnering with other groups and plan to target more security tools and open-source projects. Each compromised environment yields credentials that unlock the next target. Wiz's security team put it well: Trivy gets compromised, LiteLLM gets compromised, credentials from tens of thousands of environments are exposed. It's a snowball.


The specific lesson here is about AI tooling. The general lesson is older: your software supply chain is only as strong as the weakest dependency in it. And the weakest dependency is often something you trust implicitly because it's supposed to be making you more secure.


The 40,000-star package that routes all your LLM API keys was three hours away from draining your cloud credentials into a` .tar.gz` and shipping them to an attacker's server. The security scanner that was supposed to protect your pipeline was the entry point. That's the world we're building in now, and it would be a good idea to start acting like it.
