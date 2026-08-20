---
schema_version: "1.0.0"
document_id: "9ea7e755602df3b2d8622cb1bf56f90d34dc59af3406512d0d63eb86e0d984e4"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/easy-cross-platform-cgo-builds/"
published_at: "2025-04-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:cdcfe233e3a928804a0952d54e361738f45063624237af3e0324adcb507eb786"
---

# Easy Cross-Platform cgo Builds

When I first started writing Go software a little over a decade ago, one of the features I found particularly intriguing was the ability to build statically-linked binaries for multiple operating systems and architectures without a lot of headache. This build toolchain feature is widely relied upon by nearly all Go developers, especially when needing to build multi-arch container images destined to be run in a Kubernetes cluster consisting of **amd64** and/or **arm64** nodes.


Things become a bit more challenging when your application requires cgo and you still have to run your application on multiple architectures. Your previously simple build process of interchanging **GOOS** and **GOARCH** environment variables to match your target environment are now complicated with the introduction of having to use various cross-compilation toolchains and the need to add support for different platforms. But regardless of whether you’ve run into this before or you’re dealing with it for the first time, I have some good news for you: it doesn’t have to be the arduous experience that it’s made out to be. Using a combination of tools and techniques outlined in this article, including docker multi-platform builds and a utility docker image **tonistiigi/xx** , you’ll be able to greatly simplify your cross-platform builds easily and with practically little-to-no effort.


## Introduction to cgo Builds


cgo builds enable developers to create Go packages that call C code, allowing for seamless integration between the two languages. This feature is particularly useful for multi-platform development, where a single codebase can be used to support multiple platforms, including mobile, desktop, and web. By leveraging **cgo** , developers can tap into the vast ecosystem of C libraries and frameworks, expanding the capabilities of their Go apps. For example, a developer can use **cgo** library and access its features from within their Go code, making it easier to build cross-platform apps. This integration not only enhances the functionality of the application but also ensures that it can run efficiently on various platforms without significant modifications to the codebase.


## Addressing the cgo Elephant in the Room


An oft-quoted Go-ism is “cgo is not Go”, referencing a[2016 post](https://dave.cheney.net/2016/01/18/cgo-is-not-go) Dave Cheney wrote on his blog in which he outlined the problems of introducing **cgo** (i.e. **import** **“C”** ) into your Go applications. It is certainly salient and valuable advice for any professional Go developer, especially for those that are relatively new to Go. If you haven’t read the article, I encourage you to do so. I won’t go into all of the potential merits and drawbacks of **cgo** , but I will say that while it is advised to avoid **cgo** , in some instances it is simply unavoidable. For example:


1. No Go-native implementation exists.
2. A Go-native implementation does exist, but it lacks widespread adoption and/or can’t be considered stable enough for production use.
3. A stable, production-ready Go-native implementation does exist, but the project on which you are working is already using a cgo implementation and it would be prohibitively expensive or risky to change.


To work with **cgo** , developers need to understand the requirements for building and running **cgo** packages, including the added complexity of managing project configurations when additional platforms are included. This involves adapting code and ensuring that platform-specific APIs and frameworks are conditionally accessed depending on the platforms that have been added.


## Understanding cgo Requirements


To work with **cgo** , developers need to understand the requirements for building and running **cgo** packages. This includes setting up the necessary tools and dependencies, such as the Go toolchain and C compilers. Additionally, developers must ensure that their source files are properly configured to work with **cgo** , including the use of special comments and directives. By understanding these requirements, developers can create efficient and effective **cgo** builds that support their multi-platform development needs. Note that **cgo** supports various C libraries and frameworks, making it a powerful tool for building apps that require platform-specific features. Properly setting up the environment and understanding the intricacies of **cgo** can significantly streamline the development process and reduce potential issues during the build.


## An Example Application


Let’s start with a simple, albeit contrived, example: a “process info reporter” application that produces messages to a Kafka topic. In some cases, specific handling is required for certain C types that differ from standard pointer representations. Once every 5 seconds, the application will format a string containing the current time, the process id, and user id and send it to a topic named **info** . Granted there are pure-go Kafka client implementations, but for the sake of demonstration, I’ll be using a package that makes use of **librdkafka** to perform client work: **github.com/confluentinc/confluent-kafka-go** . I will also be using the following constraints:


1. All **go build** operations happen in an explicit stage in the project’s **Dockerfile** in order for builds to be consistent regardless of where they occur (e.g. a developer’s local machine, CI executor, etc).
2. The resulting application container will run in Kubernetes on either linux/amd64 or linux/arm64 nodes, so builds should produce multi-arch builds for both.


```text
package   main


import   (
"  fmt  "
"  log  "
"  os  "
"  os/user  "
"  time  "


"  github.com/confluentinc/confluent-kafka-go/v2/kafka  "
)


func   main  () {
topic   :=   "info"
brokerAddress   :=   os.  Getenv  (  "KAFKA_HOST"  )   +   ":9092"


p, err   :=   kafka.  NewProducer  (  &  kafka  .  ConfigMap  {  "bootstrap.servers"  : brokerAddress})
if   err   !=   nil   {
log.  Fatalf  (  "Failed to create producer:   %s\\\\  n"  , err)
}
defer   p.  Close  ()


// Go-routine to handle delivery reports of produced messages
go   func  () {
for   e   :=   range   p.  Events  () {
switch   ev   :=   e.(  type  ) {
case   *  kafka  .  Message  :
if   ev.TopicPartition.Error   !=   nil   {
log.  Printf  (  "Delivery failed:   %v\\\\  n"  , ev.TopicPartition)
}   else   {
// log.Printf("Delivered message to %v\\\\n", ev.TopicPartition)
}
}
}
}()


pid   :=   os.  Getpid  ()
usr, err   :=   user.  Current  ()
uid   :=   "unknown"
if   err   ==   nil   {
uid   =   usr.Uid
}


for   range   time.  Tick  (  5   *   time.Second) {
currentTime   :=   time.  Now  ()


messageValue   :=   fmt.  Sprintf  (  "Time:   %s  , PID:   %d  , UID:   %s  "  ,
currentTime.  Format  (time.RFC3339),
pid,
uid,
)


// Produce messages to topic (asynchronously)
err   =   p.  Produce  (  &  kafka  .  Message  {
TopicPartition:   kafka  .  TopicPartition  {Topic:   &  topic, Partition: kafka.PartitionAny},
Value:          []  byte  (messageValue),
Timestamp:      currentTime,
},   nil  )


if   err   !=   nil   {
log.  Fatalf  (  "Failed to produce message:   %v\\\\  n"  , err)
}


log.  Printf  (  "Successfully attempted to publish message to topic   %s  "  , topic)
}
}
```


A naive Dockerfile like the following would be fine if all we needed to support were Kubernetes nodes that matched our own system’s architecture:


```text
FROM golang:1.24 AS build


WORKDIR /app
COPY . .
RUN apt update && apt install -y --no-install-recommends librdkafka-dev
RUN CGO_ENABLED=1 go build -o /app/my-kafka-app


FROM scratch


COPY --from=build /app/my-kafka-app /app/my-kafka-app
ENTRYPOINT ["/app/my-kafka-app"]
```


Once we start adding things to adjust this unimposing Dockerfile to include checks for architecture, installing and setting up cross-platform toolchains, and configuring various environment variables for cross-compilation like CC, CXX, etc., things start to beome a little murky a frustratingly tedious problem to solve, ending up with either an excessively complex Dockerfile or a lot of utility scripts, both of which involving a decent amount of branch logic distinguish between the build platform and the target platform. So how do we move forward? Enter: tonistiigi/xx.


## Configuring the Build Process


Configuring the build process for cgo packages involves specifying the necessary flags, options, and dependencies. Developers can use the cgo command to compile and link C code with Go code, creating a single executable file. The build process can be customized to support different platforms, including iOS, macOS, and Windows. By adjusting the build settings, developers can optimize the performance and compatibility of their cgo packages, ensuring that they run smoothly on multiple platforms. For instance, developers can use the \`\`\` -ldflags option to specify the libraries and frameworks required by their app. This level of customization allows for fine-tuning the build process to meet the specific needs of the application and the target platforms.


## Simplify Everything with tonistiigi/xx


The tonistiigi/xx docker image is a project I happened to stumble across some time ago and it was precisely the solution to the problem I was dealing with. So what is it? From the[GitHub repo](https://github.com/tonistiigi/xx) :


> xx provides tools to support cross-compilation from Dockerfiles that understand the –platform flag passed in from docker build or docker buildx build. These helpers allow you to build multi-platform images from any architecture into any architecture supported by your compiler with native performance. Adding xx to your Dockerfile should only need minimal updates and should not require custom conditions for specific architectures.


In essence, it removes the need for pretty much all of the complexities I previously described in trying to handle differences in build and target platform, allowing for seamless changes to application layouts and functionalities. It provides a number of different utilities that wrap various commands like go, apt, and apk in a way that automatically configures how your intended command should run based on both your build platform and your target platform. And it does all of this with an impressively lightweight build image dependency.


With this in mind, let’s revisit our Dockerfile. Just a couple of minor modifications are necessary. But first, something to keep in mind is the need for emulation.


## Emulation


Docker BuildKit builders have their usage constrained to a list of platforms that can accurately be emulated. To check this, run the following command:


```text
docker   buildx   ls
```


You should see at least one name/node combination, as well as the platforms it supports, like the following:


```text
NAME/NODE        DRIVER/ENDPOINT                   STATUS    BUILDKIT   PLATFORMS
multiarch*       docker-container
\\\\_ multiarch0    \\\\_ unix:///var/run/docker.sock   running   v0.20.2    linux/amd64 (+2), linux/arm64, linux/arm (+2), linux/ppc64le, (3 more)
```


If you don’t already have emulation configured, you will likely only see your current platform in the platforms list. However, this is not a difficult process and only needs to be performed once. Follow the instructions to setup[binfmt emulators](https://github.com/tonistiigi/binfmt) , which is also covered in the[Docker multi-platform documentation](https://docs.docker.com/build/building/multi-platform/#qemu) .


If your app targets do not share much code or configuration, it is advisable to continue using separate targets to maintain clarity and manageability.


Revisiting our modified **Dockerfile** , a few modifications to the original gets us a working image build for both **linux/arm64** and **linux/amd64** nodes.


```text
FROM --platform=$BUILDPLATFORM tonistiigi/xx AS xx


FROM --platform=$BUILDPLATFORM golang:1.24 AS build


WORKDIR /app


COPY --from=xx / /
COPY . .


RUN apt update && \\\\
apt install -y --no-install-recommends clang && \\\\
xx-apt install -y --no-install-recommends librdkafka-dev xx-c-essentials xx-cxx-essentials


RUN CGO_ENABLED=1 xx-go build -o /app/my-kafka-app


FROM scratch


COPY --from=build /app/my-kafka-app /app/my-kafka-app


ENTRYPOINT ["/app/my-kafka-app"]
```


That’s it. No reconciling the differences between your build and target platform, no extra packages, no tricky configuration to get right. All you need now is to build it:


```text
docker   buildx   build   --platform   linux/amd64,linux/arm64   -t   example.com/my-app:latest   -f   Dockerfile   --output   type=image,push=  false   .
```


Although this isn’t much of a change, let’s take a moment to break this down into the two primary modifications. First, we are wanting our build stage to have all of the **xx** tooling available to us so we can use them to either install dependencies or build our applications. This is handled by the two lines **FROM –platform=$BUILDPLATFORM tonistiigi/xx AS xx** and **COPY –from=xx / /** . Next, we need a couple of dependencies installed. clang will function as our compiler and installed with the platform-native apt program. After that, we need any cross-compilation dependencies or toolchains installed. Luckly, **xx** provides two metapackages for apt that do all of this for you: **xx-c-essentials** and **xx-cxx-essentials** . And if you’re not using **cgo** you can skip this step altogether.


## Working with Source File Dependencies


When working with **cgo** , it’s essential to manage source file dependencies effectively. This includes understanding how to import C code into Go source files, as well as how to handle dependencies between multiple source files. By using the **import “C”** statement, developers can access C functions and variables from within their Go code, making it easier to build complex apps. Additionally, cgo provides features like automatic dependency tracking, which helps to ensure that all necessary files are included in the build process. For directive to specify the dependencies required by their C code. Proper management of these dependencies is crucial for maintaining a clean and efficient build process, especially in larger projects with multiple source files.


## Optimizing Build Speed


Optimizing build speed is crucial for efficient multi-platform development with **cgo** . By using techniques like caching, parallel processing, and optimized compiler flags, developers can significantly reduce the time it takes to build and test their **cgo** packages. Additionally, **cgo** provides features like incremental builds, which allow developers to rebuild only the parts of the code that have changed, rather than rebuilding the entire project. By leveraging these features, developers can streamline their development workflow, making it easier to bring their apps to market faster. For instance, developers can use the **-cache** flag to enable caching and reduce build times. These optimizations not only save time but also improve the overall efficiency of the development process, allowing for quicker iterations and faster delivery of updates.


## Alternatives


This is, of course, not the only approach to dealing with some of the sharp corners associated with cross-platform **cgo** builds (or cross-platform builds in general). Most notably, the **goreleaser** (and **goreleaser-cross** ) project aims to solve the same problem, but it also tries to solve the complexities of “release engineering” as a whole (i.e. building, packaging, and publishing). It does this with declarative **yaml** in a **.goreleaser.yml** file. Notably, it is well suited for projects that not only need to compile applications for a larger set of platforms, but also need to distribute that software via numerous distribution channels like Docker images, DMG files, Linux packages (e.g. .deb and .rpm), and MSI executables. If being able to perform these kinds of tasks with declarative yaml configuration is a requirement, then **goreleaser** is likely the better choice than **xx** .


However, if your particular use case involves handling data across different platforms, then **goreleaser** will most certainly be more than you actually need.


Also, while **goreleaser** strives to be more of a “batteries included” experience, it still comes with the cross-platform tooling requirements we had to use with **xx** . The **goreleaser/goreleaser** docker image doesn’t include these by default, so you must either install and configure them yourself, or use a docker image that already contains them, like **goreleaser/goreleaser-cross** .


Both of these options come with a hidden cost: the overall size of the image. The **xx** base image introduces a negligible size overhead of about 60KB, compared to the roughly 900MB **goreleaser/goreleaser** image. Note that this isn’t accounting for any necessary toolchains that need to be installed. Opting to use the **goreleaser/goreleaser-cross** image to avoid toolchain setup balloons the dependency image size to a *staggering* 7.9GB, depending on the specific configurations required.


## What about multi platform darwin and windows?


This article was primarily focused on cross-platform *container* builds, specifically to be run on Kubernetes linux nodes, so native **darwin** and **windows** builds were intentionally left out. The **xx** project does have some support for both of these target platforms, but with some pretty strict limitations, making it essential to find the right packages and components for your specific needs. So while possible, it may require more time and effort than you may be willing to commit to. Alternatively, this may be a scenario where **goreleaser** may be more beneficial to you.


## Conclusion


Shipping cross-platform software might as well be considered a hard requirement these days, and having a robust framework to support these builds is crucial. Understanding what is happening during the build process can help developers troubleshoot and optimize their applications more effectively. Go applications that require **cgo** come with the additional hurdle of having to setup all of the necessary toolchains in order for them to compile and build properly. This typically becomes an operational pain point, one that can be simplified by using the lightweight **tonistiigi/xx** docker image as a dependency. And while there are alternatives available that work just as well, **xx** can be a much more valuable solution for Go developers that want something exeedingly simple and lightweight.
