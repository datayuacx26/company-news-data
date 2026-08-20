---
schema_version: "1.0.0"
document_id: "741f307c4a50cd260caf1546fb4fd5fbd7d0073670e99916564c93e8431913c0"
company_key: "yc-sweep"
company: "Sweep"
source_id: "yc-sweep-rss-0fc78e2214cd"
canonical_url: "https://blog.sweep.dev/posts/jetbrains-coding-agent"
published_at: "2025-11-03T00:00:00+00:00"
first_seen_at: "2026-07-24T02:57:34.319784+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:7fbdbad8172df2f72ffef11ed4b124ed62567ba160cd44f9a24f5e398786daf7"
---

# Decreasing agentic code editing failures with output normalization

[posts](https://blog.sweep.dev/posts/autocomplete-context) Decreasing agentic code editing failures with output normalization


# Decreasing agentic code editing failures with output normalization


William Zeng


•


November 03, 2025


*Building a better coding agent for JetBrains.*


---


Editing code is the most error-prone step in any coding agent.


## How Coding Agents Work


For those unfamiliar, coding agents work by taking a user request and repeatedly calling tools to complete it.


Tools are functions where the LLM provides the arguments and the agent (or harness) executes the function with those arguments.


For example, if you want an LLM to check stock prices, you would first want to implement a tool to fetch the stock price (by calling the correct api).


```text
def   check_stock  (ticker:   str  ):
# logic to check stock price
return   stock_price
```


Then the LLM can provide the correct args to “check_stock”:


```text
{
"check_stock"  : {
"ticker"  :   "AAPL"
}
}
```


Here’s an example of tool calling in a coding agent.


User


```text
write tests for the pi approximation logic in approximate_pi.py
```


Assistant


```text
I'll write comprehensive tests for the approximate_pi.py file. Let me first examine the file to understand its structure and functionality.
{
"read_file"  : {
"file_path"  :   "sweepai/approximate_pi.py"
}
}
```


The LLM calls the read_file tool and then our harness reads and returns the file contents:


User


```text
These are the contents of `sweepai/approximate_pi.py`:


def approximate_pi(num_points=  1000000  ):
"""
Approximate pi using Monte Carlo simulation.
"""
# ... rest of the code
return pi_approximation
```


This allows the LLM to generate tests by calling the create_file tool.


Assistant


```text
Now I'll create comprehensive tests for this Monte Carlo pi approximation function.
{
"create_file"  : {
"file_path"  :   "tests/test_approximate_pi.py"  ,
"content"  :   "import pytest  \n  import math  \n  import random  \n  from unittest.mock import patch  \n  from sweepai.approximate_pi import approximate_pi..."
}
}
```


Then the LLM can run the unit tests or ask if the user would like to make additional changes.


## How LLMs Edit Code


Surprisingly, most errors in coding agents occur when editing files. This involves the LLM reading the latest state of a file and expressing some form of “edit”.


Each LLM has its own preferred tool for editing files. GPT-5 performs best when using` apply_patch` (which looks like a unified diff), while Claude models like to call a tool called string replace.


### What is String Replace?


` str_replace` involves specifying the old string that the LLM wants to change and the new string to replace the old string with.


For example, if I asked Claude to rename my function from` approximate_pi` to` approx_pi` it would express the edit like this:


```text
{
"str_replace"  : {
"file_path"  :   "sweepai/approximate_pi.py"  ,
"old_string"  :   "  def approximate_pi(num_points=1000000):  "  ,
"new_string"  :   "  def approx_pi(num_points=1000000):  "
}
}
```


The function name should look like this after the edit:


```text
def   approx_pi  (num_points  =  1000000  ):
"""
Approximate pi using Monte Carlo simulation.
"""
# ... rest of the code
return   pi_approximation
```


### Why String Replace Fails


Why do LLMs struggle at this?


When we looked at production error rates, we noticed that str_replace was by far the tool with the most errors. It had at an error rate of ~13%, while a tool like “read_file” or “search” would have sub 1% error rate.


#### Tool Error Rates


77% of str_replace failures occurred because the code the agent tried to replace didn’t exist in the file (the “old_string” couldn’t be found). This happens in two main scenarios:


1. **LLM hallucination** : The model generates code that doesn’t exist in the file, or remembers it slightly differently than the actual file state.
2. **State drift** : The file changes between when the agent reads it and when it tries to edit it.


Here’s a concrete example of state drift: Suppose I ask the agent to rename` approximate_pi` to` approx_pi` . The agent reads the file and sees:


```text
def   approximate_pi  (num_points  =  1000000  ):
```


But before the agent can apply its edit, I manually change the parameter name from` num_points` to` points` . Now the file contains:


```text
def   approximate_pi  (points  =  1000000  ):
```


When the agent tries to replace` def approximate_pi(num_points=1000000):` with` def approx_pi(num_points=1000000):` , the edit fails because the old_string no longer matches what’s in the file.


While some level of failure is expected in these scenarios, a 10% overall failure rate was higher than I anticipated. So I dug deeper to understand what was causing so many mismatches.


### An Invisible State Drift


This was due to an interaction between the JetBrains IDE and Claude models.


A lesser known feature of JetBrains IDEs is that saving will remove any trailing whitespaces in the file.


This is enabled for ALL IDEs by default. This shouldn’t cause any issues if LLMs wrote nicely formatted code.


The actual problem occurs because Claude *loves* adding trailing whitespace between newlines. I asked Claude to generate this script in[claude.ai](https://claude.ai/) , and it added trailing whitespace between each of the newlines:


This causes a problematic sequence of events when using Claude as a coding agent:


1. Claude generates a new function. Let’s call this string` A`
2. The agent harness inserts this code into the IDE.
3. The IDE auto-formats` A` ->` A'` (deleting all trailing indentation). Claude doesn’t see this.
4. Claude tries to make a new edit` A` ->` B` , but` A` no longer exists in the file (only` A'` does).


The edit will fail because the state has drifted, forcing Claude to re-read the file and retry the edit. This wastes developer time, burns through tokens, and bloats the context window with redundant tool calls.


## Preventing state drift


There are a couple of ways to fix state drift. The most direct solution is prompting - we could add instructions like “don’t generate trailing indentation”, which would fix most cases.


However, this approach has two significant drawbacks:


### 1. Enforcing formatting reduces model intelligence


Requiring the model to follow strict formatting rules adds cognitive overhead that can degrade output quality. This has been documented by Aider ([LLMs are bad at returning code in JSON](https://aider.chat/2024/08/14/code-in-json.html) ), and we can observe subtle differences even in simple tasks.


To illustrate this I tested two prompts with Claude (click to see the responses):


1. “write a simple script to approximate pi”


```text
#!/usr/bin/env python3
"""
Simple script to approximate pi using the Monte Carlo method.


The method works by randomly placing points in a unit square and
counting how many fall inside a quarter circle. The ratio of points
inside to total points approximates pi/4.
"""


import   random


def   approximate_pi  (num_points  =  1000000  ):
"""
Approximate pi using Monte Carlo simulation.


Args:
num_points: Number of random points to generate (default: 1,000,000)


Returns:
Approximation of pi
"""
points_inside_circle   =   0


for   _   in   range  (num_points):
# Generate random point in unit square [0,1] x [0,1]
x   =   random.random()
y   =   random.random()


# Check if point is inside quarter circle (radius = 1)
if   x  *  x   +   y  *  y   <=   1  :
points_inside_circle   +=   1


# pi/4 ≈ (points inside circle) / (total points)
pi_approximation   =   4   *   points_inside_circle   /   num_points


return   pi_approximation


if   __name__   ==   "__main__"  :
# Try different numbers of points to see convergence
test_sizes   =   [  1000  ,   10000  ,   100000  ,   1000000  ]


print  (  "Approximating π using Monte Carlo method:  \n  "  )
print  (  f  "  {  'Points'  :<12  }   {  'Approximation'  :<15  }   {  'Error'  :<10  }  "  )
print  (  "-"   *   40  )


import   math
actual_pi   =   math.pi


for   size   in   test_sizes:
pi_approx   =   approximate_pi(size)
error   =   abs  (pi_approx   -   actual_pi)
print  (  f  "  {  size  :<12  }   {  pi_approx  :<15.10f  }   {  error  :<10.10f  }  "  )


print  (  f  "  \n  Actual π:      {  actual_pi  :.10f  }  "  )
```


2. “write a simple script to approximate pi. don’t generate trailing whitespace”


```text
import   random


def   approximate_pi  (num_points):
"""
Approximate pi using Monte Carlo method.
Generate random points in a unit square and count how many fall inside
the unit circle. The ratio gives us pi/4.
"""
inside_circle   =   0


for   _   in   range  (num_points):
x   =   random.random()
y   =   random.random()


# Check if point is inside the unit circle
if   x  *  x   +   y  *  y   <=   1  :
inside_circle   +=   1


# pi/4 = inside_circle / total_points
pi_estimate   =   4   *   inside_circle   /   num_points
return   pi_estimate


if   __name__   ==   "__main__"  :
# Try with different numbers of points
for   n   in   [  1000  ,   10000  ,   100000  ,   1000000  ]:
pi_approx   =   approximate_pi(n)
error   =   abs  (pi_approx   -   3.14159265359  )
print  (  f  "Points:   {  n  :>7  }   | π ≈   {  pi_approx  :.10f  }   | Error:   {  error  :.10f  }  "  )
```


The first response (without formatting) contains a longer and higher quality explanations, while the second response contains a slight error in its comment.


The second response’s comment` # pi/4 = inside_circle / total_points` incorrectly uses an equals sign when the first response uses approximation (≈).


These subtle degradations happen because formatting constraints shift Claude away from its natural output distribution. The model’s training distribution represents its “best” responses - forcing it to generate different token sequences (like` "\\n\\n"` instead of` "\\n \\n"` ) can degrade the quality of the actual logic and explanations.


### 2. Prompt adherence degrades over long conversations


Even with explicit formatting instructions, models struggle to maintain adherence over extended interactions.


Language model performance can break down as responses get longer. For example, in the GPT-5 prompting guide[https://cookbook.openai.com/examples/gpt-5/gpt-5_prompting_guide](https://cookbook.openai.com/examples/gpt-5/gpt-5_prompting_guide#markdown-formatting) , they mention this:


> Occasionally, adherence to Markdown instructions specified in the system prompt can degrade over the course of a long conversation. In the event that you experience this, we’ve seen consistent adherence from appending a Markdown instruction every 3-5 user messages.


This wouldn’t be a deterministic solution. There are other solutions to state drift. We could store the state, or disabling formatting - but there’s a simpler solution that works without prompt engineering or tracking state!


## Output normalization: formatting without prompting


When using Claude via the API, you can pass back arrays of messages where each message has a role (user or assistant).


```text
[
{
"role"  :   "user"  ,
"content"  :   "hi"
},
{
"role"  :   "assistant"  ,
"content"  :   "Hello! How can I help you today?"
},
{
"role"  :   "user"  ,
"content"  :   "bye"
},
{
"role"  :   "assistant"  ,
"content"  :   "Goodbye! Feel free to come back anytime if you need help with anything."
}
]
```


Most coding agents only modify the latest user response, but by modifying the *assistant messages* , we can generate code that’s both in-distribution and avoid state drift bugs.


Here’s how it works:


1. Claude generates code normally. Claude produces` old_string` and` new_string` as usual (no prompting changes). The` new_string` can contain trailing whitespaces.
2. Apply output normalization by modifying` new_string` in-place to trim all trailing whitespaces. This preempts the IDE’s auto-formatting and the code gets inserted “pre-formatted”.
3. When we apply the change, it looks like Claude never generated trailing whitespaces at all.
4. Future edits then use the normalized output. When this gets passed back to the API, Claude sees the properly formatted code in its history. This means future edits will reference the corrected output instead of the actual generation.


The implementation is straightforward:


```text
def   normalize_new_string  (new_string:   str  ):
new_string   =   re.sub(  r  "  \n  [   \t  ]  +  \n  "  ,   "  \n\n  "  , new_string)
# do this twice to handle cases like "\n\t\n\t\n"
return   new_string
```


Let’s say Claude wants to add a new function to a file. Here’s what happens:


1. Claude generates trailing whitespace.


```text
def   calculate_sum  (numbers):
"""Calculate the sum of a list of numbers."""
total   =   0
<-   trailing whitespace
for   num   in   numbers:
total   +=   num
<-   trailing whitespace
return   total
```


1. We then delete these trailing spaces using our regex method.


```text
def   calculate_sum  (numbers):
"""Calculate the sum of a list of numbers."""
total   =   0
<-   deleted whitespace
for   num   in   numbers:
total   +=   num
<-   deleted whitespace
return   total
```


1. We update the messages we send back to Claude with the correct format (` \\n\\n` instead of` \\n \\n` ).


```text
{
"role"  :   "assistant"  ,
"content"  :   "I'll add the function:  \n\n  ```python  \n  def calculate_sum(numbers):  \n      \"\"\"  Calculate the sum of a list of numbers.  \"\"\"\n      total = 0  \n\n      for num in numbers:  \n          total += num  \n\n      return total  \n  ```"
}
```


When Claude tries to edit this function later it will reference the version without trailing whitespace, preventing the state drift issue entirely.


Interestingly, this approach keeps Claude mostly on-distribution. While we’re technically modifying Claude’s conversation history, we found that history modifications have much less impact on output quality compared to system prompt changes. In fact even after we normalize the history, Claude continues to generate trailing whitespaces in new code, indicating it’s still following its natural training distribution.


## Results


After rolling out output normalization, our code editing error rate dropped 38% from **13% to 8%** . Users reported that the agent was faster, more cost-efficient, and more accurate.


#### String Replace Error Rate Before / After


Modifying assistant output is an uncommon but powerful technique for improving LLM performance. We’ve incorporated a lot of ideas like these into Sweep to make it the best coding agent for JetBrains. If you like JetBrains,[check out Sweep!](https://plugins.jetbrains.com/plugin/26860-sweep-ai)
