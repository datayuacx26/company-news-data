---
schema_version: "1.0.0"
document_id: "06d120f32df6427970215a2e1aa178bedd49be096c73a0313436d0ae522e64a0"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/github-actions-cache"
published_at: "2024-05-16T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:bf08bca1b395059e0188b6cef35a95dd12baed2e9218693bf15b2ef24f0e9627"
---

# Using GitHub Actions Cache with popular languages

## GitHub Actions Cache


GitHub Actions provides a powerful CI/CD platform enabling developers to automate workflows and streamline development pipelines. One valuable feature to speed up these pipelines is **caching** . By caching dependencies and other build artifacts, you can drastically reduce build times, particularly for frameworks that rely on extensive dependency fetching and compilation.


GitHub provides a[cache](https://github.com/actions/cache) action that allows workflows to cache files between workflow runs.


In this post, we'll dive into how to use this action for popular programming languages and frameworks, including Node.js, Python, Rust, Go, PHP, and Java. We'll also highlight common pitfalls, considerations, and shortcomings of the cache action to provide a comprehensive understanding.


## Benefits


Caching dependencies offers several benefits:


1.


**Faster Builds** : By caching dependencies, you can avoid the time-consuming process of downloading and installing them on every workflow run. This leads to faster build times and quicker feedback loops.


2.


**Reduced Network Bandwidth** : Caching minimizes the need to download dependencies repeatedly, saving network bandwidth and reducing the load on package registries.


3.


**Improved Reliability** : Caching ensures that your builds are less susceptible to network issues or outages of package registries, as the cached dependencies can be restored locally.


## Using the Cache Action


Note


GitHub provides[official environment setup actions](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#about-caching-workflow-dependencies) for a few popular languages and frameworks. These actions support caching dependencies out of the box. For the rest, you can use the` actions/cache` action directly to cache the relevant directories.


### Node.js


Caching dependencies in Node.js involves storing the package manager cache.


The official[setup-node](https://github.com/actions/setup-node) action[supports caching](https://github.com/actions/setup-node#caching-global-packages-data) by using` actions/cache` under the hood, abstracting out the setup required to cache the required package manager cache directories. It supports caching for` npm` ,` yarn` , and` pnpm` with the` cache` input. Caching is **disabled** be default.


Note


Caching the` node_modules` is[not recommended by GitHub](https://github.com/actions/cache/blob/0c45773b623bea8c8e75f6c82b208c3cf94ea4f9/examples.md#node---npm:~:text=Note%20It%20is%20not%20recommended%20to%20cache%20node_modules%2C%20as%20it%20can%20break%20across%20Node%20versions%20and%20won%27t%20work%20with%20npm%20ci) as it fails across Node versions and doesn't work well with` npm ci` .


```text
name  :   Node CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest


steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Node.js
uses  :   actions/setup-node@v4
with  :
node-version  :   20
cache  :   "npm"   # or 'yarn' or 'pnpm'


-   name  :   Install dependencies
run  :   npm ci   # or 'yarn install --frozen-lockfile' or 'pnpm install --frozen-lockfile'
```


The above action uses the relevant lockfile used by the package manager to create the cache key. For more control over caching, such as using your own cache keys or caching the` node_modules` directory, you can use the` actions/cache` action directly. Here's an example of caching the package directory for an` npm` project:


```text
name  :   Node CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest


steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Node.js
uses  :   actions/setup-node@v4
with  :
node-version  :   20


-   name  :   Cache .npm directory
uses  :   actions/cache@v4
with  :
path  :   ~/.npm
key  :   ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
restore-keys  :   |
${{ runner.os }}-node-


-   name  :   Install dependencies
run  :   npm ci
```


Note


Make sure to use the correct cache directory for your package manager (` npm` ,` yarn` , or` pnpm` ). You can get the cache directory by running` npm config get cache` or` yarn cache dir` . Learn more[here](https://github.com/actions/cache/blob/main/examples.md#node---npm) .


### Python


Caching in Python projects also involves storing the package manager cache directory. Similar to Node.js, the official[setup-python](https://github.com/actions/setup-python/) action also[supports caching](https://github.com/actions/setup-python/#caching-packages-dependencies) via the` cache` input.


```text
name  :   Python CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest


steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Python
uses  :   actions/setup-python@v5
with  :
python-version  :   "3.12"
cache  :   "pip"   # or 'poetry' or 'pipenv'


-   name  :   Install dependencies
run  :   pip install -r requirements.txt
```


Similarly, you can also use the` actions/cache` action directly for more control over caching:


```text
name  :   Python CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest


steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Python
uses  :   actions/setup-python@v5
with  :
python-version  :   "3.12"


-   name  :   Cache pip packages
uses  :   actions/cache@v4
with  :
path  :   ~/.cache/pip
key  :   ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
restore-keys  :   |
${{ runner.os }}-pip-


-   name  :   Install dependencies
run  :   pip install -r requirements.txt
```


Note


You can find the correct cache directory for` pip` by running` pip cache dir` and for` poetry` by running` poetry config cache-dir` . Learn more[here](https://github.com/actions/cache/blob/0c45773b623bea8c8e75f6c82b208c3cf94ea4f9/examples.md#python---pip)


### Golang


For Go projects, caching the Go modules directory speeds up the build process. The directory is generally located at` ~/go/pkg/mod` and can be found by running` go env GOMODCACHE` .


The[setup-go](https://github.com/actions/setup-go/) action provides[caching support](https://github.com/actions/setup-go/#caching-dependency-files-and-build-outputs) for Go projects with the` cache` input. Unlike Node.js and Python, this input is a boolean value that enables or disables caching. Caching is **enabled** by default.


```text
name  :   Go CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Go
uses  :   actions/setup-go@v5
with  :
go-version  :   "1.22"
cache  :   true   # Note: this is not required as caching is enabled by default


-   name  :   Build
run  :   go build ./...
```


You can also disable the caching by setting` cache: false` in the` actions/setup-go` action and use the` actions/cache` action directly for more control.


```text
name  :   Go CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Go
uses  :   actions/setup-go@v5
with  :
go-version  :   "1.22"
cache  :   false


-   name  :   Cache Go modules
uses  :   actions/cache@v4
with  :
path  :   ~/go/pkg/mod
key  :   ${{ runner.os }}-go-${{ hashFiles('**/go.sum') }}
restore-keys  :   |
${{ runner.os }}-go-


-   name  :   Build
run  :   go build ./...
```


Note


If you use[vendor directories](https://go.dev/ref/mod#vendoring) , the modules get loaded from your project's` vendor` directory instead of downloading from the network or restoring from cache. In such cases, caching the Go modules directory may not be necessary.


### Rust


Rust's package manager, Cargo caches the modules and binaries in the` ~/.cargo` directory. The compiled dependencies are stored in the` target` directory of the project.


Rust has no official GitHub Action for setup, so you can use the` actions/cache` action directly to cache the relevant directories.


```text
name  :   Rust CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Rust
uses  :   dtolnay/rust-toolchain@stable


-   name  :   Cache cargo registry and build
uses  :   actions/cache@v4
with  :
path  :   |
~/.cargo/bin/
~/.cargo/registry/index/
~/.cargo/registry/cache/
~/.cargo/git/db/
target
key  :   ${{ runner.os }}-cargo-${{ hashFiles('**/Cargo.lock') }}
restore-keys  :   |
${{ runner.os }}-cargo-


-   name  :   Build and test
run  :   cargo test --all
```


### Java


Java projects commonly use the Gradle or Maven build tools which have their corresponding cache directories.


Java does have an official[setup-java](https://github.com/actions/setup-java) action that[supports caching](https://github.com/actions/setup-java#caching-packages-dependencies) via the` cache` input. This input takes the name of the build tool (` maven` ,` gradle` or` sbt` ) to cache their relevant directories. Caching is disabled by default.


```text
name  :   Java CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Java
uses  :   actions/setup-java@v4
with  :
distribution  :   "temurin"
java-version  :   "17"
cache  :   "maven"   # or 'gradle' or 'sbt'


-   name  :   Build with Maven
run  :   mvn -B clean verify
```


You can also use the` actions/cache` action directly for more control over caching.


#### Maven Example


```text
name  :   Java CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Java
uses  :   actions/setup-java@v4
with  :
distribution  :   "temurin"
java-version  :   "17"
-   name  :   Cache Maven repository
uses  :   actions/cache@v4
with  :
path  :   ~/.m2/repository
key  :   ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
restore-keys  :   |
${{ runner.os }}-maven-
-   name  :   Build with Maven
run  :   mvn -B clean verify
```


#### Gradle Example


```text
name  :   Java Gradle CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Java
uses  :   actions/setup-java@v4
with  :
distribution  :   "temurin"
java-version  :   "17"


-   name  :   Cache Gradle wrapper
uses  :   actions/cache@v4
with  :
path  :   |
~/.gradle/caches
~/.gradle/wrapper
key  :   ${{ runner.os }}-gradle-${{ hashFiles('**/gradle/wrapper/gradle-wrapper.properties') }}
restore-keys  :   |
${{ runner.os }}-gradle-


-   name  :   Build with Gradle
run  :   chmod +x gradlew && ./gradlew build
```


### PHP


PHP projects with Composer can cache their dependencies by caching the Composer cache directory. PHP has no official GitHub Action for setup, so you can use the` actions/cache` action directly.


```text
name  :   PHP Cache Workflow
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest


steps  :
-   name  :   Checkout code
uses  :   actions/checkout@v4


-   uses  :   shivammathur/setup-php@v2
with  :
php-version  :   '8.3'


# The cache directory is usually located at ~/.composer/cache
# This step can be skipped and the cache directory can be hardcoded
# in the `path` field of the `actions/cache` step.
-   name  :   Get Composer Cache Directory
id  :   composer-cache
run  :   |
echo "dir=$(composer config cache-files-dir)" >> $GITHUB_OUTPUT


-   name  :   Cache Composer dependencies
uses  :   actions/cache@v4
with  :
path  :   ${{ steps.composer-cache.outputs.dir }}
key  :   ${{ runner.os }}-composer-${{ hashFiles('**/composer.lock') }}
restore-keys  :   |
${{ runner.os }}-composer-


-   name  :   Install dependencies
run  :   composer install --prefer-dist
```


### Ruby


The official[ruby/setup-ruby](https://github.com/ruby/setup-ruby) action provides[caching support](https://github.com/ruby/setup-ruby#caching-bundle-install-automatically) for Ruby projects with the` bundler-cache` input. This input takes a boolean value to enable or disable caching. Caching is **disabled** by default.


```text
name  :   Ruby CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Ruby
uses  :   ruby/setup-ruby@v1
with  :
ruby-version  :   "3.3"
bundler-cache  :   true


# Installing dependencies via `bundle install` or `gem install bundler`
# is not required as the action automatically installs dependencies.


-   name  :   Run tests
run  :   bundle exec rake test
```


We can also directly cache the` bundle` directory using the` actions/cache` action.


Note


Manually caching this directory[is not recommended](https://github.com/ruby/setup-ruby#caching-bundle-install-manually) , and the suggested approach is to use the` ruby/setup-ruby` action as shown above.


```text
name  :   Ruby CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup Ruby
uses  :   ruby/setup-ruby@v1
with  :
ruby-version  :   "3.3"


-   name  :   Cache gems
uses  :   actions/cache@v4
with  :
path  :   vendor/bundle
key  :   ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
restore-keys  :   |
${{ runner.os }}-gems-


-   name  :   Install dependencies
run  :   |
gem install bundler
bundle install --path vendor/bundle


-   name  :   Run tests
run  :   bundle exec rake test
```


### .NET (NuGet)


The official[setup-dotnet](https://github.com/actions/setup-dotnet) action provides[caching support](https://github.com/actions/setup-dotnet#caching-nuget-packages) for .NET projects with the` cache` input. This input takes a boolean value to enable or disable caching. Caching is **disabled** by default.


Note


Passing an explicit` NUGET_PACKAGES` is also recommended for caching the NuGet packages directory instead of the global cache directory since there might be some huge packages pre-installed.


```text
name  :   .NET CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   windows-latest
env  :
NUGET_PACKAGES  :   ${{ github.workspace }}/.nuget/packages
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup .NET
uses  :   actions/setup-dotnet@v4
with  :
dotnet-version  :   "6.x"
cache  :   true


-   name  :   Restore dependencies
run  :   dotnet restore --locked-mode


-   name  :   Build
run  :   dotnet build my-project
```


For caching manually, use the` actions/cache` action directly.


```text
name  :   .NET CI
on  : [  push  ,   pull_request  ]


jobs  :
build  :
runs-on  :   windows-latest
env  :
NUGET_PACKAGES  :   ${{ github.workspace }}/.nuget/packages
steps  :
-   uses  :   actions/checkout@v4
-   name  :   Setup .NET
uses  :   actions/setup-dotnet@v4
with  :
dotnet-version  :   "6.x"


-   name  :   Cache NuGet packages
uses  :   actions/cache@v4
with  :
path  :   ${{ github.workspace }}/.nuget/packages
key  :   ${{ runner.os }}-nuget-${{ hashFiles('**/packages.lock.json') }}   # Or **/*.csproj
restore-keys  :   |
${{ runner.os }}-nuget-


-   name  :   Restore dependencies
run  :   dotnet restore --locked-mode


-   name  :   Build
run  :   dotnet build my-project
```


## Considerations


While the cache action speeds up workflows, be mindful of these considerations:


1. **Cache Keys and Restoring:** Using unique cache keys ensures a cache is correctly restored or created. However, overly specific keys may result in missed cache hits.
2. **Sequencing:** Sequence of cache commits and restores could lead to unpredictable behavior in workflows, especially if the cache keys are insufficiently defined.
3. **Cache Size:** Large caches can take longer to restore, reducing the performance gain.
4. **Invalidation:** Changes in dependencies (like updates in` package-lock.json` or` go.sum` ) will cause a new cache to be created.
5. **Security Risks:** Ensure sensitive files are not accidentally cached.


## Common Pitfalls


1. **Concurrency Issues:** Parallel jobs can overwrite the cache leading to incomplete or corrupted data.
2. **Storage Limits:** Exceeding storage limits (10 GB per repository) will cause cache eviction and is a very low limit for many use cases.
3. **Compatibility:** Some caches may not be compatible across different operating systems or configurations.


## Conclusion


Leveraging the GitHub Actions cache action can significantly accelerate your CI/CD workflows, especially with common languages like Node.js, Python, Rust, Go, PHP, and Java. However, it's essential to manage cache keys, avoid overly large caches, and be cautious of security issues. For optimal results, test different strategies to see which works best for your specific project requirements.


## Unlimited, fast cache with WarpBuilds/cache action


While GitHub Actions provides great flexibility and functionality, workflow speeds can always benefit from improvements. This is where WarpBuild comes in. Offering GitHub Actions runners with unlimited, superfast caching capabilities, WarpBuild accelerates your builds with blazing speed. The` WarpBuilds/cache` action is a drop-in replacement for the` actions/cache` , so you can get started instantly.[Here are the cache docs](https://www.warpbuild.com/docs/ci/features/caching) .


- **Unlimited Caching:** Never worry about hitting cache size limits or losing important build data.
- **Fast Caching:** Save substantial time by utilizing highly optimized caching mechanisms.


By seamlessly integrating WarpBuild into your workflows, you can significantly speed up your CI/CD pipelines without compromising flexibility or reliability.[Try it out](https://www.warpbuild.com/) .


## References


- [Caching Dependencies with GitHub Actions](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows#examples)
- [actions/cache GitHub Repository](https://github.com/actions/cache)
- [More caching examples using actions/cache](https://github.com/actions/cache/blob/main/examples.md)
