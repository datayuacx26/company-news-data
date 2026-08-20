---
schema_version: "1.0.0"
document_id: "86b3a2e021394ff4239b843cd350bef331e992f31e7725015ccd883c7eaf3dcf"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/3-type-systems-not-typescript/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T13:02:10.800608+00:00"
fetched_at: "2026-08-10T13:02:12.655554+00:00"
content_hash: "sha256:3ce4515061d5aae7f05e199d05c549456f15dda3946043e23da9e5e982ba02c2"
---

# 3 Type Systems That Aren't TypeScript

TypeScript is the default way to type JavaScript, but it is not the only one. If you have ever added a build step to a small project just to get a handful of type annotations, or watched` tsc` crawl through a large codebase, you have probably wondered whether there is a lighter way to get type safety. There is, and there is more than one.


The three most credible non-TypeScript type systems for JavaScript in 2026 are **JSDoc with` // @ts-check`** , **ReScript** , and **Flow** : one gives you type-checking with no build step, one gives you stronger guarantees than TypeScript, and one is a legacy checker you’d only adopt to maintain existing code. Everything else commonly listed in “TypeScript alternatives” roundups (Deno, Dart, Kotlin/JS) is a runtime or a cross-platform language, not a type system layered on JavaScript.


This is a comparative survey for developers who already know TypeScript and want the trade-offs stated plainly: language vs. checker, build step or not, soundness against TypeScript’s intentionally unsound model, and ecosystem health right now.


## Key Takeaways


- JSDoc with` // @ts-check` type-checks plain` .js` files using the same TypeScript language service, with no build step and no` .ts` files, a capability that has shipped since TypeScript 2.3.
- ReScript is a separate compile-to-JavaScript language with a sound, fully inferred, nominal type system from the OCaml family — configured via` rescript.json` , not the removed` bsconfig.json` .
- Flow and ReScript are both written in OCaml, but Flow’s external adoption has declined sharply while it remains in production inside Meta.
- Pick JSDoc for incremental, build-free typing; ReScript for maximum soundness on greenfield projects; Flow essentially only to maintain an existing Flow codebase.


## The three real TypeScript alternatives in 2026


A genuine “type system for JavaScript” either checks types on JavaScript source (a checker or annotations layer) or compiles a typed language down to JavaScript. That definition includes JSDoc-plus-` @ts-check` , ReScript, and Flow. It excludes Deno (a runtime that happens to run TypeScript), and Dart and Kotlin/JS (separate languages that target JavaScript as one of several backends). Those are worth knowing, but they answer a different question.


For baseline context: as of mid-2026, TypeScript 7.0 is the current stable release. It shipped on[July 8, 2026](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0/) as a native Go port of the compiler, which Microsoft reports is typically 8x to 12x faster than TypeScript 6.0 on full builds. TypeScript 6.0 was the last release built on the old JavaScript codebase, a bridge whose type-checking behavior 7.0 preserves. One gap remains: 7.0 ships without a stable programmatic API, so tooling for Vue, Svelte, and Angular template type-checking waits for[TypeScript 7.1](https://www.techtimes.com/articles/320049/20260710/typescript-7-now-stable-10-faster-builds-not-vue-svelte-yet.htm) , expected around October 2026. Keep that in mind: it changes what carries forward for JSDoc, below.


Criterion JSDoc +` @ts-check` ReScript Flow TypeScript (baseline)


What it is Annotations, checked by the TS language service Compile-to-JS language Static type checker Compile-to-JS superset


Build step None, types are comments Required (` .res` →` .js` ) None at runtime; Babel strips types Required


Soundness Same unsound structural model as TS Sound, inferred, nominal Stricter than TS, still not fully sound Intentionally unsound, structural


Ecosystem 2026 Rising; the no-build default Small but active Declined externally; Meta-internal Dominant


When to reach for it Incremental typing, zero tooling Max soundness, greenfield Maintaining existing Flow code


## JSDoc +` @ts-check` : TypeScript’s type system without the build step


**What it is:** JSDoc-based typing lets you annotate plain` .js` files with structured comments and type-check them using the exact same engine as TypeScript: the TypeScript language service. This is distinct from JSDoc the documentation generator; here the annotations drive type-checking. The[TypeScript handbook documents --checkJs](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html) as the flag that reports errors in` .js` files, available since TypeScript 2.3.


**How you adopt it:** Add a single comment to the top of a file, or flip two` tsconfig` flags for the whole project.[VS Code’s JavaScript docs](https://code.visualstudio.com/docs/nodejs/working-with-javascript) describe` // @ts-check` as the way to try checking a few files without enabling it everywhere.


```text
// @ts-check


/**
*   @  typedef   {  { id: number, name: string }  }   User
*/


/**
*   @  param   {  User  }   user
*   @  returns   {  string  }
*/
function   greet  (  user  ) {
return   `  Hi,   ${  user  .  name  }`  ;
}
```


For a whole project, skip the per-file comment:


```text
{
"  compilerOptions  "  : {
"  allowJs  "  :   true  ,
"  checkJs  "  :   true
}
}
```


Because JSDoc types are erasable comments, the file runs as standard JavaScript in any browser or runtime with no compile step, which is also why you can adopt it one file at a time. The trade-off: you inherit TypeScript’s model exactly, including its deliberate unsoundness, and the annotation syntax is more verbose than` .ts` . One currency note for the 7.0 era:[Microsoft rewrote JavaScript type-checking from the ground up for TypeScript 7.0](https://devblogs.microsoft.com/typescript/progress-on-typescript-7-december-2025/) and dropped a few less-used tags, so TypeScript 7.0 does not recognize the @enum and @constructor tags.


**When to pick it:** You want incremental type safety on an existing JavaScript codebase, or a library that ships plain JS, and you don’t want a compile step in the way.


## ReScript: sound, nominal, fully inferred types


**What it is:** ReScript is a separate compile-to-JavaScript language, not an annotations layer, with a type system inherited from OCaml. TypeScript’s type system is intentionally unsound: it accepts some incorrect programs to stay compatible with JavaScript’s runtime semantics. That’s precisely the gap[ReScript](https://rescript-lang.org/) closes: it is fully inferred and, as the project puts it, has no` any` , no magic types, no surprise` undefined` . Unlike TypeScript’s structural typing, where any object with the right shape satisfies a type, ReScript’s records and variants are nominal, so two structurally identical types are not interchangeable unless declared so.


**How you adopt it:** Install the compiler and configure` rescript.json` ,[the single, mandatory build file](https://v11.rescript-lang.org/docs/manual/v11.0.0/build-configuration) needed for a ReScript project (it was` bsconfig.json` in versions prior to ReScript 11).[ReScript 12, released November 25, 2025](https://rescript-lang.org/blog/release-12-0-0/) , removes` bsconfig.json` support entirely and defaults module output to ES modules.


```text
{
"  name  "  :   "  my-app  "  ,
"  sources  "  : {   "  dir  "  :   "  src  "  ,   "  subdirs  "  :   true   },
"  package-specs  "  : {   "  module  "  :   "  esmodule  "  ,   "  in-source  "  :   true   },
"  suffix  "  :   "  .res.js  "
}
```


You can freely choose the suffix of the generated JS files; the team recommends` .res.js` or` .res.mjs` , which is also what the[official create-rescript-app templates](https://rescript-lang.org/blog/release-11-0-0/) use. A minimal module:


```text
let add = (a: int, b: int): int => a + b
let result = add(1, 2)
Console.log(result)
```


The cost is real: it’s a different language with its own syntax, you write interop bindings to consume JavaScript libraries, and the ecosystem is far smaller than TypeScript’s. But ReScript was made with gradual adoption in mind: if you ever want to go back to plain JavaScript, you remove the source files and keep the clean JavaScript output.


**When to pick it:** A greenfield project where you want the strongest possible type guarantees and are willing to commit to a language, not just annotations.


## Flow: still around, largely superseded


**What it is:**[Flow](https://flow.org/) is an open-source static type checker for JavaScript,[built by Facebook/Meta and written in OCaml](https://engineering.fb.com/2014/11/18/web/flow-a-new-static-type-checker-for-javascript/) . Like Flow, ReScript descends from OCaml, but the two now sit at opposite ends of adoption. Flow is a checker, not a language: you annotate` .js` files, mark them with` // @flow` , and strip the types with Babel at build time.


```text
// @flow
function   add  (  a  :   number  ,   b  :   number  )  :   number   {
return   a   +   b  ;
}
```


**Adoption reality in 2026:** Flow remains in production inside Meta across millions of files of JavaScript and React, but its external ecosystem has contracted sharply: fewer library definitions, fewer tutorials, and thinner tooling than TypeScript. Migration of major Meta-adjacent projects off Flow toward TypeScript has been floated in community discussion, though no primary source confirms a firm plan. Treat older “Flow has a bright future” comparisons as out of date; the momentum moved to TypeScript years ago.


**When to pick it:** Realistically, only when you are maintaining an existing Flow codebase. For anything new, the other two options are stronger bets.


## Verdict: match the tool to the situation


Reach for **JSDoc with` @ts-check`** when you want incremental typing with zero build tooling and full compatibility with plain JavaScript. It is the most underused and most practical answer, since it is TypeScript’s own checker pointed at` .js` . Choose **ReScript** when you’re starting fresh and want maximum type soundness, and you accept a separate language and interop bindings as the price. Keep **Flow** only to maintain code already written in it. If you want type safety without leaving JavaScript today, add` // @ts-check` to one file and watch the errors surface.


## FAQs


Can you use JSDoc type-checking without installing TypeScript as a dependency?


Editors like VS Code ship the TypeScript language service built in, so adding // @ts-check to a .js file gives you type-checking with no npm install at all. To run the same check on the command line or in CI, you install the typescript package and run tsc with allowJs and checkJs enabled. The types themselves stay in JSDoc comments, so nothing is added to your shipped JavaScript either way.


What is the difference between structural typing in TypeScript and nominal typing in ReScript?


Structural typing, used by TypeScript, treats any object with the right shape as satisfying a type, so two unrelated types with identical fields are interchangeable. ReScript's records and variants are nominal, meaning two structurally identical types are treated as distinct unless you explicitly declare a relationship. Nominal typing prevents accidental substitution of look-alike types, which is part of why ReScript's system is sound where TypeScript's is intentionally unsound.


Why isn't Deno considered a TypeScript alternative in this comparison?


Deno is a JavaScript and TypeScript runtime, not a type system layered on JavaScript. It runs TypeScript directly by bundling the TypeScript compiler, so it depends on TypeScript rather than replacing it. A genuine alternative must either check types on JavaScript source or compile a typed language to JavaScript. The same reasoning excludes Dart and Kotlin/JS, which are separate languages that target JavaScript as one of several compile backends.


Does moving to TypeScript 7.0 break existing JSDoc-typed JavaScript projects?


Mostly no, but the Go-based TypeScript 7.0 rewrote JavaScript type-checking from the ground up and dropped a few less-used JSDoc tags, specifically @enum and @constructor, which 7.0 no longer recognizes. Projects relying on those tags need to migrate them to supported patterns. Common tags like @param, @returns, @typedef, and @template continue to work, so most JSDoc-typed codebases carry forward unchanged. TypeScript 7.0 has been the stable release since July 2026, so run your project against it to see the exact diff.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
