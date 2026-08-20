---
schema_version: "1.0.0"
document_id: "d02f8f4e3d3c9adcc1a25987e742653538273aadb19b212a06dcbac878b4e164"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/execution-management-in-rclcpp"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:d13d4a9e28632b4e1daaa7c3b02741ec5ca58e9df2b310b5bd4e90065105ecd2"
---

# Polymath Robotics Blog | The Evolution of Execution Management in rclcpp

# Introduction


If you’re a ROS 2 user, you’ve probably written code that called` rclcpp::spin()` hundreds of times, and you’ve probably not given much thought to what it’s actually doing under the hood. And that’s partially by design! The whole point of ROS’s abstractions is that you can focus on the actual business logic for your robot in the form of callbacks, and leave how they’re actually run up to something else.


That “something else” is the executor, and for the last few releases, it’s been changing in ways that impact real performance on real machines. In the newly released ROS 2 Lyrical Luth (happy belated World Turtle Day!), the client library working group shipped an executor implementation that demonstrably outperforms the classic executors that most ROS stacks are utilizing by default.


This post is a tour of how we got here. We’ll briefly review the mechanisms of execution management in ROS, dive into the implementation details of the traditional C++ executors, their bottlenecks, and the evolution into the more optimized alternatives that exist today. We’ll then tie it all together at the end with some brand new lyrical executor benchmarks!


## The Moving Parts, In Brief


Let’s review some important parts of the ROS stack one more time:


The **Executor** lives in the client library code (rclcpp, rclpy, etc) and owns the thread(s) invoking your callbacks.` spin()` invokes one or more threads to check for ready entities (subscriptions, service / action servers, timers, waitables, etc) and service their callbacks.


**Callback Groups** are what lets us define what is safe to run in parallel for multi-threaded executors. Putting callbacks inside of a **Mutually Exclusive** callback group will ensure at most only one callback inside that group runs at once. Meanwhile, callbacks in a **Reentrant** group, or inside separate callback groups, will be allowed to run in parallel.


The **Wait Set** is how the single and multi-threaded executors poll for new work. It’s a flat collection of **entities** (subscribers, servers, timers, waitables) that gets rebuilt on each iteration and passed on down to the middleware to wait on all of them at once.


## The Classic Execution Model


### The Single Threaded Executor


The simplest executor is the one you’ve probably been using your entire ROS 2 career whether you know it or not. When you invoke **rclcpp::spin(node)** in your **main.cpp** , it expands to the following behind the scenes:


` rclcpp::executors::SingleThreadedExecutor exec;
exec.add_node(node);
exec.spin();`


For running multiple nodes all on the same thread, you may have either used a component container, or manually invoked the executor like so:


` auto node_a = std::make_shared<NodeA>();
auto node_b = std::make_shared<NodeB>();
rclcpp::executors::SingleThreadedExecutor exec;
exec.add_node(node_a);
exec.add_node(node_b);
// both nodes' callbacks run, one at a time, on this one thread
exec.spin();`


where **spin()** looks like this:


` while (rclcpp::ok(this->context_) && spinning.load()) {
rclcpp::AnyExecutable any_executable;
if (get_next_executable(any_executable)) {
execute_any_executable(any_executable);
}
}`


### The Multi-threaded Executor + Callback Groups


Once you’re at the point where you need to run callbacks in parallel, you reach for the **MultiThreadedExecutor** . It introduces multiple worker threads to pull ready callbacks off the same shared pool and execute them according to how their callback groups are set up. Callback groups themselves were an iteration on the executor interface to allow for more granularity in execution management. They were originally proposed by Ralph Lange from Bosch at ROSCon 2018, and made it into rclcpp two years later thanks to Pedro Pena and William Woodall.


In practice, you want to carve out your callback groups based on *how safe they are* to run in parallel. Is your callback modifying a bunch of existing node state, putting a message into a thread-safe data structure, etc.


One nice thing about these executors is that they’re fairly simple to reason about! Look for ready entity, service ready entity, rinse and repeat in a nice tidy loop. What’s so wrong about that?


## The Performance Bottlenecks


These two executors make use of the same underlying polling mechanism for determining which entities are ready to fire. Over the years, there were significant optimizations to this process such as the **StaticSingleThreadedExecutor** (introduced by Nobleo, with later improvements from iRobot). It introduced caching of entities such that the wait-set did not need to be rebuilt on every iteration. It worked well enough that by Jazzy, it became a generalized part of the single- and multi-threaded executors and the **StaticSingleThreadedExecutor** was deprecated and eventually removed. However, despite the improved entity bookkeeping, the polling mechanism still remained - and with it, significant performance bottlenecks.


### **Polling overhead**


Let’s go back to the “poll for ready work” diagram from before. In each loop iteration, the executor is asking every entity it owns if it has ready work:


Not so bad with a handful of entities, right? But now let’s scale up this example…


So since the executor polls for new work on every entity, the runtime cost of checking which entities are ready scales linearly with the number of entities your executor is keeping track of. On every **spin_once()** , a node with one busy subscriber and 49 idle ones is paying the same cost to check for new work as a node with 50 busy subscribers.


### How parallel is “multi-threaded”, actually?


Another subtle bugbear about the **MultiThreadedExecutor** is that the “find ready work” step needs to be protected under a **wait_mutex_** :


` while (rclcpp::ok(this->context_) && spinning.load()) {
rclcpp::AnyExecutable any_exec;
{
std::lock_guard wait_lock{wait_mutex_};
// every worker thread serializes here!
if (!get_next_executable(any_exec, next_exec_timeout_))
continue;
}
// all that actually runs in parallel
execute_any_executable(any_exec);
}`


This means in practice, the discovery half of the whole loop we’re trying to parallelize is effectively still run in series across all threads. Going back to the diagram from earlier:


So the more threads are added, the more contention is put onto the one step they all must do first! A step which, as we just discussed, scales quite poorly with the number of ROS entities. So adding more threads can mean that you’re actually paying more scheduling overhead costs without getting more work done!


And perhaps you’re thinking to yourself *“Well Skye, I snagged my 128-core hyper-v gamer mode threadripper before the AI hype cycle and global supply chain instability drove up the cost of computer parts, so why should I care? You said earlier that I’m not even supposed to have to think about any of this anyway!”* And perhaps you’ve got enough luck or money to be right. But compute certainly isn’t getting cheaper anytime soon, and the bottlenecks and resulting CPU contention become so much more apparent in mass-produced robotic systems, and *especially* on constrained hardware like ARM SBCs (which is what we mostly used at iRobot!) with fewer, weaker cores than most x86 PCs.


## Shifting the paradigm


The **EventsExecutor** , written by Alberto Soragna during his time at iRobot, *does away with the concept of wait-sets and polling entirely* . It builds on the core philosophy of not paying for what you don’t use. The loop from before is inverted - Instead of polling for ready entities in **rcl_wait()** on every iteration of **spin()** , a callback is now registered with each entity at the RMW layer at construction time, so that the entities can just tell the executor when they’re ready instead!


` typedef void (* rmw_event_callback_t)(const void * user_data, size_t number_of_events);`


RMW implementations call callbacks with this signature when a new message lands on a subscription, a service / action server receives a request, client gets a response, or on QoS events. The callback pushes an **ExecutorEvent** onto a thread-safe **events_queue** , and the **spin()** thread pops ready entities from the queue and executes their callbacks in FIFO order. Timers are serviced from a dedicated timer manager with its own thread that enqueues ready events when a timer fires.


This events-driven system, along with an rclcpp fork which added support for fully intra-process comms for actions and services, is what enabled my former team at iRobot to efficiently run a navigation stack consisting of many nodes in a single process, with minimal CPU contention and a reasonable memory footprint, *on a Raspberry Pi 4* . Which is why we were using it as our default executor for almost all of our internal ROS projects.


### Closing the Gaps with the Callback Group Events Executor


The events queue represented a huge leap forward in ROS 2 performance, however the original implementation suffered from limitations that kept it from graduating out of the **experimental** namespace and gaining wider adoption:


- No support for callback group concurrency, because only one thread to service the queue
- No support for simulated time (this was a big one!)
- Fast firing timers with callbacks longer than the timer period would overrun the queue faster than the **spin()** thread could process events, degrading latency for other entities


These issues were significant enough such that Janosch Machowinski from Cellumation wrote a new implementation from scratch to address them - the **EventsCBGExecutor** . The events subsystem is retained from its predecessor, but events are now put into a callback group aware **FIFOScheduler** which can be efficiently serviced from multiple threads, and the timer manager was reworked to support multiple sources of ROS time and remove actively firing timers from the queue so they aren’t serviced more than once.


`
while (rclcpp::ok(this->context_) && spinning.load()) {
sync_callback_groups();
auto ready_entity = scheduler->get_next_ready_entity();


if (!ready_entity.entity) {
scheduler->block_worker_thread();
continue;
}
if (ready_entity.moreEntitiesReady) {
scheduler->unblock_one_worker_thread();
}
ready_entity.entity->execute_function();
scheduler->mark_entity_as_executed(*ready_entity.entity);
}`


So we keep the efficiency gains from the events queue and combine it with a thread safe scheduler that does away with that globally serialized “find work” step and enables efficient parallelism, while adding support for simulation time! Huzzah!


After the **EventsCBGExecutor** was proven out on Cellumation’s systems, and[benchmarked against the original](https://discourse.openrobotics.org/t/ros2-state-of-the-events-executors-benchmark-comparison-between-rclcpp-eventsexecutor-and-cm-executors-eventscbgexecutor/50337) **EventsExecutor** to ensure no performance regressions in single-threaded mode, the client library working group decided to promote it out of the experimental namespace and it was landed in the source tree, along with[a refactor of component containers](https://github.com/ros2/rclcpp/pull/3134) that includes support for it, just in time for the Lyrical Luth feature freeze!


### What the Benchmarks Say, In Brief


I wanted to augment this blog post with some fresh benchmarks off of the new release, just to put some numbers behind these performance claims on top of the headliner “10 - 15% less CPU”. In particular I wanted to find out *just how much* the **wait_mutex_** was slowing everything down, so I spent a bit of time throwing together some cross-executor comparison graphs by running[ros2-benchmark-container](https://github.com/skyegalaxy/ros2-benchmark-container) off of the Lyrical testing docker images.


tl;dr:


- As the workload gets denser (more subscriptions, timers, etc), that’s when the CBG Executor (1 thread) sees the most gains over the **SingleThreadedExecutor** and even the **EventsExecutor** .
- The CBG Executor in multithreaded mode *vastly* outperforms the old multithreaded Executor, but still suffers from the cost of context-switching inherent to a thread pool which makes it less performant than in single-threaded mode.


About the benchmarks:


- Ran on my x86 8-core Lenovo developer laptop
- I took one full benchmark suite run (~4hrs) for each executor available in Lyrical, including the **EventsCBGExecutor** with both 1 and 8 worker threads.
- For the rmw implementations, “ipc_on” indicates **intra-process** comms was used (where rclcpp uses shared ptr message transport instead of going through the rmw layer)
- QoS settings: rclcpp default (Reliable, Keep Last, Volatile) with Depth 10


### A Closer Look at the Numbers


[One topology in particular](https://github.com/skyegalaxy/ros2-benchmark-container/blob/rolling/benchmark/topologies/pub-sub/topology_50_topics_2000hz.json) - the single-process ROS system with 50 topics, each publishing somewhere between 63 and 2000hz - proved to be the perfect stress-test for highlighting the cost of that pesky` wait_mutex_` . ~60k msgs per second x 20 seconds = an expected 1.2 million 1MB payloads sent over the graph. We’ll focus on 3 metrics:


- Throughput - how many msg callbacks does each subscriber *actually* get to service in that 20 seconds?
- Mean end to end latency (computed as callback entry time - msg timestamp, via wall clock)
- % CPU


Let’s have a look at the single-threaded cases first. Note that these graphs use a logarithmic y axis.


So the throughput numbers look about like we’d expect - At this high of a frequency, the intraprocess setups are really the only ones in which we comfortably reach 1.2M or ~100% of expected messages serviced. Here we can see consistent signal across all the tested transport modes that the CBG executor in 1-thread mode eats the others’ lunch, including its experimental predecessor! In almost all cases, the results show just as many or more callbacks serviced over the time window than the other executors, at significantly lower latencies. The CPU gains become much more apparent when factoring out the middleware overhead.


Taking a look at the multi-threaded cases running with the max available cores on my laptop, we arrive at some pretty huge findings. The only setups which even get close to the throughput from the single threaded executors are the CBG Executor with multiple threads in intraprocess mode, hitting a bit over 75% throughput.


For all other cases, serializing and deserializing through the middleware as an extra hoop to jump through once again proves a pretty significant burden at these speeds, dropping the throughput to ~10% for the CBG executor and <2% for the multi-threaded executor. Notice how Cyclone fails to put up *any* latency numbers at all for the multi-threaded polling case - that’s because the combined overhead from the (8x) serialized entity walk and message serialization in cyclone resulted in 4 out of the 50 subscribers receiving **zero messages** in the 20 second time window, which meant the average measured subscriber latency came out to a NaN since you can’t compute a mean of 0 samples. I reran that case multiple times just to make sure it wasn’t just a fluke.


Let’s look closer at the CPU usage and what it tells us - the callback group executor is spending about 1.3 to 1.75x more CPU than the old polling-based multi-threaded one, but look at what it’s getting for that extra spend - in the intraprocess cases, up to **6x more throughput and 50x less latency** ! The **MultiThreadedExecutor’** s lower CPU numbers are actually a symptom of core starvation because the scheduling is too slow! The events system simply gives the new executor *way more opportunities* to actually use your CPU cores to service the callbacks themselves.


### Caveats


So we’re seeing some pretty impressive results indeed, which go even deeper than the headline claim of “10 to 15% less CPU”. The exact performance gains for your code will depend on how many entities are on the executor, in that nodes with more entities ought to see much bigger wins by getting rid of the linear polling discovery overhead. At Polymath, we make pretty extensive use of Nav2, rosbag2, etc - lots of nodes that service lots of input, so we’re very excited to swap out for this new executor and see just how much more performance we can squeeze from our systems!


Of course, the cost of the abstraction is all that’s changed here, so none of the efficiency gains that we talked about today will help much if your *callbacks* are the bottleneck, or reduce the inherent overhead of your messages going through the rmw layer. None of this is magic. Changing executors won’t shift the scales if the CPU burn happens inside your code or the transport layer. In fact, scenarios which overload the CPU and slow down your node can result in the events queue growing unbounded as it fails to keep up with the backlog.


It’s also worth calling out how the executors running on more threads end up with overall less throughput than the single-threaded cases here, which makes sense - a thread pool only speeds things up if the work needed for each event is greater than the cost of coordinating it. It’s rare that a ROS callback clears that bar, so the lack of thread context switching ends up being much better for raw throughput. When you need to reach for more than one thread (e.g. you’ve got long-running CPU-heavy robotics work like planning, machine vision, sensor processing inside a callback), think about how many you *actually* need instead of just reaching for the default of **std::hardware_concurrency** ! Your OS scheduler will thank you.


## Closing


The[Callback Group Events Executor](https://github.com/ros2/rclcpp/pull/3097) is available in rclcpp on Lyrical and Rolling today. At the time of writing, the client library working group is also actively considering backporting it to Jazzy mainline, but in the interim, the experimental EventsExecutor is still around if you don’t have a need for simulated time, and the[Cellumation hosted version](https://github.com/cellumation/cm_executors/tree/jazzy) works out-of-the-box with Jazzy if you’d like to try it out yourself! That’s certainly Polymath’s plan.


Update (6/09) - The new executor[has been backported to Jazzy](http://github.com/ros2/rclcpp/pull/3163) and will be available in rclcpp as part of[the june Jazzy sync](https://discourse.openrobotics.org/t/preparing-for-jazzy-sync-and-patch-release-2026-06-08/55225) !
