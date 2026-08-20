---
schema_version: "1.0.0"
document_id: "745fb6094166e3e827e1b85ed01ee0fd4c1093682d0949f961723b8db830a618"
company_key: "yc-argument-computer-corporation"
company: "Argument Computer Corporation"
source_id: "yc-argument-computer-corporation-news-import-6d03b0bcc333"
canonical_url: "https://argument.xyz/blog/prog-intro/"
published_at: null
first_seen_at: "2026-07-21T07:37:57.416159+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:da4b9e7a37496bb28b93922fb3aceee1af0f03336b45ca7835e8fe0175a7e389"
---

# A Programmer’s Introduction to Lurk

## Overview


This is an introduction to the Lurk programming language, written for programmers.


Throughout this post, you will find simulated code blocks, like the one below. To render the outputs to these expressions, click the right-most triangle in the upper-right-hand corner. Use the two left-pointing arrows to reset. These code examples are hard-coded with outputs that replicate those returned by the[Lurk REPL](https://github.com/argumentcomputer/lurk-rs#repl) . If you want to run this code directly from the source, you can follow the instructions at the above link to run the REPL locally.


```text
(  cons   '  hello   '  world)


```


```text
Iterations  :    Result  :
```


If you’d like to experiment with a stateful, editable web component running actual Lurk code, you can edit the code directly below. You might experience difficulties on certain browsers or on mobile devices, and there might be significant delay when opening commitments (discussed much later). It is for these reasons that we chose not to rely on this web component throughout the post.


```text
;; The below code is editable. The rest of the code in this post is not. (This is a comment.)
(  cons   '  hello   '  world)


```


```text
Iterations  :    Result  :
```


Read on to learn more about the range of valid Lurk expressions, how to write non-trivial Lurk programs, and for some clues as to why you might want to. By the end of this post, you will understand how Lurk can leverage cryptographic commitments to enable provably correct applications of secret functions as well as verifiable computation over private data.


## What is Lurk?


Lurk is a Turing-complete language for recursive zk-SNARKs. The most interesting uses of Lurk are those which require proofs of evaluation. A Lurk proof may verifiably attest that X evaluates to Y while revealing no other information. Lurk proofs reveal neither the intermediate values that must be calculated in the course of evaluating an expression, nor the actual ‘content’ of all or part of either the input X or output Y of the evaluation (identified only by type and content address) apart from their types. Even the types of input and output can be effectively hidden using explicit commitments (described in more detail below) at the application level.


This post will not focus on the mechanics of how these proofs work, or even on much detail of their characteristics. For now, readers need only know that we can produce relatively small (~2-10KiB) proofs that can be verified relatively quickly (10-30ms in the best cases) and that reveal no private information.


## Lurk is a Lisp


Lurk is a Lisp, influenced by Scheme and Common Lisp. Lurk data is content-addressed, which means that every expression is identified and may be referred to by a type-tagged (cryptographic) hash digest.


Since Lurk is a Lisp, it has practically no syntax. Everything is an expression. Some expressions are self-evaluating. For example, the number 3 evaluates to itself (the number 3).


```text
3


```


```text
Iterations  :    Result  :
```


When the expression (` 3` ) above has evaluated, you should see a response with the result (` 3` ) and a count of ‘Iterations’, in this case, 1. You can think of this as representing the ‘cost’ or number of ‘clock cycles’ required to evaluate the expression. In the simplest case of a self-evaluating expression, only a single iteration is required.


Strings are also self-evaluating.


```text
"foo"


```


```text
Iterations  :    Result  :
```


Lists are printed as white-space separated expressions, enclosed in parentheses.


```text
'  (  1   2   3  )


```


```text
Iterations  :    Result  :
```


Above, the quote character (` '` ) introduces a literal expression to which the input evaluates.


---


Unquoted lists are evaluated by treating the first element as a potential function.


```text
(  +   2   3  )


```


```text
Iterations  :    Result  :
```


Technically,` +` is a built-in operator: it does not evaluate to a Lurk function but behaves like one here.


---


A list whose first element is neither a built-in operator nor evaluates to a function yields an error when evaluated.


```text
(  1   2   3  )


```


```text
Iterations  :    Result  :
```


This is because lists are evaluated by first evaluating each element of the list (in order), then treating the first result as a function to be applied to the remaining. In the case above, the number` 1` does not name a function, so it cannot be applied to the list` (2 3)` .


---


Built-in operators cannot be independently evaluated.


```text
+


```


```text
Iterations  :    Result  :
```


The following example makes clear that the arguments to` +` are first evaluated before being added. Evaluation proceeds recursively, following a strict, left-to-right evaluation order.


```text
(  +   (  *   2   7  ) (  *   3   3  ))


```


```text
Iterations  :    Result  :
```


Pairs are constructed with` CONS` , and their elements are separated by a` .` when printed.


```text
(  cons   1   2  )


```


```text
Iterations  :    Result  :
```


A linked list of pairs forms a list, and if terminated by the special self-evaluating symbol,` NIL` it is a ‘proper’ list, represented with no final dot.


```text
(  cons   9   nil  )


```


```text
Iterations  :    Result  :
```


A proper list can be extended by consing a new element onto its head.


```text
(  cons   8   '  (  9  ))


```


```text
Iterations  :    Result  :
```


Consing pairs that are not proper lists allows creation of arbitrary binary-tree structures.


```text
(  cons   (  cons   (  cons   1   2  ) (  cons   3   4  )) (  cons   (  cons   5   6  ) (  cons   7   8  )))


```


```text
Iterations  :    Result  :
```


` NIL` , the list terminator, is self-evaluating.


```text
nil


```


```text
Iterations  :    Result  :
```


Note also that symbols are normalized to all-caps when read (parsed from character strings).


---


The built-in` EQ` operator compares Lurk expressions for equality. (Note that` =` can also be used for this purpose when both arguments are numbers.)


```text
(  eq   1   2  )


```


```text
Iterations  :    Result  :
```


` NIL` is false.


```text
(  eq   1   1  )


```


```text
Iterations  :    Result  :
```


` T` , the canonical true value, is also self-evaluating.


```text
t


```


```text
Iterations  :    Result  :
```


Conditionals are implemented by` IF` , which is an expression.


```text
(  if   (  eq   1   1  )   9   8  )


```


```text
Iterations  :    Result  :
```


```text
(  if   (  eq   1   2  )   9   8  )


```


```text
Iterations  :    Result  :
```


Only the selected branch of an` IF` expression is actually evaluated.


```text
(  if   (  eq   1   1  ) (  +   9   8  )   17  )


```


```text
Iterations  :    Result  :
```


Note the iteration count (8) above and compare with that below.


```text
(  if   (  eq   1   2  ) (  +   9   8  )   17  )


```


```text
Iterations  :    Result  :
```


The second example only requires 6 iterations because a cheaper branch of the computation was followed.


---


Lurk data is immutable, and Lurk expressions are content-addressed — constructed by cryptographic hash-consing, so equality checks are deep and cheap.


```text
(  eq   '  (  1   (  2   .   "monkey"  ) ((x (y))))   '  (  1   (  2   .   "monkey"  ) ((x (y)))))


```


```text
Iterations  :    Result  :
```


Notice the iteration count above is the same as that for a simple equality check of two numbers.


---


Variable bindings are introduced by` LET` .


```text
(  let   ((a   9  ))
(  +   a   1  ))


```


```text
Iterations  :    Result  :
```


Above, the symbol` A` is bound to the value` 9` in the addition which adds` 1` to it.


---


Quoting stops evaluation of symbols too.


```text
(  let   ((a   9  ))
(  cons   '  a a))


```


```text
Iterations  :    Result  :
```


Lexical bindings can be shadowed by inner bindings.


```text
(  let   ((a   9  ))
(  let   ((a   10  ))
(  +   a   1  )))


```


```text
Iterations  :    Result  :
```


A single` LET` form can bind multiple variables.


```text
(  let   ((a   9  )
(b (  +   a   1  )))
(  *   a b))


```


```text
Iterations  :    Result  :
```


Multiple` LET` bindings are performed sequentially (so the following won’t work).


```text
(  let   ((b (  +   a   1  ))
(a   9  ))
(  *   a b))


```


```text
Iterations  :    Result  :
```


Anonymous functions are created with` LAMBDA` .


```text
(  lambda   (x) (  *   x x))


```


```text
Iterations  :    Result  :
```


The example above creates a function of one argument (` X` ), which squares its argument. Within the body of the function,` X` (the *formal parameter* ) is bound to the value passed (the *actual parameter* ) when the function is applied.


---


Anonymous functions can be immediately applied.


```text
((  lambda   (x) (  *   x x))   5  )


```


```text
Iterations  :    Result  :
```


Above,` X` is bound to` 5` , then the body` (* X X)` is evaluated.


---


Anonymous functions can be named by binding them to symbols.


```text
(  let   ((square (  lambda   (x) (  *   x x))))
(square   5  ))


```


```text
Iterations  :    Result  :
```


Since bindings can be in scope when functions are defined, those functions may act as lexical closures.


```text
(  let   ((val   123  )
(add-val (  lambda   (x) (  +   val x))))
(add-val   5  ))


```


```text
Iterations  :    Result  :
```


Above, the function named` ADD-VAL` captures (‘closes over’) the binding of` VAL` .


---


The bindings captured by a closure can themselves be established by a function call, as in the classic example.


```text
(  let   ((make-adder (  lambda   (n)
(  lambda   (x) (  +   n x))))
(adder-1 (make-adder   1  ))
(adder-9 (make-adder   9  )))
(  cons   (adder-1   3  ) (adder-9   4  )))


```


```text
Iterations  :    Result  :
```


` LET` bindings are not in scope when the expressions defining their values are evaluated.


```text
(  let   ((nope (  lambda   (x) (nope x))))
(nope   123  ))


```


```text
Iterations  :    Result  :
```


However, functions named using` LETREC` can be recursive: they can call themselves.


```text
(letrec ((  length   (  lambda   (  list  )
(  if   (  eq   list   nil  )   ;; Base case
0
(  +   1   (  length   (  cdr   list  )))))))
(  length   '  (a b c d e)))


```


```text
Iterations  :    Result  :
```


Recursion can be used to implement loops.


```text
(letrec ((sum-upto (  lambda   (n) (  if   (  =   n   0  )
0
(  +   n (sum-upto (  -   n   1  )))))))
(sum-upto   100  ))


```


```text
Iterations  :    Result  :
```


Functions can take functions as arguments.


```text
(letrec ((  map   (  lambda   (f   list  )
(  if   (  eq   list   nil  )
nil
(  cons   (f (  car   list  ))
(  map   f (  cdr   list  ))))))
(square (  lambda   (x) (  *   x x))))
(  map   square   '  (  1   2   3   4   5  )))


```


```text
Iterations  :    Result  :
```


Recall that the classic` MAKE-ADDER` *returns* a function.


```text
(  let   ((make-adder (  lambda   (x)
(  lambda   (n)
(  +   x n)))))
(make-adder   123  ))


```


```text
Iterations  :    Result  :
```


As demonstrated above, functions are first-class. They can be manipulated and passed as ordinary data. This means that Lurk supports[higher-order functions](https://en.wikipedia.org/wiki/Higher-order_function) (HOF). That is, Lurk functions can receive functions as input and return functions as output — allowing for a wide range of expressive functional programming idioms.


```text
(letrec ((filter (  lambda   (pred   list  )
(  if   (  eq   list   nil  )
nil
(  if   (pred (  car   list  ))
(  cons   (  car   list  )
(filter pred (  cdr   list  )))
(filter pred (  cdr   list  ))))))
(sums-to (  lambda   (n pair)
(  =   (  +   (  car   pair) (  cdr   pair))
n))))
(filter (sums-to   11  )
'  ((  1   .   2  ) (  5   .   6  ) (  4   .   7  ) (  9   .   9  ))))


```


```text
Iterations  :    Result  :
```


Note also that Lurk supports partial evaluation, so we could implement` MAKE-ADDER` more simply.


```text
(  let   ((make-adder (  lambda   (a b) (  +   a b))))
(make-adder   123  ))


```


```text
Iterations  :    Result  :
```


In other words, passing one argument to a two-argument function yields a one-argument function.


```text
(  let   ((make-adder (  lambda   (a b) (  +   a b)))
(adder (make-adder   123  )))
(adder   5  ))


```


```text
Iterations  :    Result  :
```


In fact, the two definitions are directly equivalent because Lurk automatically curries multi-argument functions into nested unary functions.


```text
(  eq   (  lambda   (a b) (  +   a b))
(  lambda   (a) (  lambda   (b) (  +   a b))))


```


```text
Iterations  :    Result  :
```


Given higher-order recursive functions, we can define` REDUCE` .


```text
(letrec ((  reduce   (  lambda   (acc f   list  )
(  if   (  eq   list   nil  )
acc
(  reduce   (f acc (  car   list  ))
f
(  cdr   list  )))))
(sum (  reduce   0   (  lambda   (a b) (  +   a b)))))
(sum   '  (  1   2   3   4  )))


```


```text
Iterations  :    Result  :
```


Combined with our previous definition of` MAP` , we can implement` MAP-REDUCE` .


```text
(letrec ((  reduce   (  lambda   (acc f   list  )
(  if   (  eq   list   nil  )
acc
(  reduce   (f acc (  car   list  ))
f
(  cdr   list  )))))
(  map   (  lambda   (f   list  )
(  if   (  eq   list   nil  )
nil
(  cons   (f (  car   list  ))
(  map   f (  cdr   list  ))))))
(map-reduce (  lambda   (map-fn reduce-fn initial   list  )
(  reduce   initial reduce-fn (  map   map-fn   list  ))))
(plus (  lambda   (a b) (  +   a b)))
(square (  lambda   (x) (  *   x x))))
(map-reduce square plus   0   '  (  1   2   3   4  )))


```


```text
Iterations  :    Result  :
```


Although we do not fixate on ‘performance’ here, do note that` MAP-REDUCE` can be implemented more efficiently, if also less clearly, by using a single recursive function (compare reported iteration counts).


```text
(letrec ((map-reduce (  lambda   (map-fn reduce-fn acc   list  )
(  if   (  eq   list   nil  )
acc
(map-reduce map-fn reduce-fn (reduce-fn acc (map-fn (  car   list  ))) (  cdr   list  )))))
(plus (  lambda   (a b) (  +   a b)))
(square (  lambda   (x) (  *   x x))))
(map-reduce square plus   0   '  (  1   2   3   4  )))


```


```text
Iterations  :    Result  :
```


Or even more so by factoring the constant (for the life of the` MAP-REDUCE` call) functions out of the recursion.


```text
(  let   ((map-reduce (  lambda   (map-fn reduce-fn acc   list  )
(letrec ((aux (  lambda   (acc   list  )
(  if   (  eq   list   nil  )
acc
(aux (reduce-fn acc (map-fn (  car   list  ))) (  cdr   list  ))))))
(aux acc   list  ))))
(plus (  lambda   (a b) (  +   a b)))
(square (  lambda   (x) (  *   x x))))
(map-reduce square plus   0   '  (  1   2   3   4  )))


```


```text
Iterations  :    Result  :
```


## Finite Fields


Lurk proofs are implemented in arithmetic circuits whose fundamental numeric type is the finite field. This is because zk-SNARKs use elliptic curves for their cryptographic properties. In practice, this means that arithmetic is performed modulo some large prime (the *order* of the field).


Lurk’s primary target is the[Nova](https://github.com/microsoft/Nova) proving system, which is a recursive SNARK. This requires a[curve cycle](https://eprint.iacr.org/2014/595.pdf) , and by default we use the[Pasta curves](https://electriccoin.co/blog/the-pasta-curves-for-halo-2-and-beyond/) . What this means for programmers, is that Lurk’s default numbers are elements of Fq, the[scalar field of Pallas](https://github.com/zcash/pasta_curves/blob/main/src/fields/fq.rs) .


```text
(  -   0   1  )


```


```text
Iterations  :   3
Result  : 0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000000


```


```text
Iterations  :    Result  :
```


The result,` 0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000000` is the largest value representable in Fq. That is to say, it is one less than q,[the modulus of Fq](https://github.com/zcash/pasta_curves/blob/main/src/fields/fq.rs#L21) .


---


q=0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000001q = 0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000001


q


=


0


x


40000000000000000000000000000000224698


f


c


0994


a


8


dd


8


c


46


e


b


2100000001


```text
(  +   0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000000   1  )


```


```text
Iterations  :   3
Result  :   0


```


```text
Iterations  :    Result  :
```


q≡0mod qq \\equiv 0 \\mod{q}


q


≡


0


mod


q


---


Division works in a finite field,


```text
(  /   10   2  )


```


```text
Iterations  :    Result  :
```


but the results are not intuitive if the dividend is not a multiple of the divisor.


```text
(  /   5   6  )


```


```text
Iterations  :   3
Result  : 0x0aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaab061197f56ee1c24ecb67c8580000001


```


```text
Iterations  :    Result  :
```


However, since ‘division’ is implemented as the product with the multiplicative inverse, arithmetic manipulation works as expected, which is the point.


```text
(  *   (  /   1   2  )   2  )


```


```text
Iterations  :    Result  :
```


You can use ‘fractional’ notation, but the result is still a single field element.


```text
5/6


```


```text
Iterations  :   1
Result  : 0x0aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaab061197f56ee1c24ecb67c8580000001


```


```text
Iterations  :    Result  :
```


The same result as a sum:


```text
(  +   1/3   1/2  )


```


```text
Iterations  :   1
Result  : 0x0aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaab061197f56ee1c24ecb67c8580000001


```


```text
Iterations  :    Result  :
```


Lurk defines an ordering of field elements, but take note that since finite-field arithmetic is modular, there must be a counter-intuitive case.


```text
(  let   ((largest-unsigned (  -   0   1  ))
(most-positive (  /   largest-unsigned   2  ))
(most-negative (  +   1   most-positive)))
(  <   most-negative most-positive))


```


```text
Iterations  :    Result  :
```


Above, the most-negative number is produced by adding one to the most-positive.


Note that although Lurk’s field-element ordering allows for sorting and comprehensible comparisons between ‘small’ (i.e. less than MOST-POSITIVE) field elements, elements derived from operations which overflow will violate expectations. This follows from the fact that finite fields are not[ordered fields](https://en.wikipedia.org/wiki/Ordered_field) . Nevertheless, when an application knows the domain of its operations is restricted to ‘small’ field elements, or when an opaque consistent ordering is required, Lurk’s native comparisons can be useful.


---


In all other cases, the expected identity ( n<n+1 n < n + 1


n


<


n


+


1


) holds.


```text
(  <   0   1  )


```


```text
Iterations  :    Result  :
```


Remember,` (- 0 1)` is (in some sense) the largest field element directly representable in Lurk.


```text
(  -   0   1  )


```


```text
Iterations  :   1
Result  : 0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000000


```


```text
Iterations  :    Result  :
```


This is a *large number* , but it compares as *less than zero* .


```text
(  <   0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000000   0  )


```


```text
Iterations  :    Result  :
```


Since finite field elements are required, by definition, to have additive inverses, we must treat the ‘largest’ elements as negative.


```text
(  +   1   (  -   0   1  ))


```


```text
Iterations  :    Result  :
```


Lurk provides an explicit negative-number syntax for convenience.


```text
-1


```


```text
Iterations  :   1
Result  : 0x40000000000000000000000000000000224698fc0994a8dd8c46eb2100000000


```


```text
Iterations  :    Result  :
```


---


For pedantic or curious readers (others are advised to skip this brief tangent), who want a precise definition of what we mean when we say that` -1` is ‘(in some sense) the largest’ field element, even though we define it as negative:


Please consider the definition of` LARGENESS-IN-SOME-SENSE-BASE-ONE` illustrated below. We note in passing that this humorously-intended (but still informative) definition also demonstrates the use of` STRCONS` to construct strings from characters.


```text
(letrec ((aux (  lambda   (acc n)
(  if   (  =   n   0  )
acc
(aux (strcons   #\1   acc) (  -   n   1  )))))
(largeness-in-some-sense-base-one (  lambda   (n) (aux   ""   n))))
(largeness-in-some-sense-base-one   5  ))


```


```text
Iterations  :    Result  :
```


---


Using extended numeric syntax, it’s easy to remember how to express the most negative and most positive field elements.


```text
(  let   ((most-positive   -1/2  )
(most-negative   1/2  ))
(  =   most-negative (  +   1   most-positive)))


```


```text
Iterations  :    Result  :
```


−1/2+1=1/2-1/2 + 1 = 1/2


−


1/2


+


1


=


1/2


But please note that use of fractional syntax does not yield ‘real’ fractions. The resulting field elements do not always compare as expected.


```text
(  <   3/4   7/8  )


```


```text
Iterations  :    Result  :
```


## Commitments


Lurk has built-in support for[cryptographic commitments](https://en.wikipedia.org/wiki/Commitment_scheme) .


When the value corresponding to a commitment is unknown, it cannot be opened.


```text
(  open   (comm 0x2937881eff06c2bcc2c8c1fa0818ae3733c759376f76fc10b7439269e9aaa9bc))


```


```text
Iterations  : undefined
Result  : Evaluation   Error  : Lookup   error  : hidden value could   not   be opened


```


```text
Iterations  :    Result  :
```


---


We can create a commitment to any Lurk expression with` COMMIT` .


```text
(commit   123  )


```


```text
Iterations  :   2
Result  : (comm 0x2937881eff06c2bcc2c8c1fa0818ae3733c759376f76fc10b7439269e9aaa9bc)


```


```text
Iterations  :    Result  :
```


Now that the Lurk evaluation environment *knows* that` (comm 0x2937881eff06c2bcc2c8c1fa0818ae3733c759376f76fc10b7439269e9aaa9bc)` is a commitment to` 123` , it can successfully open the commitment.


```text


```


```text
Iterations  :   4
Result  :   123


```


```text
Iterations  :    Result  :
```


---


(Note: if you happened to ‘run’ this before committing, you would have seen the same output. Remember that this is because the code examples in this post are returning pre-populated outputs. In actuality,` open` will not work if a corresponding commitment has not been made.)


---


Because Lurk commitments are based on Poseidon hashes (just as all compound data in Lurk is), it is computationally infeasible to discover a second preimage to the digest represented by a commitment. This means that Lurk commitments are (computationally) binding.


Lurk allows` OPEN` to operate on field elements (so the` (COMM …)` wrapper can be omitted).


```text
(  open   0x2937881eff06c2bcc2c8c1fa0818ae3733c759376f76fc10b7439269e9aaa9bc)


```


```text
Iterations  :   2
Result  :   123


```


```text
Iterations  :    Result  :
```


For simplicity of examples, we will use bare field elements as commitments to be opened in the remainder of this post.


---


Lurk also supports explicit hiding commitments. For when hiding is unimportant,` COMMIT` creates commitments with a default secret of` 0` .


```text
(hide   0   123  )


```


```text
Iterations  :   3


```


```text
Iterations  :    Result  :
```


However, any field element can be used as the secret, which makes Lurk commitments *hiding* as well as *binding* .


```text
(hide   999   123  )


```


```text
Iterations  :   3
Result  : (comm 0x3cb2f9666edb8e4444633e960ed56ef0486ea0c3628428680542e2b55f3bf67f)


```


```text
Iterations  :    Result  :
```


Note that the returned commitment is different from the one returned by both` (COMMIT 123)` and` (HIDE 0 123)` .


---


However, both commitments open to the same value.


```text
(  =   (  open   0x2937881eff06c2bcc2c8c1fa0818ae3733c759376f76fc10b7439269e9aaa9bc)
(  open   0x3cb2f9666edb8e4444633e960ed56ef0486ea0c3628428680542e2b55f3bf67f))


```


```text
Iterations  :   7
Result  :   T


```


```text
Iterations  :    Result  :
```


Since the committer/prover has free choice of the secret used, no information about the committed value can be deduced from the commitment when the secret is chosen at random.


For the remainder of this post, all commitments will be created with` COMMIT` for simplicity of the examples. Just remember that such commitents can always be made completely hiding if required.


## Functional Commitments


Because Lurk allows commitments to *any* Lurk expression, we can also commit to *functions* .


```text
(commit (  lambda   (x) (  +   7   (  *   x x))))


```


```text
Iterations  :   2
Result  : (comm 0x1aec2d8cfaba19acda002925ad87fa2da559f8ae431b12e96c9f2d16be3b0964)


```


```text
Iterations  :    Result  :
```


The above is a commitment to a function that squares its input then adds seven.


We can construct and evaluate an expression that can only be proven to evaluate to one value: the result of applying the function to a given input, in these examples, 9 and 11.


```text
((  open   0x1aec2d8cfaba19acda002925ad87fa2da559f8ae431b12e96c9f2d16be3b0964)   9  )


```


```text
Iterations  :   12
Result  :   88


```


```text
Iterations  :    Result  :
```


```text
((  open   0x1aec2d8cfaba19acda002925ad87fa2da559f8ae431b12e96c9f2d16be3b0964)   11  )


```


```text
Iterations  :   12
Result  :   128


```


```text
Iterations  :    Result  :
```


[Functional commitments](https://eprint.iacr.org/2016/766.pdf) are an extremely powerful construct.


Because the act of opening a functional commitment reveals only a small amount of information about the hidden function (its output when applied to a single input), functional commitment schemes require that application openings be accompanied with proofs. In order that no information about the committed function be leaked, those must be zero-knowledge proofs. As we have seen above, Lurk implements a language expressive enough to implement such a functional commitment scheme. In later posts, we will demonstrate the process of generating and verifying the proofs that make such schemes truly useful.


## Higher-order Functional Commitments


Interestingly, even though the set of primitive operations Lurk supports is quite small, they enable the possibility of *higher-order functional commitments* ‘for free’.


The following example is almost trivial in its simplicity, but this also highlights the complete generality enabled. Here, we commit to a function that simply applies *its input* to a secret internal value.


```text
(  let   ((secret-data   555  )
(data-interface (  lambda   (f) (f secret-data))))
(commit data-interface))


```


```text
Iterations  :   7
Result  : (comm 0x03836e2e16553f58294acf5eb1322b2ca65ace271cc7527402d7ca5fadced968)


```


```text
Iterations  :    Result  :
```


```text
((  open   0x03836e2e16553f58294acf5eb1322b2ca65ace271cc7527402d7ca5fadced968) (  lambda   (data) (  +   data   111  )))


```


```text
Iterations  :   14
Result  :   666


```


```text
Iterations  :    Result  :
```


We coin the term Higher-Order Functional Commitments, and as far as we are aware, Lurk is the first and only extant system enabling this powerful usage in the bare language, ‘on the metal’, of the proving system itself.


## Minimal but Expressive


What we mean by this is that when Lurk is instantiated with a given backend (Nova over the scalar field of Pallas, in the case of our initial complete implementation), the result is a transparent proving system with a *fixed verification key* . Any statement in the relation represented by Lurk’s deterministic evaluation function can be proved in constant time. This makes Lurk a minimalist (RISC: Reduced Instruction-Set Computer) virtual machine, distinguished by its direct implementation of a content-addressed functional programming language.


The Lurk language relation consists of tuples of the form (exprin,envin,contin,exprout,envout,contout,n)(expr_{in}, env_{in}, cont_{in}, expr_{out}, env_{out}, cont_{out}, n)


(


e


x


p


r


in


​


,


e


n


v


in


​


,


co


n


t


in


​


,


e


x


p


r


o


u


t


​


,


e


n


v


o


u


t


​


,


co


n


t


o


u


t


​


,


n


)


. Put differently, there is a mapping of input triples to a set of (output triple, iteration count), where a triple consists of an expression, a lexical environment, and a continuation. Notice that not every input triple has an output triple corresponding to a completed evaluation. Not only must the input be well-formed to guarantee provable terminal output, it must also correspond to an input expression whose evaluation *terminates* . Since Lurk is Turing-complete, this is not guaranteed. In fact, it is guaranteed that there are many valid inputs that do *not* terminate. However, Lurk *can* prove partial evaluation to any depth. For any n<=tn <= t


n


<=


t


(where tt


t


is the total iteration count of a terminating evaluation), Lurk can provide a proof that the input expression evaluates to the nn


n


th intermediate reduction.


When new backends are implemented, proofs in one system will be data-compatible with those in another. So a hypothetical[Halo2](https://github.com/zcash/halo2) backend over the scalar field of Pallas will accept and reject exactly the same sets of statements as the Nova backend does.


We describe higher-order functional commitments as being implemented ‘on the bare metal’ because no further language constructs need be implemented in order to enable them. While any Turing-complete proving system (Lurk is one of the few, but not the only such system) can — by definition — be used to first implement a functional programming language, then use the resulting language to implement a construct like higher-order functional commitments, only Lurk can do so ‘out-of-the-box’ with no further ado.


While all transparent SNARK systems avoid an additional trusted setup, Lurk further avoids the need for both a per-program verification *and* the need for a separate compilation step to preprocess the program. The ‘instruction set’ of the Lurk VM allows direct evaluation of expressive functional-programming code *as data* . This is a consequence of Lurk’s primary design decision. By implementing Lisp’s EVAL in its core circuit, Lurk exploits recursive SNARKs to circumvent the limitations of bounded circuits and accomplish Turing-completeness with a very small set of primitive operations. Because Lisp’s fundamental ‘memory allocator’ (CONS) can be implemented directly in RAM machines, Lurk allows for extremely concise expression of recursive algorithms over recursive data structures. We avoid the general cost of logarithmic-time lookups in linear memory by enabling computation over natural data structures in which each non-deterministic ‘memory lookup’ is shallow.


The inner Lurk circuit, which implements the[Lurk reduction step](https://github.com/argumentcomputer/lurk-rs/blob/master/notes/reduction-notes.md) , is currently about 20,000 R1CS constraints. With known optimizations, we estimate it can be further reduced to about 10,000 constraints.


The public inputs to a Lurk proof consist of 6 Lurk values (an input and output tuple) and an iteration count, where each Lurk value is represented by two field elements (a type tag and a value). Given ~256-bit (32 byte) field elements, that is 13 * 32 = 416 bytes.


## Application Example


In[Efficient Functional Commitments](https://eprint.iacr.org/2021/1342.pdf) , Boneh et al introduce a credit-score example: a credit bureau wants to preserve the secrecy of its rating algorithm while also proving it applies its proprietary algorithm fairly to all parties. This can be accomplished by first commiting to a function that implements the secret algorithm, then opening function applications on each individual’s credit data. This demonstrates that the *same* function (the one initially committed to) is used in each case, but only the result is revealed.


Although not fully realistic, a map-reduce based example seems sufficiently suggestive.


```text
(letrec ((  reduce   (  lambda   (acc f   list  )
(  if   (  eq   list   nil  )
acc
(  reduce   (f acc (  car   list  ))
f
(  cdr   list  )))))
(  map   (  lambda   (f   list  )
(  if   (  eq   list   nil  )
nil
(  cons   (f (  car   list  ))
(  map   f (  cdr   list  ))))))
(map-reduce (  lambda   (map-fn reduce-fn initial   list  )
(  reduce   initial reduce-fn (  map   map-fn   list  ))))
(plus (  lambda   (a b) (  +   a b)))
(square (  lambda   (x) (  *   x x)))
(secret-function (  lambda   (credit-data)
(map-reduce square plus   0   credit-data))))
(commit secret-function))


```


```text
Iterations  :   15
Result  : (comm 0x0ea21fab822e5414364b70a3763f15901b7512daf814bea5a48d36b8330ca2e4)


```


```text
Iterations  :    Result  :
```


```text
((  open   0x0ea21fab822e5414364b70a3763f15901b7512daf814bea5a48d36b8330ca2e4)   '  (  2   4   7   10  ))


```


```text
Iterations  :   409
Result  :   169


```


```text
Iterations  :    Result  :
```


Although the credit-score algorithm example is compelling, it focuses on the privacy of the credit bureau’s proprietary algorithm. An equally (if not more) important consideration is that of individual data privacy. The complementary example is one in which the algorithm is public, but the credit data is not revealed.


In this case, an individual commits to their private data wrapped in a functional interface (as in theDATA-INTERFACE example above). Then the function implementing the credit algorithm is passed as input to a *higher-order functional commitment application opening* .


```text
(  let   ((credit-data   '  (  2   4   7   10  ))
(data-interface (  lambda   (f) (f credit-data))))
(commit data-interface))


```


```text
Iterations  :   7
Result  : (comm 0x3dd3ad73dfece7ffd7913b5d77a29c58410d347a14d6cf1cb1828fa1abcbb949)


```


```text
Iterations  :    Result  :
```


```text
(letrec ((  reduce   (  lambda   (acc f   list  )
(  if   (  eq   list   nil  )
acc
(  reduce   (f acc (  car   list  ))
f
(  cdr   list  )))))
(  map   (  lambda   (f   list  )
(  if   (  eq   list   nil  )
nil
(  cons   (f (  car   list  ))
(  map   f (  cdr   list  ))))))
(map-reduce (  lambda   (map-fn reduce-fn initial   list  )
(  reduce   initial reduce-fn (  map   map-fn   list  ))))
(plus (  lambda   (a b) (  +   a b)))
(square (  lambda   (x) (  *   x x)))
(credit-score-function (  lambda   (credit-data)
(map-reduce square plus   0   credit-data))))
((  open   0x3dd3ad73dfece7ffd7913b5d77a29c58410d347a14d6cf1cb1828fa1abcbb949) credit-score-function))


```


```text
Iterations  :   426
Result  :   169


```


```text
Iterations  :    Result  :
```


## Onward and upward


Of course, the data-privacy example raises the question of how an individual’s commitment to private credit data might be trustworthy in the first place. Those with domain knowledge may already understand, and clever programmers with good imagination may guess. In future posts, we will demonstrate how functional commitments (including higher-order functional commitments) and their variants can be used to solve this and many other problems. Hopefully the current treatment has provided enough detail that not *too much* imagination is required. Pre-existing knowledge of the constituents should suffice to allow you to see how powerful the expressive and composable toolkit Lurk provides can be. That is the point.


We have built to higher-order functional commitments as an exemplary and *interesting* use case, and we believe the construct is sufficiently general to itself act as a nearly universal primitive. Nevertheless, we note that Lurk can often solve problems even more directly, if not constrained to use that primitive. □\\square


□
