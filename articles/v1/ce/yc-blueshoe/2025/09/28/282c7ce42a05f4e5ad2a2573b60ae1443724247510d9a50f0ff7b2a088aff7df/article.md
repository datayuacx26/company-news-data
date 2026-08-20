---
schema_version: "1.0.0"
document_id: "282c7ce42a05f4e5ad2a2573b60ae1443724247510d9a50f0ff7b2a088aff7df"
company_key: "yc-blueshoe"
company: "Blueshoe"
source_id: "yc-blueshoe-news-import-28975b8749e8"
canonical_url: "https://www.blueshoe.io/blog/nuxt4-new-features/"
published_at: "2025-09-22T10:00:00+00:00"
first_seen_at: "2026-07-24T11:21:12.712803+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:f08490e0c3b7ad7d71efec5d7f28d5f63f716ed5c39189e956f31993bdfb032e"
---

# Nuxt 4 is here: A Deep Dive into the New Features and What They Mean for Developers | BLUESHOE

## Table of Contents


- Nuxt 4 – Evolution instead of Revolution
- The Top New Features in Nuxt 4 in Detail
- The Upgrade Process: How to Successfully Switch from Nuxt 3 to 4
- What Does Nuxt 4 Mean for the Future? A Look Ahead
- Conclusion: Why Upgrading to Nuxt 4 is Worth it Now
- FAQ – Frequently Asked Questions about Nuxt 4


---


## Meet the Author


[Lukas Rüsche](https://www.blueshoe.io/team/lukas-ruesche/) With Nuxt 4, we're doubling down on developer experience through an evolutionary approach, delivering a more stable and performant framework that feels like a natural upgrade, not a rewrite.


Last updated:


2025-09-22


---


Nuxt


Vue.js


Development


Performance


Documentation


---


2025-09-22


# The next evolution of Nuxt is here


Nuxt 4 is here: A Deep Dive into the New Features and What They Mean for Developers


After months of speculation and intensive beta testing, the Nuxt team has officially released Nuxt 4.


Unlike the jump from Nuxt 2 to 3, this time the team is focusing on evolution instead of revolution – an approach that is much more developer-friendly.
In this article, we take a detailed look at the most important new features and show how Nuxt 4 noticeably improves the daily life of a developer. From the new directory structure to smarter data fetching features – here you'll learn everything you need to know about the framework's evolution.


## Nuxt 4 – Evolution instead of Revolution


### The Success of Nuxt 3


Nuxt 3 has fundamentally changed Vue development. While previous approaches often struggled with complex build configurations and tedious SSR setups, Nuxt 3 brought a new ease to development with the Nitro server, automatic imports, and seamless TypeScript integration.


### The New Strategy: Continuous Improvement


The Nuxt team has learned from past experiences. Instead of another major rewrite, they are focusing on the continuous improvement of existing concepts. This approach significantly reduces the migration effort and allows developers to benefit from the new features step by step.


### Practical Implications


The result is a framework update that allows existing Nuxt 3 projects to be upgraded without major disruptions. The improvements are immediately noticeable, while the migration effort remains manageable – a welcome contrast to previous major updates.


## The Top New Features in Nuxt 4 in Detail


### New Project Structure: Better Organization with the app Directory


One of the most noticeable new features in Nuxt 4 is the optional` app/` directory structure. This solves a well-known problem: in larger projects, numerous folders quickly accumulate in the root directory, which affects clarity.


The new structure clearly separates app-specific code from configuration files. All application files are organized under` app/` , while configuration files remain in the root directory.


**Advantages of the new structure:**


- **Clearer separation** between app code and configuration
- **Better clarity** in larger projects
- **Consistency** with other modern frameworks like Next.js
- **Improved IDE support** through a clearer directory structure


**Comparison of structures:**


```text
# Old structure (Nuxt 3)
/
├──   components/
├──   pages/
├──   composables/
├──   layouts/
├──   middleware/
├──   plugins/
├──   assets/
├──   public/
├──   server/
├──   nuxt.config.ts
└──   package.json


# New structure (Nuxt 4)
/
├──   app/
│     ├──   components/
│     ├──   pages/
│     ├──   composables/
│     ├──   layouts/
│     ├──   middleware/
│     ├──   plugins/
│     ├──   assets/
│     ├──   utils/
│     ├──   app.vue
│     └──   app.config.ts
├──   content/
├──   layers/
├──   modules/
├──   public/
├──   server/
├──   shared/
├──   nuxt.config.ts
└──   package.json


```


The new structure makes it much easier to distinguish between app-specific code and configuration files. Especially in larger teams and enterprise projects, this leads to significantly better code organization and reduces the time needed to search for specific files.


### Singleton Data Fetching Layer: Revolutionary Caching System


Data fetching in Nuxt 4 has been fundamentally revised and introduces the "Singleton Data Fetching Layer". This innovation brings significant improvements in performance and consistency.


**What has changed:**


**1. Shared Refs for the same Key:** All calls to` useAsyncData` or` useFetch` with the same key now share the same` data` ,` error` , and` status` refs. This means that all calls with an explicit key must not have conflicting` deep` ,` transform` ,` pick` ,` getCachedData` , or` default` options.


**2. Extended` getCachedData` Control:** The` getCachedData` function is now called on every data fetch, even if it is caused by a watcher or` refreshNuxtData` . The function receives a context object with the cause of the request, which allows more control over the use of cached data.


**3. Reactive Key Support:** You can now use computed refs, plain refs, or getter functions as keys. This enables automatic data refetching and stores data separately.


**4. Automatic Data Cleanup:** When the last component that fetches data with` useAsyncData` is unmounted, Nuxt automatically cleans up the corresponding data from the cache.


**Practical Example: E-commerce Product Page**


Imagine you are building an e-commerce site with product details and related products:


```text
// Product page - loads product data
const   { data: product }   =   await   useFetch  (  `/api/products/  ${  productId  }  `  , {
key:   `product-  ${  productId  }  `
})


// Related products component - shares the same data
const   { data: relatedProducts }   =   await   useFetch  (  `/api/products/  ${  productId  }  /related`  , {
key:   `related-  ${  productId  }  `
})


// Shopping cart component - loads product data again
const   { data: cartProduct }   =   await   useFetch  (  `/api/products/  ${  productId  }  `  , {
key:   `product-  ${  productId  }  `   // Same key = same data!
})
// → No additional API call, uses cached data


```


**What's happening here:**


- **One API call** for product data, but **three components** use the data
- **Automatic caching** prevents duplicate requests
- **Shared refs** ensure that all components stay in sync


**Real-world Scenario: Blog with Comments**


```text
// Blog post page
const   { data: post }   =   await   useFetch  (  `/api/posts/  ${  postId  }  `  , {
key:   `post-  ${  postId  }  `
})


// Comments component (same page)
const   { data: comments }   =   await   useFetch  (  `/api/posts/  ${  postId  }  /comments`  , {
key:   `comments-  ${  postId  }  `
})


// User navigates to another page → components are unmounted
// → Cache is automatically cleared (no memory leaks!)


// User returns → data is reloaded
// → Fresh data, optimal memory consumption


```


**Advantages in practice:**


- **Fewer API calls** = faster loading times
- **Synchronized data** = no inconsistencies between components
- **Automatic cleanup** = no memory leaks during navigation
- **Intelligent caching** = better user experience


**Pro-Tip: Composable for reusable Data Fetching Logic**


Since all components with the same key automatically share the same data, it makes sense to extract the` useAsyncData` /` useFetch` calls into their own composables:


```text
// composables/useProduct.js
export   function   useProduct  (  productId  :   string  ) {
return   useAsyncData  (
`product-  ${  productId  }  `  ,
()   =>   $fetch  (  `/api/products/  ${  productId  }  `  ),
{
deep:   true  ,
transform  : (  product  )   =>   ({
...  product,
formattedPrice:   `€  ${  product.price.  toFixed  (  2  )  }  `  ,
lastViewed:   new   Date  ()
})
}
)
}


// In different components
const   { data: product }   =   await   useProduct  (productId)
// → All use the same data and transformation logic


```


**Advantages:**


- **Consistent keys** across all components
- **Centralized transformation logic** (price formatting, etc.)
- **Easy maintenance** and updates
- **TypeScript support** through central types


**Important Notes:**


- **Consistent options:** All calls with the same key must have identical` deep` ,` transform` ,` pick` ,` getCachedData` , or` default` options
- **Reactive keys:** Computed refs, plain refs, or getter functions as keys enable automatic refetching
- **Memory management:** Automatic cleanup prevents memory leaks in large applications


**Configuration: Adjusting Cache Behavior**


If you want to disable the new cache behavior, you can configure it in` nuxt.config.ts` :


```text
export   default   defineNuxtConfig  ({
experimental: {
granularCachedData:   false  ,    // Disables granular caching
purgeCachedData:   false        // Disables automatic cleanup
}
})


```


**When this makes sense:**


- **Legacy projects** with complex caching requirements
- **Step-by-step migration** of existing caching strategies
- **Debugging** cache problems during development


These improvements ensure that unnecessary API calls are avoided and the application becomes significantly more performant. The difference is particularly noticeable in complex applications with many data sources.


### Developer Experience: Improved Productivity and Efficiency


The developer experience has been significantly improved in Nuxt 4, which has a direct impact on developer productivity. The optimizations focus on faster workflows and better development tools.


**Performance Improvements:**


- **Significantly faster start times** for the development server
- **Improved HMR reliability** (Hot Module Replacement)
- **Optimized TypeScript integration** for better type safety and autocompletion
- **Intelligent build optimizations** for faster compile times


**Practical Improvements:**


- Seamless integration with modern IDEs
- Better error messages and debugging information
- Optimized bundle size through intelligent tree-shaking
- Improved source maps for more efficient debugging


These improvements ensure that developers spend less time waiting and debugging and can concentrate more on the actual development.


## The Upgrade Process: How to Successfully Switch from Nuxt 3 to 4


The switch from Nuxt 3 to Nuxt 4 has been deliberately designed to be straightforward. The Nuxt team has placed great emphasis on ensuring that existing projects can be migrated without major changes. After the upgrade, most Nuxt 4 behaviors are already standard, but some features can still be configured to ensure backward compatibility during migration.


### Step 1: Update Nuxt to Version 4


The first step is to update the Nuxt package to version 4:


```text
# With npm
npm   install   nuxt@^4.0.0


# With yarn
yarn   add   nuxt@^4.0.0


# With pnpm
pnpm   add   nuxt@^4.0.0


# With bun
bun   add   nuxt@^4.0.0


```


**What's happening here?**


- The` nuxt` package is updated to version 4
- All Nuxt 4 behaviors are activated
- Most existing configurations remain functional


### Step 2: Perform Migration


You have two options for migration:


#### Option A: Automatic Migration with Codemods (recommended)


The Nuxt team has collaborated with the Codemod team to automate many migration steps:


```text
# Run all migration codemods


# With npm
npx   codemod@latest   nuxt/4/migration-recipe


# With yarn
yarn   dlx   codemod@latest   nuxt/4/migration-recipe


# With pnpm
pnpm   dlx   codemod@latest   nuxt/4/migration-recipe


# With bun
bun   x   codemod@latest   nuxt/4/migration-recipe


```


**What do the codemods do?**


- Automatic adjustment of the directory structure
- Migration of outdated configuration options
- Adjustment of TypeScript configurations
- Update of import paths and aliases


#### Option B: Manual Migration


If you want to perform the migration manually or the codemods do not cover all aspects:


**2.1 Create New Directory Structure**


The new directory structure offers better performance and IDE support:


```text
# Create new app/ directory
mkdir   app


# Move existing directories into app/
mv   assets   app/
mv   components   app/
mv   composables   app/
mv   layouts   app/
mv   middleware   app/
mv   pages   app/
mv   plugins   app/
mv   utils   app/


# Move app-specific files
mv   app.vue   app/
mv   error.vue   app/
mv   app.config.ts   app/


```


**2.2 Clean up Root Directory**


Make sure these directories remain in the root:


- ` nuxt.config.ts`
- ` content/`
- ` layers/`
- ` modules/`
- ` public/`
- ` server/`


**2.3 Adjust TypeScript Configuration**


Nuxt 4 uses new TypeScript project references for better type safety:


```text
// tsconfig.json
{
"files"  : [],
"references"  : [
{   "path"  :   "./.nuxt/tsconfig.app.json"   },
{   "path"  :   "./.nuxt/tsconfig.server.json"   },
{   "path"  :   "./.nuxt/tsconfig.shared.json"   },
{   "path"  :   "./.nuxt/tsconfig.node.json"   }
]
}


```


```text
// package.json - Update type-checking scripts
{
"scripts"  : {
"typecheck"  :   "nuxt prepare && vue-tsc -b --noEmit"
}
}


```


**Move Type Augmentations:**


- **App context:** Move files to` app/`
- **Server context:** Move files to` server/`
- **Shared:** Move files to` shared/`


**2.4 Adjust Configuration**


**Migrate outdated generate configuration:**


```text
// Old configuration (Nuxt 3)
export   default   defineNuxtConfig  ({
generate: {
exclude: [  '/admin'  ,   '/private'  ],
routes: [  '/sitemap.xml'  ,   '/robots.txt'  ]
}
})


// New configuration (Nuxt 4)
export   default   defineNuxtConfig  ({
nitro: {
prerender: {
ignore: [  '/admin'  ,   '/private'  ],
routes: [  '/sitemap.xml'  ,   '/robots.txt'  ]
}
}
})


```


**Remove Experimental Features:**


These features are no longer configurable in Nuxt 4:


- ` experimental.treeshakeClientOnly` (always` true` )
- ` experimental.configSchema` (always` true` )
- ` experimental.polyfillVueUseHead` (always` false` )
- ` experimental.respectNoSSRHeader` (always` false` )


### Step 3: Testing and Validation


```text
# Start development server
npm   run   dev


# Run type-checking
npm   run   typecheck


# Test build
npm   run   build


# Run all tests
npm   run   test


```


**What to test:**


- All pages load correctly
- Data fetching works
- TypeScript errors are resolved
- Performance improvements are visible
- All modules and plugins work


### Step 4: Backward Compatibility (if necessary)


If you want to keep the old directory structure:


```text
// nuxt.config.ts
export   default   defineNuxtConfig  ({
// Keep V3 structure
srcDir:   '.'  ,
dir: {
app:   'app'
}
})


```


### Important Breaking Changes


Nuxt 4 brings some significant changes that need to be considered during migration:


**1. New Directory Structure:**


- Default` srcDir` is now` app/` instead of the root directory
- ` serverDir` is now` /server` instead of` /server`
- ` layers/` ,` modules/` , and` public/` are resolved relative to``


**2. Singleton Data Fetching Layer:**


- Shared refs for the same key in` useAsyncData` and` useFetch`
- Extended` getCachedData` control with context object
- Reactive key support for automatic refetching
- Automatic data cleanup on unmount


**3. TypeScript Configuration:**


- New project references for better type safety
- Separate configurations for app, server, and build time
- Type augmentations must be in corresponding directories (` app/` ,` server/` ,` shared/` )


**Migration Guide:** For detailed information, we recommend consulting the[official Nuxt 4 Upgrade Guide](https://nuxt.com/docs/4.x/getting-started/upgrade) .


## What Does Nuxt 4 Mean for the Future? A Look Ahead


### The Roadmap: What Comes After Nuxt 4?


The Nuxt team is already working on **Nuxt 5** and **Nitro v3** , which will bring more revolutionary features. Nuxt 4 forms the solid foundation for these future developments.


**Planned Features for the Future:**


- Advanced Server-Side Rendering optimizations
- Improved edge computing support
- Enhanced TypeScript integration
- New performance metrics and monitoring tools


### The Importance for the Ecosystem


**Module Developers:** Can gradually adapt their modules to the new features and benefit from the improved API.


**Community:** Benefit from the improved developer experience and new opportunities for performance optimizations.


**Enterprise:** Have a stable basis for long-term projects with predictable upgrade paths.


### Classification: How Nuxt Continues to Expand its Position as a Leading Vue Framework


Nuxt 4 solidifies its position as one of the most modern and developer-friendly Vue frameworks and sets new standards in web development. The focus on stability and continuous improvement makes it the ideal choice for professional projects.


**Competitive Advantages:**


- Superior developer experience compared to other Vue frameworks
- Better performance through intelligent optimizations
- Strong community and extensive ecosystem
- Enterprise-ready with long-term support


## Conclusion: Why Upgrading to Nuxt 4 is Worth it Now


### Summary of the Main Advantages:


**Performance:** Significantly faster start times and better HMR reliability ensure smoother development.


**Code Quality:** Better project structure through the new` src` directory organization and optimized TypeScript integration.


**Productivity:** Smarter data fetching and an improved developer experience lead to less waiting time and more efficient development.


**Future-Proof:** Stable basis for long-term projects with predictable upgrade paths.


### Clear Recommendation


Upgrading to Nuxt 4 is recommended for all developers and teams already working with Nuxt 3. The improvements are immediately noticeable, while the migration effort remains minimal.


**For Teams:** The improved developer experience and performance optimizations justify the upgrade after a short time.


**For Enterprise:** The focus on stability and long-term support makes Nuxt 4 the ideal choice for professional projects.


Check out the[official Nuxt 4 documentation](https://nuxt.com/) now and start your first project with Nuxt 4!


---


*Have you already had experience with Nuxt 4? Share your insights in the comments and let us know which features excite you the most!*


---


## FAQ – Frequently Asked Questions about Nuxt 4


### 1. What are the most important new features in Nuxt 4?


The most important new features in Nuxt 4 are the optional` src` directory structure for better project organization, smarter data fetching with automatic caching and deduplication, and an improved developer experience with a faster CLI and optimized TypeScript integration. The focus is on stability and a smooth upgrade path from Nuxt 3.


### 2. How is Nuxt 4 different from Nuxt 3?


Nuxt 4 builds on Nuxt 3 and focuses on evolution rather than revolution. While Nuxt 3 was a complete rewrite, Nuxt 4 concentrates on refining existing concepts. The new` src` directory structure offers better organization, data fetching has become smarter, and the developer experience has been improved with faster start times and better HMR reliability.


### 3. Is the upgrade from Nuxt 3 to Nuxt 4 complicated?


No, the upgrade from Nuxt 3 to Nuxt 4 has been deliberately designed to be straightforward. The Nuxt team has placed great emphasis on ensuring that existing projects can be migrated without major changes. The breaking changes are minimal, and most existing projects should work without major adjustments.


### 4. What are the benefits of the new src directory structure in Nuxt 4?


The new optional` src` directory structure provides a clearer separation between configuration files and the actual app code. This leads to a better overview in large projects, a consistent structure with other modern frameworks, and cleaner project organization. All app-specific files are organized under` src/` organized, while configuration files remain in the root directory.


### 5. How does Nuxt 4 improve data fetching?


Nuxt 4 introduces smarter data fetching with automatic caching and deduplication of requests with the same key. Additionally, there is automatic cleanup when components are unmounted, which prevents memory leaks. The improved error handling and retry mechanisms ensure more robust applications.


### 6. What performance improvements does Nuxt 4 offer?


Nuxt 4 offers significantly faster development server start times, improved HMR reliability, and optimized TypeScript integration. In addition, intelligent build optimizations ensure faster compile times and better bundle sizes through improved tree-shaking.


### 7. Can I test Nuxt 4 features in Nuxt 3 already?


Yes, you can test features from Nuxt 4 in Nuxt 3 by using the latest beta versions. This allows for a gradual migration and early detection of compatibility issues. The official migration guide provides detailed instructions for the transition.


### 8. What are the breaking changes in Nuxt 4?


Since Nuxt 4 focuses on stability, the breaking changes are minimal. Most existing projects should work without major adjustments. Important changes mainly concern the optional` src` structure and some outdated API functions that have been replaced by more modern alternatives.


---


## Do you have questions or an opinion? With your GitHub account you can let us know...


---


### Here are a few articles that you might also find interesting:


[Act, Don't Isolate: The Blueshoe Approach to Kubernetes Development Develop locally, test in the cluster - without CI/CD wait times. Gefyra promises the holy grail of Kubernetes development: full-speed coding with real cluster context. In our case study, you’ll learn why this tool is a game-changer for your daily operations and how to efficiently integrate it into your workflow by Carl-Christian Neundorf](https://www.blueshoe.io/blog/gefyra-hands-on/)


[django-o365: A Modern Mail Backend for Django and Exchange Online](https://www.blueshoe.io/blog/django-o365-a-modern-mail-backend-for-django-and-exchange-online/)


[django-o365](https://github.com/Blueshoe/django-o365) is an open-source mail backend for Django that enables sending emails via Exchange Online using OAuth 2.0. Born from real-world project requirements, this package is aimed at teams using Microsoft 365 who want to implement their email dispatch securely, maintainably, and without workarounds. This article provides an overview of the motivation, features, and usage.


by Robert Gutschale


[Mastering Django Caching: The Ultimate Guide from Cachalot to the Low-Level API Is your Django application slower than it should be? Long loading times not only frustrate your users but are also penalized by search engines. The solution often lies in a smart caching strategy. But where do you start? by Jonathan Kölbl](https://www.blueshoe.io/blog/django-caching-cachalot-performance-guide/)


[Handoff from Designer to Developer: What We Need for Frontend Implementation The transition from design to development is a critical moment in any frontend project. A well-thought-out handoff can save weeks of development time, while an incomplete handoff leads to endless questions, delays, and compromises. by Lukas Rüsche](https://www.blueshoe.io/blog/designer-to-developer-handoff/)


[Why Our Maintenance Reports Are a Key to Successful Collaboration For us, a maintenance report is more than a table of numbers: it is a tool for communication, planning, and trust. It shows what we have done, where potential lies, and what topics are on the agenda for the next quarter - so our customers know their systems are in good hands. by Robert Gutschale](https://www.blueshoe.io/blog/quarterly-maintenance-reports/)


[More Blog Articles Discover exciting insights and practical tips in our latest blog posts. From Python and Rust to Kubernetes, Django security, and frontend topics – find valuable knowledge for your projects here. Click to explore all articles!](https://www.blueshoe.io/blog/)


## These companies trust Team Blueshoe


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
