---
schema_version: "1.0.0"
document_id: "38a172214608897f56e62a9e80bcf6f4290d9ab67b4f9d07903cf5cc97131c8b"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/go-remote-cache"
published_at: "2025-03-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:c93cfb83b4dd7520d11aa0446ec07fd8103a33e59986da7a57654cabe2e68631"
---

# Go 1.24 remote caching explained

At Depot, we’re obsessed with software performance, and to that end, we’re always looking for ways to improve build pipeline speed using strategies like caching.


When we heard that[external caching was getting added to the Go compiler in version 1.24](https://tip.golang.org/doc/go1.24#gocacheprog) , knowing that the Go compiler is already incredibly fast, we wondered: Could Go caching with Depot make it even faster?


Spoiler alert: It does. In this article, we’ll explain how Go caching used to work, how it works in Go version 1.24, and how you can utilize the feature with Depot.


[Use Depot Cache with Go to unlock faster builds with distributed remote caching that's shared with your entire team and CI environment. Try it free for 7 days →](https://depot.dev/sign-up)


## How did caching work in Go before?


Go has always had a great caching system. Before Go 1.24, the` go` command managed the build cache in the file system itself. The build outputs were put into a cache directory to reuse for future builds, and that caching system would account for changes to Go source files, compilers, compiler options, and so on.


However, the cache system only supported file system caching. File-system caching can be a performance problem in CI. The entire file system cache directory is saved and restored as a single compressed tar file in CI. The tar file could be so large that the restore and save could take longer than the build itself. I’ll describe this single tar file as “coarse-grained” caching because it contains every possible file. A coarse-grained cache can be a disadvantage if it contains more content than what you need.


## How caching works now in version 1.24


In version 1.24, the[Go caching system](https://pkg.go.dev/cmd/go/internal/cacheprog@master) can now be implemented externally. The environment variable` GOCACHEPROG` can be set to the name of a command that handles caching.


The Go compiler executes the program as a long running subprocess. The program implements a streaming JSON Lines protocol over stdin and stdout. Go asks the cache program to` PUT` and` GET` individual files. The compiler will use the cached files for building executables and tests.


Because the compiler runs an external cache program, that program can use any caching policy it wants. For example, the external program can use remote storage like that offered by[Depot Cache](https://depot.dev/docs/cache/overview) . Importantly, the cache program is “fine-grained” because it gets just the files a build needs. A fine-grained cache can be much faster than the typical coarse CI cache directory.


## Remote Go cache with Depot


We knew this Go change was coming, and so we did some engineering work so that Depot Cache could support remote Go cache.


[That functionality is live](https://depot.dev/docs/cache/integrations/gocache) , and it’s pretty dang easy to set up, which is something we strive for in every new feature and capability we add. All you have to do is set the environment variable` GOCACHEPROG` to` “depot gocache”` , and the Go compiler will` GET` and` PUT` cache entries to the Depot Cache global store.


### Okay, but is it *fast,* fast?


Yeah, it’s fast. The Go compiler is legendary for its speed in and of itself. We wanted to know how fast it is in a CI environment, though. So we simulated building a large Go project,[Tailscale](https://github.com/tailscale/tailscale) (in homage, we might add, to the primary author of` GOCACHEPROG` , Brad Fitzpatrick).


Without any cache, Tailscale and Tailscaled build in about one minute. With remote caching through Depot, however, we can shave 22% off that build time:


The remote cache even stores test and lint builds, so even they get a slight speed-up:


## How does` GOCACHEPROG` work?


The new Go cache system has a wonderful architecture that separates concerns. The Go compiler asks` GOCACHEPROG` for a cached file by sending a key, and the external cache program sends back a path to a file on disk. We particularly like this architecture because it doesn’t dictate how to get the data. Other remote cache systems require you to implement a memory-intensive gRPC network protocol with a multitude of configurable options. Because they require a specific network communication protocol, your design is dictated down to the very last byte.


The Go system, again, just asks for a key and returns a file path. Because it simply wants to read from that file path, it’s not dictating to you how to get the file, nor how to interact with the remote cache. You get to decide how to get and interact with the file.


Let’s take a deeper dive into how` GOCACHEPROG` works. First, some definitions used by the Go compiler:


- Action: the compiler executes an action that might be compiling a file, linking an executable, etc.
- [ActionID](https://github.com/golang/go/blob/dceee2e983f5dab65c3905ecf40e70e15cf41b7d/src/cmd/go/internal/work/exec.go#L241) : the compiler hashes the inputs to the action that produced the packages or binary to make an ActionID. The inputs could be the operating system, architecture, file, etc.
- Output: the action execution output.
- [OutputID](https://github.com/golang/go/blob/dceee2e983f5dab65c3905ecf40e70e15cf41b7d/src/cmd/go/internal/cache/prog.go#L311-L321) : the compiler hashes the content of the output to make an OutputID.


### Initialization


The compiler executes` GOCACHEPROG` as a subprocess that runs for the entire compilation process. The compiler and` GOCACHEPROG` communicate over` stdin` and` stdout` . Each command from the compiler and response from` GOCACHEPROG` are newline delimited JSON.


` GOCACHEPROG` initializes the protocol by sending the first message describing its capabilities. The first message sent to the Go compiler looks like this:


```text
{   "ID"  :   0  ,   "KnownCommands"  : [  "get"  ,   "put"  ,   "close"  ] }\n
```


This message tells the Go compiler that the` GOCACHEPROG` supports the “get”, “put” and “close” commands. Get means that the compiler can get cached files; put means the compiler can put cached files; and close means that the compiler will let` GOCACHEPROG` know when the compile has finished.


### Requests


After the` GOCACHEPROG` initial capabilities message, the Go compiler begins sending numbered “get” and “put” requests.


#### Gets


A` GET` request looks like this:


```text
{   "ID"  :   1  ,   "Command"  :   "get"  ,   "ActionID"  :   "<base64 ActionID>"   }\n
```


The` GOCACHEPROG` response ID ***must*** correspond with the compiler request ID. The` ActionID` is the cache key to find. If` GOCACHEPROG` has the` ActionID` in cache it should respond like this:


```text
{   "ID"  :   1  ,   "OutputID:"  <base64   OutputID>  ", "  Size  ":8, "  Time  ":"  <RFC3339>  ", "  DiskPath  ":"  <path>  " }\n
```


We’ll talk about` OutputID` in a moment when we talk about “put.” The` Size` is the size of the cached ActionID in bytes. The` Time` is an RFC 3339 formatted time when the ActionID was cached. Finally, and most importantly, the` DiskPath` is the file path where` GOCACHEPROG` wrote the content. For example, in[depot gocache](https://depot.dev/docs/cli/reference/container-builds#depot-gocache) we place the files in` $HOME/.cache/go-build` .


The compiler uses the content at` DiskPath` rather than compiling.


However, if` GOCACHEPROG` does not have` ActionID` , it should respond with a cache miss like this:


```text
{   "ID"  :   1  ,   "Miss"  :   true   }\n
```


#### Puts


The compiler’s` PUT` request is a bit more complicated because it has two lines. The first request line looks like:


```text
{   "ID"  :   2  ,   "Command"  :   "put"  ,   "ActionID"  :   "<base64 ActionID>"  ,   "OutputID"  :   "<base64 OutputID>"  ,   "BodySize"  :   128   }\n
```


And the second request line looks like this:


```text
"<base64 of file content>"  \n
```


The` OutputID` , as mentioned above, is the hash of the file content. Technically, we're supposed to treat the` OutputID` as opaque bytes, but (between us) it’s the SHA-256. It needs to be stored along with the file content and returned with the “get” response.


The second request line is a JSON string of the base64-encoded bytes of the file. Note that sometimes the size of the file is zero bytes and the second request line will be absent.


In` depot gocache` we set the cache key to the` ActionID` . The content of our cache key is the` OutputID` followed by the file content.


` GOCACHEPROG` responds to a` PUT` request like this:


```text
{   "ID"  :   2  ,   "DiskPath"  :   "<path>"   }\n
```


The` DiskPath` must contain the cache file content. In` depot gocache` we store the content both on the filesystem and in our global remote cache. We write to the global remote cache in a background go routine to reduce latency.


#### Close


When the Go compiler finishes compiling it will send a close command like this:


```text
{   "ID"  :   3  ,   "Command"  :   "close"   }\n
```


The compiler will wait indefinitely for` GOCACHEPROG` to exit, which allows` GOCACHEPROG` time to finish and clean up.


For` depot gocache` we wait ten seconds for any remaining` PUT` requests to the global remote cache to finish writing.


### Errors


` GOCACHEPROG` can respond to any request with an error. The compiler will print the error message and[continue](https://github.com/golang/go/blob/f77bba43aa223fc86fd223f3ea4ef60db8e0c583/src/cmd/go/internal/work/buildid.go#L428) to execute the action without cache. An error response looks like this:


```text
{   "ID"  :   1  ,   "Err"  :   "error message"   }\n
```


## Final Thoughts and the Future


The new external caching in Go 1.24 is really cool. It allows developers to tune caching, especially in CI environments. Our new` depot gocache` command can help reduce build times with minimal setup.


In the future I could imagine further optimizations where` depot gocache` could optimistically download the latest used cache. This way the files could already be available to the compiler before it even asks for them.


Give` depot gocache` a try! The only thing better than a fast Go compiler, after all, is a faster one 😉.


## Related posts


- [Introducing Depot Cache](https://depot.dev/blog/introducing-depot-cache)
- [Remote build caching: The secret to lightning-fast builds](https://depot.dev/blog/remote-build-caching-secret-to-software-builds)
- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)


Chris Goller


Principal Software Engineer at Depot
