---
schema_version: "1.0.0"
document_id: "98e368c5139e28246d2d2ea5a273baf76c0399dfd8eebe6b43ca946bc687e9a1"
company_key: "yc-weweb-io"
company: "weweb.io"
source_id: "yc-weweb-io-news-import-be394dfb89cc"
canonical_url: "https://www.weweb.io/blog/npm-supabase-guide-install-ssr-edge-functions"
published_at: null
first_seen_at: "2026-07-22T19:44:15.124283+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:f68518917a98ffdce6de388504ce067b7b42aa3d961a6c07eef681d8e0d51575"
---

# The npm Supabase Guide 2026: Install, SSR, Edge Functions

Building modern applications often means bringing together the best tools for the job. For many developers, that involves the powerful backend features of Supabase and the vast ecosystem of packages available through npm. Understanding how to use **npm Supabase** libraries and dependencies is key to a smooth development workflow, especially when working with serverless environments like Deno and Supabase Edge Functions. If you’re delivering client projects, check out[WeWeb for Agencies](https://www.weweb.io/weweb-for-agencies) to accelerate front‑end delivery.


This guide walks you through everything you need to know, from installing the essential Supabase packages with npm to managing complex dependencies in your serverless functions. We will cover the tools and techniques that make the **npm Supabase** combination so effective for building scalable, production ready apps. For hands-on tutorials, explore the[WeWeb Academy](https://www.weweb.io/academy) .


## Getting Started with Core Supabase Packages


Before diving into advanced configurations, let’s cover the two primary npm packages you’ll encounter when working with Supabase in a JavaScript or TypeScript project.


### Installing the Main Supabase JS Library


The cornerstone of any Supabase project is the official JavaScript client library,` @supabase/supabase-js` . This is an isomorphic library, meaning it’s designed to work seamlessly in both browser and Node.js environments. It’s your main tool for interacting with your Supabase backend, allowing you to query your database, manage user authentication, listen for real time changes, and store files.


Getting started is as simple as running a single command in your project’s terminal:


```text
npm install @supabase/supabase-js


```


This package is incredibly popular and actively maintained, with over 3.3 million weekly downloads on npm, a testament to its reliability and widespread adoption. Once installed, you can import the` createClient` function, initialize it with your project URL and public key, and you’re ready to communicate with your Supabase backend.


### Handling Server Side Rendering


If you’re using a modern framework like Next.js, SvelteKit, or Remix, you’ll be dealing with server side rendering (SSR). In an SSR context, user session management needs a different approach since server environments don’t have access to browser features like` localStorage` .


This is where the` @supabase/ssr` package comes in. It’s a helper library specifically designed to make the main **npm Supabase** client work smoothly in SSR frameworks. It cleverly uses cookies to manage user sessions, ensuring authentication flows work correctly on both the server and the client. You can install it with a familiar command:


```text
npm install @supabase/ssr


```


While the library is technically in beta, meaning its API could see changes, it already boasts over 576,000 weekly downloads. This indicates strong community trust for handling Supabase authentication in server rendered applications.


## Using npm Packages in Deno and Edge Functions


Supabase Edge Functions run on Deno, a modern JavaScript and TypeScript runtime. A game changing feature in Deno is its native support for npm packages, which completely transforms how you can use the **npm Supabase** ecosystem in your serverless logic. On the frontend, you can bind data from[Google Sheets](https://www.weweb.io/integrations/google-sheets) via WeWeb’s native integration.


### The “npm:” Specifier: Unlocking Node Modules in Deno


In Deno, you can import packages directly from the npm registry using the` npm:` import specifier. This simple prefix tells Deno to fetch a module from npm, much like Node.js would. For example, to use the Supabase client in a Deno file, you can write:


```text
import { createClient } from 'npm:@supabase/supabase-js@2';


```


When Deno sees this, it automatically downloads and caches the package. A major benefit here is that you don’t need a` package.json` file or a` node_modules` folder, which keeps your projects lean. This approach is now the recommended way to use npm packages in Deno, offering a seamless bridge to millions of existing libraries.


### How Supabase Edge Functions Import npm Packages


Supabase has integrated this Deno feature directly into its Edge Functions. You can now use your favorite npm packages in your serverless functions without any complex build steps. The Supabase platform’s edge runtime, which is built on Deno, handles everything behind the scenes.


When you deploy a function, Supabase bundles your code and all its` npm:` imports into a single, optimized file. This pre bundling process ensures your functions start quickly without needing to fetch dependencies from the internet at runtime. This streamlined workflow makes it easier than ever to build powerful backend logic with the libraries you already know and love. While you build your backend with the power of **npm Supabase** , you can accelerate your frontend development significantly by using a visual platform like[WeWeb’s no-code web app builder](https://www.weweb.io/no-code-web-app-builder) , which allows teams to build production grade applications without writing code.


## Managing Your Project’s Dependencies


As your project grows, managing dependencies becomes crucial for stability and maintainability. Deno provides modern tools for this, which are fully supported by Supabase Edge Functions.


### ` deno.json` : The Modern Way to Manage Dependencies


The` deno.json` file is Deno’s equivalent of` package.json` . It’s the recommended way to manage dependencies for your Supabase Edge Functions. You can create a` deno.json` file inside each function’s directory to define an` imports` map. This allows you to create aliases for your dependencies.


For example, your` deno.json` might look like this:


```text
{
"imports": {
"supabase": "npm:@supabase/supabase-js@2"
}
}


```


Then, in your function code, you can use a clean, short import:


```text
import { createClient } from "supabase";


```


This practice keeps your code tidy and makes updating versions a breeze, as you only need to change them in one central file. For production, Supabase strongly recommends that each function has its own` deno.json` file to ensure dependencies are properly isolated. If you expose a GraphQL API from Supabase or another service, you can connect it to your UI with WeWeb’s[GraphQL integration](https://www.weweb.io/integrations/graphql) .


### Understanding Legacy Import Maps


Before` deno.json` became the standard, Deno used a separate file, often called` import_map.json` , for the same purpose. This is now considered a legacy approach within the Supabase ecosystem. While Edge Functions still support it for backward compatibility,` deno.json` is the preferred method. If both files exist in a function’s directory, the settings in` deno.json` will take precedence.


### Working with Private Packages Using` .npmrc`


Many organizations use private npm registries to host proprietary code. To use these private packages in your Supabase Edge Functions, you can add a` .npmrc` file to your function’s directory. This file works just like it does in a standard Node.js project, allowing you to specify registry URLs and authentication tokens.


After configuring your` .npmrc` file, you can import your private package using the` npm:` specifier just like any public package. The Supabase runtime will use the configuration to securely fetch the dependency during deployment.


### Setting a Custom Registry with` NPM_CONFIG_REGISTRY`


For organizations that route all package installations through a custom proxy or mirror for security or caching, Supabase offers another solution. You can set the` NPM_CONFIG_REGISTRY` environment variable. This tells the Supabase CLI and runtime to use your specified URL as the default registry for all npm packages. It’s a global setting that simplifies configuration when all your dependencies must come from a single, non public source.


## Advanced TypeScript Integration


Deno has first class support for TypeScript, but what happens when you import an npm package that was written in plain JavaScript? Deno provides a simple and elegant solution for adding type definitions.


### Importing Types from npm Packages in Deno


Many modern npm packages include their own type definitions, and Deno will automatically detect and use them. For those that don’t, you can pull in types from the DefinitelyTyped repository using a special comment directive.


For instance, if you’re using the popular` express` package, you can add its types like this:


```text
// @deno-types="npm:@types/express@^4.17"
import express from "npm:express@^4.17";


```


The` // @deno-types` comment instructs Deno to apply the types from` @types/express` to the` express` import. This gives you full autocompletion and type checking in your editor, providing the safety of TypeScript even when using JavaScript libraries. This powerful feature ensures that your **npm Supabase** development experience is both productive and secure.


Building a complex application requires getting both the backend and frontend right. Teams at companies like PwC and Decathlon have found success by pairing a robust backend like Supabase with a flexible frontend builder. Using a platform like[WeWeb](https://www.weweb.io/showcase) , they can rapidly develop internal tools and customer facing applications, proving that a smart technology stack is a major competitive advantage. You can also kickstart projects with[production-ready templates](https://www.weweb.io/templates) .


## Frequently Asked Questions about npm Supabase


**1. What is the main npm Supabase package?**
The main package is` @supabase/supabase-js` . It is the official JavaScript client library used to interact with your Supabase project from any JavaScript environment, including browsers and Node.js.


**2. Can I use any npm package in Supabase Edge Functions?**
Yes, Supabase Edge Functions run on Deno, which has native support for npm packages. You can import most npm packages directly into your functions using the` npm:` import specifier, unlocking a massive ecosystem of libraries for your serverless logic.


**3. How do I manage different dependency versions for different Edge Functions?**
The recommended approach is to place a separate` deno.json` configuration file inside each function’s directory. This allows you to define and pin specific versions of your **npm Supabase** dependencies for each function, ensuring they are isolated and won’t conflict with one another.


**4. What is the difference between` deno.json` and a legacy` import_map.json` ?**
Both are used to map dependency aliases, but` deno.json` is the modern, recommended standard. It is integrated into Deno’s tooling and offers a more streamlined configuration.` import_map.json` is a legacy format that is still supported but should be avoided in new projects.


**5. How do I use TypeScript types for a JavaScript npm package in Deno?**
You can use a special` // @deno-types` comment directive above your import statement. This tells Deno to fetch and apply type definitions from a specified types package, like those available in the` @types` scope on npm, giving you full IntelliSense and type safety.


Combining the power of the npm ecosystem with a Supabase backend gives you incredible flexibility. If you’re looking to build and launch your application faster, consider a visual development platform for your frontend. You can[build with WeWeb](https://www.weweb.io/) to turn your ideas into a reality with the speed of no code and the power of custom code when you need it. You can also accelerate scaffolding with[WeWeb AI](https://www.weweb.io/product/ai) .
