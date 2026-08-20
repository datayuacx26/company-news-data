---
schema_version: "1.0.0"
document_id: "782dfb2b705d6de30ceac7adaade7668f1cdda8841050e789eb1f95ca1e944c4"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/opentelemetry-and-go-a-whole-new-ebpf-world/"
published_at: "2023-08-10T04:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:33843f14ac96155fd3f03252309ad928a6ffdf018d9792f741105d643c96ace5"
---

# OpenTelemetry and Go: a whole new eBPF world

In major news from the OpenTelemetry Go project, the inclusion of eBPF techniques has allowed the addition of automatic instrumentation for Go services using OpenTelemetry. Previously, Go had serious limitations when it came to automatically instrumenting your applications, that limited the reach of the OpenTelemetry Go project. A guide that promised ‘automatic instrumentation’ of a Go application still ended up[requiring some editing of the application code](https://utsavbatra5.medium.com/automatic-instrumentation-of-a-go-application-using-opentelemetry-4c1b0e4c98a) .


It might seem minor to ask for a single call to be added at the start of a new transaction. But if you’re reading this blog you’re probably working on a team and with an architecture of some scale. While manually adding instrumentation works fine for a demo app or a starter monolith, often the operations team tasked with adding observability on a large microservice architecture won’t even have access to edit application code. And getting dozens or hundreds of coders to add ‘one little call’ every time a transaction starts is an organizational nightmare.No, the ideal is something like the experience with the OpenTelemetry Kubernetes Operator, where operations can add a service to the cluster, feed each pod some config, and start monitoring their services without involving developers at all. In Java-land, this is possible!


Why can’t we achieve in Go the same ‘truly automatic’ process we have with the Java OpenTelemetry sdk? The issue is bytecode manipulation. Java code is compiled to bytecode which is interpreted, and that bytecode can be patched with instrumentation calls. This isn’t a ‘hack’ of Java, it’s explicitly[enabled by the javaagent spec](https://blogs.sap.com/2016/03/09/java-bytecode-instrumentation-using-agent-breaking-into-java-application-at-runtime/) . This bytecode patching isn’t supported in Go, so up until very recently there was no way to use OpenTelemetry with Go without doing at least some Go code edits.


## Enter eBPF


eBPF (extended Berkeley Packet Filter) is a powerful technology that allows for the dynamic modification of kernel code in real time. By using eBPF, it is possible to monitor and analyze network traffic, system calls, and other kernel events. One of the most important features of eBPF is the ability to access user code and variables by analyzing the stack and CPU registers. This feature enables the development of powerful and flexible instrumentation that can be used to monitor and troubleshoot complex systems. Again this isn’t a hack, eBPF’s main application is for adding instrumentation.


The same use case for eBPF makes projects like[Falco](https://falco.org/docs/event-sources/kernel/) (security),[Pixie](https://px.dev/) (APM for apps on Kubernetes), and[Cilium](https://cilium.io/) (networking monitoring) possible.


## Not just a prototype: stable instrumentation


The team worked extremely hard to get a stable version of instrumenation that was usable in production. Just one example: eBPF programs require a way to identify the location of specific data structures and variables in the user space. For example, to read the value of the target field in the google.golang.org/grpc.ClientConn struct (as shown in the[gRPC instrumentor](https://github.com/open-telemetry/opentelemetry-go-contrib/blob/main/instrumentation/google.golang.org/grpc/otelgrpc/interceptor.go) ), the eBPF program needs to determine the field’s offset within the struct definition. This offset is used by the eBPF program to access the target field and perform the necessary analysis.


One way to determine the offset of a field in a struct is to hard code the offset information into the eBPF program. However, this approach can make instrumentation very unstable. Field locations inside structs may change, which means the eBPF program must be recompiled every time the struct definition changes. This process can be time-consuming and error-prone, especially for large and complex codebases. The result is instrumentation that is fragile and significantly increases build times.


There is a way to extract necessary offsets without hard coding them into the eBPF program. This can be done by analyzing the target binary using DWARF (Debugging With Attributed Record Formats), a debugging information format used by many compilers, including the Go compiler. The DWARF debug information is generated by the compiler and stored inside the binary. By analyzing the DWARF information, it is possible to extract the necessary offsets for the eBPF program.


DWARF information is usually stripped from production binaries to reduce their size and improve performance. This means that the eBPF program may not be able to extract the necessary offsets from a stripped binary. To solve this problem, the team developed a library called[offsets-tracker](https://github.com/keyval-dev/offsets-tracker) . This library tracks the offset of different fields across versions and stores them in a database.


The offsets-tracker library provides a stable and flexible solution for eBPF instrumentation, even when data structures change and binaries are stripped.


## One more challenge: Time


Golang really is a very different environment from the other backend web applications, and that’s put on stark display when you learn that not even epoch time is readily available. From the project’s writeup on Go instrumentation:


eBPF programs can access the current timestamp by calling bpf_ktime_get_ns(). The value returned by this function is fetched from the CLOCK_MONOTONIC clock and represents the number of nanoseconds since the system boot time.


According to OpenTelemetry specification start time and end time should be timestamps and represent exact point in time. Converting from monotonic time to epoch timestamp is automatically handled by this library. Conversion is achieved by discovering the epoch boot time and adding it to the monotonic time collected by the eBPF program.


## What this means for the Go community


This represents a massive shift for the Golang community since automatic instrumentation means that OpenTelemetry will be much easier to add everywhere in a microservice cluster. This matters because, honestly, it’s unlikely that the issue with a Go service is slow code within the service, rather we must see the entire cluster working, and trace requests across our cluster, to identify the source of performance problems.


## Conclusions


The use of eBPF for automatic instrumentation in Go is a game-changer for the OpenTelemetry project and the Go community. It allows for truly automatic instrumentation, without the need for manual code edits, and provides stable instrumentation even as data structures change and binaries are stripped. This will make it much easier to add OpenTelemetry to microservice clusters, enabling easier tracing of requests and identification of performance problems.


‍
