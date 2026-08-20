---
schema_version: "1.0.0"
document_id: "18eef96fac587a0f94e310b43c3478c247eb4e71e8c5f30e556434a609456afc"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-03-02-whywechosego/"
published_at: "2020-03-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:b964902d3bbe30dc51e43a4aa3d92b3a26d4a27ff2a651bf1a97686dddf565e3"
---

# Why We Chose Go

To the outside, trivago appears to be one single software product providing our popular hotel meta search. Behind the scenes, however, it is home to dozens of projects and tools to support it. Teams are encouraged to choose the programming languages and frameworks that will get the job done best. Only few restrictions are placed on the teams in these decisions, primarily long-term maintainability. As a result, trivago has a largely polyglot code base that fosters creativity and diverse thinking. It allows us to make informed decisions based on actual requirements rather than legacy code or antiquated projects.


A few months ago, the opportunity to work on a greenfield project presented itself. With the aim of improving the user experience across multiple sessions, the project “Recent Searches” was launched. The task was to develop a[gRPC](https://grpc.io/) service handling requests from the front-end to store, retrieve and aggregate recent searches of a logged-in user. Part of the task is to run the service in Kubernetes and to authenticate incoming requests against our trivago[OAuth2](https://tools.ietf.org/html/rfc6749) authentication server. Our team already had plenty of prior experience with comparable projects in a similar environment using JVM languages like Java or Kotlin. This time, however, we chose Go. and here is why.


## Race Detector


Running a user-facing service at trivago means handling potentially thousands of incoming requests simultaneously. Moreover, being exposed to the open internet absolutely requires proper management of[timeouts](https://blog.cloudflare.com/exposing-go-on-the-internet/) and shared resources. For both points, we were very certain that we can rely on Go’s excellent built-in support for concurrency:


- [Timing out, moving on](https://blog.golang.org/go-concurrency-patterns-timing-out-and)
- [Pipelines and cancellation](https://blog.golang.org/pipelines)
- [Context](https://blog.golang.org/context)


With concurrent requests and access to shared resources being the norm, mistakes can happen. In a similar project before, our integration tests randomly started failing with varying results after we decided to run them in parallel. The error pattern was pointing to a possible race condition and upon inspection, we were quickly able to find it. Can you spot a potential problem in the following simplified version of the source code when you consider this class to be a[singleton](https://en.wikipedia.org/wiki/Singleton_pattern) or a shared resource between requests?


```text
public   class   Service   {
private   String value;


public   handle  (Request   req  ) {
this  .value  :   req.stringField;
System.out.  println  (  this  .value);
}
}
```


The` handle` method stores data of an individual request in the` value` field of the class instance and uses it again later. If another request comes in during that time, the[data race](https://en.wikipedia.org/wiki/Race_condition#Data_race) occurs, and the behavior is undefined. Fortunately, we found this issue before a user did, but we don’t want to leave that to chance. Let’s have a look at the same example in Go:


```text
type   Service   struct   {
value   string
}


func   (s   *  Service)   handle  (req Request) {
s.value: req.stringField
fmt.  Println  (s.value)
}
```


The Go code exhibits the same behavior as the Java code and is therefore prone to a data race. This is where Go’s race detector comes into play that was introduced in 2013. A new and ordinary build flag` -race` now allows data race detection to be enabled:


> The compiler instruments all memory accesses with code that records when and how the memory was accessed, while the runtime library watches for unsynchronized accesses to shared variables.
>
>
> —[Introducing the Go Race Detector](https://blog.golang.org/race-detector)


When compiling the above example using this flag, handling two concurrent requests will cause the following warning to be printed, and directly point us to the problematic code:


```text
==================
WARNING:   DATA   RACE
Write   at   0x00c0000901e0   by   goroutine   7  :
main.(*Service  )  .handle  ()
<module-path>@/cmd/test/main.go:16 +0x3e


Previous write at 0x00c0000901e0 by main goroutine:
main.(  *  Service)  .handle  ()
<module-path>@/cmd/test/main.go:16 +0x3e
main.main()
<  module-path>@/cmd/test/main.go:8   +0xc3


Goroutine   7   (running) created at:
main.main  ()
<module-path>@/cmd/test/main.go:7 +0xa0
==================
Found 1 data race(  s  )
```


## Statically-Linked Binaries


From the[Go FAQ](https://golang.org/doc/faq) :


> The linker in the` gc` toolchain creates statically-linked binaries by default. All Go binaries therefore include the Go runtime, along with the run-time type information necessary to support dynamic type checks, reflection, and even panic-time stack traces.
>
>
> (Here,` gc` refers to the Go compiler, not the garbage collector)


Compared to languages like Python or Java, running a binary compiled with Go does not require a matching version of an interpreter or a virtual machine. By additionally disallowing packages to call C code ([cgo](https://golang.org/cmd/cgo/) ), we can create statically-linked binaries without any runtime dependencies. This gives us the opportunity to take our Docker build process one step further. Instead of using a parent image like` debian:stable-slim` or` alpine` , we can create our image directly` FROM scratch` :


```text
# This is a minimal example to demonstrate the 'FROM scratch' usage.
# It does not include steps to create and use a non-privileged user,
# add root certificates and time zone data, or perform other checks.
FROM   golang  :1.13.8 as build
WORKDIR /build
COPY . .
RUN CGO_ENABLED=0 go build ./cmd/my-tool


FROM scratch
COPY --from=build /build/my-tool /entrypoint
ENTRYPOINT ["/entrypoint"]
```


This almost shrinks the size of the Docker image (20 MB) to the size of our application (18 MB). For comparison, a minimal Docker image for Java like` openjdk:8-jre-alpine` or` gcr.io/distroless/java:8` already weighs between 85 MB and 125 MB by itself. Not requiring an interpreter or a virtual machine also means that the image has basically no[startup](https://github.com/bdrung/startup-time)[time](https://github.com/bdrung/startup-time) . Given our requirement to run the service in Kubernetes, a small image and a low startup time are very desirable because they allow us to deploy and auto-scale quickly.


## ` go fmt`


Spacing and brace position are arguably two of the most controversial topics in debates around software engineering. Unless you are using a language where[“semantics depend on invisible characters”](https://www.youtube.com/watch?v=VoS7DsT1rdM&feature=youtu.be&t=1263) , they are essentially subject to personal style and have no influence on the correctness or performance of the code. Go ships with a very opinionated source code formatter. Here is an example:


```text
if   err  !=  nil   {  return   err}
```


```text
$   go   fmt
example.go
```


```text
if   err   !=   nil   {
return   err
}
```


Tools that automatically format source code exist for pretty much every language, so why mention it as a reason to choose Go over any other of them? Besides, “What if I like the first version better? For sure there must be a setting to configure the behavior?“. I will attempt to answer both questions using a quote by Rob Pike from his[talk at Gopherfest](https://www.youtube.com/watch?v=PAAkCSZUG1c) about the[Go Proverbs](https://go-proverbs.github.io/) .


> A lot of people, especially beginners, say “I wanna move the braces”, or “Why tabs instead of spaces”, or whatever. Who cares, shut up![Gofmt](https://golang.org/cmd/gofmt/) ’s style is nobody’s favorite. The way that it formats isn’t even the way Robert Griesemer likes code to look, and he wrote the program.
>
>
> — Go Proverbs - Rob Pike - Gopherfest - November 18, 2015


The point is, you can spend days, weeks and probably months, trying to find a coding style that everybody agrees on, and still fail. Having a code formatter integrated into the toolchain as opposed to an external one prevents a lot of meaningless bikeshedding. And that is why we love to[go fmt our code](https://blog.golang.org/go-fmt-your-code) and focus on functionality instead.


## Conclusion


Go has proven to be a good fit for our microservice, but it isn’t the only one. Rust is another modern language that supports statically-linked binaries, and where[“data races are mostly prevented through Rust’s ownership system”](https://doc.rust-lang.org/nomicon/races.html) . This means they will be caught at compile time, not just at runtime. However, Go’s simplicity and its sophisticated tooling let us scale not only our service but more importantly,[the process of software engineering itself](https://talks.golang.org/2012/splash.article) . Reducing the friction of onboarding and training someone has a significant impact on the company’s productivity, even more so in a constantly moving environment like trivago.


And that is why we chose Go.
