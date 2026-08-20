---
schema_version: "1.0.0"
document_id: "38c971cd22353753a743f47aaed5ce37890a7fd05bf112601b70376f1544c186"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/the-trouble-with-mounting/"
published_at: "2017-01-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:28.827282+00:00"
content_hash: "sha256:3d57ffa8a20e89c7a248da8ad13d0baa72237e813a27885afb9554881662fe15"
---

# The trouble with mounting

Greg Meyer


Yann Mahe


A few months ago, some customers sent us bug reports that they were seeing long gaps in their metrics on some systems. Sometimes, there’s an error of some sort in one metric, so you see gaps in one metric or in a class of metrics. This was a gap in every metric. The agent was just going silent.


We quickly ruled out the obvious possibilities. It wasn’t a communications error. The agent wasn’t erroring out. And, when we looked at the logs, sometimes the agent just hung. It should be killed, we have a ‘watchdog’ for this. So, it not only hangs, it hangs in an unkillable state.


We noticed a pattern. It always hung during the disk check. (I know, some of you are probably about to scream at me with the answer.) In developer mode, the agent reports how long the longest function calls took. So, we isolated these reports, and found that` os.statvfs` was taking up a very long time each time the agent stalled.


We looked at the Python source code for` os.statvfs` (the agent uses CPython) and it just calls` statvfs` in C.` statvfs` is a Linux system call, it’s implemented in glibc and Python just uses the glibc implementation.


There is one major, relatively well know instance where` statvfs` can cause the system to hang: when you’re trying to stat a remote directory mounted with NFS.


I’ll take a short step back here for a moment. NFS (Network File Systems) is a protocol for sharing a file system over a network. One computer picks a directory to share and any other computer can mount it like any other disk.


NFS can cause programs to hang in an unkillable state by design. You can mount the directory as a` hard` mount. A hard mount will never timeout a system call. It will keep trying forever. So when the Datadog agent makes a request of the NFS mount, it will just hang until the NFS server responds.


You can also mount NFS as a` soft` mount, which will eventually error out if there’s a timeout on connection. And, you can also add an` intr` option, which allows you to interrupt the program that’s making the call.


Hard mounts can be worthwhile. Maybe you want an assurance that a program will write or read to the directory, regardless of the lag. If the connection to the NFS server is relatively reliable, then you’ll only have to deal with the program hanging for a short time. However, it’s also the default option. And, if you mount an NFS disk without understanding these issues, especially if you have a flaky connection to the server, you can quickly run into problems.


There’s an additional problem with our use of it. Glibc’s implementation of it is to stat every directory listed in` /proc/mounts` until it reaches the one it’s looking for. A dropped NFS connection can cause` statvfs` to hang even if you’re not looking for the NFS mount itself.


For most people, after discovering this, it would be a simple process of changing the mount settings. However, the Datadog agent has to be able to operate on anyone’s system. They can set whatever mount options they want, we have to deal with them. So, we run the statvfs call on a separate thread, and if it’s hanging, the main thread will just continue after a timeout.


It’s not a perfect solution, and it will slightly increase memory usage on systems that have a hard mounted NFS disks. However, we often have to make these kinds of trade offs. The agent has to operate in a vast multitude of heterogenous environments and we have very little control over what our customers do with their systems.


---


We’re always looking for engineers who are excited about monitoring, metrics and troubleshooting. Contactfindmeacareer@datadoghq.com for more information about internship opportunities or visit our[careers page](https://careers.datadoghq.com/engineering/) for full-time positions.


-
-
-
