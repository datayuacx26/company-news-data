---
schema_version: "1.0.0"
document_id: "7cba229da55c3aba168580b12d9fadcdd7c194d2585b2001f9fa96d20ce2c748"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/testing-golang-with-httptest/"
published_at: "2026-01-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:23:03.686122+00:00"
content_hash: "sha256:72aa915d01d44818990e48c6931668599b3683a8c84119b56b675b01541c95d4"
---

# Testing Golang with httptest Best Practices

[Go](https://go.dev/) , often referred to as *Golang* , is a popular programming language built by Google. Its design and structure help you write efficient, reliable, and high-performing programs. Often used for web servers and rest APIs, Go offers the same performance as other low-level languages like C++ while also making sure the language itself is easy to understand with a good development experience.


The[httptest](https://pkg.go.dev/net/http/httptest) package is a powerful tool in the Go standard library designed specifically for testing HTTP servers and clients. It simplifies the server testing process by allowing developers to create mock requests and responses, ensuring that their HTTP handlers and clients behave correctly under various conditions. By using httptest, you can simulate different scenarios without relying on external services, making your tests more reliable and easier to manage.


The httptest package was primarily built to test Golang HTTP handlers using net/http, with which it works smoothly. It can also be extended. httptest can serve as a drop-in replacement for your third-party integrations as a stub, and it can be easily adapted to your local development environment during testing.


Whether you’re testing an HTTP server listening for new incoming server requests or an HTTP client making requests to a web server, httptest provides the necessary tools to create a controlled testing environment. This package is particularly useful for testing HTTP handlers, as it allows you to construct request objects and response writers that mimic real-world interactions. With httptest, you can ensure that your web servers handle requests and generate responses as expected, leading to more robust and reliable applications.


This article provides an overview of how to use httptest to test Golang handlers in a web server or REST API and to test HTTP clients.


## What Is httptest?


As mentioned earlier, *httptest* , or net/http/httptest in full, is a standard library for writing constructive and unit tests for your handlers. It provides ways to mock requests to your HTTP handlers and eliminates the need of having to run the server. On the other hand, if you have an HTTP client that makes requests to a remote server, you can mock the server responses using something like[Speedscale](https://speedscale.com/) and test your client.


## httptest—How It Works


Before looking at the test package, it’s important to first understand how the HTTP handler package itself works and how your HTTP requests are processed.


### HTTP Handler Package


The standard library http package net/http has a client and a server interface. The server is basically a group of handlers. When you send a request to an endpoint or a server path, the handler intercepts this request, and based on the request, it returns a specific response.


### Handler Interface


Below is a simple interface of a handler (http.Handler):


```text
type   Handler   interface   {
ServeHTTP  (  ResponseWriter  ,   *  Request  )
}
```


The ServeHTTP takes in ResponseWriter and Request. The Request object holds the incoming HTTP request from the client, and the ResponseWriter can be used to create a response.


### Example Handler


Here is an example of a simple handler:


```text
// With no ServeHTTP
func   handler  (  w   http  .  ResponseWriter  ,   r   *  http  .  Request  ) {
w.  Write  ([]  byte  (  "Hello, World!"  ))
}


// With ServeHTTP
type   home   struct   {}
func   (  h   *  home  )   ServeHTTP  (  w   http  .  ResponseWriter  ,   r   *  http  .  Request  ) {
w.  Write  ([]  byte  (  "Hello, World!"  ))
}
```


The first method is more common than the second. It’s less confusing and clearer to declare your handler as a function. When the handler takes a HTTP request, you can compose a HTTP response after carrying out whatever needs to be done using the ResponseWriter interface. This process can either be reading from the database, third-party services, static server data, or processing a file. In the above code, the line w.Write(\[\]byte(“Hello, World!”)) sends the response.


### Test Golang HTTP Handlers with httptest


Even though the handlers are just functions, you can’t write[unit tests](https://www.digitalocean.com/community/tutorials/how-to-write-unit-tests-in-go-using-go-test-and-the-testing-package) for them the usual way. The hindrance comes from the parameters of the handler function with types ResponseWriter and Request. These are constructed automatically by the http library when a request is received by the server. So how do you construct a ResponseWriter and Request object to test the handler? This is where httptest comes in. httptest has two methods: NewRequest and NewRecorder, which help simplify the process of running tests against your handlers. NewRequest mocks a request that would be used to serve your handler. NewRecorder is a drop-in replacement for ResponseWriter and is used to process and compare the HTTP response with the expected output. Here’s a simple example of the httptest methods, an instruction to print the HTTP response status code:


```text
import   (
"  fmt  "
"  net/http  "
"  net/http/httptest  "
)


func   handler  (  w   http  .  ResponseWriter  ,   r   *  http  .  Request  ) {
w.  Write  ([]  byte  (  "Hello, Worldn"  ))
}
func   main  () {
req   :=   httptest.  NewRequest  (  "GET"  ,   "https://google.com"  ,   nil  )
w   :=   httptest.  NewRecorder  ()
handler  (w, req)
resp   :=   w.  Result  ()
fmt.  Println  (resp.StatusCode)
}
```


### Line by line breakdown


The line below allows you to create a new mock request for your server. You are passing it to your handler as your request object. Details like the request method or query parameters are specified here:


```text
req   :=   httptest.  NewRequest  (  "GET"  ,   "https://google.com"  ,   nil  )
```


The next line is your ResponseWriter interface and records all the responses from the handler:


```text
w   :=   httptest.  NewRecorder  ()
```


You can now use these variables to call the handler function:


handler(w, req)


Once the request has been fulfilled, these lines let you see the results and the HTTP response details of the request:


```text
resp   :=   w.  Result  ()
fmt.  Println  (resp.StatusCode)
```


### Full Example


Let’s now build a complete example. First, create a new Go project and create a file named server.go:


```text
package   main


import   (
"  fmt  "
"  log  "
"  net/http  "
"  net/url  "
)


func   RequestHandler  (  w   http  .  ResponseWriter  ,   r   *  http  .  Request  ) {
query, err   :=   url.  ParseQuery  (r.URL.RawQuery)
if   err   !=   nil   {
w.  WriteHeader  (http.StatusBadRequest)
fmt.  Fprintf  (w,   "Bad request"  )
return
}
name   :=   query.  Get  (  "name"  )
if   len  (name)   ==   0   {
w.  WriteHeader  (http.StatusBadRequest)
fmt.  Fprintf  (w,   "You must supply a name"  )
return
}
w.  WriteHeader  (http.StatusOK)
fmt.  Fprintf  (w,   "Hello   %s  "  , name)
}
func   main  () {
http.  HandleFunc  (  "/greet"  , RequestHandler)
log.  Fatal  (http.  ListenAndServe  (  ":3030"  ,   nil  ))
}
```


The above code creates a handler that returns a greetings message. The name query parameter is used to create a greeting, which is returned as a response. Run the code and send a request to[http://localhost:3030/greet](http://localhost:3030/greet) with a name parameter, for example,[http://localhost:3030/greet?name=john](http://localhost:3030/greet?name=john) to see the response.


### Testing our example


To test this server, create a file server_test.go and add the following code:


```text
package   main


import   (
"  io/ioutil  "
"  net/http  "
"  net/http/httptest  "
"  testing  "
)


func   TestRequestHandler  (  t   *  testing  .  T  ) {
expected   :=   "Hello john"
req   :=   httptest.  NewRequest  (http.MethodGet,   "/greet?name=john"  ,   nil  )
w   :=   httptest.  NewRecorder  ()
RequestHandler  (w, req)
res   :=   w.  Result  ()
defer   res.Body.  Close  ()
data, err   :=   ioutil.  ReadAll  (res.Body)
if   err   !=   nil   {
t.  Errorf  (  "Error:   %v  "  , err)
}
if   string  (data)   !=   expected {
t.  Errorf  (  "Expected Hello john but got   %v  "  ,   string  (data))
}
}
```


The expected variable holds the expected response of the server. The NewRequest method creates a[mock](https://speedscale.com/blog/mock-services-top-use-cases/) request to /greet with a name parameter. The handler then responds with the appropriate data, which is then validated against the expected value. Run the test with go test, and you should see it pass.


## Test Golang HTTP Clients with httptest


Another important use case of httptest is to[test HTTP clients](https://speedscale.com/blog/how-to-test-http-2-apis/) . Whereas HTTP servers intake requests and churn out a response, HTTP clients sit on the other end, making requests to a HTTP server and accepting responses from it. Testing client code is trickier since they depend on an external HTTP server. Imagine a scenario where your client makes a request to a third-party service, and you wish to test your client against all types of responses returned by the third-party service; however, it’s not in your control to decide how the third-party service will respond. This is where the NewServer function of httptest comes into play.


### Testing clients with NewServer


The NewServer method creates a new server that mocks the response you want. You can use it to mimic the response of a third-party system for end-to-end HTTP tests. Let’s see an example in action.


### Installing dependencies


Create a new Go project and install the required dependencies:


mkdir httptest-client && cd httptest-client go init example/user/httptest go get github.com/pkg/errors


Create a file called client.go. Here, you’ll define the client:


```text
package   main


import   (   "  io/ioutil  "   "  net/http  "   "  fmt  "   "  github.com/pkg/errors  "   )


type   Client   struct   { url   string   }


func   NewClient  (  url   string  )   Client   {   return   Client  {url} }


func   (  c   Client  )   MakeRequest  () (  string  ,   error  ) {
res, err   :=   http.  Get  (c.url   +   "/users"  )
if   err   !=   nil   {
return   ""  , errors.  Wrap  (err,   "An error occured while making the request"  )
}
defer   res.Body.  Close  () out, err   :=   ioutil.  ReadAll  (res.Body)
if   err   !=   nil   {
return   ""  , errors.  Wrap  (err,   "An error occured when reading the response"  )
}
return   string  (out),   nil
}


func   main  () {
client   :=   NewClient  (  "https://gorest.co.in/public/v2/"  )
resp, err   :=   client.  MakeRequest  ()
if   err   !=   nil   {
panic  (err)
}
fmt.  Println  (resp)
}
```


The client simply makes an API call to[https://gorest.co.in/public/v2/users](https://gorest.co.in/public/v2/users) , which returns some data. The response is then printed to the console. To test the client, create a file called client_test.go:


```text
package   main


import   (   "  fmt  "   "  net/http  "   "  net/http/httptest  "   "  strings  "   "  testing  "   )


func   TestClientUpperCase  (  t   *  testing  .  T  ) {
// Dummy JSON response expected := "{'data': 'dummy'}"
svr   :=   httptest.  NewServer  (http.  HandlerFunc  (  func  (  w   http  .  ResponseWriter  ,   r   *  http  .  Request  ) {
fmt.  Fprintf  (w, expected) }))
defer   svr.  Close  () c   :=   NewClient  (svr.URL) res, err   :=   c.  MakeRequest  ()
if   err   !=   nil   {
t.  Errorf  (  "expected err to be nil got   %v  "  , err)
}
res   =   strings.  TrimSpace  (res)
if   res   !=   expected {
t.  Errorf  (  "expected res to be   %s   got   %s  "  , expected, res)
}
}
```


### Explanation


Here, the expected variable holds the expected result that the client must return. In this case, the client returns whatever it gets from the new server intact, but you might have some processing done before returning the data. The next line creates the[mock server](https://speedscale.com/blog/mockserver-https-apis/) with a handler that returns the expected result:


```text
svr   :=   httptest.  NewServer  (http.  HandlerFunc  (  func  (  w   http  .  ResponseWriter  ,   r   *  http  .  Request  ) { fmt.  Fprintf  (w, expected) }))
```


You can change the HTTP response value here to test the client against different responses. Remember to change the expected value as well. Finally, a client is constructed against this mock server, and the request is made:


```text
c   :=   NewClient  (svr.URL)
res, err   :=   c.  MakeRequest  ()
```


Run the test with go test to see the results.


## Writing Effective Tests


Writing effective tests is crucial for ensuring the reliability and correctness of your web application. When testing HTTP handlers and clients, it’s essential to consider the following best practices:


- **Keep Tests Concise and Focused** : Each test should focus on a specific piece of functionality. This makes it easier to identify issues and understand what the test is verifying.
- **Use Descriptive Names** : Descriptive test names help you quickly understand what each test is checking. This is especially important when you have a large suite of tests.
- **Mock Requests and Responses** : Use the httptest package to mock requests and responses. This allows you to test your code in isolation, without relying on external services. For example, you can use httptest.NewRequest to create a mock request and httptest.NewRecorder to capture the response.
- **Test Happy Paths and Error Scenarios** : Ensure that your tests cover both successful outcomes and error conditions. This helps verify that your code handles unexpected situations correctly, such as invalid input or server errors.


By following these best practices, you can write effective tests for your HTTP handlers and clients, ensuring that your web application is reliable, correct, and maintainable.


## Best Practices


### Populate a Context for Tests


When testing HTTP handlers that expect data to be passed through a context.Context, it’s essential to create and populate a context with the necessary data for tests. This can include authentication tokens, user information, or any other data that the handler might need to perform its tasks. To populate a context for tests, you can use the context.WithValue() function to add values to the context. These values can then be retrieved in the handler using the context.Value() function. For example: ctx := context.WithValue(context.Background(), “userID”, “12345”) req := httptest.NewRequest(“GET”, “/path”, nil).WithContext(ctx) This approach ensures that your HTTP handlers have access to the required context data during tests, allowing you to verify their behavior accurately.


### Don’t Forget to Mock Database Calls


Mocking database calls is crucial for testing HTTP handlers that interact with a database. It allows you to simulate database interactions without needing an actual database, making your tests faster, more reliable, and easier to set up and tear down. To mock database calls, you can use a mocking library such as testify or[gomock](https://speedscale.com/blog/getting-started-gomock/) . These libraries provide tools for creating mock objects that can be used to simulate database interactions. For example, you can create a mock database client and define expected behaviors for specific queries: mockDB := new(MockDB) mockDB.On(“GetUser”, “12345”).Return(User{Name: “John Doe”}, nil) By following these best practices and using the httptest package, you can write effective tests for your HTTP handlers and clients, ensuring that your web application is reliable, correct, and maintainable.


## Conclusion


Unit tests are crucial for validating the proper working of any application. Testing HTTP servers or clients is tricky because of the dependence on external services. To test a web server, you need a client and vice versa. The httptest package solves this problem by mocking requests and responses. The lightweight and fast nature of httptest makes it an easy way to test Golang code with end-to-end HTTP tests. This article showed you how to use httptest to test your API handlers and HTTP clients.
