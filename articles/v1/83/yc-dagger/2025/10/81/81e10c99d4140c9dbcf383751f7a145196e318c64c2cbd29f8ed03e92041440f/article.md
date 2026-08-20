---
schema_version: "1.0.0"
document_id: "81e10c99d4140c9dbcf383751f7a145196e318c64c2cbd29f8ed03e92041440f"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/dagger-0-19/"
published_at: "2025-10-01T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:1ee00133526a5aecc0885f3defeb39bdd3345e7ea802262dcbd4db132ce750a0"
---

# Dagger 0.19: performance, new APIs, build-an-agent!

We’re thrilled to be bringing the next big release of Dagger, version 0.19! This release makes working with containers smoother, debugging less painful, caching more intelligent, as well as a whole bundle of bug fixes and lots of polish. Dagger now plays even nicer with your host’s container runtime (so you can finally replace that last` docker build` if you like), and better supports for coding agents. Plus, we’ve been busying away behind the scenes, making lots of performance tweaks and caching improvements, which should already be giving you better performance (with more to come later)!


## Run Dagger without Docker


When running the Dagger CLI, the engine is automatically downloaded and run as an OCI image, bundling everything the runtime needed. In older Dagger versions, we only supported this automatic behavior when the user had` docker` installed.


In more recent versions of Dagger, we now officially support not only` docker` , but also` podman` ,` nerdctl` ,` finch` and Apple’s` container` tool. Now, dagger works out-of-the-box in more environments, using whatever container runtime you happen to have installed – no need for Docker if you don’t have it!


Find out more:


-


[https://docs.dagger.io/reference/container-runtimes/](https://docs.dagger.io/reference/container-runtimes/)


-


[https://github.com/dagger/dagger/pull/10714](https://github.com/dagger/dagger/pull/10714)


## Import and export local containers


In addition to pulling and pushing container images


We recently added support for loading and saving container images to and from the` Host` . Without this, getting images in and out of dagger was a bit of a pain, requiring lots of manual calls to` docker import` and` docker export` . However, as of 0.19, this interaction gets significantly easier – you can do it all from the dagger API!


For example, given the following function:


```text
// Build returns an alpine package with the given packages installed
func   (  mod   *  MyModule  )   Build  (  packages   []  string  )   *  dagger  .  Container   {
return   dag.  Container  ().
From  (  "alpine"  ).
WithExec  (  append  ([]  string  {  "apk"  ,   "add"  ,   "--"  }, packages  ...  ))
}
```


To build it and export it to the local container runtime, you can now just chain the` Container.exportImage` API onto the end of your` build` command:


```text
$   dagger   -m   my-module   call   build   --packages   build-base,git   export-image   --name=my-image
●   myModule:   MyModule!   0.0s
▶   .build  (  packages:   [  "build-base"  ,   "git"]  )  :   Container!   6.3s
▶   .exportImage  (  name:   "my-image"  )  :   Void!   17.2s
```


This will export the container as an image to the local container runtime! e.g. when using` docker` :


```text
$   docker   images
REPOSITORY                        TAG               IMAGE   ID         CREATED            SIZE
my-image                          latest            f1de8459fcf4     47   seconds   ago     272MB
```


You can also do the reverse, importing an image from the host into dagger using the` Host.containerImage` API, and then chain on for more operations. For example:


```text
$   dagger   core   host   container-image   --name   my-image   with-exec   --args   git,--version   stdout
●   host:   Host!   0.0s
▶   .containerImage  (  name:   "my-image"  )  :   Container!   2.9s   CACHED
▶   .withExec  (  args:   [  "git"  ,   "--version"]  )  :   Container!   0.2s
▶   .stdout:   String!   0.3s
git   version   2.49.1
```


You can learn more in our docs, see **WHERE** for more information on these new APIs.


(Needs[https://github.com/dagger/dagger/pull/11128](https://github.com/dagger/dagger/pull/11128) )


If you’re interested in the code changes, see where we implemented this:


- [https://github.com/dagger/dagger/pull/10662](https://github.com/dagger/dagger/pull/10662)
- [https://github.com/dagger/dagger/pull/10810](https://github.com/dagger/dagger/pull/10810)


## Changeset API


Dagger is now much better at managing generated files, such as docs or generated code, thanks to the new` Changeset` type. You can now represent *changes* *to a directory* as an artifact and return that artifact to the user. When calling a function returning a Changeset - for example your codegen function - the Dagger CLI will display the diff to the user, and prompt them to apply it to their local directory.


For example, you can now use Dagger to easily manage the lifetime of your generated go files. You can wrap a call to` go generate` in a simple dagger function.


```text
func   (  m   *  MyModule  )   Generate  (
// +defaultPath="./"
dir   *  dagger  .  Directory  ,
)   *  dagger  .  Changeset   {
golang   :=   dag.  Container  ().  From  (  "golang:latest"  )
generated   :=   golang.
WithWorkdir  (  "/src"  ).
WithMountedDirectory  (  "."  , dir).
WithExec  ([]  string  {  "go"  ,   "generate"  ,   "-v"  ,   "./..."  }).
Directory  (  "."  )
return   generated.  Changes  (dir)
}
```


If you call this function without chaining, you’ll be prompted to apply the changes into the current directory.


You can see the implementation added in:


- [https://github.com/dagger/dagger/pull/10946](https://github.com/dagger/dagger/pull/10946)
- [https://github.com/dagger/dagger/pull/11064](https://github.com/dagger/dagger/pull/11064)


## Build-an-agent


“Coding agents” are all the rage these days. But they’re not dark magic! In fact they are assembled from relatively simple building blocks: LLM client, tool calls, file edits, terminal interactions… What if those building blocks were available directly in the context of your existing workflows? Instead of building a new toolchain from scratch, you could simply augment your existing stack by sprinkling agentic behavior where it makes sense.


Well, that’s now possible with Dagger!


Dagger now provides all the building blocks for assembling custom AI agents, natively integrated in your workflows. This gives you the best of both worlds: the creativity and intelligence of LLMs, wrapped in the deterministic context and modularity of your Dagger environments.


For an example coding agent built on Dagger, check out[Doug](https://github.com/vito/daggerverse/blob/main/doug/main.go) , or[use it in your module](https://github.com/vito/dang/blob/991fc0a93f25f67fe1872fc897bcee0373e5a644/.dagger/main.dang#L72-L85) to tune a coding agent just for your project.


The key aspects are below:


- ` Env.withWorkspace` sets a workspace` Directory` to automatically propagate between tool calls
- ` LLM.withMCPServer` lets you install an MCP server into an LLM, whose tools inherit the environment’s workspace and propagate changes to subsequent tool calls
- ` Env.withModule` and` Env.withCurrentModule` installs a module into an LLM environment, providing tools that automatically run with the environment’s workspace
- A TUI sidebar now shows the workspace changes, with a Ctrl+S hotkey to save the agent’s changes locally


For a more detailed itinerary, a walkthrough of how to try it out, and some pre-recorded demos can be found in the following pull request:[https://github.com/dagger/dagger/pull/10907](https://github.com/dagger/dagger/pull/10907)


## API improvements


As well as all the above bigger features, we’ve been working on all sorts of little API features to make it easier to use Dagger and clean up some of our rougher edges.


To give just a few examples…


### Container.combinedOutput


It’s always been possible to grab the` stdout` or` stderr` of a container. However, sometimes, you just need both – as they were outputted.


Thankfully, you can grab that:


```text
$   dagger   shell   -c   'container | from alpine | with-exec ls,-l,/etc/fstab,/etc/non-existent --expect=any | combined-output'
ls:   /etc/non-existent:   No   such   file   or   directory
-rw-r--r--      1   root       root              89   Mar   25    2025   /etc/fstab
```


This gets the combined results of the two output streams, correctly interspersed as should be!


### address


The new` address` API includes the logic that *was* part of the CLI to parse flags. This logic was the bit that converted` ./path/to/my/file` into a` File` object, or a` alpine:latest` into a` Container` object. Now, instead of just having that logic live entirely in the CLI, you can hook into it using the` address` API directly!


```text
$   dagger   shell
▶   address   ./path/to/my/file   |   file
File@xxh3:60730cfe39a05dc6
▶   address   alpine:latest   |   container
Container@xxh3:134738b0cc077845
```


### Cloud.traceURL


When running against[https://dagger.cloud](https://dagger.cloud/) to generate easily viewable logs and traces from a run, you might want to get the current run.


Now, you can!


```text
func   (  m   *  MyModule  )   Report  (  ctx   context  .  Context  ) (  string  ,   error  ) {
currentURL, err   :=   dag.  Cloud  ().  TraceURL  (ctx)
if   err   !=   nil   {
return   ""  , err
}
currentTime   :=   time.  Now  ().  Format  (time.RFC1123)
report   :=   strings.  Join  ([]  string  {
"MyModule Report"  ,
"================="  ,
fmt.  Sprintf  (  "Generated at:   %s  "  , currentTime),
fmt.  Sprintf  (  "Trace URL:   %s  "  , currentURL),
},   "  \n  "  )
return   report,   nil
}
```


When you run this, you can generate a report that contains a link to Dagger Cloud with the logs for itself:


```text
$   dagger   call   report   >   out.md
$   cat   out.md
MyModule   Report
=================
Generated   at:   Mon,   29   Sep   2025   15:47:27   UTC
Trace   URL:   https://dagger.cloud/dagger/traces/43131b8527d32966c9c908206c4e633e
```


### More GitRepository methods


There’s a lot more` GitRepository` methods, to make it easier to work with git repos!


You can easily track where a` GitRepository` type came from (which can be quite useful, since we now allow generic schemeless URLs like` \[github.com/dagger/dagger\](http://github.com/dagger/dagger)` in more cases!)


```text
$   dagger   shell
▶   dagger=  $(  git   github.com/dagger/dagger  )
▶   $dagger   |   url
https://github.com/dagger/dagger
As   well   as   getting   the   repo   tags,   you   can   now   easily   get   all   the   branches:
▶   $dagger   |   branches
main
```


You can also get the latest tagged version in a repo – in case you always want the latest version, but don’t want to track every commit on a branch:


```text
▶ $dagger | latest-version | ref 0.1s
refs/tags/v0.19.0
```


If you have multiple refs, you can also compute the` commonAncestor` of those two refs, which computes the most recent commit that both of the two refs share!


```text
▶ $dagger | latest-version | common-ancestor $($dagger | head) | commit
4c0989b202a02fcee68c9f983f7f1671c6c397b8
```


A lot of this is possible due to loads of improvements in the underlying git implementation – generally the entire thing should just be more performant and cache better out-of-the-box!


## What’s Next?


v0.19.0 is already out, so you can give it a go today by following our[installation instructions](https://docs.dagger.io/getting-started/installation) .


Your feedback helps us prioritize features and fixes, so keep it coming! We read every message on[Discord](https://discord.com/invite/dagger) and every PR and issue on our[GitHub](https://github.com/dagger/dagger) . We’re already hard at work on the next release, and we can’t wait to share what’s dropping next!
