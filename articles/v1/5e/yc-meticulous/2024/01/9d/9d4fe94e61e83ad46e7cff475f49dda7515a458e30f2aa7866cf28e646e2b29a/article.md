---
schema_version: "1.0.0"
document_id: "9d4fe94e61e83ad46e7cff475f49dda7515a458e30f2aa7866cf28e646e2b29a"
company_key: "yc-meticulous"
company: "Meticulous"
source_id: "yc-meticulous-news-import-624e920f0172"
canonical_url: "https://www.meticulous.ai/blog/javascript-unshift-complete-guide"
published_at: "2024-01-16T00:00:00+00:00"
first_seen_at: "2026-07-25T14:57:24.607504+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:0f389854b9ac2aee1d9b2faa1828f69b9f205c40a5eefc49e7a15f306df39e76"
---

# JavaScript Unshift | In-Depth Guide & Tutorial

## Introduction


In this blog post we will examine unshift's function signature, it's use case and two potential pitfalls of using unshift due to it's mutation property and performance cost. We examine the underlying implementation of unshift and the algorithm prescribed by ECMAScript to inform this analysis, and wrap up with a few alternative options. If you are already familiar with the basics of unshift, skip the 'what is unshift section'.


This is going to be fun! 🤓


## What is Unshift


Unshift is a method for adding elements to the beginning of an array or an object resembling an array. It's one of the most commonly used[JavaScript](https://www.meticulous.ai/blog/javascript-ui-testing-best-practices) methods when dealing with arrays and has the signature of unshift(element1, element2... elementN). It takes these elements and inserts them at the beginning of the array. This works for any number of elements less than 2^53-1 (more on this later).


Let's examine how it works below.


```text
const arr = [2,3]


arr.unshift(1)


console.log(arr)


```


The output in this example is \[1,2,3\].


Let's try with multiple elements:


```text
const arr = [3,4,5]


const res = arr.unshift(1,2)


console.log(arr)


console.log(res)


```


The first log statement here will output \[1,2,3,4,5\] , which is the value of arr. The second log statement will output 5, because the length of the new array is 5. If you're interested, you can read more about a simple logging solution in React[here](https://meticulous.ai/blog/getting-started-with-react-logging/) .


So that's the signature of unshift. This may seem straightforward, but there's a lot of interesting detail here. Let's start with unshift's mutation.


## Unshift Mutates


You will notice here that arr is *mutated* . We assigned the returned value to a new variable res, which contains the length of the new array. The original array arr was mutated and contains a different value before and after the .unshift call. Mutation is something to be careful about because it is a common cause of unexpected issues which can be tricky to debug. To drive this point home, the paradigm of functional programming has an explicit goal of avoiding mutation and having pure functions (functions without mutation or side effects) for this very reason.


We won't cover mutation, but if you're intersted, here are a few interesting articles: Clojure's philosophy on mutation[here](https://clojure.org/about/state) , MIT's course[here](https://web.mit.edu/6.005/www/fa15/classes/09-immutability/) or this blog post[here](https://blog.sapegin.me/all/avoid-mutation/#:~:text=Mutation%20may%20lead%20to%20unexpected,careful%20when%20reading%20the%20code..) .


Fortunately unshift does *not* return the new array. Fucntion signatures which both mutate the original object and return a new newly mutated object are particularly pernicious, since it is often not clear that the original object has been mutated.


Suppose instead we had an immutable function call that returns a new variable. If a new variable is created, it can be given a new name, thereby reducing the chance of confusion for the developer. Naming things is one of the most powerful tools we have as developers!


OK. Enough about mutation. Onto performance!


## Unshift's Performance Cost


Before we can examine unshift's performance cost, we need to understand it's underlying implementation. Different browsers' have different JavaScript engines and so may have different implementations. Fortunately for us, ECMAScript specifies an exact algorithm for how JavaScript engines should implement unshift. This is sufficient for our purposes, so let's take a look at ECMA20's specification for the unshift method:


```text
1) Let O be ? ToObject(this value).
2) Let len be ? LengthOfArrayLike(O).
3) Let argCount be the number of elements in items.
4) If argCount > 0, then
a. If len + argCount > 253 - 1, throw a TypeError exception.
b. Let k be len.
c. Repeat, while k > 0
i. Let from be ! ToString(𝔽(k - 1)).
ii. Let to be ! ToString(𝔽(k + argCount - 1)).
iii. Let fromPresent be ? HasProperty(O, from).
iv. If fromPresent is true, then
1. Let fromValue be ? Get(O, from).
2. Perform ? Set(O, to, fromValue, true).
v. Else,
1. Assert: fromPresent is false.
2. Perform ? DeletePropertyOrThrow(O, to).
vi. Set k to k - 1.
d. Let j be +0𝔽.
e. For each element E of items, do
i. Perform ? Set(O, ! ToString(j), E, true).
ii. Set j to j + 1𝔽.
5) Perform ? Set(O, "length", 𝔽(len + argCount), true).
6) Return 𝔽(len + argCount).


```


Wow! There's a lot going on here and the notation is a little arcane. You can read the full implementation, along with references[here](https://262.ecma-international.org/12.0/#sec-array.prototype.unshift) . We're not going to dissect this code line-by-line in this post, but there is one key takeaway here:


Unshift has a computational complexity of O(N+X), where N is the number of arguments passed to unshift and X is the number of elements in the array or array-like object. This means that the time taken to execute the operation scales linearly with the number of elements in the array. This can quickly get expensive, particularly if we're repeatedly calling unshift within some sort of loop construct.


Another interesting note is that unshift will fail for an array and number of arguments where their combined lengths exceed 2^53-1. This is because the implementation above specifies a number type for storing the number of elements of an array, which has a max value specified by MAX_SAFE_INTEGER which is 2^53-1. However, you will encounter an out of memory error before encountering this error. Suppose each element had just one byte, then the array would reach 9000TB before hitting the MAX_SAFE_INTEGER limit. This would result in an out of memory error.


In terms of optimizing the performance cost, there's not too much we can do directly and this really is more dependent upon the context in which unshift is being called. If you are regularly prepending to an array, you could consider an alternative data structure with a lower time complexity cost, such as a linked list or circular buffer.


One strategy for avoiding the mutation is to make use of JavaScript's spread operator, which 'unpacks' the values within an array. The performance cost of the spread operator is still O(N), where N is the number of elements in the array being unpacked. The spread operator can be used to make new arrays, like the example below:


```text
const arr = [2,3]


const newArr = [1, ...arr]


console.log(newArr);


```


The output here is \[1,2,3\].


Beware of unshift and hope you enjoyed this article!


## Meticulous


Meticulous creates and maintains an exhaustive suite of e2e ui tests with **zero** developer effort.


This quote from the CTO of Traba sums the product up best: "Meticulous has fundamentally changed the way we approach frontend testing in our web applications, fully eliminating the need to write any frontend tests. The software gives us confidence that every change will be completely regression tested, allowing us to ship more quickly with significantly fewer bugs in our code. The platform is easy to use and reduces the barrier to entry for backend-focused devs to contribute to our frontend codebase."


This[post](https://www.meticulous.ai/blog/lessons-from-a-decade) from our CTO (formerly lead of Palantir's main engineering group) sets out the context of why exhaustive testing can double engineering velocity. Learn more about the product[here.](https://www.meticulous.ai/)
