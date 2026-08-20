---
schema_version: "1.0.0"
document_id: "8c0e1463ff8749ff0f9d80d6220157717635f1e3e524df76437e2f47dbb2016e"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/how-to-configure-local-logging-for-on-prem-next-gen-waf-agent/"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:8b35b0b510080c75c53a2464ff7d2584152895aa24d3ffff69059079acabc72c"
---

# How to Configure Local Logging for an On-Prem Next-Gen WAF Agent

Do you have a Fastly NGWAF (Next-Gen WAF) Agent deployed on-prem? And do you want to capture and inspect WAF logs locally? Of course you do! Local logging is incredibly helpful for debugging, analyzing traffic, and verifying your security rules before pushing them to production.


Let’s go over how to configure logging for an on-prem agent deployment and walk through a minimal example to see it working in real-time.


## Configuring Local Logging for Fastly's Next-Gen WAF


To tell the NGWAF Agent to write request data to a file, you need to configure the` waf-data-log` setting. *(You can read the full official documentation on this here:*[Fastly Next-Gen WAF Agent Config Docs](https://www.fastly.com/documentation/reference/ngwaf/agent-config/#agentcfg_waf-data-log) *)*


For containerized deployments, we can achieve this easily using environment variables:


-


` SIGSCI_WAF_DATA_LOG` : Specifies the file path inside the container where logs will be written.


-


` SIGSCI_WAF_DATA_LOG_ALL` : When set to` true` , tells the agent to log *all* requests (both benign and malicious). If false or omitted, it usually only logs requests with a signal.


## Minimal Docker Example for Local Logging


To make testing easy, we will use a` Makefile` to start the NGWAF agent with all the necessary configurations in a Docker container.


**Prerequisites:** This setup assumes you have a functional container environment (like Docker Desktop or[Colima](https://github.com/abiosoft/colima) ) and the Docker CLI installed. You will also need your NGWAF access keys exported as environment variables (` NGWAFACCESSKEYID` and` NGWAFACCESSKEYSECRET` ).


Create a file named` Makefile` and paste the following:


Copied!


```text
DOCKERNAME?=localfastlyngwaf


runexeclogs:
@docker run -d --publish 8888:8888 --publish 9999:9999 --name $(DOCKERNAME) --env SIGSCI_ACCESSKEYID=${NGWAFACCESSKEYID} --env SIGSCI_SECRETACCESSKEY=${NGWAFACCESSKEYSECRET} --env SIGSCI_WAF_DATA_LOG="/sigsci/waf_data_log.log" --env SIGSCI_WAF_DATA_LOG_ALL=true --env SIGSCI_REVPROXY_LISTENER="app1:{listener=http://0.0.0.0:8888,upstreams=https://http-me.edgecompute.app:443/,pass-host-header=false}; app2:{listener=http://0.0.0.0:9999, upstreams=https://http.edgecompute.app/,pass-host-header=false}" --add-host=host.docker.internal:host-gateway signalsciences/sigsci-agent
@bash -c '\
trap "echo '\''Cleaning up...'\''; kill 0" SIGINT SIGTERM EXIT; \
nc -lk 5555 & \
sleep 2; \
docker exec -i $(DOCKERNAME) /bin/sh -c "tail -F /sigsci/waf_data_log.log | nc host.docker.internal 5555" & \
wait \
'


clean:
-docker kill $(DOCKERNAME)
-docker rm $(DOCKERNAME)


rerunexeclogs:
make clean
make runexeclogs
```


#### **What is happening in this Makefile?**


1.


**The Docker Run Command:** We spin up the` signalsciences/sigsci-agent` container. We pass in our auth keys, enable the WAF data log via environment variables, and configure a reverse proxy (` SIGSCI_REVPROXY_LISTENER` ) to listen on ports` 8888` and` 9999` .


2.


**The Bash Script (Log Tailing):** Because the logs are written *inside* the container, the bash script sets up a clever pipeline. It starts a local netcat listener (` nc -lk 5555` ) on your host machine. Then, it runs` tail -F` inside the container to grab the logs as they are written and pipes them over the network back to your host machine. This means you see the logs in your terminal instantly!


### **Testing the Deployment**


**Step 1:** Start the NGWAF agent and with log tailing by running the following command in your terminal:


Copied!


```text
make runexeclogs
```


**Step 2:** Open a *new* terminal window. We are going to simulate an attack by sending an HTTP request containing a classic Directory Traversal payload (` ../../../etc/passwd` ).


Copied!


```text
curl "http://0.0.0.0:8888/anything/why_do_pirates_like_urls?because_of_the_args=../../../etc/passwd"
```


### **Analyzing the Log Output**


If everything is configured correctly, look back at the terminal where you ran the make command. You should see a new JSON log line appear. It will look similar to this:


Copied!


```text
{
"Version": "1",
"Timestamp": "2026-04-15T21:24:50Z",
"Method": "GET",
"Path": "/anything/why_do_pirate_like_urls",
"Protocol": "HTTP/1.1",
"RemoteAddr": "172.17.0.1",
"RequestIDStr": "69e002226563ecc3deaaa7b4",
"RequestHeaders": [
{
"Name": "Host",
"Value": "0.0.0.0:8888"
},
{
"Name": "User-Agent",
"Value": "curl/8.19.0"
},
{
"Name": "Accept",
"Value": "*/*"
}
],
"ResponseCode": 200,
"ResponseHeaders": [
{
"Name": "X-Served-By",
"Value": "cache-dfw-kdal2120020-DFW"
},
{
"Name": "Date",
"Value": "Wed, 15 Apr 2026 21:24:49 GMT"
},
{
"Name": "Vary",
"Value": "accept-encoding"
},
{
"Name": "Accept-Ranges",
"Value": "none"
},
{
"Name": "Content-Type",
"Value": "application/json"
},
{
"Name": "Access-Control-Allow-Origin",
"Value": "*"
}
],
"ResponseMillis": 30,
"ResponseSize": 616,
"Scheme": "http",
"ServerHostname": "6827bddcb511",
"ServerName": "0.0.0.0:8888",
"Signals": [
{
"Type": "TRAVERSAL",
"Location": "QUERYSTRING",
"Name": "because_of_the_args",
"Value": "because_of_the_args=../../../etc/passwd",
"Detector": "DIR1V5",
"Redaction": "none"
},
{
"Type": "SUSPECTED-BAD-BOT",
"Location": "REQUEST",
"Name": "",
"Value": "👾; Malicious Probe",
"Detector": "BOTS",
"Redaction": "none"
},
{
"Type": "corp.system-attack",
"Location": "",
"Name": "",
"Value": "",
"Detector": "660ff11f66dd7b84d6e5895f",
"Redaction": "none",
"Attrs": {
"overrideExcludeFromAlerting": "true"
}
}
],
"TLSCipher": "",
"TLSProtocol": "",
"URI": "/anything/why_do_pirate_like_urls",
"UserAgent": "curl/8.19.0",
"WAFResponse": 200
}
```


## Final Thoughts on Fastly NGWAF Local Logging


By enabling local logging for your Fastly NGWAF agent, you gain real-time visibility into requests, detected threats, and rule behavior before changes reach production. Whether you're troubleshooting false positives or validating attack detection, local logs are an invaluable tool for improving security confidence and operational insight.


Ready to take your NGWAF deployment further? Start experimenting with custom rules, signal tuning, and automated log analysis to build a more resilient application security workflow.
