---
schema_version: "1.0.0"
document_id: "e21be2e196c61a5eafea17ef88e45d7f0979ca651e634423547ea3928989aa7d"
company_key: "netskope-inc-class-a-common-stock"
company: "Netskope Inc."
source_id: "netskope-inc-class-a-common-stock-rss-c0a3e1ef9778"
canonical_url: "https://www.netskope.com/blog/npm-stealer-reads-its-c2-from-an-ethereum-contract"
published_at: "2026-08-04T18:45:16+00:00"
first_seen_at: "2026-08-10T01:01:10.452862+00:00"
fetched_at: "2026-08-10T01:01:12.343007+00:00"
content_hash: "sha256:46dffa982713382b9899e3aaea72af858fd5ddf5d97e270fcc2bb145c273060c"
---

# npm Stealer Reads Its C2 From an Ethereum Contract

Netskope Threat Labs identified and analyzed 28 malicious npm package versions published across four unrelated enterprise namespaces (` @servicetitan` ,` @or-sdk` ,` @onereach` , and` @umacloud` ) on 2026-08-04. The packages arrived in two rapid bursts: the` @or-sdk` and` @onereach` packages at 10:39 UTC, the` @servicetitan` packages two minutes later at 10:41 UTC, and` @umacloud/knowledge` nearly three hours after that at 13:18 UTC. All 28 carry an identical or functionally equivalent payload, confirmed by hash match for` @umacloud/[\[email protected\]](https://www.netskope.com/cdn-cgi/l/email-protection)` , the one tarball still live when we retrieved it. The operator behind the campaign calls it Shai-Hulud, consistent naming with similar attacks we have tracked over the past year \[[1](https://www.netskope.com/blog/shai-hulud-2-0-aggressive-automated-one-of-fastest-spreading-npm-supply-chain-attacks-ever-observed) \], \[[2](https://www.netskope.com/blog/shai-hulud-intercom-client-7-0-4) \], \[[3](https://www.netskope.com/blog/shai-hulud-style-npm-worm-hits-tanstack) \]. At install time, the packages fetch a signed Bun runtime release from GitHub, execute an obfuscated JavaScript stealer under it, and delete the runtime.


Detonating` @or-sdk/[\[email protected\]](https://www.netskope.com/cdn-cgi/l/email-protection)` in the Netskope Advanced Threat Protection sandbox produced the full chain. Measured from the start of npm install to the outbound POST, the payload read SSH private keys, AWS credentials, Docker registry authentication, Jenkins secrets, and` /etc/shadow` , requested a GitHub CLI token, enumerated AWS Systems Manager Parameter Store and Secrets Manager across 17 regions, queried the EC2 instance metadata service, attempted a HashiCorp Vault AWS login against localhost, and posted an encrypted 4.8 KB bundle to` npm-cache.com/router` .


That domain is not written into the payload. Deobfuscating the recovered stealer showed that the exfiltration host is resolved at runtime via an eth_call to an Ethereum smart contract, with three public Ethereum RPC providers tried in turn until one responds. The operator can re-point exfiltration to new infrastructure by updating contract state, without republishing a single package. The payload we recovered from` @umacloud/[\[email protected\]](https://www.netskope.com/cdn-cgi/l/email-protection)` hashes identically to the sample used against the` keyv` package family the same day,[as reported by Aikido Security](https://www.aikido.dev/blog/keyv-and-friends-compromised-in-npm-supply-chain-attack) , confirming a single campaign toolset. The four namespaces we document here are additional to that reporting.


**Key findings**


- **One payload, four unrelated namespaces, two publishing bursts.** The` @or-sdk` and` @onereach` packages (12 packages) landed at 10:39 UTC and the 15` @servicetitan` packages two minutes later, consistent with separate automated publish pipelines firing in close succession.` @umacloud/knowledge` arrived nearly three hours after. The payload SHA256 matches the` keyv` family compromise. The publishing pattern is consistent with multiple stolen organization tokens, worm propagation from a compromised maintainer machine, or theft of GitHub Actions OIDC credentials that can be exchanged for short-lived npm publish tokens scoped to a namespace, which our deobfuscation confirms is one of the payload’s named collection categories. We cannot yet distinguish among vectors.
- **The payload ships no binary and hardcodes no C2.** The install hook downloads Bun v1.3.13 from the official` oven-sh/bun` release URL, runs the payload under it, and then deletes the runtime. The exfiltration domain is not in the file either: an` eth_call` to an Ethereum smart contract returns it at runtime. Detection has to fire on behavior, because there is no malicious executable in the tarball and the executable that does run is legitimately signed.
- **The credential sweep is built to maximize yield wherever npm install runs.** Developer workstation paths (` ~/.aws` ,` ~/.ssh` ,` ~/.docker` , GitHub CLI tokens) and CI/CD-specific targets (Jenkins secret directories, EC2 IMDS, Vault AWS IAM auth, 17-region Secrets Manager enumeration) are swept in the same pass. The CI/CD paths only pay off on build agents, but the package will run on developer machines too, and the operator collects from both.


## IOCs


All publication timestamps are 2026-08-04 UTC, taken from npm registry metadata.


### Malicious package versions


**Package** **Version** **Published (UTC)**


@or-sdk/accounts 2.3.5 10:39


@or-sdk/auth 0.38.1 10:39


@or-sdk/adapters 0.3.6 10:39


@or-sdk/billing-internal 27.2.1 10:39


@or-sdk/content-request 0.2.6 10:39


@or-sdk/providers 0.3.6 10:39


@or-sdk/permissions-lambda 2.5.1 10:39


@or-sdk/druid 1.4.7 10:39


@or-sdk/base 0.44.4 10:39


@or-sdk/sdk-api 0.29.2 10:39


@onereach/types-contacts-api 9.0.8 10:39


@onereach/billing-shared 27.2.1 10:39


@servicetitan/error-boundary 38.1.1 10:41


@servicetitan/line-item-editor 1.5.1 10:41


@servicetitan/ko-bridge 38.1.1 10:41


@servicetitan/data-query 41.3.1 10:41


@servicetitan/feature-spotlight 3.9.1 10:41


@servicetitan/carto-react-kit 0.8.4 10:41


@servicetitan/react-hooks 7.7.1 10:41


@servicetitan/temporal-lite 3.4.1 10:41


@servicetitan/anvil2 3.9.1 10:41


@servicetitan/form-state 41.3.1 10:41


@servicetitan/stylelint-config 38.1.1 10:41


@servicetitan/json-render-react 0.4.6 10:41


@servicetitan/datadog-rum 38.1.1 10:41


@servicetitan/web-components 38.1.1 10:41


@servicetitan/ajax-handlers 38.1.1 10:41


@umacloud/knowledge 1.0.74 13:18


Note: burst timestamps are per-namespace group. The` @or-sdk` and` @onereach` packages published within two seconds of each other at 10:39, and the` @servicetitan` packages within one second of each other at 10:41.


### Payload


**Indicator** **Type** **Role**


9fc2570b7cef51c1b8df116d144d11ff4096357be7d2c4c6367cfc2509cf1bcc SHA256 Stealer payload (math_init.js), identical to the keyv-family sample


math_init.js File name Stealer payload, executed under the downloaded Bun runtime


Math_Symbol.js File name Alternate payload file name for the same stealer


setup.mjs File name Dropper, invoked by the preinstall hook


bun-linux-x64-baseline.zip File name Bun runtime archive fetched from GitHub on Linux hosts, deleted after unpacking. The dropper selects the equivalent darwin-arm64, darwin-x64, or win32 archive on other platforms


bun-dl-* Drop path Randomized runtime directory (/tmp/bun-dl-*/bun on Linux and macOS, temp path equivalent on Windows), binary deleted after payload execution


### Network


**Indicator** **Type** **Role**


npm-cache.com Domain Exfiltration endpoint (POST /router), registered 2026-05-22, Cloudflare-fronted. Resolved at runtime from contract state, so treat as perishable


104.21.35.216 IPv4 npm-cache.com resolution (Cloudflare)


172.67.179.231 IPv4 npm-cache.com resolution (Cloudflare)


eth.llamarpc.com Domain Ethereum RPC provider used for C2 domain resolution


go.getblock.io Domain Ethereum RPC provider used for C2 domain resolution


eth-mainnet.nodereal.io Domain Ethereum RPC provider used for C2 domain resolution


0x02d1e413 Ethereum call selector Function selector for the C2-resolving eth_call against the smart contract. Durable indicator, survives C2 domain rotation. Full call argument pending complete deobfuscation


Note on the Ethereum RPC endpoints: these are legitimate public RPC providers that the payload abuses for C2 resolution. Use them for detection and alerting, not for blind blocking.
