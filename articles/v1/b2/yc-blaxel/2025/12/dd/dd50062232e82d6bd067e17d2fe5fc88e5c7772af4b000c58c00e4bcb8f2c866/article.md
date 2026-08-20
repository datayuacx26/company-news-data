---
schema_version: "1.0.0"
document_id: "dd50062232e82d6bd067e17d2fe5fc88e5c7772af4b000c58c00e4bcb8f2c866"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/how-to-slash-sandbox-memory-usage-by-75-using-overlayfs"
published_at: "2025-12-03T13:52:12+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T22:25:05.780563+00:00"
content_hash: "sha256:51c9a34d9f5b3d8abab87413900849dc501b9df4f0d3d64e189fa52b859245ea"
---

# How to slash sandbox memory usage by 75% using OverlayFS

When running development runtimes for AI agents, a core component is the sandbox: an isolated environment where agents can execute code safely. One way to deploy these environments is to mount all the files in memory, and you'd have good reasons to do so. For starter, the performance of the sandbox would be unmatched in terms of latency. But if you run a lot of these isolated environments, you’ll quickly realize that loading an entire filesystem into memory every time comes with its challenges.


For a concrete example, a base Next.js image would require a full 4GB in memory to run! Let’s walk through how other sandbox architecture designs can dramatically reduce the memory needed to spin it up, down to 1 GB.


## What trouble you face when you load everything in the sandbox memory


The naive initial approach is quite straightforward: to guarantee speed and minimize disk I/O, you can use an` initramfs` . In this flow, the entire filesystem is loaded into memory at boot, by unpacking a` cpio` archive that contains the filesystem into a` tmpfs` . This approach is simple and highly performant once loaded, but it has a significant drawback.


The sandbox’s memory footprint is dictated by the size of its base image and not its actual workload.


If you needed an environment with a large set of pre-installed tools, you'd have to provision a sandbox with enough RAM to hold the entire OS and all those tools, even if your agent only ended up using` curl` . This would lead to higher memory usage and slower startup times as multi-gigabyte images are copied into RAM.


## How using OverlayFS can save memory usage at boot


A way around this bloated memory usage is` overlayfs` , a dynamic on-demand file system. Here’s how it works.


At the bottom is a read-only base image. You can think of it as a frozen, compressed snapshot of a complete operating system. It includes all the necessary binaries, libraries, and tools like Python. This image is stored on the hypervisor's disk in a highly-efficient format called` erofs` (Extendable Read-Only File System).


Instead of unpacking a whole` cpio` archive into memory at boot, the sandbox kernel now memory-maps this` erofs` image directly from the disk. When your agent needs to run a program like` python` , the kernel reads the specific data blocks for that program into the read-only image on-demand.


This “lazy-loading” approach means the initial memory footprint is incredibly small. Your sandbox only pays the memory cost for the components it actively uses.


So, what about writing files? On top of the read-only base,` overlayfs` adds a **writable layer** that lives entirely in the sandbox's RAM using` tmpfs` .


When your agent creates a log file or modifies a configuration, those changes are written to this in-memory layer. From the perspective of your code, the filesystem looks like a single, normal, and writable disk. Under the hood,` overlayfs` directs all reads to the efficient` erofs` base and all writes to the fast` tmpfs` layer, which will grow the memory footprint and will be snapshotted along.


## What this means for you


This new architecture delivers three key benefits:


The first is drastically reduced memory usage. Because the base system isn't fully loaded into RAM, your sandboxes require significantly less memory to run. You can now run more complex environments in smaller, more cost-effective instances.


The second benefit are even faster cold starts. Boot times are no longer tied to image size. With the file system loaded on-demand, sandboxes are ready almost instantaneously, allowing your agents to start their work faster than ever before.


Finally, snapshots are more efficient. This new model allows for smaller and more optimized snapshots of your sandbox images, making your build and deployment pipelines more efficient.
