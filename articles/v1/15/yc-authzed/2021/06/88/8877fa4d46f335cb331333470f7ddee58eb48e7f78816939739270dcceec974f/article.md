---
schema_version: "1.0.0"
document_id: "8877fa4d46f335cb331333470f7ddee58eb48e7f78816939739270dcceec974f"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/buf"
published_at: "2021-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:7d953c65d68cbcea6c2e952cef1b08892f92ce61dbaa859ff23dd7d0c335f8e7"
---

# Maintaining a Stable Protobuf API

This is a cross-post originally written for the[Buf blog](https://blog.buf.build/2021/06/15/authzed-case-study-maintaining-a-stable-protobuf-api/) .


## The first day of the rest of your life


Congrats!


You've hit a stable release or maybe you haven't and instead customers have already decided to depend on your API. Rather than pulling the rug out from beneath your users, you've taken it upon yourself to keep everyone happy. Now that you've been tasked with maintaining API compatibility, you'll need a guiding light to copy for inspiration.


For many developers,[Stripe](https://stripe.com/blog/api-versioning) is that holy grail of API compatibility: Applications with payment systems written using the Stripe API over a decade ago still function today without changes. While we should all aspire to have a comparable compatibility story, not everyone is using the same technology stack. What does maintaining an API at Stripe's level look like if you're using gRPC/Protobuf rather than JSON/HTTP?


## Taking the first step


At[Authzed](https://authzed.com/) , we've begun our journey towards diligent Protobuf API versioning.


Our initial goals are to:


1. Catch breaking changes before they ship and release a new version of the API if need be
2. Make the API consistent, intuitive, and ecosystem-friendly
3. Remove as much friction as possible for our developers while making changes


The starting place for us was obvious: migrate our existing ad-hoc Protobuf setup to the[Buf toolchain](https://buf.build/) . Buf is a new, faster Protobuf compiler, but compilation speed isn't why we're switching: we're sold on its robust feature set.


## Iterating with confidence


Our previous workflow had been to read the Protobuf documentation to determine whether or not a change is backward compatible. Occasionally, we'd spend time testing out code locally just to ensure that a change is wire-compatible. This is time-consuming and adds a requirement for more tribal knowledge from our team of developers.


Buf eliminates this concern with[breaking change detection](https://docs.buf.build/breaking-overview) that can be built into CI/CD workflows. Going forward, we'll be able to publish an official versioning and deprecation policy, which can be easily and confidently enforced with Buf.


## Discovering idioms


Even though we've worked with Protobuf APIs in the past and even have a Xoogler on the team that has worked on the internal Protobuf tooling team at Google, we still struggle to write and maintain idiomatic objects and service definitions. Buf has a massive index of linting rules and presets like those used at Google and Uber. These linting rules are the culmination of experience from years of engineering and are a great source to learn from. The[linting documentation](https://docs.buf.build/lint-rules) includes descriptions of the common rules and justifications for why they should be applied. These idioms include package naming, which in turn describes how to best version your packages, too!


We're currently sticking with the defaults which we've found quite sane. However, there's a fine balance between following idioms and making trade-offs for user experience; not every idiom yields ergonomic code generated in each language. When we eventually run into particular rules that choose to ignore, Buf makes[exceptions](https://docs.buf.build/lint-configuration) a single-line change.


## Standardizing the build flow


Before Buf, we had shell scripts for generating code from our Protobuf service definitions. Each shell script varied from project to project and had to include additional logic like determining where the script was executed relative to where our` .proto` files live. Only afterward could we focus on passing the right flags to` protoc` to generate code. All of this, however, is already built into Buf, allowing us to abandon our shell scripts entirely.


Now we have a` buf.gen.yaml` that specifies our plugins' arguments. By adding a[shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) to the beginning of the YAML file, we even make it so you can easily *execute* the YAML file to generate the code for a project:


yaml


1


2


3


4


5


6


7


8


9


10


11


12


```text
#!/usr/bin/env -S buf generate ../protos --template
version:    "v1beta1"
plugins:
-    name:    "go"
out:    "pkg/proto"
opt:    "paths=source_relative"
-    name:    "go-grpc"
out:    "pkg/proto"
opt:    "paths=source_relative"
-    name:    "validate"
out:    "pkg/proto"
opt:    "paths=source_relative,lang=go"


```


yaml


1


2


3


4


5


6


7


8


9


10


11


12


```text
#!/usr/bin/env -S buf generate ../protos --template
version:    "v1beta1"
plugins:
-    name:    "go"
out:    "pkg/proto"
opt:    "paths=source_relative"
-    name:    "go-grpc"
out:    "pkg/proto"
opt:    "paths=source_relative"
-    name:    "validate"
out:    "pkg/proto"
opt:    "paths=source_relative,lang=go"


```


sh


1


```text
./buf.gen.yaml


```


sh


1


```text
./buf.gen.yaml


```


Now our devs don't even have to learn how to use Buf: our config files know how to run themselves and CI/CD pipelines can handle the rest.


## A lifelong journey


This is just the beginning of our service's API. We know you can never truly escape[Hyrum's Law](https://xkcd.com/1172/) , but these are our first steps towards minimizing the impact of the changes we make. Buf enabled us to fly past the first step of validating our APIs and now we can focus on building out API metrics that will be used for data-driven decision-making for our deprecation policies and API design efforts. We're extremely excited for the future Buf and its impact on the ecosystem of Protobuf tooling.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- The first day of the rest of your life
- Taking the first step
- Iterating with confidence
- Discovering idioms
- Standardizing the build flow
- A lifelong journey
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
