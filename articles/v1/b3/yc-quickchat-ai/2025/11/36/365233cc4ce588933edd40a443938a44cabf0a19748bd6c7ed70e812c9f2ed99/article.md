---
schema_version: "1.0.0"
document_id: "365233cc4ce588933edd40a443938a44cabf0a19748bd6c7ed70e812c9f2ed99"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/one-prompt-arxiv-filter"
published_at: "2025-11-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:419f3ddd155ad28f0cdb42c0e61c11081cd9828ec0ab6b92d7c9ed6c4a68b6aa"
---

# One-prompt arXiv filter: parse email digest, output three must-read papers

## How to stay on top of current AI research?


Reading research papers is very **time-consuming** , but it is **necessary** to:


- have good intuitions about where tech (AI) is going
- learn about new ideas early
- be inspired and think for yourself


In this **tiny side project** I did the following:


- signed up to an arXiv daily digest of all papers published in my fields of interest
- wrote an automation that:


- reads the most recent arXiv digest every morning
- uses a single GPT-5 prompt to select 3 papers most relevant to me
- sends me 3 daily must-reads to me in a separate email that looks like this:


*My arXiv digest example*


## 1. Sign up to the arXiv digest email


If you’d like to receive daily emails from arXiv with new papers in your fields of interest you can sign up to[the arXiv digest](https://info.arxiv.org/help/subscribe.html) . The way to do it is to send an email to arXiv with your preferences.


Follow these steps:


1. Choose the archive you are interested in from[the following options](https://arxiv.org/category_taxonomy) : Computer Science, Economics, Mathematics, etc., and find its **archive ID** .
2. You will need to send an email to` archiveid@arxiv.org` . In my case it was Computer Science whose archive ID is` cs` so I sent the email to` cs@arxiv.org` .
3. Find the[categories](https://arxiv.org/category_taxonomy) of your archive you are interested in. I went for 3: **Artificial Intelligence** , **Computation** and **Language and Multiagent Systems** .
4. Send the email:


**To** :` archiveid@arxiv.org`


**Subject** :` Subscribe Your Name`


**Body** :` add category` (for each category you selected)


In my case the email looked as follows:


```text
To: cs@arxiv.org
Subject: Subscribe Piotr Grudzien
Body:
add Artificial Intelligence
add Computation and Language
add Multiagent Systems
```


*How I subscribed to the arXiv daily digest*


What does the arXiv daily digest email look like? Here’s an excerpt describing two research papers (includes their **title** and **abstract** ):


But a single daily digest contains **~200* research papers newly published that day!* *


*A small part of the 200-papers’ arXiv digest email*


Let’s dive into how we can try and fetch the **very few** papers that are the most relevant so that I would actually have the time to read!


*Just the other day, there was some[activity](https://blog.arxiv.org/2025/10/31/attention-authors-updated-practice-for-review-articles-and-position-papers-in-arxiv-cs-category/) aimed at bringing that number down which stirred some[discussions](https://news.ycombinator.com/item?id=45782136) .


## 2. The full Python script


Here is what the script does step by step (please read carefully and customize to your needs!):


1. Fetch the most recent **arXiv digest** email from your inbox
2. Extract the content of the email and append to your **prompt**
3. Send the prompts to an LLM ( **GPT-5** , high reasoning effort)
4. Parse and format output
5. **Send results back** to the user’s email address


```text
from   __future__   import   annotations


IMAP_HOST   =   "..."    # <--- 1️⃣ your IMAP host ("imap.gmail.com" if you're using Gmail)
IMAP_USER   =   "..."    # <--- 2️⃣ your email address
IMAP_APP_PASSWORD   =   "..."    # <--- 3️⃣ in case of Gmail, follow instructions provided below
IMAP_MAILBOX   =   "INBOX"
IMAP_SEARCH   =   'FROM "no-reply@arxiv.org" SUBJECT "cs daily Subj-class mailing"'    # <--- 4️⃣ customize with your arXiv category ID
SMTP_USER   =   IMAP_USER
SMTP_PASS   =   IMAP_APP_PASSWORD
EMAIL_TO   =   IMAP_USER
OPENAI_KEY   =   "sk-..."   # <--- 5️⃣ your OpenAI key


import   email
import   imaplib
import   re
from   typing   import   List, Optional
import   smtplib
from   email.message   import   EmailMessage
import   datetime   as   dt


from   pydantic   import   BaseModel, Field
from   langchain_core.prompts   import   ChatPromptTemplate
from   langchain_core.messages   import   SystemMessage, HumanMessage
from   langchain_openai   import   ChatOpenAI
from   email.utils   import   parseaddr


DAYS_BACK   =   3


class   Pick  (  BaseModel  ):
title:   str
motivation:   str
why:   str
link:   str


class   Structured  (  BaseModel  ):
picks: List[Pick]   =   Field(  default_factory  =  list  ,   description  =  "Max 3 items"  )


_html_tag_re   =   re.compile(  r  "  <  [  ^  >]  +  >  "  )
_whitespace_re   =   re.compile(  r  "  \s  +  "  )


def   _strip_html  (html:   str  ) ->   str  :
text   =   _html_tag_re.sub(  " "  , html)
return   _whitespace_re.sub(  " "  , text).strip()


def   _get_body_text  (msg: email.message.Message) ->   str  :
if   msg.is_multipart():
# Prefer text/plain; fallback to text/html
for   part   in   msg.walk():
if   part.get_content_type()   ==   "text/plain"  :
payload   =   part.get_payload(  decode  =  True  )   or   b  ""
return   payload.decode(part.get_content_charset()   or   "utf-8"  ,   errors  =  "replace"  )
payload   =   msg.get_payload(  decode  =  True  )   or   b  ""
text   =   payload.decode(msg.get_content_charset()   or   "utf-8"  ,   errors  =  "replace"  )
return   _strip_html(text)   if   msg.get_content_type()   ==   "text/html"   else   text


def   fetch_latest_arxiv_email  () -> Optional[  str  ]:
imap   =   imaplib.IMAP4_SSL(  IMAP_HOST  )
imap.login(  IMAP_USER  ,   IMAP_APP_PASSWORD  )
try  :
imap.select(  IMAP_MAILBOX  )
typ, data   =   imap.search(  None  ,   IMAP_SEARCH  )


ids   =   data[  0  ].split()
for   msg_id   in   reversed  (ids):    # newest first
typ, msg_data   =   imap.fetch(msg_id,   "(RFC822)"  )
raw   =   msg_data[  0  ][  1  ]
msg   =   email.message_from_bytes(raw)
addr   =   parseaddr(msg.get(  "From"  )   or   ""  )[  1  ].lower()
if   addr   !=   "no-reply@arxiv.org"  :
continue
body   =   _get_body_text(msg)
if   body   and   len  (body)   >   200  :
return   body
return   None
finally  :
imap.logout()


def   send_email  (r: Structured) ->   None  :
msg   =   EmailMessage()
today   =   dt.date.today().isoformat()
msg[  "Subject"  ]   =   f  "arXiv picks –   {  today  }  "
msg[  "From"  ]   =   SMTP_USER
msg[  "To"  ]   =   EMAIL_TO


lines   =   []
if   not   r.picks:
lines.append(  "No must-reads today."  )
for   i, p   in   enumerate  (r.picks,   1  ):
url   =   p.link   or   (  f  "https://arxiv.org/abs/  {  p.arxiv_id  }  "   if   p.arxiv_id   else   ""  )
lines.append(  f  """#  {  i  }   {  p.title  }


Motivation
{  p.motivation  }


Why
{  p.why  }


{  url  }  """  )
msg.set_content(  "  \n\n  "  .join(lines))


with   smtplib.SMTP_SSL(  "smtp.gmail.com"  ,   465  )   as   s:
s.login(  SMTP_USER  ,   SMTP_PASS  )
s.send_message(msg)


def   load_system_prompt  () ->   str  :
return   """You will read an entire daily arXiv email (multiple sub-categories).
Return 0–3 must-read papers for me today using the Structured schema. Prefer 1–2 unless 3 are truly exceptional.


### What I care about
- Field-shaping work (big ideas, strong baselines, SOTA, or big names).
- LLMs, computing, ML, AI, cognitive science, theory of mind, theory of consciousness; especially where they connect.
- Philosophy where it touches AI (esp. Theory of Mind), psychology/economics links to AI, blockchain/smart-contract mechanisms if they intersect with AI, creative takes linking AI to the theory of evolution and ideas on how the human brain works.
- Agentic systems relevant to Quickchat AI (we build enterprise AI agents). Track any credible hints of **what might replace today’s RAG/tool-calling/reasoning-LMs** (new memory models, planning frameworks, MCP-driven approaches, etc.). But also note that at Quickchat, we don't train or even host any models to run inference - we use foundational models made available by other labs via API. That's why I am less interested in technical details of model training, acceleration etc. But rather on downstream effects of those on model capabilities today and in the future.
- Creative, non-obvious approaches with promising results; anything that signals a **directional shift** (e.g., replacing text with image inputs; novel uses of MCP like in robotics).


### How to decide (strict)
- Read all titles/abstract snippets. For each candidate, think briefly whether it satisfies the above *and why*.
- Prefer **generality or conceptual clarity** over narrow benchmark wins.
- Avoid incremental “yet another fine-tune” unless it closes a known, important gap.
- Prefer diversity (don’t pick 3 papers that say the same thing).


### Output rules
- `title`: exact paper title from the email.
- `link`: if and only if the email contains a canonical arXiv URL for that paper; otherwise leave null. Do not fabricate URLs.
- `motivation`: 1-2 sentences; a high-level less technical summary of why the paper is important and specifically relevant to me.
- `why`: 1–2 sentences; be specific about *what is new/important* (e.g., “unifies X and Y,” “shows Z is unnecessary,” “early evidence that A may replace B in agents,” etc.). No filler.


Think step by step, then output only the Structured schema."""


def   run_chain  (email_text:   str  ) -> Structured:
system_prompt   =   load_system_prompt()
prompt   =   ChatPromptTemplate.from_messages([
SystemMessage(  content  =  system_prompt),
HumanMessage(  content  =  email_text),
])
llm   =   ChatOpenAI(
model  =  "gpt-5"  ,
temperature  =  1  ,
api_key  =  OPENAI_KEY  ,
model_kwargs  =  {  "reasoning_effort"  :   "high"  }
)
chain   =   prompt   |   llm.with_structured_output(Structured)
return   chain.invoke({})


def   main  () ->   int  :
email_text   =   fetch_latest_arxiv_email()
result   =   run_chain(email_text)
send_email(result)
return   0


if   __name__   ==   "__main__"  :
raise   SystemExit  (main())
```


## 3. How to generate the Gmail App Password


1. Go to[myaccount.google.com](https://myaccount.google.com/)
2. Search for` app passwords` or go straight to[myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
3. Generate your app password


> You must have 2-factor authentication enabled!


## 4. Customize the prompt


Below is the prompt I used in the script. Make sure to customize it to your preferences!


```text
You will read an entire daily arXiv email (multiple sub-categories).


### What I care about
- Field-shaping work (big ideas, strong baselines, SOTA, or big names).


### How to decide (strict)
- Prefer **generality or conceptual clarity** over narrow benchmark wins.
- Prefer diversity (don’t pick 3 papers that say the same thing).


### Output rules
- `title`: exact paper title from the email.


Think step by step, then output only the Structured schema.
```


## 5. (optional) Automate the script to run daily


Every time you run the script it checks your email inbox for the latest arXiv digest and analyses it. I recommend using tools like cron to automate that.


Below are the steps to set up the script as a daily job **on a Mac** :


1. Create a new file in the` LaunchAgents` folder, for example:` ~/Library/LaunchAgents/com.piotr.arxivfetch.plist`
2. Paste and adjust the content of the file:


```text
<?  xml   version  =  "1.0"   encoding  =  "UTF-8"  ?>
<!  DOCTYPE   plist   PUBLIC "-//Apple//DTD PLIST 1.0//EN"
"http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<  plist   version  =  "1.0"  >
<  dict  >
<  key  >Label</  key  >
<  string  >com.piotr.arxivfetch</  string  > <  --   1️⃣ your LaunchAgents file name


<key>ProgramArguments</  key  >
<  array  >
<  string  >/usr/local/bin/python3</  string  > <  --   2️⃣ your Python path
<string>/Users/piotr/Projects/arxiv_daily/fetch.py</  string  > <  --   3️⃣ your path to the script
</array>


<  key  >StartCalendarInterval</  key  >
<  dict  >
<  key  >Hour</  key  ><  integer  >9</  integer  >
<  key  >Minute</  key  ><  integer  >0</  integer  >
</  dict  >


<  key  >StandardOutPath</  key  >
<  string  >/Users/piotr/Projects/arxiv_daily/log.txt</  string  > <  --   4️⃣ your path to the script directory


<key>StandardErrorPath</  key  >
<  string  >/Users/piotr/Projects/arxiv_daily/error.txt</  string  > <  --   5️⃣your path to the script directory
</dict>
</  plist  >
```


1. Load the script:


```text
launchctl   load   ~/Library/LaunchAgents/com.piotr.arxivfetch.plist
```


1. Verify that the script runs correctly by triggering it on demand:


```text
launchctl   start   com.piotr.arxivfetch
```


---


Thank you for making it this far! If you end up launching the script yourself, make sure to share some of your top picks!
