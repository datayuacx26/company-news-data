---
schema_version: "1.0.0"
document_id: "94d85aa4ca478d4c117342a1589f1693a318bed87818a1d48ec7e3adb0610b6e"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/javascript-code-quality-eslint-plugin-unicorn/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:804adf324538a328e0e98f355bbd26df8027f6e794b7a1e6560860c54e8a8319"
---

# Improving JavaScript Code Quality with eslint-plugin-unicorn

[eslint-plugin-unicorn](https://www.npmjs.com/package/eslint-plugin-unicorn) is a large, opinionated ESLint plugin — 300+ rules — that pushes modern JavaScript and TypeScript idioms and catches subtle quality bugs, and most of its rules are autofixable. It requires ESLint >=10.4, flat config, and ESM, which means the old` .eslintrc` plus` extends: 'plugin:unicorn/recommended'` setup no longer applies. If you already run ESLint with a basic config and want to raise code quality without authoring custom rules, this plugin is the fastest way to do it.


The catch is that “opinionated” cuts both ways. Some rules prevent accidentally-quadratic loops and real correctness bugs; others enforce stylistic preferences that will fight an established codebase. This guide separates the two: what the plugin enforces, which rules carry their weight, which to downgrade or disable, and a copy-pasteable` eslint.config.mjs` that works on current tooling.


## Key Takeaways


- eslint-plugin-unicorn ships 300+ mostly autofixable rules and, as of v69 (June 2026), requires ESLint ≥10.4, flat config, and ESM —` .eslintrc` no longer works because ESLint 10 removed it.
- The one-line adoption path: extend` unicorn/recommended` , run` eslint --fix` , then turn off the handful of rules that fight your codebase — or start from the` unopinionated` preset, which pre-disables the opinionated rules for you.
- The plugin exports three presets:` recommended` ,` unopinionated` (recommended minus the opinionated rules), and` all` .
- ` prefer-set-has` turns accidentally-quadratic` .includes()` -in-a-loop lookups into O(n)` Set.has()` checks, and` no-array-for-each` rewrites` .forEach()` to` for…of` for better TypeScript type narrowing.
- Pair it with` @typescript-eslint/recommended-type-checked` to combine idiom enforcement with type-aware linting.


## What eslint-plugin-unicorn is and how it thinks


eslint-plugin-unicorn is a collection of ESLint rules that nudge code toward modern, less error-prone JavaScript and TypeScript patterns — preferring` Set` membership over array scans,` for…of` over` .forEach()` ,` node:` import specifiers over bare module names, and so on.[It requires ESLint >=10.4, flat config, and ESM](https://github.com/sindresorhus/eslint-plugin-unicorn) . The repository describes itself as shipping more than 300 rules, and[the latest version is 69.0.0](https://www.npmjs.com/package/eslint-plugin-unicorn) , with releases roughly monthly.


Two design facts shape how you adopt it. First, most rules are autofixable, so a large share of violations resolve with` eslint --fix` rather than manual editing. Second, the plugin ships preset configs so you don’t enable rules one by one. The plugin exports a recommended config that enforces good practices, and separately an all config that enables every rule, except deprecated ones. The rules table uses emoji to mark status: ✅ for rules set in the recommended configuration, ☑️ for the unopinionated configuration, and 🔧 for rules automatically fixable by the` --fix` CLI option.


Scope is mostly JavaScript and TypeScript, with a useful aside: while most rules target JavaScript and TypeScript, some also lint other file types when used with the corresponding ESLint language plugin such as @eslint/css, @eslint/json, @eslint/markdown, or @html-eslint/eslint-plugin. For most teams, the JS/TS rules are the reason to install it.


## Installing eslint-plugin-unicorn with flat config and ESM


Install the plugin and a compatible ESLint, then extend the recommended preset inside a flat` eslint.config.mjs` . The current requirement is ESLint 10.x —[ESLint 10](https://eslint.org/blog/2026/02/eslint-v10.0.0-released/) removed the legacy eslintrc system entirely, leaving flat config as the only supported format, so any tutorial showing` .eslintrc.json` with` extends: 'plugin:unicorn/recommended'` is out of date. ESLint requires a current Node.js runtime (Node 20.19+, 22.13+, or 24+ per the[ESLint npm page](https://www.npmjs.com/package/eslint) ).


```text
npm   install   --save-dev   eslint   eslint-plugin-unicorn
```


A minimal working config scoped to JavaScript and TypeScript files:


```text
// eslint.config.mjs
import   unicorn   from   '  eslint-plugin-unicorn  '  ;
import   {   defineConfig   }   from   '  eslint/config  '  ;


export default   defineConfig  ([
{
files  :   [  '  **/*.{js,mjs,cjs,ts,tsx}  '  ],
extends  :   [  unicorn  .  configs  .  recommended  ],
rules  :   {
// overrides go here
},
},
]);
```


This mirrors the official usage: import` unicorn` from` 'eslint-plugin-unicorn'` , import` { defineConfig }` from` 'eslint/config'` , then within` defineConfig` set` files` and` extends: \[unicorn.configs.recommended\]` . Scoping with` files` matters once you lint non-JavaScript languages in the same config—keep Unicorn’s JS rules in their own config object.


There are three presets to choose your starting point from:


- ` recommended` — the curated default.
- ` unopinionated` — recommended minus the opinionated rules.
- ` all` — every non-deprecated rule.


If your team finds the opinionated rules too noisy, swap` unicorn.configs.recommended` for` unicorn.configs.unopinionated` and skip much of the manual tuning below.


A migration note: if you still depend on a plugin that ships only an eslintrc-style config, wrap it with` FlatCompat` from[@eslint/eslintrc](https://eslint.org/docs/latest/use/configure/migration-guide) . Use the latest` @eslint/eslintrc` on ESLint 10.x.


## The highest-value eslint-plugin-unicorn rules, with before and after


The highest-value Unicorn rules are worth adopting first because each prevents a performance, correctness, or readability problem rather than enforcing taste. Every example below is what` eslint --fix` produces, not hand-written ideal code.


### prefer-set-has — kills accidentally-quadratic lookups


` prefer-set-has` flags an array that is only used for` .includes()` membership checks and steers you to a` Set` .[It prefers Set#has() over Array#includes() when checking for existence or non-existence](https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/prefer-set-has.md) . This matters most inside loops:` Array.prototype.includes` is O(n) per call, so calling it once per iteration over another collection is O(n²), while[Set.prototype.has](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/has) is roughly constant-time.


```text
// ❌ O(n²): includes() rescans the whole array on every iteration
const   allowed   =   [  '  read  '  ,   '  write  '  ,   '  admin  '  ,   /* …hundreds… */  ];
const   granted   =   requested  .  filter  ((  scope  )   =>   allowed  .  includes  (  scope  ));


// ✅ O(n): Set.has() is roughly constant-time per lookup
const   allowed   =   new   Set  ([  '  read  '  ,   '  write  '  ,   '  admin  '  ,   /* …hundreds… */  ]);
const   granted   =   requested  .  filter  ((  scope  )   =>   allowed  .  has  (  scope  ));
```


The failure mode is silent: it works fine on small inputs and degrades sharply as the data grows. These are exactly the slow-interaction regressions you later end up diagnosing in production rather than in review.


### no-array-for-each — better TypeScript narrowing and control flow


` no-array-for-each` rewrites` .forEach()` callbacks to` for…of` loops. It prefers for…of over the forEach method. The win is more than style:` for…of` gives you better TypeScript type narrowing across the loop body, lets you use` await` directly without callback gymnastics, and supports` break` /` continue` — none of which work cleanly inside a` .forEach()` callback.


```text
// ❌
users  .  forEach  ((  user  )   =>   {
sendEmail  (  user  );
});


// ✅ autofixed
for (  const   user   of   users  ) {
sendEmail  (  user  );
}
```


### prefer-node-protocol — unambiguous Node built-ins


` prefer-node-protocol` rewrites bare built-in imports to the explicit` node:` form. It prefers using the node: protocol when importing Node.js builtin modules. The explicit prefix makes the import unambiguously a Node built-in and harder to shadow with a same-named npm package.


```text
// ❌
import   fs   from   '  fs  '  ;
import   {   join   }   from   '  path  '  ;


// ✅ autofixed
import   fs   from   '  node:fs  '  ;
import   {   join   }   from   '  node:path  '  ;
```


### A few more autofixable wins


These each map a legacy pattern to its modern equivalent and fix automatically:


```text
// throw-new-error — Require new when throwing an error
throw   Error  (  '  boom  '  );          // ❌
throw   new   Error  (  '  boom  '  );      // ✅


// prefer-string-replace-all — global replace without a /g regex
str  .  replace  (  /  -  /  g  ,   '   '  );       // ❌
str  .  replaceAll  (  '  -  '  ,   '   '  );     // ✅


// prefer-array-flat-map — collapse map().flat()
rows  .  map  ((  r  )   =>   r  .  cells  ).  flat  ();     // ❌
rows  .  flatMap  ((  r  )   =>   r  .  cells  );        // ✅


// new-for-builtins — require new for Map/Set/Array/Date, etc.
const   cache   =   Map  ();          // ❌
const   cache   =   new   Map  ();      // ✅
```


The` throw-new-error` rule requires` new` when creating an error (covering both` const e = Error()` and` throw Error()` ), and[it is in the recommended config and autofixable](https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/throw-new-error.md) .` new-for-builtins` enforces the use of new for all builtins, except String, Number, Boolean, Symbol and BigInt.[Array.prototype.flatMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) is both clearer and avoids the intermediate array that` .map().flat()` allocates.


Two readability-focused rules round out the high-value set.` better-regex` improves regexes by making them shorter and more consistent (for example,` \[0-9\]` becomes` \\d` ).` prefer-ternary` collapses simple if/else blocks: it prefers ternary expressions over simple if statements that return or assign values. An if-else statement typically produces more lines than a single-line ternary, and it can force you to declare variables with` let` solely to reassign them in the blocks —[leaving them unnecessarily mutable and preventing prefer-const from flagging the variable](https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/prefer-ternary.md) .


```text
// ❌
function   status  (  ok  ) {
if (  ok  ) {
return   '  pass  '  ;
} else {
return   '  fail  '  ;
}
}


// ✅ autofixed
function   status  (  ok  ) {
return   ok   ?   '  pass  '   :   '  fail  '  ;
}
```


## The controversial rules and how to decide


Some recommended rules enforce preference rather than correctness, and they generate the most noise on an existing codebase. The table below is the keep/warn/disable call for the usual suspects, all of which are enabled in` recommended` .


Rule Verdict Reason


` prevent-abbreviations` Warn or scope Renames` e` →` error` ,` req` →` request` ,` props` →` properties` ; fights names your framework and team already use.


` no-null` Disable if you use` null` Unicorn prefers` undefined` , but` null` is legitimate when an ORM or API returns it — there’s no single right answer.


` no-array-reduce` Keep or warn It disallows Array#reduce() and Array#reduceRight(); reasonable for readability, but` reduce` is fine for many teams.


` filename-case` Scope ignores Clashes with framework-mandated filenames; keep it but exclude the exceptions.


The` no-null` case is the clearest: Unicorn prefers` undefined` , yet` null` is what many database drivers and HTTP APIs hand back, so forcing one over the other creates conversion noise with no correctness benefit. Disable it when` null` is part of your data contract.


You do not have to disable these by hand. The` unopinionated` preset leaves the opinionated rules — including` no-null` and` prevent-abbreviations` — off by default, so it’s the built-in middle ground between` recommended` and a heavily customized config.


For the rules you keep but want to tune, scope rather than disable. A common pattern for` filename-case` excludes framework files:


```text
{
files  : [  '  **/*.{js,ts,tsx}  '  ],
extends  : [  unicorn  .  configs  .  recommended  ],
rules  : {
'  unicorn/no-null  '  :   '  off  '  ,
'  unicorn/prevent-abbreviations  '  :   '  warn  '  ,
'  unicorn/filename-case  '  : [
'  error  '  ,
{
cases  :   {   pascalCase  :   true  ,   camelCase  :   true   },
ignore  :   [  '  next-env  \\  .d  \\  .ts  '  ,   '  vite  \\  .config  \\  .ts  '  ],
},
],
},
}
```


## Adopting it incrementally on an existing codebase


Roll out the plugin in stages so the first run doesn’t bury you in errors. The sequence that works:


1. **Start from a preset.** Extend` unicorn/recommended` , or` unicorn/unopinionated` if the opinionated rules are too noisy for your team.
2. **Autofix first.** Run` npx eslint . --fix` . Because most rules are autofixable, a large fraction of violations disappear here, and you review one mechanical diff instead of hundreds of inline errors.
3. **Downgrade the noisy rules to` warn` .** Anything still firing in volume that isn’t a correctness issue —` prevent-abbreviations` is the usual offender — goes to` 'warn'` so it doesn’t block CI while you migrate.
4. **Scope ignores instead of disabling outright.** Use the` ignore` option (as with` filename-case` ) and` files` globs to carve out exceptions rather than killing a useful rule everywhere.
5. **Layer in type-aware linting.** Add[@typescript-eslint/recommended-type-checked](https://typescript-eslint.io/users/configs/) alongside Unicorn. Unicorn enforces idioms; typescript-eslint’s type-checked config catches bugs only the type system can see. The two are complementary, and enabling both is a strong default for a TypeScript project.


Treat your config as a living document. As you upgrade the plugin — releases land roughly monthly — re-check the rules table for new rules and renames, and revisit which` warn` rules are ready to promote to` error` .


The fastest path to better JavaScript with eslint-plugin-unicorn is unglamorous: extend` unicorn/recommended` , run` eslint --fix` , and spend your attention only on the handful of opinionated rules that genuinely conflict with how your team writes code. Start with the preset today, let the autofixer do the bulk of the work, and tune from there — the[official rules table](https://github.com/sindresorhus/eslint-plugin-unicorn) is the reference for every rule’s current status and options.


## FAQs


Does eslint-plugin-unicorn work alongside typescript-eslint, or do they conflict?


They are complementary and designed to run together. Unicorn enforces modern JavaScript and TypeScript idioms, while typescript-eslint adds type-aware rules that catch bugs only the type system can see. Add both as separate config objects in your flat eslint.config.mjs and pair unicorn/recommended with the recommended-type-checked config from typescript-eslint. Enabling both is a strong default for a TypeScript project, with no rule overlap to resolve.


What is the difference between the recommended and unopinionated presets?


The recommended preset is the curated default that enables both correctness rules and opinionated stylistic rules. The unopinionated preset is recommended minus the opinionated rules, so it pre-disables controversial entries like no-null and prevent-abbreviations that fight established codebases. Choose unopinionated when your team finds the stylistic rules too noisy; it is the built-in middle ground between recommended and a heavily customized config. A third preset, all, enables every non-deprecated rule.


Why does my old .eslintrc config with extends plugin:unicorn/recommended stop working?


ESLint 10 removed the legacy eslintrc system entirely, leaving flat config as the only supported format, and eslint-plugin-unicorn requires ESLint 10.4 or higher, flat config, and ESM. The old .eslintrc plus extends 'plugin:unicorn/recommended' string syntax no longer loads. Migrate to an eslint.config.mjs that imports unicorn from 'eslint-plugin-unicorn' and sets extends to unicorn.configs.recommended inside a config object scoped with files.


Will adding eslint-plugin-unicorn break my build with hundreds of errors?


Most violations resolve automatically because the majority of unicorn rules are autofixable. Run eslint with the fix flag first, which clears a large fraction of issues as a single mechanical diff rather than hundreds of inline errors. For rules that still fire in volume but are not correctness issues, such as prevent-abbreviations, set them to warn so they do not block CI during migration. This staged rollout keeps the build green while you adopt the plugin.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
