---
schema_version: "1.0.0"
document_id: "0f48a8197c564bc17827e1d4313a9f20fefab464cc5a668f46aff89933723584"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/proftpd-cve-2026-42167-auth-bypass-privesc-rce"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:15:34.522080+00:00"
content_hash: "sha256:d844ed3f9cb25aa81e132a0263cc5e1f039152112598ce713c224da2adc0df05"
---

# CVE-2026-42167 Allows Auth Bypass And RCE In ProFTPD

## Summary


ZeroPath Research discovered a SQL injection vulnerability in ProFTPD's mod_sql extension. Depending on configuration, the flaw can be exploited before authentication and may lead to authentication bypass, privilege escalation, or remote code execution.


MITRE has assigned the flaw[CVE-2026-42167](https://nvd.nist.gov/vuln/detail/CVE-2026-42167) and rated it 8.1 on the CVSSv3 severity scale.


Bypassing auth to inject a backdoor user with full disk access.


## Impacted Software


Vulnerable Versions Patched Versions


- <= 1.3.9


- >= 1.3.9a


## Timeline


- 2026-03-28 — Issue reported to ProFTPD maintainers
- 2026-04-07 — ProFTPD maintainers and ZeroPath work to verify patch
- 2026-04-24 —[CVE-2026-42167](https://nvd.nist.gov/vuln/detail/CVE-2026-42167) issued
- 2026-04-27 — Commit[af90843baf7dcb8c6be1e5261be2d0b5b5850673](https://github.com/proftpd/proftpd/commit/af90843baf7dcb8c6be1e5261be2d0b5b5850673) fixes issue
- 2026-04-27 — 1.3.9a released with fix


## ProFTPD


### Background


ProFTPD is a widely-deployed FTPd daemon, with over 160,000 instances accessible on the public internet, according to Shodan.


162,329 public ProFTPD instances as of April 28, 2026.


Most modern Linux distributions include a ProFTPD package. It's also commonly bundled with web hosting administration packages including:


- DirectAdmin
- Plesk
- ISPConfig
- Webmin
- cPanel


Based on our testing, we estimate that at least 1% of publicly-accessible ProFTPD instances are vulnerable to SQL injection before authentication. A likely larger number are vulnerable post-authentication, since many more attack vectors are reachable after login. What an attacker can do with the injection depends on each instance's configuration — ranging from exfiltrating sensitive data, to auth bypass, to RCE.


### mod_sql


ProFTPD comes bundled with the mod_sql extension. When enabled, this extension can power a wide range of functionality, from quota tracking, to ban lists, to authentication.


Two capabilities are especially important for understanding[CVE-2026-42167](https://nvd.nist.gov/vuln/detail/CVE-2026-42167) and its impact.


#### Authentication


Using the` SQLAuthenticate` and` SQLUserInfo` directives, an admin can configure ProFTPD to authenticate users against a SQL table instead of the local` /etc/passwd` file. This can come in handy for use cases like web hosting — it's not necessary to maintain full valid unix users for every FTP user, and it's quick to update user records in a centralized database.


#### Logging


Using the` SQLNamedQuery` and` SQLLog` statements, an admin can configure ProFTPD to store its logs in a SQL database where they can be easily accessed and aggregated. Like its SQL authentication feature, this is especially useful for hosting services.


## The Flaw: CVE-2026-42167


### Logging: Key Attack Surface


Admins configure what gets logged to SQL with statements like these:


```text
SQLNamedQuery log_activity INSERT "'%U', '%r', '%m'" activity_log
SQLLog        *           log_activity
SQLLog        ERR_*       log_activity


```


` SQLLog` selects particular commands to log. In this case all commands are logged.` SQLNamedQuery` specifies how to insert log entries into the database.


Critically, the` SQLNamedQuery` includes magic` %` expansions, which get replaced by data from the request. Many of these expansions are potentially attacker-controlled, including:


Variable Meaning


%A anonymous-login password string


%J command parameters (everything after the verb)


%S response message string (may include attacker input echoed back in errors)


%U original username from` USER` (set before auth, available even on failed login)


%d directory name (last path component)


%l RFC 1413 ident response (attacker-controlled if they run identd)


%m FTP method/verb (attacker chooses which command to send)


%r full FTP command (verb + args)


%u authenticated username


%{basename} filename component of the path argument, no directory prefix


The attack surface here is obvious. Can an attacker use any of the parameters they control to slip an injection into an admin-configured` SQLNamedQuery` logging statement?


### Sink: sql_resolved_append_text()


Escape sequences get expanded into the logging SQL query in` sql_resolved_append_text()` :


```text
// contrib/mod_sql.c
static     int     sql_resolved_append_text  (  pool   *  p  ,     struct     sql_resolved     *  resolved  ,
const     char     *  text  ,     size_t   text_len  )     {
char     *  new_text  ;
size_t   new_textlen  ;


// ...


if     (  is_escaped_text  (  text  ,   text_len  )     ==   FALSE  )     {
// ZP: In this branch, escape value
modret_t     *  mr  ;


mr   =     sql_dispatch  (  sql_make_cmd  (  p  ,     2  ,   resolved  ->  conn_name  ,   text  )  ,
"sql_escapestring"  )  ;
if     (  check_response  (  mr  ,   resolved  ->  conn_flags  )     <     0  )     {
errno   =   EIO  ;
return     -  1  ;
}


new_text   =     (  char     *  )   mr  ->  data  ;
new_textlen   =     strlen  (  new_text  )  ;


}     else     {
// ZP !!! In this branch, do not escape value
pr_trace_msg  (  trace_channel  ,     17  ,
"text '%s' is already escaped, skipping escaping it again"  ,   text  )  ;


new_text   =     (  char     *  )   text  ;
new_textlen   =   text_len  ;
}
}
```


Immediately, we see a very interesting fork in the logic: if` is_escaped_text()` returns` TRUE` , a value will be inserted directly into the SQL query without further processing. The next question then, is whether this function's logic can be abused. The routine is pretty straightforward:


```text
static     int     is_escaped_text  (  const     char     *  text  ,     size_t   text_len  )     {
register     unsigned     int   i  ;


if     (  text  [  0  ]     !=     '\''  )     {
return   FALSE  ;
}


if     (  text  [  text_len  -  1  ]     !=     '\''  )     {
return   FALSE  ;
}


for     (  i   =     1  ;   i   <   text_len  -  1  ;   i  ++  )     {
if     (  text  [  i  ]     ==     '\''  )     {
return   FALSE  ;
}
}


return   TRUE  ;
}
```


If a value starts with a single quote, ends with a single quote and contains no single quotes within it, it's considered to be already escaped.


Presumably, the authors built this to account for cases where an already-constructed string was being passed in (e.g. some previous layer has turned` %something` into` 'a value'` ).


Unfortunately, attackers can easily set one of the expansions they control to a single-quoted value that allows for SQL injection. For example, given this logging statement that records the username (` %U` ) of everyone that tries to authenticate:


```text
SQLNamedQuery log_activity INSERT "'%U', '%r', '%m'" activity_log
SQLLog        *           log_activity
SQLLog        ERR_*       log_activity


```


An attacker can try to authenticate like this:


```text
USER ' || (SELECT 1) ||'


```


The start of the query then becomes:


```text
INSERT   "  ''     ||     (  SELECT     1  )     ||     ''
```


Notice that the single quote at the beginning and the end of the attacker username match the hardcoded single quotes in the SQL statement. The statement now reads:


> "Insert empty string concatenated with the result of a subquery concatenated with an empty string."


### Impact: RCE, Auth Bypass, Privesc and More


#### Access Necessary


What an attacker can do with this vulnerability depends on the admin's ProFTPD config. If the admin hasn't enabled mod_sql at all, or has not configured mod_sql-based logging, their instances are not vulnerable.


If mod_sql logging is enabled, the access an attacker needs to get up to mischief depends on how that logging is configured.


If pre-auth verbs like` USER` are logged, and that logging includes attacker-controlled values like` %U` (the username), an attacker only needs network access to the ProFTPD instance.


If post-auth verbs like` STOR` are logged in a way that includes attacker-controlled values, like the filename (` %f` ), then the attacker must authenticate to exploit the issue (but this authentication can include anonymous FTP login if the server is configured for anonymous access).


#### RCE


When ProFTPD connects to Postgres with superuser privileges, the impact extends beyond database access. Existing Postgres command-execution primitives allow SQL injection to be escalated to remote code execution in this configuration. (See[POCs](https://github.com/ZeroPathAI/proftpd-CVE-2026-42167-poc) for more details.)


#### Auth Bypass And Privilege Escalation


In the more common case, where a non-Postgres datastore is used, or ProFTPD is not authenticating to Postgres with a superuser, attackers can bypass authentication or expand their privileges if mod_sql is configured for authentication via the` SQLAuthenticate` directive.


The malicious user simply inserts a record into the users table with the privilege, home directory and password that they desire. They then login as this user with the password they set.


If pre-auth input, like username, are logged, this means the attacker can bypass authentication altogether. Even in cases where the attacker can only insert a user record after authenticating, they can significantly expand their privilege — e.g. setting their home directory to` /` so that they can browse and download the entire filesystem, not just a constrained directory within it.


> **Note:** Inserting a user is easiest with SQLite and Postgres backends. If the admin has configured MySQL as the backend, the attacker needs to work around not being able to stack multiple SQL statements within the insert.


#### Credential Exfil


Using timing-based blind SQL injection techniques, an attacker can exfiltrate the contents of arbitrary tables character by character, including the users table used by SQL-backed authentication (if enabled). This users table can contain plaintext or hashed passwords, depending on config. (See[POCs](https://github.com/ZeroPathAI/proftpd-CVE-2026-42167-poc) for more details.)


#### Other


mod_sql can do a lot more than authentication and logging. Abusing the authentication functionality was an obvious POC choice for us, because of its security relevance, but any function that depends on mod_sql for data storage can likely be subverted in some way. For example, a malicious user could evade quotas enforced by mod_quotatab_sql by altering relevant tables.


If you have logging via mod_sql enabled, and your log statements include attacker-controlled input, you should assume that any other mod_sql-dependent functionality is potentially compromised (out of an abundance of caution).


## POC


We've included[several full POCs](https://github.com/ZeroPathAI/proftpd-CVE-2026-42167-poc) on GitHub.


The POCs also include a setup script to stand up a vulnerable instance of ProFTPD within Docker for easy testing.


## Mitigation


- Upgrade ProFTPD to at least 1.3.9a
- If upgrade is not possible, disable logging via mod_sql
- Monitor ProFTPD instances for suspicious activity


## Takeaways


Several factors complicate finding this vulnerability.


The first is that logging substitutions like` %U` can evade simple taint analysis. For each expansion in a log statement,` pr_jot_resolve_logfmt()` replaces the format string with the value from the session and then dispatches the result to a dynamic callback passed in by the caller.


It's only when mod_sql is configured that the callback passed to` pr_jot_resolve_logfmt()` at runtime is a function that will ultimately use the session input in a dangerous way. The flow from source to sink is data and configuration-dependent, and requires among other things understanding the possible states of function pointers at runtime.


A second complicating factor is that SQL escape logic does exist. If you're starting from a potential sink (the SQL query) and moving upwards, you see that in most cases values are properly sanitized. The flaw is at the semantic level: the values ProFTPD chooses not to sanitize are not the same as those that are actually safe… but to understand that you have to have a full sense of what input gets to the logs and how, as well as why the escape logic excludes the values it does.


LLMs do well at reasoning about complex data and config-dependent flows like the one involved here, as well as analyzing semantic level bugs: moving beyond proving "this index can grow past the length of this buffer" to "this sanitization function is sound, but I don't think it achieves the author's desired intent."
