---
schema_version: "1.0.0"
document_id: "b7d4093e12f08011c1a1a2df658dffd13191ca42635df50611893761eacf54c0"
company_key: "broadcom-inc-common-stock"
company: "Broadcom Inc."
source_id: "broadcom-inc-common-stock-rss-6823269edf1e"
canonical_url: "https://news.broadcom.com/mainframe-software/when-novels-meet-postcards-xcom-teaches-distributed-to-speak-z-os"
published_at: "2026-04-30T17:25:08+00:00"
first_seen_at: "2026-07-20T23:21:13.318217+00:00"
fetched_at: "2026-07-28T22:15:30.724879+00:00"
content_hash: "sha256:1ed44e0fc9ce4f0b47127c3628668d6a38ca1a69c162a9ea4f56463c39623e7f"
---

# When Novels Meet Postcards: XCOM Teaches Distributed to Speak z/OS

-
-
-


Distributed writes novels. z/OS writes postcards. And XCOM? It’s the overqualified postal clerk that makes it all work.


We recently had a customer ask a deceptively simple question:


*“Can I store a binary Linux* ® *file in a z/OS dataset?”*


​​At first glance, this sounds like a non-event. Moving binary files between distributed systems (Linux → Windows, etc.) is usually ​ *boringly* easy.


But this isn’t that. This is no longer just moving a “file” but instead asking a stream-based world to talk to a record-based world.


**Two Worlds. Two Post Offices.**


Let’s set the scene: In the distributed world, a file is a *novel* . One long, uninterrupted stream of bytes. No pages or structure. Just flow.


It could be 10 KB or 10 GB, and the system doesn’t care. It just delivers the whole thing.


In the z/OS world, things are more disciplined. z/OS doesn’t do novels: It does postcards.


Every piece of data is a record which is a neatly defined chunk with rules, format, and structure. These postcards are bundled together for efficiency, cataloged, and handled with precision.


So, when you try to move a Linux file into z/OS, you’re not just transferring data. You’re asking a novel to become a stack of postcards.


**Translating Two Data Philosophies**


Here’s the catch:


- A stream has no natural boundaries
- z/OS requires boundaries


If you don’t handle this properly, you risk:


- chopping data incorrectly
- corrupting binary content
- or worse: thinking it worked when it didn’t


Therefore, the real challenge isn’t just “move the file.”


Instead, the challenge lies in translating between two completely different data philosophies without losing a single byte.


**Enter the Overqualified Postal Clerk**


This is where XCOM earns its paycheck. Think of it as an extremely overqualified bilingual postal clerk standing between two worlds. On one side: A massive, unstructured novel; On the other: A system that demands perfectly formatted postcards


XCOM’s job is to:


- Take the continuous stream
- Break it into records (postcards)
- Preserve every single byte exactly as-is
- Make it all look natural on the z/OS side


No translation errors, missing words, or “close enough” data.


**The Trick: Flexible, Maximum-Sized Postcards**


To pull this off cleanly, we need two key decisions:


**1. Use Variable-Length Records (RECFM=V or VB)**


Fixed-length records would force artificial cuts or padding. Variable records let each “postcard” be exactly as big as it needs to be.


**2. Max Out the Record Size (LRECL=32756)**


Bigger postcards = fewer of them = better I/O efficiency.


Consider a subtle but important detail: That LRECL value of **32,756 bytes includes 4 bytes** for the RDW (Record Descriptor Word), making your actual usable data per record is 32752 bytes


The RDW is the “stamp” on the postcard telling z/OS how big the record is.


**What This Actually Looks Like**


Here’s where theory meets reality. You define your dataset like this:


- ​​RECFM=VB → variable-length records ​
- ​​LRECL=32756 → max size (including RDW) ​
- ​​BLKSIZE=32760 → Max block size allowed by IBM​
- ​​CODE=BINARY → do not touch the bytes ​
- ​​RECSEP=NO → don’t look for line breaks (this is binary, not text) ​


This tells z/OS: “Don’t try to be clever. Just store the data exactly as it arrives.”


**Let’s have a look at a sample JCL job for an XCOM z/OS partner to receive a file:**


` //jobcard
//DELDS EXEC PGM=IEFBR14
//DELDS EXEC PGM=IEFBR14
//DELDD01 DD DISP=(MOD,DELETE,DELETE),DSN=PUBLIC.MYDATA.TSTVB32K,
// UNIT=SYSALLDA,SPACE=(TRK,0)
//*
//XCOMJOB1 EXEC PGM=XCOMJOB,
// PARM=('TYPE=EXECUTE,',
// 'DFLTAB=XCOMDFLT,',
// 'TRACE=NO')
//*
//*
//XCOMCNTL DD DISP=SHR,DSN=XCOMHLQ.R12.CONFIG.XCOMCNTL
//XCOMGLOB DD DISP=SHR,DSN=USERID.XCOMGLOB
//XCOMREST DD DISP=SHR,DSN=USERID.XCOMREST
//SYSIN01 DD DISP=SHR,DSN=USERID.XCOMENCR(ENCSYZOS)
// DD *
IPNAME=10.1.2.3
IPPORT=8044
FILEOPT=CREATE
**RECFM=VB - this where you define records of variable size**
**LRECL=32756 - also includes 4 bytes for the Record Descriptor Words (RDW)**
BLKSIZE=32760 - IBM allows a maximum block size of 32760 bytes
FILETYPE=FILE
TYPE=RECEIVE
//* RECSEP=NO: Read the UNIX file in binary mode, giving a length
//* and a number of bytes to fetch (rather than all the bytes up to Line
//* Feed, which would be a record separator for text files)
**RECSEP=NO - read the max record length, do not look for line endings**`
` **CODE=BINARY** - *(this tells XCOM not to change the byte value; leave the data unchanged)*
LFILE=PUBLIC.MYDATA.TSTVB32K
FILE=/tmp/my_32k_bin_file
/*
//*
//* Calculate MD5
//*
//STEPMD5 EXEC PGM=BPXBATCH
//STDOUT DD SYSOUT=*
//STDERR DD SYSOUT=*
//STDPARM DD *
SH md5 "//'PUBLIC.MYDATA.TSTVB32K'"
/*`


**An Exact Match**


Let’s validate and simplify what we see above. The execution shows the following:


- Records received: **2245**
- Calculated dataset size: **73,524,809 bytes**
- Linux file size: **73,524,809 bytes**
- MD5 checksum (z/OS): cde7bbed9eebe8532f933d51c746442c
- MD5 checksum (Linux): cde7bbed9eebe8532f933d51c746442c


Perfect match. Not “close” or “good enough” but exact.


**Instead of receiving the file “z/OS initiated” would you rather have it “Linux initiated” send file?**


Here is the command equivalent to the above JCL:


`` /opt/CA/XCOM/bin/xcomtcp -c1 -f queue=NO
**code_flag=BINARY**
**carriage_flag=NO**
userid=userid password=password
number_of_retries=0 xtrace=0 ckpt=0
compress=NO
local_file=/tmp/my_32k_bin_file
remote_file=PUBLIC.MYDATA.TSTVB32K
remote_system=10.3.2.1
file_option=CREATE``


**recfm=VB LRECL=32756 BLKSIZE=32760 maxreclen=32752**


The key detail:


- maxreclen=32752 (data portion only, excluding RDW)


**Back to Linux**


Want to get it back to Linux? Same idea, but simply reverse the process as shown:


` /opt/CA/XCOM/bin/xcomtcp -c4 -f queue=NO
**code_flag=BINARY**
**carriage_flag=NO**
userid=userid password=password
number_of_retries=0 xtrace=0 ckpt=0
compress=NO
local_file_rf=/tmp/got_my_32k_bin_file_back
remote_file_rf=PUBLIC.MYDATA.TSTVB32K
remote_system_rf=10.3.2.1
file_option=CREATE
**recfm=VB LRECL=32756 BLKSIZE=32760 maxreclen=32752**`


Now your postcards get reassembled into a novel without losing a single word.


Let’s check if they match:


` $ **md5sum /tmp/my_32k_bin_file /tmp/got_my_32k_bin_file_back**
`
` cde7bbed9eebe8532f933d51c746442c /tmp/my_32k_bin_file
`
` cde7bbed9eebe8532f933d51c746442c /tmp/got_my_32k_bin_file_back```


## **Bridging Two Data Models With XCOM**


This isn’t just about moving files, but more aligned to bridging two fundamentally different data models: Flexible vs Structured. And when done right, z/OS and distributed systems look like lifelong pen pals, even when they secretly write in completely different languages.


XCOM truly is the overachieving postal clerk who speaks both languages, respects both systems, and somehow makes it all look effortless.


**Have questions about configuring XCOM?**


Schedule a[no-cost consultation](https://engage.broadcom.com/mainframe-xcom-engineering-consult) with Broadcom Mainframe engineers and architects.


---


About the Author


About the Author


[Heinz Sporri](https://news.broadcom.com/author/heinz-sporri)
