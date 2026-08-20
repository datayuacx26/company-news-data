---
schema_version: "1.0.0"
document_id: "26aa63a2f6a04b10e59de976be2eb87dad17acc57c0cc0f48baa4311c3debc97"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/talk/nil-is-not-nil/"
published_at: "2022-05-10T05:02:11+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:0736aa8fbd27621b7c8e80ab343c74a490980037574fad6a036a6868ad2a8722"
---

# Nil Is Not Nil

# Summary


This lightning talk was presented at the Utah Go Meetup and covered some unintuitive ways that` nil` and interfaces can interact in Go.


# Key Takeaways


Interfaces in Go wrap a value and include both a` type` as well as a` value` . In order for an interface to be nil, both the` type` and` value` must be nil. For example:


```text
package    main


func    main  ()    {
var    a    *  int             // a is nil
var    b    interface  {}      // b is nil
c    :=    interface  {}(  a  )    // c is not nil
}


```


In this snippet` a` is clearly` nil` becase the default value for all pointer types in Go is` nil` . b is also nil because the default` type` and` value` for an interface{} are both` nil` . c however, is not nil because while it wraps a nil` value` , it’s underlying type is` *int` .


Name Type Value Is Nil?


a n/a nil yes


b nil nil yes


c *int nil no


This can lead to some unexpected results when using` nil` checks in your code.


```text
package    main


import    (
"bytes"
"io"
)


func    main  ()    {
var    bodyBuf    *  bytes  .  Buffer
example  (  bodyBuf  )
}


func    example  (  body    io  .  Reader  )    {
if    body    ==    nil    {    // prevent nil panics
return
}


switch    v    :=    body  .(  type  )    {
case    *  bytes  .  Buffer  :
bodyLen    :=    v  .  Len  ()    // still get a nil panic
}
}


```


In the example above you can see that while we do check if body is nil, we still end up with a` nil` panic when the code is run. This is because` body` is actually an interface with a concrete type` *bytes.Buffer` and therefore is not` nil` . Once we convert the interface back into a` *bytes.Buffer` we’re back to a` nil` value. This causes the code to panic when we try to call the buffers Len method.


We can fix this code in one of three ways.


1. We can avoid passing` nil` values into the example function and instead only pass` nil` in explicitly. This works because an explicit nil has not type so the underlying interface will have a` nil` type as well.


```text
package    main


import    (
"bytes"
"io"
)


func    main  ()    {
var    bodyBuf    *  bytes  .  Buffer
if    bodyBuf    ==    nil    {
example  (  nil  )
}    else    {
example  (  bodyBuf  )
}
}


...


```


1. We can check for nil after type casting inside the example function.


```text
package    main


import    (
"bytes"
"io"
)


func    main  ()    {
var    bodyBuf    *  bytes  .  Buffer
example  (  bodyBuf  )
}


func    example  (  body    io  .  Reader  )    {
if    body    ==    nil    {
return
}


switch    v    :=    body  .(  type  )    {
case    *  bytes  .  Buffer  :
if    v    ==    nil    {    // check for nil again
return
}
bodyLen    :=    v  .  Len  ()
}
}


```


1. Or, we can directly check if the value inside the interface is` nil` using the reflect package.


```text
package    main


import    (
"bytes"
"io"
"reflect"
)


func    main  ()    {
var    bodyBuf    *  bytes  .  Buffer
example  (  bodyBuf  )
}


func    example  (  body    io  .  Reader  )    {
if    body    ==    nil    ||    isNilV  (  body  )    {
return
}


// ...
}


func    isNilV  (  r    io  .  Reader  )    bool    {
if    r    ==    nil    {
return    true
}


// https://pkg.go.dev/reflect#Value.IsNil
rv    :=    reflect  .  ValueOf  (  r  )
if    (  rv  .  Kind  ()    ==    reflect  .  Ptr    &&    rv  .  IsNil  ())    ||
(  rv  .  Kind  ()    ==    reflect  .  Chan    &&    rv  .  IsNil  ())    ||
(  rv  .  Kind  ()    ==    reflect  .  Func    &&    rv  .  IsNil  ())    ||
(  rv  .  Kind  ()    ==    reflect  .  Map    &&    rv  .  IsNil  ())    ||
(  rv  .  Kind  ()    ==    reflect  .  Interface    &&    rv  .  IsNil  ())    ||
(  rv  .  Kind  ()    ==    reflect  .  Slice    &&    rv  .  IsNil  ())    {
return    true
}


return    false
}


```


A quick note on this final fix.` isNilV` as presented here is error-prone and does not cover several edge cases. Because of this,` reflect` should be used with caution to check for nil interfaces.


# Details


The full source code fo this talk can be found[here](https://github.com/brandon-ja/talks/tree/main/nil_is_not_nil)
