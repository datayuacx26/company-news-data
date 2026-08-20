---
schema_version: "1.0.0"
document_id: "01fb5f925e12170e7e1a891927242e60165af12feb6ca2c0d0115ea9526b4be4"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/sudo-bug-fixes"
published_at: "2026-03-18T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T22:18:37.089432+00:00"
content_hash: "sha256:b1fa9d02307a9f107e2141675c4a49fee559465a56f90c2ae15127d1008f7535"
---

# ZeroPath's 36 Sudo Bug Fixes Reduce CrackArmor's Impact

## CrackArmor Includes Rediscovered ZeroPath Finding


We were excited to see ZeroPath name-checked in Qualys’ excellent[write up](https://cdn2.qualys.com/advisory/2026/03/10/crack-armor.txt) of their CrackArmor vulnerability, which allows regular users to write AppArmor profiles and then abuse them to gain more privilege in various ways. One of the exploit chains they built on top of this core AppArmor flaw included a rediscovered sudo issue originally reported by ZeroPath in late 2025:


This particular fix is one of 36[Joshua Rogers](https://joshua.hu/) found and reported using ZeroPath in late 2025 ([full list](https://github.com/search?q=repo%3Asudo-project%2Fsudo+zeropath&type=commits) ). It’s the first to be used in a real attack chain, but we suspect it won’t be the last.


Prior to our patch, sudo tried to drop privilege when invoking a mailer to send out a notification, but it didn’t verify that the drop succeeded. As a result, if you used a malicious AppArmor profile to block the setuid() syscall, sudo would execute the command as root, allowing for code execution as root if the particular mailer used had other flaws.


Since one of our sudo discoveries came up in the context of CrackArmor, it seemed like a good time to share the rest (including a previously-unpublished RCE with working POC) in case they’re useful to anyone else out there. While sudo has patched these issues, the patched version of sudo hasn’t worked its way into all linux distros yet – it may be worth verifying that your systems are not vulnerable.


## Selected Findings


### CrackArmor-Related Privilege Drop Issue


The sudo flaw rediscovered by Qualys was fixed in this commit:


[3e474c2f201484be83d994ae10a4e20e8c81bb69](https://github.com/sudo-project/sudo/commit/3e474c2f201484be83d994ae10a4e20e8c81bb69) (2025-11-08)


The part relevant to CrackArmor is pretty straightforward… when invoking a mailer to send out notifications,` exec_mailer()` tried to drop to the mail user's UID, but didn't verify the drop succeeded and didn't drop group privileges at all:


```text
lib  /  eventlog  /  eventlog  .  c  :  331


if     (  setuid  (  evl_conf  ->  mailuid  )     !=     0  )     {
sudo_debug_printf  (  SUDO_DEBUG_ERROR  ,     "unable to change uid to %u"  ,
(  unsigned     int  )  evl_conf  ->  mailuid  )  ;
}
```


If` setuid()` failed for any reason (for example, because an attacker controlling AppArmor profiles blocked the syscall) sudo silently continued and invoked the mailer with its full root privileges. Even when the call succeeded, the mailer still ran in the root group because group privileges were never dropped.


The fix addressed all of these issues. It re-asserts root, drops group privileges, then drops to the mail user's UID, and any failure aborts via` goto bad` instead of being silently ignored:


```text
lib  /  eventlog  /  eventlog  .  c  :  324


if     (  setuid  (  ROOT_UID  )     !=     0  )     {
sudo_debug_printf  (  SUDO_DEBUG_ERROR  ,     "unable to change uid to %u"  ,
ROOT_UID  )  ;
goto   bad  ;
}
if     (  setgid  (  evl_conf  ->  mailgid  )     !=     0  )     {
sudo_debug_printf  (  SUDO_DEBUG_ERROR  ,     "unable to change gid to %u"  ,
(  unsigned     int  )  evl_conf  ->  mailgid  )  ;
goto   bad  ;
}
if     (  setgroups  (  1  ,     &  evl_conf  ->  mailgid  )     !=     0  )     {
sudo_debug_printf  (  SUDO_DEBUG_ERROR  ,     "unable to set groups to %u"  ,
(  unsigned     int  )  evl_conf  ->  mailgid  )  ;
goto   bad  ;
}
if     (  setuid  (  evl_conf  ->  mailuid  )     !=     0  )     {
sudo_debug_printf  (  SUDO_DEBUG_ERROR  ,     "unable to change uid to %u"  ,
(  unsigned     int  )  evl_conf  ->  mailuid  )  ;
goto   bad  ;
}
```


### Remote Code Execution Via Sudo Log Server


#### The Issue


Relevant fix commits:


- [186f94507f1222f8b21349f4c91c5d5e810fa872](https://github.com/sudo-project/sudo/commit/186f94507f1222f8b21349f4c91c5d5e810fa872) (2025-11-11)
- [4bd549d7f41c7d3a0365f35b43f8bbf40a3a2821](https://github.com/sudo-project/sudo/commit/4bd549d7f41c7d3a0365f35b43f8bbf40a3a2821) (2025-11-11)


Sudo ships with an optional log server component, logsrvd. If enabled, it listens on port 30344 or 30343 and allows for centralized aggregation of sudo logs. While it is not enabled in any mainstream linux distribution by default, it typically can be installed as an optional package.


Critically, if users use the default config, no authentication is required to connect to the service.


Within the config for this service, the user specifies a directory to store the logs in using the iolog_dir and iolog_file configuration values. By default, they’re set to:


```text
iolog_dir = /var/log/sudo-io
iolog_file = %{seq}


```


%{seq} here is an example of a special escape sequence users can include in the log path or filename. They can include things like %{hostname}, %{username} which are supplied to the logging daemon by the client.


If the user includes any of the escape sequences that are client-controlled in their configured log path, the logging service, which runs as root, becomes vulnerable to unauthenticated code execution.


The core issue is that the daemon does not sanitize input before inserting it into the path. If the user has configured an iolog_file pattern of %{user}, for example, and a malicious user submits an event with user “.../../../etc/cron.hourly/foo”, the logger will emit its message to a path that evaluates to “/etc/cron.hourly/foo” – meaning that the contents of that file will be executed by cron hourly.


Since the user controls many elements of the log message, they can inject newlines into values to ensure that at least one valid cron configuration line gets written to the target file.


```text
logsrvd  /  iolog_writer  .  c  :  131


struct     eventlog     *
evlog_new  (  const   TimeSpec   *  submit_time  ,   InfoMessage   *     const     *  info_msgs  ,
size_t   infolen  ,     struct     connection_closure     *  closure  )
{
// ...


// ZP !!!: Several fields provided by client added to eventlog struct
if     (  strcmp  (  key  ,     "submituser"  )     ==     0  )     {
if     (  type_matches  (  info  ,   source  ,   INFO_MESSAGE__VALUE_STRVAL  )  )     {
free  (  evlog  ->  submituser  )  ;


if     (  (  evlog  ->  submituser   =     strdup  (  info  ->  u  .  strval  )  )     ==     NULL  )     {


```


*Client-supplied input added to event*


```text
logsrvd  /  iolog_writer  .  c  :  573


static   bool
create_iolog_path  (  struct     connection_closure     *  closure  )
{
// ...


// ZP !!!: Two calls to construct where the log file will be written


if     (  !  expand_iolog_path  (  logsrvd_conf_iolog_dir  (  )  ,   expanded_dir  ,
sizeof  (  expanded_dir  )  ,     &  path_escapes  [  1  ]  ,     &  path_closure  )  )     {


// ...


if     (  !  expand_iolog_path  (  logsrvd_conf_iolog_file  (  )  ,   expanded_file  ,
sizeof  (  expanded_file  )  ,     &  path_escapes  [  0  ]  ,     &  path_closure  )  )     {
```


*Determine effective log path, expanding escape sequences*


```text
lib  /  iolog  /  iolog_path  .  c  :  43


bool
expand_iolog_path  (  const     char     *  inpath  ,     char     *  path  ,     size_t   pathlen  ,
const     struct     iolog_path_escape     *  escapes  ,     void     *  closure  )
{
// ...


// ZP !!!: Look for any %{ } escape sequences in path
// ZP !!!: replace them with user input
for     (  src   =   inpath  ,   dst   =   path  ;     *  src   !=     '\0'  ;   src  ++  )     {
if     (  src  [  0  ]     ==     '%'  )     {
if     (  src  [  1  ]     ==     '{'  )     {
endbrace   =     strchr  (  src   +     2  ,     '}'  )  ;
if     (  endbrace   !=     NULL  )     {
const     struct     iolog_path_escape     *  esc  ;
// ...


// ZP !!!: This FP gets relevant field from eventlog struct
len   =   esc  ->  copy_fn  (  dst  ,     (  size_t  )  (  pathend   -   dst  )  ,   closure  )  ;
```


*Expand escapes in the log path by replacing them with user input*


#### POC


Working python proof of concept if you’d like to check your own infrastructure. To be vulnerable, a system must:


- Have sudo’s logsrvd configured
- Have authentication disabled (default)
- Have a log directory configured that includes the` %{user}` escape in it.


[https://github.com/ZeroPathAI/public-pocs/blob/main/sudo/logsrvd_rce_poc.py](https://github.com/ZeroPathAI/public-pocs/blob/main/sudo/logsrvd_rce_poc.py)


### Use After Free


Relevant fix commit:


[1f3dbcda62b4cf94a91fffab089c2feaeab36932](https://github.com/sudo-project/sudo/commit/1f3dbcda62b4cf94a91fffab089c2feaeab36932) (2025-10-16)


When a command is approved by policy and about to execute, sudo uses log_server_accept() to record that fact. log_server_accept() initializes a local variable audit_details to store the audit information in, but passes that local variable to log_server_open(), which stores a pointer to the local variable in a long-lived, heap-allocated data structure.


As soon as log_server_accept() returns, audit_details is deallocated and the log_details field of the closure is left pointing to a random part of the stack.


```text
plugins  /  sudoers  /  audit  .  c  :  244


static   bool
log_server_accept  (  const     struct     sudoers_context     *  ctx  ,     struct     eventlog     *  evlog  )
{
// ...
struct     log_details   audit_details  ;


// ...


client_closure   =     log_server_open  (  &  audit_details  ,     .  .  .  )  ;


```


*Initial allocation of local audit_details variable*


```text
plugins  /  sudoers  /  log_client  .  c  :  2100


struct     client_closure     *
log_server_open  (  struct     log_details     *  details  ,     struct     timespec     *  start_time  ,
bool log_io  ,     enum     client_state   initial_state  ,     const     char     *  reason  )
{
struct     client_closure     *  closure  ;


// ...


// ZP !!!: Allocate a long-lived data structure, which will
// reference details
closure   =     client_closure_alloc  (  details  ,   start_time  ,   log_io  ,   initial_state  ,  reason  )  ;
```


*log_server_open() uses client_closure_alloc() to embed the local details variable in a closure.*


```text
plugins  /  sudoers  /  log_client  .  c  :  2049


static     struct     client_closure     *
client_closure_alloc  (  struct     log_details     *  details  ,     struct     timespec     *  start_time  ,
{
struct     client_closure     *  closure  ;
// ...


if     (  (  closure   =     calloc  (  1  ,     sizeof  (  *  closure  )  )  )     ==     NULL  )


// ...


// ZP !!!: details is local audit_details var from log_server_accept
closure  ->  log_details   =   details  ;


// ...
```


*client_closure_alloc sets long-lived pointer to local stack-allocated variable*


## Conclusion


While some of the individual problems ZeroPath found were small, the 36 fixes as a whole addressed real issues used in real attacks (e.g. CrackArmor) and significantly improved sudo’s security posture. They speak to our commitment to quietly make the open source projects we rely on better.


Thanks to[Joshua Rogers](https://joshua.hu/) for his work using ZeroPath to find these issues and get them fixed!


### Appendix: Full Bug List


#### 2025-10-16: Dangling pointer in audit details


Commit[1f3dbcda6](https://github.com/sudo-project/sudo/commit/1f3dbcda62b4cf94a91fffab089c2feaeab36932)


**Subject** log_server_accept: audit_details cannot be a local variable.


**Vulnerable Since** unreleased (post-1.9.17p2)


**Potential Security Impact** Use-After-Free, RCE, Information Disclosure


**Security Issue** Use-after-free / dangling pointer vulnerability. The audit_details struct was declared as a local variable in log_server_accept(), but a pointer to it was stored in the client_closure which persists beyond the function's scope. After the function returned, the closure held a dangling pointer to stack memory that had been reclaimed, leading to undefined behavior when the closure later accessed audit_details (e.g., corrupted log data, potential code execution). Additionally, the log_servers string list within audit_details was freed immediately after the closure was created, while the closure still referenced it. The fix makes audit_details a static global variable so it remains valid for the lifetime of the closure, and defers freeing until log_server_exit().


#### 2025-10-25: FD leak on connection failure


Commit[942efe61a](https://github.com/sudo-project/sudo/commit/942efe61af44f07c96272b8ca7b3b5ab2c187401)


**Subject** new_connection: Fix fd leak if connection_closure_alloc() fails


**Vulnerable Since** 1.9.7


**Potential Security Impact** DoS, FD Exhaustion


**Security Issue** File descriptor leak in sudo_logsrvd. When new_connection() accepted a socket but connection_closure_alloc() subsequently failed, the accepted socket file descriptor was never closed. In a long-running log server daemon, repeated allocation failures (e.g., under memory pressure or during a denial-of-service attack) would leak file descriptors, eventually exhausting the process's fd limit and preventing the server from accepting any new connections, resulting in a denial of service.


#### 2025-10-25: Passwords logged in plaintext by default


Commit[6a1fe4248](https://github.com/sudo-project/sudo/commit/6a1fe4248316885f28b83dac88c5a4346f99721b)


**Subject** Disable log_passwords by default in sudoers and sudo_logsrvd.conf.


**Vulnerable Since** 1.9.10


**Potential Security Impact** Credential Exposure


**Security Issue** Plaintext password exposure in I/O logs. The log_passwords option was enabled (true) by default in both sudoers and sudo_logsrvd.conf. This meant that when I/O logging of terminal input was active, user passwords typed at authentication prompts were recorded in plaintext in the I/O logs. These logs could be read by administrators or anyone with access to the log files/server, exposing sensitive credentials. The fix changes the default to false, so that by default the system uses passprompt_regex to detect and redact passwords from I/O log recordings.


#### 2025-10-25: Memory leak in network interface enumeration


Commit[5846cdeda](https://github.com/sudo-project/sudo/commit/5846cdeda7d53c371971d940f00f4ce8f97ab45b)


**Subject** get_net_ifs: fix memory leak in SIOCGIFCONF version


**Vulnerable Since** 1.9.7


**Potential Security Impact** Memory Leak, DoS


**Security Issue** Memory leak in network interface enumeration. In the SIOCGIFCONF code path of get_net_ifs(), a separate ifconf_buf variable was used to track the allocated buffer, but on the cleanup path, free(ifconf_buf) was called instead of free(ifconf.ifc_buf). If ifconf.ifc_buf was reallocated (the code doubles the buffer size in a loop), the ifconf_buf pointer became stale and the actual buffer was leaked. Since get_net_ifs() is called during sudo startup to gather network interface information for sudoers matching, this constitutes a memory leak on every sudo invocation. In a daemon context (sudo_logsrvd), repeated calls could lead to significant memory consumption.


#### 2025-10-25: NULL deref in TLS relay timeout


Commit[930a087bf](https://github.com/sudo-project/sudo/commit/930a087bf939636c66ddab935b3e065aeb225623)


**Subject** connect_relay_tls: Fix NULL deref when relay connect_timeout is 0.


**Vulnerable Since** 1.9.7


**Potential Security Impact** DoS, NULL Pointer Dereference


**Security Issue** NULL pointer dereference crash in sudo_logsrvd TLS relay connection. When the relay connect_timeout configuration was set to 0 (or unset), logsrvd_conf_relay_connect_timeout() returned NULL. The connect_relay_tls() function unconditionally dereferenced this return value, causing a NULL pointer dereference and crashing the log server daemon. An attacker who could influence the server configuration or trigger this code path could cause a denial of service. The fix adds a NULL check and clears the timeout struct when no timeout is configured.


#### 2025-10-30: Error handler corrupted on config failure


Commit[d61959651](https://github.com/sudo-project/sudo/commit/d6195965193fba06b3162d09e07f4cf767a254de)


**Subject** logsrvd_conf_apply: Open server log after opening the event log


**Vulnerable Since** 1.9.8


**Potential Security Impact** Error Handling Bypass, Audit Evasion


**Security Issue** Error handling bypass due to premature conversation function override. In logsrvd_conf_apply(), the server log was opened before the event log. Opening the server log calls sudo_warn_set_conversation(), which changes the global warning/error output handler. If the subsequent event log open failed and the function returned false, the conversation function had already been permanently changed, corrupting the error reporting path for the caller. This could cause error messages to be lost, misdirected, or trigger unexpected behavior during error recovery, potentially masking security-relevant failures. The fix reorders operations so the event log (which can fail gracefully) is opened first, and sudo_warn_set_conversation() is only called at the point of no return.


#### 2025-10-30: Silent fallback to unencrypted LDAP


Commit[db82b90ae](https://github.com/sudo-project/sudo/commit/db82b90aeb0777b6ab611b1a5053fc1c7cd6ae6a)


**Subject** sudo_ldap_open: Error out if start_tls specified but not supported


**Vulnerable Since** 1.7.0


**Potential Security Impact** TLS Downgrade, Credential Exposure


**Security Issue** Silent TLS downgrade / insecure LDAP connection. When ldap.conf specified 'SSL start_tls' but the compiled LDAP libraries did not support ldap_start_tls_s() or ldap_start_tls_s_np(), the code printed a warning but continued to proceed with an unencrypted LDAP connection. This meant sudoers rules, user credentials, and other sensitive data were transmitted in plaintext over the network despite the administrator explicitly requesting TLS encryption. The fix treats this as a fatal error, refusing to connect without the requested TLS protection.


#### 2025-10-30: NULL reject reason in intercept mode


Commit[7eaa52694](https://github.com/sudo-project/sudo/commit/7eaa5269478d428e7abe5174f3084182d5923564)


**Subject** Pass explicit reason to fmt_alert_message() and fmt_reject_message()


**Vulnerable Since** 1.9.4


**Potential Security Impact** DoS, Audit Evasion


**Security Issue** NULL pointer passed as reject/alert reason in intercept mode, leading to protocol violation or crash. When a sub-command was rejected by policy during intercept mode, fmt_reject_message() and fmt_alert_message() read the reason from closure->reason, which is NULL in the intercept sub-command context. This caused a NULL reason to be sent in the RejectMessage/AlertMessage to sudo_logsrvd, which recent server changes require to be non-NULL. This could crash the log server or cause the rejection/alert to be silently dropped, meaning unauthorized command execution attempts would not be properly logged or reported. The fix passes the reason string explicitly from the caller rather than relying on the closure field.


#### 2025-10-30: Uninitialized socket FD on error path


Commit[25d1f08ff](https://github.com/sudo-project/sudo/commit/25d1f08fffa7d763b88ff79bc45bd6c618385d6e)


**Subject** prepare_listener: initialize sock to -1


**Vulnerable Since** 1.9.8


**Potential Security Impact** Arbitrary FD Close, DoS


**Security Issue** Uninitialized file descriptor variable leading to closing an arbitrary fd. In prepare_listener(), the sock variable was uninitialized. If an error occurred before sock was assigned a valid socket fd (e.g., during random token generation or socket creation), the error cleanup path would call close(sock) on whatever garbage value happened to be on the stack. This could close an unrelated, legitimate file descriptor belonging to the sudo process, potentially disrupting I/O logging, PTY handling, or other security-critical operations. The fix initializes sock to -1 so the cleanup path correctly skips the close() call.


#### 2025-11-06: Plaintext listener enabled with TLS builds


Commit[f764980de](https://github.com/sudo-project/sudo/commit/f764980def4f78ef74bc97718850da770cd6c2b4)


**Subject** Only enable plaintext listened by default if not built with TLS support.


**Vulnerable Since** 1.9.0


**Potential Security Impact** TLS Downgrade, Information Disclosure


**Security Issue** sudo_logsrvd would accept plaintext (unencrypted) connections by default even when built with TLS support. When no 'listen_address' was configured, both a plaintext listener on port 30343 and a TLS listener on port 30344 were enabled. This meant that sensitive log data (including command output and credentials) could be transmitted in cleartext over the network if a client connected to the plaintext port, even on deployments where TLS was intended. The fix makes the plaintext listener only enabled by default when TLS support is not compiled in; if TLS is available, only the TLS listener is enabled by default.


#### 2025-11-08: Out-of-bounds read in utmp handling


Commit[820d0ca5e](https://github.com/sudo-project/sudo/commit/820d0ca5edd5dbdb659f4947068bdd95414a0a8f)


**Subject** utmp_setid: Make sure we don't read past the end of ut_line


**Vulnerable Since** 1.8.1


**Potential Security Impact** Out-of-Bounds Read, Information Disclosure


**Security Issue** Out-of-bounds read in utmp_setid(). The ut_line field in the utmp structure is a fixed-size character array that is not guaranteed to be NUL-terminated. The code used strlen() on ut_line, which would read past the end of the buffer into adjacent memory until a NUL byte was found. This could leak sensitive memory contents or cause a crash. The fix replaces strlen() with strnlen() bounded by sizeof(ut_line) to prevent reading beyond the buffer.


#### 2025-11-08: Incomplete privilege drop in mailer


Commit[3e474c2f2](https://github.com/sudo-project/sudo/commit/3e474c2f201484be83d994ae10a4e20e8c81bb69)


**Subject** exec_mailer: Set group as well as uid when running the mailer


**Vulnerable Since** 1.9.4


**Potential Security Impact** Privilege Escalation, Incomplete Privilege Drop


**Security Issue** Incomplete privilege dropping when executing the mailer process. exec_mailer() called setuid() to drop to the mail user but did not call setgid() or setgroups(), leaving the mailer process running with root's group ID and supplementary groups. This violates the principle of least privilege: the mailer (and any child processes it spawns) retained root group membership, potentially allowing access to group-restricted files. Additionally, failures from setuid() were silently ignored rather than being treated as fatal, so the mailer could continue running as root if privilege dropping failed.


#### 2025-11-08: FD leak in UUID storage


Commit[658bbc4af](https://github.com/sudo-project/sudo/commit/658bbc4af983d78e0dd3e24af60719571d142ca9)


**Subject** iolog_store_uuid: Fix file descriptor leak on success


**Vulnerable Since** unreleased (post-1.9.17p2)


**Potential Security Impact** DoS, FD Exhaustion


**Security Issue** File descriptor leak in iolog_store_uuid() in the logsrvd I/O log writer. On the success path, the function opened a file descriptor to write UUID data but never closed it. Over time, with many log server connections, this would exhaust available file descriptors, leading to denial of service as the log server could no longer open files or accept connections.


#### 2025-11-08: FD leak on I/O log setup failure


Commit[ba1d5b77c](https://github.com/sudo-project/sudo/commit/ba1d5b77ce3a3c5f23ac77f77654de4ac6b2d4f3)


**Subject** sudoers_io_open_local: Close iolog_dir_fd and iolog_files\[\] on error


**Vulnerable Since** 1.9.0


**Potential Security Impact** DoS, FD Exhaustion


**Security Issue** File descriptor leak on error paths in sudoers_io_open_local(). When I/O logging setup failed partway through (e.g., after opening the iolog directory fd or creating some iolog files), the function returned an error without closing iolog_dir_fd or any already-opened iolog_files\[\] entries. Repeated failures would leak file descriptors, eventually exhausting the process's fd limit and causing denial of service or preventing subsequent I/O logging from functioning.


#### 2025-11-09: Audit log FD inherited by child processes


Commit[a24c73621](https://github.com/sudo-project/sudo/commit/a24c73621d0d13976c6c6bfa5d02273dbf4c5ce1)


**Subject** audit_json_open: Set the close-on-exec flag for the JSON audit fd


**Vulnerable Since** 1.9.0


**Potential Security Impact** FD Leak to Child, Audit Tampering, Information Disclosure


**Security Issue** The JSON audit log file descriptor was not marked close-on-exec (FD_CLOEXEC). When sudo executes a child command, the open audit log file descriptor would be inherited by the child process. This could allow the executed command to write to or read from the audit log file, potentially tampering with audit records or leaking sensitive information about other sudo sessions recorded in the same file.


#### 2025-11-09: LDAP credentials written to debug log


Commit[d83714aac](https://github.com/sudo-project/sudo/commit/d83714aac0829ec4ce4df32b7d4eabc5bb3a288a)


**Subject** Do not log LDAP bindpw or tls_keypw to debug log.


**Vulnerable Since** 1.8.7


**Potential Security Impact** Credential Exposure


**Security Issue** Sensitive LDAP credentials (bind password and TLS key password) were written in cleartext to the debug log. The LDAP bind password (bindpw) was logged verbatim via DPRINTF1, and the decoded secret returned by sudo_ldap_decode_secret() was also returned through debug_return_str which logs the return value. Anyone with read access to the debug log file could obtain the LDAP bind credentials, potentially allowing unauthorized access to the LDAP directory. The fix masks the password in debug output and replaces the cleartext value with '********'.


#### 2025-11-11: Dangling pointer in eventlog struct


Commit[b48fd11e3](https://github.com/sudo-project/sudo/commit/b48fd11e3945080cdbf0ee6df99c40ef69bc1c75)


**Subject** Embed struct eventlog into struct log_details instead of using a pointer.


**Vulnerable Since** 1.9.4


**Potential Security Impact** Use-After-Free, Information Disclosure


**Security Issue** Use-after-free / dangling pointer risk in struct log_details. The evlog field was a pointer (struct eventlog *) that in some code paths was set to point to a stack-allocated or function-scoped struct eventlog via shallow copy, while in other paths it pointed to heap-allocated memory. After the function that set the pointer returned, the pointer could become dangling if the original eventlog went out of scope. Accessing log_details.evlog after the originating function returned could read freed or reused stack memory, potentially leaking sensitive data or causing corruption. The fix embeds the struct directly, ensuring the data is copied by value and persists for the lifetime of log_details.


#### 2025-11-11: Path traversal via unsanitized log paths


Commit[186f94507](https://github.com/sudo-project/sudo/commit/186f94507f1222f8b21349f4c91c5d5e810fa872)


**Subject** Replace '/' with '_' in paths using the user, group, host or command.


**Vulnerable Since** 1.9.0


**Potential Security Impact** Path Traversal, Arbitrary File Write, RCE


**Security Issue** Path traversal vulnerability in I/O log path construction on the logsrvd server. When constructing I/O log directory paths, user-controlled values (username, group, hostname, command name) were interpolated directly into file paths without sanitizing '/' characters. A malicious user or hostname containing '/' (e.g., a username like '../../etc') could escape the intended log directory and write I/O log files to arbitrary locations on the filesystem. The fix replaces '/' with '_' in these interpolated values and uses only the basename for command expansion.


#### 2025-11-11: Directory traversal via '..' in log paths


Commit[4bd549d7f](https://github.com/sudo-project/sudo/commit/4bd549d7f41c7d3a0365f35b43f8bbf40a3a2821)


**Subject** Check for embedded ".." in the expanded I/O log dir and file.


**Vulnerable Since** 1.9.0


**Potential Security Impact** Path Traversal, Arbitrary File Write, RCE


**Security Issue** Path traversal vulnerability in sudo logsrvd I/O log path handling. When expanding the I/O log directory and file paths (via expand_iolog_path), the resulting strings were not checked for '..' path components. A malicious client or crafted configuration could inject '..' sequences into path variables used during expansion, allowing the log server to write I/O log files to arbitrary locations outside the intended log directory. This could be used to overwrite sensitive files on the system running logsrvd.


#### 2025-11-11: Queue errors ignored after daemonizing


Commit[fbd0e2254](https://github.com/sudo-project/sudo/commit/fbd0e22545c977989bb3a79be59d9054108d933a)


**Subject** Scan the outgoing queue and setup associated events before daemonizing.


**Vulnerable Since** 1.9.7


**Potential Security Impact** Audit Evasion, Silent Failure


**Security Issue** Failure to validate the outgoing relay queue before daemonizing in logsrvd. The logsrvd_queue_scan() call occurred after daemonize(), meaning errors during queue initialization (e.g., malformed queue entries, corrupted journal files) would go undetected since the daemon had already detached from the terminal and the parent process had exited successfully. Additionally, the return value of logsrvd_queue_scan() was never checked. This could allow the daemon to run in a broken state, silently failing to relay queued log messages and potentially losing audit data.


#### 2025-11-11: Relay falls back to wrong TLS context


Commit[ddb9cfef5](https://github.com/sudo-project/sudo/commit/ddb9cfef517e8604e81a945a6c74ada7ffc304c9)


**Subject** logsrvd_relay_tls_ctx: Do not fall back on server TLS context


**Vulnerable Since** 1.9.7


**Potential Security Impact** TLS Downgrade, MITM


**Security Issue** TLS context confusion in logsrvd relay configuration. When a relay was configured without its own TLS settings, logsrvd_relay_tls_ctx() would fall back to using the main server's TLS context. The server TLS context may have different certificate verification settings, trusted CA chains, or cipher requirements than what is appropriate for the relay connection. This could cause the relay to connect to an upstream server using unintended or weaker TLS credentials/verification, potentially allowing man-in-the-middle attacks or connections to untrusted relay targets.


#### 2025-11-11: Stale journal path after fdopen failure


Commit[61c5a99f7](https://github.com/sudo-project/sudo/commit/61c5a99f735ec74f7e004cb01614ae8c91a81c9e)


**Subject** journal_fdopen: free closure->journal_path on fdopen() error


**Vulnerable Since** 1.9.7


**Potential Security Impact** Use-After-Free, Audit Evasion, Memory Leak


**Security Issue** Resource leak and stale state after fdopen() failure in logsrvd journal handling. When fdopen() failed on the journal file descriptor, closure->journal_path was not freed or set to NULL. Since other parts of logsrvd check closure->journal_path to determine whether journaling is active, the stale non-NULL pointer would cause the code to incorrectly believe journaling was in progress. This could lead to use-after-free or operations on invalid file handles, potentially causing crashes, log data corruption, or missed audit records.


#### 2025-11-11: I/O log write failures silently ignored


Commit[c3975e568](https://github.com/sudo-project/sudo/commit/c3975e56867549a6c7cb5724b30c1344b7c97692)


**Subject** iolog_write() returns -1 on failure, not false.


**Vulnerable Since** 1.9.0


**Potential Security Impact** Audit Evasion, Silent Failure


**Security Issue** Incorrect error checking for iolog_write() return value in logsrvd. The code used boolean-style '!iolog_write(...)' checks, treating the return value as true/false. However, iolog_write() returns -1 on failure (not 0/false). Since -1 is truthy in C (non-zero), the error condition was never detected. This meant I/O log write failures were silently ignored, allowing the log server to continue without recording command input/output data, effectively losing audit trail data critical for security monitoring and forensics.


#### 2025-11-12: Arbitrary signal names in suspend messages


Commit[4ed05da3b](https://github.com/sudo-project/sudo/commit/4ed05da3b83b1d075a31bbb4cfebb058406a9828)


**Subject** handle_suspend: Only allow STOP, TSTP, CONT, TTIN, TTOU signals


**Vulnerable Since** 1.9.0


**Potential Security Impact** Log Injection, Input Validation


**Security Issue** Insufficient validation of signal names in CommandSuspend messages in logsrvd. The handle_suspend() function only checked that the signal string was non-empty, but did not validate that it was one of the legitimate job-control signals (STOP, TSTP, CONT, TTIN, TTOU). A malicious sudo client could send a CommandSuspend message with an arbitrary signal name, which would then be logged and potentially processed downstream. This could be used for log injection/spoofing (injecting misleading audit data) or to trigger unexpected behavior in log consumers that parse the signal name field.


#### 2025-11-12: Negative snprintf return unchecked


Commit[dfa1434c6](https://github.com/sudo-project/sudo/commit/dfa1434c6edd1a78b1aa2b98c70742b7bccb515f)


**Subject** logsrvd_queue_scan: check for snprintf() return value < 0


**Vulnerable Since** 1.9.7


**Potential Security Impact** Buffer Underflow, Out-of-Bounds Access


**Security Issue** Missing check for snprintf() encoding error in logsrvd_queue_scan(). The code only checked if snprintf() returned a value >= the buffer size (truncation), but did not check for a return value < 0, which indicates an encoding error. On encoding error, snprintf() returns a negative value, which when compared to the buffer size as a signed value would pass the truncation check, leaving 'dirlen' set to a negative number. This negative dirlen could then be used in subsequent operations (e.g., path manipulation), leading to buffer underflows, out-of-bounds memory access, or path construction errors.


#### 2025-11-12: SSL wrapper returns wrong values on error


Commit[59afb334f](https://github.com/sudo-project/sudo/commit/59afb334f9536c0df81e45d4b1a922558fcbbd6f)


**Subject** Make SSL_read_ex and SSL_write_ex wrappers set number of bytes on failure.


**Vulnerable Since** 1.9.15


**Potential Security Impact** Uninitialized Memory Read, TLS Protocol Error


**Security Issue** The SSL_read_ex() and SSL_write_ex() compatibility wrappers did not match the OpenSSL API contract. On error, the real OpenSSL functions set *readbytes/*written to 0 and return 0, but these wrappers returned the raw negative SSL_read()/SSL_write() return value (e.g., -1) directly. Callers expecting the standard API behavior (return 0 on failure, 1 on success) would misinterpret -1 as a truthy success value, causing them to proceed as if the TLS operation succeeded. Additionally, the *readbytes/*written output parameters were left uninitialized on error, so callers could read garbage values as the byte count, leading to processing of uninitialized memory or incorrect buffer advancement in TLS communication.


#### 2025-11-12: Short reads accepted as complete records


Commit[3c78d0876](https://github.com/sudo-project/sudo/commit/3c78d087684a91af812a74b737331ccc1bc7dead)


**Subject** read_io_buf: Treat a short read as an error


**Vulnerable Since** 1.9.15


**Potential Security Impact** Audit Evasion, Data Integrity


**Security Issue** Short reads from I/O log files were silently accepted in sendlog's read_io_buf(). The code only checked for iolog_read() returning -1 (error), but if iolog_read() returned fewer bytes than expected (a short read), the function would treat the partial data as a complete record and send it to the log server. This could result in truncated or corrupted I/O log data being transmitted, undermining the integrity of the audit trail. An attacker who could truncate log files on disk could exploit this to cause incomplete audit records to be relayed without any error being raised.


#### 2025-11-13: Missing required field checks in JSON parser


Commit[a8546ab2c](https://github.com/sudo-project/sudo/commit/a8546ab2c3f8193d6c6fa878aa738a72d17926a1)


**Subject** iolog_parse_loginfo_json: Add check for required eventlog entries


**Vulnerable Since** 1.9.0


**Potential Security Impact** NULL Pointer Dereference, DoS


**Security Issue** The I/O log JSON parser (iolog_parse_loginfo_json) did not validate that required fields (command, cwd, runargv, runuser, submituser, ttyname) were present in the parsed eventlog structure. A maliciously crafted or corrupted log.json file missing these fields could cause NULL pointer dereferences in downstream code that assumes these fields are always populated after a successful parse.


#### 2025-11-13: NULL deref in TLS peer verification


Commit[3226ef7b3](https://github.com/sudo-project/sudo/commit/3226ef7b3d66097d3432d2a49915b3fc457203dd)


**Subject** verify_peer_identity: Check for missing application specific data.


**Vulnerable Since** 1.9.0


**Potential Security Impact** TLS Verification Bypass, MITM, DoS


**Security Issue** The TLS peer certificate verification callback (verify_peer_identity) did not check whether SSL_get_ex_data() and X509_STORE_CTX_get_ex_data() returned NULL before dereferencing the returned pointers to access peer_info/closure data (hostname, IP address). If the application-specific data was missing from the SSL object, this would result in a NULL pointer dereference. In the worst case, a crafted TLS connection could bypass hostname verification if the callback crashed rather than returning a verification failure, potentially allowing a man-in-the-middle attack.


#### 2025-11-13: Re-initialization after partial failure


Commit[fd4b369d7](https://github.com/sudo-project/sudo/commit/fd4b369d7522e86479b2d30dfc4f75095d6c9ef5)


**Subject** sudoers_init: initialize ret to 0, not -1


**Vulnerable Since** 1.9.1


**Potential Security Impact** Inconsistent State, Double Initialization


**Security Issue** The sudoers_init() function used a static variable 'ret' initialized to -1 and checked 'if (snl != NULL)' to prevent re-initialization. If initialization failed partway through (before snl was set), the function would attempt to re-initialize on subsequent calls, potentially re-executing partially completed setup code in an inconsistent state. Early error returns also bypassed cleanup code. The fix initializes ret to 0 and uses 'ret != 0' as the re-initialization guard, ensuring that both successful initialization (ret=1) and failed initialization (ret=-1) prevent re-entry, and routes all error paths through a common cleanup label.


#### 2025-11-14: Listeners dropped during config reload


Commit[b040db2be](https://github.com/sudo-project/sudo/commit/b040db2be79b635c72e7c8b63a2190cd6c181c94)


**Subject** server_setup: preserve old listener if it matches the new config


**Vulnerable Since** 1.9.0


**Potential Security Impact** DoS


**Security Issue** During a sudo_logsrvd configuration reload (SIGHUP), server_setup() unconditionally closed all existing listener sockets and reopened them. This created a race condition window where no listeners were active, causing a denial of service for incoming connections. More critically, under resource starvation conditions (e.g., file descriptor exhaustion), the server could fail to reopen sockets that were previously working, resulting in a permanent denial of service where the log server becomes completely unreachable. The fix preserves existing listeners whose addresses match the new configuration, only closing sockets that are no longer needed.


#### 2025-11-14: Server crashes on allocation failure


Commit[c69f90259](https://github.com/sudo-project/sudo/commit/c69f902590c711829f23fa978a65e60a2b500eea)


**Subject** register_listener: don't exit on failure, just return false


**Vulnerable Since** 1.9.0


**Potential Security Impact** DoS


**Security Issue** The register_listener() function in sudo_logsrvd called sudo_fatalx() (which terminates the process) on memory allocation failures when creating listener structures or event objects. This meant that a transient memory pressure condition during listener registration (especially during a config reload) would crash the entire log server daemon, causing a denial of service for all sudo logging. The fix changes fatal errors to non-fatal warnings that return false, allowing the server to continue operating with the listeners it was able to create.


#### 2025-11-15: Unvalidated TimeSpec from client messages


Commit[a257a7dd1](https://github.com/sudo-project/sudo/commit/a257a7dd192f3f86c595e9fa4d6de01803129c00)


**Subject** Validate a TimeSpec before using it.


**Vulnerable Since** 1.9.0


**Potential Security Impact** Integer Overflow, Input Validation


**Security Issue** The sudo_logsrvd server accepted TimeSpec values from client messages (RestartMessage, IoBuffer, ChangeWindowSize, CommandSuspend) without validating that tv_sec and tv_nsec were non-negative and that tv_nsec was less than 1,000,000,000. A malicious client could send crafted protobuf messages with negative or out-of-range TimeSpec values, leading to undefined behavior in timespec arithmetic, potential integer overflows, or incorrect I/O log timing/replay behavior.


#### 2025-11-15: NULL deref in journal seek


Commit[353df3965](https://github.com/sudo-project/sudo/commit/353df3965d7cab763022957fb04488a5c2f7462c)


**Subject** journal_seek: Sanity journaled message field before using


**Vulnerable Since** 1.9.7


**Potential Security Impact** NULL Pointer Dereference, DoS


**Security Issue** The journal_seek() function in sudo_logsrvd's journal replay code dereferenced message union fields (ttyin_buf, ttyout_buf, stdin_buf, stdout_buf, stderr_buf, winsize_event, suspend_event) and their delay sub-fields without checking for NULL. If a journal file was corrupted or maliciously crafted with invalid protobuf messages, this could cause NULL pointer dereferences when seeking through the journal. The fix adds NULL checks and TimeSpec validation for each message type before accessing the delay field, preventing crashes from corrupted journal files.


#### 2025-11-27: TLS failure kills entire relay event loop


Commit[e89bb8118](https://github.com/sudo-project/sudo/commit/e89bb8118024c2123ed78f49efa06310e762338e)


**Subject** Add TLS connection error callback function.


**Vulnerable Since** 1.9.7


**Potential Security Impact** DoS, Audit Evasion, Memory Leak


**Security Issue** When a TLS handshake failed in sudo_logsrvd's relay mode, the error path in tls_connect_cb() unconditionally called sudo_ev_loopbreak(), which broke out of the event loop entirely. For the relay use case, a single TLS negotiation failure with one relay server would prevent logsrvd from trying alternative relay servers configured in the relay list, effectively causing a denial of service for log forwarding. Additionally, tls_ctx_client_setup() did not free a previous SSL object before allocating a new one, causing a memory leak on reconnection attempts. The fix adds a configurable error callback so relay mode can attempt the next relay server on TLS failure.


#### 2025-12-26: Read error ignored in visudo


Commit[a642d9802](https://github.com/sudo-project/sudo/commit/a642d9802d94b05ed7f79ec71e1eda56822d1a16)


**Subject** edit_sudoers: Check for read() error


**Vulnerable Since** 1.8.0


**Potential Security Impact** Access Control Bypass, Data Integrity


**Security Issue** In visudo's edit_sudoers() function, the return value of read() was not checked for -1 (error). A read error (e.g., from a disk I/O failure) would cause nread to be -1 and the loop to exit, but the code would then proceed to process the incompletely-read sudoers file content as if it were valid. This could result in a truncated or partially-read sudoers file being accepted by visudo, potentially removing security-critical access control rules that appeared after the point of the read failure.
