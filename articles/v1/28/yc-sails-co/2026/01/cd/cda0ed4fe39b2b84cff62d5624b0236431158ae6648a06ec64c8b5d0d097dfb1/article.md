---
schema_version: "1.0.0"
document_id: "cda0ed4fe39b2b84cff62d5624b0236431158ae6648a06ec64c8b5d0d097dfb1"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/wish-1-0-0/"
published_at: "2026-01-08T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:b5c5698a402fbfafd9a9c49c1ed73b376d49f6a8728563aabb7baf8cf484973e"
---

# Wish 1.0.0

Wish 1.0.0 is the first stable release of the OAuth hook for Sails.js. This release brings a completely redesigned configuration system that’s simpler, more flexible, and production-ready out of the box.


## What’s New


### Simplified Configuration


All OAuth configuration now lives under a single` providers` namespace in` config/wish.js` :


```text
// config/wish.js
module  .  exports  .wish   =   {
provider:   'github'  ,
providers: {
github: {
clientId:   'your-client-id'  ,
clientSecret:   'your-client-secret'  ,
redirect:   'http://localhost:1337/auth/callback'
}
}
}
```


### Automatic Environment Variable Detection


Wish now automatically detects credentials from environment variables. For production, you can set your env vars and use near-zero config - just specify the redirect URL (since Wish can’t know where your app handles callbacks):


```text
// config/wish.js
module  .  exports  .wish   =   {
provider:   'github'  ,
providers: {
github: {
redirect:   'https://myapp.com/auth/callback'
}
}
}
```


The following environment variables are automatically detected:


Provider Environment Variable


GitHub` GITHUB_CLIENT_ID`


GitHub` GITHUB_CLIENT_SECRET`


GitHub` GITHUB_CALLBACK_URL`


Google` GOOGLE_CLIENT_ID`


Google` GOOGLE_CLIENT_SECRET`


Google` GOOGLE_CALLBACK_URL`


### Config Resolution Order


Wish merges configuration in this order (later values override earlier):


1. **Built-in defaults** - scopes, OAuth URLs, etc.
2. **Environment variables** -` GITHUB_CLIENT_ID` , etc.
3. **Your config** - values in` config/wish.js` or` config/local.js`


This means you only need to configure what you want to change.


### Multiple Instances of Same Provider


Need multiple OAuth apps for the same provider? Use the` type` property:


```text
// config/wish.js
module  .  exports  .wish   =   {
providers: {
google: {
clientId:   'consumer-app-client-id'  ,
clientSecret:   'consumer-app-secret'  ,
redirect:   'http://localhost:1337/auth/google/callback'
},
'google-workspace'  : {
type:   'google'  ,    // Tells Wish this uses Google OAuth
clientId:   'workspace-client-id'  ,
clientSecret:   'workspace-secret'  ,
redirect:   'http://localhost:1337/auth/workspace/callback'
}
}
}
```


### Native Fetch


Wish now uses native` fetch` (Node.js 18+) instead of the` sails-hook-node-fetch` dependency. This simplifies installation and removes an unnecessary peer dependency.


## Breaking Changes


- **Node.js 18+ required** - Wish uses native` fetch`
- **Configuration namespace changed** - Credentials now go under` wish.providers` instead of at the root level
- **Removed` sails-hook-node-fetch` peer dependency**


## Migration Guide


### Step 1: Update Your Configuration


**Before (0.x):**


```text
// config/local.js
module  .  exports   =   {
github: {
clientId:   'your-client-id'  ,
clientSecret:   'your-client-secret'  ,
redirect:   'http://localhost:1337/auth/callback'
}
}
```


**After (1.0):**


```text
// config/local.js
module  .  exports   =   {
wish: {
providers: {
github: {
clientId:   'your-client-id'  ,
clientSecret:   'your-client-secret'  ,
redirect:   'http://localhost:1337/auth/callback'
}
}
}
}
```


### Step 2: Create config/wish.js


For production with environment variables, create` config/wish.js` with near-zero config - just specify the redirect URL:


```text
// config/wish.js
module  .  exports  .wish   =   {
provider:   'github'  ,    // or 'google'
providers: {
github: {
redirect:   'https://yourapp.com/auth/callback'
}
}
}
```


The` clientId` and` clientSecret` will be automatically loaded from` GITHUB_CLIENT_ID` and` GITHUB_CLIENT_SECRET` environment variables.


### Step 3: Remove Production OAuth Config


If you had OAuth credentials in` config/env/production.js` or` config/custom.js` , you can remove them. Just ensure your environment variables are set:


- ` GITHUB_CLIENT_ID`
- ` GITHUB_CLIENT_SECRET`
- ` GITHUB_CALLBACK_URL` (optional - can be set in config instead)


### Step 4: Update Package


```text
npm   update   sails-hook-wish
```


### Step 5: Remove sails-hook-node-fetch (If Installed)


If you had` sails-hook-node-fetch` as a dependency, you can remove it:


```text
npm   uninstall   @sailscasts/sails-hook-node-fetch
```


## Full Documentation


For complete documentation, visit[docs.sailscasts.com/wish](https://docs.sailscasts.com/wish/) .
