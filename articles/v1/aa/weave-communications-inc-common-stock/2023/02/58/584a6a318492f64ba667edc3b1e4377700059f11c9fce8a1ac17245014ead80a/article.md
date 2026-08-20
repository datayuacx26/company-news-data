---
schema_version: "1.0.0"
document_id: "584a6a318492f64ba667edc3b1e4377700059f11c9fce8a1ac17245014ead80a"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/post/test-the-hard-parts/"
published_at: "2023-02-02T18:55:00+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:1d4a0b4c3257ded6ef8fc5c82c1d18f2d33817ee725a3b0c7d66d234cda43e14"
---

# Testing, The Hard Parts

### Testing At Weave


When Weave was still small, like many startup companies, shipping new product features was our top priority. The focus was on continuing to grow the product and suit it to the needs of a growing customer base. Weave, however, is not a startup anymore. Where once the addition of new features was the singular focus of most of our engineering department, there is now a need to take some additional technical concerns into account. Chief among these, as of late, has been the drive to quantitatively increase the quality of Weave’s SaaS offering. This year we have taken substantial steps to measure, instrument, understand and grow the quality of our code base. In particular, Weave has been building a strong culture around testing.


As part of building up this culture of testing, we’ve asked our development teams to include significantly more tests in their code. While we have not targeted any specific test coverage percentage, we’ve seen a lot of good come about from building the expectation of unit testing into our development cycles. However, as anyone who has written a lot of tests can tell you, some pieces of code are much easier to test than others. Pure functions are an easy choice, since their output depends entirely on their input. Small methods which express simple behavior are also good targets. However, as code gets more complicated or introduces side effects, it can become increasingly difficult to test.


When we initially began focusing on improving our software quality, writing new tests was fairly easy. There were many straightforward tests to write and a lot of low-hanging fruit to tackle. As the year has worn on however, that low-hanging fruit has been exhausted and much of the remaining code is more difficult to write tests for. The good news is, Weave is full of clever engineers who have found some great ways to test even tricky code. This has led to some key insights around writing good tests.


There are a few key techniques that show up repeatedly when testing code. Strategies like dependency injection, and building mocks are examples. Managing side effects and setting up a suitable test is much easier when deterministic data can be injected into a function under test. As an additional benefit, injecting dependencies can actually make a function signature clearer and more descriptive of its behavior. Building powerful mocks can compliment this pattern by making injection at the call sight simple. Both dependency injection and mocking are greatly improved by leveraging polymorphism through Go’s interfaces. Using all these approaches together can really expand the testability of a codebase.


The following sections cover 4 different methods for writing difficult tests, each of which implements these principles in unique ways. While the examples provided here are specifically focused on Go testing, the general principles described should be useful in any language.


### Testing With Temp Dirs


One example of code that can be hard to test is code that interacts with the file system. For example, we have lots of software here at Weave that needs to read/write files from an underlying OS for various reasons. Whether it’s analyzing developer code in our CI/CD platform, parsing through custom configuration files, or building caches for our CLI tools, we do a lot of file ops on the[DevX](https://engineering.getweave.com/post/platform-engineering-at-weave/) team alone. Reading and writing files is essential, but it can often cause some issues when it comes to testing. Writing to the underlying file system, is a classic example of a side effect. This often means functions that read and write to the file system are considered “bad candidates” for tests and are left out of unit test coverage. This may not be an issue in some code bases, but there are plenty of examples where testing such functionality is necessary. To overcome this challenge, we have found a couple different methods for testing code that touches the file system.


Did you know that the Go testing package has an extremely simple API for creating temporary testing directories? The Go testing package offers a[t.TempDir()](https://pkg.go.dev/testing#T.TempDir) method that can be very useful for unit testing functions that need to work with the file system.` t.TempDir()` creates a temporary directory that you can use in your unit tests. The best part is, this directory will be automatically cleaned up by Go once the test has been finished. You can even call` t.TempDir()` multiple times, and it will return a new directory on each call. Here’s some example code from our codebase that needs to interact with the file system (a quick note that this code, and other code examples in this article, have been modified slightly. This was done both to improve readability as well as to remove Weave specific code).


```text
// ExtractTarFile takes the given tarball and extracts it to the given destination path.
// It also sets the permission bits so the resulting file is executable (0755)
func    ExtractTarFile  (  tarFile    []  byte  ,    extractFileName  ,    destPath    string  )    error    {
// make sure the destination dir exists
err    :=    os  .  MkdirAll  (  filepath  .  Dir  (  destPath  ),    0  o755  )
if    err    !=    nil    {
return    err
}


// xtar is Weave's custom library for working with tar files
files  ,    err    :=    xtar  .  UnzipGzipStream  (  bytes  .  NewBuffer  (  tarFile  ),    destPath  )
if    err    !=    nil    {
return    err
}


var    foundFile    bool
for    _  ,    file    :=    range    files    {
if    filepath  .  Base  (  file  )    !=    extractFileName    {
continue
}
foundFile    =    true


err    :=    os  .  Rename  (  file  ,    destPath  )
if    err    !=    nil    {
return    err
}


err    =    os  .  Chmod  (  destPath  ,    0755  )
if    err    !=    nil    {
return    err
}
}


if    !  foundFile    {
return    fmt  .  Errorf  (  "failed to find file %s"  ,    extractFileName  )
}


return    nil
}


```


Ideally, a good test for this function would check:


1. That the function returns an error if the destPath is invalid or if extracting the tar file fails
2. If no error is returned, that the extracted file exists at the specified destPath
3. That, if the function is successful, the resulting file has the correct permission bits set (0755)


Leveraging the` t.TempDir()` method, we can cover all of these cases:


```text
//go:embed test_files/test.tar
var    testTarFile    []  byte


func    TestExtractTarFile  (  t    *  testing  .  T  )    {
type    args    struct    {
tar                []  byte
extractFileName    string
destPath           string
}
tests    :=    []  struct    {
name       string
args       args
wantErr    bool
}{
{
"missing tar file"  ,
args  {
tar  :                nil  ,
extractFileName  :    "test_file"  ,
destPath  :           "extracted/file"  ,
},
true  ,
},
{
"missing dest path"  ,
args  {
tar  :                testTarFile  ,
extractFileName  :    "test_file"  ,
destPath  :           ""  ,
},
true  ,
},
{
"golden path"  ,
args  {
tar  :                testTarFile  ,
extractFileName  :    "test_file"  ,
destPath  :           "extracted/file"  ,
},
false  ,
},
}
for    _  ,    tt    :=    range    tests    {
t  .  Run  (  tt  .  name  ,    func  (  t    *  testing  .  T  )    {
// use a temp dir so our test gets cleaned up once it's done
temp    :=    t  .  TempDir  ()


destPath    :=    filepath  .  Join  (  temp  ,    tt  .  args  .  destPath  )
if    err    :=    ExtractExecFile  (  tt  .  args  .  tar  ,    tt  .  args  .  extractFileName  ,    destPath  );    (  err    !=    nil  )    !=    tt  .  wantErr    {
t  .  Errorf  (  "ExtractExecFile() error = %v, wantErr %v"  ,    err  ,    tt  .  wantErr  )
}
if    tt  .  wantErr    {
// if we wanted an error there's no need to check anything else
return
}


// check that the file was extracted and has the right permissions
info  ,    err    :=    os  .  Stat  (  destPath  )
if    err    !=    nil    {
t  .  Error  (  "ExtractExecFile() failed to find extracted file"  )
}
if    info  .  Mode  ().  Perm  ()    !=    0  o755    {
t  .  Errorf  (  "ExtractExecFile() wanted file permissions 0755 but got 0o%o"  ,    info  .  Mode  ().  Perm  ())
}
})
}
}


```


You can probably think of some additional test cases here, but this is hopefully a clear example of the benefits of using this tool for testing.


### Testing With The Afero File System


Using the testing package to create temporary directories is great, but what about cases where you can’t, or would prefer not to use a temporary directory?


For example, maybe your tests run in a docker container with a read-only file system. Or perhaps the function under test has a specific directory with which it interacts, and can’t be trivially pointed at a temporary directory. In these cases we like to use the[afero](https://github.com/spf13/afero) file system abstraction. Afero is “A FileSystem Abstraction System for Go” and is an excellent tool for unit testing.


Afero exposes a file system interface called` Fs` , which can be used to abstract interactions with files. Here’s another example code snippet from our codebase.


```text
// DeleteCacheDir deletes the cache dir if it exists at HOME/.weave_cache
func    DeleteCacheDir  (  afs    afero  .  Fs  )    error    {
// make sure the cache folder exists before trying to delete it
// config.HOME is the current users home directory
cacheDir    :=    filepath  .  Join  (  config  .  HOME  ,    ".weave_cache"  )
if    _  ,    err    :=    afs  .  Stat  (  cacheDir  );    err    !=    nil    {
return    nil
}


// remove the cache directory
err    :=    afs  .  RemoveAll  (  cacheDir  )
if    err    !=    nil    {
return    err
}


return    nil
}


```


This function checks for, and deletes the cache folder used by one of our internal CLI tools. It comes in handy as a tool for recovering from cache corruption errors, which happen from time to time. Given the fact that the specific purpose of this function is to clear out the` ~/.weave_cache` folder, it wouldn’t make much sense to pass the file path into the function. Rather than expect the caller to remember to pass in the correct location of the current user’s cache, we chose to leverage an` afero.Fs` in this function. This keeps the API of the function simple (e.g.` DeleteCacheDir(afero.NewOsFs()))` ), while still allowing us to write some good unit tests.


```text
func    TestDeleteCacheDir  (  t    *  testing  .  T  )    {
type    args    struct    {
afs    afero  .  Fs
}
tests    :=    []  struct    {
name       string
args       args
wantErr    bool
}{
{
"cache not found"  ,
args  {
// leave the file system empty so there is no cache to be deleted
afs  :    afero  .  NewMemMapFs  (),
},
false  ,
},
{
"success"  ,
args  {
// use an anonymous function to set up the afero.Fs
afs  :    func  ()    afero  .  Fs    {
afs    :=    afero  .  NewMemMapFs  ()


// create the cache dir so there's something to delete
cacheFilePath    :=    filepath  .  Join  (  config  .  HOME  ,    ".weave_cache"  ,    "test.yaml"  )
err    :=    afero  .  WriteFile  (  afs  ,    cacheFilePath  ,    []  byte  (  "test: data\n"  ),    0666  )
if    err    !=    nil    {
t  .  Fatalf  (  "deleteCacheDir() failed to set up test afero file system %s"  ,    err  )
}


return    afs
}(),
},
false  ,
},
}
for    _  ,    tt    :=    range    tests    {
t  .  Run  (  tt  .  name  ,    func  (  t    *  testing  .  T  )    {
if    err    :=    DeleteCacheDir  (  tt  .  args  .  afs  );    (  err    !=    nil  )    !=    tt  .  wantErr    {
t  .  Errorf  (  "deleteCacheDir() error = %v, wantErr %v"  ,    err  ,    tt  .  wantErr  )
}


// ensure the cache is gone
if    _  ,    err    :=    tt  .  args  .  afs  .  Stat  (  filepath  .  Join  (  config  .  HOME  ,    ".weave_cache"  ));    !  os  .  IsNotExist  (  err  )    {
t  .  Errorf  (  "deleteCacheDir() cache dir was not deleted"  )
}
})
}
}


```


Using` afero` , we can set up a test` .weave_cache` directory and make sure that the cache directory is correctly removed. It also allows us to easily check that the function will not fail if the cache directory has already been removed before the function is called. While this is a simple example, we have much more complicated code where we rely on Afero for writing quality unit tests. It has proven to be a great tool for testing, and is also an open source project that you can check out[here](https://github.com/spf13/afero) .


### Testing With GitOps


Git should be a familiar tool for most developers, as it (or another tool like it) has become essential for working with large teams on production code bases. GitOps leverages the power of Git to do all sorts of neat operational tricks, and we’re a big fan of it here at Weave. This is especially true for our infrastructure teams where we manage things like application secrets, deployments, cluster configuration and permissions, all through GitOps. This means Weave has a lots of tools dedicated to interacting with Git and our remote GitHub repos. While this is a powerful coding pattern, GitOps code can be really tricky to test. This is especially true because this code often needs to interact with a remote repo in GitHub in order to function correctly. This means not only does our GitOps code talk to remote resources, it does so through a command line utility that also modifies the underlying file system. Like much of the code we write, however, our GitOps code is often mission critical, so our infrastructure teams had a big incentive to get that code tested. Fortunately for us, Git repos are just directories, and given the previous section of this post, we already have some good tools for dealing with the file system.


At Weave, we’ve built a tool called` wgit` which functions as a wrapper for running Git commands in our code. Here is an example of the type of code we write with this tool:


```text
// CommitAndPublish commits any changes in the repo and then pushes those changes to the remote
// if there are no changed files then nothing will be done
func    CommitAndPublish  (  ctx    context  .  Context  ,    repo    *  wgit  .  Repo  )    error    {
if    repo    ==    nil    {
return    errors  .  New  (  "repo must not be nil"  )
}


changes  ,    err    :=    repo  .  GetChangedFiles  (  ctx  )
if    err    !=    nil    {
return    err
}
if    len  (  changes  )    ==    0    {
// repo is clean, skipping commit/push
return    nil
}


err    =    repo  .  AddAll  (  ctx  )
if    err    !=    nil    {
return    err
}


err    =    repo  .  Commit  (  ctx  ,    "automatic commit by the schema generator"  )
if    err    !=    nil    {
return    err
}


err    =    repo  .  Push  (  ctx  )
if    err    !=    nil    {
return    err
}


return    nil
}


```


This function works on a local repo (usually cloned into a temporary directory) and commits any changes that the service may have made. This code, in particular, comes from our schema generator system where we do code generation and push results to a generated repo.


At first, testing something like this seems extremely tricky. We could, for example, build a mock for the` wgit.Repo` type. Unfortunately, that would mean none of our git operations would actually be tested. We could also try to spin up a mock ‘github.com’ server, but that’s a pretty big task to take on. We could also just set up` wgit.Repo` to just use a local repo and test off of that. Again, though, this decision means any code that expects a “remote” repo to exist will not be testable.


Ideally, it would be nice to have a “test” remote repo that we could clone locally into a test directory (check out the section on[testing with temp dirs](https://engineering.getweave.com/post/test-the-hard-parts/#testing-with-temp-dirs) for more info on this). Then, as long as our test environment has access to the` git` command and a writable` /tmp` directory, we could use the “test” wgit repo just like a production one. Well, it turns out there is a way to do exactly that.


While this is not a commonly used feature of Git, did you know that you can clone a Git repo from another valid Git repo on your local machine? It’s true, if you hand` git clone` a directory instead of a URL (e.g.` git clone /tmp/repo` ) it will clone you a copy of that repo with the provided directory set as the upstream. You can even push and pull changes from that remote, all from the comfort of your local file system. Using this information, I’m sure you can now see how to create a test` wgit.Repo` for our` CommitAndPublish` function.


We just initialize a test repo in a` t.TempDir()` , then we clone that test repo into a different` t.TempDir()` . The result is a` wgit.Repo` that behaves exactly the same as a normal` wgit.Repo` minus any interaction with the network. This new test repo is completely local and all contained in testing temporary directories, which are automatically cleaned up once the test is done. Importantly, it allows us to write clean tests like the following.


```text
func    TestCommitAndPublish  (  t    *  testing  .  T  )    {
type    args    struct    {
ctx     context  .  Context
repo    *  wgit  .  Repo
}
tests    :=    []  struct    {
name       string
args       args
wantErr    bool
}{
{
"commit changed files"  ,
args  {
ctx  :    context  .  Background  (),
repo  :    func  ()    *  wgit  .  Repo    {
repo  ,    err    :=    wgit  .  NewTestRepo  (  t  )
if    err    !=    nil    {
t  .  Fatalf  (  "CommitAndPush() failed to set up test case, %s"  ,    err  )
}


// change the repo readme file
err    =    repo  .  WriteFile  (  "README.md"  ,    []  byte  (  "# CHANGE README FILES"  ),    0666  )
if    err    !=    nil    {
t  .  Fatalf  (  "CommitAndPush() failed to set up test case, %s"  ,    err  )
}


return    repo
}(),
},
false  ,
},
}


for    _  ,    tt    :=    range    tests    {
t  .  Run  (  tt  .  name  ,    func  (  t    *  testing  .  T  )    {
if    err    :=    CommitAndPublish  (  tt  .  args  .  ctx  ,    tt  .  args  .  repo  );    (  err    !=    nil  )    !=    tt  .  wantErr    {
t  .  Fatalf  (  "CommitAndPush() error = %v, wantErr %v"  ,    err  ,    tt  .  wantErr  )
}
if    tt  .  wantErr    {
return
}


// check if the repo status is clean (e.g. all changes are committed and pushed)
isClean  ,    err    :=    tt  .  args  .  repo  .  Repo  .  IsClean  ()
if    err    !=    nil    {
t  .  Fatalf  (  "CommitAndPush() failed to check git status %s"  ,    err  )
}


if    !  isClean    {
t  .  Errorf  (  "CommitAndPush() repo still has outstanding changes"  )
}
})
}
}


```


Tests like this are great, because they can be used to accurately exercise Git functionality in a self-contained way. Consequently, such tests give us good confidence that the GitOps performed in the code will have the desired final effect. This does require that we run our test code in an environment that has a writable` /tmp` directory and access to a` git` command line tool. This of course introduces some challenges (e.g. making sure` git` is installed and correctly configured in our CI/CD environment), however, setting this up is well worth it for the peace of mind these types of tests give our team.


### Testing With Cmd Exec


But what if you don’t want your test code to actually run real` git` commands? Or maybe you have code that runs other executables that just can’t be executed inside a test. After all, there’s a reasonable argument that running shell commands is outside the scope of many tests. This kind of code is also fairly common in our codebase. For example, much of our platform and infrastructure code needs to interact with developers underlying OS or the docker container they run within. The following example is taken from some of our[buf](https://buf.build/) generation code where we generate Go and Typescript code for our engineers.


```text
// CheckBackwardsCompatibility runs the backwards compatibility check with the buf cli
func    CheckBackwardsCompatibility  (  ctx    context  .  Context  ,    exec    puffin  .  Exec  ,    dir    string  )    []  *  LintError    {
cmd    :=    exec  .  CommandContext  (  ctx  ,    "buf"  ,    "breaking"  ,    "--against"  ,    ".git#branch=main"  ,    "--error-format=json"  )
cmd  .  SetDir  (  dir  )
output  ,    err    :=    cmd  .  Output  ()
if    err    !=    nil    {
var    lintErrors    []  *  LintError
for    _  ,    line    :=    range    strings  .  Split  (  string  (  output  ),    "\n"  )    {
if    line    ==    ""    {
continue
}
var    lintErr    LintError
err    :=    json  .  Unmarshal  ([]  byte  (  line  ),    &  lintErr  )
if    err    !=    nil    {
panic  (  "Failed to unmarshal lint error: "    +    line  )
}
lintErrors    =    append  (  lintErrors  ,    &  lintErr  )
}


return    lintErrors
}


return    nil
}


```


Here we use[puffin](https://github.com/weave-lab/puffin) , which wraps the` os/exec` package, to run a backwards compatibility check using the` buf` command line tool. Checking for backwards compatibility issues is an important part of maintaining the health of our micro-services. Backwards incompatible changes need to be handled carefully, and such failures can often be hard to spot in a large ecosystem. This example is just one of many where using a shell command might be a part of critical code.


In order to protect this critical system against potential future bugs, it would be nice if we could write some tests. However, setting up` buf` to work correctly is non-trivial, as it relies on many other plugins and interacts with Git as well. Rather than trying to find a way to set up this full environment to test such code, we instead rely on puffin to help us.


Puffin is an open source tool that I wrote for handling these types of tests, and it works by providing an abstraction for Go’s` os/exec` package. It works much like` Afero` , which was covered earlier. You’ll notice in the code above that the` exec` argument to the function shadows the` os/exec` package name. While most of the time this is not desirable behavior, it’s actually quite useful in this specific instance. Doing this makes it easy to refactor code that uses the` os/exec` package to instead use` puffin` .


Including this extra` exec` argument does not significantly increase the complexity of calling the function. For example, callers that want to use the` CheckBackwardsCompatibility` function can call it by passing in a` puffin.NewOsExec()` like this:


```text
lintErrors    :=    CheckBackwardsCompatibility  (  ctx  ,    puffin  .  NewOsExec  (),    schemaRepoDir  ),


```


In exchange, for this small addition, testing the code is now significantly simpler.


```text
func    TestCheckBackwardsCompatibility  (  t    *  testing  .  T  )    {
type    args    struct    {
ctx     context  .  Context
exec    puffin  .  Exec
dir     string
}
tests    :=    []  struct    {
name    string
args    args
want    []  *  LintError
}{
{
"has lint errors"  ,
args  {
ctx  :    context  .  Background  (),
exec  :    puffin  .  NewFuncExec  (  puffin  .  NewHandlerMux  (
func  (  cmd    *  puffin  .  FuncCmd  )    int    {
_  ,    err    :=    cmd  .  Stdout  ().  Write  ([]  byte  (  `
{"path":"test","start_line":5,"start_column":10,"end_line":15,"end_column":20,"type":"test","message":"this is a test failure"}`  )
if    err    !=    nil    {
t  .  Errorf  (  "CheckBackwardsCompatibility() failed to write to command std err, %s"  ,    err  )
}
return    1
},
)),
},
[]  *  LintError  {{
Path  :           "test"  ,
StartLine  :      5  ,
StartColumn  :    10  ,
EndLine  :        15  ,
EndColumn  :      20  ,
Type  :           "test"  ,
Message  :        "this is a test failure"  ,
}},
},
}
for    _  ,    tt    :=    range    tests    {
t  .  Run  (  tt  .  name  ,    func  (  t    *  testing  .  T  )    {
got    :=    CheckBackwardsCompatibility  (  tt  .  args  .  ctx  ,    tt  .  args  .  exec  ,    tt  .  args  .  dir  )
if    diff    :=    deep  .  Equal  (  got  ,    tt  .  want  );    len  (  diff  )    !=    0    {
t  .  Errorf  (  "CheckBackwardsCompatibility() diff = %v"  ,    diff  )
}
})
}
}


```


In the code above,` puffin.NewFuncExec` is used instead of` puffin.NewOsExec` . Doing this allows Puffin to intercept shell commands, and run Go functions instead. This in turn gives the test author essentially limitless control over how the shell command behaves during testing. This control does not require that the developer set up any surrounding test environment. In this test, a consistent set of linting failures is returned by using a single function. This allows all the dependent code to be tested quite easily, ensuring it behaves as intended. There are several other test cases which could be written for this function. For example, what happens if we write an error to stdErr and return a non-zero error code? What if the command succeeds but malformed json data is returned? All these test cases and more are easy to write by simply changing the` handler` function that Puffin runs.


As you can see, using` puffin` allows us to write tests for code that would normally be considered untestable. The best part is, Weave maintains` puffin` as an open-source Go project, so you’re free to use it in any of you own code. You can even contribute to it, if you feel like you have an idea to improve` puffin` .


# Conclusion


This year has been an important one for engineering at Weave. It has marked significant growth for the maturity of our backend infrastructure through an increased focus on quality. It has led to thousands of additional critical lines of our code being tested. Through that process we’ve found the importance of leveraging dependency injection and polymorphism for writing good testable code. Robust mocks like` Puffin` and` Afero` have also been key in our testing strategies. Using these packages along with Go’s powerful interfaces has made testing even tricky-to-test code possible.


Fortunately, most of the tools discussed here are either part of the standard library or open source. Each of these was covered at a fairly high level, so you should absolutely dig deeper into any tool that you found particularly interesting. All the ideas presented here have much more depth than could possibly be explored in just this single article. We would especially love any feedback on` Puffin` as it’s one of our newest open source projects, and we would love the Go community’s input.


While it may be easy to write some sections of your codebase off as “un-testable”, I hope this illustrates that with some clever engineering, most code can be tested. This has been especially true at Weave where we’ve pushed our engineers to create increased confidence around critical code paths. Not every piece of code necessarily needs a test, but some code certainly does. With a little hard work and the right tools at your disposal even the hard parts of testing can be great.
