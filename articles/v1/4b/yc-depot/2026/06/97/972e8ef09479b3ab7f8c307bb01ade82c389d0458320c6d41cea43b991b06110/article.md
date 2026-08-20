---
schema_version: "1.0.0"
document_id: "972e8ef09479b3ab7f8c307bb01ade82c389d0458320c6d41cea43b991b06110"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/staying-in-control-of-your-codebase-in-the-ai-era"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:9b8c4a48e468ca9c7e475f3bfa4a0d2c297808934c3df9e6e9bffbc78bd55ca5"
---

# Staying in control of your codebase in the AI era

I love coding, and I love my job. Like many others in the industry, I found it hard to cope with the changes of the last couple of years. At first, I had an existential crisis. Then I started experimenting and watching how people actually work with AI, which greatly reshaped my view. (I really recommend this[video from Jon Gjengset](https://www.youtube.com/watch?v=vmKvw73V394) , where he shows how he works with AI, what it's good for, and how it's ultimately just another tool in your toolbox.)


Embracing AI and steering it to produce maintainable code at an acceptable cost is the name of the game. There are countless smart people out there working on different harnesses and techniques to crack the problem. The current craze is loops, but aside from being too expensive, the approach just doesn't click with me.


There's a difference in effective AI use between working alone and being part of a team on the same project. In the latter case, I have obligations to my employer and my team. Some employment contracts even contain a clause like this: "the Employee shall perform their duties to the best of their ability and skill." I tend to take that line rather seriously. Ridiculous as it may sound, this is my creed. I take great pride in my work, and I plan to keep it that way. I invest heavily in keeping my code maintainable and reviewable, to make life easy for my peers and my future self. Let me show you the AI assisted development workflow I'm currently happy with.


## What works for me


The approach that works today is constraint. Selectively adding guardrails to get more consistent, predictable output. I work with Go the most in my day-to-day, so my examples will use it, but I think these techniques apply to almost any other language. That said, Go has the advantage that different developers tend to produce fairly similar code, thanks to the language's rigid structure.


### Linter


I find that linting is the alpha and omega of steering agents to produce acceptable code. As expected, I'm using the standard` golangci-lint` as my linter. These are the rules I mostly rely on to influence the shape the code takes:


```text
settings:
cyclop:
max  -  complexity:   10
funlen:
lines:   90
statements:   50
revive:
rules:
-   name: argument  -  limit
arguments:
-   8
-   name: file  -  length  -  limit
severity:   error
arguments:
-   max:   1000
skipComments:   true
skipBlankLines:   true
```


Your peers will thank you later for the effort you put into constraining the code's dimensions. Limiting the lines of code in a single file and capping cyclomatic complexity forces the agent to break code down into smaller logical units across multiple files. Function length and the number of allowed function arguments complement these rules, making the code far more readable to humans.


### Testing


Agents are useful for generating (unit) tests to save you from the grunt work. Proper test coverage has always been important, but it matters more than ever in the era of full project rewrites. Take the agent by the hand and don't let go until it can produce sane test cases.


I usually start by writing a few very good test cases manually. I almost always write table-driven tests, so that's a good starting point. Agents can easily generate mocks, but I usually give them a framework to do so. Invest early in coming up with sane interfaces, and force the agent to use those to generate mock structs with deterministic code generators such as moq. Agents like to solve problems in creative ways. Without guardrails, they usually produce working but hard-to-read assertions. GPT 5.4+ likes to write tests that basically "grep" a line of business logic, checking character by character for the existence of a function and calling it a unit test. Weird.


I usually instruct the agent to use the testify package to keep test cases standardized and easy to read. Once the preparations are done, you can just ask the agent to look at your reference test implementation and write future tests based on it.


Take a look at this simple go project:


```text
package   main


import   "  errors  "


type   calculator   struct  {}


func   (  c   calculator  )   add  (  a   int  ,   b   int  ) (  int  ,   error  ) {
if   a   ==   0   ||   b   ==   0   {
return   0  , errors.  New  (  "values cannot be 0"  )
}
return   a   +   b,   nil
}


func   (  c   calculator  )   subtract  (  a   int  ,   b   int  ) (  int  ,   error  ) {
return   a   -   b,   nil
}


func   addWrapper  (  a   int  ,   b   int  ,   c   calculator  ) (  int  ,   error  ) {
return   c.  add  (a, b)
}


func   subtractWrapper  (  a   int  ,   b   int  ,   c   calculator  ) (  int  ,   error  ) {
return   c.  subtract  (a, b)
}


func   main  () {
addWrapper  (  10  ,   10  ,   calculator  {})
}
```


I prompted` Claude 4.8 (1M context) High-thinking` with the following text:


```text
Please take a look at blogpost/main.go. I would like you to write unit tests for addWrapper function. The test should have at least 3 different inputs. Write idiomatic go.
```


The agent actually produced table-driver tests without being asked, and the tests themselves are fine, but we could make them more readable. I'm not happy with how the table-driven tests look, and those assertions could be much easier on the eyes. I'm also not happy with the direct use of` calculator` , which could change at any time, for example by introducing dependencies.


See the agent's first attempt


```text
package   main


import   (
"  testing  "
)


func   TestAddWrapper  (  t   *  testing  .  T  ) {
tests   :=   []  struct   {
name      string
a         int
b         int
want      int
wantErr   bool
}{
{
name:   "two positive values"  ,
a:      10  ,
b:      10  ,
want:   20  ,
},
{
name:   "positive and negative values"  ,
a:      5  ,
b:      -  3  ,
want:   2  ,
},
{
name:      "zero operand returns error"  ,
a:         0  ,
b:         7  ,
wantErr:   true  ,
},
}


for   _, tt   :=   range   tests {
t.  Run  (tt.name,   func  (  t   *  testing  .  T  ) {
got, err   :=   addWrapper  (tt.a, tt.b,   calculator  {})


if   tt.wantErr   &&   err   ==   nil   {
t.  Fatalf  (  "addWrapper(%d, %d) error = %v, want %v"  , tt.a, tt.b, err, tt.wantErr)
}
if   got   !=   tt.want {
t.  Errorf  (  "addWrapper(%d, %d) = %d, want %d"  , tt.a, tt.b, got, tt.want)
}
})
}
}
```


Lets make some changes to the main file, and create a reference unit test implementation:


```text
package   main


import   "  errors  "


var   errZeroValues   =   errors.  New  (  "values cannot be 0"  )


//go:generate moq -stub -out calculator_mocks.go . Calculator
type   Calculator   interface   {
add  (  a   int  ,   b   int  ) (  int  ,   error  )
subtract  (  a   int  ,   b   int  ) (  int  ,   error  )
}


type   calculator   struct  {}


if   a   ==   0   ||   b   ==   0   {
return   0  , errZeroValues
}
return   a   +   b,   nil
}


return   a   -   b,   nil
}


func   addWrapper  (  a   int  ,   b   int  ,   c   Calculator  ) (  int  ,   error  ) {
return   c.  add  (a, b)
}


func   subtractWrapper  (  a   int  ,   b   int  ,   c   Calculator  ) (  int  ,   error  ) {
return   c.  subtract  (a, b)
}
```


I introduced a sentinel error called` errZeroValues` . I also added a Calculator interface, along with a generator that writes a mock to` calculator_mocks.go` . Now let's write our reference test implementation:


```text
package   main


import   (
"  testing  "


"  github.com/stretchr/testify/require  "
)


var   mockCalculator   =   &  CalculatorMock  {
subtractFunc:   func  (  a  ,   b   int  ) (  int  ,   error  ) {
return   a   -   b,   nil
},
}


func   TestSubtractWrapper  (  t   *  testing  .  T  ) {
tcs   :=   map  [  string  ]  struct   {
a         int
b         int
want      int
wantErr   error
}{
"positive and positive values"  : {
a:      10  ,
b:      10  ,
want:   0  ,
},
"positive and negative values"  : {
a:      10  ,
b:      -  10  ,
want:   20  ,
},
"negative and negative values"  : {
a:      -  10  ,
b:      -  10  ,
want:   0  ,
},
"zero operands"  : {
a:      0  ,
b:      0  ,
want:   0  ,
},
}


for   name, tc   :=   range   tcs {
t.  Run  (name,   func  (  t   *  testing  .  T  ) {
got, err   :=   subtractWrapper  (tc.a, tc.b, mockCalculator)
require.  NoError  (t, err)
require.  Equal  (t, tc.want, got)
})
}
}
```


Lets see what the next prompt produces:


```text
Please re-read blogpost/main.go, and the reference test implementation in blogpost/main_test.go. I would like you to follow the practices seen in the TestSubtractWrapper reference unit test, and write TestAddtWrapper.
```


The agent extended the mock and used require, just as asked. (Please ignore the fact that the mock and the real implementation are the same; this is just a small example.) This shows that agents are capable of producing sane unit tests, given some investment in crafting good examples.


See the agent's revised attempt


```text
package   main


import   (
"  testing  "


"  github.com/stretchr/testify/require  "
)


var   mockCalculator   =   &  CalculatorMock  {
addFunc:   func  (  a  ,   b   int  ) (  int  ,   error  ) {
if   a   ==   0   ||   b   ==   0   {
return   0  , errZeroValues
}
return   a   +   b,   nil
},
subtractFunc:   func  (  a  ,   b   int  ) (  int  ,   error  ) {
return   a   -   b,   nil
},
}


func   TestSubtractWrapper  (  t   *  testing  .  T  ) {
tcs   :=   map  [  string  ]  struct   {
a         int
b         int
want      int
wantErr   error
}{
"positive and positive values"  : {
a:      10  ,
b:      10  ,
want:   0  ,
},
"positive and negative values"  : {
a:      10  ,
b:      -  10  ,
want:   20  ,
},
"negative and negative values"  : {
a:      -  10  ,
b:      -  10  ,
want:   0  ,
},
"zero operands"  : {
a:      0  ,
b:      0  ,
want:   0  ,
},
}


for   name, tc   :=   range   tcs {
t.  Run  (name,   func  (  t   *  testing  .  T  ) {
got, err   :=   subtractWrapper  (tc.a, tc.b, mockCalculator)
require.  NoError  (t, err)
require.  Equal  (t, tc.want, got)
})
}
}


func   TestAddWrapper  (  t   *  testing  .  T  ) {
tcs   :=   map  [  string  ]  struct   {
a         int
b         int
want      int
wantErr   error
}{
"positive and positive values"  : {
a:      10  ,
b:      10  ,
want:   20  ,
},
"positive and negative values"  : {
a:      10  ,
b:      -  10  ,
want:   0  ,
},
"negative and negative values"  : {
a:      -  10  ,
b:      -  10  ,
want:   -  20  ,
},
"zero operands"  : {
a:         0  ,
b:         0  ,
want:      0  ,
wantErr: errZeroValues,
},
}


for   name, tc   :=   range   tcs {
t.  Run  (name,   func  (  t   *  testing  .  T  ) {
got, err   :=   addWrapper  (tc.a, tc.b, mockCalculator)
if   tc.wantErr   !=   nil   {
require.  ErrorIs  (t, err, tc.wantErr)
return
}
require.  NoError  (t, err)
require.  Equal  (t, tc.want, got)
})
}
}
```


### Agents.md and Claude.md


I often think my Markdown files might look naive to agentic power users, but I like to keep things simple, and it seems to work for me. This is the basic structure I usually start with:


```text
# AGENTS


<Short description what the project is about>


# Main components
<A list of the main components and their path within the project with a very short description>


# Most important go packages
<A list of important go packages and what they should be used for>


# Development


`make build` cross compiles to linux
`make generate` generates the gRPC files
`make test` executes the unit-tests
`make lint` executes the linter check (needs docker)
`make lint-proto` lints the proto files


## Coding style
- The code should be idiomatic go
- You should listen to the linter all the time
- Code should be self-explanatory. prefer code readability over comments
- Comments should be short, to the point
- Comments shouldn't include ticket numbers
- Comments shouldn't include made up worlds and phrases not in the english dictionary
- New files shouldn't extend over 1000 lines of code (excluding comments and whitespaces)
- Think twice before adding code to files where the number of lines in a file already surpass 700
- Unit tests are nice, but don't write tests just for the sake of writing tests
- When adding new unit-tests, keep the style from the existing tests
- PRs should be small, encapsulating a single feature or fix.
- If you need to stuff multiple features or related changes into a PR, oranize them into separate commits


# Deployment
<How to deploy to STAGING>. # deploying to production should follow process
```


That's all. It overlaps somewhat with what we covered earlier on linter rules and unit tests, but I find that the more we surround the agent with walls, the more deterministic and higher-quality its output becomes.


The remarks about comments are probably the interesting part. I've found that Opus 4.7+ and GPT 5.4+ models produce complete garbage in their comments when not regulated. Let's take a look at the following comment:


```text
exitCode, exitErr   :=   command.  GetExitState  ()
if   exitErr   !=   nil   {
// Synthetic "exit status N" goes on the legacy line rail only; it
// would corrupt callers that concatenate StderrRaw to reconstruct
// the process stderr (DEP-5505).
if   err   :=   stream.  Send  (  &  somepackage  .  ExecuteResponse  {
ExitCode: exitCode,
Stderr:   exitErr.  Error  (),
}); err   !=   nil   {
return   fmt.  Errorf  (  "send exit error to stream: %w"  , err)
}
}
```


1. It includes a ticket number for no good reason.
2. It contains a made-up expression like legacy line rail.
3. It refers to code it just changed as "legacy."
4. It takes real mental effort just to understand what the comment is even about. What does "synthetic" even mean in the context of exit codes?


Furthermore, I've observed that after slightly changing a function, GPT 5.4+ likes to write something like: "Previously myfunction was working like this … (and then it details how the function worked before the change), but since TICKET-NUMBER, the function now does this … ." I'm not sure why the models write comments like this, but I'm pretty sure this is what Git is for. To add to that, in Rust code GPT 5.4+ will happily write comments 50 to 100 lines long. It would be insane to burden my peers and my future self with that much noise, so I like to guide the agent on how to write sane comments, and I think you probably should too. As a bonus, you might want to set up Cursor Bugbot or another reviewer agent to check comments against a rubric on open PRs.


## Processes


So far, we've worked to produce sane agentic code that resembles something the team would expect from other engineers. To pass the ceremony of validating and merging code into the codebase, we need to take a few extra steps.


### PR descriptions


When I'm not working across a lot of parallel lanes, I still like to write PR descriptions. I aim to keep them short and to the point, and I occasionally include screenshots of the product changes, which help peers understand them. When I have the agent write them, I hold it to the same rules I described earlier for comments.


### Making PRs reviewable


As a rule of thumb, I try to keep changes small, say under 200 changed lines overall. Of course, sometimes that isn't possible: there are large refactors and new features. When the changeset is sufficiently large, I try to decompose the changes into separately reviewable commits. You can split the changes into commits by logical boundaries, modules, packages, and so on. Whatever works best for your team. Agents are insanely good at this; you can just tell them to break your change into small commits right before opening a PR. Another promising approach is GitHub[stacked PRs](https://github.github.com/gh-stack/) . The feature is in preview, but it makes reviewing code so much easier.


## The missing cherries on top


I'm quite happy with my current workflow, since these simple tricks can greatly improve the quality of the code produced, but there's always room for improvement. There are a few things on my wishlist that would make life easier:


### Organization level Agents.md


It would be so nice if GitHub (and other forges) supported defining organization-level AGENTS.md directives that would merge with repo-level Markdown files. This way, I could define global defaults (` comment styling` ), and the repo-specific directives (` deploy` ,` build` , etc.) would stay in the repositories.


### Closing the feedback loop


Since Opus 4.7 and GPT 5.4, agents have started using some REALLY weird tools. For example, I often see them run Perl scripts. Other times, instead of editing a file, they make a copy, then move the new file in to replace the original. Sometimes they offer to run a shell script so exotic that I'd rather ask them to do something else. An obvious solution is to run the agents in (I'm not super happy with the local isolation levels agents provide). Running them in a remote sandbox solves the isolation problem, but takes away the instant feedback I get locally. I'd like to run my integration suite after each remote modification to my codebase, so it mirrors my local workflow. It's absolutely doable, but involved, and as far as I know no provider supports it natively.


## Haven’t you heard that coding is solved?


You might be thinking, "But Peter, so many words wasted on code quality, when coding is already solved. Nobody needs to read code anymore!"


I respectfully beg to differ. LLMs make a lot of mistakes even today, and the quality of the code they produce is far from deterministic or uniform. Furthermore, I'm responsible for the work I release, or that goes out under my supervision. After all, I'm the one on call, not Claude. There are many cases where I don't care about code quality at all, such as quick experiments, small scripts, visualizations, and so on. But until the day LLMs stop making mistakes (that day might come soon), and you can't tell human code from agent code, expect me to show up and read all the code. To the best of my ability and skill, as the contract says.


## FAQ


How do I get AI agents to produce more consistent code?


Constraint. Lean on your linter. Capping things like cyclomatic complexity, function length, file length, and the number of function arguments forces the agent to break work into smaller logical units across multiple files. The more guardrails you put up, the more deterministic and readable the output gets. I rely on golangci-lint with cyclop, funlen, and revive rules for exactly this.


How can I make AI-generated unit tests more readable?


Write a good reference test first, then point the agent at it. I write a few solid table-driven tests by hand, standardize on testify for assertions, and use moq with an interface to generate deterministic mocks. Once you have one clean example checked in, you can just ask the agent to follow that style for the rest of your tests.


Why does the agent keep writing strange comments, and how do I stop it?


Recent models love to drop in ticket numbers, made-up phrases, and play-by-play notes about how a function used to work before the change. Git already tracks that history, so it's pure noise. My fix is to spell out comment rules right in AGENTS.md: keep them short, no ticket numbers, no invented jargon. Setting up a reviewer agent like Cursor Bugbot to check comments against a rubric on open PRs helps too.


Can I run agents in a remote sandbox without losing fast local feedback?


Not cleanly yet. Remote sandboxes solve the isolation problem, so you're not letting an agent run some exotic shell script on your machine, but you lose the instant feedback you get locally. What I want is to run my integration suite after each remote change so it mirrors my local workflow. It's absolutely doable, just involved, and as far as I know no provider supports it natively today.


## Related posts


- [Using AI as my engineering copilot (not autopilot)](https://depot.dev/blog/using-ai-as-my-engineering-copilot-not-autopilot)
- [The bottleneck has shifted from writing code to integrating it](https://depot.dev/blog/the-bottleneck-has-shifted)
- [Paranoia is baseline now: Security in the AI era](https://depot.dev/blog/paranoia-is-baseline-now)


Peter Hasko-Nagy


Staff Engineer at Depot
