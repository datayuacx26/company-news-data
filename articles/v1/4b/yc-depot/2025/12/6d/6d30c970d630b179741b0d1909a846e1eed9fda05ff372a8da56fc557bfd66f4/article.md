---
schema_version: "1.0.0"
document_id: "6d30c970d630b179741b0d1909a846e1eed9fda05ff372a8da56fc557bfd66f4"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/graceful-shutdown-in-go"
published_at: "2025-12-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:e07010d5d61b0d8aa03cc256d70bebfaab99d5a28e17a8158f24c8a43cf1765f"
---

# Graceful Shutdown in Go

A friend messaged me the other day, "What's your strategy on propagating context canceled? Like should I be checking if the error is canceled and then returning nil? It's all just such a pain :P"


I started typing a response and realized it was going to be way too long for Signal. Figured it was worth writing up properly. So here we are.


At Depot we use a lot of[Go in our backend](https://depot.dev/blog/from-go-code-to-container-image-with-depot-api) . When I write Go programs I spend a lot of time thinking about the best way for them to shut down. I want to make sure that everything that needs to be completed actually gets done before the program exits.


For server-side software a clean and orderly shutdown matters a lot. From a client-side viewpoint, the client wants to finish its request to the server. From the server-side viewpoint, the server needs to complete or suspend its background tasks.


Additionally, because server software nowadays is turned on and off all the time with new versions, making sure tasks are in a good state before turning off can really reduce bugs.


I've rarely seen folks add a structured way to support server shutdown until way, way after the fact. And by that point, it can be difficult to sort out. I’m going to share some ways of handling graceful shutdown and talk about why it’s important.


## An example of clean shutdown at Depot


One of our most important services is written in Go. Here’s a high-level view of how it works.


We have queues of work to be done. Work might be turning on a machine, or running checks or tests. When we release a program change, the worker receives a signal to shut down. However, the worker might be “holding” work in progress. What does it do with that job? Since every queued job is a real customer waiting for a build, we’re definitely not going to just drop it.


The worker stops accepting any new work as soon as it receives the shutdown signal. A timeout countdown begins to give the worker time to finish any jobs in progress. Work that cannot be finished in time is returned to the queue for another worker to pick up.


Our work queues are just one example of where a clean shutdown makes our whole system more reliable. In the next few sections, we’ll go over the details of applying this shutdown pattern to any Go program.


## Best practices for using context and stopping


In Go, you use[context](https://pkg.go.dev/context) to say when time is up. The common pattern is to shut down when you receive unix signals like SIGTERM and SIGINT.


The following example shows how to create a context that cancels when the program receives SIGINT or SIGTERM signals:


```text
ctx, stop   :=   signal.  NotifyContext  (context.  Background  (), os.Interrupt, syscall.SIGTERM)
defer   stop  ()
```


This context,` ctx` , is created all the way at the top of main. It’s now the program’s parent context and is passed to all other parts of the program as a child context. When that parent context is done, all child contexts are immediately canceled. By default, this will interrupt any ongoing requests immediately and shut down the program.


The problem is that a running program holding state may need time to finish or clean up before shutdown. If the program exits immediately, that state vanishes.


For a graceful shutdown, you can use context to do two things. First, it can tell you to stop starting new work and second, it can tell you to finish current work.


### Stop starting new work


When the program gets a SIGTERM, the parent is done.


Here is a typical main loop that checks the context before it takes new work:


```text
outerLoop:
for   {
select   {
case   <-  ctx.  Done  ():
slog.  Info  (  "shutting down"  )
break   outerLoop
default  :
}
work   :=   <-  getWork
// ... do work
}
```


If the context cancels, the loop breaks. No new work begins. Note the` default` in the` select` ; this is a non-blocking check. The default means we don’t wait forever on the` ctx.Done()` channel. I certainly have made the mistake many times of forgetting the` default` !


### Wait for active jobs


Stopping input is not enough. Work in progress must end.


We use a` sync.WaitGroup` to count active jobs. Add one when a job starts. Done when it ends. After the loop breaks, we wait:


```text
for   {
// ... context check from previous example
work   :=   <-  getWork
wg.  Add  (  1  )
go   func  () {
defer   wg.  Done  ()
// ... do work
}
}   // our for loop!
wg.  Wait  ()
```


This WaitGroup keeps the program running until all jobs are done or they have saved their state. For example, at Depot long-running job sessions are one of the work states that matter because we cannot start them over from the beginning. We need to know how much work has finished.


### Finish current work


Many developers miss this part. When the main context cancels, the signal spreads. Every child context cancels. If you try to save state to a database with a context that has already been canceled, the write fails. Again a mistake yours truly has done many times. Nevertheless, you must save your state, or perhaps you must acknowledge a message from the queue.


The typical pattern for context propagation is something like this:


```text
ctx, cancel   :=   context.  WithTimeout  (parentCtx,   5  *  time.Second)
defer   cancel  ()
err   :=   Save  (ctx, state)
```


However, if instead you use` context.Background()` you can actually ignore the parent context’s stop signal. Use a timeout because nothing should run forever. This is what I call a detached context.


```text
ctx, cancel   :=   context.  WithTimeout  (context.  Background  (),   5  *  time.Second)
defer   cancel  ()
err   :=   Save  (ctx, state)
```


In this example, the task gets five seconds and ignores the parent context because it is using the context.Background(). If the program is signaled to stop at second three, the task keeps going. The longer timeout lets tasks finish. This contrasts with using the parent signal because the parent signal would stop the` Save()` immediately without necessarily saving anything.


## The complete shutdown pattern


Check` ctx.Done()` in polling loops. Stop polling if done. Pass parent context to operations that can be interrupted. They respect immediate shutdown because their work can be tried again.


Use` context.Background()` with a timeout for work that must finish for consistency.


Wait for everything to complete before exit.


Programs that follow these steps can stop at any time. They shut down cleanly without losing work or producing inconsistent results. This pattern gives your Go programs a clean and graceful shutdown. And hopefully your server startup is now clean and graceful too!


## Conclusion


Back to my friend's question about propagating` context.Canceled` . My recommended strategy is to identify and understand the places in your program where you must save state so you can resume cleanly. And then implement clean shutdown in those places. If you don't, you'll end up with "weird" states on startup that mean you can't really trust anything your program is doing.


If you can, you should think about shutdown early. It's way easier to implement when you're developing the program than it is to retrofit it later.


If you haven't implemented a clean shutdown, you don't need to be overwhelmed by the task. Start by identifying the parts of the program where it's really important to know where you left off. Maybe you don't want to have to repeat work for performance reasons, or you require consistent results for correctness reasons. That's where a clean shutdown will have the biggest impact.


In my experience, beyond better performance, clean shutdown avoids unexpected behavior at startup which equals far fewer bugs throughout the program. Your programs become a lot more performant and reliable.


## FAQ


How do I implement graceful shutdown in Go?


Use signal.NotifyContext to create a context that cancels on SIGTERM or SIGINT, then do three things: stop accepting new work by checking ctx.Done() in your polling loop, track active jobs with a sync.WaitGroup, and use context.Background() with a timeout for operations that must complete like saving state. The WaitGroup keeps your program running until everything finishes or times out.


Why do I need the default case when checking ctx.Done() in a select?


Without the default case, your select statement blocks waiting on ctx.Done() and never gets to receive new work. The default makes it a non-blocking check—you see if shutdown was signaled and immediately move on if not. I've definitely forgotten this more times than I'd like to admit, and it means your loop hangs instead of doing anything useful.


How long should the timeout be for operations that need to finish during shutdown?


Long enough to complete but short enough that your deployment doesn't time out. For database writes or queue acknowledgments, 5 seconds is usually reasonable. For longer work like Depot's build jobs, we give workers enough time to finish in progress work before the timeout kicks in. The key is knowing what work can realistically finish in time and what you need to resume later.


What happens to work that can't finish before shutdown completes?


You need to save enough state to resume or retry it cleanly. At Depot, incomplete jobs get returned to the queue for another worker to grab. The whole point of clean shutdown is making sure you know exactly where you left off. Otherwise you end up with work that's half-done and no way to know what actually completed, which leads to all those weird bugs at startup.


## Related posts


- [Go 1.24 remote caching explained](https://depot.dev/blog/go-remote-cache)
- [Container security at scale: Building untrusted images safely](https://depot.dev/blog/container-security-at-scale-building-untrusted-images-safely)
- [From Go code to container image with Depot API](https://depot.dev/blog/from-go-code-to-container-image-with-depot-api)


Chris Goller


Principal Software Engineer at Depot
