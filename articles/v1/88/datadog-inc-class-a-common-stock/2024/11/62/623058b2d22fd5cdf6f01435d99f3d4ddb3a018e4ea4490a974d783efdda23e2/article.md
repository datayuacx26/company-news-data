---
schema_version: "1.0.0"
document_id: "623058b2d22fd5cdf6f01435d99f3d4ddb3a018e4ea4490a974d783efdda23e2"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/ruby-test-impact-analysis/"
published_at: "2024-11-01T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:50e45a1ab2bbdef637184ce5bd1d20e21139f16707e689eadfcbf37fefec6e1d"
---

# How we built a Ruby library that saves 50% in testing time

Andrey Marchenko


Gillian McGarvey


Team Lead, Technical Content Editor


Do you know that feeling when the coding is done and the pull request is approved, and you only need a green pipeline for the merge to be complete? Then the dreaded sequence of events occurs: running your test suite takes 20+ minutes and then fails because of some flaky test that has no connection to your changes. You have to run the tests again, wasting a lot of time and energy for nothing.


Lengthy, unstable CI pipelines are a common issue for many teams. As a software project grows, intermittent pipeline failures are almost inevitable because the chances of triggering them increase with time. The simplest and most common way to speed up a test suite is to run tests in parallel. This can greatly reduce testing time—at the cost of spending more on cloud computing resources—but it doesn’t solve the issues with flaky tests.


Another approach is to run tests selectively, which results in faster pipelines, fewer cloud resources used, and a smaller chance of unrelated test flakiness. After all, why run the entire test suite if your change only touches a small part of the application? The best known approach to this is[test impact analysis](https://martinfowler.com/articles/rise-test-impact-analysis.html) , which involves dynamically generating a map between each test in the codebase and the source code files that are executed during the test.


Here is an example mapping for a test in an imaginary Ruby on Rails application:


Example of a test impact analysis map for a single test. Example of a test impact analysis map for a single test.


The idea behind test impact analysis is that if we create a list of files that a test impacts, we can decide whether to run it by checking if there’s an overlap with the files changed in the latest git commit. If there is at least one common file, the test should be run; if not, it’s safe to be skipped.


Datadog built the[Intelligent Test Runner](https://www.datadoghq.com/blog/streamline-ci-testing-with-datadog-intelligent-test-runner/) product around this idea. An important component of Intelligent Test Runner is a library that collects impacted source code files dynamically during test runs. This library has the following requirements:


-


**Correctness** : Skipping a test that shouldn’t be skipped might lead to a broken test in the default branch.


-


**Performance** : Test impact might change with every code change, so the library needs to run on every commit in every feature branch. This is only feasible if the impact collector has low enough overhead so that the overall pipeline execution time isn’t affected too much.


-


**Seamlessness** : The library must be invisible to the end user, require no code changes from the user’s side, and must not affect the tests’ behavior.


This post tells the story of building a test impact analysis library for Ruby that cuts testing time in half, and the challenges and solutions we discovered along the way.


## Overview of existing solutions


A common approach to building test impact analysis tools is to use the existing code coverage tools provided by a programming language. The logic behind this is simple: if a test executes a line of code, then it must depend on the source file where that line of code is located.


All Ruby coverage libraries use[Ruby’s built-in Coverage module](https://rubyapi.org/3.3/o/coverage) under the hood, so it would be natural to rely on this module for per-test code coverage as well. The Coverage module was developed with the total code coverage use case in mind, but in Ruby 3.1,[resume/suspend methods](https://rubyapi.org/3.1/o/coverage#method-c-resume) were added to support per-test code coverage. To evaluate this approach,[we created a prototype](https://github.com/DataDog/datadog-ci-rb/pull/124) .


The prototype worked, but it had significant limitations:


-


This approach wasn’t compatible with total code coverage, which a user might be collecting for their project. This meant they would need to choose between using simplecov (for total code coverage) or collecting per-test coverage using our tool.


-


The measured performance overhead of this solution was as high as 300 percent, which meant the test suite was four times slower than without test impact analysis.


There is an alternative way to spy on code execution in Ruby:[TracePoint](https://rubyapi.org/2.7/o/tracepoint) . TracePoint provides an API to subscribe to a number of VM events, one of them being` line` , which fires when code is executed on a new line. This sounded like what we needed, so we used it for[our next prototype](https://github.com/DataDog/datadog-ci-rb/pull/128) .


This approach worked a lot better than using the Coverage module. It didn’t interfere with the total code coverage setup using simplecov, and it had an easy-to-use API that returned the exact data we needed. However, there was still a high performance overhead of about 200 percent (up to 400 percent in some worst cases), which is not something anyone wants for their test suite.


## Diving deeper into the Ruby VM: Writing our own coverage tool using interpreter events


At this point, we realized that we need to create our own solution to improve performance. We decided to dive deeper into the Ruby source code and see how things work under the hood. First, we looked at the` coverage.c` module, which is relatively easy to navigate because it is comparatively small. We found the[rb_coverage_resume C function](https://github.com/ruby/ruby/blob/bc85c8d8529b58c5c649f418ca549569ba348caa/ext/coverage/coverage.c#L140) , which calls the[rb_resume_coverages function](https://github.com/ruby/ruby/blob/bc85c8d8529b58c5c649f418ca549569ba348caa/thread.c#L5770) defined in` thread.c` , which uses` rb_add_event_hook2` to register callback for VM events. This looked promising.


The official[documentation on C extensions for Ruby](https://ruby-doc.org/core-2.7.0/doc/extension_rdoc.html#label-Hooks+for+the+Interpreter+Events) confirms that this API is available to the extensions’ authors and that it supports the` RUBY_EVENT_LINE` event, which is what we needed.


Here is a shortened version of our initial proof of concept (POC) for test impact analysis using interpreter events (some code has been removed for clarity; see full[original POC here](https://github.com/DataDog/datadog-ci-rb/pull/127) ):


```text
1   static   VALUE   dd_cov_start  (VALUE   self  )    2   {    3       // get current thread    4        VALUE thval =   rb_thread_current  ();    5       // add event hook for RUBY_EVENT_LINE for the current thread    6       rb_thread_add_event_hook  (thval, dd_cov_update_line_coverage, RUBY_EVENT_LINE, self);    7
8       return   self;    9   }    10
11   static   VALUE   dd_cov_stop  (VALUE   self  )    12   {    13       // get current thread    14        VALUE thval =   rb_thread_current  ();    15       // remove event hook for the current thread to stop coverage collection    16       rb_thread_remove_event_hook  (thval, dd_cov_update_line_coverage);    17
18       // get the collected coverage data (it is a Ruby hash)    19        VALUE cov =   dd_cov_data  ->  coverage  ;    20       // clear the coverage hash    21       dd_cov_data  ->  coverage   =   rb_hash_new  ();    22
23       return   cov;    24   }    25
26   // returns true if "pre" string is a prefix of "str"    27   static     bool     prefix  (  const     char   *  pre  ,   const     char   *  str  )    28   {    29       return     strncmp  (pre, str,   strlen  (pre)) ==   0  ;    30   }    31
32   static     void     dd_cov_update_line_coverage  (  rb_event_flag_t     event  , VALUE   data  , VALUE   self  , ID   id  , VALUE   klass  )    33   {    34       const     char   *filename =   rb_sourcefile  ();    35       if   (filename ==   0  )   // don't process file if Ruby source location is not found    36        {    37         return  ;    38        }    39       // check if file is located under project's root    40       if   (!  prefix  (  dd_cov_data  ->  root  , filename))    41        {    42         return  ;    43        }    44
45       // create a new Ruby string from C-string    46       unsigned     long   len_filename =   strlen  (filename);    47        VALUE rb_str_source_file =   rb_str_new  (filename, len_filename);    48       // store the file path in the coverage hash    49       rb_hash_aset  (  dd_cov_data  ->  coverage  , rb_str_source_file, Qtrue);    50   }
```


This solution turned out to be the best among the ones we tried. Its performance overhead was about 60–80 percent, it was compatible with simplecov, and it offered a lot of flexibility for future improvements. We released the[native extension](https://github.com/DataDog/datadog-ci-rb/pull/137) as the test impact analysis tool for Datadog’s library.


After releasing this product to a limited number of customers, we worked on optimizing it. A significant amount of overhead came from creating Ruby strings from C strings returned by the[rb_sourcefile](https://github.com/ruby/ruby/blob/66afde9ceaafe92f8727087332939bc94fadcbdf/vm.c#L1854) call. We needed to find another solution to access the file path in Ruby for executed files without the overhead that came with transforming strings from one representation to another. The solution we settled on used a different Ruby VM API call.


The function[rb_profile_frames](https://github.com/ruby/ruby/blob/66afde9ceaafe92f8727087332939bc94fadcbdf/vm_backtrace.c#L1752) returns the[frames](https://kddnewton.com/2022/12/03/advent-of-yarv-part-3.html) that are in the current execution stack. By using[rb_profile_frame_path](https://github.com/ruby/ruby/blob/260d4c7af8767dea5fd19c342282359feee4e6d9/vm_backtrace.c#L1799) , we could get the Ruby string with the path of the currently executed file. To make it more performant, we asked it to return only a single top frame:


```text
1   #define   PROFILE_FRAMES_BUFFER_SIZE   1    2   // .. ..    3
4   // in RUBY_EVENT_LINE handler    5   VALUE top_frame;    6   int   captured_frames_count =   rb_profile_frames  (    7         0   /* stack starting depth */  ,    8          PROFILE_FRAMES_BUFFER_SIZE,    9          &  top_frame  ,    10         NULL  );    11
12   if   (captured_frames_count != PROFILE_FRAMES_BUFFER_SIZE)    13   {    14       return  ;    15   }    16
17   VALUE filename =   rb_profile_frame_path  (top_frame);    18   if   (filename == Qnil)    19   {    20       return  ;    21   }
```


With this change, we saved some time by accessing the filename string directly without copying it. But we still needed to perform two expensive prefix checks to determine if a file was in the project directory but not under the bundle path (where a project’s dependencies are located). We don’t track source code files that are external to a project; they aren’t part of your git repository, so we don’t know if they changed in recent commits. It would be inefficient to repeat these checks on every` RUBY_EVENT_LINE` event. This event often fires for consecutive lines within the same file, which means we would waste CPU time performing the same calculation multiple times.


Initially, we considered caching the filename as a string, but string comparisons are even slower than prefix checks. To fix this, we used the` rb_sourcefile` call, which returns a memory address pointer to the` filename` string. Comparing pointers is much faster than comparing strings, because a pointer is just a number. By caching this` filename` pointer, we could skip the prefix checks if the pointer hadn’t changed. If the pointer had changed due to[GC compaction](https://jemma.dev/blog/gc-compaction) , we just performed the prefix checks again. This optimization saved another 5–10 percent in performance overhead:


```text
1      const     char   *c_filename =   rb_sourcefile  ();    2
3      // skip if we cover the same file again    4      uintptr_t   current_filename_ptr = (  uintptr_t  )c_filename;    5      if   (dd_cov_data->last_filename_ptr == current_filename_ptr)    6       {    7        return  ;    8       }    9       dd_cov_data->last_filename_ptr = current_filename_ptr;
```


## Ruby magic and “code-less classes”


A really interesting challenge came next: there are cases when line coverage isn’t enough to correctly collect test impact. Consider the following example:


```text
1   # this class doesn't have any executable lines    2   class     MyClass   <   OtherClass    3   end    4
5   # this test has zero impacted source files    6   test     "instantiate MyClass"     do    7        assert   MyClass  .  new   !=   nil    8   end
```


The only executable line of the` MyClass` class is the class definition itself, which is executed during the code loading stage.` MyClass` is essentially “code-less”—it inherits its behavior from` OtherClass` and doesn’t add anything by itself. When` MyClass.new` is called, we get no` RUBY_EVENT_LINE` events with the` my_class.rb` source file. So if someone adds a constructor to` MyClass` with mandatory arguments, the tests would start failing. We wouldn’t know this until the change was merged to the main branch (as no tests would be impacted by the change).


Because there are so many “code-less classes” in Rails, this line coverage limitation is significant:


```text
1   # perfectly valid Rails model that is not covered by any test    2   class     Account   <   ApplicationRecord    3        belongs_to   :user    4   end
```


The Active Record model above does not have any code that would trigger` RUBY_EVENT_LINE` in the` account.rb` file. Every bit of logic there is provided by the` activerecord` gem, with no test impact collected. It is impossible to imagine a working selective test runner that misses the impact of Active Record models on tests—so we had to fix this limitation.


## Using allocation profiling to improve correctness


Because line coverage didn’t work for “code-less classes,” we needed to find inspiration based on other internal tracepoints. We turned to Datadog’s[Continuous Profiler](https://www.datadoghq.com/blog/ruby-profiling-datadog-continuous-profiler/) , which has many examples of performant and safe spying on the Ruby VM.


For example, consider the[allocation counting](https://github.com/DataDog/dd-trace-rb/pull/2635) feature. It uses the` RUBY_INTERNAL_EVENT_NEWOBJ` event to count how many objects were allocated on the heap. What if we used the same approach, but instead of counting the objects, we tracked which Ruby classes the test instantiated? Then the test impact would include two sets of source files. First, it would include the files that were executed during the test run. Second, it would include the files in which the classes of instantiated objects were defined. This approach also promised to be performant enough, because there are usually fewer objects instantiated than lines of code or methods called.


A prerequisite for this approach was the ability to reliably determine the source file in which any given class is defined. Ruby has a[Module.const_source_location method](https://ruby-doc.org/core-2.7.1/Module.html#method-i-const_source_location) that works for classes (in Ruby, class is a constant). However, it was not as reliable as we wanted it to be—it has a lot of “gotchas” for classes defined in C and anonymous classes, and we ran into a few crashes until we correctly handled all of the return values for it. But with careful use, this API provided exactly what we wanted: the source files for classes defined in the project under test.


Allocation tracing presented another important limitation—it is forbidden to call a Ruby API in the internal tracepoints. Since instantiating Ruby objects in the internal tracepoints could lead to a VM crash, we had to be careful and do only a minimal amount of work in the tracepoint, postponing most of the work to a later point in time. Also, we could not use Ruby’s hash, so we used the C implementation of hash from the[st.c module](https://github.com/ruby/ruby/blob/0923a98868d6c2bed00eb1d8fa2ddf98fb42a708/st.c) directly.


Take a look at the[final PR](https://github.com/DataDog/datadog-ci-rb/pull/197) to see what this small domain-specific heap allocation profiler looks like.


## Limitations and workarounds


Our test impact analysis tool is highly optimized for performance and tailored to the specific use case of enabling selective test runners. However, there are some known limitations which we may address in the future:


-


The test impact analysis is contained within a single Ruby process; it doesn’t instrument anything outside of that. This means that if the test makes calls to external services or runs external processes, they will not be included in the test impact analysis.


-


The same is true for external data files—because these are not executable code, they are not tracked as impacted.


To work around these limitations, our Intelligent Test Runner solution allows users to:


-


Define a set of tracked files and run all tests when these files are changed (fixtures and DB migrations are obvious candidates for this).


-


Mark specific tests as[unskippable](https://docs.datadoghq.com/intelligent_test_runner/setup/ruby/?tab=onpremisesciproviderdatadogagent#marking-tests-as-unskippable) if they have external dependencies. This is done in a framework-specific way and is supported for all the test frameworks that we instrument (RSpec, Minitest, and Cucumber).


## Key takeaways


This test impact analysis library powers the Intelligent Test Runner product that is being used by a number of Datadog customers. The tool has a median performance overhead of 25 percent (up to 70 percent for a worst-case scenario), which results in about **5–10 times better performance than the existing solutions we tried** . The tool is performant enough to run on every commit without significantly slowing down the test suite. By collecting test impact data on every test run, we make sure that the test impact map is always up to date. This allows us to skip the correct set of tests every time without any heuristics, randomization, or guessing—effectively creating a fully deterministic tool.


By continuously improving the performance and correctness of this tool, we have created a robust test impact analysis solution that can be the basis for a number of exciting tools beyond a selective test runner. For example, having an always up-to-date test impact map opens up possibilities to perform automated root cause analysis for failing builds or flaky tests.


The best way to try it is to use Datadog’s[Intelligent Test Runner](https://docs.datadoghq.com/intelligent_test_runner/) , which automatically collects test impact data and uses it to skip irrelevant tests in your CI pipeline. As with all of our observability libraries, Intelligent Test Runner is open source, so you can see how we’re threading the needle to build this feature with super-low overhead. See the[full source code in the datadog_cov.c native extension](https://github.com/DataDog/datadog-ci-rb/blob/c842a90dfd8286699a96c77e21cc47732c0fa08d/ext/datadog_cov/datadog_cov.c) and the example usage in the[test optimization component of the datadog-ci gem](https://github.com/DataDog/datadog-ci-rb/blob/c842a90dfd8286699a96c77e21cc47732c0fa08d/lib/datadog/ci/test_optimisation/component.rb#L197-L205) .


## What’s next?


Next, we plan to expand the tool to provide the correct line coverage data per test for every impacted file with executable lines. We will continue to collect feedback about our test impact analysis solution and iterate upon it.


Interested in working with teams who are building innovative tools that save developers time and let them focus on what matters? Datadog is[hiring](https://careers.datadoghq.com/all-jobs/?parent_department_Engineering%5B0%5D=Engineering) !


-
-
-
