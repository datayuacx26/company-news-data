---
schema_version: "1.0.0"
document_id: "b83bb5fb2fc83d7a8e7cfb82be523eedfed06fca4bbba9a806ac2e5c7746c731"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/ssh-port-forwarding-ssm/"
published_at: "2025-02-07T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:ecfdec3abfbb8c9db314b92c1f1cd06a858f9694a2e9c16391fa44e83e945ded"
---

# Down the Rabbit Hole: Implementing SSH Port Forwarding over AWS Session Manager

I’m Formal’s newest hire. I[graduated](https://www.linkedin.com/posts/saligrama_im-excited-to-share-that-i-graduated-from-activity-7287906878536253440-qs-D?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAACXWLtsBnuUyaYdCrwWUDDYLcKevuxUXQMA) from Stanford two months ago, and my first day on the job was Monday, January 20. After a fast onboarding setting up my work laptop and phone, I was quickly added to our customer-shared channels.


That day, one of our customers paged us asking for help with an issue they were having. Two of the many protocols that the Formal Connector supports include SSH and AWS Session Manager (SSM), which our customer heavily relies on for engineering access to compute resources. This had been working well for them, but they had recently encountered some issues in trying to set up[Remote SSH development](https://code.visualstudio.com/docs/remote/ssh) via Visual Studio Code, in order to run machine learning workflows on a remote dev box with a GPU. This seemed like an odd bug, and so my first Formal task ended up being to investigate and fix the issue.


What began as a bug fix quickly turned into a real technical quest through obscure SSH and SSM features, culminating in **forking and fixing several concurrency bugs** in AWS’s own reference library for connecting to compute instances using SSM. The Formal connector now supports connections via VS Code Remote SSH.


## Formal’s SSH connector


Formal helps security teams better understand and control the flow of data around production infrastructure. The Formal Connector, a protocol-aware reverse proxy typically deployed as a single binary in a container, is at the core of this approach.


Today, Formal customers who deploy the Formal Connector in their cloud environments can use it as an SSH proxy. Uniquely, the Formal Connector supports connecting to remote hosts not only via the SSH protocol, but also via[AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html) , which allows connecting to AWS EC2 and ECS Fargate compute instances without needing to install SSH keys or deal with machine user passwords — a security advantage.


When users SSH into the Formal Connector, they are presented with a TUI menu in which they can select an SSH, EC2, or ECS Fargate host they can connect to. Alternatively, they can also connect directly to a remote host of their choice by running a command against the Formal Connector.


Depending on the remote connection type, the Formal Connector then initiates an SSH or SSM connection to the remote host, and then proxies user input and shell output between the client SSH stream and the remote SSH or SSM stream.


This enables a number of features that security teams might find convenient: they can leverage Formal’s policy engine to restrict access to production-sensitive hosts to a subset of users, and they can ensure the entire SSH session, including commands and their outputs, are fully logged and analyzed for their risk level.


## Remote SSH Development with VS Code


The Remote SSH extension in VS Code allows users to “open a remote folder on any remote machine, virtual machine, or container with a running SSH server and take full advantage of VS Code’s feature set” across the entire remote host’s file system. This gives users the ability to interact with code on a remote machine almost as if it was a local one, including with code completions and debuggers.


To provide this “local-quality” experience, VS Code relies on more than just the standard shell stream provided by SSH. As it turns out, there are a number of steps that VS Code takes to prepare a remote SSH host for remote development.


Here, VS Code establishes an SSH connection to a remote host and runs a shell script against this connection. This setup script first downloads the Code Server binary onto the remote host, then starts the Code Server bound to a random port on the remote host listening on[localhost](http://localhost/) only. VS Code then looks for the Code Server binary to print out the random port in the SSH connection, at which point it asks the remote host to initiate two TCP port forwards (for redundancy) of that port back to the local machine. Once these TCP tunnels are established, VS Code can enable all of its features for the user.


The key part of this flow is the SSH port forwarding step. The SSH protocol supports a number of different multiplexed channel types: the canonical login shell channel is of type` session` , but there’s also a` direct-tcpip` channel type that can be used for client-initiated forwarding of server ports. This is the channel type that powers[SOCKS proxies](https://en.wikipedia.org/wiki/SOCKS) , for example. Note that the client doesn’t need to declare the port forward at the beginning of the session, but can rather open arbitrary port forwards at any time during the session.


Prior to this work, the Formal Connector didn’t yet support TCP/IP forwarding over SSH. Thus, when VS Code was trying to initialize the Code Server on a remote machine via the Formal Connector, the flow looked more like this:


Here, when VS Code requested the port forward of the remote Code Server port, the Formal Connector didn’t know what to do with that request! Thus, anyone attempting to use VS Code Remote SSH via the Formal Connector would simply see their code editor freeze indefinitely while trying to set up the remote machine.


## Implementing support for TCP/IP forwarding for SSH remotes


The Formal Connector’s SSH support is based on the[Charm Wish](https://charm.sh/) framework for building SSH apps (the same framework that powers[terminal.shop](http://terminal.shop/) , for example). This framework takes care of most of the undifferentiated heavy lifting of the SSH protocol and allows us to focus on implementing Formal’s unique policy and audit system as a middleware. Once a client SSH connection is established, the connector works with the abstraction of the connection as a byte stream and can proxy bytes back and forth between the client and remote (established using either Go’s` crypto/ssh` standard library, or a custom data channel library in the SSM case — more on that later) connections.


Fortunately for us, the Wish framework and` crypto/ssh` have native support for` direct-tcpip` SSH channels for port forwarding. Implementing support in the Connector was thus relatively straightforward. We could take the client’s port forwarding request and simply forward it to the remote server, giving us two` io.ReadWriteCloser` interfaces that we could easily pass data back and forth between. In Wish, we simply register this function as a channel handler, and the framework takes care of the rest.


Once this was implemented, it was now easy for users to perform port forwarding through the Formal Connector. For example, we could run a stub HTTP server on` http://localhost:9000` on a remote machine, then port forward it via the Formal Connector and curl it from a laptop.


## The AWS Session Manager Protocol


The next step was to implement SSH TCP/IP forwarding for AWS SSM remotes. Before explaining how we did that, I’ll walk through how the protocol actually works. Unfortunately, the SSM protocol is poorly documented, but the reference[client](http://github.com/aws/session-manager-plugin) and[server](http://github.com/aws/amazon-ssm-agent) implementations are fortunately open-source.


The best way to describe the SSM protocol is “byte streams encapsulated in JSON with sequencing, encapsulated in a custom binary protocol with sequencing, over WebSockets.” It’s a fairly complex protocol that can handle multiple types of sessions, including interactive and non-interactive shell sessions and commands, as well as port forwarding from local (the machine where the agent is installed) and remote (elsewhere in the AWS environment) hosts.


To use the AWS SDK in Go to initiate a session with an EC2 host, one could do


The returned` session` struct contains` StreamUrl` and` TokenValue` strings that can be used to dial and authenticate a websocket session with the SSM control plane. However, you’ll notice that nowhere in the SDK is functionality to dial and interact with such a websocket. This is probably due to the fact that SSM sessions aren’t originally intended to be interacted with programmatically — several years ago, Corey Quinn[likened](https://www.reddit.com/r/aws/comments/9r0tp8/ssm_session_manager_interface_issues/) it to “a serial console more than an SSH session.” AWS provides a separate[client](https://github.com/aws/session-manager-plugin) that can be used to run these websocket sessions, but it’s not easily integrable in a Go codebase as it’s designed to be used as a standalone binary.


Communications over the data channel are conducted in the format of client messages, which are binary messages that encapsulate JSON payloads.


Thus, prior to this work, we were using a custom library to handle communication over the SSM data channel. I spent a significant amount of time trying to make port forwarding work using that custom library, thinking that once I had an SSM port forwarding session started using the AWS SDK, I could just write to the opened data channel using the custom library to encapsulate messages. However, there was a lot more complexity than I bargained for.


## SSM and Port Forwarding


In a typical shell session scenario, which our library was designed for, the SSM protocol operates exactly as depicted in the above image. But there’s an additional wrinkle when setting up a port forwarding session.


In a port forwarding session, before any data can be sent, the agent initiates a handshake request that must be both acknowledged and responded to with a handshake response, and then the agent delivers a handshake complete message. Of course, each of these messages have their own sequence numbering as well.


As I tried to implement port forwarding support, I found that I would keep getting SSM control messages in my port forwarding data stream, leading to errors like` curl: (1) Received HTTP/0.9 when not allowed` . It turned out that this was because our custom library didn’t have support for this handshake setup. I tried for a while to add it, but I kept running into off-by-one sequencing issues that would cause the session to freeze before any data was transferred.


Shelving that approach, my next step was to use a different, publicly available[alternate SSM client](https://github.com/mmmorris1975/ssm-session-client) that did have support. But this, as well suffered from an issue that necessitated I fork it.


### Intermediary SSM bugs


The SSM agent, on an AWS compute instance, uses the reported client version during the handshake to decide which features to support. The version reported by this library was old enough to trigger a[bug](https://github.com/mmmorris1975/ssm-session-client/issues/17) in the agent that would inadvertently shut down the port forwarding session within 30 seconds.


Thus, I forked the library just to bump that version number. I integrated it with my port forwarding code, only to get these cryptic errors from agent logs on the remote end:` Unable to accept stream: invalid protocol`


With some digging, I came to realize that the issue was triggered by these lines of code in the agent:


As you can see, if the client reports a version high enough to avoid the session shutdown bug, it is also required to support multiplexed port forwarding over the[smux](https://github.com/xtaci/smux) protocol. Thus, I was getting the invalid protocol errors because the remote SSM agent was expecting` smux` frames that encapsulated SSM messages, rather than the SSM messages themselves.


So…time to implement a mux client!


…or at least I tried to implement a mux client. I even got one` curl` invocation working once, but then when I went to tidy up the code changes on a long Tuesday evening, I filed some PRs only to find I couldn’t reproduce it. No matter what I tried, there was always some sequence numbering issue that prevented me getting useful data over the stream. It was time for a different approach.


### Forking the Session Manager Plugin


I decided that the best way to get this working would be to simply use as much of the AWS reference protocol implementation as possible.


Despite being written in Go (just like the Formal Connector), there were both *many* issues preventing the AWS Session Manager Plugin from being integrated in a Go codebase *and* a few unaddressed bugs as well. For example, the plugin wasn’t based on Go modules (despite those being the default method of structuring Go projects since 2019), and it didn’t give programmatic callers any access to the data channels used to stream data. It also vendored a number of ancient dependencies, including UUID and logging libraries that hadn’t been updated in a decade.


This necessitated my forking it, and my first change was to[apply](https://github.com/formalco/session-manager-plugin/commit/b4e88056268188fee5a369b1efd4ba749cea808e) a few patches from unmerged PRs to fix the above versioning bug and to give callers better control over session termination. I also migrated the fork to Go modules and unvendored the dependencies, and also allowed port forwarding callers to pass in a Unix socket file name that the plugin could write the data from the remote port to. Now, we could establish a port forward over the Unix socket, and then proxy bytes between that Unix socket and the client’s TCP channel.


Now, we could reliably perform the same flow as with the SSH remote:


But still, I would get errors establishing a VS Code remote SSH session over the Formal Connector, where it would still freeze while setting up the port forward.


### Data races in the SSM Plugin


Turning to the connector logs, I saw output of the form


I would get several of these data race warnings each time I tried to` curl` the forwarded port. I hypothesized that this could be causing a deadlock in the VS Code scenario, which of course requests two port forwards (and therefore two SSM channels) rather than one.


So now it was time to go data race hunting. It soon became apparent that there was very little in the way of synchronization implemented in the reference implementation, despite multiple Goroutines concurrently reading and writing fields on an individual` DataChannel` struct.


I[added](https://github.com/formalco/session-manager-plugin/commit/c094eb00c28b5009b2c346e5abb74cbcf8f1a2bd) a mutex to each` DataChannel` struct and figure out where to take and release the lock to avoid both deadlocks and data races. I made the code changes, and stopped getting data race messages in the` curl` scenario. Success, right?


…nope. I’d still get freezes and data races while trying to set up VS Code. After a bit more prompting o1, I finally found the last bug that was preventing VS Code from working.


By default, the Session Manager Plugin uses a “plugin registry” (a` map\[string\]ISessionPlugin` ) to figure out whether it should open a shell session or a port forwarding session when receiving session configuration from the SSM control plane API. This means that there’s exactly one session instance per invocation of the plugin, and trying to work with a second session instance overwrites data in the existing one — fine for a binary, less so for a library. This was obviously causing issues with VS Code, which was trying to request two port forwards.


The fix was[simple](https://github.com/formalco/session-manager-plugin/commit/57840554be7f2afc17e8751545444e460735dbef) : modify the plugin registry to store session type constructors, rather than live session instances. We’ve also opened a[pull request](https://github.com/aws/session-manager-plugin/pull/105) on AWS’s` session-manager-plugin` repository that fixes these issues we found.


After making that change (and[replacing](https://github.com/formalco/session-manager-plugin/commit/ea0a42ddcaf7e708df230360cf8affe3e0d3c042) an ancient UUID library that also stored global state causing data races), we were able to get VS Code to successfully initiate Remote SSH connections to EC2 and ECS Fargate instances through the Formal Connector!


## Conclusion


We’ve since shipped this functionality, and if you’re a Formal customer, we hope you enjoy using secure, audited connections for your VS Code remote coding sessions. If you’re interested in building modern, performant solutions to protect customer data in the cloud, Formal is[hiring](https://jobs.ashbyhq.com/formal) .
