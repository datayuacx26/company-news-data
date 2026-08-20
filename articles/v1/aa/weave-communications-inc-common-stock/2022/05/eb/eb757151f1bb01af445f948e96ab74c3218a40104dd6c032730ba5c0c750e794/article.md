---
schema_version: "1.0.0"
document_id: "eb757151f1bb01af445f948e96ab74c3218a40104dd6c032730ba5c0c750e794"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/talk/stop-using-package-variables/"
published_at: "2022-05-10T05:02:11+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:0dc9c0c6a87347e035675588eb86ff041d7896c2a7cc70d971598467e75184f9"
---

# Stop Using Package Variables

# Summary


This lighting talk was presented at the Utah Go Meetup and covered some of the major pitfalls and problems that come with using package variables in Go.


# Key Takeaways


Package variables often look like this:


```text
package    main


// x is a package variable
var    x    =    1


func    main  ()    {
println  (  x  )
}


```


A simple example like this might seem innocuous. But it is easy to introduce subtle bugs. Because any function in the package can manipulate them. You end up with shared state. And shared state is dangerous!


A more complete example that illustrates how bugs can be introduced can be seen in the following code:


```text
// START MAIN OMIT
var    (
url    =    ""
)


func    main  ()    {
s    :=    httptest  .  NewServer  (  http  .  HandlerFunc  (  func  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
io  .  WriteString  (  w  ,    "You called "  +  r  .  URL  .  Path  )
}))
defer    s  .  Close  ()
url    =    s  .  URL


go    startHealthProbes  ()


doHello  ()
time  .  Sleep  (  time  .  Second  )
doHello  ()
}


```


The code above does the following:


- It starts a web server which prints the requested url path for all requests
- It sets the *package variable*` url` to the generated url for the test server
- It starts a background health probe process to request` /health` every half second
- It requests` /`
- It waits one second
- It requests` /` again


The code for` doHello` is trivial:


```text
func    doHello  ()    {
r  ,    _    :=    http  .  Get  (  url  )
io  .  Copy  (  os  .  Stdout  ,    r  .  Body  )
fmt  .  Println  ()
}


```


The code for` startHealthProbes` seems simple enough as well.


```text
func    startHealthProbes  ()    {
for    range    time  .  Tick  (  time  .  Second    /    2  )    {
doHeathProbe  ()
}
}


func    doHeathProbe  ()    {
// the health probe needs a timeout (default is 0 which means no timeout)
http  .  DefaultClient  .  Timeout    =    time  .  Second    *    30


// the health probe needs to use a different url
url    =    url    +    "/health"


r  ,    err    :=    http  .  Get  (  url  )
if    err    !=    nil    {
panic  (  err  )
}
defer    r  .  Body  .  Close  ()


// read until EOF
io  .  Copy  (  io  .  Discard  ,    r  .  Body  )
}


```


However, If you run the code above then you will see that the first call to` doHello` prints correctly, but the second call prints an unexpected result for the second call to` doHello` :


```text
$ go run main.go
You called /
You called /health


```


The reason is that` url` is a package variable! So when` doHeathProbe` changes it to add the` /health` to the end, it breaks it for the second call to` doHello` .


This example is simple enough and could be avoided with a slight change to` doHealthProbe` but hopefully it illustrates one way of how package variables can introduce subtle bugs.


> The full example code for the above problem is[here](https://github.com/carsonoid/lightning-talks/blob/main/stop-using-package-variables/examples/package-variables-broken.go)


# Details


The full source code for the talk can be found at:


- [https://github.com/carsonoid/lightning-talks/tree/main/stop-using-package-variables](https://github.com/carsonoid/lightning-talks/tree/main/stop-using-package-variables)
