---
schema_version: "1.0.0"
document_id: "645136ba01bafc344a82c44448e0ca9f026869ea53b1773a6e7e05160726326e"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/concurrent-tests"
published_at: "2024-04-18T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:7717fbdcd7c6c8d9eba1a7dc7eceef6f18dbf8c1c061cd08cef4ee94f4f709b6"
---

# Concurrent tests in GitHub Actions

Running builds and tests are probably the most common use-cases for GitHub Actions. Modern test frameworks and build systems have built-in support for sharding tests and remote execution. Sharding is essentially running your tests or builds in parallel, not only on different threads or cores but distributing them across different machines altogether.


## Concurrent tests for popular test frameworks


This guide details how to set up concurrent tests for some of the popular test frameworks. We will explore how to set up test sharding in GitHub Actions for Jest, Playwright and Pytest.


### Jest


Jest is a popular JavaScript testing framework. It provides[native support for test sharding](https://jestjs.io/docs/cli#--shard) using the` --shard` option to run your tests in parallel simultaneously across multiple machines. The option takes an argument in the form of` shardIdx/shardCount` , where` shardIdx` is a number representing index of the shard and` shardCount` is the total number of shards.


A simple benchmark on[a dummy test suite run](https://github.com/WarpBuilds/concurrent-tests/actions/runs/8738287628) showed that sharding improves the run time from 3 minutes to 30 seconds.


Here's how you can set it up in GitHub Actions:


```text
name  :   CI
on  :   push


jobs  :
test  :
name  :   Tests
runs-on  :   warp-ubuntu-latest-x64-4x


strategy  :
fail-fast  :   false
matrix  :
shardCount  : [  10  ]
shardIdx  : [  1  ,   2  ,   3  ,   4  ,   5  ,   6  ,   7  ,   8  ,   9  ,   10  ]


steps  :
-   uses  :   actions/checkout@v4
-   uses  :   actions/setup-node@v4
-   run  :   npm ci


-   name  :   Run Jest tests
run  :   npx jest --shard=${{ matrix.shardIdx }}/${{ matrix.shardCount }}
```


Tip


Jest parallelizes test runs across workers to maximize performance. You could optimize the performance of each shard by using the[--maxWorkers](https://jestjs.io/docs/cli#--maxworkersnumstring) option to specify the number of workers to use.


### Playwright


Playwright is a popular end-to-end automation and testing framework for web applications.[Native support for test sharding](https://playwright.dev/docs/test-sharding) is available using the` --shard` option. Just like we saw with Jest earlier, the` --shard` option takes an argument in the form of` shardIdx/shardCount` , where` shardIdx` is the index of the shard and` shardCount` is the total number of shards.


Sharding the tests improve the time from around 5 minutes 26 seconds to 1 minute 25 seconds for a[dummy test suite run](https://github.com/WarpBuilds/concurrent-tests/actions/runs/8740435954) .


The setup is very similar to that of Jest:


```text
name  :   CI
on  :   push


jobs  :
tests  :
name  :   Tests
runs-on  :   warp-ubuntu-latest-x64-4x


strategy  :
fail-fast  :   false
matrix  :
shardCount  : [  10  ]
shardIndex  : [  1  ,   2  ,   3  ,   4  ,   5  ,   6  ,   7  ,   8  ,   9  ,   10  ]


steps  :
-   uses  :   actions/checkout@v4
-   uses  :   actions/setup-node@v4
-   run  :   npm ci


-   name  :   Install Playwright browsers
run  :   npx playwright install --with-deps


-   name  :   Run Playwright tests
run  :   npx playwright test --shard=${{ matrix.shardIndex }}/${{ matrix.shardCount }}
```


Tip


1. Playwright[runs your tests in parallel](https://playwright.dev/docs/test-parallel) by default using worker processes. To further optimize the tests in each shard, you can use the` --workers` option to specify the number of workers to use.
2. Consider using[container-based jobs](https://playwright.dev/docs/ci-intro#via-containers) to potentially speed up tests by reducing the overhead of browser installation for each shard.


### Pytest


Pytest is a widely used testing framework for Python. Although Pytest doesn't natively support test sharding across machines, the third-party plugin[pytest-split](https://github.com/jerry-git/pytest-split) can distribute tests based on duration or name.


The performance gain after sharding was really significant since Pytest doesn't run tests in parallel by default. The[dummy test runs](https://github.com/WarpBuilds/concurrent-tests/actions/runs/8738290262) showed a 10x improvement in test run times - from 500 to 50 seconds.


Setting up the workflow involves installing the` pytest-split` package and running pytest with the` --splits` and` --group` options:


```text
name  :   CI
on  :   push


jobs  :
test  :
name  :   Tests
runs-on  :   warp-ubuntu-latest-x64-4x


strategy  :
fail-fast  :   false
matrix  :
splitCount  : [  10  ]
group  : [  1  ,   2  ,   3  ,   4  ,   5  ,   6  ,   7  ,   8  ,   9  ,   10  ]


steps  :
-   uses  :   actions/checkout@v4
-   uses  :   actions/setup-python@v5
-   run  :   pip install pytest pytest-split


-   name  :   Run pytest
run  :   pytest --splits ${{ matrix.splitCount }} --group ${{ matrix.group }}
```


Tip


You can also optimize the tests to run faster by parallelizing the tests within each shard by using the[pytest-xdist](https://github.com/pytest-dev/pytest-xdist) plugin.


## Comparison and Notes


All of the test suites used, workflows and their runs can be found in our[concurrent-tests](https://github.com/WarpBuilds/concurrent-tests) repository. Here is a comparison the test run performance before and after sharding for the three test frameworks:


Test Framework Default Sharded x10 Improvement Workflow Run


Jest 3m 30s 6x[Link](https://github.com/WarpBuilds/concurrent-tests/actions/runs/8738287628)


Playwright 5m 26s 1m 25s 3.8x[Link](https://github.com/WarpBuilds/concurrent-tests/actions/runs/8740435954)


Pytest 8m 20s 50s 10x[Link](https://github.com/WarpBuilds/concurrent-tests/actions/runs/8738290262)


Note


An important thing to keep in mind before sharding your tests with GitHub Actions is - **all** the steps inside the job are run again **for each shard** . This includes dependency installs, fetching third party actions, etc. For example, as stated earlier in the Playwright setup, we need to install the browsers for each shard.


Steps like these could add significant overhead and the performance gain might not be worth it or even inexistent. In worst cases, it might even increase the overall run time. It makes sense to benchmark your tests before and after sharding to see the benefits. Usually, large test suites with long run times benefit the most from sharding.


## Limitations


While GitHub Actions' matrix strategy is really handy for parallelizing our tests and builds, a constraint that can't be ignored for performance is the imposed limit on number of concurrent jobs. GitHub has[a limit of 20 concurrent jobs per account](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#usage-limits) . You can increase this limit to 40 on a pro account or 60 on a team account but it is still very limiting for large teams.


## Further improvements


Overall CI workflow times with GitHub actions can be reduced by parallelizing tests. Run times can be further improved by making the tests within each shard run faster by using the native parallelization features of the test frameworks or by using third-party packages.


WarpBuild provides blazing fast runners, optimized for CPU, network, and disk performance that are 30% faster at half the cost. **At[WarpBuild](https://warpbuild.com/) , there are *no limits* on the number of concurrent jobs** . You can run as many jobs as you want in parallel and minimize the wait times on GitHub Actions workflows. It takes only ~2 minutes to[get started](https://app.warpbuild.com/) .


Note


Even if WarpBuild doesn't impose a concurrency limitation, there is an upstream constraint by GitHub which limits the maximum number of jobs generated by a job matrix to 256. But that limit is seldom the problem in practice.
