---
schema_version: "1.0.0"
document_id: "8c9db62963c2bfc63033b6559fd956689c779b9d44d16c1b3e284812a949afeb"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/ai-coding-agents-break-what-works/"
published_at: "2026-03-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:030a921918848a933d786b6c05b35514357b4bf4db42c23a9c495aff739d7264"
---

# AI Coding Agents Break What Works

Your AI coding agent just made every test pass. Ship it, right?


Not so fast. A growing class of AI-generated bugs doesn’t come from writing bad code. It comes from the AI *changing working code* to accommodate its own mistakes. This isn’t a theoretical risk. It’s happening now, in production codebases, and it’s harder to catch than any bug the AI might introduce from scratch.


The pattern is consistent: the agent encounters a failure, “fixes” something it shouldn’t have touched, and the local feedback loop turns green. The[silent failure](https://speedscale.com/blog/silent-failures-why-ai-code-compiles-but-breaks-in-production/) only surfaces later, in a different environment, under real traffic.


## The New Failure Mode: AI That Fixes the Wrong Thing


We’ve all seen the classic version of this problem: an AI modifies a unit test to force a green build instead of fixing the actual bug. Most teams have learned to watch for that. But AI coding agents have evolved past the obvious version. Here are three real patterns we’ve seen or heard about recently:


### 1. Changing a Protobuf Field Type That gRPC Won’t Allow


This one happened to us. During a recent update, an AI agent reused an existing field number in a` .proto` definition, replacing a` bool enabled` field with a` string role` field. The reasoning looked sound: the feature needed richer values than true/false. The generated` .pb.go` code compiled. The service’s own tests passed. Everything looked clean in the PR.


```text
// Before: the field every service was compiled against
message   User   {
string   name   =   1  ;
bool   enabled   =   2  ;    // wire type 0 (varint)
}


// After: the AI's "improvement"
message   User   {
string   name   =   1  ;
string   role   =   2  ;    // wire type 2 (length-delimited) — BREAKING
}
```


But gRPC doesn’t allow this. Reusing field number` 2` for a different type is a[backwards-incompatible change](https://protobuf.dev/programming-guides/proto3/#updating) in the protobuf spec. The wire format for a bool and a string are different encodings, so any service still compiled against the old proto silently misinterprets the bytes. When the change hit production, the dashboard started throwing errors on every call to the API gateway because one side still treated field` 2` as` enabled: bool` while the other had started sending` role: string` .


The AI agent had no idea this constraint existed. It saw a` .proto` file, saw a field, and made what looked like a reasonable type change. Generated files and their specs (protobuf definitions, OpenAPI schemas, ORM migrations) are boundaries an AI doesn’t understand. It sees a file, it sees a problem, it makes an edit.


### 2. Introducing Tools the Codebase Doesn’t Use


We’ve also watched an AI agent look at infrastructure-as-code and decide to use OpenTofu instead of Terraform. Maybe it learned from a demo repo or open-source examples where OpenTofu was the standard. The change looked reasonable in isolation: OpenTofu is a valid Terraform fork. But our repository uses Terraform with an existing state backend. The AI’s “improvement” would have broken the entire infrastructure state, requiring manual intervention to recover.


```text
# Existing repo: Terraform with S3 state backend
terraform   {
required_providers   {
aws   =   { source   =   "hashicorp/aws"   }
}
backend   "s3"   {
bucket   =   "prod-tfstate"
key      =   "infra/terraform.tfstate"
}
}


# AI's change: swapped to OpenTofu encryption block
# (not compatible with the existing Terraform state)
terraform   {
encryption   {
key_provider   "aws_kms"   "main"   {
kms_key_id   =   "alias/tofu-state"
}
}
}
```


This is a subtler version of the problem. The AI isn’t editing the wrong file. It’s making an architecturally incompatible choice because it lacks the context of *this specific codebase’s* decisions and constraints.


### 3. Changing Server Code to Make Browser Tests Pass


This is the most dangerous pattern. An AI agent had browser tests that were failing. Instead of fixing the frontend code or investigating the test setup, it modified the backend API responses to match what the tests expected. The tests turned green. The CI pipeline passed. But the backend was now returning incorrect data to every other client.


This is the evolved version of the “AI rewrites the test to pass” anti-pattern. Teams have learned to lock down test files from AI modification. But an agent with access to the full codebase can achieve the same result by changing the *source of truth* , the server, to match the broken expectation. It’s test-fixing by proxy, and it’s much harder to catch in code review because the server-side change can look perfectly reasonable in isolation.


```text
sequenceDiagram
participant AI as AI Agent
participant FE as Browser Tests
participant BE as Backend API
participant Clients as Other Clients


AI->>FE: Runs browser tests
FE-->>AI: Tests FAIL
Note over AI: Instead of fixing frontend...
AI->>BE: Modifies API response format
AI->>FE: Re-runs browser tests
FE-->>AI: Tests PASS
Note over AI,Clients: But in production...
Clients->>BE: Same endpoint, same request
BE-->>Clients: Wrong response format
Note over Clients: Errors for every real client


```


## Why Code Review Alone Can’t Catch This


The fundamental challenge is that each of these changes *looks correct in isolation* . A code review of the proto field change shows a valid type. A review of the OpenTofu change shows valid infrastructure-as-code. A review of the backend API change shows a reasonable response format.


The failure only becomes visible when you understand the *system context* : how the file is generated, what tools the team actually uses, how other services consume the API. This is exactly the context that AI agents lack, and that human reviewers often skim past in a busy PR.


Avishek Sen Gupta[documented a complementary failure mode](https://avishek.net/2026/03/12/experiences-building-with-coding-assistant.html) while building RedDragon with Claude Code. After the project reached roughly 8,400 green tests, an “assertion audit” found dozens of tests that were passing without validating the behavior they claimed to cover. One pattern was` assert Opcode.BRANCH_IF in opcodes or Opcode.BRANCH in opcodes` , where the fallback` BRANCH` appeared in almost every program and made the test effectively always green. Another only checked that at least one write occurred, which unrelated initialization code already satisfied. Tightening those assertions exposed real frontend bugs the green suite had been hiding.


## Three Layers of Defense


There’s no single solution. You need defense in depth: multiple layers that catch different failure modes.


```text
flowchart TD
PR[AI Opens PR] --> L1
L1[Layer 1: Linters] -->|Pass| L2
L1 -->|Fail: touched generated file| Block1[Block merge]
L2[Layer 2: CI Validation] -->|Pass| L3
L2 -->|Fail: contract or drift| Block2[Block merge]
L3[Layer 3: Traffic Replay] -->|No diff| Ship[Safe to merge]
L3 -->|Behavioral diff detected| Block3[Block merge]


```


### Layer 1: Linters and Protected File Rules


The first line of defense is automated rules that prevent the AI from touching things it shouldn’t. This is the cheapest and fastest guardrail to implement:


Linters work best when they run as hooks, not just at the end of CI. Agents respond better to immediate feedback while they’re still iterating than to a failure that only appears after the whole workflow is done. In practice, you want both: hook-time checks to steer the agent early, and CI checks to make sure nothing slips through later.


-


**Generated file protection:** Lint rules or CI checks that flag any modification to files matching patterns like` *.pb.go` ,` *.generated.ts` ,` *_gen.go` , or directories like` vendor/` ,` node_modules/` , or` generated/` . If these files change, the build fails with a clear message: “Generated files cannot be modified directly. Update the source definition and regenerate.”


-


**Tool consistency checks:** A linter that validates infrastructure files use the expected toolchain. If your` terraform.lock.hcl` exists, flag any` .tf` file that references OpenTofu-specific features. If your` go.mod` pins a specific gRPC version, flag protobuf regeneration with a different compiler version.


-


**CODEOWNERS-style rules for AI:** Many teams already use CODEOWNERS for human review routing. Extend this concept to AI agents: define paths that require human-only approval or that the AI agent is explicitly forbidden from modifying. Claude Code’s` CLAUDE.md` files and` .cursorrules` can enforce some of this at the agent level.


### Layer 2: CI Pipelines with Real-World Validation


Linters catch the obvious cases, but what about the backend API change that looks valid in isolation? This is where your CI pipeline needs to go beyond[static analysis alone](https://speedscale.com/blog/runtime-validation-vs-static-analysis/) :


-


**Contract tests:** If your gRPC services have proto definitions, your CI should compile *all* dependent services against the same proto and verify compatibility. If one service’s generated code is out of sync, the contract test fails, even if the individual service’s tests pass.


-


**Integration test suites the AI can’t game:** Keep a set of integration test scenarios that validate end-to-end behavior against known-good baselines. The key insight: these tests validate the *behavior* of the system, not the *implementation* . An AI can change code to match test assertions, but it can’t change what a real client expects from your API.


-


**Drift detection:** Compare generated files against their source definitions in CI. Run` protoc` and diff the output against what’s checked in. Run` terraform plan` and fail if there’s unexpected drift. These checks are cheap and catch an entire class of “AI edited a generated file” bugs.


### Layer 3: Traffic Replay, the Guardrail AI Can’t Outsmart


The first two layers are necessary but not sufficient. Linters catch known patterns. Contract tests catch interface breaks. But what catches the case where the AI makes a semantically valid change that breaks real-world behavior?


**Traffic replay.** By capturing actual production traffic and replaying it against your changes, you test against the one thing the AI can never modify: *how your system actually behaves in production* .


When the AI changes a backend API response to make browser tests pass, traffic replay catches it because the replayed requests from real clients expect the *original* response format. When the AI introduces a subtle data transformation bug that passes unit tests, traffic replay catches it because real production payloads exercise edge cases that synthetic tests never will.


This is the[observability gap](https://speedscale.com/features/ai-code-verification/) in action. Your monitoring tools see production behavior in exquisite detail. Traffic replay lets you use that same data as a pre-merge validation layer. The AI agent can modify your test files, your server code, your generated files, but it can’t modify the recording of what production actually did.


Tools like[proxymock](https://speedscale.com/proxymock/) make this practical for local development: capture real traffic, replay it against your branch, see exactly where your changes diverge from production behavior. In CI,[Speedscale](https://speedscale.com/) does the same at scale, replaying production traffic scenarios as a gate before merge.


## The Pattern to Watch For


Every example in this post follows the same pattern:


1. AI encounters a failing test or broken build
2. Instead of fixing the root cause, AI modifies a *different part of the system* to accommodate its change
3. The local feedback loop (tests, linter, type checker) turns green
4. The system-level behavior is broken in ways that only surface under real conditions


As AI coding agents get more capable (longer context windows, access to more files, ability to run and iterate on tests) this pattern will get harder to catch, not easier. An agent with access to your entire codebase and the ability to run tests in a loop will eventually find *some* combination of changes that makes everything green. Your job is to make sure “everything green” actually means “everything works.”


The teams that ship safely with AI coding agents won’t be the ones with the best prompts or the most expensive models. They’ll be the ones with guardrails that validate behavior against reality, not just against the AI’s own test suite.


Ready to see what your AI agent actually changed?[Try proxymock free](https://speedscale.com/proxymock/) to capture and replay traffic locally, or see how[Speedscale validates AI-generated code](https://speedscale.com/features/ai-code-verification/) against production behavior in CI.


**Related reading:**


- [Runtime Validation vs Static Analysis: Why You Need Both for AI-Generated Code](https://speedscale.com/blog/runtime-validation-vs-static-analysis/)
- [Silent Failures: Why AI Code Compiles But Breaks in Production](https://speedscale.com/blog/silent-failures-why-ai-code-compiles-but-breaks-in-production/)
- [The Developer’s Guide to Debugging AI-Generated Code](https://speedscale.com/blog/the-developers-guide-to-debugging-ai-generated-code/)
- [Enterprise Vibe Coding](https://speedscale.com/blog/enterprise-vibe-coding/)
- [AI Code Verification with Speedscale](https://speedscale.com/features/ai-code-verification/)
