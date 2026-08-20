---
schema_version: "1.0.0"
document_id: "4c82562ee8adaf7e2d480c65e0ffe112087cfcc496919fde2b15d4bd58e61b29"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/typescript-best-practices-large-projects/"
published_at: "2026-07-26T00:00:00+00:00"
first_seen_at: "2026-07-26T19:47:49.227479+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:f49b7af6e7a85c047a1aa4270977d79e98e4fc8e920c96108444b915d309f036"
---

# TypeScript Best Practices for Large Projects

At scale, TypeScript pays off only with disciplined consistency: strict mode as the baseline, explicit and validated boundaries, types generated at the edges so teams can’t drift, and a small set of patterns the whole team actually agrees on. The language stopped being the hard part years ago — the hard part is keeping a million-line, multi-contributor codebase refactorable without a type-safety regression sneaking in on every PR. This guide covers the conventions, compiler flags, and architectural patterns that hold up at that size, with a migration path for the relaxed codebase you probably inherited, and it’s current to the two releases that reset the 2026 landscape: TypeScript 6.0 (GA) and 7.0 (Release Candidate).


## Key Takeaways


- As of TypeScript 6.0 (released March 23, 2026),` strict` defaults to` true` at the compiler level, so on a modern large codebase strict mode is the starting line, not the finish line.
- The two flags that actually move the needle on a large codebase aren’t in` strict` at all:` noUncheckedIndexedAccess` and` exactOptionalPropertyTypes` must be enabled explicitly, and they catch the array-index and optional-property bugs strict silently allows.
- Generated types are the single highest-leverage practice for a multi-team codebase: when frontend and backend both derive types from one OpenAPI or Prisma schema, the two sides physically cannot drift, and CI fails the moment the contract changes.
- Static types are a compile-time promise, not a runtime check — a response typed as` User` is only *asserted* to be one, which is why every external boundary needs runtime validation in addition to a generated type.
- Microsoft reports TypeScript 7.0’s Go-based compiler is often ~10× faster than 6.0 on large codebases, and at its June 2026 Release Candidate it ships inside the standard` tsc` binary and` typescript` package.


## Compiler discipline: strict mode is the floor, not the achievement


Every thin best-practices post still tells you to “enable strict mode” as if it’s a heroic opt-in. That framing is now obsolete. As of[TypeScript 6.0’s release notes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html) ,` strict` is` true` by default at the compiler level — if you were relying on the old default of` false` , you now have to set` "strict": false` explicitly. TypeScript 6.0 was[announced on March 23, 2026](https://devblogs.microsoft.com/typescript/announcing-typescript-6-0/) , and is intended to be the last release based on the current JavaScript codebase. So on any project that updates its compiler, strict is the assumed baseline.


The real large-project upgrade is the two high-value flags` strict` does *not* bundle.` strict` toggles roughly nine type-safety checks (` noImplicitAny` ,` strictNullChecks` , and friends), but it leaves out[noUncheckedIndexedAccess](https://www.typescriptlang.org/tsconfig/noUncheckedIndexedAccess.html) , which adds` undefined` to every un-declared index access, and[exactOptionalPropertyTypes](https://www.typescriptlang.org/tsconfig/exactOptionalPropertyTypes.html) , which distinguishes a property set to` undefined` from an absent one. These catch the exact bugs that slip through a “strict” codebase: the array lookup that assumes an element exists, and the optional field that’s present-but-undefined.


A version-stamped large-project` tsconfig.json` (TypeScript 6.0.x):


```text
{
"  compilerOptions  "  : {
"  strict  "  :   true  ,                        // default since 6.0; keep it explicit for older toolchains
"  noUncheckedIndexedAccess  "  :   true  ,      // arr[i] is T | undefined, not T
"  exactOptionalPropertyTypes  "  :   true  ,    // { x?: number } rejects { x: undefined }
"  verbatimModuleSyntax  "  :   true  ,          // enforce type-only imports (see build perf)
"  noImplicitOverride  "  :   true  ,
"  noFallthroughCasesInSwitch  "  :   true  ,
"  composite  "  :   true                      // required for project references
}
}
```


### The incremental-strictness migration playbook


If you inherited a relaxed codebase, don’t flip every flag at once — apply strictness directory-by-directory, track a` type-coverage` percentage in CI, and ratchet the threshold up so coverage can never regress. A 200k-line app with thousands of implicit` any` s won’t compile clean on day one, and a 2,800-error PR is unreviewable.


A realistic sequence:


1. Turn` strict: true` on globally but scope enforcement: keep a permissive base` tsconfig` and add stricter` tsconfig.json` files per feature directory using project references, tightening the worst offenders first.
2. Add[type-coverage](https://github.com/plantain-00/type-coverage) to CI as a ratchet — fail the build if the typed-symbol percentage drops below the last recorded number. Coverage can plateau but never regress.
3. Stage the extra flags (` noUncheckedIndexedAccess` ,` exactOptionalPropertyTypes` ) with` // @ts-expect-error` on the remaining violations, then burn the list down.` @ts-expect-error` self-reports when a suppression becomes unnecessary, so the backlog can’t silently rot.


## Type design at scale


Good types at scale make illegal states uncompilable and make domain mistakes loud. Three patterns do most of the work; the rest is consistency.


**The` interface` vs` type` rule, stated once:** use` interface` for public, extensible object contracts (it supports declaration merging and tends to produce better error messages on large object shapes), and use` type` for unions, intersections, mapped, and conditional types. That’s the whole debate. Pick the rule, lint it, move on.


### Make impossible states unrepresentable


Boolean-flag soup is the most common source of “this should never happen” bugs in a large UI codebase. The type below permits sixteen combinations, most of them nonsense —` isLoading` and` error` set at once,` data` present during an error:


```text
// Anti-pattern: every field independent, impossible states allowed
interface   RequestState  <  T  > {
isLoading  :   boolean  ;
isError  :   boolean  ;
data  ?:   T  ;
error  ?:   Error  ;
}
```


A discriminated union collapses that to exactly the states that can occur, and the compiler forces you to handle each one:


```text
type   RequestState<  T  >   =
|   {   status  :   "  idle  "   }
|   {   status  :   "  loading  "   }
|   {   status  :   "  success  "  ;   data  :   T   }
|   {   status  :   "  error  "  ;   error  :   Error   };


function   render  <  T  >(  state  :   RequestState  <  T  >) {
switch (  state  .  status  ) {
case   "  success  "  :
return   state  .  data  ;     // data exists only here
case   "  error  "  :
return   state  .  error  ;    // error exists only here
// a missing case is a compile error with a proper exhaustiveness check
}
}
```


Session replays of boolean-flag request state frequently reveal the failure mode this pattern eliminates: a UI that renders a spinner and stale data simultaneously because two independent booleans drifted out of sync.


### Brand your domain IDs


Branded types turn` UserId` and` OrderId` into incompatible types even though both are` string` at runtime, making it a compile error to pass one where the other is expected:


```text
declare   const   brand  :   unique   symbol  ;
type   Brand<  T  ,   B  >   =   T   &   {   readonly   [  brand  ]  :   B   };


type   UserId   =   Brand  <  string  ,   "  UserId  "  >;
type   OrderId   =   Brand  <  string  ,   "  OrderId  "  >;


const   asUserId   =   (  s  :   string  )   =>   s   as   UserId  ;


function   cancelOrder  (  id  :   OrderId  ) {   /* ... */   }


const   u   =   asUserId  (  "  u_123  "  );
cancelOrder  (  u  );   // ❌ Argument of type 'UserId' is not assignable to 'OrderId'
```


In a function signature with five string arguments, branding is the difference between a transposed-argument bug found at compile time and one found in production.


**Prefer` unknown` over` any` .**` any` disables the checker and propagates silently;` unknown` forces a narrowing step before use. Ban` any` in lint and treat every external value —` JSON.parse` ,` catch` bindings, untyped library returns — as` unknown` until proven otherwise. Use` as const` on literal config and lookup tables so they infer as narrow literal types rather than widened primitives.


## Model and generate types at your boundaries


The highest-leverage architectural decision in a large codebase is how you type the edges. Two rules.


First, don’t reuse one` User` type across the wire, the database, and the UI — model the API response, the DTO, and the domain entity as three distinct types so a change at one boundary can’t silently corrupt another. The shape your backend serializes, the shape your ORM returns, and the shape your components consume diverge over time; collapsing them into one type couples every layer to every other.


Second, **generate** the boundary types instead of hand-writing them. When frontend and backend both derive their types from one schema, the two sides physically cannot drift, and CI fails the moment the contract changes. Use[openapi-typescript](https://github.com/openapi-ts/openapi-typescript) to turn an OpenAPI 3.0/3.1 document into runtime-free types,[Prisma](https://www.prisma.io/docs) for database-derived types, or[GraphQL Code Generator](https://the-guild.dev/graphql/codegen) for typed operations. Regenerate in CI and fail on a diff:


```text
# CI step: regenerate and fail if the committed types are stale
npx   openapi-typescript   ./openapi.yaml   -o   ./src/api/schema.gen.ts
git   diff   --exit-code   ./src/api/schema.gen.ts
```


But a generated type is still only a compile-time promise. Static types are a compile-time promise, not a runtime check — a response typed as` User` is only *asserted* to be one, which is why every external boundary needs runtime validation in addition to a generated type. Validate the actual payload with a schema library like[Zod](https://zod.dev/) or[Valibot](https://valibot.dev/) and derive the static type from the schema, so one definition guards both layers:


```text
import   {   z   }   from   "  zod  "  ;


const   User   =   z  .  object  ({   id  :   z  .  string  (),   email  :   z  .  email  ()   });
type   User   =   z  .  infer  <  typeof   User  >;


const   res   =   await   fetch  (  "  /api/me  "  );
const   user   =   User  .  parse  (  await   res  .  json  ());   // throws if the real shape lies
```


The case for validating boundaries, not just typing them, is empirical: static types vanish at runtime, and session replay is one technique for *seeing* the failures types can’t — the moment a real API response doesn’t match its declared shape and the UI breaks in a user’s session.


## Organize types for a multi-contributor codebase


- Colocate types with the code that uses them — same file, or a sibling` *.types.ts` — and reserve a` types/index.ts` (or a dedicated package) for genuinely shared contracts only.
- Organize by feature/domain folder, not by technical layer, so a feature’s types, components, and logic live together and ownership is obvious.
- In a monorepo, wire packages together with[project references](https://www.typescriptlang.org/docs/handbook/project-references.html) and` paths` mapping so imports cross clean module boundaries (` @org/billing` ) instead of brittle` ../../../` chains, and so the compiler enforces the dependency graph.


## Build performance in 2026: the native compiler changes the math


The build-performance conversation has fundamentally changed, and it’s no longer about shaving seconds with` tsc` flags. TypeScript announced the 7.0 Release Candidate on June 18, 2026; with native code speed and shared-memory parallelism, it is often about 10 times faster than TypeScript 6.0. Microsoft’s headline benchmark checked the ~1.5M-line VS Code codebase in roughly 7.5 seconds versus 77.8 on the previous compiler, though the multiple is smaller on small projects.


The packaging is the part to get right. The main practical change in the RC is packaging:[the Go-based rewrite moved from a separate native-preview package into the regular TypeScript npm package](https://visualstudiomagazine.com/articles/2026/06/22/typescript-7-0-rc-moves-microsofts-go-rewrite-into-the-mainline-compiler.aspx) , so TypeScript 7.0 is now ready for broader testing as the normal` tsc` compiler. Install it with` npm install -D typescript@rc` and run the standard` tsc` binary — the older` tsgo` /` @typescript/native-preview` packages now carry nightlies only. As of late June 2026, the[latest stable is TypeScript 6.0.3](https://www.npmjs.com/package/typescript) , with 7.0 at RC and stable GA expected within approximately one month of the RC. Treat the version/stage as volatile and re-check at adoption time.


The structural levers you control in your own config remain worth it on any compiler: project references for incremental, dependency-aware builds, and type-only imports enforced by[verbatimModuleSyntax](https://www.typescriptlang.org/tsconfig/verbatimModuleSyntax.html) so type-only symbols are erased and never emitted as runtime imports.` verbatimModuleSyntax` (introduced in 5.0) is the current recommended approach; the flags it replaced,` importsNotUsedAsValues` and` preserveValueImports` , were made no-ops in 5.5 and are an error to specify as of 6.0.


```text
import   type   {   User   }   from   "  ./user  "  ;    // erased entirely from JS output
import   {   fetchUser   }   from   "  ./api  "  ;     // value import, preserved
```


## Automate the guardrails


Conventions that aren’t enforced decay. Run[typescript-eslint](https://typescript-eslint.io/) with type-aware rules (` no-explicit-any` ,` no-floating-promises` ,` no-misused-promises` ) so the patterns above are checked mechanically, not in review. Leave type-only-import enforcement to` verbatimModuleSyntax` rather than the` consistent-type-imports` lint rule — running both is redundant and can produce conflicting errors. Run` tsc --noEmit` in CI on every PR as a hard gate, alongside the` type-coverage` ratchet from the migration playbook. And keep one human guardrail above all the automation: clarity over cleverness. A deeply nested conditional-and-mapped type that takes a senior engineer ten minutes to read is a liability, not a flex — most large-project type code should be boring, legible, and obvious.


The throughline is consistency, not sophistication. Turn on the two flags` strict` omits, make your illegal states uncompilable, generate and validate your boundaries, and let CI enforce the rest. Pick the version-pinned` tsconfig` above as your starting baseline this week, then point the` type-coverage` ratchet at your worst directory and start climbing.


## FAQs


Is strict mode enough for a large TypeScript project?


No. As of TypeScript 6.0, strict already defaults to true at the compiler level, so it is the baseline rather than an achievement. The two flags that matter most on a large codebase are not in the strict family at all: noUncheckedIndexedAccess, which adds undefined to un-declared index access, and exactOptionalPropertyTypes, which distinguishes a property set to undefined from an absent one. Enable both explicitly, then add boundary types and runtime validation.


What is the difference between interface and type in TypeScript, and when should I use each?


Use interface for public, extensible object contracts because it supports declaration merging and tends to produce clearer error messages on large object shapes. Use type for unions, intersections, mapped types, and conditional types, which interface cannot express. For a large team the practical rule is to pick this convention once, enforce it with a lint rule, and stop debating it. Both compile to identical type checks for plain object shapes, so the choice is about expressiveness and consistency, not capability.


Do generated types from OpenAPI or Prisma make runtime validation unnecessary?


No. A generated type is a compile-time promise only. A JSON response typed as User is merely asserted to match that shape; the compiler never inspects the actual payload at runtime, so a backend change or a null field still slips through. Generated types prevent the frontend and backend from drifting on the contract, but you still need a schema library like Zod or Valibot to validate the real payload at every external boundary. Derive the static type from the schema so one definition guards both layers.


How do I install and run TypeScript 7.0 at its Release Candidate stage?


Install it with npm install -D typescript@rc and run the standard tsc binary. At the June 2026 Release Candidate, the Go-based native compiler moved out of the separate native-preview package and into the regular typescript npm package, so there is no longer a distinct tsgo binary for the RC; the older tsgo and typescript native-preview packages now carry nightlies only. Microsoft reports 7.0 is often about ten times faster than 6.0 on large codebases. Treat the version and stage as volatile and re-check before adopting.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
