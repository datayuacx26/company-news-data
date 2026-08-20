---
schema_version: "1.0.0"
document_id: "2664352c6bf7d9f3480a23a59e0c8336fc80aab0d0b45d668fe743fa0c45dbbb"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild"
published_at: "2026-05-09T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:15:07.431097+00:00"
content_hash: "sha256:84a9f3f2db2045666ccdfb7c10a31393986a268b6e2e8554bb25990d4a060e04"
---

# Copy Fail and DirtyFrag: Linux Page Cache Bugs in the Wild

9 May 2026 •


[Ruben Groenewoud](https://www.elastic.co/security-labs/author/ruben-groenewoud) •


[Eric Forte](https://www.elastic.co/security-labs/author/eric-forte) •


[Samir Bousseaden](https://www.elastic.co/security-labs/author/samir-bousseaden)


# Copy Fail and DirtyFrag: Linux Page Cache Bugs in the Wild


This research analyzes the Linux kernel privilege escalation vulnerabilities Copy Fail and DirtyFrag, which exploit subtle page cache corruption bugs to create reliable paths to root access. Additionally, Elastic Security Labs is releasing detection logic for these vulnerabilities.


4 min read


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering)


##


Introduction


Recent Linux kernel privilege escalation vulnerabilities, Copy Fail (CVE-2026-31431) , Copy Fail 2, and DirtyFrag, highlight how subtle page cache corruption bugs can become practical, reliable paths to root. These issues are especially relevant for defenders because exploitation involves legitimate kernel interfaces, local execution, and short proof-of-concept code. Copy Fail has been reported as exploited in the wild and was added to CISA's Known Exploited Vulnerabilities catalog.


To help mitigate these threats, Elastic Security Labs has developed detection logic focused on the exploitation patterns around these vulnerabilities rather than only matching a specific proof-of-concept implementation.


##


Copy Fail


Copy Fail is a logic bug in the Linux kernel's` authencesn` cryptographic template. The vulnerability chains` AF_ALG` and` splice()` to create a controlled 4-byte write into the page cache of any readable file. In practice, this corrupts the in-memory view of a setuid binary like` /usr/bin/su` and escalates privileges without changing the file on disk. The public exploit is a 732-byte Python script that works across Ubuntu, Amazon Linux, RHEL, and SUSE.


##


DirtyFrag


DirtyFrag expands the same bug class into the networking stack with two page-cache write variants. The ESP path uses XFRM security associations via` AF_NETLINK` to perform in-place crypto operations on spliced pages, overwriting` /usr/bin/su` with a minimal root-shell ELF. The RxRPC fallback path uses` AF_RXRPC` with` pcbc(fcrypt)` to corrupt` /etc/passwd` , clearing root's password field. Both paths require` unshare(CLONE_NEWUSER | CLONE_NEWNET)` to gain namespace capabilities before triggering the page-cache write.


DirtyFrag does not depend on the` algif_aead` module, meaning systems that only applied the Copy Fail mitigation may still be exposed.


##


Detection


For these vulnerabilities, we focused on detecting the underlying primitives and behavior, not only a specific exploit implementation. That distinction matters, Copy Fail already has multiple public reimplementations (Python, Go, Rust, C, Metasploit), and DirtyFrag ships as a public C proof-of-concept. Trying to detect only a specific PoC leaves defenders one step behind.


###


Syscall-Level Primitives (Auditd)


Both Copy Fail and DirtyFrag rely on` socket(AF_ALG)` to access the kernel crypto subsystem, and` splice()` to inject read-only file pages into network buffers where in-place cryptographic operations corrupt the page cache. DirtyFrag additionally uses` socket(AF_RXRPC)` as a fallback when` AF_ALG` is unavailable. These primitives are visible through auditd syscall auditing` socket` with` a0` hex values of` 26` (` AF_ALG` ) or` 21` (` AF_RXRPC` ), and` splice` calls from non-root processes. We use these as early-stage signals, correlated via EQL sequences with the final privilege escalation step of gaining effective uid 0 from a non-root caller:


```text
sequence with maxspan=60s
[any where host.os.type == "linux" and
(
(event.category == "process" and auditd.data.syscall == "socket" and auditd.data.a0 in ("26", "21")) or
(event.category == "process" and auditd.data.syscall == "splice") or
(event.category == "network" and event.action == "bound-socket" and data_stream.dataset == "auditd_manager.auditd" and ?auditd.data.socket.family == "38")
)
and user.id != "0"]  by process.pid, host.id, user.id with runs=10
[process where host.os.type == "linux"  and event.action == "executed" and
(
(user.effective.id == "0" and user.id != "0") or
(process.name in ("bash", "sh", "zsh", "dash", "fish", "ksh", "busybox") and
process.args in ("-c", "--command", "-ic", "-ci", "-cl", "-lc", "-bash", "-sh", "-zsh", "-dash", "-fish", "-ksh"))
)] by process.parent.pid, host.id, user.id
```


Example of matches :


###


Namespace Creation (DirtyFrag-Specific)


DirtyFrag's exploit chain also relies on` unshare(CLONE_NEWUSER | CLONE_NEWNET)` to gain namespace capabilities. We correlate this event with a root process execution or a` setuid(0)` syscall shortly after:


```text
sequence by host.id, process.parent.pid with maxspan=30s
[process where host.os.type == "linux" and
(
(auditd.data.syscall == "unshare" and auditd.data.class == "namespace" and auditd.data.a0 in ("10000000", "50000000", "70000000", "10020000", "50020000", "70020000")) or


(process.name == "unshare" and
(process.args in ("--user", "--map-root-user", "--map-current-user") or process.args like ("-*U*", "-*r*")))
) and user.id != "0" and user.id != null]
[process where host.os.type == "linux" and
user.id == "0" and user.id != null and
(
process.name in ("su", "sudo", "pkexec", "passwd", "chsh", "newgrp", "doas", "run0", "sg", "dash", "sh", "bash", "zsh", "fish",
"ksh", "csh", "tcsh", "ash", "mksh", "busybox", "rbash", "rzsh", "rksh", "tmux", "screen", "node") or
process.name like ("python*", "perl*", "ruby*", "php*", "lua*")
)]
```


###


Generic SUID Binary Abuse (Process Exec Events)


We also assessed detection options using process exec events only, as those tend to be enabled in more environments than auditd syscall auditing. A common final step for both exploits is to corrupt or influence the in-memory execution of a SUID binary such as` su` ,` sudo` ,` pkexec` ,` passwd` ,` chsh` , or` newgrp` , causing it to run attacker-controlled code as root.


Detection looks for suspicious executions where the process runs as effective UID 0, the real user is non-root, the parent process is also non-root, the SUID binary is launched with minimal arguments, and the parent process is a scripting runtime, shell one-liner, or executable from a user-writable path:


```text
process where event.type == "start" and event.action == "exec" and (
(process.user.id == 0 and process.real_user.id != 0) or
(process.group.id == 0 and process.real_group.id != 0)
) and (
(process.name == "su" and process.args_count <= 2) or
(process.name == "sudo" and process.args_count == 1) or
(process.name == "pkexec" and process.args_count == 1) or
(process.name == "passwd" and process.args_count <= 2)
) and
(
process.parent.name like (".*", "python*", "perl*", "ruby*", "lua*", "php*", "node", "deno", "bun", "java") or
process.parent.executable like ("./*", "/tmp/*", "/var/tmp/*", "/dev/shm/*", "/run/user/*", "/var/run/user/*", "/home/*/*") or
(
process.parent.name in ("bash", "dash", "sh", "tcsh", "csh", "zsh", "ksh", "fish", "mksh") and
process.parent.args in ("-c", "-cl", "-lc", "--command", "-ic", "-ci", "-bash", "-sh", "-zsh", "-dash", "-fish", "-ksh") and
process.parent.args_count <= 4
)
)
```


Without relying on a child process being spawned, we can also hunt proactively for exploitation activity using ES|QL. Both Copy Fail and DirtyFrag produce a distinctive burst of interleaved` socket(AF_ALG)` and` splice()` syscalls from the same process. Copy Fail iterates 48 times to write 192 bytes, and DirtyFrag follows a similar pattern across its ESP and RxRPC paths.


The following query aggregates these syscalls by process and surfaces any non-root process combining` AF_ALG` or` AF_RXRPC` sockets with` splice` calls at volume :


```text
FROM logs-auditd_manager.auditd-default*
| WHERE host.os.type == "linux" AND user.id != "0" AND
(
(event.category == "process" AND auditd.data.syscall == "socket" AND auditd.data.a0 IN ("26", "21")) OR
(event.category == "process" AND auditd.data.syscall == "splice") OR
(event.category == "network" AND event.action == "bound-socket" AND auditd.data.socket.family == "38")
)
| EVAL
is_af_alg   = CASE(auditd.data.syscall == "socket" AND auditd.data.a0 == "26", 1, 0),
is_af_rxrpc = CASE(auditd.data.syscall == "socket" AND auditd.data.a0 == "21", 1, 0),
is_splice   = CASE(auditd.data.syscall == "splice", 1, 0),
is_bind_alg = CASE(event.action == "bound-socket" AND auditd.data.socket.family == "38", 1, 0)
| STATS
socket_af_alg   = SUM(is_af_alg),
socket_af_rxrpc = SUM(is_af_rxrpc),
splice_count    = SUM(is_splice),
bind_af_alg     = SUM(is_bind_alg),
total_calls     = COUNT(*),
first_seen      = MIN(@timestamp),
last_seen        = MAX(@timestamp)
BY host.name, user.name, process.executable, process.pid
| EVAL
duration_seconds = DATE_DIFF("seconds", first_seen, last_seen),
distinct_syscalls = CASE(
socket_af_alg > 0 AND splice_count > 0 AND bind_af_alg > 0, "af_alg+splice+bind",
socket_af_alg > 0 AND splice_count > 0, "af_alg+splice",
socket_af_rxrpc > 0 AND splice_count > 0, "af_rxrpc+splice",
socket_af_alg > 0, "af_alg_only",
socket_af_rxrpc > 0, "af_rxrpc_only",
splice_count > 0, "splice_only",
"other"
)
| WHERE total_calls >= 10 AND
(socket_af_alg > 0 OR socket_af_rxrpc > 0) AND
splice_count > 0
| SORT total_calls DESC
| LIMIT 50
```


###


Auditd rules:


The following rules can be added to your[Auditd](https://www.elastic.co/docs/reference/integrations/auditd_manager) integration config to enable visibility on these exploit primitives:


```text
-a always,exit -F arch=b64 -S socket -k socket_syscall
-a always,exit -F arch=b32 -S socketcall -k socket_syscall
-a always,exit -F arch=b64 -S splice -k splice-syscall
-a always,exit -F arch=b32 -S splice -k splice-syscall
-a always,exit -F arch=b64 -S bind -k socket_bound
-a always,exit -F arch=b32 -S bind -k socket_bound
```


###


Detection rules :


- [Potential Copy Fail (CVE-2026-31431) Exploitation via AF_ALG Socket](https://github.com/elastic/detection-rules/blob/ef78eb503dba59b19710cedffd3d1697185abbb4/rules/linux/privilege_escalation_potential_copy_fail_cve_2026_31431_exploitation_via_af_alg_socket.toml)
- [Suspicious SUID Binary Execution](https://github.com/elastic/detection-rules/blob/ef78eb503dba59b19710cedffd3d1697185abbb4/rules/linux/privilege_escalation_suspicious_suid_binary_execution.toml)
- [Suspicious Kernel Feature Activity rule](https://github.com/elastic/detection-rules/blob/ebe2a089b8806989e77531adde70314958851648/rules/linux/defense_evasion_sysctl_kernel_feature_activity.toml#L79)
- [Namespace Manipulation Using Unshare](https://github.com/elastic/detection-rules/blob/ef78eb503dba59b19710cedffd3d1697185abbb4/rules/linux/privilege_escalation_unshare_namespace_manipulation.toml)
- [Privilege Escalation via SUID/SGID](https://github.com/elastic/detection-rules/blob/ef78eb503dba59b19710cedffd3d1697185abbb4/rules/linux/privilege_escalation_potential_suid_sgid_exploitation.toml)


##


Mitigation


Detection should be paired with hardening and patching. The primary remediation for both vulnerabilities is to update the Linux kernel once distribution patches are available.


Where immediate patching is not possible, targeted module blocking can reduce the attack surface. For Copy Fail, disabling the` algif_aead` module prevents the AF_ALG AEAD path used by the exploit:


```text
echo "install algif_aead /bin/false" > /etc/modprobe.d/copyfail.conf
rmmod algif_aead 2>/dev/null
```


For DirtyFrag, disabling the affected networking modules blocks both the ESP and RxRPC exploit paths:


```text
printf 'install esp4 /bin/false\ninstall esp6 /bin/false\ninstall rxrpc /bin/false\n' > /etc/modprobe.d/dirtyfrag.conf
rmmod esp4 esp6 rxrpc 2>/dev/null
```


After applying either mitigation, dropping the page cache ensures any previously corrupted in-memory pages are discarded:


```text
echo 3 > /proc/sys/vm/drop_caches
```


These mitigations should be tested in a staging environment before production deployment, as disabling kernel modules may impact IPsec VPNs, crypto applications, or other services depending on the affected subsystems. Dropping the page cache causes a brief I/O spike and should be avoided during peak load.


Restricting unprivileged user namespace creation also hardens against DirtyFrag and similar exploits:


```text
sysctl -w kernel.unprivileged_userns_clone=0
```


On RHEL/Fedora, use` user.max_user_namespaces=0` instead. This setting may affect applications that rely on unprivileged namespaces such as certain container runtimes and browser sandboxes. Evaluate compatibility before applying.


##


References :


- [https://copy.fail/](https://copy.fail/)
- [https://xint.io/blog/copy-fail-linux-distributions](https://xint.io/blog/copy-fail-linux-distributions)
- [https://github.com/V4bel/dirtyfrag/tree/master](https://github.com/V4bel/dirtyfrag/tree/master)
- [https://github.com/0xdeadbeefnetwork/Copy_Fail2-Electric_Boogaloo/](https://github.com/0xdeadbeefnetwork/Copy_Fail2-Electric_Boogaloo/)
- [https://access.redhat.com/security/vulnerabilities/RHSB-2026-003](https://access.redhat.com/security/vulnerabilities/RHSB-2026-003)
- [https://ubuntu.com/blog/copy-fail-vulnerability-fixes-available](https://ubuntu.com/blog/copy-fail-vulnerability-fixes-available)
- [https://aws.amazon.com/security/security-bulletins/rss/2026-027-aws/](https://aws.amazon.com/security/security-bulletins/rss/2026-027-aws/)
- [https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=a664bf3d603d](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=a664bf3d603d)


#### Jump to section


- [Introduction](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#introduction)
- [Copy Fail](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#copy-fail)
- [DirtyFrag](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#dirtyfrag)
- [Detection](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#detection)
- [Syscall-Level Primitives (Auditd)](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#syscall-level-primitives-auditd)
- [Namespace Creation (DirtyFrag-Specific)](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#namespace-creation-dirtyfrag-specific)
- [Generic SUID Binary Abuse (Process Exec Events)](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#generic-suid-binary-abuse-process-exec-events)
- [Auditd rules:](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#auditd-rules)
- [Detection rules :](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#detection-rules--)
- [Mitigation](https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild#mitigation)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Copy%20Fail%20and%20DirtyFrag:%20Linux%20Page%20Cache%20Bugs%20in%20the%20Wild&url=https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild&title=Copy%20Fail%20and%20DirtyFrag:%20Linux%20Page%20Cache%20Bugs%20in%20the%20Wild)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/copy-fail-dirtyfrag-linux-page-bugs-in-the-wild&title=Copy%20Fail%20and%20DirtyFrag:%20Linux%20Page%20Cache%20Bugs%20in%20the%20Wild)
