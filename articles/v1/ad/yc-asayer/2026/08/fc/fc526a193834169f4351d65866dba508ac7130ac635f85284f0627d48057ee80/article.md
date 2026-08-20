---
schema_version: "1.0.0"
document_id: "fc526a193834169f4351d65866dba508ac7130ac635f85284f0627d48057ee80"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/typescript-satisfies-operator/"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-02T13:12:05.513475+00:00"
fetched_at: "2026-08-05T03:48:28.288743+00:00"
content_hash: "sha256:537254700f64c35ff80af05891e61e2cf3cc9119950b98d0fe5bad610b15f8f2"
---

# A Practical Guide to TypeScript's satisfies Operator

The` satisfies` operator checks a value against a type without changing the value’s inferred type — so you get the type’s safety and the value’s narrowness at the same time. That single behavior fixes a specific, everyday annoyance: a colon annotation on a config object protects you from wrong values but throws away the literal keys and narrow types you wanted to keep. This guide gives you the mental model, the canonical example, and a decision rule for choosing` satisfies` over` as` or a plain colon annotation. Every snippet is written for TypeScript 4.9 and up.


## Key Takeaways


- ` satisfies` validates a value against a type while preserving the value’s narrow inferred type, so autocomplete and literal narrowing survive.
- With a colon annotation the declared type wins and the value is widened to fit it; with` satisfies` the value wins and the type is only used to validate it.
- ` as` doesn’t check your value — it overrides the checker, which is why` const user = {} as User` compiles cleanly and throws at runtime when you read` user.name` .
- Annotating and using` satisfies` at once (` const x: T = {…} satisfies T` ) is redundant: the colon takes precedence and defeats the narrowing you wanted.
- ` satisfies` is compile-time only and emits zero JavaScript; reach for a runtime validator like Zod when data arrives from a network or a file.


## The problem` satisfies` solves


Two patterns push developers toward` satisfies` . The first is a colon annotation on a keyed object, which widens the type and destroys autocomplete.[Matt Pocock’s routes example](https://www.totaltypescript.com/clarifying-the-satisfies-operator) shows it: annotate with` Record<string, {}>` and you can read any key, even a garbage one, with no error.


```text
const   routes  :   Record  <  string  , {}>   =   {
"  /  "  :   {},
"  /users  "  :   {},
"  /admin/users  "  :   {},
};
routes  .  awdkjanwdkjn  ;   // No error — the type is now Record<string, {}>
```


The second pattern is a union-typed property. In[freeCodeCamp’s example](https://www.freecodecamp.org/news/typescript-satisfies-operator/) , a property typed as a union of a string literal and an object won’t let you call string methods without a manual guard.


```text
type   Info   =   "  John  "   |   "  Jack  "   |   {   id  :   number  ;   age  :   number   };
type   Person   =   {   myInfo  :   Info  ;   myOtherInfo  :   Info   };


const   applicant  :   Person   =   {   myInfo  :   "  John  "  ,   myOtherInfo  :   {   id  :   123  ,   age  :   22   }   };
applicant  .  myInfo  .  toUpperCase  ();
// Property 'toUpperCase' does not exist on type 'Info'
```


You end up writing` if (typeof applicant.myInfo === "string")` before every access. Both problems share a root cause: the colon annotation replaced your specific value type with a wider declared one.


## What does the satisfies operator do?


` satisfies` validates that an expression matches a type without changing the type TypeScript infers for it. It was[introduced in TypeScript 4.9](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html) , released[November 15, 2022](https://devblogs.microsoft.com/typescript/announcing-typescript-4-9/) , and it behaves identically through the current[TypeScript 6.0 stable](https://www.npmjs.com/package/typescript) and the[7.0 release candidate](https://devblogs.microsoft.com/typescript/announcing-typescript-7-0-rc/) . Because the 7.0 Go port kept type-checking semantics structurally identical to 6.0,` satisfies` enforces the exact same rules on the new compiler.


The mental model, in Pocock’s framing: **with a colon annotation the type beats the value; with` satisfies` the value beats the type.** When you use` satisfies` , TypeScript infers the narrowest possible type and uses the annotation only to check it. It is compile-time only — it emits zero JavaScript and has no runtime cost, so it can catch a typo or a wrong value type before the code ever runs.


## The canonical satisfies example


The fix is to move the annotation from the colon to a trailing` satisfies` : both problems disappear at once — you keep the narrow literal types *and* you still get an error on a wrong value.


```text
const   routes   =   {
"  /  "  :   {},
"  /users  "  :   {},
"  /admin/users  "  :   {},
} satisfies   Record  <  string  , {}>;


routes  .  awdkjanwdkjn  ;
// Property 'awdkjanwdkjn' does not exist on type
// '{ "/": {}; "/users": {}; "/admin/users": {}; }'
```


Autocomplete on` routes` now lists the real paths. Validation still holds: assign something the annotation forbids and the compiler rejects it.


```text
const   routes   =   {
"  /  "  :   null  ,   // Type 'null' is not assignable to type '{}'
} satisfies   Record  <  string  , {}>;
```


The same trick fixes the union case.` applicant.myInfo` narrows to the literal` "John"` , so` .toUpperCase()` is legal with no guard:


```text
const   applicant   =   {
myInfo  :   "  John  "  ,
myOtherInfo  :   {   id  :   123  ,   age  :   22   },
} satisfies   Person  ;


applicant  .  myInfo  .  toUpperCase  ();   // OK — inferred as "John"
```


## satisfies vs as vs a colon annotation


` as` doesn’t check your value — it overrides the type checker, which is why` const user = {} as User` compiles cleanly and then throws at runtime the moment you read` user.name` . That is the core distinction across the three tools:


Tool Checks the value? Keeps narrow inference? Can you lie to TS? Use when


` : Type` (colon) Yes No — widens to the type No You deliberately want a wider type


` satisfies Type` Yes Yes No You want validation *and* narrow inference


` as Type` No N/A Yes Almost never as a default


The runtime danger is concrete:


```text
type   User   =   {   id  :   string  ;   name  :   {   first  :   string  ;   last  :   string   }   };
const   user   =   {}   as   User  ;
user  .  name  .  first  ;   // No error in the IDE — throws at runtime
```


` as` also rots silently. This compiles today, but add one required field to` User` and` defaultUser` becomes invalid with **no error** :


```text
type   User   =   {   id  :   string  ;   name  :   string   };
const   defaultUser   =   {   id  :   "  123  "  ,   name  :   "  Matt  "   }   as   User  ;
```


This is the class of bug session replay is built to surface: you see the real object shape at the moment a property access threw, not the shape you asserted. Swap in` satisfies` and the compiler flags the missing field immediately.


**The rule of thumb: never reach for` as` by default — use` satisfies` to validate while keeping inference, and use a plain colon annotation only when you deliberately want a wider type you’ll reassign later.**


### The redundant-annotation trap


Annotating and using` satisfies` at once —` const joe: TUser = {…} satisfies TUser` — is redundant: the colon annotation takes precedence and silently defeats the narrowing` satisfies` was meant to preserve. The[Refine.dev writeup](https://refine.dev/blog/typescript-satisfies-operator/) shows the fallout — accessing a nested property fails because the declared type won and the internal narrowing was thrown away. Pick one. If you want narrowing, drop the colon.


## Where satisfies earns its place


Use` satisfies` for typed config,` Record` -keyed maps, and discriminated-union values you want to keep narrowed. Theme and palette maps are the archetype — the official` palette` example validates each RGB entry while keeping the per-key literal types intact. For optional keys, wrap the record in` Partial` so missing keys are allowed but present ones are still checked:


```text
type   Keys   =   "  id  "   |   "  name  "   |   "  email  "   |   "  age  "  ;


const   person   =   {
id  :   12345  ,
name  :   "  Jacky  "  ,
email  :   "  jacky@test.com  "  ,
} satisfies   Partial  <  Record  <  Keys  ,   string   |   number  >>;


person  .  name  .  toUpperCase  ();   // narrowed to string
```


## When not to use satisfies


Skip` satisfies` for a plain object where a simple` : Type` annotation already says everything you need. Skip it when you *want* the wider type — if you plan to reassign a variable later,` satisfies` blocks you because it locks in the narrow inferred type:


```text
// colon annotation — reassignment is fine
let   id  :   string   |   number   =   "  123  "  ;
id   =   456  ;   // OK


// satisfies — the value wins, so it's narrowed to string
let   id2   =   "  123  "   satisfies   string   |   number  ;
id2   =   456  ;   // Type 'number' is not assignable to type 'string'
```


And skip it entirely for data you don’t control.` satisfies` never runs, so it can’t validate a JSON payload or a form submission at runtime — reach for a runtime schema validator such as[Zod](https://zod.dev/) or io-ts when the data crosses a network or file boundary.


Reach for` satisfies` whenever you’re typing a literal you also want to keep specific — configs, route maps, palettes, union values. Replace your reflexive` as` with it, keep colon annotations for the cases where a wider type is the point, and your next config object will hold both its safety and its autocomplete.


## FAQs


Does the satisfies operator work in JavaScript files with JSDoc?


Yes. TypeScript 5.0 added a @satisfies JSDoc tag that does exactly what the satisfies operator does for TypeScript files. In a JavaScript file you write the tag above a declaration, for example an @satisfies annotation naming a type, and the checker validates the value against that type while keeping the narrow inferred type. This lets JSDoc-typed JavaScript projects get the same validation-plus-narrowing benefit without switching to .ts files.


Does satisfies add any runtime cost or generate extra JavaScript?


No. satisfies is a compile-time only, type-level operator that emits zero JavaScript, so it has no runtime cost and no bundle-size impact. The keyword and the type after it are erased during compilation, exactly like a colon annotation. Because nothing runs, it also cannot validate data at runtime, which is why network payloads or form input still need a runtime schema validator such as Zod.


Why does my object still fail type-checking when I use both a colon annotation and satisfies?


Because the colon annotation always takes precedence and the satisfies clause becomes dead syntax. Writing const config: Theme = {…} satisfies Theme means the declared Theme type wins, so the value is widened to Theme and the narrow inference satisfies was meant to preserve is discarded. Accessing a nested literal property then fails as if satisfies were absent. Drop the colon and keep only the trailing satisfies to retain narrowing.


When should I use Zod instead of satisfies for validating data?


Use Zod, or another runtime schema validator like io-ts, whenever the data arrives at runtime from a source you do not control, such as a network response, a JSON file, or a form submission. satisfies checks only literals you write in your source code at compile time and emits no runtime code, so it cannot inspect unknown incoming data. Use satisfies for typed config, Record maps, and union values in your own code; use Zod for external boundaries.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
