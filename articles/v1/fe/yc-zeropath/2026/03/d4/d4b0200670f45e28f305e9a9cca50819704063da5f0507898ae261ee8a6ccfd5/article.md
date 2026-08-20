---
schema_version: "1.0.0"
document_id: "d4b0200670f45e28f305e9a9cca50819704063da5f0507898ae261ee8a6ccfd5"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/zeropath-exploit-development-ctfs"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:4ed7dbcaced6ca8c1c5100d7bd8bfdab84f5d587f99a664db16217a15886763c"
---

# ZeroPath Exploit Development CTFs

# ZeroPath Exploit Development CTFs


## Introduction


Want to learn how to exploit complex real world vulnerabilities, but aren’t sure where to start? We’ve just released[zeropath-ctf](https://github.com/ZeroPathAI/zeropath-ctf) , a set of self-contained exploit development exercises based on CVEs from the CISA Known Exploited Vulnerabilities list, along with hints and solutions in case you get stuck.


Launching a CTF for CVE-2017-1000367


## Background


Our CTFs try to walk a middle course between two bad alternatives:


On the one hand, diving right into checking out large open source projects with known vulnerabilities can be daunting for a beginner… hundreds of thousands of lines of irrelevant code, hours invested getting the application built, configured and running in a sandbox in just the right way, all to try to develop an exploit without a lot of reliable, distilled guidance.


On the other, intentionally vulnerable apps or toy vulnerabilities tend to lack the complexity and subtlety of real vulnerabilities… developing exploits for them is easy, but you’re not exercising all the muscles you need to do it for real.


To solve this problem ZeroPath’s shapeshifter vulnerability generation suite analyzes real world vulnerabilities and produces standalone synthetic lookalikes that prove-ably contain the same key flaws and structure, along with working exploits and walkthroughs for both LLM and human consumption.


We’ve used shapeshifter to create CTFs for 10 CVEs, including well-known issues like Baron Samedit, and a range of flaw types, like heap overflows, command injections and TOCTOU race conditions. Each one is packaged in a docker image with a harness around it to verify solutions and walk you through the process of developing your exploit.


## Video Walkthrough


This video walks you through the core functionality of the CTF suite:


If you’d prefer written instructions, our[README.md](https://github.com/ZeroPathAI/zeropath-ctf/blob/main/README.md) is a good starting point.


## Technology Behind the CTFs


### Overview


These CTFs are only useful if shapeshifter-produced vulnerabilities do in fact mirror the key structure of the real world vulnerabilities they’re based on. In future posts, we’ll go into more detail about how we verify this, and how we apply these synthetic vulnerabilities to benchmarking SAST solutions at scale.


Here, we’ll do a quick walkthrough of the original issue one of the synthetics is based on, highlighting the key points of similarity.


### CVE-2017-1000367


#### Overview


CVE-2017-1000367 is a well-known issue in sudo. It’s a little more complex than many demo vulnerabilities so, it’s a perfect one to dive deeper with to understand how shapeshifter preserves the complexity that matters while still distilling the vuln down to its essence.


The heart of the bug is two flaws – one involving improper parsing of user input and another involving trusting devices in world-writable directories, stacked with two exploitable race conditions.


#### Flaw 1: Unsafe parsing of user-provided input


The first bug that contributes to the vulnerability is in sudo_ttyname_dev(), which runs at application startup. This function opens /proc/<pid of sudo>/stat, which is a simple text file, and tries to parse it to get the current tty number.


Everything starts in \`get_process_ttyname()\` (\`src/ttyname.c:479\`). On Linux, sudo reads \`/proc/\[pid\]/stat\` to discover its controlling terminal's device number. This file looks like:


```text
1234 (bash) S 1233 1234 1234 34818 1234 ...


```


The fields are space-separated, and field 7 is \`tty_nr\`. The parser counts spaces to find it:


```text
// src/ttyname.c:497-504
char     *  cp   =   line  ;
char     *  ep   =   line  ;
int   field   =     0  ;
while     (  *  ++  ep   !=     '\0'  )     {
if     (  *  ep   ==     ' '  )     {
*  ep   =     '\0'  ;
if     (  ++  field   ==     7  )     {
dev_t   tdev   =     strtonum  (  cp  ,   INT_MIN  ,   INT_MAX  ,     &  errstr  )  ;
```


The bug: field 2 (comm) is the process name in parentheses. This doesn’t seem like user-controlled input, but it is: the attacker can launch the process using a symlink, and the name of that symlink will become the process name.


If the symlink is named "<multiple spaces>34873 ", the stat line becomes:


```text
1234 (     34873 ) S 1233 1234 1234 34818 ...
^  spaces inside comm  ^


```


The naive space-counting parser hits the spaces inside the parentheses, treats them as field separators and incorrectly reads 34873 as tty_nr – the attacker's chosen device number instead of the real value (34818).


In our synthetic vuln, this flaw is reproduced in “get_serial_port()”:


```text
static     char     *
get_serial_port  (  const     char     *  status_path  ,     char     *  name_out  ,     size_t   name_sz  )
{
// ...
char     *  cp   =   line  ;
char     *  ep   =   line  ;
int   field   =     0  ;


while     (  *  ++  ep   !=     '\0'  )     {                              /* ← VULNERABLE PARSER */
if     (  *  ep   ==     ' '  )     {
*  ep   =     '\0'  ;
if     (  ++  field   ==     7  )     {
/* Extract device number */


```


#### Flaw 2: Scanning all of /dev/ including world-writable directories


Spoofing a TTY by itself doesn’t seem very useful, but another flaw and two race conditions make it much more interesting.


The second mistake occurs when sudo tries to open the relevant tty device file – starting by passing spoofed device number 34873 into sudo_ttyname_dev() (src/ttyname.c:317). This function initially tries a fast path — it extracts the minor number and constructs a standard tty device file path directly:


```text
// src/ttyname.c:332-347
if     (  strcmp  (  devname  ,     "/dev/pts/"  )     ==     0  )     {
(  void  )  snprintf  (  buf  ,     sizeof  (  buf  )  ,     "%spts/%u"  ,   _PATH_DEV  ,
(  unsigned     int  )  minor  (  rdev  )  )  ;
if     (  stat  (  buf  ,     &  sb  )     ==     0  )     {
if     (  S_ISCHR  (  sb  .  st_mode  )     &&   sb  .  st_rdev   ==   rdev  )     {
// Match! Return this path.
```


34873 decodes to major 136, minor 57 (since dev_t = major * 256 + minor on Linux). So sudo constructs /dev/pts/57 and stat()s it. The attacker ensures this PTY does not exist yet, so stat() fails.


Having exhausted the fast paths, sudo falls through to a breadth-first scan of the entire /dev/ tree:


```text
// src/ttyname.c:372
ret   =     sudo_ttyname_scan  (  _PATH_DEV  ,   rdev  ,   false  ,   name  ,   namelen  )  ;
```


This scan walks every subdirectory under /dev/… critically including /dev/shm/, which is a world-writable tmpfs. Also,it follows symlinks (using stat(), not lstat()) and matches any character device whose st_rdev equals the target:


```text
// src/ttyname.c:248,284
if     (  stat  (  pathbuf  ,     &  sb  )     ==     -  1  )
continue  ;
.  .  .
// Found it — store pathbuf as the tty name
strlcpy  (  name  ,   pathbuf  ,   namelen  )  ;
```


The attacker has prepared a symlink at /dev/shm/_tmp/_tty pointed to the correct pts device ( /dev/pts/57). Since stat() follows symlinks, if /dev/pts/57 exists at the moment of the check, the symlink resolves to a character device with st_rdev == 34873, and sudo accepts /dev/shm/_tmp/_tty as its tty name.


At this point, sudo has mis-parsed malicious user input and selected a user-controlled symlink as the device to send its output to… however, actually doing anything useful with the issue involves taking advantage of two race conditions.


##### Race condition 1: Making the PTY appear during the scan


The problem is timing. During the fast-path check, /dev/pts/57 must not exist (so sudo falls through to the scan paths like /dev/shm). During the broader scan, it must exist (so that sudo will accept the attacker-controlled symlink to it as a valid device).


The attacker can make this easier on themselves using signals:


1. The attacker sets up inotify on /dev/shm/_tmp/, watching for IN_OPEN
2. Sudo unsuccessfully tries to open /dev/pts/57
3. Sudo proceeds to do its broader search across /dev for the tty
4. When sudo_ttyname_scan() calls opendir("/dev/shm/_tmp/"), the inotify fires
5. The attacker sends SIGSTOP to the sudo process, freezing it mid-scan. (Permitted because the user spawned sudo themselves)
6. The attacker calls openpty() in a loop until the kernel allocates /dev/pts/57, making the their symlink point to a real device
7. The attacker sends SIGCONT to resume sudo


Sudo's scan now continues. It reads the _tty directory entry, calls stat() on the symlink, follows it to the now-existing /dev/pts/57, sees S_ISCHR and st_rdev == 34873, and concludes: “my tty is /dev/shm/_tmp/_tty.”


Key steps in exploiting the first race


##### Race condition 2: Swapping the symlink before relabel_tty() opens it


We still don’t have anything terribly useful at this point though. We’ve just jumped through many hoops to have sudo send its output to a real pty.


This is where the second race condition comes in – we need to swap out the destination of the symlink after sudo has fully verified it points to a real tty but before it actually starts sending output to it.


The tty path flows from get_process_ttyname() through to selinux_setup() (src/selinux.c:322), which passes it to relabel_tty(). When the -r role flag is used (triggering SELinux RBAC), relabel_tty() opens the tty path by name:


```text
// src/selinux.c:162-163
if     (  ptyfd   ==     -  1  )     {
se_state  .  ttyfd   =     open  (  ttyn  ,   O_RDWR  |  O_NOCTTY  |  O_NONBLOCK  )  ;


And later  ,   redirects   stdin  /  stdout  /  stderr   to it  :


// src/selinux.c:218-219
for     (  fd   =   STDIN_FILENO  ;   fd   <=   STDERR_FILENO  ;   fd  ++  )     {
if     (  isatty  (  fd  )     &&     dup2  (  se_state  .  ttyfd  ,   fd  )     ==     -  1  )     {
```


Between the scan resolving the name and relabel_tty() opening it, there's a window. The attacker exploits it:


1. Monitor /dev/shm/_tmp/ for IN_CLOSE_NOWRITE — this fires when the scan's stat() closes
2. SIGSTOP sudo again
3. Replace the symlink: /dev/shm/_tmp/_tty now points to /etc/passwd (or any target file) instead of /dev/pts/57
4. SIGCONT sudo


Sudo calls open("/dev/shm/_tmp/_tty", O_RDWR) — but the symlink now points to /etc/passwd. The open() succeeds (sudo is running as root), and dup2() redirects the command's stdout to the target file. The attacker can now overwrite arbitrary root-owned files with command output.


Key steps in exploiting the second race


#### Flaw and race conditions in the synthetic vuln


The synthetic vulnerability includes the same broad search for a tty device file in /dev, including /dev/shm, and the same race conditions.


```text
static     char     *
find_serial_dev  (  dev_t   rdev  ,     char     *  name_out  ,     size_t   name_sz  )
{


// First tries /dev/pts/<number> like sudo does
// if that fails, invokes the broader search:
ret   =     scan_device_dir  (  buf  ,   rdev  ,     0  ,   name_out  ,   name_sz  )  ;
/// ...


static     char     *
scan_device_dir  (  const     char     *  dir  ,     dev_t   rdev  ,     int   is_known  ,
char     *  name_out  ,     size_t   name_sz  )
{
// ...
// like sudo, scan_device_dir uses stat to examine potential devices
// which follows symlinks
​​     if     (  stat  (  pathbuf  ,     &  sb  )     ==     -  1  )


// ..


// also like sudo, scan_device_dir verifies that the file is in fact
// the expected device


```


These are all the conditions that allow an exploit developed against the real CVE-2017-1000367 to work against this synthetic with minimal modification:


- User-controlled input is mis-parsed in the same way
- That input is used to look for an invalid tty in a known location
- A broader search for the tty descends /dev/shm in the same way
- That broader search verifies that the fake tty is the right device (missing that it’s a symlink)
- The device is then actually opened and used well after the check


## Conclusion


Our free[zeropath-ctf](https://github.com/ZeroPathAI/zeropath-ctf) repository contains ten playable exploit development CTFs, including one for CVE-2017-1000367. These CTFs leverage synthetic lookalikes of the real vulnerabilities, reducing complexity without losing the essential shape of the flaw, and are coupled with walkthroughs and example, working POCs.


Hopefully, this is a fun and useful resource for people out there interested in leveling up their exploit development skills!
