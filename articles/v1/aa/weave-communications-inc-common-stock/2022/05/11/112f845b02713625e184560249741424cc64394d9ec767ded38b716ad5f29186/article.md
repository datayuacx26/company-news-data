---
schema_version: "1.0.0"
document_id: "112f845b02713625e184560249741424cc64394d9ec767ded38b716ad5f29186"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/post/go-jwt-3/"
published_at: "2022-05-08T18:26:47+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:cb9a867b4ae81525787b67fcdc8bd354ad77aa1b1856bb2b2d3acddc18ab3123"
---

# Distributed Auth With Go: From JWT to HTTP to gRPC - Part 3

Welcome back to our[series on JWT authentication in Go](https://engineering.getweave.com/series/go-jwt-auth) !


Now it’s time to wrap it up with part 3. Where we will take a fully functional but verbose system and turn it into something that is a joy to work with.


#### First a Refresher


- In part 1, we covered the basics of JWT, what it does well and how we use it in Go. *
- In part 2, we took our basic JWT system and expanded it to include a fully distributed three service stack where each layer did its own validation.


This image below outlines what we have currently built.


#### Current


Recall that a valid end to end flow for this architecture has two steps:


1. Send a request to the` auth-api` to login and get a JWT token.
2. Use that JWT to make requests to the` frontend` over HTTP.


In some cases, the` frontend` may need to make calls to the` backend` to service the requests. When that happens, the frontend wraps the user-provided JWT and passes it down to the` backend` , where the entire validation process happens again.


---


In the current code, there are three places that we have to manually and consistently deal with JWT tokens:


1. In each` frontend` HTTP handler function.
2. Before each` backend` gRPC client invocation.
3. In each` backend` gRPC handler function.


We must remember to call the protocol-specific token parsing function manually even if we don’t need a token. It’s not only[toil](https://sre.google/sre-book/eliminating-toil/) , but it’s also prone to user error: “Whoops, I forgot to parse the token to do auth.”


So, there has to be a better way, right? Yep! And we call that Middleware: We register our code with our HTTP server, gRPC client, & server, which will run **automatically** so we can stop worrying about auth and just let it happen. That makes our functions cleaner and reduces the chance of errors.


### First: A talk about Context


Before we get to the Middleware itself, we need to take a detour to discuss the` context` package that comes shipped with Go. There are many articles entirely about` context` , but we should talk about how it relates to our Middleware.


The entire idea of Middleware is that we transparently and automatically execute code before our handlers run. So this presents an issue: How do you pass data to a handler that doesn’t even know you exist? In Go, the answer is with` context.Context.`


Both the the` HTTP` package and` gRPC` packages we used in Part 2 have native support for attaching a Context to each request. While Context can be used for **request cancellation** , the part we care about here is using it for **value injection.**


Basically, we will write two functions in` simplejwt` :to add a value into a context and get the value from the context.Context.


` context.Context` Best practices encourage users to use package-specific private types and constants for storing data in the context. Then, the package exposes public helper functions to interact with these types. There are good reasons for this we will cover below.


#### Writing the context key type and constant


```text
// middlewareContextKey is our custom type to ensure our context values are unique
type    middlewareContextKey    string


// tokenContextKey is the key used for a parsed token
const    tokenContextKey    middlewareContextKey    =    "token"


```


This code is just a string type and a single constant using that type. In some cases, you may see packages using` iota` to do integer constants, but we use a string instead.


#### Writing the Context Setter function


```text
// ContextWithToken adds the given token to the given context
func    ContextWithToken  (  ctx    context  .  Context  ,    token    *  jwt  .  Token  )    context  .  Context    {
return    context  .  WithValue  (  ctx  ,    tokenContextKey  ,    token  )
}


```


This function is straightforward, essentially just a wrapper around` context.WithValue` that stores the token in the context with our` tokenContext` key type.


This token will only really be used by our Middleware handler functions.


#### Writing the Context Getter function


```text
// ContextGetToken tries to get the token from the context
// it returns an error if the token is missing or invalid
//
// It DOES NOT validate the token claims or signature
// That would require the public key and should have been handled
// by the process that set the token originally
func    ContextGetToken  (  ctx    context  .  Context  )    (  *  jwt  .  Token  ,    error  )    {
val    :=    ctx  .  Value  (  tokenContextKey  )
if    val    ==    nil    {
return    nil  ,    errors  .  New  (  "no token in context"  )
}


t  ,    ok    :=    val  .(  *  jwt  .  Token  )
if    !  ok    {
return    nil  ,    errors  .  New  (  "unexpected token type in context"  )
}


return    t  ,    nil
}


```


This function is a bit more complicated, but not much. Essentially it is just fetching the` *jwt.Token` from the context value map, then converting the returned` interface{}` to the right type and returning errors along the way.


This function will be used by any handlers that need to get the token to use the data within.


#### Bonus “panic more” with:` MustContextGetToken`


```text
// MustContextGetToken parses the token out of the context
// it will panic if the token is not found
func    MustContextGetToken  (  ctx    context  .  Context  )    *  jwt  .  Token    {
t  ,    err    :=    ContextGetToken  (  ctx  )
if    err    !=    nil    {
panic  (  err  )
}


return    t
}


```


Since getting tokens is a typical operation and something that will work for the entire code base or not. We can decide to dramatically streamline our codebase by having a Must version of our token getter function that does the normal token get but has a panic on error.


We panic on the error here instead of returning the token getter error. We do this because the only case where our getter would fail in this instance would be an improperly configured Middleware. That’s exceptional behavior, and because of this,[it’s ok to panic](https://levelup.gitconnected.com/its-ok-to-panic-in-go-8169e4e3ce6c) .


---


That’s everything we’ll be doing with context. Just setting the extracted JWT token in it and then maybe fetching it back later. Now, on to the good stuff.


### Adding HTTP Server Middleware to the frontend


The first place that toil exists in our HTTP handler func is in the snippets from the current` frontend` code below. You can see that **every** handler must explicitly check the token in order to enable authentication.


For example, the` RootHandler` below is getting the token and checking the error to require auth but throwing away the returned token value.


```text
func    (  f    *  Frontend  )    RootHandler  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
// get the token just to do auth, ignore the actual value
_  ,    err    :=    f  .  getHeaderToken  (  r  .  Header  )
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusBadRequest  )
w  .  Write  ([]  byte  (  "auth error:"    +    err  .  Error  ()))
return
}


w  .  Write  ([]  byte  (  "ok\n"  ))
}


```


Also, the` ClaimsHandler` is checking the token to require auth. It then uses the token later (not shown).


```text
func    (  f    *  Frontend  )    ClaimsHandler  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
// get the token so we can use it to print claims
token  ,    err    :=    f  .  getHeaderToken  (  r  .  Header  )
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusBadRequest  )
w  .  Write  ([]  byte  (  "auth error:"    +    err  .  Error  ()))
return
}
// ...


```


Finally, the` HelloHandler` is also checking and using the token:


```text
func    (  f    *  Frontend  )    HelloHandler  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
// [code omitted]


// get the token to pass it down, even though we don't use
// it here, we do require it
token  ,    err    :=    f  .  getHeaderToken  (  r  .  Header  )
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusBadRequest  )
w  .  Write  ([]  byte  (  "auth error:"    +    err  .  Error  ()))
return
}


```


Hopefully, you can see how much toil, repeated code, and cognitive overhead we have in our frontend by now. But the good news is that there is an easy way to eliminate this code: Middleware!


#### What is HTTP Middleware?


At its core, The HTTP Middleware in Go is just a normal HTTP handler function. The only difference is that this function exists purely to do setup work and then call the next function in the chain.


So let’s write it already! Just like with the` Validator` and` Issuer` , we will be adding this middleware to our` simplejwt` package so we can write the code once and use it in multiple services as needed.


We start with a basic wrapper struct and factory function.


```text
// Middleware handles all jwt parsing and validation automatically
type    Middleware    struct    {
// embed the validator to make token calls cleaner
Validator
}


// NewMiddleware creates a new middleware that validates using the
// given public key file
func    NewMiddleware  (  publicKeyPath    string  )    (  *  Middleware  ,    error  )    {
validator  ,    err    :=    NewValidator  (  publicKeyPath  )
if    err    !=    nil    {
return    nil  ,
fmt  .  Errorf  (  "unable to create validator: %w"  ,    err  )
}


return    &  Middleware  {
Validator  :    *  validator  ,
},    nil
}


```


Notice that the` Middleware` will require our JWT public key. This is because the Middleware will completely handle all JWT auth for us. In fact, we aren’t rewriting the validation. Instead, we are re-using the` Validator` type from parts 1 and 2.


> NOTE: One choice made here was to embed the` Validator` struct into` Middleware` . This is not a functional requirement and was done simply to make the` Middleware` calls cleaner. Merely having the` Validator` as a regular struct member would also be acceptable.


Now that we have a basic type, we only need one method of this type. The code below may seem complicated. But if you look carefully, you will see that the bulk of this function is the **exact** code from the` getHeaderToken` function we wrote in Part 2. I will explain the main differences below the snippet.


```text
func    (  m    *  Middleware  )    HandleHTTP  (  h    http  .  Handler  )    http  .  HandlerFunc    {
return    func  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
parts    :=    strings  .  Split  (  r  .  Header  .  Get  (  "Authorization"  ),    " "  )
if    len  (  parts  )    <    2    ||    parts  [  0  ]    !=    "Bearer"    {
w  .  WriteHeader  (  http  .  StatusUnauthorized  )
w  .  Write  (
[]  byte  (  "missing or invalid authorization header"  ),
)
return
}
tokenString    :=    parts  [  1  ]


token  ,    err    :=    m  .  GetToken  (  tokenString  )
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusUnauthorized  )
w  .  Write  ([]  byte  (  "invalid token: "    +    err  .  Error  ()))
return
}


// Get a new context with the parsed token
ctx    :=    ContextWithToken  (  r  .  Context  (),    token  )


fmt  .  Println  (  "* HTTP SERVER middleware validated and set set token"  )


// call the next handler with the updated context
h  .  ServeHTTP  (  w  ,    r  .  WithContext  (  ctx  ))
}
}


```


So, what sets this code apart from` getHeaderToken` and our old handlers? A few things:


**Difference 1: The function signature**


We don’t have a normal handler func here, although we are **returning** a normal handler func. Instead, the signature is a bit more complicated, so I’ll explain it here:


```text


```


This function takes in a handler function and then returns a handler function. It’s the job of` HandleHTTP` to return a function that does something, in our case authentication, and then call the following function in the chain as needed. We call it Middleware because it literally sits in the middle of the webserver and the next handler.


If the token validation fails, we simply write a failed auth header to the user and complete the request. That means that **our Middleware now acts as our entire authentication layer.**The handler functions can assume that authentication happens and not even bother with the token if they don’t need it!


**Difference 2: Context Value Injection**


We finally get to the` context` operations discussed earlier in the article. We take the current context from the request, use our setter function to inject the token, add the returned context back into the request and call the next handler in the chain.


```text
// Get a new context with the parsed token
ctx := ContextWithToken(r.Context(), token)


fmt.Println("* HTTP SERVER middleware validated and set set token")


// call the next handler with the updated context
h.ServeHTTP(w, r.WithContext(ctx))


```


The print is not something you would typically see in production code, but I have put it in for now so we can visualize middleware token handling in a later demonstration.


#### Using the HTTP middleware


Now, we just need to set up the Middleware in our` frontend` server’s` main.go` . This is done in the same place we would have previously made the` Validator` , but instead, we are making the` Middleware` by passing it the public key for our JWT system and letting it do validation:


```text
func    main  ()    {
if    len  (  os  .  Args  )    !=    2    {
fmt  .  Printf  (  "USAGE %s <public-ed-key-path>\n"  ,    os  .  Args  [  0  ])
os  .  Exit  (  1  )
}


// create middleware using the given public key path
middleware  ,    err    :=    simplejwt  .  NewMiddleware  (  os  .  Args  [  1  ])
if    err    !=    nil    {
panic  (  err  )
}


```


To enable the HTTP Middleware, we will create two` mux` structs. One is for our business logic, and one is for our Middleware.


**The business logic mux**


```text
// create "business logic" mux
// thanks to the middleware, we can just write simple handlers
// that can assume auth is always done
mux    :=    http  .  NewServeMux  ()
// add handlers here
mux  .  HandleFunc  (  "/claims"  ,    frontend  .  ClaimsHandler  )
mux  .  HandleFunc  (  "/hello"  ,    frontend  .  HelloHandler  )


```


Notice that we can now write all our business logic handlers with the business logic only thanks to our Middleware.


**The Middleware “root” mux**


```text
// create a root mux
root    :=    http  .  NewServeMux  ()


// all routes run through the middleware
// which calls business logic mux if auth passes
root  .  Handle  (  "/"  ,    middleware  .  HandleHTTP  (  mux  ))


fmt  .  Println  (  "Listening on :8082"  )
err    =    http  .  ListenAndServe  (  ":8082"  ,    root  )


```


Some more alternative and more advanced mux packages in Go have a native concept of Middleware, but the code above shows how you can do it with the standard Go` HTTP` package.


In short, our root mux has only one handler func called for every request. And then that handler func is responsible for calling the business logic mux, which can further route the request.


This pattern has nothing auth specific and can be used to implement any business logic required.


#### Rewriting the handler functions


Now that authentication is automatic, and we have our` context` helpers to get tokens when needed, we can rewrite our handlers, and they become greatly simplified.


```text
// get the token from the context to write the claims
token    :=    simplejwt  .  MustContextGetToken  (  r  .  Context  ())


w  .  Write  ([]  byte  (  fmt  .  Sprint  (  token  .  Claims  )))
}


```


The` ClaimsHandler` uses the` MustContextGetToken` helper to get out the token and then writes the claims back.


```text
w  .  Write  ([]  byte  (  "OK"  ))
}


```


The` RootHandler` handler is now just a single line. Authentication has already happened for the Middleware to have let the request through. So we no longer have to have this function even care about the token. It can just do its work and take auth as a given.


What about our` HelloHandler` ? Before we talk about that, we need to review gRPC. Sure, we have an HTTP Middleware, but by using gRPC client Middleware, we can even further streamline our handlers.


> NOTE: If you don’t care about gRPC you can skip to the demo at the end.


> Everything from here on out is gRPC related. I will also be a bit briefer on how gRPC middleware works since many other articles cover it in more depth than I will here.


### Adding gRPC Client Middleware to the frontend HTTP server


So, even though automatic JWT parsing is now coming into the` frontend` we still have a case where we would need to extract the token from the` context` to add it back into our gRPC client` metadata` .


In part 2, we did this manually, but again, that is toil and prone to error. And in Go, this can be 100% automated using a` UnaryClientInterceptor` function, AKA: gRPC client middleware.


> Don’t be put off by the term “Unary” here. Since gRPC can also make streaming requests, we must clarify what kind of middleware we are registering: Unary, meaning one request and one response, or Streaming Middleware. In this article, we are only dealing with Unary interceptors.


The` UnaryClientInterceptor` function type is defined in the official[Go gRPC Docs](https://pkg.go.dev/google.golang.org/grpc#UnaryClientInterceptor) . Here it is with added comments for clarity:


```text
type    UnaryClientInterceptor    func  (
ctx    context  .  Context  ,       // the outgoing context
method    string              // The method name
req  ,    reply    interface  {},    // the req, reply data
cc    *  ClientConn  ,            // client connection information
invoker    UnaryInvoker  ,      // the next invoker in the chain
opts    ...  CallOption  ,        // the upstream included options
)    error


```


There is a lot here. But for our auth purposes, we only need to mess with` ctx` . That’s where we’ll add in our custom` metadata` to attach the string version of our token to every outgoing client request. We do this by writing a function in the` Middleware` struct that fits the required definition.


```text
func    (  m    *  Middleware  )    UnaryClientInterceptor    func  (
ctx    context  .  Context  ,       // the outgoing context
method    string              // The method name
req  ,    reply    interface  {},    // the req, reply data
cc    *  ClientConn  ,            // client connection information
invoker    UnaryInvoker  ,      // the next invoker in the chain
opts    ...  CallOption  ,        // the upstream included options
)    error
// Get the token from the context
token  ,    err    :=    ContextGetToken  (  ctx  )
if    err    !=    nil    {
return    fmt  .  Errorf  (  "token not set in context: %w"  ,    err  )
}


// add the auth token to the outgoing grpc context using
// the generic grpc metadata tools
ctx    =    metadata  .  NewOutgoingContext  (  ctx  ,
metadata  .  New  (
map  [  string  ]  string  {
"jwt"  :    token  .  Raw  ,
},
),
)


fmt  .  Println  (  "* gRPC CLIENT middleware set token"  )


// call the invoker with everythign else untouched
return    invoker  (  ctx  ,    method  ,    req  ,    reply  ,    cc  ,    opts  ...  )
}


```


Notice that this is just the manual metadata work that used to be in our HTTP handler from part 2. We get the token from the context, then create new metadata and add it to the context. Then we simply log a message and call the next invoker in the chain with our updated context, and the other values are passed to the interceptor function.


Ok, so we have written a client Middleware. How do we make sure our client uses it? Easy! We just need to register the function when we create the client:


```text
// create our client, add the client interceptor (middleware)
// that way we automatically pass on the token from the context
// to the next call
conn  ,    err    :=    grpc  .  Dial  (  "localhost:8083"  ,
grpc  .  WithTransportCredentials  (  insecure  .  NewCredentials  ()),
grpc  .  WithUnaryInterceptor  (  middleware  .  UnaryClientInterceptor  ),
)
if    err    !=    nil    {
log  .  Fatalf  (  "did not connect: %v"  ,    err  )
}
defer    conn  .  Close  ()


backendClient    :=    pb  .  NewGreeterClient  (  conn  )


```


Now, any users of` backendClient` will automatically have a token from the context added to every outgoing client method.


#### Summing up the HTTP Server changes


So, now we have added both HTTP server middleware to handle validation for all handlers, **and** we have added a client interceptor to the gRPC client used by our` SayHello` handler. What does this mean for our frontend code? We get consistent and automatic token handling at both edges of our service. It also means that our frontend code gets to be incredibly simple while retaining the automated security provided by the Middleware.


Thanks to the two kinds of Middleware we just implemented,[the final frontend code is only about 100 lines long](https://github.com/carsonoid/talk-lets-auth-with-go/blob/main/cmd/1-frontend-mw/main.go) !


### Adding gRPC Server Middleware to the backend


To implement gRPC Server Middleware, all we need is something that implements the requisite function type. In gRPC terms, our Server Middleware is called a` UnaryInterceptor` . The type is shown below. The comments added here are not in the[official docs](https://pkg.go.dev/google.golang.org/grpc#UnaryServerInterceptor) but merely added here for clarity.


```text
type    UnaryServerInterceptor    func  (
ctx    context  .  Context  ,       // the incoming context
req    interface  {},           // the incoming request
info    *  UnaryServerInfo  ,     // information about the server
handler    UnaryHandler       // the next handler in the chain
)    (  resp    interface  {},    err    error  )


```


In our case, we will add the required function into our current` Middleware` struct. The function below satisfies the required definition, and if you look close, you might recognize the rest of the code.


```text
func    (  m    *  Middleware  )    UnaryClientInterceptor  (  ctx    context  .  Context  ,    method    string  ,    req  ,    reply    interface  {},    cc    *  grpc  .  ClientConn  ,    invoker    grpc  .  UnaryInvoker  ,    opts    ...  grpc  .  CallOption  )    error    {
// Get the token from the context
token  ,    err    :=    ContextGetToken  (  ctx  )
if    err    !=    nil    {
return    fmt  .  Errorf  (  "token not set in context: %w"  ,    err  )
}


// add the auth token to the outgoing grpc context using
// the generic grpc metadata tools
ctx    =    metadata  .  NewOutgoingContext  (  ctx  ,
metadata  .  New  (
map  [  string  ]  string  {
"jwt"  :    token  .  Raw  ,
},
),
)


fmt  .  Println  (  "* gRPC CLIENT middleware set token"  )


// call the invoker with everything else untouched
}


```


It seems like a lot, but it is essentially doing what we did in part 2: Getting the token and adding it to` metadata` inside the request` context` . Then just like with our HTTP middleware, we call the following method in the chain.


So how do we register this new function with our gRPC server? We just need to create the Middleware struct and pass the interceptor function when creating our initial gRPC server.


```text
// create middleware using the given public key path
if    err    !=    nil    {
panic  (  err  )
}


s    :=    grpc  .  NewServer  (
// just pass our middleware function as the interceptor
grpc  .  UnaryInterceptor  (  middleware  .  UnaryServerInterceptor  ),
)
pb  .  RegisterGreeterServer  (  s  ,    backend  )


```


Our server will automatically have our middleware function called before our handler functions are called. Just like with HTTP, we now have utterly automated authentication. If the Middleware ever fails to fetch and validate a token, it will respond with an error and return early.


We can clean up our handlers like our HTTP server, which simplifies everything!


```text
func    (  b    *  Backend  )    SayHello  (
ctx    context  .  Context  ,    in    *  pb  .  HelloRequest
)    (  *  pb  .  HelloReply  ,    error  )    {
// not having a token is now an exceptional state
// and we can just let the context helper
// panic if that happens
token    :=    simplejwt  .  MustContextGetToken  (  ctx  )


// dig the roles from the claims
roles    :=    token  .  Claims  .(  jwt  .  MapClaims  )[  "roles"  ]


return    &  pb  .  HelloReply  {
Message  :    fmt  .  Sprintf  (  "Hello %s! I am the backend. You have roles %v"  ,    in  .  GetName  (),    roles  ),
},    nil
}


```


Since we can now expect our handler only to be called when a valid token has been given and processed by the Middleware, we can just use our` MustContextGetToken` helper to get the token and use it as needed. Like our HTTP server, even if we didn’t get the token, this endpoint would automatically be doing authentication for every handler**.**


Thanks to the Middleware, our final gRPC server implementation is[less than 75 lines long!](https://github.com/carsonoid/talk-lets-auth-with-go/blob/main/cmd/2-backend-mw/main.go) But more importantly, we now get authentication for every single handler. That means developers spend less time worrying about securing their gRPC servers and more time working on features.


### The Final Stack


That’s it! We now have a three-service, fully self-contained, and automated authentication stack using JWT and Go. We have implemented automatic token handling at every one of our services’ entry and exit points. The more complex JWT work has been cleanly abstracted into our opinionated[simplejwt](https://github.com/carsonoid/talk-lets-auth-with-go/tree/main/pkg/simplejwt) package!


The last thing I will leave you with is a full-stack demo example. In all 3 of our Middleware functions, we added some simple` println` logging, and this is where we get to see all our Middleware in action. We will start and run our test with the following code:


```text
# run all 3 services in the background and give them time to start
go run ./cmd/0-auth-api auth.ed  &   sleep  1
go run ./cmd/1-frontend-mw auth.ed.pub  &   sleep  1
go run ./cmd/2-backend-mw auth.ed.pub  &   sleep  1


# Get a new JWT  token from the auth-api
t  =  $(  curl -s admin:pass@localhost:8081/login )
echo    "token:   $t  "


# use the token to call the fronted, which in turn calls the backend
curl -s -H  "Authorization: Bearer   $t  "   localhost:8082/hello


```


Looking at the full stack diagram, we expect all three of our Middleware handlers to get called. This is what we see when we run this:


```text
Listening on :8081
Listening on :8082
server listening at [::]:8083
token: eYjhbG.......
* HTTP SERVER middleware validated and set the token
* gRPC CLIENT middleware set token
* gRPC SERVER middleware validated and set token
Greeting: Hello my friend! I am the backend. You have roles [admin basic]


```


Thanks to the printed lines, we can see all 3 Middleware functions live as they happen.


For reference, the entire code flow for the call to` /hello` was:


- The HTTP Server Middleware will take the token string out of` Authorization` header, validate it, and put it in the request context.
- The` SayHello` HTTP handler function will get called. Remember, even though this handler does not use the token directly, the token was verified thanks to the Middleware. So this handler is protected by token authentication.
- The HTTP handler will make a call to the backend using our Middleware-enabled client.
- The gRPC client: the gRPC` UnaryClientInterceptor` will extract the token from the context and reformat it into the gRPC` metadata` before making the request.
- In the gRPC backend server, the gRPC` UnaryServerInterceptor` will extract the metadata from the request, check for a token in the data, and validate the token. If validation passes, it will add it into the new` context` that is passed to the gRPC handler.
- The gRPC handler can then fetch the token from its context and use the data inside when building a response.


That may seem like many steps, but remember, **that automatically happens for every call.**There is no need to remember any functions or do anything special for any handler function we write.


### Wrapping Up


Hopefully, this series has helped you get a good grasp of the fundamentals of JWT Authentication with Go. Not only that, I hope you see that with just a bit more work, all of the hard work around authentication can be automated away, and you can ease the developer burden. Many of the techniques used here could also apply just as easily to other types of authentication and not just JWT.


You can also find the[full code base for this series on GitHub](https://github.com/carsonoid/talk-lets-auth-with-go) , and a[video of this presentation](https://www.youtube.com/watch?v=f4bmW8CiNCs) can be found on the[Forge Utah YouTube channel](https://www.youtube.com/channel/UC7aCz1ur-s48fwm8Zfhjlbg) .


## Companion Video


If you would prefer a video version of this talk you can find it on the Forge Utah YouTube channel
