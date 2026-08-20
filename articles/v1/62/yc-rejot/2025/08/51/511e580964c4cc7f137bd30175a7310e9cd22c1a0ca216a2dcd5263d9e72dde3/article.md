---
schema_version: "1.0.0"
document_id: "511e580964c4cc7f137bd30175a7310e9cd22c1a0ca216a2dcd5263d9e72dde3"
company_key: "yc-rejot"
company: "ReJot"
source_id: "yc-rejot-news-import-01598ccac029"
canonical_url: "https://rejot.dev/blog/call-of-duty-scripting/"
published_at: "2025-08-21T00:00:00+00:00"
first_seen_at: "2026-07-31T19:47:01.544738+00:00"
fetched_at: "2026-07-31T19:47:01.910446+00:00"
content_hash: "sha256:6bf34b6a67fcf49ab5af46fc852e4b826484a8bb0d5267967fefa51b04a0e9ec"
---

# Concurrency in Call of Duty's VM, Compared to JavaScript

It has been more than 18 years since the mod tools for *Call of Duty 4: Modern Warfare* were released. These tools enabled the community to build custom maps and game modes. I’ve never had the patience for mapping, but it did get me into programming.


The scripting language at the heart of the *Call of Duty* franchise is known as Game Script, or GSC. This is a custom, domain-specific language that runs atop a virtual machine architecture directly inspired by the QVM (Quake Virtual Machine)1 . Like the QVM, GSC’s runtime provides a portable execution environment that enables flexibility for Call of Duty developers and modders.


At the time, I didn’t fully appreciate the design choices made in Call of Duty’s scripting language, but looking back now, its execution model is quite fascinating. This blog post provides an overview of the cooperative multitasking system used in GSC and compares it to JavaScript’s event loop.


# Syntax and Core Constructs


Before we dive into the execution model, let’s take a look at the syntax and core constructs of the GSC scripting language2 . GSC’s syntax is C-like, making it immediately approachable for most programmers. The control flow structures are mostly the same as C. Infinite loops like` for(;;)` are commonly used to wait for events, which we’ll get into later.


```text
testMenu  () {
self   endon  (  "death"  );
self   endon  (  "disconnect"  );


for  (;;) {
wait  (  10.0  );


notifyData   =   spawnStruct  ();
notifyData.titleText   =   &  "MP_CHALLENGE_COMPLETED"  ;
notifyData.notifyText   =   "wheee"  ;
notifyData.sound   =   "mp_challenge_complete"  ;


self thread maps\mp\gametypes\  _hud_message::notifyMessage  (notifyData);
}
}
```


*A typical GSC function: test code found in official CoD4 code3 .*


## Type System


Unlike C, GSC is a dynamically and loosely typed language, a design choice that aligns it more closely with other scripting languages. Variables are not formally declared with a type; their type is inferred at runtime based on the value they are assigned. GSC supports a standard set of primitive data types: integers, floats, strings, and booleans. Vectors, arrays, and structs are also included. There’s also a special` undefined` value.


Arguably the most interesting type is the entity type. This is a reference to any object within the game world managed by the C++ engine, such as a player, a weapon, an AI character, or a trigger volume. All interaction with the game world is mediated through these entity references.


## Contextual Entity-Based Scoping


To manage state and context within its object-oriented world, GSC relies on two essential keywords:` self` and` level` .


- ` self` : This is a dynamic, contextual keyword that refers to the specific entity on which a function or thread is currently operating.
- ` level` : This keyword acts as a global singleton object, providing a shared namespace that is accessible from any script at any time. It’s the primary mechanism for managing global game state.


Any entity can be used to provide context to a function call. When a script executes a command like` player giveWeapon("ak47_mp");` , the player entity is given the weapon. In this case, the` giveWeapon` function is a “system call”4 . If this were a user-defined function, the` self` context in that function would become the player.


# Concurrency Model


A concise description of GSC’s concurrency model could be **frame-based cooperative multitasking system with coroutines** . This model is built around a scheduler that keeps track of active and paused threads. When a thread cooperatively yields, the scheduler will move on to the next thread. Because of the cyclical nature of game simulation, all threads must finish executing or yield within the frame budget. Even though execution is concurrent, it’s not parallel. Only one thread can run at a time. By default, the server runs at 20 frames per second (ticks), meaning each frame has a 50-millisecond budget for script execution5 .


## The` thread` Keyword


Function calls are blocking by default, but can also be spawned in a new, lightweight execution context using the` thread` keyword, which creates a coroutine. This is the fundamental mechanism for achieving concurrency in GSC, allowing processes such as an AI’s decision-making loop or a script that monitors a player’s status to run concurrently.


## The` wait` Keyword


Because GSC’s concurrency is cooperative, not preemptive, each “thread” is responsible for voluntarily yielding control back to the central script scheduler. If a thread fails to do so, it will monopolize the execution time for a given frame, leading to a freeze and eventually an error. The primary mechanism for yielding is the` wait` command. This command pauses the execution of the current thread for a specified duration in seconds. For example,` wait 0.05;` will pause for one server frame. This allows the scheduler to move on to execute other active threads.


```text
checkEveryTick  () {
for  (;;) {
wait   0.05  ;
doTheCheck  ();
}
}
```


*A thread that checks something every tick.*


## The Event System:` notify` ,` waittill` , and` endon`


GSC’s most powerful feature is its event-driven communication system, which allows threads to yield until a specific game event occurs. This system is built around three core functions:` notify` ,` waittill` , and` endon` .


In this pattern, game entities (player, level, triggers) can broadcast named events about changes in their state without needing to know who, if anyone, is listening. This is done using the non-blocking` notify` function. For example, a script handling player damage might end with` self notify("damage");` .


Threads can subscribe to these events on a specific entity using the` waittill` function. A call to` self waittill("damage");` will pause the thread indefinitely, yielding control back to the scheduler. The thread will remain dormant until the entity referenced by` self` broadcasts a “damage” notification. At that point, the scheduler will wake the thread, which will then resume execution from the line immediately following the` waittill` . This provides an elegant way to create reactive game logic that responds to specific triggers rather than constantly checking (polling) for state changes in a loop.


```text
// A basic script that waits for a player to connect, then spawns
init  () {
level thread   onPlayerConnect  ();
}


onPlayerConnect  () {
level   endon  (  "game_ended"  );
for   (;;) {
level   waittill  (  "connected"  , player);
player thread   onPlayerSpawn  ();
}
}


onPlayerSpawn  () {
self   endon  (  "disconnect"  );
for  (;;) {
self   waittill  (  "spawned_player"  );
self   takeAllWeapons  ();
self   giveWeapon  (  "rpg_mp"  );
}
}
```


*A simple implementation of an “RPG-only” game mode.*


In the code above, we can also see the` endon` keyword. This is used to register a terminal condition. It tells the scheduler to immediately and silently terminate this thread if the “disconnect” event is ever notified on the self entity. This is used for cleanup and resource management.


# Trigger Example


This style of programming allows for easily setting up logic in the game world. In this example, the trigger` my_bounce_pad` would be placed in the map by the level designer. The script will bounce the player up in the air when they enter the trigger.


```text
main  () {
trigger   =   getEnt  (  "my_bounce_pad"  ,   "targetname"  );
while  (  true  ) {
// A `player` reference is obtained from the trigger event.
trigger   waittill  (  "trigger"  , player);
player thread   bouncePlayerUpInAir  ();
}
}
```


*An implementation of a bounce pad that can be placed by the level designer.*


# Comparing to JavaScript’s Event Loop


A slightly more well-known programming language that uses a single-threaded cooperative concurrency model is JavaScript. Different design choices are made in GSC that are tailored to the needs of a game engine. There are still a number of similarities between the two.


-


**Scheduler:** In JavaScript, the event loop processes tasks (macrotasks) from a task queue; after each task completes it runs microtasks (for example,` Promise.then` callbacks) before rendering. Tasks are scheduled via APIs like timers (` setTimeout` ), I/O, and event dispatch. In GSC, scheduling work is explicit: the` thread` keyword starts a new coroutine, and the VM cooperatively schedules active (and waiting) threads across frames. In both systems, work runs until completion or an explicit yield, which makes the execution model cooperative.


-


**Yielding control:** In JavaScript, the` await` keyword is used to yield control to the event loop. In GSC, threads can yield control by using the` wait` or` waittill` keywords. In both languages, the main thread can be hogged by executing CPU-intensive code, such as calling a hash function in a loop. In Call of Duty’s VM the frame would be actively blocked and the server could not continue processing. In JavaScript, the rendering thread could be blocked and the page would appear frozen.6


-


**Events:** GSC has entity-scoped events via` notify` and` waittill` , which suspend execution until a specific event for a specific entity. In JavaScript, events are dispatched as tasks; all registered listeners run synchronously during that task. Promise callbacks are microtasks that run after the current task finishes.


-


**Cancellation:** GSC’s` endon("event")` auto-terminates a thread when a terminal event fires. In JavaScript, you typically pass an` AbortSignal` , call` removeEventListener` , or resolve/reject a Promise to stop work.


In general, I’d argue constructs in GSC are more explicit than in JavaScript. Its units of work are explicitly started using the` thread` keyword, while tasks in JavaScript are implicitly queued. Events are first-class citizens in both languages but are handled differently. Because of the way entities work in GSC, they are a good fit as a communication channel between threads7 . Code written in GSC can look more sequential than JavaScript. In JavaScript, the heavy use of callbacks makes the control flow harder to reason about since it jumps around a lot. In GSC, the control flow is more linear and arguably easier to reason about.


The combination of frame-based execution, cooperative coroutines, and an entity-based event system creates a runtime environment that is well-suited for game scripting. Deep-diving into this scripting language so many years later has definitely given me an appreciation for this design, and it makes me wonder how JavaScript APIs would have been designed if the scheduler (event loop) were different.


Thanks for reading! If you enjoyed this post, you can find me on Twitter/X[@WilcoKr](https://x.com/WilcoKr) .


## Footnotes


1.


For a detailed technical overview of the Quake III Virtual Machine architecture, see[Fabien Sanglard’s Quake 3 Source Code Review: Virtual Machine](https://fabiensanglard.net/quake3/qvm.php) .↩


2.


For a detailed technical overview of the GSC scripting language, see[CoD Script Handbook](https://wiki.zeroy.com/index.php?title=Call_of_Duty_4:_CoD_Script_Handbook) .↩


3.


The full` testMenu` function is taken from the official CoD4 codebase and can be found[here](https://github.com/promod/CoD4-Mod-Tools/blob/master/raw/maps/mp/gametypes/_globallogic.gsc#L781) .↩


4.


All engine methods can be found in[this reference](https://scripts.zeroy.com/cod4_script/index.html) .↩


5.


The` sv_fps` console variable can be used to change the tick rate, but the scripts included in the game all use` wait 0.05;` , so they will wait more frames if` sv_fps` is higher.↩


6.


Browsers mitigate this problem by showing a *“a script is taking too long to run”* dialog. The VM in Call of Duty will abort a thread after a set amount of time with a *“script runtime warning: potential infinite loop in script”* message. So there actually is some preemption happening.↩


7.


If you squint your eyes, this can even be seen as similar to Go’s channels.↩
