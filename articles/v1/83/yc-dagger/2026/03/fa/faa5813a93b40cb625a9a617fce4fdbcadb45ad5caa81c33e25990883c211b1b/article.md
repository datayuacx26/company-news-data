---
schema_version: "1.0.0"
document_id: "faa5813a93b40cb625a9a617fce4fdbcadb45ad5caa81c33e25990883c211b1b"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/cache-control-for-modules/"
published_at: "2026-03-30T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:1fea08546df1389b5c96193e0478e41b1f1e8c629a831966bc6fe2a9eee69b99"
---

# Cache control for modules

**TL;DR: Dagger has always cached system functions. Now module functions are cached too. Use` cache: "never"` for side effects.**


[Read the changelog](https://dagger.io/changelog#function-cache-control)


Dagger executes your CI pipelines incrementally: when a pipeline runs twice, it skips work that’s already done and can complete faster. This caching process happens automatically, sparing you the pain of maintaining fragile configuration files.


Until now, only system functions could be cached in this way, and not functions defined in a module.


Without module caching


In practice, system functions do most the heavy lifting; modules are the cheap orchestration glue, so the overhead of re-running them is acceptable - but it can still add up, especially in large pipelines! So, as of Dagger 0.19.4, module functions are cached by default.


With module caching


Module developers can use a new cache control API to tell the engine which functions have side effects.


## Cache control


To cache module functions by default, we needed a way to know whether your function is pure. A function called` deploy()` looks the same as` build()` to the engine, and caching the wrong one would break your pipeline. The solution is to annotate` deploy()` to let us know that it has a side effect. This is the purpose of cache control.


**TypeScript**


```text
@  func  ()                       // default: cached up to 7 days
async   build  ():   Promise  <  Directory  >   {   ...   }


@  func  ({ cache:   "15m"   })      // re-fetch periodically
async   latestBaseDigest  ():   Promise  <  string  >   {   ...   }


@  func  ({ cache:   "session"   })   // this engine session only
async   currentUser  ():   Promise  <  string  >   {   ...   }


@  func  ({ cache:   "never"   })    // always execute
async   deploy  ():   Promise  <  string  >   {   ...   }
```


**Python**


```text
@dagger.function                      # default: cached up to 7 days
def   build  (self) -> dagger.Directory:   ...


@dagger.function  (  cache  =  "15m"  )        # re-fetch periodically
def   latest_base_digest  (self) ->   str  :   ...


@dagger.function  (  cache  =  "session"  )    # this session only
def   current_user  (self) ->   str  :   ...


@dagger.function  (  cache  =  "never"  )      # always execute
def   deploy  (self) ->   str  :   ...
```


**Go**


```text
func   (  m   *  MyModule  )   Build  ()   *  dagger  .  Directory   {   ...   }    // default: cached up to 7 days


// +cache="15m"
func   (  m   *  MyModule  )   LatestBaseDigest  ()   string   {   ...   }     // re-fetch periodically


// +cache="session"
func   (  m   *  MyModule  )   CurrentUser  ()   string   {   ...   }          // this session only


// +cache="never"
func   (  m   *  MyModule  )   Deploy  ()   string   {   ...   }               // always execute
```


## The four modes


Mode Syntax Use case


Default *(none)* Pure functions. Most of your code.


TTL` "10s"` ,` "15m"` ,` "2h"` External data that goes stale.


Session` "session"` Consistent within a run, not across runs.


Never` "never"` Deployments, notifications, side effects.


## Backwards compatibility


Existing projects get` "disableDefaultFunctionCaching": true` added to` dagger.json` , which preserves the old behavior. New projects get caching by default. To opt in, mark your side-effecting functions with` cache: "never"` , then remove the flag.


Learn more in the[function caching docs](https://docs.dagger.io/extending/function-caching) , or see the[v0.19.4 changelog](https://dagger.io/changelog#v0.19.4) .
