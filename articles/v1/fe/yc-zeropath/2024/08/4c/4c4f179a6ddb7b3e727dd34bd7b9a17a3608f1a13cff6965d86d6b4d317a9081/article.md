---
schema_version: "1.0.0"
document_id: "4c4f179a6ddb7b3e727dd34bd7b9a17a3608f1a13cff6965d86d6b4d317a9081"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/fonoster-voiceserver-lfi-vulnerability"
published_at: "2024-08-24T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:60d2cc05ec5aab8537cb8e9b5d76b2937d648bdece7d250847e2d65e2ea154ab"
---

# Fonoster VoiceServer LFI Vulnerability (CVE-2024-43035)

# Fonoster VoiceServer LFI Vulnerability (CVE-2024-43035)


*By Nathan Hrncirik, Co-Founder and Security Researcher at ZeroPath*


Security researchers at ZeroPath discovered a Local File Inclusion (LFI) vulnerability in[Fonoster VoiceServer](https://github.com/fonoster/fonoster) , a popular open-source platform for building voice applications.


*Proof of concept demo*


## Vulnerability Discovery + Details


The vulnerability stems from insecure handling of user input in the file serving functionality of Fonoster VoiceServer. Specifically, the vulnerability was identified in two endpoints within the VoiceServer:


- ` /sounds/:file`
- ` /tts/:file`


Both of these endpoints are handled by the` serveFiles` function in` utils.ts` . The core issue here is the insecure joining of the config.pathToFiles and req.params.file variables without proper validation:


```text
export     const     serveFiles     =     (  config  :     ServerConfig  )     =>     {
return     (  req  ,   res  )     =>     {
fs  .  readFile  (
join  (  config  .  pathToFiles  ,   req  .  params  .  file  )  ,
function     (  err  ,   data  )     {
if     (  err  )     {
res  .  send  (  "unable to find or open file"  )  ;
}     else     {
res  .  setHeader  (  "content-type"  ,     "audio/x-wav"  )  ;
res  .  send  (  data  )  ;
}
res  .  end  (  )  ;
}
)  ;
}  ;
}  ;
```


## Proof of Concept


To demonstrate the vulnerability, we can send a malicious GET request to the` /sounds` endpoint. For example, this command attempts to read the` /etc/passwd` file:


```text
curl   http://localhost:3000/sounds/  ..  %2f  ..  %2f  ..  %2fetc%2fpasswd
```


This simple cURL request exploits the lack of security checks to traverse the directory structure and access local system files.


## Mitigation


To address these kinds of vulnerabilities, we recommend using the following security measures:


1.


**Input Validation** : Validate the` file` parameter conforms to your expected input, and doesn't contain any path traversal attempts.


2.


**Path Normalization** : Use` path.normalize()` to remove any` ../` sequences from the file path.


3.


**Path Resolution** : Check that the new normalized path remains within the intended directory. config.pathToFiles in this case.


Here's an example of how the` serveFiles` function could be improved:


```text
return     (  req  ,   res  )     =>     {
const   requestedPath   =     join  (  config  .  pathToFiles  ,   req  .  params  .  file  )  ;
const   normalizedPath   =     normalize  (  requestedPath  )  ;


if     (  !  normalizedPath  .  startsWith  (  config  .  pathToFiles  )     ||     isAbsolute  (  req  .  params  .  file  )  )     {
res  .  status  (  403  )  .  send  (  'Access denied'  )  ;
return  ;
}


fs  .  readFile  (  normalizedPath  ,     (  err  ,   data  )     =>     {
if     (  err  )     {
res  .  status  (  404  )  .  send  (  'Unable to find or open file'  )  ;
}     else     {
res  .  setHeader  (  'Content-Type'  ,     'audio/x-wav'  )  ;
res  .  send  (  data  )  ;
}
}  )  ;
}  ;
}  ;
```


## Conclusion + Timeline


- July 21, 2024: Initial email sent to Fonoster developers detailing the vulnerability.
- July 25, 2024: Follow-up email sent due to lack of response.
- July 30, 2024: Second follow-up email sent, still receiving no response.
- August 21, 2024: Submitted vulnerability details and a patch via GitHub.
- August 23, 2024: Public disclosure of the vulnerability.


Currently, there is no patched version for the Fonoster VoiceServer. We recommend making the suggested mitigations yourself or taking down your Fonoster server until an official patch is released.


## Want to chat?


This local file inclusion vulnerability gives a partial demonstration of ZeroPath's scanning capabilities. While many vulnerability scanners might have flagged this issue, ZeroPath's ability to automatically investigate results across large numbers of repositories was a big help with initial identification.


If you're interested in how you can use ZeroPath to improve your code security, please set up a[call](https://cal.com/team/zeropath/chat) with our team!


## Legal Disclaimer


The Proof of Concept (PoC) provided serves solely for educational and research objectives. Its purpose is to showcase a specific vulnerability and aid in comprehending associated security risks.


The creators and contributors of this blog disclaim all liability for the improper use or any damage or harm resulting from the use of this PoC. By utilizing this PoC, you consent to use it in a responsible manner and at your own risk.
