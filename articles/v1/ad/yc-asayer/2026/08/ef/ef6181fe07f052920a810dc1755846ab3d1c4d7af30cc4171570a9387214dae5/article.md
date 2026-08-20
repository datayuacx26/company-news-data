---
schema_version: "1.0.0"
document_id: "ef6181fe07f052920a810dc1755846ab3d1c4d7af30cc4171570a9387214dae5"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/3-javascript-gotchas/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T07:50:16.961097+00:00"
fetched_at: "2026-08-05T07:50:18.082714+00:00"
content_hash: "sha256:958ed412e66e139fe6d1ae9bd7f596954b438a47a8b4f83a1baef025251bd4e1"
---

# 3 JavaScript Gotchas Explained

` 0.1 + 0.2` does not equal` 0.3` ,` NaN` is not equal to itself, and` await` inside a loop can make a fast page slow — three JavaScript behaviors that look like bugs but are actually the language doing exactly what its specification requires. This article explains the mechanism behind each one, not just the weird console output, and gives the correct fix for each. Every one of them ships as a real user-facing symptom: a total that’s off by a penny, a validation that lets bad input through, a screen that loads sluggishly for no obvious reason. Understanding the *why* is what lets you avoid all three in production and answer them crisply in an interview.


## Key Takeaways


- ` 0.1 + 0.2` returns` 0.30000000000000004` because IEEE 754 doubles store numbers in binary, and neither` 0.1` nor` 0.2` has an exact finite binary representation, so each is rounded before they are added.
- For money, calculate in integer cents rather than floating-point dollars; for general float comparisons, test` Math.abs(a - b) < tolerance` with a tolerance sized to your numbers’ magnitude, not a blanket` Number.EPSILON` .
- ` NaN === NaN` is` false` because the IEEE 754 spec defines NaN as unequal to every value including itself — use` Number.isNaN()` , which does no coercion, instead of the global` isNaN()` .
- ` await` inside a` for` loop pauses the entire loop on every iteration; start the promises first and` await Promise.all(...)` to run independent requests concurrently.


## Gotcha #1: Why` 0.1 + 0.2` isn’t` 0.3` (floating-point math)


` 0.1 + 0.2` returns` 0.30000000000000004` because JavaScript numbers are[IEEE 754 double-precision floats](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Numbers_and_strings) stored in base-2, and neither` 0.1` nor` 0.2` has an exact finite binary representation. Each literal is rounded to the nearest representable 64-bit value the moment you write it, and the sum of those two already-rounded values rounds to a number just above` 0.3` .


```text
0.1   +   0.2  ;               // 0.30000000000000004
0.1   +   0.2   ===   0.3  ;       // false
(  0.1  ).  toPrecision  (  20  );   // "0.10000000000000000555"
```


That last line is the tell: the value stored for` 0.1` was never exactly` 0.1` . This is not a JavaScript quirk — it is a property of double-precision floating point shared by Python, Java, C, and any language that uses the same format. The number` 0.1` in binary is a repeating fraction, the same way` 1/3` is` 0.333…` in decimal, so it has to be truncated.


The fix depends on what you’re computing:


```text
// Money: work in integer cents, format only for display
const   total   =   1010   +   2030  ;     // 3040 cents
(  total   /   100  ).  toFixed  (  2  );      // "30.40"


// General comparison: tolerance sized to your magnitude
Math  .  abs  ((  0.1   +   0.2  )   -   0.3  )   <   Number  .EPSILON;   // true
```


For currency, store and calculate in integer cents and never in floating-point dollars, then divide and[toFixed(2)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) at the display edge. For approximate equality, compare against a tolerance.[Number.EPSILON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON) works only for numbers around the magnitude of 1 — MDN explicitly warns it is not a safe blanket threshold, so scale the tolerance to the size of the values you’re comparing. If you’re summing an array,[Math.sumPrecise()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sumPrecise) became[Baseline newly available in April 2026](https://web.dev/blog/baseline-digest-apr-2026?hl=en) , so it may not run on older devices, and note that it still cannot avoid the` 0.1 + 0.2` precision problem for individual literals — it only prevents error from accumulating across a long summation.


A penny-off total reproduces only with the *exact* sequence of values that were summed, which is why a session replay that reconstructs the real input order is more useful here than a screenshot of the wrong number.


## Gotcha #2:` NaN` is the only value not equal to itself


` NaN === NaN` is` false` because the IEEE 754 standard defines NaN (“Not-a-Number”) as unequal to every value including itself, and JavaScript follows that rule literally. This makes NaN the only value in the language that is not equal to itself — a fact you can even use as a NaN detector.


```text
NaN   ===   NaN  ;     // false
NaN   ==   NaN  ;      // false
typeof   NaN  ;      // "number"
```


` typeof NaN` is` 'number'` : NaN is a numeric value that represents an undefined or unrepresentable numeric result, not a separate type. The real trap is the global[isNaN()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN) , which coerces its argument to a number *before* testing, so values that aren’t NaN at all report as though they were checked as numbers:


```text
isNaN  (  ''  );     // false  — '' coerces to 0
isNaN  ([]);     // false  — [] coerces to 0
isNaN  (  '  45  '  );   // false  — '45' coerces to 45
isNaN  ({});     // true   — {} coerces to NaN
```


The fix is[Number.isNaN()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN) , added in ES2015, which does no coercion and returns` true` only for the actual` NaN` value:


Input` isNaN()` (coerces)` Number.isNaN()` (no coercion)


` NaN`` true`` true`


` 'NaN'`` true`` false`


` ''`` false`` false`


` \[\]`` false`` false`


` '45'`` false`` false`


` undefined`` true`` false`


Reach for` Number.isNaN()` to test “is this specifically the NaN value,” and[Number.isFinite()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isFinite) when you want “is this a real, finite number” — it rejects` NaN` ,` Infinity` , and non-numbers without coercion. A related myth worth retiring:` parseInt("032")` returns` 32` , not` 26` . Auto-detection of leading-zero strings as octal was[removed in ECMAScript 5](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) , so the widely-copied` 26` claim is a pre-2011 relic — still pass a radix, but for the right reason.


When a validation misfires because` isNaN('')` coerced an empty string to` 0` , the offending value often looks “empty” in a bug report; replaying the actual keystrokes and field state shows exactly what slipped through.


## Gotcha #3:` await` in a loop serializes requests and slows your UI


` await` inside a` for` loop pauses the entire loop on every iteration, so requests that have no dependency on each other run strictly one-after-another instead of in parallel. Each iteration waits for its promise to settle before the next request even starts, turning N independent round-trips into N sequential ones.


```text
// Serial: each await blocks the next iteration
async   function   getUsers  (  ids  ) {
const   users   =   [];
for (  const   id   of   ids  ) {
users  .  push  (  await   fetchUser  (  id  ));   // waits ~1.5s, every time
}
return   users  ;
}
```


The fix is to start all the promises first and then await them together with[Promise.all()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) , which fires the requests concurrently and resolves once they’ve all completed:


```text
async   function   getUsers  (  ids  ) {
return   Promise  .  all  (  ids  .  map  (  fetchUser  ));
}
```


With a simulated fixed 1.5-second latency per request (illustrating serialization, not real network timing), the difference scales with the batch size:


Approach 3 requests 10 requests


` await` in a loop (serial) ~4.5s ~15s


` Promise.all` (concurrent) ~1.5s ~1.5s


One caveat:` Promise.all` rejects as soon as any single promise rejects, discarding the rest. When one failure shouldn’t abort the batch, use[Promise.allSettled()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled) instead, which waits for every promise and reports each as` fulfilled` or` rejected` .` Promise.allSettled` shipped in ES2020 and has been baseline across browsers since early 2023, so it needs no polyfill in modern environments. Serialize deliberately only when each request truly depends on the previous one’s result.


A screen that’s “just slow” for some users is hard to catch in code review; a session replay surfaces requests firing one-after-another instead of together, which points straight at an awaited loop.


## Where to go from here


These three behaviors are specification-correct, which is exactly why they survive code review and reach users: binary floats round because IEEE 754 says so, NaN rejects itself because the standard defines it that way, and` await` serializes because that is what pausing execution means. The defenses are small and mechanical — integer cents and magnitude-scaled tolerances for floats,` Number.isNaN()` and` Number.isFinite()` for numeric checks,` Promise.all` or` Promise.allSettled` for independent async work. Audit your own currency math, validation guards, and data-fetching loops against these three patterns, and the class of “impossible” bug they cause stops shipping.


## FAQs


What is the difference between the global isNaN() and Number.isNaN()?


The global isNaN() coerces its argument to a number before testing, so isNaN('') and isNaN(\[\]) both return false because '' and \[\] coerce to 0, while isNaN('NaN') returns true because 'NaN' coerces to the NaN value. Number.isNaN(), added in ES2015, does no coercion and returns true only for the actual NaN value. Use Number.isNaN() when you specifically need to detect NaN.


Does the floating-point problem with 0.1 + 0.2 only happen in JavaScript?


No. It occurs in any language that uses IEEE 754 double-precision floats, including Python, Java, C, C++, and Ruby. Neither 0.1 nor 0.2 has an exact finite binary representation, so each is rounded when stored, and the sum rounds to 0.30000000000000004. It is a property of the binary floating-point format itself, not a JavaScript bug, so the same integer-cents and tolerance-based fixes apply across languages.


When should I use Promise.allSettled instead of Promise.all?


Use Promise.allSettled when one failure should not abort the whole batch. Promise.all rejects as soon as any single promise rejects and discards the results of the others, so it suits all-or-nothing operations. Promise.allSettled waits for every promise regardless of outcome and returns an array reporting each as fulfilled or rejected, which is what you want when fetching multiple independent resources and partial success is acceptable. It shipped in ES2020 and needs no polyfill in modern environments.


Is it ever correct to use await inside a loop?


Yes, when each iteration genuinely depends on the previous one's result, such as paginating through an API where the next request needs a cursor returned by the last response, or rate-limiting deliberately to avoid overwhelming a server. In those cases serial execution is the intended behavior. The gotcha only applies to independent requests that have no dependency on each other, where awaiting in a loop needlessly serializes work that Promise.all could run concurrently.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
