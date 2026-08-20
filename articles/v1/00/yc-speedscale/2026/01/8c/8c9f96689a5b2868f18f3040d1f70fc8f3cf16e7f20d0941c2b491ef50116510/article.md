---
schema_version: "1.0.0"
document_id: "8c9f96689a5b2868f18f3040d1f70fc8f3cf16e7f20d0941c2b491ef50116510"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/getting-started-gomock/"
published_at: "2026-01-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:22:56.687510+00:00"
content_hash: "sha256:09ced313900f4f21746bc75f18833646d94c27d89d063276023f94c3bb44dddc"
---

# How to Get Started with GoMock Playbook

GoMock is a powerful tool for generating mock objects in Go, making it an essential asset for developers aiming to write advanced unit tests. By simulating the behavior of real objects, GoMock allows you to test your code in isolation, ensuring that each component functions correctly on its own. This capability is particularly useful in a language like Go, where interfaces play a crucial role in defining the behavior of different components.


With GoMock, you can create mock objects that mimic the behavior of real objects, allowing you to test how your code interacts with these objects without relying on their actual implementations. This not only speeds up the testing process but also makes your tests more reliable and easier to maintain. In the following sections, we’ll delve into the specifics of using GoMock to create and manage mock objects, starting with understanding expected calls.


By[mocking the API](https://speedscale.com/blog/mock-services-top-use-cases/) , i.e. by simulating its responses, you can validate all the functionality of your application without the performance of the real API playing a role. This post covers how to implement GoMock in your application alongside the benefits and limitations of mocks.


## What is GoMock and Why Choose It?


GoMock is a testing package provided by the Golang team. It integrates with the rest of the Go ecosystem, like[go:generate](https://eli.thegreenplace.net/2021/a-comprehensive-guide-to-go-generate) and the built-in testing package. GoMock isn’t the only[Golang testing framework](https://speedscale.com/blog/golang-testing-frameworks-for-every-type-of-test/) , but it’s the preferred tool for many because of its powerful type-safe assertions.


As you’ll see in the next section, GoMock works by generating a Mock Object from[interfaces](https://www.digitalocean.com/community/tutorials/how-to-use-interfaces-in-go) , meaning GoMock requires interfaces to function. In practical terms, generating a Mock Object is creating a new file that contains mock implementations of your interface and all the logic needed to create stubs and define mock behavior.


Choosing GoMock - or any mock package - depends on solving dependency issues in testing. While integration tests are valuable, as they test the entire chain of a request, they’re often time-consuming, complex, and/or costly. Additionally, if your mocks are properly configured and match the real dependency, you may find a decreased need to perform full integration tests in the first place.


Essentially, mocks provide unit testing ease and most of the benefits from integration testing. You get to test your source code changes in isolation while validating interoperability with other applications.


### API Mocking Tools: Our Top 8 Picks


Learn about the benefits, drawbacks, and key use cases for our top 5 API mocking tools: Postman, MockServer, GoMock, MockAPI, and Speedscale


## Prerequisites for GoMock


Before you dive into the tutorial part of this post, make sure you meet a few prerequisites.


- A basic understanding of the Go programming language


While not strictly necessary, this tutorial is more useful if you have a good grasp of Go’s structure. If not, start learning at[GoByExample](https://gobyexample.com/) .


- A system with Go installed


This tutorial is written using Go v1.20.1. Ensure you’ve installed[this version](https://go.dev/doc/install) or a later one.


## Installing GoMock


Before working with GoMock, you need to install it. Do this by running:


```text
go   install   go.uber.org/mock/mockgen@latest
```


> **Note:** GoMock has moved from` github.com/golang/mock` to` go.uber.org/mock` . The examples below use the updated import paths.


Note that this installs the mockgen tool into $GOPATH/bin. Mockgen uses code generation to create mock code. However, the mockgen tool isn’t necessary to make GoMock work because you can write mock code without the tool. However, the tool makes the process much easier.


Next, install GoMock in your Go project. First you have to initialize your project as a Go Module:


```text
mkdir   $GOPATH  /src/gomock-getting-started
cd   $GOPATH  /src/gomock-getting-started
go   mod   init   gomock-getting-started
```


Start adding any necessary modules to your project, like GoMock:


```text
go   get   go.uber.org/mock/gomock
```


By now, you’ve installed everything you need, and you can start creating mock code to test your application. To do so in this tutorial, you need to create an application first.


## Creating Your First Mock with GoMock


Keeping with the example from the introduction, let’s make a simple program that resembles an API interacting with a payment processor. To do so, create a main.go file that implements an interface and two structs.


NOTE: Source code can be found[in this GitHub repo](https://github.com/KSiig/gomock-getting-started) .


```text
$ cat   <<  EOF   >   main.  go
package   main


import   (
"  net/http  "
"  bytes  "
"  encoding/json  "
"  errors  "
)


type   PaymentProcessor   interface   {
Charge  (  amount   float64  ,   token   string  )   error
}


type   StripePaymentProcessor   struct  {}


func   (  s   *  StripePaymentProcessor  )   Charge  (  amount   float64  ,   token   string  )   error   {
// charge the credit card
data   :=   map  [  string  ]  string  {}
json_data, err   :=   json.  Marshal  (data)


resp, err   :=   http.  Post  (  "https://api.stripe.com/v1/charges"  ,   ""  , bytes.  NewBuffer  (json_data))


if   err   !=   nil   {
return   err
}


var   res   map  [  string  ]  interface  {}
json.  NewDecoder  (resp.Body).  Decode  (  &  res)


return   nil
}


type   PaymentProcessorClient   struct   {
PaymentProcessor   PaymentProcessor
}


func   (  c   *  PaymentProcessorClient  )   Charge  (  amount   float64  ,   token   string  )   error   {
if   amount   <   20   {
return   errors.  New  (  "Charge too low"  )
}
return   c.PaymentProcessor.  Charge  (amount, token)
}
EOF
```


Understanding the specifics of this source code line isn’t necessary, although you can read the inline comments for more info. But you need to understand that there’s a PaymentProcessor interface that defines a Charge function. Then, the StripePaymentProcessor implements this interface and interacts with the Stripe API. The function body mainly showcases the typical flow of such requests. But you don’t need it in this tutorial because that isn’t what you’ll be[mocking.](https://speedscale.com/blog/service-virtualization-vs-mocking/)


After this, create a PaymentProcessorClient struct that uses the PaymentProcessor interface, as shown in the Charge function, where it’s passing the function parameters to the interface implementation.


The principle behind this code is that the PaymentProcessorClient can interact with any payment processor. In this case, it’s Stripe, but you can easily swap it because it’s using an interface.


Now that you’ve defined a simple application with an interface, you can generate your first Mock Object.


### Creating a Mock Object


After defining your interfaces, use the mockgen tool to generate your Mock Object by defining the interfaces’ import path and the destination’s import path to where Mock Objects from code generation should live. If no destination is specified, mocks will print to standard output. Although mockgen supports other modes like reflect mode, let’s use source mode to mock from a source file (differences between source mode and reflect mode can be found in the[GitHub’s readme](https://github.com/uber-go/mock?tab=readme-ov-file#running-mockgen) ).


```text
mockgen   -destination=mocks/mocks.go   -source=main.go
```


If you look at source mode’s mock implementations in mocks/mocks.go, you’ll see that it’s created a number of structs. You don’t need to understand all the code, but here’s what structs are generated into a destination file or standard output.


```text
// Code generated by MockGen. DO NOT EDIT.
// Source: main.go


// Package mock_main is a generated GoMock package.
package   mock_main


import   (
reflect   "  reflect  "


gomock   "  go.uber.org/mock/gomock  "
)


// MockPaymentProcessor is a mock of PaymentProcessor interface.
type   MockPaymentProcessor   struct   {
ctrl       *  gomock  .  Controller
recorder   *  MockPaymentProcessorMockRecorder
}


// MockPaymentProcessorMockRecorder is the mock recorder for MockPaymentProcessor.
type   MockPaymentProcessorMockRecorder   struct   {
mock   *  MockPaymentProcessor
}


// NewMockPaymentProcessor creates a new mock instance.
func   NewMockPaymentProcessor  (  ctrl   *  gomock  .  Controller  )   *  MockPaymentProcessor   {
mock   :=   &  MockPaymentProcessor  {ctrl: ctrl}
mock.recorder   =   &  MockPaymentProcessorMockRecorder  {mock}
return   mock
}


// EXPECT returns an object that allows the caller to indicate expected use.
func   (  m   *  MockPaymentProcessor  )   EXPECT  ()   *  MockPaymentProcessorMockRecorder   {
return   m.recorder
}


// Charge mocks base method.
func   (  m   *  MockPaymentProcessor  )   Charge  (  amount   float64  ,   token   string  )   error   {
m.ctrl.T.  Helper  ()
ret   :=   m.ctrl.  Call  (m,   "Charge"  , amount, token)
ret0, _   :=   ret[  0  ].(  error  )
return   ret0
}


// Charge indicates an expected call of Charge.
func   (  mr   *  MockPaymentProcessorMockRecorder  )   Charge  (  amount  ,   token   interface  {})   *  gomock  .  Call   {
mr.mock.ctrl.T.  Helper  ()
return   mr.mock.ctrl.  RecordCallWithMethodType  (mr.mock,   "Charge"  , reflect.  TypeOf  ((  *  MockPaymentProcessor  )(  nil  ).Charge), amount, token)
}
```


MockPaymentProcessor is the main struct that makes GoMock work because it contains the controller and recorder for your tests. This is GoMock’s brain, as you’ll see in the implementation shortly. To view the description of each part of the Mock Object, use the inline comments.


### Configuring GoMock


By now, you’re ready to start using mocks inside your tests. Start by creating a main_test.go in your root directory with all the boilerplate code needed:


```text
$ cat   <<  EOF   >   main_test.  go
package   main_test


import   (
"  fmt  "
"  testing  "
"  gomock-getting-started  "
"  gomock-getting-started/mocks  "
"  go.uber.org/mock/gomock  "
"  errors  "
)


func   TestCharge  (  t   *  testing  .  T  ) {
}
EOF
```


Note that it’s important to save your project to $GOPATH/src because Go uses the path for local imports. It’s why “gomock-getting-started” and “gomock-getting-started/mocks” works and placing your code anywhere else will result in an error.


With the boilerplate code in place, start building the test. Start the test by defining the mock controller and use it to generate the mock PaymentProcessor and the test PaymentProcessorClient, which you’ll use to test the Charge()function:


```text
func   TestCharge  (  t   *  testing  .  T  ) {
mockCtrl   :=   gomock.  NewController  (t)
mockPaymentProcessor   :=   mock_main.  NewMockPaymentProcessor  (mockCtrl)
}
```


The next step is ensuring that GoMock verifies any expectations you set during mock behavior configuration, like the number of times a given function needs to be called:


```text
testPaymentProcessorClient   :=   &  main  .  PaymentProcessorClient  { PaymentProcessor: mockPaymentProcessor }
defer   mockCtrl.  Finish  ()
```


Define the behavior of the Mock Object before writing the tests for the PaymentProcessorClient’s Charge() function. Although GoMock and mockgen are smart, they can’t analyze your code to generate responses. This is why you should analyze your code as part of your test. Do this using the EXPECT() function:


```text
defer   mockCtrl.  Finish  ()
mockPaymentProcessor.  EXPECT  ().  Charge  (  100.0  ,   "test_token"  ).  Return  (  nil  ).  Times  (  1  )
```


Here, the` EXPECT()` call is immediately followed by the.Charge() call where you define the parameters the mock implementation of the PaymentProcessor’s Charge()function should accept. Your test will fail if any of the test code makes calls to the Charge() function not matching the parameters you’ve defined.


To avoid hard coding you can replace parameters with gomock.Any(). However, where possible, use specific parameters to make your tests less ambiguous.


The last two functions Return(nil) and Times(1) define that a call to Charge(100.0, “test_token”) should return nil. But the call has to be used once during the Mock Object lifetime. Use AnyTimes() to avoid specifics about the number of times you call a function, although this should be avoided when possible to reduce ambiguity. Now that the Mock Object is fully configured, start writing your test. Remember to use the Finish method to ensure that all the methods intended to be called were indeed invoked. This is crucial for the integrity of test results.


### Understanding Expected Calls


In GoMock, an expected call is a predefined interaction with a mock object that you anticipate occurring during your test’s execution. These expected calls are crucial for verifying that your code behaves as intended when interacting with the mock object. To define an expected call, you use the EXPECT method on the mock object.


The EXPECT method allows you to specify the function that should be called, along with the expected arguments and return values. By setting these expectations, you can ensure that your test will fail if the mock object is not used as expected. This helps you catch errors early and provides clear feedback on what went wrong. In the next section, we’ll explore how to use argument matchers to define the expected arguments for these calls.


### Using Argument Matchers


Argument matchers in GoMock are used to specify the expected arguments for a mocked method call. These matchers allow you to define a range of acceptable values, making your tests more flexible and robust. GoMock provides several pre-defined matchers, including eq (equal), ne (not equal), lt (less than), le (less than or equal), gt (greater than), ge (greater than or equal), contains, and matches.


For example, you can use gomock.Eq(100.0) to specify that the argument must be exactly 100.0, or gomock.Any() to indicate that any value is acceptable. Additionally, you can create custom matchers by implementing the Matcher interface, allowing you to define more complex matching logic tailored to your specific needs. This flexibility ensures that your tests can accurately reflect the expected behavior of your code.


### Writing Tests With the Mock Object


To call the function in your test is like calling it anywhere else in your codebase:


```text
err   :=   testPaymentProcessorClient.  Charge  (  100.0  ,   "test_token"  )
```


With the function having been called, validate the return value of the function:


```text
err   :=   testPaymentProcessorClient.  Charge  (  100.0  ,   "test_token"  )
if   err   !=   nil   {
t.  Fail  ()
}
```


This is a simple test case like other tests you’d write in Go. If there’s an error, the test fails. If not, it passes. If you look at the main.go file, you’ll see that the PaymentProcessor.Charge() function also contained logic that validates the charge amount not being below 20. Let’s write a test for that.


```text
err   =   testPaymentProcessorClient.  Charge  (  10.0  ,   "test_token"  )


if   err.  Error  ()   !=   "Charge too low"   {
fmt.  Println  (fmt.  Errorf  (  "Error:   %w  "  , err))
t.  Fail  ()
}
```


Here, you see the same err variable being used as before, but the Charge amount has been changed to 10.0. Then, the code is verifying that the error response matches what’s been defined in the function implementation in main.go. Now, verify that both tests are working as expected by running:


```text
go   test
PASS
ok        gomock-getting-started    0.003s
```


The Mock works as expected, and you’ve successfully verified the implementation of Charge() without using the actual Stripe API. A clear indicator is getting no errors, athough the call to Stripe defined in main.go is not a proper implementation.


You can also verify this by modifying the mocks to purposefully fail:


```text
// mockPaymentProcessor.EXPECT().Charge(10.0, "test_token").Return(errors.New("Error in Stripe")).AnyTimes()
mockPaymentProcessor.  EXPECT  ().  Charge  (  30.0  ,   "test_token"  ).  Return  (errors.  New  (  "Error in Stripe"  )).  AnyTimes  ()
// err = testPaymentProcessorClient.Charge(10.0, "test_token")
err   =   testPaymentProcessorClient.  Charge  (  30.0  ,   "test_token"  )
```


This time the test should fail on differing return values:


```text
go   test
---   FAIL:   TestCharge   (0.00s)
main_test.go:44:   Error   returned   was:   Error   in   Stripe
FAIL
exit   status   1
FAIL      gomock-getting-started    0.003s
```


This is a simple example of how to use GoMock in your tests; however, it’s far from a comprehensive overview. The official[GoMock README](https://github.com/uber-go/mock) is a great place to get started if you want to learn more.


### Specifying Mock Actions


Mock objects in GoMock can be enhanced with actions using the Do method. Actions allow you to specify complex assertions about the call arguments and the behavior of the mock object. For instance, you can use actions to modify the return values based on the input arguments or to perform additional checks during the call.


The Do method is particularly useful when you need to simulate more intricate interactions with the mock object. By using actions, you can ensure that your mock object behaves in a way that closely mirrors the real object, providing more accurate and meaningful test results. This capability is essential for testing scenarios where the behavior of the mock object depends on the specific arguments passed to it.


### Enforcing call order


Since mocked method calls can run in any order, we can enforce varied call order dependencies. The following enforce equivalent call order dependencies:


```text
firstCall   :=   mockObj.  EXPECT  ().  SomeMethod  (  1  ,   "first"  )
secondCall   :=   mockObj.  EXPECT  ().  SomeMethod  (  2  ,   "second"  ).  After  (firstCall)
mockObj.  EXPECT  ().  SomeMethod  (  3  ,   "third"  ).  After  (secondCall)
gomock.  InOrder  (
mockObj.  EXPECT  ().  SomeMethod  (  1  ,   "first"  ),
mockObj.  EXPECT  ().  SomeMethod  (  2  ,   "second"  ),
mockObj.  EXPECT  ().  SomeMethod  (  3  ,   "third"  ),
```


With equivalent call order dependencies, our once varied call order dependencies are now closer to source code.


### Modifying Failure Messages


When a test fails, GoMock provides a detailed failure message that includes the expected call and the actual call that was made. These failure messages are invaluable for debugging, as they help you quickly identify what went wrong. However, there may be times when you need to customize these messages to provide additional context or clarity.


You can modify the failure messages using the String method on a mock object. The String method allows you to return a custom string that describes the expected call, making it easier to understand the cause of the test failure. By tailoring the failure messages to your specific needs, you can streamline the debugging process and ensure that your tests provide the most useful feedback possible.


By understanding and utilizing these features of GoMock, you can create more effective and reliable unit tests for your Go applications. Whether you’re defining expected calls, using argument matchers, specifying mock actions, or modifying failure messages, GoMock provides the tools you need to ensure that your code behaves as expected.


## Limitations of GoMock


As seen in this post, using GoMock isn’t too complex. But consider some of the limitations before integrating it into your infrastructure.


### Only works with Go


You’re unlikely to have issues if your organization only uses Go. But many organizations are switching to microservices architecture that use different programming languages. In this case—or cases of uncertainties about the future use of Go—consider using a language-agnostic tool instead.


### Sits directly in your codebase


A GoMock living as code directly in your codebase is positive and negative. It allows easy structure and access to writing tests.


But the inability to create a central system for tests is a possible drawback. For example, deciding whether to use a new version of GoMock organization-wide is time-consumning, depending on the number of individual projects.


### Mostly suited for unit testing


Although it’s the core idea behind GoMock, you must know it’s only suited for unit testing, not integration and end-to-end testing. It means implementing other types of testing inherently involves adding other tools.


### Only works with interfaces


If you’re using mockgen for code generation, then you’re limited to mocking interfaces. The upside is that it encourages the use of interfaces, which is arguably a good development paradigm. But it’s a limitation you should consider.


## Conclusion


Overall, GoMock is a great tool for unit testing in Go. However, consider using[user traffic to build a mock server](https://speedscale.com/blog/mock-services-in-software-development/) , which addresses all the limitations above. Irrespective of the type of testing you’re doing, you’ll gain[troves of advantages](https://speedscale.com/blog/mock-services-top-use-cases/) by using mocks.
