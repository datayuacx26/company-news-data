---
schema_version: "1.0.0"
document_id: "33175005f209b013a9ef944963007ca67016136c15827fc39c4b454404477752"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/cgo-and-python/"
published_at: "2018-04-16T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:3e3b5a2082609beca9bccb6dd6e50db63e65a9b3e71b685de2c3eda1912d5d1c"
---

# Cgo and Python

Massimiliano Pippi


If you look at the[new Datadog Agent](https://github.com/DataDog/datadog-agent/) , you might notice most of the codebase is written in Go, although the checks we use to gather metrics are still written in Python. This is possible because the Datadog Agent, a regular Go binary,[embeds](https://docs.python.org/2/extending/embedding.html) a CPython interpreter that can be called whenever it needs to execute Python code. This process can be made transparent using an abstraction layer so that you can still write idiomatic Go code even when there’s Python running under the hood.


There are a number of reasons why you might want to embed Python in a Go application:


-


It is useful during a port; gradually moving portions of an existing Python project to the new language without losing any functionality during the process.


-


You can reuse existing Python software or libraries without re-implementing them in the new language.


-


You can dynamically extend your software by loading and executing regular Python scripts, even at runtime.


The list could go on, but for the Datadog Agent the last point is crucial: we want you to be able to execute custom checks or change existing ones without forcing you to recompile the Agent, or in general, to compile anything.


Embedding CPython is quite easy and well documented. The interpreter itself is written in C and a C API is provided to programmatically perform operations at a very low level, like creating objects, importing modules, and calling functions.


In this article we’ll show some code examples, and we’ll focus on keeping the Go code idiomatic while interacting with Python at the same time, but before we proceed we need to address a small gap: the embedding API is C but our main application is Go, how can this possibly work?


## Introducing cgo


There are[a number of good reasons](https://dave.cheney.net/2016/01/18/cgo-is-not-go) why you might not want to introduce cgo in your stack, but embedding CPython is one of those cases where you must.[Cgo](https://golang.org/cmd/cgo/) is not a language nor a compiler. It’s a[Foreign Function Interface](https://en.wikipedia.org/wiki/Foreign_function_interface) (FFI), a mechanism we can use in Go to invoke functions and services written in a different language, specifically C.


When we say “cgo” we’re actually referring to a set of tools, libraries, functions, and types that are used by the go toolchain under the hood so we can keep doing` go build` to get our Go binaries. An absolutely minimal example of a program using cgo looks like this:


```text
1   package     main    2
3   // #include <float.h>    4   import     "C"    5   import     "fmt"    6
7   func     main  () {    8         fmt  .  Println  (  "Max float value of float is"  ,   C  .  FLT_MAX  )    9   }
```


The comment block right above the` import "C"` instruction is called a “preamble” and can contain actual C code, in this case an header inclusion. Once imported, the “C” pseudo-package lets us “jump” to the foreign code, accessing the` FLT_MAX` constant. You can build the example by invoking` go build` , the same as if it was plain Go.


If you want to have a look at all the work cgo does under the hood, run` go build -x` . You’ll see the “cgo” tool will be invoked to generate some C and Go modules, then the C and Go compilers will be invoked to build the object modules and finally the linker will put everything together.


You can read more about cgo on the[Go blog](https://blog.golang.org/c-go-cgo) . The article contains more examples and few useful links to get further into details.


Now that we have an idea of what cgo can do for us, let’s see how we can run some Python code using this mechanism.


## Embedding CPython: a primer


A Go program that, technically speaking, embeds CPython is not as complicated as you might expect. In fact, at the bare minimum, all we have to do is initialize the interpreter before running any Python code and finalize it when we’re done. Please note that we’re going to use Python 2.x throughout all the examples but everything we’ll see can be applied to Python 3.x as well with very little adaptation. Let’s look at an example:


python-2.7


```text
1   package     main    2
3   // #include <Python.h>    4   import     "C"    5   import     "fmt"    6
7   func     main  () {    8         C  .  Py_Initialize  ()    9         fmt  .  Println  (  C  .  GoString  (  C  .  Py_GetVersion  ()))    10         C  .  Py_Finalize  ()    11   }
```


The example above does exactly what the following Python code would do:


```text
1   import   sys    2   print  (sys.version)
```


You can see we put a` #cgo` directive in the preamble; those directives are passed to the toolchain to let you change the build workflow. In this case, we tell cgo to invoke “pkg-config” to gather the flags needed to build and link against a library called “python-2.7” and pass those flags to the C compiler. If you have the CPython development libraries installed in your system along with pkg-config, this would let you keep using a plain` go build` to compile the example above.


Back to the code, we use` Py_Initialize()` and` Py_Finalize()` to set up and shut down the interpreter and the` Py_GetVersion` C function to retrieve the string containing the version information for the embedded interpreter.


If you’re wondering, all the cgo bits we need to put together to invoke the C Python API are boilerplate code. This is why the Datadog Agent relies on[go-python](https://github.com/sbinet/go-python) for all the embedding operations; the library provides a Go friendly thin wrapper around the C API and hides the cgo details. This is another basic embedding example, this time using go-python:


```text
1   package     main    2
3   import   (    4         python     "github.com/sbinet/go-python"    5   )    6
7   func     main  () {    8         python  .  Initialize  ()    9         python  .  PyRun_SimpleString  (  "print 'hello, world!'"  )    10         python  .  Finalize  ()    11   }
```


This looks closer to regular Go code, no more cgo exposed and we can use Go strings back and forth while accessing the Python API. Embedding looks powerful and developer friendly. Time to put the interpreter to good use: let’s try to load a Python module from disk.


We don’t need anything complex on the Python side, the ubiquitous “hello world” will serve the purpose:


foo.py


```text
1   def     hello  ():    2         """    3          Print hello world for fun and profit.    4          """    5         print     "hello, world!"
```


The Go code is slightly more complex but still readable:


main.go


```text
1   package     main    2
3   import     "github.com/sbinet/go-python"    4
5   func     main  () {    6         python  .  Initialize  ()    7         defer     python  .  Finalize  ()    8
9         fooModule   :=   python  .  PyImport_ImportModule  (  "foo"  )    10         if     fooModule   ==   nil   {    11             panic  (  "Error importing module"  )    12          }    13
14         helloFunc   :=   fooModule  .  GetAttrString  (  "hello"  )    15         if     helloFunc   ==   nil   {    16             panic  (  "Error importing function"  )    17          }    18
19         // The Python function takes no params but when using the C api    20         // we're required to send (empty) *args and **kwargs anyways.    21         helloFunc  .  Call  (  python  .  PyTuple_New  (  0  ),   python  .  PyDict_New  ())    22   }
```


Once built, we need to set the` PYTHONPATH` environment variable to the current working dir so that the import statement will be able to find the` foo.py` module. From a shell, the command would look like this:


Terminal window


```text
1   $     go     build     main.go   &&   PYTHONPATH  =  .     ./main    2   hello,     world!
```


## The dreadful Global Interpreter Lock


Having to bring in cgo in order to embed Python is a tradeoff: builds will be slower, the Garbage Collector won’t help us managing memory used by the foreign system, and cross compilation will be non-trivial. Whether or not these are concerns for a specific project can be debated, but there’s something I deem not negotiable: the Go concurrency model. If we couldn’t run Python from a goroutine, using Go altogether would make very little sense.


Before playing with concurrency, Python, and cgo, there’s something we need to know: it’s the Global Interpreter Lock, also known as the GIL. The GIL is a mechanism widely adopted in language interpreters (CPython is one of those) preventing more than one thread from running at the same time. This means that no Python program executed by CPython will be ever able to run in parallel within the same process. Concurrency is still possible and in the end, the lock is a good tradeoff between speed, security, and implementation simplicity. So why should this pose a problem when it comes to embedding?


When a regular, non-embedded Python program starts, there’s no GIL involved to avoid useless overhead in locking operations; the GIL starts the first time some Python code requests to spawn a thread. For each thread, the interpreter creates a data structure to store information about the current state and locks the GIL. When the thread has finished, the state is restored and the GIL unlocked, ready to be used by other threads.


When we run Python from a Go program, none of the above happens automatically. Without the GIL, multiple Python threads could be created by our Go program. This could cause a race condition leading to fatal runtime errors, and most likely a segmentation fault bringing down the whole Go application.


The solution to this problem is to explicitly invoke the GIL whenever we run multithreaded code from Go; the code is not complex because the C API provides all the tools we need. To better expose the problem, we need to do something CPU bounded from Python. Let’s add these functions to our foo.py module from the previous example:


foo.py


```text
1   import   sys    2
3   def     print_odds  (  limit  =  10  ):    4         """    5          Print odds numbers < limit    6          """    7         for   i   in     range  (limit):    8             if   i%  2  :    9                  sys.stderr.write(  "  {}  \n  "  .format(i))    10
11   def     print_even  (  limit  =  10  ):    12         """    13          Print even numbers < limit    14          """    15         for   i   in     range  (limit):    16             if   i%  2   ==   0  :    17                  sys.stderr.write(  "  {}  \n  "  .format(i))
```


We’ll try to print odd and even numbers concurrently from Go, using two different goroutines (thus involving threads):


```text
1   package     main    2
3   import   (    4         "sync"    5
6         "github.com/sbinet/go-python"    7   )    8
9   func     main  () {    10         // The following will also create the GIL explicitly    11         // by calling PyEval_InitThreads(), without waiting    12         // for the interpreter to do that    13         python  .  Initialize  ()    14
15         var     wg     sync  .  WaitGroup    16         wg  .  Add  (  2  )    17
18         fooModule   :=   python  .  PyImport_ImportModule  (  "foo"  )    19         odds   :=   fooModule  .  GetAttrString  (  "print_odds"  )    20         even   :=   fooModule  .  GetAttrString  (  "print_even"  )    21
22         // Initialize() has locked the the GIL but at this point we don't need it    23         // anymore. We save the current state and release the lock    24         // so that goroutines can acquire it    25         state   :=   python  .  PyEval_SaveThread  ()    26
27         go     func  () {    28             _gstate   :=   python  .  PyGILState_Ensure  ()    29             odds  .  Call  (  python  .  PyTuple_New  (  0  ),   python  .  PyDict_New  ())    30             python  .  PyGILState_Release  (  _gstate  )    31
32             wg  .  Done  ()    33          }()    34
35         go     func  () {    36             _gstate   :=   python  .  PyGILState_Ensure  ()    37             even  .  Call  (  python  .  PyTuple_New  (  0  ),   python  .  PyDict_New  ())    38             python  .  PyGILState_Release  (  _gstate  )    39
40             wg  .  Done  ()    41          }()    42
43         wg  .  Wait  ()    44
45         // At this point we know we won't need Python anymore in this    46         // program, we can restore the state and lock the GIL to perform    47         // the final operations before exiting.    48         python  .  PyEval_RestoreThread  (  state  )    49         python  .  Finalize  ()    50   }
```


While reading the example you might note a pattern, the pattern that will become our mantra to run embedded Python code:


1.


Save the state and lock the GIL.


2.


Do Python.


3.


Restore the state and unlock the GIL.


The code should be straightforward but there’s a subtle detail we want to point out: notice that despite seconding the GIL mantra, in one case we operate the GIL by calling` PyEval_SaveThread()` and` PyEval_RestoreThread()` , in another (look inside the goroutines) we do the same with` PyGILState_Ensure()` and` PyGILState_Release()` .


We said when multithreading is operated from Python, the interpreter takes care of creating the data structure needed to store the current state, but when the same happens from the C API, we’re responsible for that.


When we initialize the interpreter with go-python, we’re operating in a Python context. So when` PyEval_InitThreads()` is called it initializes the data structure and locks the GIL. We can use` PyEval_SaveThread()` and` PyEval_RestoreThread()` to operate on already existing state.


Inside the goroutines, we’re operating from a Go context and we need to explicitly create the state and remove it when done, which is what` PyGILState_Ensure()` and` PyGILState_Release()` do for us.


## Unleash the Gopher


At this point we know how to deal with multithreading Go code executing Python in an embedded interpreter but after the GIL, another challenge is right around the corner: the Go scheduler.


When a goroutine starts, it’s scheduled for execution on one of the` GOMAXPROCS` threads available—[see here](https://morsmachine.dk/go-scheduler) for more details on the topic. If a goroutine happens to perform a syscall or call C code, the current thread hands over the other goroutines waiting to run in the thread queue to another thread so they can have better chances to run; the current goroutine is paused, waiting for the syscall or the C function to return. When this happens, the thread tries to resume the paused goroutine, but if this is not possible, it asks the Go runtime to find another thread to complete the goroutine and goes to sleep. The goroutine is finally scheduled to another thread and it finishes.


With this in mind, let’s see what can happen to a goroutine running some Python code when a goroutine is moved to a new thread::


1.


Our goroutine starts, performs a C call, and pauses. The GIL is locked.


2.


When the C call returns, the current thread tries to resume the goroutine, but it fails.


3.


The current thread tells the Go runtime to find another thread to resume our goroutine.


4.


The Go scheduler finds an available thread and the goroutine is resumed.


5.


The goroutine is almost done and tries to unlock the GIL before returning.


6.


The thread ID stored in the current state is from the original thread and is different from the ID of the current thread.


7.


Panic!


Luckily for us, we can force the Go runtime to always keep our goroutine running on the same thread by calling the LockOSThread function from the runtime package from within a goroutine:


```text
1   go     func  () {    2         runtime  .  LockOSThread  ()    3
4         _gstate   :=   python  .  PyGILState_Ensure  ()    5         odds  .  Call  (  python  .  PyTuple_New  (  0  ),   python  .  PyDict_New  ())    6         python  .  PyGILState_Release  (  _gstate  )    7         wg  .  Done  ()    8   }()
```


This will interfere with the scheduler and might introduce some overhead, but it’s a price that we’re willing to pay to avoid random panics.


## Conclusions


In order to embed Python, the Datadog Agent has to accept a few tradeoffs:


-


The overhead introduced by cgo.


-


The task of manually handling the GIL.


-


The limitation of binding goroutines to the same thread during execution.


We’re happy to accept each of these for the convenience of running Python checks in Go. But by being conscious of the tradeoffs, we’re able to minimize their effect. Regarding other limitations introduced to support Python, we have few countermeasures to contain potential issues:


-


The build is automated and configurable so that devs have still something very similar to` go build` .


-


A lightweight version of the agent can be built stripping out Python support entirely simply using Go build tags.


-


Such a version only relies on core checks hardcoded in the agent itself (system and network checks mostly) but is cgo free and can be cross compiled.


We’ll re-evaluate our options in the future and decide whether keeping around cgo is still worth it; we could even reconsider whether Python as a whole is still worth it, waiting for the[Go plugin package](https://golang.org/pkg/plugin/) to be mature enough to support our use case. But for now the embedded Python is working well and transitioning from the old Agent to the new one couldn’t be easier.


Are you a polyglot who loves mixing different programming languages? Do you love learning about the inner workings of languages to make your code more performant?[Join us at Datadog!](https://careers.datadoghq.com/)


-
-
-
