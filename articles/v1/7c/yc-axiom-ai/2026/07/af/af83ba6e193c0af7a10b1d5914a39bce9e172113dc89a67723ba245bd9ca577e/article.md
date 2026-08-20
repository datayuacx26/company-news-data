---
schema_version: "1.0.0"
document_id: "af83ba6e193c0af7a10b1d5914a39bce9e172113dc89a67723ba245bd9ca577e"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/browser-automation-with-claude/"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:2ccd9d9808dea5ce203bbce16770fb1112093baa9b99ddd03d775abe3a79aaab"
---

# Browser automation with Claude

Claude comes with many different approaches for browser automation: Claude in Chrome, Claude Cowork, Claude Computer use, and writing scripts with Claude Code.


Each approach suits particular use cases; however, most are suited to doing a single task *once* . They are less suited to doing the same thing a thousand times, as part of a complex business process. They don't come with the batteries included to handle the edge cases of browser automation debugging, and running browsers on infrastructure at scale.


For that, we recommend using Claude Code and axiom.ai together. Use axiom's no-code or code system with Claude, leveraging features for debugging, monitoring, scheduling, and our other browser automation infrastructure to scale your automation beyond a single use.


## Driving a browser with Claude


There are several options here:


### Claude in Chrome


The easiest way to put Claude in a browser, and the one most people may reach for first, as it's from Anthropic. Claude in Chrome is a browsing agent that lives in your browser as an extension. You ask it to do something on a page and it navigates, clicks, and fills forms for you, acting on the page directly. No selectors, no script, nothing to set up beyond installing it. It is a beta product, so treat it as capable but still finding its edges. It is built for the task in front of you now, not running the same task ten thousand times.


### Claude Cowork / Claude Desktop


One step up from the extension. The desktop agent app can use Claude in Chrome as one of its tools, so instead of steering the browser yourself, you hand a multi-step job to the desktop agent and it drives the browser as part of a larger task.


This fits when the browsing is one piece of something beyond the browser: gather from a few sites, then write the result to a file, and use it with Excel locally. Same caveat as the extension. It is an agent deciding as it goes, so it suits open-ended work better than a fixed routine you repeat.


### Claude Computer use (and other agentic tools)


Claude looks at a screenshot of the screen and moves the mouse and types, the way a person would. It's not a script acting on the structure of the page. You give it a goal and an environment to act in, and it works out what to do dynamically.


This is the most general option and the most expensive per run (with API calls for every operation). Every step is a model call, so it costs more and runs slower than a script.


Agents are most suitable for dynamic tasks you cannot predict.


If you're running the same predictable process repeatedly, like web scraping or data mapping, you're paying a premium to use the LLM over regular code designed specifically for that task.


### Using Claude Code to write code (or no-code!)


Another way, and the way we recommend, is to use Claude Code to do what it's best at: write and maintain code (it also works the same way with our no-code tool).


We believe this is the best of both worlds: the intelligence of Claude Code when you need it, and the predictability, speed, and scalability of coded automations.


With[axiom's Claude skill](https://axiom.ai/build-in-claude) , we enable Claude to build and maintain browser automations.


### Claude Skill for browser automations


A skill packages the know-how so Claude builds browser automations for you, then hands you something that can run on your local machine or our infrastructure.


The[axiom skill](https://axiom.ai/build-in-claude) takes a plain description and builds either a no-code automation you run in the[Chrome extension](https://chromewebstore.google.com/detail/axiom-browser-automation/cpgamigjcbffkaiciiepndmonbfdimbb) , or a coded one you drop into your stack.


Either way, the finished bot runs on its own, with no model call per step.


## Example: Data entry on a Google Form


Filling a Google Form from a sheet is a repeat job. Same form, different rows, over and over. So the agentic options carry an overhead: I do not want to pay for vision on every field of every row, and I do not want a hundred submissions riding on the model not drifting. I want something deterministic that I build once and loop.


## Automating a Google Form with the Claude skill


### Install the skill


The skill runs in[Claude Code](https://code.claude.com/docs/en/overview) . Open a session and run these one at a time. Pasting all three at once concatenates them and the install fails.


```text
/plugin marketplace add git@github.com:axiom-browser-automation/claude-skill.git
/plugin install axiom@axiom-skills
/reload-plugins


```


No axiom account? Tell Claude you need one and the skill signs you up and mints an API key as you build. The full steps are on the[build in Claude](https://axiom.ai/build-in-claude#install) page.


### Describe the form


Ask Claude to build an axiom and describe the Google Form the way you would explain it to a person. List the questions in order and say where each answer comes from in your sheet.


```text
Build me an axiom that fills this Google Form from my Google Sheet.
For each row: enter the name, select the gender, enter the email,
enter the address, enter the zip code, then click submit and start
a new response.


```


The skill reads that and picks a format. Ask for a schedule or to save it to your account and it builds a no-code axiom. Ask for a Node script, or name a language, and it writes code. If your ask is open, it asks you which you want.


### Build it no-code


You can do the same thing with axiom's no-code[Chrome extension](https://chromewebstore.google.com/detail/axiom-browser-automation/cpgamigjcbffkaiciiepndmonbfdimbb) , and be guided by Claude on debugging.


If you'd rather work with your IDE, the skill works well with VS Code, and can save it straight to your Axiom account. From there, you can run, schedule, and leverage our infrastructure.


### Or build it in code


Ask for code instead, and the skill writes a Node script built on` @axiom_ai/api` .


Axiom's browser automation API and step functions solve a lot of the boilerplate necessary with Playwright and Puppeteer, adding autowaits and handling other browser edge cases.


The same loop, as calls you can read and keep:


```text
import   { AxiomApi }   from   '@axiom_ai/api'


const   FORM   =   'https://docs.google.com/forms/d/e/FORM_ID/viewform'


// Each row would come from your Google Sheet
const   rows   =   [
{ name:   'Ada Lovelace'  , gender:   'Female'  , email:   'ada@example.com'   },
{ name:   'Alan Turing'  ,  gender:   'Male'  ,   email:   'alan@example.com'   },
]


async   function   main  () {
const   axiom   =   new   AxiomApi  (process.env.  AXIOM_API_KEY  )
await   axiom.  browserOpen  ()
try   {
for   (  const   row   of   rows) {
await   axiom.  goto  (  FORM  )


// Match each field by its aria-label so a reorder does not break it
await   axiom.  enterText  (  'input[aria-label="Name"]'  , row.name)
await   axiom.  click  (  `div[role="radio"][aria-label="${  row  .  gender  }"]`  )
await   axiom.  enterText  (  'input[aria-label="Email"]'  , row.email)


await   axiom.  click  (  'div[role="button"][aria-label="Submit"]'  )
await   axiom.  click  (  'a[aria-label="Submit another response"]'  )
}
}   finally   {
await   axiom.  browserClose  ()
}
}


await   main  ()


```


Run it with` npm install @axiom_ai/api` , set` AXIOM_API_KEY` in your environment, then` node` the file. The browser runs in axiom's cloud, so there is nothing to install locally.


### Target each question by its label


Google Forms is the one part worth getting right yourself, no-code or code. It renders each question as a block with no stable id, so a selector tied to position breaks the moment someone reorders a question. Match on the field's aria-label instead. In the[selector tool](https://axiom.ai/docs/no-code-tool/the-builder/selector-tool) , switch to a custom CSS selector:


```text
input  [  aria-label  =  "Email"  ]


```


That finds the field by its accessible label rather than where it sits on the page, so the step keeps working when the form changes. It is the same idea in code:` axiom.enterText('input\[aria-label="Email"\]', row.email)` .


### Run one, then loop


Run a single row first and check the form's response Sheet. One good row proves the mapping before you turn it loose. Then loop the rest: on the desktop app while you watch, or in the cloud once it is solid. The skill asks before it saves to your account or spends cloud runtime, so nothing runs without your say-so.


## What I'd watch out for


Google Forms looks simple, and mostly it is. A few things trip a bot.


- **File uploads need a Google sign-in.** A file upload question only works when the run is signed into a Google account. Store the cookies for that account, or leave upload questions out.
- **Required questions and validation stop a submit.** If a required field is empty or an answer fails validation, the run stalls on the first row. Map every required question before you loop.
- **Test one response first.** It is far easier than deleting a hundred bad ones later.


This is for forms you are meant to fill: submitting your own entries from a spreadsheet, registering a list of people, seeding a form with test responses before launch. Stuffing a poll, rigging a ballot, or sending spam is not, and it is against Google's terms.


## Where to go next


To go no-code, install the[Chrome extension](https://chromewebstore.google.com/detail/axiom-browser-automation/cpgamigjcbffkaiciiepndmonbfdimbb) ; to build with code, add the[Claude skill](https://axiom.ai/build-in-claude) . The short version: Claude is the builder, the axiom bot is what runs. Use Claude to get from a description to a working automation, then let the automation handle the thousand rows.
