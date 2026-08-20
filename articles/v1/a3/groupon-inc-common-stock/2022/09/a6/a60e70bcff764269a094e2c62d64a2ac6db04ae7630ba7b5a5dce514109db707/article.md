---
schema_version: "1.0.0"
document_id: "a60e70bcff764269a094e2c62d64a2ac6db04ae7630ba7b5a5dce514109db707"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/exporting-node-modules-in-2022-e8fd97f0f5a9"
published_at: "2022-09-01T20:01:47+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:84cc44cffa45a9f785105a004dc6f6b0038f6beafebd03d378bf5545f3b9ef95"
---

# Exporting Node Modules in 2022

# Exporting Node Modules in 2022


[David Bushong](https://medium.com/@dbushong?source=post_page---byline--e8fd97f0f5a9---------------------------------------)


6 min read


·


Sep 1, 2022


--


Groupon maintains literally hundreds of NPM modules, both open source and internal. Many of these are consumed by our custom NodeJS-based middleware web layer we call “The Interaction Tier” (itself a topic for another post someday).


As folks write new modules, a common question is “what’s the best way to export things from our published modules to maximize compatibility?” — and that is the topic of this post. First, some background and history:


## Flavors of exported modules


### CommonJS in Node


In the beginning, there was[CommonJS](https://nodejs.org/dist/latest-v16.x/docs/api/modules.html) :


Here are 3 sample files with exports:


```text
// export1.js - exporting individual properties w/ CommonJS   'use strict';  function   foo() { }  exports.foo  =   foo;  exports.bar  =   42;  // export2.js - exporting a single object w/ CommonJS   'use strict';  function   baz() { }   const   garply  =   88;  module.exports  =   { baz };  // dynamically (conditionally!) exported!   if   ( Math.random() > 0.5  ) module.exports.garply  =   garply;  // export3.js - exporting a bare function w/ CommonJS   'use strict';  function   quux() { }  module.exports  =   quux;  // sometimes there are extra properties added to the bare function   quux.yadda  =   42;
```


Those CommonJS exports can be imported either into other CommonJS files or into ES Module (more on that below) files:


```text
// import.js - importing CommonJS modules into a CJS file   'use strict';  const   { foo, bar }  =   require('./export1');  const   { baz, garply }  =   require('./export2');  if   ( Math.random > 0.9  ) {     // can also dynamically decide when to import      const   quux  =   require('./export3');     // can poke into properties tacked onto functions      const   { yadda }  =   require('./export3');  }  // import.mjs - importing CommonJS modules into an ESM file   // node is willing to turn exported objects into named exports   import   { foo, bar }  from   './export1.js';  import   { baz, garply }  from   './export2.js';  import   quux  from   './export3.js';  // cannot access added property "yadda" directly; hence:   const   { yadda } = quux;
```


You can read more elsewhere, but the key features are:


1. The module files are synchronously executed, and at the end of their sync execution, anything present in` module.exports` is available to files that` require()` this module. This means exports can be dynamically constructed.
2. You may either add properties to the existing` exports` object, which starts the same as` module.exports` , or reassign all of` module.exports` to a single thing (which may happen to be an object)
3. The reason` const { a, b } = require(...` works is because it is a convention to export an object - but you can export anything you want (bare function, array, number, etc)


### Native ES Modules in Node


This is a[large topic](https://nodejs.org/dist/latest-v16.x/docs/api/esm.html) , but here are the highlights:


First, three example ES Modules-exporting files:


```text
// export1.mjs - named exports w/ ESM   export    function   foo() { }  export    const   bar  =   42;  // export2.mjs - more named exports w/ ESM   export    function   baz() { }  // cannot dynamically choose to not export things   // i.e. no if (...) export possible   export    const   garply  =   88;  // export3.mjs - default export w/ ESM   export    default    function   quux() { }  // can export something *other* than default also   export    const   yadda  =   99;
```


ES Module exports can be imported easily in other ES Module files, and can be imported asynchronously (only!) in CommonJS files:


```text
// import.mjs - importing ESM w/ ESM   import   { foo, bar }  from   './export1.mjs';  import   { baz, garply }  from   './export2.mjs';  // can import default export and others   import   quux, { yadda }  from   './export3.mjs';  // import.js - importing ESM w/ CommonJS   'use strict';  // you cannot directly require() ESM files,   // you must use import() which is an async operator   async    function   someFn() {     const   { foo, bar }  =    await    import  ('./export1.mjs');     const   { baz, garply }  =    await    import  ('./export2.mjs');     // the default export has property "default"      const   { default: quux, yadda }  =    await    import  ('./export3.mjs');  }
```


### BabelScript / Webpack / TypeScript


When early proposals of ECMAScript Modules were announced, a number of bundlers (and TypeScript) ran with it, and supported various syntaxes which were similar to, but not entire semantically compatible with, ES Modules as ultimately supported natively in NodeJS 14.


Most projects generally compile these files to CommonJS, so we should understand what they actually compile to.` export1.mjs` and` export2.mjs` would generally compile to their equivalents from the CommonJS section above.` export3.mjs` would compile to something like:


```text
// dist/export3.js   'use strict';  function   quux() { }  exports. default    =   quux;  exports.yadda  =   99;
```


This is importantly different from the original CommonJS` export3.js` , because there is no bare function exported, but rather an object with property` default` . To maintain backward compatibility when required, TypeScript offers a non-standard syntax:


```text
// export3.ts   function   quux() { }  export    =   quux;  // non-standard syntax   // cannot export anything else, like yadda, though you can:   quux.yadda = 99;
```


This compiles to CommonJS exactly like the original` export3.js` , but makes the use of this file from other TypeScript files crummier in some cases:


```text
// import.ts   // more non-standard syntax   import   quux  =   require('./export3');
```


## Solutions


So what’s the Right Way to support all of the different use cases seamlessly in our published NPM packages?


We don’t know.


Yet.


Currently, here are some tips:


### Don’t use default or bare exports in new code


If you’re building something new, don’t export bare functions/classes (either using` export default` or with` export =` ) publically. The former makes for an ugly CommonJS experience (` const { default: coolFn } = require('./cool-fn');` ), and the latter makes for an ugly TS experience (` import coolFn = require('./cool-fn');` ). Using named exports gives every environment as similar an experience as possible.


(Note: this tip only applies to your NPM package’s public interface — things you’re exporting between internal source files don’t apply)


```text
// bad1.ts   // don't do this   export    default   someFn() { }  // bad2.ts   // don't do this in NEW code   function   someFn() { }  export    =   someFn;  // good.ts   export    function   someFn() { }  // in CommonJS this is: const { someFn } = require('./good');   // in TS/ESM this is: import { someFn } from './good';
```


### Write new CommonJS like ES Modules


If you have to write new code as CommonJS, write it in a style that is as compatible with ES Modules as possible. That means no default exports, as above, but also it’s best to use a style that “looks like” named exports, to make future porting clear and unambiguous:


```text
// good.js   'use strict';  function   someFn() { }  exports.someFn = someFn;  function   anotherFn() { }  exports.anotherFn = anotherFn;
```


(another option is to directly use` exports.someFn = () => { }` , which has its own pros and cons outside the scope of this post)


### Publish as CommonJS


Don’t publish Native ES Modules (via compile targets or hand-written) in NPM packages yet. CommonJS is much easier to use from ESModules than the reverse, and currently ~90% of our backend code at Groupon is CommonJS on NodeJS. As our teams move to writing new app code using Native ES Modules (or TypeScript), this guidance will gradually change.


Even if you write the code as TypeScript (recommended!) or as NodeJS-compatible ESM exports directly, make sure to compile it (using TypeScript or Babel) into CommonJS` *.js` files. Also, ensure that you do not have` type` in your` package.json` set to` module` ; that’s not what you’re *publishing* .


If you’re writing your code as ES Modules and using babel to compile, then in order to make lint work correctly, you may need to name your source files` .mjs` or include some sort of` overrides` section in your` .eslintrc` to make it properly recognize` .js` files as module type without setting it in the` package.json`


### Include type declarations


Even if you’re writing JavaScript, please try to include basic` .d.ts` files in your repository - developers are increasingly writing TypeScript (or even using VSCode which provides type hints while writing JavaScript) and it’s a huge aid to writing robust code and faster refactors.


### Consider when to make a BREAKING CHANGE


If you are porting a library from JavaScript to TypeScript, e.g., and it has a bare function export, e.g.:


```text
const   myFn  =   require('@grpn/my-fn');
```


…there’s really no good backward-compatible way to offer this in TypeScript — you have to use the ugly TS backward-compatibility hack:


```text
function   myFn() { }  export    =   myFn;
```


…which keeps your CommonJS` require()` statements unchanged… but makes your type declarations and imports ugly:


```text
import   myFn  =   require('@grpn/my-fn');
```


Therefore it’s worth considering whether this is a good occasion for a[BREAKING CHANGE](https://github.com/groupon/nlm/blob/main/CONTRIBUTING.md#examples) commit to make it a named export:


```text
export    function   myFn() { }
```


which is breaking because now the CommonJS usage looks like:


```text
const   { myFn }  =   require('@grpn/my-fn');
```


## Final Thoughts


The particulars of what your project should export will vary depending on:


- TypeScript adoption among your ecosystem’s developers
- ES Module adoption among users of your package
- The JavaScript runtime you’re using (NodeJS, deno, browser…) — this post was written for NodeJS users primarily.


Hopefully the steps above are a good starting point to minimize developer confusion and churn while adapting to new technologies.
