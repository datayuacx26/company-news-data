---
schema_version: "1.0.0"
document_id: "0c7ce01ad05619d184c959b33aa412a997050c382a283521fbba4fe34913c142"
company_key: "yc-meticulous"
company: "Meticulous"
source_id: "yc-meticulous-news-import-624e920f0172"
canonical_url: "https://www.meticulous.ai/blog/range-errors-and-how-to-prevent-them"
published_at: "2024-01-16T00:00:00+00:00"
first_seen_at: "2026-07-25T14:57:24.607504+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:37c91041ec2cc24e41597ff7e58cb4f1ac7843812afcc62b8be2f21655f07ad8"
---

# JavaScript RangeErrors | How to Prevent Them, Tutorial

If you’ve spent time developing in[JavaScript](https://www.meticulous.ai/blog/javascript-ui-testing-best-practices) , you’ve likely come across a RangeError. A RangeError is one of the eight Error subtypes that are a part of the JavaScript standard built-in objects. Other standard errors include the AggregateError, EvalError, ReferenceError, SyntaxError, TypeError, and URIError. The InternalError can also be mentioned here, but it’s currently not standard and not on track to become standard.


In this article, you’ll learn all about RangeErrors and what causes them. You’ll also learn how to effectively deal with RangeErrors when they occur and, more importantly, how to mitigate the error in the future.


## What is a RangeError


A RangeError is used to convey to a user that a value was passed into a function that does not accept an input range that includes that value. For example, if a function converts centimeters to inches, then the input range might be all the non-zero positive numbers. In this instance, the number -100 would not be in the allowed input range and would cause a RangeError to be thrown.


Understanding what a RangeError is, why one might be thrown, and when you should generate one yourself will help you write more expressive, correct code. In addition, understanding the motivation behind a RangeError will help you understand how to quickly and easily recover when one is thrown by the JavaScript standard library or external dependencies. As an example, take a look at the following code:


```text
const numberGuesser = (numberGuess) => {
// Validate guess is valid before checking if it's correct
if (numberGuess > 10 || numberGuess <= 0) {
throw RangeError(`Invalid Guess: ${numberGuess}, number must be between 1 and 10, inclusive`)
}
…
}


```


In the previous code, the allowed number set is implicitly defined as the integers between 1 and 10, inclusive. This is an example of a custom code that could reasonably throw the RangeError. There are also a few JavaScript functions and constructors that could throw the RangeError, including the following:


- Calling the String.prototype.normalize(): this returns the[Unicode Normalization of a string](https://unicode.org/reports/tr15/) and, optionally, accepts a form argument, which is any of the following sets {"NFC", "NFD", "NFKC", "NFKD"}. If the form is not one of these strings, then a RangeError is thrown. You can read more about this function in the[official MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize) .
- Creating an array with an illegal length: in this instance, an illegal length is one that is negative or too large. An array length must be[between 0 and 2^32 - 1](https://www.oreilly.com/library/view/javascript-the-definitive/9781449393854/ch07.html) . If you want to create an array that’s too large, in Google Chrome, open up[DevTools](https://developer.chrome.com/docs/devtools/) and type Array(Math.pow(2,32)). If performed correctly, you should be met with an uncaught RangeError with the message “Invalid array length.”
- Passing invalid values to the methods: invalid values include Number.prototype.toExponential(digits), Number.prototype.toFixed(fractionDigits), or Number.prototype.toPrecision(precision). The methods toExponential and toFixed accept a digits and fractionDigits argument, respectively, between 0 and 100, inclusive. toPrecision accepts a precision argument between 1 and 100, inclusive.
- Exceeding the maximum call stack size: this occurs when a recursive function calls itself too many times. You can test this in Google Chrome by writing a simple recursive function, like the following:


```text
const recurse = (numTimes) => {
if (numTimes == 0) {
return
}
recurse(numTimes - 1)
}


```


After writing the function, you can call it with different inputs. In this instance, a RangeError will be thrown since the function was called with a numTimes argument of about 11,400. (Please note: your experiment may be different depending on what processes are running on your machine and how much memory is available.)


The earlier bullet points list situations when RangeError can be instantiated and thrown by built-in functions in ES6 JavaScript. Other JavaScript built-in functions can also propagate RangeError from the function calls mentioned earlier. In addition, other dependencies may also throw RangeError, either propagated from the functions mentioned or explicitly instantiated and thrown.


## Preventing and Mitigating Range Errors


Mitigating and preventing uncaught RangeError entail understanding which functions are throwing them and how you should deal with each of those instances.


You can prevent a RangeError from ever instantiating by ensuring that every function that throws a RangeError is given valid input that is included in the expected set. For example, if you’re creating a spreadsheet-style app and would like the user to be able to specify how many digits should be shown for a given cell, you may want to use the Number.prototype.toFixed method.


To ensure a RangeError will not be thrown, you need to make sure that the user’s desired number of digits appearing after the decimal point are between 1 and 100:


```text
const updateCellFormat = (desiredPrecision, cell) => {
if (desiredPrecision < 1 || desiredPrecision > 100) {
return cell
}
cell.contents = cell.contents.toFixed(desiredPrecision)
return cell
}


```


The previous code looks at the user’s desired precision (the desiredPrecision parameter), and if it’s not in the valid range for the toFixed function, it simply returns the unaltered cell. Otherwise, it updates the cell’s contents (assuming the contents are a JavaScript Number type) to reflect the user’s desired decimal precision and returns the updated cell.


Ensuring a RangeError can never be thrown is extremely difficult in practice. Input sanitization, especially in JavaScript, can be a daunting task. Users can behave unpredictably, and JavaScript’s dynamic typing doesn’t help. In addition, checking all the inputs in every method can lead to large functions, run-time, and maintenance costs, as well as overly[defensive programming](https://www.wikiwand.com/en/Defensive_programming) . Ideally, you should be able to rely solely on the interfaces of libraries you interact with without needing to look at the implementation to see if a RangeError might be thrown.


You can also mitigate RangeError damage by catching them before they propagate up the call stack. This can be achieved by ensuring that every function call to a function that can throw a RangeError is wrapped in a try...catch block. For example, you could change your spreadsheet formatter code to look like this:


```text
const updateCellFormat = (desiredPrecision, cell) => {
try {
cell.contents = cell.contents.toFixed(desiredPrecision)
return cell
} catch (error) {
if (error instanceof RangeError) {
return cell
}
}
}


```


Instead of explicitly checking if the desiredPrecision parameter is in an allowable range, the previous code tries to call the toFixed function in a try...catch block, which will automatically throw a RangeError if the parameter is invalid. You can read more about try-catch in this blog post[here](https://meticulous.ai/blog/javascript-try-catch-guide/) .


Another option to mitigate errors is by writing unit tests that verify valid function inputs do not result in an unexpected RangeError. This will help give you confidence that an uncaught RangeError will not make an appearance in the production system.


## Best Practices using Range Errors


There are cases in which a RangeError is a valid response and can even be displayed to the user. You can follow some best practices to ensure you’re using RangeErrors appropriately.


For example, in the number guesser game program shown earlier in the “What Is a RangeError” section, you might choose to display the error directly to the user since it provides valuable information that the user can apply to make better decisions in the future. However, you still don’t want to allow the user to witness an uncaught error, but you do want them to understand that an error was thrown:


```text
​​ var guess = window. prompt("Enter a guess between 1-10.");
try {
numberGuess(guess)
} catch (error) {
if (error instanceof RangeError) {
console.error(error.message)
}
}


```


In the previous code, if a user typed “11,” they would get a console error saying, “Invalid Guess: number must be between 1 and 10, inclusive,” as that’s the message you initialized your custom RangeError with. This system provides useful feedback to the user because they’re able to understand why their input was incorrect and how to correct it in the future.


You can also try and prevent a user from entering invalid input into the text field, but that would require more code and more complex unit tests, and you still might need to figure out a way to inform the user why they’re unable to enter certain numbers in the text field.


In some cases,[TypeScript](https://www.typescriptlang.org/) can help catch a RangeError that shouldn’t be a RangeError. For example, check out this[TypeScript](https://www.meticulous.ai/blog/safer-exhaustive-switch-statements-in-typescript) function:


```text
const chooseJerseyNumber = (jerseyRequest: number) => {
const validJerseyNums= new Set([4, 13])
if (!validJerseyNums.has(jerseyRequest)) {
throw RangeError(`Invalid Number: ${jerseyRequest}, jersey request should be one of: ${Array.from(a.validJerseynums())}`)
}
…
}


```


In this example, it doesn’t make sense to throw a RangeError if the input is a string version 4 or 13 since these aren’t invalid elements (they are mistyped). The user will be very confused if they receive an error message along the lines of “Invalid Number: ‘4’, jersey request should be one of \[4, 13\].” In this instance, the error is truly a variable type error. TypeScript has the ability to throw a[TypeError error](https://www.meticulous.ai/blog/javascript-typeerrors-and-techniques-to-prevent-them) the moment the function call executes with the string “4” instead of the number “4”. TypeScript will display something like “Argument of type ‘string’ is not assignable to parameter of type ‘number’.”


As a developer, encountering a[TypeError](https://www.meticulous.ai/blog/javascript-typeerrors-and-techniques-to-prevent-them) instead of a RangeError should make it immediately obvious what needs to be fixed in the code module that calls the chooseJerseyNumber function.


## Using Third-Party Dependencies


Aside from the number guesser example earlier, in most cases, a RangeError should be prevented from ever manifesting. As mentioned earlier, this is an incredibly daunting task, and sometimes, it’s not practical or prudent to spend engineering hours ensuring every possible error is caught. That’s where[Meticulous](https://meticulous.ai/) comes in.


While errors can certainly be mitigated through the methods discussed in the previous section, any nontrivial app will usually have some errors or unexpected behavior. Generally, you might rely on manual testing or automated QA sessions to make sure a new feature works correctly and doesn’t break any existing functionality. Integrating Meticulous with your app allows you to replay developer or QA testing sessions to see where uncaught JavaScript errors arise after introducing a new feature or update.


Meticulous also plays nicely with external HTTP requests. During session recordings, requests are mocked so that they don’t actually execute again during the recording playback, ensuring no unintended side effects. For example, if you’re testing a sign-up flow, Meticulous intelligently saves the sign-up response from the backend. Then when replaying the session, Meticulous uses the saved response. In that way, replaying a sign-up QA session ten times doesn’t create ten new users.


Mocking HTTP requests also ensures all changes are not dependent on external dependencies during playback, which might experience outages or other unexpected behavior; in this way, replaying sessions will always be consistent.


## Conclusion


Error handling is an incredibly powerful aspect of programming. Solid error handling portrays new information to other developers or users, allowing them to understand what went wrong and how they should best proceed.


Lack of errors and exceptions can create a frustrating experience, which is why the RangeError should be used to ensure you understand how functions (or mechanisms) are constrained so they can be used as designed without unexpected behavior.


Ensuring valid input to functions that throw a RangeError, catching range errors, incorporating TypeScript, and using[Meticulous](https://meticulous.ai/) are all engineering decisions that can be incorporated to help create an error-free product that works the way you intend.


## Meticulous


Meticulous creates and maintains an exhaustive suite of e2e ui tests with **zero** developer effort.


This quote from the CTO of Traba sums the product up best: "Meticulous has fundamentally changed the way we approach frontend testing in our web applications, fully eliminating the need to write any frontend tests. The software gives us confidence that every change will be completely regression tested, allowing us to ship more quickly with significantly fewer bugs in our code. The platform is easy to use and reduces the barrier to entry for backend-focused devs to contribute to our frontend codebase."


This[post](https://www.meticulous.ai/blog/lessons-from-a-decade) from our CTO (formerly lead of Palantir's main engineering group) sets out the context of why exhaustive testing can double engineering velocity. Learn more about the product[here.](https://www.meticulous.ai/)
