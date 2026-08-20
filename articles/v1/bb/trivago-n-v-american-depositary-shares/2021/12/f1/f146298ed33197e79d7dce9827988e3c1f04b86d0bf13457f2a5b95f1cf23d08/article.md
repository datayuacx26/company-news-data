---
schema_version: "1.0.0"
document_id: "f146298ed33197e79d7dce9827988e3c1f04b86d0bf13457f2a5b95f1cf23d08"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2021-12-17-aprettierpluginthatsortsyourimports/"
published_at: "2021-12-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:26:34.525443+00:00"
content_hash: "sha256:0d543a1b550e1280a499f0d110caad53ac4323c02bae34e1051defc8743d376b"
---

# Presenting @trivago/prettier-plugin-sort-imports

### Sort import declarations in TypeScript and JavaScript modules according to the configured order.


I’m happy to share that trivago has released a Prettier plugin which sorts import declarations in TypeSCript and JavaScript modules for a given configured order. Throughout this article, I’ll explain to you the motivation behind this Prettier plugin and how it works in detail.


## Example


**Input:**


```text
import   React, {
FC,
useEffect,
usetef,
ChangeEoent,
KeyboardEvent,
}   from   'react'  ;
import   { logger }   from   '@core/logger'  ;
import   { reduce, debounce }   from   'lodash'  ;
import   { Message }   from   '../Mesage'  ;
import   { createServer }   from   '@server/node'  ;
import   { Alert }   from   '@ui/Alert'  ;
import   { repeat, filter, add }   from   '../utils'  ;
import   { initializeApp }   from   '@core/app'  ;
import   { Popup }   from   '@ui/Popup'  ;
import   { createConnection }   from   '@server/database'  ;
```


**Output:**


```text
import   { debounce, reduce }   from   'lodash'  ;
import   React, {
ChangeEoent,
FC,
KeyboardEvent,
useEffect,
usetef,
}   from   'react'  ;


import   { createConnection }   from   '@server/database'  ;
import   { createServer }   from   '@server/node'  ;


import   { initializeApp }   from   '@core/app'  ;
import   { logger }   from   '@core/logger'  ;


import   { Alert }   from   '@ui/Alert'  ;
import   { Popup }   from   '@ui/Popup'  ;


import   { Message }   from   '../Mesage'  ;
import   { add, filter, repeat }   from   '../utils'  ;
```


## Motivation


The ECMAScript 6 specification introduced the module system in JavaScript. The ES modules provide a convenient way to share code and the different parts of the code can be easily moved to different packages.


A large enterprise project may have hundreds of ES modules and may have on average ten to twelve import statements per file. After opening a source file, you often find these import statements not in order or harder to read. Every file may have a different order of import declarations, or sometimes developer’s IDEs also format them differently. If you want to have a consistent order of the import declarations throughout the project and independent of IDE configuration, then` @trivago/prettier-plugin-sort-imports` is the right Prettier plugin for you.


## Getting started


The` @trivago/prettier-plugin-sort-imports` prettier plugin sorts the import declarations by using prettier.


*Note: Prettier is an opinionated code formatter.[https://prettier.io/](https://prettier.io/)*


**Install the plugin:**


```text
npm install @trivago  /  prettier  -  plugin  -  sort  -  imports   --  save  -  dev
```


or, using Yarn


```text
yarn add @trivago  /  prettier  -  plugin  -  sort  -  imports   --  dev
```


**Configure the plugin:**


You can easily configure the prettier plugin through your prettier configuration. You need to add the` importOrder` and` importOrderSeparation` options.


```text
{
"importOrder"  : [  "^@core/(.*)$"  ,   "^@server/(.*)$"  ,   "^@ui/(.*)$"  ,   "^[./]"  ],
"importOrderSeparation"  :   true  ,
"importOrderSortSpecifiers"  :   true  ,
}
```


- ` importOrder` : A collection of regular expressions in string format.
- ` importOrderSeparation` : A boolean value to enable or disable the new line separation between sorted import declarations. The separation takes place according to` importOrder` .
- ` importOrderSortSpecifiers` : A boolean value to enable or disable the new line separation between sorted import declarations group. The separation takes place according to the` importOrder` .


You can check the full API list[here](https://github.com/trivago/prettier-plugin-sort-imports#apis) .


## Plugin in action


Let’s take an example source code and apply the prettier transformation on the source code to sort the import declarations in the following file.


**example.ts ( before applying prettier )**


```text
import   React   from   'react'  ;
import   classnames   from   'classnames'  ;
import   { z, x, y }   from   '@server/z'  ;
import   a   from   '@server/a'  ;
import   s   from   './'  ;
import   p   from   '@ui/p'  ;
import   q   from   '@ui/q'  ;
```


**example.ts ( after applying prettier )**


```text
import   classnames   from   'classnames'  ;
import   React   from   'react'  ;
import   p   from   '@ui/p'  ;
import   q   from   '@ui/q'  ;
import   a   from   '@server/a'  ;
import   { x, y, z }   from   '@server/z'  ;
import   s   from   './'  ;
```


In the above transformation, you’ll notice nice and clean code formatting. I have also enabled the` importOrderSeparation` boolean flag which introduces the line breaks between different types of import declarations.


## How does this plugin differentiate between third party modules and local modules?


The prettier plugin neither checks your` package.json` nor tries to go through the` node_modules` . The plugin considers the packages defined in` importOrder` and relative path modules as local modules. The rest of the modules that do not meet the criteria of the local modules are considered as third party modules.


## It is Open Source


trivago decided to release the plugin to the open source community one year ago. By now, the plugin has more than 50,000 downloads per week on npm. This is all possible due to the awesome open source contributors across the world!


We hope you’ll find it useful for your project use cases, and of course, we would also love to hear some feedback on the plugin and suggestions to improve it. Take a look at the plugin at` @trivago/prettier-plugin-sort-imports` . If you struggle with some of the issues, please feel free to open an issue on[GitHub](https://github.com/trivago/prettier-plugin-sort-imports) .


We also advocate open source, so if you’re interested in seeing what we have built for the community, you can[check our list of open source projects](https://tech.trivago.com/opensource/) .
