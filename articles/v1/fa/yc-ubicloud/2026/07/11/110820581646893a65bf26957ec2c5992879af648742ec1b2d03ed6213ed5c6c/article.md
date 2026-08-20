---
schema_version: "1.0.0"
document_id: "110820581646893a65bf26957ec2c5992879af648742ec1b2d03ed6213ed5c6c"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/rebuilding-deep-research-with-open-models"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:d7565a37dc875dd36748ac1f899c417358268635a905a0ddd41297bf98b7a774"
---

# Dewey.py: Rebuilding Deep Research with Open Models

## Dewey.py: Rebuilding Deep Research with Open Models


March 24, 2025 · 5 min read


Junhao Li


Senior Software Engineer


Deep Research is an AI-powered solution that automates multi-step research processes, mimicking the work of a seasoned research analyst.[OpenAI](https://openai.com/index/introducing-deep-research/) ,[Google](https://blog.google/products/gemini/google-gemini-deep-research/) , and[xAI](https://x.com/xai/status/1892400134178164775) already released their proprietary deep research products.


In this blog, we’ll show you how to build Dewey, your own deep research agent, using only open source. This way, you can see how it works under the hood, customize the process based on your own needs, and deploy it on your own infrastructure. We will describe the key technical details here, and you can find the full implementation on[GitHub](https://github.com/ubicloud/deep-research) .


To give you a sense of the style and quality of Dewey, here are a few sample reports it has generated:


- **Technology:**[Differences between Microsoft’s and Google’s recent quantum breakthroughs](https://drive.google.com/file/d/1Pjvcv-I9xhdmnN-qf9PzFaWPxh3m8C9b/view?usp=sharing) .
- **Healthcare:**[Advances in gene therapy for Alzheimer’s disease](https://drive.google.com/file/d/1BS9Z2WchFwqXF40Rb7hubZrz-okeaqrD/view?usp=sharing) .
- **Travel:**[A two-day itinerary for exploring New York City](https://drive.google.com/file/d/1YuJeHm5VYmzD8tXWxULgZO8m4reYILwR/view?usp=share_link) .


Each research takes about 5 to 10 minutes to run and costs less than 10 cents. Sounds interesting? Let’s get started!


### The Research Process


At a high level, Dewey receives a topic from the user, gathers information from the web, develops an initial answer, iteratively refines it with further exploration, and then creates an outline and generates the report. The diagram below illustrates the process.


Let’s dive into each step in detail.


### Gathering Information


After receiving a topic as input from the user, the agent first gathers relevant information by performing a web search, retrieving the content from the URLs, and summarizing the extracted content.


- **Web Search:** This requires a third-party web search API.[DuckDuckGo](https://pypi.org/project/duckduckgo-search/) offers a free and convenient API that doesn’t require an account, making it easy to get started. However, you may run into rate limit restrictions. For a more reliable solution, you may consider an API key-based service like[Tavily Search](https://blog.tavily.com/getting-started-with-the-tavily-search-api/) , which requires registration but includes a free monthly quota, typically sufficient for development. The following code returns a list of search results, each containing a title and a URL.


```text
%pip install duckduckgo_search tavily-python
from duckduckgo_search import DDGS
from tavily import TavilyClient
def search(query, search_engine):
if   search_engine   ==   "duckduckgo"  :
return   DDGS()  .text(query, max_results=  5  )
elif search_engine   ==   "tavily"  :
return   TavilyClient(TAVILY_API_KEY)  .search(query=query)  ["  results  "]
```


1. **Content Extraction:** Some deep research implementations rely solely on snippets from search results. However, we find that fetching the full content provides more detailed information for generating comprehensive research reports. We also find that PDFs are invaluable for many academic topics, as many latest insights only appear in research papers. Therefore, we perform both full content fetching and PDF extraction, as illustrated below


```text
%pip install beautifulsoup4 PyPDF2 requests
import io
from bs4 import BeautifulSoup
import PyPDF2
import requests
def fetch  _url(  url  )  :
response = requests.get(url, timeout=  30  )
content_type = response.headers.get(  "Content-Type"  ,   ""  ).lower  ()
if     "application/pdf"     in   content_type   or   url.lower  ()  .endswith(  ".pdf"  ):
pdf_file = io.  BytesIO(  response  .  content  )
PyPDF2.  PdfReader(  pdf_file  )
return   ""  .join(page.extract  _text()     or     ""     for   page   in   reader.pages)
soup =   BeautifulSoup(  response  .  content  , '  html  .  parser  ')
return soup.get  _text()
```


1. **Summarization:** To generate a concise context for further reasoning and report creation, we summarize the extracted content using an LLM. The[Mistral 24B](https://huggingface.co/mistralai/Mistral-Small-24B-Instruct-2501) model performs well for this task. You can access the Mistral model via[Ubicloud](https://www.ubicloud.com/) ’s managed OpenAI-compatible inference endpoints. To do so, sign up for a Ubicloud account and obtain a UBICLOUD_API_KEY


from the AI Inference section. The following code shows the core logic of the summarization process.


```text
%pip install openai
import openai
SUMMARIZATION_MODEL =   "mistral-small-3"
SUMMARIZATION_CONTENT_CUTOFF =   50000
def summarize(title, content):
base_url = f  "https://{SUMMARIZATION_MODEL}.ai.ubicloud.com/v1/"
client = openai.  OpenAI(  api_key  =UBICLOUD_API_KEY,   base_url  =  base_url  )
completion = client.chat.completions.create(  [
create_system_message  ("""S  ummarize     the     content  .
O  nly     include     information     that     is     relevant     to     the     title  .
B  e     succinct  . S  ummarize     directly  ,   don  '  t     repeat     the     title  ."""),
create_user_message  ({
"  title  ":   title  ,
"  content  ":   content  [:SUMMARIZATION  _CONTENT_CUTOFF  ]
})
])
return completion.choices  [  0  ]  .message.content
```


Here, SUMMARIZATION_CONTENT_CUTOFF


ensures that the content does not exceed the context length limit of the Mistral model. The cutoff of 50,000 characters is a safe choice, considering Mistral’s 32K token context length and the average of around 4 characters per token for English. Additionally, create_system_message and create_user_message are convenient methods for formatting messages to an OpenAI-compatible structure, which Ubicloud inference endpoints use.


```text
def create  _system_message(  content  )  :
return {  "role"  :   "system"  ,   "content"  : content}


def create  _user_message(  content  )  :
return {  "role"  :   "user"  ,   "content"  : str(content)}
```


### Thinking and Drafting Response


Once the relevant information is gathered, we analyze the content, apply reasoning, and synthesize a draft response to the topic. By using a reasoning model like DeepSeek R1, the agent can inherently gain the ability to evaluate information, cross-reference sources, and analyze problems using topic-specific frameworks. We find that the[32B distilled R1](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B) model performs well for our needs, and Ubicloud’s hosted AI inference service is optimized for this use case. By leveraging techniques like speculative decoding and prefix caching, we enhanced inference speed without compromising quality. The following code shows the core logic:


```text
REASONING_MODEL =   "ds-r1-qwen-32b"
def thinking(state):
base_url = f  "https://{REASONING_MODEL}.ai.ubicloud.com/v1/"
completion = client.chat.completions.create(  [
create_system_message  ("""T  ake     a     deep     breath  ,   go     through     the     search     results  ,   think     through     the     topic     step     by     step  ,   and     provide     a     well  -  reasoned     answer  .
U  se     information     from     the     search     results     as     needed  . B  e     objective  ,   reasonable  ,   and     comprehensive  ."""),
create_user_message  (  state  )
]  )
content = completion.choices  [  0  ]  .message.content
return {**state,   "thinking"  : content}
```


Here, the state


is a Python dictionary that stores all obtained and generated information during the research process. At this stage, it should contain the topic and search_results. As the research progresses, we will iteratively expand the contents of state. The message.content


field contains only the final output of the DeepSeek reasoning model. If you are interested in the internal reasoning process, you can access it via message.reasoning_content


. We moved the reasoning to a separate field to make our content format consistent with OpenAI. Another thing you may notice is the use of magic phrases like “take a deep breath” or “think step by step” in the system message. They are[proven prompt engineering techniques](https://arxiv.org/pdf/2309.03409) , discovered by machine learning researchers, to enhance the quality of the responses.


### Deep Dive


To deliver a more comprehensive response, we need to go beyond surface-level information by iteratively identifying subtopics for further exploration and refining the draft responses multiple times.


- **Subtopic Identification:** We can leverage the reasoning capabilities of the R1 model again to identify key areas for deeper exploration. The following code illustrates the core logic.


```text
JSON_MODEL =   "ds-r1-qwen-32b"
def identify  _subtopics(  state  )  :
base_url = f  "https://{JSON_MODEL}.ai.ubicloud.com/v1/"
completion = client.chat.completions.create(  [
create_system_message  ("""I  dentify     3     key     areas     for     deeper     exploration     on     the     given     topic     and     thinking  .
R  eturn     a   JSON   array     of     strings  . E  ach     string     should     be     a     well  -  structured     search     engine     query  .
"""),
create_user_message  ({"  topic  ":   state  ["  topic  "]  ,   "thinking"  : state  ["  thinking  "]  })
])
content = completion.choices  [  0  ]  .message.content
return extract  _json(  content  )
```


The extract_json method extracts the JSON object from the response, which may include both text and JSON. It looks for code blocks \`\`\`...\`\`\`


and parses that as JSON. If you’re familiar with OpenAI models, you may recall that they have a JSON mode that outputs JSON directly. However, most open-source models use Markdown’s triple backticks \`\`\`


as \`the delimiter to return JSON.


```text
def extract  _json(  content  )  :
match   = re.search(r  "```(?:\w+)?\s*(.*?)\s*```"  , content, re.DOTALL)
if     match  :
return json.loads(  match  .group(  1  ).strip  ()  )
```


1. **Subtopic Exploration:** After identifying the subtopics, we gather additional information on each subtopic and rethink the original topic. The core logic is exactly the same as in the gathering information and reasoning steps above.


The above process can be repeated multiple times as needed. We find that 3 is usually a good number of iterations for most questions. However, for complex or high-value problems, using more iterations allows for more extensive research and deeper analysis, leading to a more comprehensive and insightful response.


### Creating an Outline


With all the gathered information and the refined reasoning, we can now construct a well-structured outline for the final report. This step is essential for ensuring clarity, coherence, and logical organization throughout the report.


The following code shows the core logic for generating the outline:


```text
def thinking(state):
base_url = f  "https://{REASONING_MODEL}.ai.ubicloud.com/v1/"
completion = client.chat.completions.create(  [
create_system_message  ("""G  enerate     an     outline     of     a     professional     report     on     the     given     topic     and     thinking  .
T  hink     step     by     step  . U  se     search     results     as     needed  ."""),
create_user_message  (  state  )
]  )
content = completion.choices  [  0  ]  .message.content
return {**state,   "outline"  : content}
```


### Writing the Report


Finally, we generate the research report and export it.


- **Final Composition:** We use the Mistral model to generate the report based on all the intermediate results above. We find the writing style of this model aligns best with our expectations. To ensure accurate referencing, we include the index of each search result in the input and manually compile the reference list, which also helps reduce inference costs. Here’s the main logic.


```text
WRITING_MODEL =   "mistral-small-3"
def write  _report(  state  )  :
base_url = f  "https://{WRITING_MODEL}.ai.ubicloud.com/v1/"
completion = client.chat.completions.create(  [
create_system_message  ("""G  enerate     an     extremely     detailed     professional     report     based     on     the     provided     topic  ,   thinking  ,   and     outline  .
U  se     search     results     as     needed  .
E  nsure     it     is     well  -  organized     and     each     section     is     well  -  developed  .
U  se     subsections     and     lists     as     needed  .
I  nclude     the     topic     as     the     title  . I  nclude     an     executive     summary     at     the     beginning  .
R  efer     to     search     results     by     their     index     as     needed  . D  o     not     include     the     list     of     references     at     the     end  .
U  se     heading     level     1     for     the     title  . D  o     not     include     figures  .
"""),
create_user_message  (  state  )
]  )
content = completion.choices  [  0  ]  .message.content
references =   [
f  "  1.   {  result  ["  title  "]  }. (n.d.). Retrieved from {result  ['  url  ']  }  "
for result in state["  search_results  "]]
content += "  \n\n## References\n\n  " + "  \n  ".join(references)"
return {**state,   "report"  : content}
```


- **Report Export:** We find it useful to export the report as a PDF for easy reading and sharing, and to save the entire state as a JSON file, enabling refinement and resumption of the research. Here’s the implementation for your reference.


```text
%pip install markdown-pdf
import json
from markdown_pdf import MarkdownPdf, Section
def export(state, filename):
pdf =   MarkdownPdf(  toc_level  =2)
pdf.add  _section(Section(  state  [  "report"  ])  )
pdf.meta  ["  title  "]   = state  ["  title  "]
pdf.save(f  "{filename}.pdf"  )
with     open  (f  "{filename}.pdf"  ,   "w"  , encoding=  "utf-8"  )   as   f:
json.dump(state, f, ensure_ascii=False, indent=  2  )
```


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)


### Lessons Learned


Here are some lessons we learned from building and using Dewey:


- Deep research, at least with this simple implementation, excels at gathering and organizing comprehensive information into a well-structured report. However, it offers limited benefits for complex reasoning, such as solving math problems, unless relevant prior analyses are already available online.
- If you know how to break down a problem effectively, manually breaking it down into multiple queries rather than relying solely on the deep research agent often yields more comprehensive results. Expert insights remain invaluable in such cases.
- Building something that looks good is easy, but creating something truly useful is much harder. We started with a simple implementation—just search and report writing—without thinking or iterative deep dives, and at first glance, the reports still looked great. However, upon closer inspection, we found them riddled with hallucinations, lacking comprehensiveness, and ultimately not much more useful than a standard chatbot. Transforming it into a truly useful research agent capable of automating our real research workflows requires many iterations of prompt engineering, output evaluation, and self-introspection.


### Conclusion


From information gathering to reasoning and report generation, we now have Dewey, a deep research agent that can help us automate and accelerate our routine research workflow.


Again, you can find the full implementation on[GitHub](https://github.com/ubicloud/deep-research) . Feel free to experiment with it—explore new topics, customize the steps as needed, and let us know if you run into any issues.


Happy researching! Let’s create something remarkable with the power of AI.
