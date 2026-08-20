---
schema_version: "1.0.0"
document_id: "c399fb1a76b843998fdceee518d764b206c31bcc00c5ae6dcd2771ab449c63b5"
company_key: "yc-superagent"
company: "Superagent"
source_id: "yc-superagent-news-import-3c33ed06ece7"
canonical_url: "https://www.superagent.sh/blog/when-terminal-output-owns-your-clipboard-osc-52-in-warp"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-24T02:47:34.464363+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:c7d73a00ed74c4668719b0f96c304b439735d8f9e840d96ee12db888643a1c54"
---

# When Terminal Output Owns Your Clipboard: OSC 52 in Warp

Affected versions of[Warp](https://github.com/warpdotdev/warp) honor OSC 52 clipboard escape sequences emitted by terminal output, with no confirmation prompt and no setting that defaults to deny.
Because a terminal renders bytes from many sources, any process that can write to it - a remote host, a local program, content an agent fetches, or the agent driving the session - can silently read the system clipboard or overwrite it.
Tracked as[GHSA-wgqj-4c26-7c4g](https://github.com/warpdotdev/warp/security/advisories/GHSA-wgqj-4c26-7c4g) / **CVE-2026-48725** , scored **8.1 High** (CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:N), fixed in` v0.2026.05.06.15.42.stable_01` .


## Terminal output is untrusted input


A terminal does not only draw glyphs. It interprets a small in-band command language embedded in the byte stream: escape sequences that move the cursor, set colors, and manipulate the clipboard. It cannot tell a sequence a human intended from one that merely appeared in a program's output. A control byte is a control byte regardless of origin, so any output stream is an injection vector.


## OSC 52


OSC 52 is the clipboard-manipulation sequence,` ESC \] 52 ; Pc ; Pd ST` , where` Pc` selects the target (` c` for the system clipboard) and` Pd` is the payload. If` Pd` is base64 data, the terminal writes it to the clipboard. This sets the clipboard to` hello` :


```text
printf    '\033]52;c;aGVsbG8=\a'
```


If` Pd` is the single character` ?` , the terminal instead reports the current clipboard, emitting its contents back into the session:


```text
printf    '\033]52;c;?\a'
```


We call that second behavior *clipboard reflection* . It turns a one-way output stream into a two-way channel: output poses a query, and the terminal answers by feeding the clipboard back into the input the program can observe.


## The vulnerability


Affected Warp builds (` >=v0.2021.04.25.23.05.stable_00` ) honored OSC 52 for both write and read from ordinary output, with no gate defaulting to deny. The advisory classifies this as CWE-276, Incorrect Default Permissions. OSC 52 is legitimate - tmux, vim, and remote-editing workflows use it - but the capability was granted to arbitrary output by default rather than being opt-in.


The write path is an integrity problem: output can replace a copied value (a command, a URL, a wallet address) between copy and paste. The read path is the more serious, confidentiality problem, because reflection is an exfiltration primitive: a secret on the clipboard - a password, a one-time code, an API token - can be read by whatever produced the query. The only precondition is that the user views attacker-controlled output, hence` AV:N` /` UI:R` with confidentiality and integrity both rated High.


## Reproduction


The behavior is client-side and does not surface in source review or network testing alone; confirming it means running the app and watching runtime state. We reproduced it by driving the real Warp build with computer-use - installing and operating the native macOS app directly - then rendering crafted output and watching the clipboard get both read and overwritten. The same property makes this class relevant to agentic terminals: the sources that can emit output now include the model and every tool and page it relays, each inheriting the same clipboard access.


## Remediation


The fix adds an OSC 52 setting that defaults to deny; update to` v0.2026.05.06.15.42.stable_01` or later. No reliable workaround exists on an affected build. Builders of agent and terminal tooling should gate clipboard escape sequences behind explicit consent and strip OSC 52 - reflection queries in particular - from relayed output.


Two properties worth keeping in mind: terminal output is untrusted input, and the clipboard is a trust boundary.
