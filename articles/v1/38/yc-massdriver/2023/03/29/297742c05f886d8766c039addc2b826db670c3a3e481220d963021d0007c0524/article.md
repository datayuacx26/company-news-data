---
schema_version: "1.0.0"
document_id: "297742c05f886d8766c039addc2b826db670c3a3e481220d963021d0007c0524"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/dilating-github-actions-using-dialyzer"
published_at: "2023-03-17T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:c04fe7394139212030e256e6ebd2944fefaaef4854fc6b7ad4641a7bd106451f"
---

# Dilating GitHub Actions using Dialyzer

Elixir is a[strong dynamic language](https://thinkingelixir.com/elixir-in-the-type-system-quadrant/) : it checks types at run time, enabling some of its most powerful features like pattern matching and macros. The “strong” part tells us that type conversion needs to be explicit, unlike JavaScript, which aims to always do “something” with your code, and happily converts between types.


These properties make Elixir a flexible and productive language, but open the possibility for bugs such as passing the wrong type to a function during run time, crashing the process. The likelihood of such bugs increases as a project’s code base grows.


Many of these bugs can be caught by using static analysis - checking as many assumptions as possible before executing the code.[Dialyzer](https://www.erlang.org/doc/man/dialyzer) is a great tool for this, using Elixir and Erlang’s built in type system to catch type errors and unreachable code. A welcome side effect of using dialyzer is that it encourages extensive use of the[type system](https://hexdocs.pm/elixir/1.14/typespecs) available in Elixir, increasing readability and maintainability.


A robust Elixir or Erlang codebase can add Dialyzer in its CI pipeline to increase confidence in code changes and prevent regressions. At Massdriver we host our code on[GitHub](https://github.com/massdriver-cloud) and make extensive use of[GitHub Actions](https://github.com/features/actions) for CI. In this post I’ll show you how to set up Dialyzer in a GitHub Action while avoiding some common mistakes..


## Time is Money


The example GitHub Action on the dialyxir[README](https://github.com/jeremyjh/dialyxir) is a good starting point:


```text
# ...
steps:
-    uses:    actions/checkout@v2
-    name:    Set    up    Elixir
id:    beam
uses:    erlef/setup-beam@v1
with:
elixir-version:    '1.12.3'    # Define the elixir version
otp-version:    '24.1'    # Define the OTP version


# Don't cache PLTs based on mix.lock hash, as Dialyzer can incrementally update even old ones
# Cache key based on Elixir & Erlang version (also useful when running in matrix)
-    name:    Restore    PLT    cache
uses:    actions/cache@v2
id:    plt_cache
with:
key:    |
${{ runner.os }}-${{ steps.beam.outputs.elixir-version }}-${{ steps.beam.outputs.otp-version }}-plt
restore-keys:    |
path:    |
priv/plts


# Create PLTs if no cache was found
-    name:    Create    PLTs
if:    steps.plt_cache.outputs.cache-hit    !=    'true'
run:    mix    dialyzer    --plt


-    name:    Run    dialyzer
run:    mix    dialyzer    --format    github


```


‍


This is a good start, but depending on the size of your code base and dependencies, the “Create PLTs” step which builds the[Persistent Lookup Tables](https://github.com/jeremyjh/dialyxir#plt) - the static analysis output - can take a long time. For the main Massdriver application, this step can take over 10 minutes for a complete rebuild! But if the next step,` Run dialyzer` , fails, the cache is not saved, so the next run will have to rebuild the PLT from scratch - again!


## Fun with Caches


By default, the[GitHub Cache](https://github.com/actions/cache) action will only save the cache if all steps in the job succeed. But since` actions/cache@v3` we can separate the all-in-one action into` actions/cache/restore@v3` to restore the PLTs, build them if there was no cache hit, and then finally use` actions/cache/save@v3` to save the PLTs even if the` Run dialyzer` step fails. This way, if a commit that fails` mix dialyzer` is pushed (which happens to me all the time), the subsequent fix will complete CI much faster.


‍


```text


...
- name: Restore PLT cache
-       uses: actions/cache@v2
+       uses: actions/cache/restore@v3
id: plt_cache
with:
key: |
restore-keys: |
path: |
priv/plts


# Create PLTs if no cache was found
- name: Create PLTs
if: steps.plt_cache.outputs.cache-hit != 'true'
run: mix dialyzer --plt


+     - name: Save PLT cache
+       uses: actions/cache/save@v3
+       if: steps.plt_cache.outputs.cache-hit != 'true'
+       id: plt_cache_save
+       with:
+         key: |
+           ${{ runner.os }}-${{ steps.beam.outputs.elixir-version }}-${{ steps.beam.outputs.otp-version }}-plt
+         path: |
+           priv/plts


- name: Run dialyzer
run: mix dialyzer --format github


```


‍


We haven’t yet fixed our Dialyzer bug, but we saved the PLTs to the cache even though` Run dialyzer` failed!


Now let’s push a fix, and see how long it takes to run Dialyzer again:


A fast CI shortens feedback loops and enables developers to move faster. Every minute saved on CI is a minute saved every time a dev pushes a commit. Build time optimization is often overlooked, but can have an outsized impact on developer productivity.


‍
