---
schema_version: "1.0.0"
document_id: "d02a96df10a3ebc8e83a040d62a324e43c079eb889b411e19150a7843af4e563"
company_key: "yc-wave"
company: "Wave"
source_id: "yc-wave-rss-404ae0f13af9"
canonical_url: "https://www.wave.com/en/blog/coding/"
published_at: "2019-08-01T19:17:23+00:00"
first_seen_at: "2026-07-24T06:52:44.556575+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:2a73b2cd35ba6a083740dab7d037f820fa55fa8df3170126b784b1540299c597"
---

# Our design and coding guidelines

# Our design and coding guidelines


Ben Kuhn


on


August 1, 2019


engineering


---


This document grew out of our internal “questions to ask during code review” checklist: we realized that if we turned the questions into advice, it made a great summary of what we think makes code great. We’re publishing it here so that if you’re interested in working at Wave, you can see whether your taste meshes with ours–and if you’re interviewing, you can see how you’ll be evaluated.


## Ship.


We succeed by learning new things, and we learn by shipping. Therefore, the faster we ship, the faster we succeed. Don’t waste time dithering about names when we could be shipping. We can always fix it later. (If it’s something we can’t fix later, then you’re allowed to dither—or even better, ask for design help.)


Why do we care about code quality at all, then? Because it’s a marathon, not a sprint, and we want to still be shipping fast 2 years from now. Don’t spend time dithering about names we can change later—but *do* invest in building a *reflex* of coming up with great names. (And yes, that can mean slowing down a little bit today—but focus on learning the underlying heuristics and mental processes, not on making all the code perfect right now.)


## Names


Naming things well is the most important single thing we can do to make our code easy to understand.


From reading a function’s name and signature, a reader should be able to guess its full behavior—think` locks.acquire_or_wait` . (If we can’t think of a name satisfying this property, maybe we need a betterAbstraction .) If readers can do that, then when they’re reading code that calls the function, they won’t have to context-switch to read the docstring—or worse, make a bad assumption about what the function does.


Names can be shorter the more core something is. We think about peer-to-peer money transfers constantly, so` logic.p2p` is an okay abbreviation. We don’t use` DeviceKeyset` s constantly, so they don’t get to be called` dks` .


## Simplicity


Keep the code as simple as possible. Simplicity can mean many conflicting things — none of these are hard and fast — but here are some common examples: Don’t save something in the db when we could compute it on the fly. Don’t use a class where we could use a function. Don’t hide behind an interface when there’s only one implementation. Don’t use an` if/else` when just an` if` with no` else` would work; don’t use` if` at all when the code could go in a straight line. Don’t add a new piece of infrastructure if we can get away without it. Don’t write code yourself if we can pay someone else to. Every branch, layer of indirection, or piece of state makes it harder to get a full-stack understanding of the system.


For example, compare the code samples of the best-in-class HTTP library from Python vs Java. (Python’s design culture values simplicity; Java’s doesn’t.)


```text
import    requests
print  (  requests  .  get  (  "https://www.google.com"  )  .  text  )


```


versus:


```text
import    okhttp3.*  ;


public    static    void    main  (  String  []    args  )    throws    IOException    {
OkHttpClient    client    =    new    OkHttpClient  ();


Request    request    =    new    Request  .  Builder  ()
.  url  (  "https://www.google.com/"  )
.  build  ();


Response    response    =    client  .  newCall  (  request  ).  execute  ();
System  .  out  .  println  (  response  .  body  ().  string  ());
}


```


Or compare these two APIs for getting a customer’s bank account transaction history:[Plaid](https://plaid.com/docs/api/#user-flow) vs[Yodlee](https://developer.yodlee.com/Yodlee_API/API_Flow/Add_Account_With_Webhooks) .


## Abstraction


Abstractions (classes, functions, modules, database tables) should do one thing, in the most predictable possible way.


“One thing” can operate at multiple levels. Some functions do a single low-level thing (“check if this password matches this hash”), and some do a single high-level thing (“send a money transfer from wallet A to B”)—using multiple lower-level pieces. But the higher-level functions should still correspond to a single cohesive concept. (As one rule of thumb, if a function’s name or one-line summary includes` and` , it might be a bad abstraction.)


For example, the code responsible for sending text messages to[Twilio](https://www.twilio.com/) shouldn’t be intertwingled with the code for persisting text-message info to the database. If we split those tasks into two different modules (and add a third wiring them together), each one will be easier to understand, test and modify.


A great acid test for a good abstraction is to imagine how it would accommodate future changes to user-facing functionality. (Don’t build unnecessary support for those changes right now, because[you ain’t gonna need it](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) . But if an abstraction can easily accommodate a few types of change, that’s a good sign that it[“carves nature at the joints”](https://plato.stanford.edu/entries/natural-kinds/) .)


For instance, right now we only make a single attempt to send a given text message. But when we were designing the SMS system, thinking about possible retry behavior led us to make a clean conceptual separation between a “text message” (a string of text that we want to appear on a user’s phone) and a “text message attempt” (a single attempt at sending them the message). If we wanted to add retries now, we wouldn’t have to touch the database persistence code or the third-party integrations, only the part of the logic that triggers the attempts.


Good abstractions:


-


` models.ledger` keeps our core transaction ledger as simple as possible (but no simpler) by providing a tightly-scoped interface to this extremely safety-critical code.


We made a bunch of decisions here to reduce coupling at the cost of writing more code. For instance, the ledger uses different database sessions from the rest of our code (so that it can easily use SERIALIZABLE isolation and have complete control over its own database transactions), and we banned having real foreign keys to the ledger or joining to it from normal database queries, so that it could be as isolated as possible from the application logic.


-


` models.bill_types` : does a relatively complex job with minimal ceremony (it allows the server to control the list of data fields required for each type of bill, which lets us introduce new bill types without shipping client updates).


Bad abstractions:


- A long time ago, the Sendwave and Wave codebases were the same codebase, and in fact, shared the same` users` table for international vs domestic users. This was fine for a while, but eventually we ended up with completely different user models (e.g. logging in with email/password vs mobile/pin/SMS-code; having a debit card vs. a ledger account)—and we ended up with lots of problems from e.g. domestic users getting blocked by international anti-fraud rules. We should have split it into 2 different abstractions.
- The git command line interface. Git has an[extremely elegant internal model](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) , but the high-level actions exposed by the command line[make no sense](http://stevelosh.com/blog/2013/04/git-koans/) .


## Foolproofing


Any piece of knowledge should be represented in exactly one place in the code or data. That way, when the information changes, only one place in the code/data has to be updated. If we do have to make assumptions about behavior in other parts of the codebase, document those assumptions and figure out how to enforce them.


This holds even at the most local level. Define interfaces so that it’s impossible to mess up the implementation. Pick function signatures so that it’s impossible to pass invalid parameters. Structure database columns so that the table can never contain invalid data. (In the database world this is known as[“normalization.”](https://www.geeksforgeeks.org/database-normalization-normal-forms/) )


(Be pragmatic about normalization. Sometimes it’s easier to prevent errors with assertions or constraints, than to prevent them with clever use of the type system. For instance, databases are often denormalized to make queries perform better.)


**Bad foolproofing:** Consider the following code, which encrypts a byte string in Java:


```text
import    javax.crypto.*  ;


byte  []    ciphertext  ,    key  ;
try    {
Cipher    cipher    =    Cipher  .  getInstance  (  "AES/ECB/PKCS5Padding"  );
SecretKeySpec    secretKeySpec    =    new    SecretKeySpec  (  key  )
cipher  .  init  (  Cipher  .  ENCRYPT_MODE  ,    secretKeySpec  );
byte  []    ciphertext    =    cipher  .  doFinal  (  plaintext  );
}    catch    (  NoSuchAlgorithmException
|    NoSuchPaddingException
|    InvalidKeyException
|    InvalidAlgorithmParameterException
|    IllegalBlockSizeException
|    BadPaddingException    e  )    {
// these will provably never be raised!
throw    new    RuntimeException  (  e  );
}


```


All of this brain-deadness stems from the fact that` Cipher` is way too generic, trying to handle every type of crypto operation in a single class.


- The same class is used for encryption and decryption, toggled by a flag passed to` init`


- So the encrypt method needs to be named` doFinal` and you need to catch` BadPaddingException` even though it only ever happens on decrypt.


- You supply the algorithm name as a string, rather than by constructing a different` Cipher` subclass


- So you need to check for` NoSuch{Algorithm,Padding}Exception`
- And you need to call` getInstance` and then` init` in two separate lines, making it possible to have a` Cipher` exist in an uninitialized state
- And the` init` function has to take a generic` AlgorithmParameterSpec` instance, even though we know that for AES it has to be a` SecretKeySpec` instance. Which is why we need to catch` InvalidKeyException` and` InvalidAlgorithmParameterException` .


If we instead had an abstract class` Cipher` with concrete subclasses for different ciphers, we could write` new AESCipher(key).encrypt(plaintext)` in one line.


## Organization


Well-organized code makes it much, much easier to understand a new codebase, which lets new engineers get productive more quickly. Since speeding up onboarding and training has[superlinear benefits](https://lethain.com/productivity-in-the-age-of-hypergrowth/) , keeping our code organized is incredibly valuable.


For instance, we divide our backend code into different layers depending on what its job is (business logic, database access, web UI, third-party integrations, etc.). Within each layer, we keep individual modules similarly decoupled and avoid dependency cycles. The particular layer choices aren’t that important, but what matters is that we try hard to respect them, extend them when necessary, and resist having code expand haphazardly wherever is easiest.


## Tests


Invest a lot in tests! Great test coverage is the only way to ship quickly with a large team. Since iterating quickly is the way we succeed, tests are really important.


Invest in tests at the appropriate time. When we’re building a new UI, if it’s not widely used and we’re iterating quickly, it might not be worth writing tests—UIs are hard to test and they change fast. But once things are more stable, go back and test what we just shipped! (Make sure you work with the product manager to budget time for tests and figure out when the right moment is.)


Good tests…


- don’t break unless there’s an actual bug in the code under test. (This is impossible to fully achieve, but should be the goal.) For instance, don’t directly call prod code to set up test fixtures (use` unittests.model_factories` instead)—otherwise, if the prod code’s interface changes, every test will need to be refactored.
- are *pure* —they’re completely deterministic and don’t have side-effects that change how other tests run. For instance, avoid concurrency (or if concurrency is necessary, synchronize carefully). Use[freezegun.freeze_time](https://github.com/spulec/freezegun) to avoid depending on the system clock.
- are *orthogonal* —having the least possible overlap in the code that they test. For example, we used to test all of our business logic by mimicking a full call to an HTTP endpoint. Each endpoint would be called by 10 or 15 different tests, testing different parts of the underlying logic. When we deprecated our original HTTP API, we had to keep the code around, *solely* because it was used by a bunch of tests. Instead, most tests should have called the underlying Python functions directly.


Invest a lot in making tests easy, so that people write more of them. It’s particularly useful to have helpers for constructing fixture data with sane defaults (` model_factories` ) and for making complex assertions (` assertlib` ). Investing in the usability of these libraries will pay huge dividends.


## Documentation


The best use of documentation is for what’s not already embedded in the code:


- What is the purpose of this code?
- How do all its moving parts fit together?


Each source file should include a comment at the beginning explaining this. Write this comment for an engineer who’s seeing the code for the first time. Cross-reference copiously. Diagrams are encouraged (see our Lucidchart playbook).


When documenting individual classes/functions, focus on their assumptions, invariants, possible failure modes (e.g. exceptions thrown), and how they relate to the rest of the code. Don’t write docs that are redundant with the name or type annotations.


Don’t use documentation as a crutch to patch around bad design. If some code needs a huge docstring or a long explanation, this is a sign that we’ve built something too complicated—consider whether we should improve itsname ,abstraction ororganization .


## Comments


Use comments to explain the “why” behind a particular line of code when it’s not obvious, or to help future authors improve the code (by leaving a` TODO` or warning them not to copy an outdated pattern).


Consider first whether we could remove the need for the comment by simplifying the code. For instance, instead of describing a variable, work on itsname . Instead of using comments to delineate subsections of a function, consider breaking them out into sub-functions. Instead of writing “if you change something here, remember to make a matching change somewhere else,” tryfoolproofing instead.


Good comments:


-


An explanation in the ledger of why we do database operations in a certain order within our ledger:


```text
# Create the LedgerTransaction row. We delay until this point to ensure that its
# transaction ID and timestamp are created *after* we read the amounts,
# thus guaranteeing that transaction IDs and timestamps are monotonic (if you
# have a higher transaction ID, you observed a later set of balances) and
# suitable for sorting on.
#
# Proof: ...


```


-


` csvfile.writerow("\\ufeff") # UTF-8 BOM helps excel show this file correctly`


-


` TODO(lincoln): Update this to use typing.Type once we have it (py3.6)`


Bad comments:


- ` # find the date of the most recent finished day in the loan's timezone` (followed by a few lines of datetime manipulation).


- Part of the computation should probably be factored out into a function` timelib.get_date(timestamp, timezone)` , which would make this code self-documenting.


- ` u = g.session.get(User, u_id=u_id) # fetch the user` (redundant / extra noise)


## Further reading


John Ousterhout,[A Philosophy of Software Design](https://www.amazon.com/Philosophy-Software-Design-John-Ousterhout/dp/1732102201)


Jasmin Blanchette,[The Little Handbook of API Design](https://people.mpi-inf.mpg.de/~jblanche/api-design.pdf)


Butler Lampson,[Hints for Computer System Design](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-17.pdf)


D. L. Parnas,[On the Criteria To Be Used in Decomposing Systems into Modules](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf)


Sandi Metz,[The Wrong Abstraction](https://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction)


Yossi Kreinin,[Redundancy vs. Dependencies: Which is Worse?](http://yosefk.com/blog/redundancy-vs-dependencies-which-is-worse.html)


---


We work on Wave because we think it’s[an extremely effective way to improve the world](https://www.wave.com/en/blog/world/) . If that’s how you want to spend your career too,[come work with us](https://www.wave.com/en/careers/) !


---


If you liked this post, you can subscribe to[our RSS](https://www.wave.com/en/blog/index.xml) or our mailing list:
