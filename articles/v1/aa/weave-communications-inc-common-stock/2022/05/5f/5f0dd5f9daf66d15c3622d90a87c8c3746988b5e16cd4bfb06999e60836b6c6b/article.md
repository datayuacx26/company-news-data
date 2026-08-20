---
schema_version: "1.0.0"
document_id: "5f0dd5f9daf66d15c3622d90a87c8c3746988b5e16cd4bfb06999e60836b6c6b"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/post/go-jwt-2/"
published_at: "2022-05-01T18:26:46+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:153850bbff2eee1afeeba472b6580bdb0d1e72e5090668223d72b9e477f5c16f"
---

# Distributed Auth With Go: From JWT to HTTP to gRPC - Part 2

Welcome back to our[series on JWT authentication in Go](https://engineering.getweave.com/series/go-jwt-auth) !


In the previous article, we built an opinionated JWT handling package called` simplejwt` . We made a choice to use asymmetric keys and learned how to issue and validate a JWT token properly. We then built a simple CLI tool that could issue tokens and another that could validate. The resulting architecture looked something like this:


But in the real world, CLIs aren’t what our users usually want. And so, in this article, we move on to using JWT for something more useful: Authentication over HTTP (and as a bonus, gRPC!)


### Now to the services!


The command-line clients have proven that the system works, but now it’s time to build something more akin to what we might see in a real-world scenario. This is the final architecture we are now going to build:


This may seem like a lot, but all we have done is add an extra validator and moved from command line tools to HTTP & gRPC based services. We will just be adding simple servers around our existing` simplejwt` package as we move on.


One thing worth calling out in the diagram is that this architecture allows for fully distributed validation. One of the best things about a JWT system is that each validating service need only have the public key from our key pair. My favorite part of this diagram is that **there are no lines from our validation services back to the` auth-api.`** This means that we do not have to scale our auth api as our traffic increases!


Before we go on, let’s clarify a few terms. In the context of this article, “frontend” will refer to an HTTP-based, user-facing REST API service written in Go. It does not refer to any UI, and there will be no JavaScript anywhere in the examples.


You could think of this more as the “gateway” service to your infrastructure. One focused on providing an API that would be consumable via a proper web UI. Additionally, some calls to the “frontend” will need data from the downstream “backend” service, and we will want that backend service to be able to make its own authentication decisions. Having “gatekeeper” services at the edge and assuming trust later in the stack is a bad idea, and we will avoid it by doing JWT validation at every entry point.


#### Building the “auth-api” Service


This will all start with an` AuthService` struct. This struct will simply include a field for a` simplejwt.Issuer` that will be responsible for actually creating tokens. Here we have a basic struct and a helper factory function.


```text
// AuthService handles authentication and issues tokens
type    AuthService    struct    {
issuer    *  simplejwt  .  Issuer
}


// NewAuthService creates a new service using the given issuer
func    NewAuthService  (  issuer    *  simplejwt  .  Issuer  )    (  *  AuthService  ,    error  )    {
if    issuer    ==    nil    {
return    nil  ,    errors  .  New  (  "issuer is required"  )
}
return    &  AuthService  {
issuer  :    issuer  ,
},    nil
}


```


Next, we will write a very standard function that satisfies the` http.HandleFunc` signature. This function is going to do our “login” flow. It takes in credentials, validates them, and if they are valid, it will issue a new token for the user.


Before we go on: **Most of this function is placeholder code.**It does static checking of the username and password to illustrate where that checking would happen. In any real-life scenario, you should use a more advanced and secure way of storing and validating user credentials or delegating the validation to an SSO service.


```text
func    (  a    *  AuthService  )    HandleLogin  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
// check basic auth
user  ,    pass  ,    ok    :=    r  .  BasicAuth  ()
if    !  ok    {
w  .  WriteHeader  (  http  .  StatusUnauthorized  )
w  .  Write  ([]  byte  (  "missing basic auth"  ))
return
}
// This is a bad idea in anything real.
// this article is about JWTs and not auth systems
// so we only have trivial credential checking here
if    user    !=    "admin"    ||    pass    !=    "pass"    {
w  .  WriteHeader  (  http  .  StatusUnauthorized  )
w  .  Write  ([]  byte  (  "invalid credentials"  ))
return
}
tokenString  ,    err    :=    a  .  issuer  .  IssueToken  (  "admin"  ,    []  string  {  "admin"  ,    "basic"  })
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusInternalServerError  )
w  .  Write  ([]  byte  (  "unable to issue token:"    +    err  .  Error  ()))
return
}
_  ,    _    =    w  .  Write  ([]  byte  (  tokenString    +    "\n"  ))
}


```


Notice how we don’t have much JWT specific code here. All the hard work and design choices were made when we wrote` issuer.IssueToken` . So, really all the` auth-api` has to do is put some slight HTTP wrappings around the issuer.


Finally, we just need to make a` main` func where we create and start a go HTTP server with our handler enabled:


```text
func    main  ()    {
if    len  (  os  .  Args  )    !=    2    {
fmt  .  Printf  (  "USAGE %s <private-ed-key-path>\n"  ,    os  .  Args  [  0  ])
os  .  Exit  (  1  )
}


issuer  ,    err    :=    simplejwt  .  NewIssuer  (  os  .  Args  [  1  ])
if    err    !=    nil    {
panic  (  err  )
}


auth  ,    err    :=    NewAuthService  (  issuer  )
if    err    !=    nil    {
panic  (  err  )
}


mux    :=    http  .  NewServeMux  ()
mux  .  HandleFunc  (  "/login"  ,    auth  .  HandleLogin  )


fmt  .  Println  (  "Listening on :8081"  )
err    =    http  .  ListenAndServe  (  ":8081"  ,    mux  )
if    err    !=    nil    {
panic  (  err  )
}
}


```


Other than the HTTP mux work, there is very little new code here compared to our command line issuer. In this case, we still create a new issuer using the **private key** of our keypair, but instead of issuing and returning, we start an HTTP server that handles the issue requests.


That’s it! Now, if we were to start this service in the background and then curl it:


```text
go run ./cmd/0-auth-api auth.ed &
t=$(curl -s admin:pass@localhost:8081/login)
echo "token: $t"


```


Assuming our basic auth credentials are valid, we would get a token in the response:


```text
token: eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcGkiLCJleHAiOjE2NDczMTA1MjgsImlhdCI6MTY0NzMxMDQ2OCwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgxIiwibmJmIjoxNjQ3MzEwNDY4LCJyb2xlcyI6WyJhZG1pbiIsImJhc2ljIl0sInVzZXIiOiJhZG1pbiJ9.uGoHiR8YIdhvz9iW-WUtCF70QDOD3ncIyfsE_1AFv1PIc1IXeYi6UlfXYh1yg0k7Xs_0jfrffymprdj0JpJ7CA


```


Now, if a proper web UI was using this` auth-api` , it could save that token and use it for later API calls to the frontend. We will also be using this service for later run examples.


#### Building the “frontend” HTTP Service


Now that we have a service that can issue tokens, it’s time to write a service that uses them to do something! This new “frontend” service will listen on HTTP, require a token for all endpoints, and in some cases it will, use user data from the token in the responses or make runtime authorization decisions.


Much like with the` auth-api` , it all starts with a struct and a helper factory function:


```text
type    Frontend    struct    {
validator        *  simplejwt  .  Validator
backendClient    pb  .  GreeterClient
}


func    NewFrontend  (  validator    *  simplejwt  .  Validator  ,    backendClient    pb  .  GreeterClient  )    (  *  Frontend  ,    error  )    {
return    &  Frontend  {
validator  :        validator  ,
backendClient  :    backendClient  ,
},    nil
}


```


The only thing new here is the` backendClient` which is a generated gRPC client. **We will cover that later,** but it is shown here for completeness and can be ignored for now.


> If you wonder why this code statically returns a nil error: As we go forward from here, some of the code omits basic things like nil checking for brevity. Any actual production code should properly check for invalid input.


Next, we are going to write a helper method. Since every single endpoint will need to do validation, it makes sense to write that validation once to make it easy to consume. We will also eliminate the need for this function entirely when we get to middleware later on.


```text
func    (  f    *  Frontend  )    getHeaderToken  (  h    http  .  Header  )    (  *  jwt  .  Token  ,    error  )    {
auth    :=    strings  .  Split  (  h  .  Get  (  "Authorization"  ),    " "  )
if    len  (  auth  )    <    2    ||    auth  [  0  ]    !=    "Bearer"    {
return    nil  ,    errors  .  New  (  "invalid Authorization header"  )
}
tokenString    :=    auth  [  1  ]


// Parse the token from the header
token  ,    err    :=    f  .  validator  .  GetToken  (  tokenString  )
if    err    !=    nil    {
return    nil  ,    fmt  .  Errorf  (  "invalid token: %w"  ,    err  )
}
return    token  ,    nil
}


```


This is a fairly basic function. Its job is to take a client header that looks like this:


```text
Authorization: Bearer eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJhd...


```


Then split out the token bit and validate it before returning it in a parsed struct format. We have put this into a dedicated function so that we can re-use it in all our handlers.


> Remember the` Validator.GetToken` method from` simplejwt` doesn’t just parse the token to a struct. It also checks the token signature and time fields. So, even if we don’t need the result of the` validator.GetToken` , it **must** get called to actually check the token validity.


Now we just need to build our HTTP handler functions! Since all the complex JWT validation is hidden in our helper. These functions are pretty simple.


First, we write a` ClaimsHandler` func which will serve traffic under` /claims` This function calls` getHeaderToken` to check that the request has a valid JWT token in the` Authorization` header and then uses the resulting token to write the token claims back to the client. This wouldn’t be useful for most production cases but is very nice for a basic smoke-test of our validation.


```text
func    (  f    *  Frontend  )    ClaimsHandler  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
// get the token so we can use it to print claims
// this will fail if the token is not provided or is invalid
token  ,    err    :=    f  .  getHeaderToken  (  r  .  Header  )
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusBadRequest  )
w  .  Write  ([]  byte  (  "auth error:"    +    err  .  Error  ()))
return
}


w  .  Write  ([]  byte  (  fmt  .  Sprintf  (  "%#v\n"  ,    token  .  Claims  )))
}


```


Next is another basic handler that still does token validation to handle authentication but does not use the token.


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


> As things stand, we **must** add the token fetcher func to each and every handler individually if we want the handlers to require JWT authentication. For now, we are doing this manually. In Part 3, we will be replacing these manual checks with HTTP middleware that will do it automatically for all handlers! But for now, we will write handlers with manual validation.


Finally, we will write the` main` for our HTTPservice. First, we we make a` simplejwt.Validator` just like we did for our CLI issuer:


```text
import    "github.com/carsonoid/talk-lets-auth-with-go/pkg/simplejwt"


func    main  ()    {
if    len  (  os  .  Args  )    !=    2    {
fmt  .  Printf  (  "USAGE %s <public-ed-key-path>\n"  ,    os  .  Args  [  0  ])
os  .  Exit  (  1  )
}


validator  ,    err    :=    simplejwt  .  NewValidator  (  os  .  Args  [  1  ])
if    err    !=    nil    {
panic  (  err  )
}


```


Next, we use our frontend factory function, create a standard go HTTP mux, and then add our handlers.


```text
frontend  ,    err    :=    NewFrontend  (  validator  ,    backendClient  )
if    err    !=    nil    {
panic  (  err  )
}


mux    :=    http  .  NewServeMux  ()
mux  .  HandleFunc  (  "/"  ,    frontend  .  RootHandler  )
mux  .  HandleFunc  (  "/claims"  ,    frontend  .  ClaimsHandler  )
mux  .  HandleFunc  (  "/hello"  ,    frontend  .  HelloHandler  )


fmt  .  Println  (  "Listening on :8082"  )
err    =    http  .  ListenAndServe  (  ":8082"  ,    mux  )
if    err    !=    nil    {
panic  (  err  )
}
}


```


> Eagle-eyed readers may have noticed the extra` HelloHandler` route. We will come back to this after writing the backend service.


That’s it! The actual code for the frontend is relatively minimal, and, as with the CLI, all the complex JWT work is hidden away behind our opinionated wrapper package.


We just need to run it! The following code starts our auth-api and frontend, gets a token, then uses it to issue two requests.


```text
go run ./cmd/0-auth-api auth.ed
go run ./cmd/1-frontend auth.ed.pub
t  =  $(  curl -s admin:pass@localhost:8081/login )
echo    "token:   $t  "
curl -s -H  "Authorization: Bearer   $t  "   localhost:8082/
curl -s -H  "Authorization: Bearer   $t  "   localhost:8082/claims


```


Running the code above might result in something like this:


```text
Listening on :8081
Listening on :8082
token: eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcGkiLCJleHAiOjE2NDc2NDQ2OTQsImlhdCI6MTY0NzY0NDYzNCwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgxIiwibmJmIjoxNjQ3NjQ0NjM0LCJyb2xlcyI6WyJhZG1pbiIsImJhc2ljIl0sInVzZXIiOiJhZG1pbiJ9.zCbVTZxO5KXNlAb8G02iTzE-ZbL3TcU2jafTv1zv0tANTy8tTEjqCz8Zw_BNHdCSMHyZjI6Bm2StqeDuuFcOBA


ok
map[aud:api exp:1.647644694e+09 iat:1.647644634e+09 iss:http://localhost:8081 nbf:1.647644634e+09 roles:[admin basic] user:admin]


```


Running the` curl` command without the header also proves that both require authentication:


```text
$ curl -s localhost:8082/
auth error:invalid Authorization header


$ curl -s localhost:8082/claims
auth error:invalid Authorization header


```


We now have a two-party distributed authentication system! We created an HTTP server and added routes with auth. We could stop here, but let’s make it more interesting and throw a gRPC “backend” service into the mix!


> If you aren’t interested in gRPC, you can stop here, be sure to come back for Part 3 to see how we use HTTP middleware to eliminate all the manual authentication checks and make things even cleaner.


#### Building the “backend” gRPC Service


We could keep adding complexity to our frontend API. But it’s pretty common for services to distribute work to other services in the stack. And gRPC is a fantastic choice for internal, service-to-service communication.


Since this is a JWT article, I will not be digging too deep into the specifics of how you generate gRPC code. In fact, I will not be generating any code at all! Instead, we will use the publicly available` helloworld` proto provided as part of the google gRPC examples.


The example proto file looks like this:


```text
syntax    =    "proto3"  ;


option    go_package    =    "google.golang.org/grpc/examples/helloworld/helloworld"  ;


package    helloworld  ;


// The greeting service definition.
service    Greeter    {
// Sends a greeting
rpc    SayHello    (  HelloRequest  )    returns    (  HelloReply  )    {}
}


// The request message containing the user's name.
message    HelloRequest    {
string    name    =    1  ;
}


// The response message containing the greetings
message    HelloReply    {
string    message    =    1  ;
}


```


Notice that it sets a specific public go package name option! We can simply import that into our codebase and skip past gRPC generation and dig into how we make auth work.


For our purposes, you only need to understand that to implement a` Greeter` service, we need a struct with a` SayHello` function. We will write this in a second.


> For more information on how this` proto` works, and how the Go code is generated, see the[gRPC Go quickstart](https://grpc.io/docs/languages/go/quickstart/) page.


With our gRPC foundation out of the way, let’s dig into the backend code! First, we start by building a basic backend struct and factory.


```text
import    pb    "google.golang.org/grpc/examples/helloworld/helloworld"


type    Backend    struct    {
pb  .  UnimplementedGreeterServer
validator    *  simplejwt  .  Validator
}


func    NewBackend  (  validator    *  simplejwt  .  Validator  )    (  *  Backend  ,    error  )    {
return    &  Backend  {
validator  :    validator  ,
},    nil
}


```


Now, before we write our` SayHello` function, let’s talk about how we send our JWT token string via` metadata` :


Unlike HTTP, gRPC is stateful. This means that we cannot use headers as we did with HTTP. Instead, when we need to send metadata, like a` JWT` , from a gRPC client to a server, we need a new standard. In go, this standard is called` metadata` and is implemented via` “google.golang.org/grpc/metadata”.`


We will be using two helper functions from this package to get and set our JWT:


- SET:` metadata.NewOutgoingContext(ctx, map\[string\]string)`
- GET:` metadata.FromIncomingContext(ctx)`


These functions work like a gRPC version of HTTP headers, and it’s essentially just a standardized way to send a map of key/value pairs for each request.


An example way to set a JWT token string in the metadata would look like this:


```text
// add the auth token to the outgoing grpc context using
// the generic grpc metadata tools
ctx    :=    metadata  .  NewOutgoingContext  (
r  .  Context  (),
metadata  .  New  (
map  [  string  ]  string  {
"jwt"  :    token  .  Raw  ,
},
),
)


```


An example way to GET the JWT token string out of metadata would look something like this:


```text
func    (  b    *  Backend  )    tokenFromContextMetadata  (  ctx    context  .  Context  )    (  *  jwt  .  Token  ,    error  )    {
// rip the token from the metadata via the context
headers  ,    ok    :=    metadata  .  FromIncomingContext  (  ctx  )
if    !  ok    {
return    nil  ,    errors  .  New  (  "no metadata found in context"  )
}
tokens    :=    headers  .  Get  (  "jwt"  )
if    len  (  tokens  )    <    1    {
return    nil  ,    errors  .  New  (  "no token found in metadata"  )
}
tokenString    :=    tokens  [  0  ]


token  ,    err    :=    b  .  validator  .  GetToken  (  tokenString  )
if    err    !=    nil    {
return    nil  ,    fmt  .  Errorf  (  "invalid token: %w"  ,    err  )
}


return    token  ,    nil
}


```


This is **very** much like our` getHeaderToken` function for an HTTP server. It just uses a slightly different format and package. The result is the same: as long as all the gRPC handlers call this function, they can be assured that there was a valid token in the request.


> The same rules apply to our HTTP server. **Every** handler must manually call this function to enforce authentication, whether they need the token or not. Again, stay tuned for Part 3 when we replace the manual toil with gRPC middleware, so you get automatic auth for every handler.


Now (at last!), lets build our` Backend` handler:


```text
// SayHello implements helloworld.GreeterServer it
// requires a valid token in the context and
// prints the included preferred name from the request
// and roles from the token
func    (  b    *  Backend  )    SayHello  (  ctx    context  .  Context  ,    in    *  pb  .  HelloRequest  )    (  *  pb  .  HelloReply  ,    error  )    {
token  ,    err    :=    b  .  tokenFromContextMetadata  (  ctx  )
if    err    !=    nil    {
return    nil  ,    fmt  .  Errorf  (  "could not get token: %w"  ,    err  )
}


// dig the roles from the claims
roles    :=    token  .  Claims  .(  jwt  .  MapClaims  )[  "roles"  ]


return    &  pb  .  HelloReply  {
Message  :    fmt  .  Sprintf  (
"Hello %s! I am the backend. You have roles %v"  ,
in  .  GetName  (),    roles  ),
},    nil
}


```


Notice that this isn’t much different from an HTTP handler, other than the fact that we get to receive and send structs and errors instead of dealing with things like HTTP requests and HTTP writers.


Our` SayHello` call validates the token from the per-request metadata and then uses it to say hello to the user with a message about the roles found in their token.


The last thing we need to do for the backend is put it all together in a main. The first bit is identical to our CLI validator and our frontend server code. We just build a validator using the public key:


```text
func    main  ()    {
if    len  (  os  .  Args  )    !=    2    {
fmt  .  Printf  (  "USAGE %s <public-ed-key-path>\n"  ,    os  .  Args  [  0  ])
os  .  Exit  (  1  )
}


if    err    !=    nil    {
panic  (  err  )
}


```


Next, we build our backend struct, create a gRPC server, register it as a` GreeterServer` , then start the server on a local listener:


```text
backend  ,    err    :=    NewBackend  (  validator  )
if    err    !=    nil    {
panic  (  err  )
}


s    :=    grpc  .  NewServer  ()
pb  .  RegisterGreeterServer  (  s  ,    backend  )


lis  ,    err    :=    net  .  Listen  (  "tcp"  ,    ":8083"  )
if    err    !=    nil    {
panic  (  err  )
}


fmt  .  Printf  (  "server listening at %v\n"  ,    lis  .  Addr  ())
if    err    :=    s  .  Serve  (  lis  );    err    !=    nil    {
panic  (  err  )
}
}


```


With this in place, it’s time to revisit our` frontend` service and add our` SayHello` This is the handler that will use the backend to do some work for a specific client request.


First, we need to update the frontend main to create and use a new backend client. This is the bit of code we will add to the` frontend\`\`main` :


```text
conn  ,    err    :=    grpc  .  Dial  (  "localhost:8083"  ,
grpc  .  WithTransportCredentials  (  insecure  .  NewCredentials  ()),
)
if    err    !=    nil    {
log  .  Fatalf  (  "did not connect: %v"  ,    err  )
}
defer    conn  .  Close  ()


backendClient    :=    pb  .  NewGreeterClient  (  conn  )


frontend  ,    err    :=    NewFrontend  (  validator  ,    backendClient  )


```


Again, this is all generated code, and we just need to make a client and tell it where the server will be.


Now we write the handler:


```text
func    (  f    *  Frontend  )    HelloHandler  (  w    http  .  ResponseWriter  ,    r    *  http  .  Request  )    {
// get preferred name from the request, default to my friend
preferredName    :=    r  .  URL  .  Query  ().  Get  (  "preferredName"  )
if    preferredName    ==    ""    {
preferredName    =    "my friend"
}


// get the token to pass it down, even though we don't use
// it here, we do require it
token  ,    err    :=    f  .  getHeaderToken  (  r  .  Header  )
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusBadRequest  )
w  .  Write  ([]  byte  (  "auth error:"    +    err  .  Error  ()))    //nolint
return
}


// add the auth token to the outgoing grpc context using
// the generic grpc metadata tools
ctx    :=    metadata  .  NewOutgoingContext  (
r  .  Context  (),
metadata  .  New  (
map  [  string  ]  string  {
"jwt"  :    token  .  Raw  ,
},
),
)


```


This handler is a tiny bit more complicated. In order, it does the following:


- Check the url query params for a` preferredName` field, defaulting to “my friend” if not found.
- Get the token from HTTP headers to require auth for the endpoint **and** to get the token to pass to the backend.
- Adds the valid token to a new gRPC metadata and injects that metadata into a new context built off the HTTP request context.


Now, still in the handler func, we can do the downstream call and return the results to the user:


```text
// make the call with the new context
resp  ,    err    :=    f  .  backendClient  .  SayHello  (  ctx  ,    &  pb  .  HelloRequest  {
Name  :    preferredName  ,
})
if    err    !=    nil    {
w  .  WriteHeader  (  http  .  StatusBadRequest  )
w  .  Write  ([]  byte  (  fmt  .  Sprintf  (  "could not greet: %v"  ,    err  )))
return
}


w  .  Write  ([]  byte  (  fmt  .  Sprintf  (  "Greeting: %s"  ,    resp  .  GetMessage  ())))
}


```


#### What we have now


A full-stack demo would look something like this:


```text
go run ./cmd/0-auth-api auth.ed  &   sleep  1
go run ./cmd/1-frontend auth.ed.pub  &   sleep  1
go run ./cmd/2-backend auth.ed.pub  &   sleep  1
sleep  1
t  =  $(  curl -s admin:pass@localhost:8081/login )
echo    "token:   $t  "
curl -s -H  "Authorization: Bearer   $t  "   localhost:8082/hello
curl -s -H  "Authorization: Bearer   $t  "   localhost:8082/hello?preferredName =  Carson


```


We run a local auth-api, frontend, and backend just as before. Then we get a new token and make a call to the frontend, which we know delegates work downstream. But we can see that the downstream does its own token validation! That keeps our architecture clean and keeps us from having “gatekeeper” services.


An example output from the test might look like this:


```text
Listening on :8081
Listening on :8082
server listening at [::]:8083
token: eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcGkiLCJleHAiOjE2NDc2NDcyNDUsImlhdCI6MTY0NzY0NzE4NSwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgxIiwibmJmIjoxNjQ3NjQ3MTg1LCJyb2xlcyI6WyJhZG1pbiIsImJhc2ljIl0sInVzZXIiOiJhZG1pbiJ9.n33-_IScorkMZF7-Sb8U3lFlkRwSfxbtf1eezQKnPBx_ddfmlRYw_xakD464G-KIvAr0KTVt2XJcd5-6ArUUBw
Greeting: Hello my friend! I am the backend. You have roles [admin basic]
Greeting: Hello Carson! I am the backend. You have roles [admin basic]


```


---


#### Next time: Eliminate toil with Middleware


Keep an eye out for Part 3, where we reduce **all** the manual token operations with HTTP server, gRPC server, and gRPC client middleware. You will be amazed at how clean and simple our service handlers have become.


## Companion Video


If you would prefer a video version of this talk you can find it on the Forge Utah YouTube channel
