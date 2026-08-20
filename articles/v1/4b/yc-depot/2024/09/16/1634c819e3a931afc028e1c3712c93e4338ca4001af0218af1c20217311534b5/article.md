---
schema_version: "1.0.0"
document_id: "1634c819e3a931afc028e1c3712c93e4338ca4001af0218af1c20217311534b5"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/docker-system-prune"
published_at: "2024-09-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:b531164ccc0d2321ee2fd0a4c68a2cdd53d56de51d587f776a333eda74783d9b"
---

# Tidy up with docker system prune

If you've started using Docker, you've inevitably become familiar with one of its biggest annoyances: clutter.


Docker can accumulate many unused containers, images, networks, and volumes over time, which can consume valuable disk space and potentially impact your machine's performance. Luckily, there is a command to help with all that:` docker system prune` .


In this post, we will explore the basic usage of` docker system prune` , explore some of its more advanced options, and discuss best practices for keeping your Docker environment clean and efficient.


## Understanding` docker system prune`


The` docker system prune` command is meant to remove all unused containers, networks, images, and even volumes. It's kind of a one-stop shop for nuking those bulky Docker artifacts chewing through your disk space.


When you run` docker system prune` , Docker will prompt you with a warning message listing the types of objects that will be removed. These include:


1. All stopped containers
2. All networks not used by at least one container
3. All dangling images (those not associated with a tagged image)
4. Any unused build cache


One thing to note is that Docker will not remove volumes by default. This is to prevent accidental deletion of important data. If you want to remove volumes as well, you can use the` --volumes` flag; more on that later.


## Basic usage


To use Docker system prune, simply open your terminal and run:


```text
$   docker   system   prune
WARNING!   This   will   remove:
-   all   stopped   containers
-   all   networks   not   used   by   at   least   one   container
-   all   dangling   images
-   unused   build   cache


Are   you   sure   you   want   to   continue?   [y/N] y
Deleted   Containers:
5e01927539ffdf34cbe5cb7a6e7989f8aaf32f0cd6eadcbfa606c46790eb9816
2656336a214305b2541338729d6a5563fdf4fd1653a52b3f494c4dfb3471617d
7586782d5ccf8d34fd691b88a00bfe9b856ee96142b36c4aa836f5518afada88


Deleted   Images:
untagged:   sha256:35d7c8ede3d7636904c9612026ee01985928f6caec9f415a6bb64eb209aa406c
deleted:   sha256:35d7c8ede3d7636904c9612026ee01985928f6caec9f415a6bb64eb209aa406c
deleted:   sha256:737a86ecfaf48a0b54af0740a05fa4a1c199c302f56649919bd0113dcecf9826


Deleted   build   cache   objects:
6ptijmtda1f6kn4tz6miod6v9
leiz3wvdbhjwax0iekmzjevju
tubu9t4eia2w3pcln4aody1qj


Total   reclaimed   space:   8.566GB
```


The output above shows that` docker system prune` has deleted all of my stopped containers, cleaned up some dangling images and removed some unused build cache. It also tells me how much disk space I've reclaimed, a rather astonishing 8.56 GB in this case.


### More advanced options


While the basic command is useful,` docker system prune` offers several options to customize its behavior:


1.


` -a` or` --all` : This flag extends the prune operation to include all unused images, not just dangling ones. It's particularly useful when you want to perform a more thorough cleanup. Reminder: this will remove all unused images, not just ones that are not associated with a tag.


2.


` --volumes` : By default, volumes are not removed to prevent accidental deletion of important data. Using this flag will also remove volumes not used by any containers.


3.


` -f` or` --force` : This option bypasses the confirmation prompt, allowing for automated cleanup in scripts or CI/CD pipelines.


4.


` --filter` : Introduced in API version 1.28, this option allows you to provide filter values to target specific objects for removal. We will explore this flag further later.


Let's look at an example of a more comprehensive cleanup:


```text
docker   system   prune   -a   --volumes
```


This command will remove all unused containers, networks, images (including non-dangling ones), and anonymous volumes. It's a powerful way to reset your Docker environment to a clean slate, but use it with caution, as any data stored on a volume will be lost.


### How to use filtering in the prune command


The` --filter` option provides granular control over what gets removed. You can filter based on two main criteria,` until` and` label` . Here's how they work:


```text
docker   system   prune   --filter   "until=1h30m"
```


Using the` until` filter, you can remove objects created before a specified timestamp. It accepts Unix timestamps, date-formatted timestamps, or Go duration strings (e.g., "10m" for 10 minutes ago) computed relative to the machine's time.


```text
docker   system   prune   --filter   "label=env=testing"
```


With the` label` filter, you can target objects with specific labels. You can use it to remove or keep objects based on their labels. In the example above, we ask to remove all objects with the label` env=testing` . But you can also use it to keep objects with a specific label:


```text
docker   system   prune   --filter   "label!=env=testing"
```


This command will remove all objects that do not have the label` env=testing` .


## Best Practices and other considerations


The` docker system prune` command is a powerful tool, but you should use it carefully to avoid unintended data loss. Here are some best practices to keep in mind:


1.


Incorporate` docker system prune` into your regular maintenance routine to prevent resource buildup.


2.


Before running a system-wide prune, ensure you've backed up any critical data, especially when using the` --volumes` flag.


3.


For more targeted cleanup, consider using specific prune commands like[docker image prune](https://depot.dev/blog/docker-clear-cache#removing-images) or[docker container prune](https://depot.dev/blog/docker-clear-cache#removing-containers-from-the-docker-cache) instead of system prune.


4.


Use` docker system df` to monitor Docker's disk usage and determine when it needs to be cleaned up.


5.


The` --all` flag can remove images you might need later. Use it sparingly unless you're sure you want to remove all unused images.


## Conclusion


Docker system prune is a valuable tool to keep in your back pocket or in an alias somewhere. It simplifies the process of maintaining a clean Docker environment by reclaiming disk space that has been hogged by artifacts.


There are also some nice options, like` --filter` to` docker system prune` , allowing you to customize which resources you want to be cleaned up. Giving you more control instead of having to lean on the all-or-nothing approach.


## Related posts


- [How to clear Docker cache and free up space on your system](https://depot.dev/blog/docker-clear-cache)
- [Building and Caching Docker Images in Bitbucket Pipelines](https://depot.dev/blog/bitbucket-pipelines-docker-build-limitations)
- [How Depot speeds up Docker builds](https://depot.dev/blog/depot-magic-explained)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
