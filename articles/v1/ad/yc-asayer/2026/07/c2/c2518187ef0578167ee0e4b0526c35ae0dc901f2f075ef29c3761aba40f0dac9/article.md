---
schema_version: "1.0.0"
document_id: "c2518187ef0578167ee0e4b0526c35ae0dc901f2f075ef29c3761aba40f0dac9"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/10-js-interview-questions-testing/"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:ddc0fb3f976be5e3b59d1f0f0b0c6e775529acc60f3a02b1c892306d11b93f08"
---

# 10 JS Interview Questions and What They're Really Testing

Most JavaScript interview questions aren’t asking for the output — they’re asking whether you understand the model that produces it. A senior interviewer who writes` console.log(x); var x = 5;` on the whiteboard already knows it logs` undefined` . What they’re scoring is whether you can explain *why* without reaching for the phrase “hoisting moves it to the top,” because that phrase is the mental model, not the mechanism. The candidates who advance are the ones who can name the runtime behavior, predict the output, and articulate the reasoning out loud in two clean sentences.


This article takes ten questions that map to ten distinct fundamentals — the execution model, the Temporal Dead Zone, closures, lexical scope,` this` binding, the event loop, and coercion — and breaks each one into four parts: the snippet, the exact output and the reason, **what the interviewer is really testing** , and **the follow-up you’ll get hit with next** . Every output is spec-defined and reproducible in any current engine. The behaviors themselves are stable across all current engines — these are language semantics, not version-specific quirks.


## Key Takeaways


- The` var` hoisting question doesn’t test whether you know the output is` undefined` — it tests whether you understand that JavaScript creates variable bindings when it enters a scope, before any line runs.
- ` let` and` const` are hoisted too, but they stay uninitialized in the Temporal Dead Zone until their declaration line, so reading them early throws a` ReferenceError` instead of returning` undefined` .
- In the classic` setTimeout` -in-a-` for` -loop bug,` var` logs the final loop value (` 3 3 3` ) because all callbacks share one function-scoped binding, while` let` logs each iteration’s value (` 0 1 2` ) because it creates a fresh binding per iteration.
- ` this` is not decided where a function is written — it’s decided by how the function is called; arrow functions are the exception because they have no` this` of their own.
- Promise callbacks (microtasks) always run before` setTimeout` callbacks (macrotasks), even with a` 0` ms delay.


## JavaScript interview questions on hoisting and the execution model


### 1. Why does` var` log` undefined` before assignment?


```text
console  .  log  (  x  );   // undefined
var   x   =   20  ;
console  .  log  (  x  );   // 20
```


The first line logs` undefined` , not a` ReferenceError` . The popular explanation is that the declaration is “moved to the top,” but that’s a metaphor. What actually happens: when the engine enters a scope, it instantiates the bindings for that scope *before* executing any statements, and a` var` binding is initialized to` undefined` at that point. The assignment` = 20` stays on its original line and runs in order. This binding-creation step is defined in the[ECMAScript Language Specification](https://tc39.es/ecma262/) under variable environment instantiation — nothing is physically relocated.


**What they’re really testing:** whether you understand that JS has a creation phase before the execution phase. The` undefined` answer is trivia; the two-phase execution model is the competency. See[MDN’s Hoisting glossary entry](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) for the canonical framing.


**Likely follow-up:** “What if it were` let` instead of` var` ?” — which is question 2.


### 2. Why does` let` throw instead of logging` undefined` ?


```text
console  .  log  (  y  );   // ReferenceError: Cannot access 'y' before initialization
let   y   =   20  ;
```


` let` and` const` are hoisted — the binding is created at scope entry — but they are left *uninitialized* until execution reaches the declaration. The window between scope entry and the declaration line is the Temporal Dead Zone, and reading a binding inside it throws` ReferenceError: Cannot access 'y' before initialization` . This is the load-bearing distinction:` var` is initialized to` undefined` at creation;` let` /` const` are not initialized at all until their line runs. MDN documents this under the[let Temporal Dead Zone section](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz) .


**What they’re really testing:** whether you separate *binding creation* from *binding initialization* . A candidate who says “let isn’t hoisted” has the wrong model; the correct answer is that it is hoisted but uninitialized.


**Likely follow-up:** “So why does the TDZ exist?” — say it makes` const` enforceable and turns use-before-declaration into a loud error rather than a silent` undefined` .


Here is the reference table interviewers expect you to reconstruct verbally:


Behavior` var`` let`` const`


Hoisted (binding created at scope entry) Yes Yes Yes


Initialized at creation` undefined` No (TDZ) No (TDZ)


Read before declaration` undefined`` ReferenceError`` ReferenceError`


Scope Function Block Block


Redeclarable in same scope Yes No No


Reassignable Yes Yes No


### 3. Function declaration vs. function expression hoisting


```text
foo  ();   // "I run"
bar  ();   // TypeError: bar is not a function


function   foo  () {   console  .  log  (  "  I run  "  ); }
var   bar   =   function   () {   console  .  log  (  "  I don't  "  ); };
```


A function *declaration* is hoisted whole — the name and the body — so` foo()` works before its definition. A function *expression* assigned to` var bar` hoists only the` bar` binding, initialized to` undefined` . Calling` undefined` throws a` TypeError` , not a` ReferenceError` — the binding exists, it just isn’t a function yet. Getting the error *type* right is the tell here. MDN covers the split under[function declarations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function) .


**What they’re really testing:** whether you grasp that declarations hoist *values* while expressions hoist only *bindings* . This is the same distinction as questions 1 and 2, applied to functions.


**Likely follow-up:** “What if` bar` were declared with` let` ?” — then the early call throws a` ReferenceError` from the TDZ instead of a` TypeError` .


## JavaScript interview questions on closures and scope


### 4. The` setTimeout` -in-a-loop classic


```text
for (  var   i   =   0  ;   i   <   3  ;   i  ++  ) {
setTimeout  (()   =>   console  .  log  (  i  ),   0  );   // 3 3 3
}


for (  let   i   =   0  ;   i   <   3  ;   i  ++  ) {
setTimeout  (()   =>   console  .  log  (  i  ),   0  );   // 0 1 2
}
```


The` var` loop logs` 3 3 3` . There is one function-scoped` i` ; by the time the callbacks fire (after the synchronous loop finishes),` i` is` 3` , and all three closures read that same binding. The` let` loop logs` 0 1 2` because` let` creates a fresh binding for each iteration, so each callback closes over its own` i` . This is the single most-botched snippet on the web, and it stacks three concepts: closures, scope, and the event loop (the callbacks are deferred, so the loop completes first). MDN’s[Closures guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) documents the per-iteration` let` binding.


**What they’re really testing:** whether you can reason about *when* a closure reads a variable versus *when* it was created. This isn’t trivia — it’s the same class of bug that ships to production as stale-closure state in event handlers and React effects. Session replays of stale-closure handlers frequently reveal them logging a value captured from an earlier render.


**Likely follow-up:** “Fix it without` let` .” — wrap the body in an IIFE that takes` i` as an argument, creating a new scope per iteration.


### 5. The private counter


```text
function   makeCounter  () {
let   count   =   0  ;
return   {
increment  :   ()   =>   ++  count  ,
value  :   ()   =>   count  ,
};
}


const   c   =   makeCounter  ();
c  .  increment  ();   // 1
c  .  increment  ();   // 2
c  .  value  ();       // 2
// count is unreachable from outside
```


A closure is a function bundled with the variables it had access to where it was *defined* , not where it’s *called* — which is why the returned methods still reach` count` after` makeCounter` has returned. Because nothing outside exposes` count` directly, it is effectively private. This is encapsulation built from scope, not from a` #private` field.


**What they’re really testing:** whether you understand closures as a memory and encapsulation tool, not just a quiz answer. The follow-up probes whether you know the cost.


**Likely follow-up:** “Does this leak memory?” — the` count` variable stays alive as long as` c` is reachable, because the closure holds a reference; that’s intended here, but uncontrolled closures over large objects are a real leak source.


### 6. Lexical scope: defined, not called


```text
const   x   =   10  ;


function   outer  () {
const   x   =   20  ;
return   inner  ;
}


function   inner  () {
console  .  log  (  x  );   // 10
}


outer  ()();   // 10
```


` inner` logs` 10` , not` 20` . JavaScript resolves free variables by where a function is *defined* in the source, not where it’s *called* .` inner` is defined at the top level, so its` x` resolves to the top-level` 10` — the` x` inside` outer` is irrelevant to it. This is lexical (static) scoping, and it’s what makes closures predictable.


**What they’re really testing:** whether you confuse the call stack with the scope chain. Dynamic-scope languages would log` 20` ; JavaScript does not.


**Likely follow-up:** “Now move the` inner` declaration *inside*` outer` .” — then it resolves to` 20` , because its definition site changed.


## JavaScript interview questions on` this` binding


### 7. What does` this` refer to here?


```text
const   user   =   {
name  :   "  Ada  "  ,
greet  ()   {   return   this  .  name  ;   },
};


const   fn   =   user  .  greet  ;
user  .  greet  ();   // "Ada"
fn  ();           // undefined (or throws in strict mode)
```


` this` is not decided where a function is written — it’s decided by how the function is *called* . Called as` user.greet()` , the call-site object is` user` , so` this.name` is` "Ada"` . Assigned to` fn` and called bare, there is no call-site object, so` this` is the global object in sloppy mode (making` this.name`` undefined` ) or` undefined` in strict mode (making` this.name` throw). The four binding rules are summarized in[MDN’s this reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) .


**What they’re really testing:** whether you know` this` is dynamic and call-site-driven, not lexically fixed.


**Likely follow-up:** “How do you pin` this` to` user` ?” —` fn.call(user)` ,` fn.apply(user)` , or` user.greet.bind(user)` .


Call form What` this` binds to


` fn()` (bare)` undefined` (strict) / global object (sloppy)


` obj.fn()` (method)` obj` (the object left of the dot)


` fn.call(o)` /` fn.apply(o)` /` fn.bind(o)`` o` (explicit)


` new Fn()` the freshly created instance


arrow function inherited from the enclosing lexical scope


### 8.` this` inside a callback


```text
const   timer   =   {
seconds  :   0  ,
startBroken  ()   {
setInterval  (  function   ()   {   this  .  seconds  ++  ;   },   1000  );   // this is wrong
},
startFixed  ()   {
setInterval  (()   =>   {   this  .  seconds  ++  ;   },   1000  );   // this is timer
},
};
```


In` startBroken` , the plain` function` passed to` setInterval` is called by the timer mechanism with no call-site object, so` this` is not` timer` —` this.seconds++` mutates the wrong object. In` startFixed` , the arrow function has no` this` of its own and inherits it from` startFixed` ’s scope, where` this` is` timer` . This is the textbook reason arrow functions exist for callbacks. See[MDN on arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) .


**What they’re really testing:** whether you understand lexical` this` and why arrow functions solved the old` var self = this` workaround. The lost-` this` handler bug surfaces constantly in production class components and event listeners.


**Likely follow-up:** “Why can’t you use an arrow function as a constructor or an object method that needs its own` this` ?” — because it has no` this` binding to assign, so` new` throws and a method-level arrow captures the *outer*` this` instead of the object.


## JavaScript interview questions on the runtime model


### 9. Predict the console order (event loop)


```text
console  .  log  (  "  1  "  );
setTimeout  (()   =>   console  .  log  (  "  2  "  ),   0  );
Promise  .  resolve  ().  then  (()   =>   console  .  log  (  "  3  "  ));
console  .  log  (  "  4  "  );
// Output: 1 4 3 2
```


Promise callbacks (microtasks) always run before` setTimeout` callbacks (macrotasks), even with a` 0` ms delay. The execution trace:


1. ` console.log("1")` runs synchronously →` 1` .
2. ` setTimeout` schedules its callback on the **macrotask** queue.
3. ` Promise.resolve().then(...)` schedules its callback on the **microtask** queue.
4. ` console.log("4")` runs synchronously →` 4` .
5. The call stack is now empty. The engine drains the **entire microtask queue** before any macrotask →` 3` .
6. Only then does it pick up the next macrotask →` 2` .


MDN’s[microtask guide](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide) describes this ordering.


**What they’re really testing:** whether you understand the event loop has two priority tiers, not one queue. The two-tier event loop is the model behind every “my state updated a tick late” bug.


**Likely follow-up:** “Where does` async/await` fit?” — the code after an` await` runs as a microtask, so it queues at the same priority as a` .then()` callback.


### 10.` ==` vs` ===` and coercion


```text
0   ==   "  0  "  ;          // true
0   ===   "  0  "  ;         // false
null   ==   undefined  ;   // true
NaN   ===   NaN  ;       // false
[]   ==   !  [];         // true
```


` ==` performs type coercion before comparing while` ===` does not, which is why` 0 == "0"` is` true` but` 0 === "0"` is` false` . The counterintuitive ones:` null == undefined` is` true` by a special rule (and they equal nothing else under` ==` );` NaN` is never equal to anything, including itself; and` \[\] == !\[\]` is` true` because` !\[\]` is` false` , which coerces to` 0` , and` \[\]` coerces to` ""` then` 0` . The full algorithm lives in[MDN’s equality comparisons guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness) .


**What they’re really testing:** whether you can predict coercion — not whether you can recite “always use` ===` .” The interviewer wants the mechanism, then the rule of thumb.


**Likely follow-up:** “What about assigning to an undeclared variable?” — whether` value = 42` (no` var` /` let` /` const` ) throws or silently creates a global depends on mode:[strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) and ES modules throw a` ReferenceError` ; sloppy-mode scripts create an implicit global. Same snippet, two correct answers — flagging that distinction is itself a senior signal.


## What separates a hire from a pass


The through-line across all ten questions: interviewers score the explanation, not just the output. Predicting` 3 3 3` or` 1 4 3 2` proves you’ve seen the puzzle; naming the binding model, the scope chain, the call-site rule, and the two-tier event loop proves you understand the language well enough to debug it under pressure. Drill these ten until you can say the *why* in two sentences without notes — and since none of these behaviors are version-dependent, you can verify every snippet yourself in any current engine and trust the result.


## FAQs


What is the difference between the Temporal Dead Zone and a variable simply being undefined?


A variable in the Temporal Dead Zone has been hoisted but not yet initialized, so reading it throws a ReferenceError, whereas an undefined var has been hoisted and initialized to the value undefined, so reading it returns undefined. Only let and const create a TDZ; it runs from scope entry until the declaration line executes. The distinction is binding creation versus binding initialization.


Why do arrow functions break when used as object methods or constructors?


Arrow functions have no this binding of their own, so they cannot serve roles that require one. Used as an object method, an arrow captures this from the enclosing lexical scope rather than the object, so this.property does not point at the object. Used with new, the engine throws a TypeError because there is no this binding to assign the new instance to. Use regular functions for both cases.


Does the setTimeout-in-a-loop bug behave the same in every JavaScript engine?


Yes. The var version logs the final loop value in every current engine because var creates one function-scoped binding shared by all callbacks, and the let version logs per-iteration values everywhere because the specification defines a fresh binding per iteration for let in a for loop. This behavior is spec-defined in ECMA-262, not engine-specific, so Node.js, V8, SpiderMonkey, and JavaScriptCore all produce identical output.


Does async and await change the event loop priority compared to Promise.then?


No. Code after an await runs as a microtask, at the exact same priority as a Promise.then callback, so it executes before any setTimeout macrotask. An await effectively pauses the function and schedules the continuation on the microtask queue when the awaited value settles. This means an awaited continuation and a then callback queued at the same point resolve in source order, and both run before any timer callback set to zero milliseconds.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
