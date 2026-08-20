---
schema_version: "1.0.0"
document_id: "2b8b9b1ef6257d2119829a0a79459eec1f43ada55be6a95661d22cf5155afd50"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/security-debt-we-never-created/"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T21:44:45.479848+00:00"
content_hash: "sha256:958e973cb9c4c1f69be8eb7b946e74110508d6de6a29b9096d0acc1f4059e661"
---

# Security Debt We Never Created

Formal is critical-path software. In order to guard access to our customers’ most sensitive data and compute resources, the Formal Connector sits in a privileged network position in customer VPCs with connectivity and authentication to that infrastructure. Because of this setup, we take this responsibility incredibly seriously and invest significantly in reliability and security to ensure the availability of customer access and confidentiality of customer data.


As an early-stage startup, however, these efforts don’t require a large security team in order to provide full security coverage. Rather, we’ve kept the architecture straightforward, avoiding overcomplexity and obfuscation.


# Memory safety and runtime security


There’s no excuse for writing net-new software in memory-unsafe languages nowadays. Since Formal’s inception, all our control-plane backend services, as well as the Formal Connector and Satellites, have been written in Go with some Rust components.


We also use Protocol Buffers and gRPC for all APIs, which gives us the benefit of strong typing on all shared network surfaces between the control plane, Connector, Satellites, Terraform provider, desktop app, and frontend. Though pure gRPC is not well-supported in the browser, we leverage the[ConnectRPC](https://connectrpc.com/) framework and[Protovalidate](https://protovalidate.com/) on top of gRPC, which allow serializing Protobuf from JSON representation and enforcing further constraints on API payloads, respectively.


The security benefits should be obvious — we avoid an entire class of memory safety vulnerabilities across all surfaces that handle user input, as well as an entire class of deserialization-related vulnerabilities across our APIs. Beyond security, strong typing also helps us ship faster. Coding agents benefit significantly from types as a form of explicit documentation. It also helps them receive feedback about incorrect code earlier in the development process — at compile time rather than at runtime.


# Scratch containers


Container security occupies exactly $0 of our security budget at Formal. All of our control plane backend services, including APIs and workers, are written in Go, which has the super helpful property of easily compiling into a statically linked single binary (i.e., no dependencies on` libc` or a full container OS).


This means that each service can be deployed from a *scratch* container, i.e., a Docker layer without any files on it — there’s no filesystem, no shells, and no coreutils. The build process looks a little like this:


```text
##################################################
### Builder image
##################################################
FROM     golang     AS     builder


# build tools
RUN   go install github.com/bufbuild/buf/cmd/buf@latest
RUN   go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
RUN   go install connectrpc.com/connect/cmd/protoc-gen-connect-go@latest
RUN   go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway@latest


# Go dependencies
COPY   go.mod go.sum /go/src/github.com/formalco/monorepo
WORKDIR     /go/src/github.com/formalco/monorepo
RUN   --mount =  type  =  secret,id =  buf-user,env =  BUF_USER  \
--mount =  type  =  secret,id =  buf-api-token,env =  BUF_API_TOKEN  \
GOPROXY  =  "https://proxy.golang.org,https://  ${  BUF_USER  }  :  ${  BUF_API_TOKEN  }  @buf.build/gen/go"    \
go mod download


COPY   ./ /go/src/github.com/formalco/monorepo/


WORKDIR     /go/src/github.com/formalco/monorepo/backend/apis


# Protobufs
RUN   --mount =  type  =  secret,id =  buf-api-token,env =  BUF_API_TOKEN  \
echo    "  ${  BUF_API_TOKEN  }  "    |   buf registry login --token-stdin  &&    \
buf generate


# Explicitly build a static binary
RUN    CGO_ENABLED  =  0   go build -o server -ldflags =  "-w -s -extldflags \"-static\""


##################################################
### Final image
##################################################
FROM     scratch


# Only the final binary and a TLS truststore goes in the production image!
COPY   --from =  builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY   --from =  builder /go/src/github.com/formalco/monorepo/backend/apis/server /app/server


CMD    [  "./app/server"  ]


```


and the entire filesystem of the container looks like


```text
aditya@laptop ~ $ orb debug apis
@apis / $ tree
├── app
│   └── server
└── etc
└── ssl
└── certs
└── ca-certificates.crt


```


These containers don’t have a Linux distribution or any OS components at all, to the extent that we need to unpack the following` tar` file using the Dockerfile` ADD` directive just for the Connector to be able to write to` /tmp` :


```text
aditya@laptop ~ $ tar tvf tmp.tar
drwxrwxrwt   0    0         0              0   Feb  18     2025   tmp/


```


As a result, we don’t need to worry about container supply chain integrity at all! Our supply-chain considerations are simplified to only our Go dependencies, which the Go packaging ecosystem makes significantly easier than many other languages.


# Serverless deployment


That each of our services run on scratch containers allows us to pursue a managed, locked-down serverless deployment strategy. Our control plane runs on AWS ECS Fargate, which means we don’t manage VM instances. This eliminates a significant amount of potential attack surface, since host updates and patches as well as container orchestration hardening are AWS’s responsibility rather than ours.


Moreover, we disable ECS Exec functionality that could otherwise be used to shell into production containers. ECS Exec is widely used in the industry for debugging production issues, but since our service containers don’t even have a usable shell, we can go further and remove this from our attack surface.


# Terraform-only production management


At Formal, all production infrastructure is provisioned entirely via Terraform. Using Terraform already has a number of benefits for infrastructure management. For example, it ensures that all changes to production are held to the same auditing and code-review standards as the codebase itself. From a security perspective, we heavily use custom Terraform modules to simplify the process of deploying new control plane services. This allows us to define security-relevant properties, such as security groups and IAM permissions, of the infrastructure in one place. It also means that deploying a new API or worker can be done in as little as a single line of code.


Together, the above two components of our infrastructure strategy confer two nice properties:


- The state of our production infrastructure is defined exclusively by the Terraform code in the` main` branch of our monorepo
- The state of our deployed control plane is defined exclusively by the application Go and Rust code in the` main` branch of our monorepo


In other words, there is nowhere in our production environment that a human (or AI agent!) could make changes outside our strict code review process.


# Secrets management


We maintain a number of strict secrets management properties:


- Zero secrets in the codebase
- Zero long-lived credentials, if the vendor provides a better option.


We use[Doppler](https://www.doppler.com/) to manage the secrets our production codebase needs access to. Doppler supports multiple environments, meaning that we can maintain development secrets in Doppler to inject at runtime and avoid committing them to the codebase. Additionally, Doppler also lets us sync production secrets to AWS Secrets Manager, where that integration can be maintained in Terraform.


We also enforce the usage of OpenID Connect (OIDC) for authentication between our infrastructure vendors where possible. In particular, GitHub Actions and Terraform Cloud authenticate to AWS via OIDC, as does our GitHub Actions runner vendor to GitHub.


Until[very recently](https://www.linkedin.com/posts/ahmb84_today-is-a-very-sad-day-at-formal-we-activity-7424478029755760640-SSBL) , we had zero IAM users in our production account. The only reason we needed to add one was a bug in the AWS SDK for Rust.


# Availability and reliability


This section alone could fill multiple blog posts. Our reliability philosophy is simple: eliminate single points of failure that could compromise the availability of the control plane or Connectors.


Connectors, for instance, are designed to stay available even if the control plane goes down. Each Connector instance holds the current control plane state in memory, pulling ordered events to stay current. If the control plane becomes unavailable, the Connector stops receiving updates but remains fully operational on its last known state.


A few other highlights:


- Operations that generate large volumes of temporary data — like our Data Inventory workflow — now run against dedicated Postgres instances.
- We replaced a subscription model over a long-lived gRPC stream with an event-based pull model, eliminating the risk of a Connector instance missing a data change event.
- If the control plane experiences latency processing logs, the Connector now buffers log writes to disk — preventing log processing delays from affecting resource access.


# Conclusion


The common thread across all of these decisions is that **we’re biased toward preventing security problems in the first place** , rather than heavily relying on detecting and responding to ones we created. Scratch containers don’t need constant vulnerability scanning and a long tail of patching and remediation. OIDC tokens aren’t leaked. Terraform-only, serverless infrastructure can’t be misconfigured via a compromised console session when console access is disallowed entirely.


This philosophy also informs many of our product decisions! For example,


- We recently added support for[AWS SigV4](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_sigv.html) authentication to our public API endpoints. This allows customer machine workloads to authenticate to the Formal control plane without needing long-lived static credentials.
- The Formal Connector is also shipped via a scratch container.
- We provide Terraform and Pulumi providers (and IaC documentation examples). It’s only natural for customers who GitOps their infrastructure management to also GitOps their infrastructure *security* management.
- Our Permissions system, through which customers control access to the control plane, evaluates Rego policies just like Connector policy evaluation. As a result, control plane authorization is just as flexible and incorporates the ability to include specific API endpoints *and their arguments* as control targets.


By shifting security left into our architecture *and product* , we’ve eliminated entire categories of concerns that would otherwise require constant vigilance, and would take time away from building a great product for our customers.
