---
schema_version: "1.0.0"
document_id: "5dba32c5e888e9ccd0ef38e4b7871b062d4a74565030136aa918c8c95692f274"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/neuralception/python-better-unittest-pytest"
published_at: "2023-10-07T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:782844375a318e79c14932960b17e9ff7f550533e0c2ea0209b1e6688ff84006"
---

# Better unit tests with pytest-mock and monkeypatch

## Introduction


Whenever we make changes to a system, it's essential to ensure that these alterations don't disrupt existing functionality and that they perform as intended. This underscores the critical role of testing in software development. Tests serve a dual purpose; not only do they validate the behaviour of the code, but they also serve as documentation, providing insights into how methods and classes are used. This documentation aspect is particularly valuable for fostering collaboration among developers and ensuring that your future self can understand and maintain the code.


One widely adopted testing framework for Python is` pytest` , which streamlines the process of creating and executing tests.


This post explores how unit tests can be written with pytest and its extension,` pytest-mock` , across various scenarios and levels of complexity. We begin with foundational tests and subsequently explore the concepts of patching and mocking. By understanding these concepts, we can elevate the quality of our unit tests and bolster the robustness of our code.


Please note that there are different ways to write tests in Python and that there are no clear guidelines on which tool to use for what. That is what motivated me to write this post, to document how I write tests in different scenarios.


You can find the code[here](https://github.com/daved01/python-tutorials) if you want to follow along.


## Basics of a Test


Before delving into the specifics of unit testing with Python, let's take a moment to revisit the fundamental concepts of testing and its inherent structure. While software testing encompasses a broad spectrum of practices, this post focuses on unit testing, which targets individual units of code.


A typical test comprises four distinct phases:


**Arrange -** Preparation for the test, for example instantiating required objects.


**Act -** Perform the state-changing action for the test, for example calling the function we want to test.


**Assert -** Check the result of the state-changing action, for example the output of a function or the value of an attribute.


**Cleanup -** Removal of remains from the test so other tests are not influenced.


For more information see[here](https://docs.pytest.org/en/7.1.x/explanation/anatomy.html#test-anatomy) . Now, let's write some tests.


## Pytest Basic Test


We begin with a basic test. We want to test the method` double_pos_int()` from the` FirstClass` class.


```text
def     double_pos_int  (  self  ,   pos_value  :     int  )     -  >     int  :
if   pos_value   <=     0  :
raise   RuntimeError  (  "Pos value should be greater 0."  )
return   pos_value   *     2
```


Because it takes and returns an integer it can be tested by calling it with integers and checking the outputs. To create a test in` pytest` , we write a function in the form` test_*.py` or` *_test.py` . Additionally, I like to group related tests in classes like I did in the example in` TestExampleBasic` . That said, let's look at our first test.


```text
def     test_double_pos_val_pass  (  self  )  :
first_class   =   FirstClass  (  )
res   =   first_class  .  double_pos_int  (  2  )
assert   res   ==     4
```


Here, we simply assert the output in the last line.


If we want to perform this test with different input values we can parametrize the test with pytest. Here, we create three tests without code duplication


```text
@pytest  .  mark  .  parametrize  (  "val_in, val_should"  ,     [  (  1  ,     2  )  ,     (  3  ,     6  )  ,     (  10  ,     20  )  ]  )
def     test_double_pos_val_pass  (  self  ,   val_in  ,   val_should  )  :
first_class   =   FirstClass  (  )
res   =   first_class  .  double_pos_int  (  val_in  )
assert   res   ==   val_should
```


### Function That Raises an Exception


Our function is supposed to work on positive integers only which is why there is a check of the input value in the first line, which raises a` RuntimeError` for invalid inputs. To test this codepath, we can call the method with an invalid value and assert that the exception was raised. To do so, we wrap the method call with` pytest.raises(<Exception>)`


```text
def     test_double_pos_val_fail  (  self  )  :
first_class   =   FirstClass  (  )
with   pytest  .  raises  (  RuntimeError  )  :
_   =   first_class  .  double_pos_int  (  -  1  )
```


This test passes if the` RuntimeError` is raised.


## Making Code Modular with Fixtures


So far we have written two tests (plus one with parametrization). In all of these test we perform the same setup. We initialize a` first_class` object in the arrange step. To make the tests more modular, we can move this into a fixture and use it in the test.


A fixture is a feature in pytest which is used to set-up and tear-down resources for tests. We create a fixture by defining a function and decorating it with` @pytest.fixture` . To use it, just pass it as an argument to your test function. Then, the code of the fixture is executed first, and then the test. It is time for an example.


```text
@pytest  .  fixture
def     setup_first_class  (  self  )  :
return   FirstClass  (  )
```


Now, we can re-write the first test like so


```text
def     test_double_pos_val_fixture_pass  (  self  ,   setup_first_class  )  :
res   =   setup_first_class  .  double_pos_int  (  2  )
assert   res   ==     4
```


This might not look like much, but if you imagine that the setup requires setting multiple attributes etc. this is very useful. Additionally, fixtures can import fixtures.


## Parametrizing Tests


We can also use fixtures in combination with parametrization. Then, we have to pass in the **name** of the fixture as string, and extract the value of the fixture in a separate step using` request.getfixturevalue()` . Here is an example


```text
@pytest  .  fixture
def     input_data  (  self  )  :
return     {  "val1"  :     1  ,     "val2"  :     2  ,     "val3"  :     3  }


@pytest  .  mark  .  parametrize  (  "input, expected"  ,     [  (  "input_data"  ,     [  1  ,     2  ,     3  ]  )  ]  )
def     test_unpack_dict  (  self  ,   setup_first_class  ,   request  ,     input  ,   expected  )  :
input_dict   =   request  .  getfixturevalue  (  input  )
res   =   setup_first_class  .  unpack_dict  (  input_dict  )
assert   res   ==   expected
```


Since this was a brief introduction to fixtures, it's important to note two additional aspects. First, fixtures can be configured with varying scopes, including function or class-level scopes, allowing flexibility in their application. Second, fixtures are invaluable when it comes to managing the cleanup phase of a test, a topic that is extensive enough to warrant a dedicated discussion of its own. For further insights on specifically this subject, see[yield fixtures](https://docs.pytest.org/en/7.1.x/how-to/fixtures.html#teardown-cleanup-aka-fixture-finalization) , and fixtures in general see the pytest[documentation](https://docs.pytest.org/en/7.1.x/reference/reference.html#fixtures-api) .


## Patching and Mocking


Up to this point, the methods we've examined have been independent of both internal and external functions or classes. However, real-world scenarios often involve dependencies, like network access or file operations. For instance, consider the following class:


```text
class     SecondClass  :
def     __init__  (  self  ,   url  )  :
self  .  url   =   url


def     add_to_remote_number  (  self  ,   num  :     int  ,   key  :     str  )     -  >   Optional  [  int  ]  :
"""Gets the number which corresponds to `key` from the endpoint and adds num to it."""
res   =   self  .  _get_request  (  self  .  url  )
remote_number   =   res  .  get  (  key  )
return   remote_number   +   num   if   remote_number   else     None


def     _get_request  (  self  ,   url  :     str  )     -  >     dict  [  str  ,     int  ]  :
"""Example return data: {'ten': 10, 'three': 3}"""
res   =   requests  .  get  (  url  =  url  ,   params  =  {  "key"  :     "value"  }  )
res  .  raise_for_status  (  )
return   res  .  json  (  )
```


In this scenario, our goal is to conduct a unit test for the` add_to_remote_number()` method. This method relies on the` _get_request()` method to retrieve a dictionary mapping strings to integers from a network endpoint. Subsequently, it extracts an integer from this dictionary and combines it with the integer provided as an argument to the method.


In the context of unit testing, it's important to avoid reliance on network calls due to their inherent slowness and the lack of control over external resources. To address this, we must patch the network call within the` add_to_remote_number()` method and supply a predetermined response. This enables us to assess the logic of` add_to_remote_number()` independently from the behaviour of` _get_request()` .


In this context, *patching* refers to the dynamic replacement of a method or class at runtime.


In Python there are multiple ways to achieve this. I personally like to work with` monkeypatch` and the` mocker` fixture. The latter is available through the extension` pytest-mock` , which is a wrapper around` unittest.mock` . We will see both in action below. Note that there[are no clear guidelines](https://github.com/pytest-dev/pytest/issues/4576#issuecomment-449864333) on when to use which.


It is time for some examples.


### Patching with pytest-mock


To start, let's replace the return value of` _get_request()` with a dictionary` {“three”: 3}` so that we can test` add_to_remote_number()` . The package` pytest-mock` provides us with the fixture` mocker` which we can use to accomplish that.


To use` mocker` , just pass it to your test as an argument. Then, you can call` mocker.patch()` to patch a method like so.


```text
def     test_add_to_remote_number  (  self  ,   mocker  )  :
second_class   =   SecondClass  (  "https://url.com/fake"  )
mocker  .  patch  (  "my_code.SecondClass._get_request"  ,   return_value  =  {  "three"  :     3  }  )
res   =   second_class  .  add_to_remote_number  (  5  ,     "three"  )
assert   res   ==     8
```


Note that the path to the function is the path to where you use it, not where the function is defined. For more information on which path to use and why see[here](https://docs.python.org/3/library/unittest.mock.html#id6) .


Now that we have tested` add_to_remote_number()` let's test the method we have patched before next. We can patch` request.get()` and return a fixed value. There are multiple ways to accomplish this. One way is to use` mocker.Mock()` to mock the object.


```text
def     test_get_request_mocker_with_mock  (  self  ,   mocker  )  :
mock_get   =   mocker  .  patch  (  "requests.get"  ,   return_value  =  mocker  .  Mock  (  )  )
mock_get  .  return_value  .  json  .  return_value   =     {  "a"  :     1  }
mock_get  .  return_value  .  status_code   =     200


second_class   =   SecondClass  (  "https://url.com/fake"  )
res   =   second_class  .  _get_request  (  "fake"  )


assert   res   ==     {  "a"  :     1  }
```


We can also create a mock object by ourselves like this:


```text
def     test_get_request_mocker  (  self  ,   mocker  )  :
second_class   =   SecondClass  (  "https://url.com/fake"  )


class     MockGet  :
def     __init__  (  self  )  :
pass


def     raise_for_status  (  self  )  :
pass


def     json  (  self  )  :
return     {  "a"  :     1  }


mocker  .  patch  (  "requests.get"  ,   return_value  =  MockGet  (  )  )
res   =   second_class  .  _get_request  (  "fake"  )
assert   res   ==     {  "a"  :     1  }
```


I prefer using` Mock()` over the second approach as it is more concise, but both versions are fine.


### Patching with monkeypatch


Instead of using` mocker` we can also test` add_to_remote_number()` using the built-in` monkeypatch` fixture. It is included with pytest by default, so it requires no extra installation and patches attributes safely: changes are applied only for the duration of the test, and pytest automatically restores the original behavior afterwards. This ensures that your patch does not interfere with other tests.


To use` monkeypatch` here, we define a replacement function` mock_request` that returns our desired data. Then, we use` monkeypatch.setattr` to replace the real method with our mock.


```text
def     test_add_to_remote_number_monkeypatch  (  self  ,   monkeypatch  )  :
second_class   =   SecondClass  (  "https://url.com/fake"  )


# Define the mock replacement function
def     mock_request  (  *  args  )  :
return     {  "three"  :     3  }


# Apply the patch
monkeypatch  .  setattr  (  SecondClass  ,     "_get_request"  ,   mock_request  )


res   =   second_class  .  add_to_remote_number  (  5  ,     "three"  )
assert   res   ==     8
```


So should I use` mocker` or` monkeypatch` ?


Both are safe to use. However, I personally prefer` mocker` (from` pytest-mock` ) because it often requires less boilerplate code. As seen above,` monkeypatch` required us to define a separate inner function (` mock_request()` ), whereas` mocker` allows us to configure the return value in a single line. Additionally, mocker provides extra utilities, such as tracking how many times a function was called (spying), see[documentation](https://pytest-mock.readthedocs.io/en/latest/) .


Ultimately, there is no right or wrong choice here, it comes down to preference and whether you want to install an external plugin.


## Testing File Operations


As a general practice, I tend to avoid writing files during unit tests since this can potentially impact the test's speed. Nevertheless, sometimes it is necessary.


To ensure the test maintains cleanliness, we can use the built-in` tmp_path` fixture. This fixture provides a temporary unique directory (as a` pathlib.Path` object) for each test invocation and automatically cleans it up afterwards.


```text
class     ThirdClass  :
# Omitted code


def     write_to_csv  (  self  ,   filename  :     str  ,   data  :     list  [  list  [  str  ]  ]  )     -  >     None  :
"""Writes data to a csv file."""
with     open  (  filename  ,     "w"  ,   encoding  =  "utf-8"  )     as   filehandler  :
writer   =   csv  .  writer  (  filehandler  )
for   row   in   data  :
writer  .  writerow  (  row  )
```


The test looks like this:


```text
import   csv
import   pytest


def     test_write_to_csv  (  self  ,   tmp_path  )  :
third_class   =   ThirdClass  (  )
data   =     [  [  "A"  ,     "2"  ]  ,     [  "B"  ,     "3"  ]  ]


# tmp_path is a pathlib.Path object.
# We can use the / operator to create a path for our file.
file_path   =   tmp_path   /     "my_file.csv"


# We convert the path to a string to match the method signature
third_class  .  write_to_csv  (  str  (  file_path  )  ,   data  )


# Read file content to verify
with     open  (  file_path  ,     "r"  ,   encoding  =  "utf-8"  )     as   filehandler  :
reader   =   csv  .  reader  (  filehandler  )
# Convert the reader object directly to a list for comparison
saved_data   =     list  (  reader  )


assert   saved_data   ==   data
```


By using` tmp_path` in` test_write_to_csv()` , we avoid the arrange phase boilerplate and ensure our paths are constructed safely regardless of the operating system.


**Note:** A previous version of this post used the[tempfile](https://docs.python.org/3.11/library/tempfile.html) module of the standard library with a context manager instead of the` tmp_path` fixture. This works too but is outdated and path handling can be error-prone.


## Conclusion


In this post we have explored the creation of unit tests for scenarios of varying complexity. For basic functions that just return values and lack external dependencies, testing involves straightforward calls and validation of return values. However, when dealing with more complex functions, the need arises to temporarily substitute dependencies during testing. We have seen how to accomplish that with` monkeypatch` and` mocker.patch` from` pytest-mock` .


It's important to note that there are multiple approaches to write tests in Python, and the choice of method often lacks clear-cut guidelines to dictate when one should be preferred over another. I hope with this post I can help you tackle your unit tests.


## References


-


[Anatomy of a test](https://docs.pytest.org/en/7.1.x/explanation/anatomy.html#test-anatomy)


-


[Pytest fixtures](https://docs.pytest.org/en/7.1.x/reference/reference.html#fixtures-api)


-


[Pytest yield fixtures](https://docs.pytest.org/en/7.1.x/how-to/fixtures.html#teardown-cleanup-aka-fixture-finalization)


-


[How to use mocker.patch](https://docs.python.org/3/library/unittest.mock.html#id6)


-


[Great explanation of using monkeypatch](https://pytest-with-eric.com/pytest-best-practices/pytest-monkeypatch/#:~:text=Is%20MonkeyPatch%20The%20Same%20As%20Mocking%20or%20Patching%3F,-Yes%20and%20No&text=The%20two%20are%20very%20similar,as%20part%20of%20your%20test)
