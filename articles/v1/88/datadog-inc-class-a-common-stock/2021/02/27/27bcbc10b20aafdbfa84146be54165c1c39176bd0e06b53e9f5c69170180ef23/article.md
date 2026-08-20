---
schema_version: "1.0.0"
document_id: "27bcbc10b20aafdbfa84146be54165c1c39176bd0e06b53e9f5c69170180ef23"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/php-8-observability-baked-right-in/"
published_at: "2021-02-02T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:5e91ff319b7bacbac427c2595b0f7a6db329a1003d7c44e7a79470721085558d"
---

# PHP 8: Observability baked right in

Sammy Powers


Levi Morrison


Over the past two decades, the Zend Engine, which powers PHP, has evolved to accommodate specific improvements. A significant jump in performance came with the release of PHP 7, which greatly improved the performance of traditional web apps like WordPress.[PHP 8](https://www.php.net/releases/8.0/en.php) followed suit by introducing a just-in-time compiler (JIT) to significantly improve computationally-heavy algorithms.


However, the primary observability hook that tracers, profilers, and debuggers used to observe PHP’s function-call behavior has not evolved alongside the advancements made to the Zend Engine and, as a result, this aging hook has had an increasingly adverse effect on observed PHP application runtimes. For example, the[Datadog PHP tracer](https://github.com/DataDog/dd-trace-php) couldn’t evolve in the era of PHP 8 without changes to the observability hook first.


The release of PHP 8 includes changes that bring modern observability to the PHP runtime. Our team, along with help from the PHP internals community, developed and shipped the new[observer API](https://github.com/php/php-src/blob/PHP-8.0/Zend/zend_observer.h) . There were many challenges that we and other PHP developers faced prior to the release of this API.


To better understand the impact and constraints that observability hooks can have on development, let’s take a look at the limitations of observability before PHP 8.


## The observability landscape before PHP 8


### VM execute hook


The PHP virtual machine (VM) is responsible for handling the PHP runtime. There is a function pointer that extensions can override within the VM called` zend_execute_ex` . If an extension uses this hook, all calls to functions or methods that are defined in PHP run through this hook.


Although this was one of the most popular function-call-interception hooks prior to PHP 8, it had a number of drawbacks:


-


PHP has a virtually unlimited call stack. However, If an extension hooks` zend_execute_ex` , all PHP function calls are placed on the native C stack, which is limited by the value set in` ulimit -s` . This can lead to a stack overflow and crash of the PHP process.


-


When an extension hooks` zend_execute_ex` , every single userland (PHP) function call is intercepted during runtime, not just the ones that an extension would like to observe. This adds extra overhead that is most noticeable when a PHP script makes many function calls.


-


The compiler emits optimized function call opcodes that distinguish userland (PHP) function calls (` DO_UCALL` ) from internal function calls (` DO_ICALL` ), but when an extension hooks` zend_execute_ex` the compiler is unable to do this.


-


The hook requires extensions to forward the hook along to neighboring extensions that also might want to use it. This can cause “noisy neighbor” issues that result in unexpected behavior or even crashes if the hook is not forwarded along properly.


-


The hook is not compatible with the new[JIT](https://github.com/php/php-src/commit/d5a82e2c4eebe8337ff7790db46d812af2fbcec9#diff-9faa6c4e1812bb88ee722439bfa2d0b48c8034871c2a625e00d14e6dbfbb835cR4180-R4182) added in PHP 8.


Given the negative side effects of hooking` zend_execute_ex` , extension maintainers had to look to other engine hooks to achieve observability.


### Custom opcode handlers


PHP extensions have the option of providing custom handlers for an existing opcode. Providing custom handlers for the function-call related opcodes enables observability without hitting the “stack bomb” that comes with the` zend_execute_ex` hook.


However, custom opcode handlers, just like VM execute hook, have several drawbacks:


-


Custom opcode handlers must call neighboring extensions’ opcode handlers for the same hook. Perhaps it was due to a lack of documentation, or rarely encountering two extensions that hook the same opcode, but historically many extensions did not forward the opcode handlers to neighboring extensions.


-


Custom opcode handlers are able to mutate the VM state at runtime. This allows extensions to, for example, tell the VM not to invoke the original opcode handler. (The[Xdebug scream feature](https://xdebug.org/docs/all_settings#scream) is implemented this way.) If an extension needs to skip an opcode handler, it is not possible to reliably forward the hook along to a neighboring extension. This creates an impossible situation when two extensions provide a custom opcode handler for the same opcode and one of the extensions mutates the VM state.


-


Due to the way generators are implemented, full instrumentation of generators using custom opcode handlers is not possible.


-


The hook is not compatible with the[JIT](https://github.com/php/php-src/commit/d5a82e2c4eebe8337ff7790db46d812af2fbcec9#diff-9faa6c4e1812bb88ee722439bfa2d0b48c8034871c2a625e00d14e6dbfbb835cR4180-R4182) in PHP 8.


Due to the complexity of instrumenting with custom opcode handlers, some profiling extensions opted for the Zend Extension approach.


### Zend Extension hooks


PHP extensions can register as a special kind of extension called a[Zend Extension](https://www.phpinternalsbook.com/php7/extensions_design/zend_extensions.html) . This special extension has access to parts of the engine that regular extensions do not have, including begin and end handlers for function calls.


The main drawback to this method is that it causes the compiler to generate two additional opcodes for every function call: one opcode for the begin handler (` EXT_FCALL_BEGIN` ) and one for the end handler (` EXT_FCALL_END` ). These extra opcodes have a high performance overhead and are not suitable for a production-level tracer.


### Initial resolutions


Due to the drawbacks with the above engine hooks, some engineers sought other ways to intercept function calls. There was a[proof of concept tracer](https://github.com/beberlei/php-ast-tracer-poc) that injects nodes into the abstract syntax tree (AST) during compilation. These nodes call out to observability functions. Our team also wrote a[proof of concept tracer](https://github.com/morrisonlevi/ddastrace) based on the same AST injection idea. However, to date there are no known production ready tracers using this approach, and it’s most likely because injecting an observability hook before and after every function call has a performance overhead similar to a Zend Extension.


With sub-optimal engine hooks that were negatively impacting PHP’s performance, behavior, and stability, and the new JIT on the horizon, observability hooks were at a serious risk of ceasing to work altogether in PHP 8. This uncertainty is what led us to reach out to the PHP community with some[initial thoughts on tracing hooks for PHP in October 2019](https://github.com/SammyK/php-8-tracing-hooks/pull/1/files) . In turn, it led to the discussion of baking telemetry hooks directly into the Zend Engine, a potential vast improvement from the existing, fragile observability options. And this is how we came to create the new observer API in PHP 8.


## The observer API in PHP 8


From the beginning it was important that the observability changes didn’t just improve Datadog’s own PHP tracer; the changes needed to improve the PHP extension ecosystem across a wide variety of tracers, profilers, and debuggers.


After reaching out to the PHP internals community for feedback, a number passionate members responded to the observability initiative.[Benjamin Eberlei](https://github.com/beberlei) ,[Nikita Popov](https://www.google.com/url?q=https://github.com/nikic&sa=D&ust=1611592063011000&usg=AOvVaw1Jx3UyL00-zMVwT5RwY_4J) , and[Dmitry Stogov](https://github.com/dstogov) played a vital role in helping our team bring the observer API to fruition and also provided many code reviews along the way.[Joe Watkins](https://github.com/krakjoe) ,[Bob Weinand](https://github.com/bwoebi) , and several other PHP internals community members also weighed in and provided valuable feedback and advice.


The[observer API](https://github.com/php/php-src/blob/PHP-8.0/Zend/zend_observer.h) is designed to combat all of the negative side effects of function-call-interception hooks mentioned previously. One by one, we tackled the list of problems.


### No more stack limitations


With the observer API, extensions are able to instrument a virtually unlimited call stack. During the process startup phase (also known as module initialization, or MINIT), an extension can register as a function-call observer with` zend_observer_fcall_register` . This tells the Zend Engine to use specialized observer opcode handlers for the function-call related opcodes, and entirely removes the artificial stack limitation that occurred when overriding` zend_execute_ex` .


Better performance for unobserved requests If an observer extension is present, the compiler can emit observer-specific opcodes such as` OBSERVE_DO_UCALL` and` OBSERVE_DO_ICALL` . The problem with this approach is that it adds additional complexity to the compiler and adds a lot of new opcodes (eight to be exact) that debugging extensions would have to account for.


An alternative to the many-opcodes approach is opcode specialization. With opcode specialization, a single opcode can have multiple handlers associated with it. The compiler determines which handler should run for a specialized opcode.


One example of opcode specialization is` SPEC(RETVAL)` which creates two handler variations for an opcode; one handler will run when the return value is used and the other handler will run when the return value is not used. The example below illustrates this concept in action:


```text
1   <?php    2   $a   =   return_something  ();    3   return_something  ();
```


The compiler will emit a` DO_UCALL` opcode for each of these function calls. But since` DO_UCALL` has` SPEC(RETVAL)` opcode specialization, there are two separate handlers that are actually fired at runtime.


The observer API introduces a` SPEC(OBSERVER)` opcode specialization to the function-call related opcodes. When an observer extension is present, all of the function-call related opcodes will fire custom observer handlers (e.g.,` ZEND_RETURN_SPEC_OBSERVER_HANDLER` for the RETURN opcode). Since the handlers are assigned at compile time, this technique avoids negatively impacting the performance of unobserved requests.


### Target functions and methods


The observer API can also exclusively target functions and methods that are defined in PHP, known as userland functions. This new design allows extensions to target specific functions of interest in contrast to every extension being notified for every function call.


The diagram below illustrates how an observer extension observes the` foo()` function in the following PHP script:


example.php


```text
1   <?php    2
3   function     foo  () {}    4   function     bar  () {}    5
6   for   (  $x   =   0  ;   $x   <   2  ;   $x  ++) {    7         foo  ();    8         bar  ();    9   }
```


The observer API does not observe internal functions (functions provided as part of the standard library or from an extension). In general, only a subset of internal functions are of interest to extensions. Internal functions that handle I/O operations are often interesting to tracers and there are already mechanisms in place for an extension to instrument these functions by providing custom internal function handlers.


### Observe alongside the new just-in-time compiler (JIT)


The new just-in-time (JIT) compiler that was added in PHP 8 makes it tricky for tracers, profilers, and debuggers to intercept function calls without any negative side effects. The[JIT RFC](https://wiki.php.net/rfc/jit#to_existing_extensions) specifically calls out the need for a tracing API that would work with the JIT:


*“Run-time profiling should work even with JIT-ed code, but this might require development of additional tracing API and corresponding JIT extension, to generate tracing callbacks.”*


Thanks to the support from the JIT author and long-time PHP internals contributor Dmitry Stogov, the observer API is able to fulfill this requirement and provide observability even when the[JIT in PHP 8 is enabled](https://github.com/php/php-src/commit/d5d31ea3b32a75697bb2c2ee95acedd29ef69708) .


### Noisy neighbor resistant


All of the hooks management is now handled by the engine so that observer extensions do not need to concern themselves with forwarding along the hooks to neighboring extensions. This allows multiple tracers, profilers and debuggers to run alongside one another without the historical noisy-neighbor related crashes that were inherent of the old hooks.


## Conclusion


Historically the various function-call-interception hooks within the Zend Engine have been at odds with the engine’s evolution resulting in negative side effects to observability. The observer API in PHP 8 not only mitigates the primary negative side effects to function call interception, it introduces a more generic concept to the engine: observer.


The future of observability in PHP now has a single point of reference under the` ZEND_OBSERVER` name. The API has already expanded to include[error tracking](https://github.com/php/php-src/pull/6131) , and there are many other parts of the engine that could be opened for observability such as observing various stages of the compiler.


If you would like to get involved with furthering the advancement of observability in PHP, share your ideas with the[PHP internals mailing list](https://www.php.net/mailing-lists.php) . We would also like to invite you to contribute to the open-source[Datadog PHP tracer](https://github.com/DataDog/dd-trace-php) ; it has been using the observer API since version 0.52.0. And if nerding out on PHP extensions and opcodes is your thing, maybe apply for a career working with the APM integrations team;[Datadog is hiring](https://careers.datadoghq.com/engineering/) !


-
-
-
