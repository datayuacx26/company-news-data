---
schema_version: "1.0.0"
document_id: "6027e5b9698facb5a081c64c1aba471a285747d5a38c49ffa141da2bd1bf757e"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/dagger-supports-deno/"
published_at: "2025-05-28T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:f8d6446cbea3594274992c4363d2ab54997087843bb9c79e37dff10bbff849bd"
---

# Dagger now Supports Deno

After[adding Bun runtime support to the Dagger TypeScript SDK](https://github.com/dagger/dagger/pull/6730) last year, we are excited to announce that we now also[support the Deno runtime](https://github.com/dagger/dagger/pull/9927) . With this addition, the Dagger TypeScript SDK now supports the three most popular JavaScript runtimes:[Node.js](https://nodejs.org/en) , Deno & Bun.


Deno is a secure runtime for JavaScript and TypeScript, built with V8 and Rust, and designed by the creator of Node.js, Ryan Dahl. Deno offers first-class TypeScript support out of the box, strong security defaults with permission-based access control, and a built-in dependency inspector and formatter. This makes it a great choice for modern development workflows.


As of Dagger v0.17.1, you can write and run your Dagger modules using Deno, taking advantage of its native TypeScript support and excellent performance — all without needing to transpile or configure complex build tooling.


#### Why Deno? And why now?


We always intended our TypeScript SDK to support multiple runtimes. Node.js, Bun and Deno are all built differently, and developers should be able to choose the one best suited to their application and requirements. We want to enable as many developers as we can in the Dagger ecosystem, and the best way to do this is by making it easier for them to use Dagger with their familiar tooling and environment


We already had support for Node.js and Bun, and Deno support was always the logical next step - the[issue has been open for over two years](https://github.com/dagger/dagger/issues/4368) and we have an[active community channel](https://discord.com/channels/707636530424053791/1285644350524756150) in our Discord. However, we ran into some technical roadblocks:


- SDK sub-package installation: Deno has a different approach to workspace management than Bun and Node.js. We needed to switch to a[Deno workspace](https://docs.deno.com/runtime/fundamentals/workspaces/) so that the SDK dependencies were correctly installed when running` deno install` for the user’s module.
- Node.js/Deno compatibility: Deno’s compatibility layer makes it possible to call Node.js APIs from within Deno. However, Deno’s configuration file required[additional custom scripting to inject the values](https://github.com/dagger/dagger/blob/0f16bce5c5208c61931a6658e40ee9075095a65d/sdk/typescript/runtime/bin/__deno_config_updator.ts#L4) required for the SDK to “just work”. For example, since the TypeScript SDK is written in Node.js, we needed to enable various experimental features via` unstableFlags` to make the SDK compatible with Deno. Similarly, we needed to enable Deno’s` experimentalDecorators` flag to support Dagger-specific decorators like` @func` ,` @object` , and so on.


This is where our friendly and knowledgeable Dagger community proved invaluable. We received support and assistance from Deno experts, which coupled with recent compatibility improvements in Deno 2, finally made adding this support relatively straightforward.


#### How does it work?


Dagger will detect any project containing a` deno.json` file, and it will automatically use the Deno runtime by updating the` deno.json` file with the required configuration.


Here’s an example of a Deno project managed by a Dagger module written with the Deno runtime.


```text
# Initialize a new project
deno init


# Add a remote JSR to validate input
```


The project takes a string as input and verifies if it’s a valid email or not:


```text
import   { email, validate } from   "@celusion/simple-validation"  ;


function   validateEmail  (input:   string  ) {
if   (  !  input) {
console.  error  (  "ERROR: No email provided"  );
}


if   (  validate  (input, [[email]])   ===   true  ) {
console.  log  (  "Email is valid"  );
return  ;
}


console.  error  (  `Email ${input} is invalid`  );
}


if   (  import  .meta.main) {
validateEmail  (Deno.args[  0  ]);
}
```


To manage this project, create a Dagger module in the` dagger` directory:


```text
mkdir   dagger   &&   cd   dagger


# Initialize a deno project inside the dagger directory so it can be detected by Dagger
deno   init


# Remove init files except deno.json since we do not need them
rm   *  .ts


# Initialize a dagger module in the current directory
dagger   init   --name=deno-mail-validator   --sdk=typescript   --source=.
```


Then, write a simple Dagger module with the Deno runtime. This module includes Dagger Functions to build, test and publish your Deno project with Dagger:


```text
import   { dag, Container, Directory, File, object,   func  , argument } from   "@dagger.io/dagger"  ;


const   DENO_IMAGE_REPOSITORY   =   "denoland/deno"  ;


@  object  ()
export class DenoMailValidator {
container: Container;


constructor  (
@  argument  ({ defaultPath:   "/"  , ignore: [  "dagger"  ,   "**.md"  ] })
source: Directory,


denoVersion:   string   =   "2.2.4"
) {
this.container   =   dag
.  container  ()
.  from  (  `${DENO_IMAGE_REPOSITORY}:alpine-${denoVersion}`  )
.  withDirectory  (  "/app"  , source)
.  withWorkdir  (  "/app"  )
.  withExec  ([  "deno"  ,   "install"  ]);
}


/**
* Execute the project to verify the given email.
*/
@  func  ()
async   checkEmail  (email:   string  ): Promise {
return   await this.container.  withExec  ([  "deno"  ,   "run"  ,   "main.ts"  , email]).  stdout  ();
}


/**
* Build and return the compiled binary.
*/
@  func  ()
build  (): File {
return   this.container.  withExec  ([  "deno"  ,   "compile"  ,   "-o"  ,   "email-validator"  ,   "main.ts"  ]).  file  (  "email-validator"  );
}


/**
* Create a lightweight container with the compiled binary and publish
* it to a registry.
*/
@  func  ()
async   publish  (): Promise {
return   await dag
.  container  ()
.  from  (  "debian:bullseye-slim"  )
.  withFile  (  "/bin/email-validator"  , this.  build  ())
.  withEntrypoint  ([  "/bin/email-validator"  ])
.  publish  (  "ttl.sh/deno-mail-validator"  );
}
}
```


#### See it in action


You can also directly observe the module’s behavior in Dagger Cloud:


#### Ready to build with Deno?


As a Deno developer, you now have the ability to create Dagger workflows using the tools you’re already using - no context switching or compatibility bridges required! It’s the best of both worlds - Deno’s simplicity, speed, and security combined with Dagger’s portability, modularity, and observability.


As always, we’d love to hear what you think — share your feedback, and ask questions in the Dagger Discord or on GitHub. Join the discussion in our[Deno Discord channel](https://discord.com/channels/707636530424053791/1285644350524756150) , share what you’re building, and[report issues in our GitHub issue tracker](https://github.com/dagger/dagger/issues) .


Happy hacking! 🚀
