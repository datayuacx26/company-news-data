---
schema_version: "1.0.0"
document_id: "5c3ff59ea02c2f9e15bd20999af70c79c48ca71953ee6f7c45074ff2e0222187"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/when-to-use-mocks-vs-stubs"
published_at: "2025-01-08T09:00:00+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:9855977274e61f51df6b274c8ee87d031756b124e060540a4f18bd5bfc48a0ab"
---

# When To Use Mocks vs. Stubs

Common feeling of confusion when figuring out whether to use a mock or stub


One of the more common misnomers when it comes to unit testing is the use of the term “mock."


Software engineers are often imprecise with its definition thinking that that the difference is trivial. In our experience, we've found that many engineers are really referring to a stub when they say that they’ve mocked an object.


So, what’s the difference between a stub and a mock? And why is it important?


## Same Same, But Different


To start off, both mocks and stubs are both forms of **test doubles** , which are pretend objects that substitute for a real object for the sake of automated testing.


I like to use[Martin Fowler’s definitions](https://martinfowler.com/articles/mocksArentStubs.html#TheDifferenceBetweenMocksAndStubs) for the terms:


- Mocks are doubles “pre-programmed with expectations which form a specification of the calls they are expected to receive.”
- Stubs are doubles that “provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.”


Mocks use **behavior verification** . They record and validate interactions between the object and its collaborators, which means we can track call frequency and raise exceptions for unexpected calls.


Stubs use **state verification** . They provide controlled inputs to simulate dependencies, so we can determine whether the exercised method worked correctly by examining the outcome.


## Scenario: Function That Sends an Email


To make this more concrete, here’s an example scenario to demonstrate when and why you should choose one over the other.


Suppose we have a` EmailNotifier` class:


```text
class    EmailNotifier  :
def    __init__  (  self ,   email_service )  :
self .  email_service  =   email_service


def    send_welcome_email  (  self ,   user_email ,   user_name )  :
if    not   user_email  or    not   user_name :
raise   ValueError (  "Email and name are required"  )


email_content  =    {
"to"  :   user_email ,
"subject"  :    "Welcome to our platform!"  ,
"body"  :    f"Hi   {  user_name }   ,\n\nWelcome to our platform! We're excited to have you here."
}


try  :
result  =   self .  email_service .  send_email (  **  email_content )
return    {
"success"  :    True  ,
"message_id"  :   result .  message_id ,
"sent_at"  :   result .  timestamp
}
except   Exception  as   e :
return    {
"success"  :    False  ,
"error"  :    str  (  e )
}


```


Now let's say we want to test the email sending functionality. We have two approaches at hand.


### Approach 1: Using a Stub


A stub purely pretends to be the real` send_welcome_email` method without checking how it’s called. All we care about is **returning data** .


```text
class    EmailServiceStub  :
def    send_email  (  self ,   to ,   subject ,   body )  :
# Simply return a successful response without actually sending email
return    type  (  'EmailResult'  ,    (  )  ,    {
'message_id'  :    'stub-msg-123'  ,
'timestamp'  :    '2025-01-09T10:00:00Z'
}  )


def    test_with_stub  (  )  :
stub_email_service  =   EmailServiceStub (  )
notifier  =   EmailNotifier (  stub_email_service )


result  =   notifier .  send_welcome_email (
"user@example.com"  ,
"Jane Doe"
)


# Only verify the result, not the interaction
assert   result [  "success"  ]    ==    True
assert   result [  "message_id"  ]    ==    "stub-msg-123"


```


- Simply **returns a successful result** without any real email sending
- Good for simple tests where you just want to verify the success path
- Doesn't verify how the service was used


### Approach 2: Using a Mock


A mock both pretends to be the real object **and** lets us verify interactions (e.g., call count, arguments).


```text
# Mock example using unittest.mock
from   unittest .  mock  import   Mock
import   pytest
from   datetime  import   datetime


def    test_welcome_email_with_mock  (  )  :
# Create the mock
mock_email_service  =   Mock (  )


# Configure mock to return a successful response
mock_result  =    type  (  'EmailResult'  ,    (  )  ,    {
'message_id'  :    'mock-msg-456'  ,
'timestamp'  :    '2025-01-09T10:00:00Z'
}  )
mock_email_service .  send_email .  return_value  =   mock_result


# Create notifier with mock
notifier  =   EmailNotifier (  mock_email_service )


# Test sending welcome email
result  =   notifier .  send_welcome_email (
"user@example.com"  ,
"John Doe"
)


# Verify the mock was called with correct arguments
mock_email_service .  send_email .  assert_called_once_with (
to =  "user@example.com"  ,
subject =  "Welcome to our platform!"  ,
body =  "Hi John Doe,\n\nWelcome to our platform! We're excited to have you here."
)


# Verify the result
assert   result [  "success"  ]    ==    True
assert   result [  "message_id"  ]    ==    "mock-msg-456"


```


Additionally, we can test error handling with a mock:


```text
def    test_email_failure_with_mock  (  )  :
mock_email_service  =   Mock (  )
mock_email_service .  send_email .  side_effect  =   Exception (  "SMTP server error"  )


notifier  =   EmailNotifier (  mock_email_service )
result  =   notifier .  send_welcome_email (  "user@example.com"  ,    "John Doe"  )


assert   result [  "success"  ]    ==    False
assert   result [  "error"  ]    ==    "SMTP server error"


```


- Verifies the **exact content** of the email being sent
- Checks that the subject line, body, and recipient are correct
- Can **simulate failures** to test error handling
- Allows verification of the exact interactions


## Why It Matters


The difference between mocks and stubs may seem trivial but there is usually a better approach between the two in any given scenario.


1. **Use stubs** if you only care about controlling inputs or outcomes. This keeps tests simpler and more focused on business logic.
2. **Use mocks** when you need to confirm an external function was called a certain number of times or with specific parameters.


Avoid over-mocking as a general guideline. Too many mocks can make your tests fragile, especially when your implementation changes but your overall outcome remains the same.


Stubs often give you the flexibility to refactor code without rewriting tons of tests, which is why they may be preferred for features that are still in flux. Mocks are better suited for mature, business-critical features.


---


Hopefully, this clears the air when it comes to the subtle differences between the types of test doubles that you have at your disposal. Use mocks and stubs thoughtfully and your engineering team will thank you.


At[Tusk](https://usetusk.ai/) , we make sure that our **AI test generation agent** consistently follow these guidelines when we generate unit tests that involve test doubles.


If you’d like to see how our agent figures out whether to mock or stub,[apply for access](https://tally.so/r/nP29pQ) and we’ll get you on a demo.
