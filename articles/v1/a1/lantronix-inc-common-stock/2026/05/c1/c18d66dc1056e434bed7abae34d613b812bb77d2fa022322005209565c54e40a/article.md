---
schema_version: "1.0.0"
document_id: "c18d66dc1056e434bed7abae34d613b812bb77d2fa022322005209565c54e40a"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/reach-any-device-in-your-network-just-ask/"
published_at: "2026-05-31T02:54:13+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:d4bbfaebaf6375466926e1d4af62cdaf8e402ccf5a5a8da29019277323014f97"
---

# Reach Any Device in Your Network. Just Ask.

May 30, 2026


- [General](https://www.lantronix.com/blog/category/general/)


### Reach Any Device in Your Network. Just Ask.


Percepxion with MCP gives your entire team natural language access to any device in your infrastructure, without learning a new tool, navigating a portal, or knowing anything about how out-of-band access works.


[GitHub Repository](https://github.com/Lantronix/percepxion-mcp-server)[Talk to an Expert](https://outlook.office365.com/book/LantronixDemo@Lantronix.onmicrosoft.com/?ismsaljsauthenabled=true)


---


# 0


New tools to learn for most users


# Any


Device in your network accessible by name


# 1


Admin setup. Whole team benefits.


# 100%


Open source – MIT license; GitHub: Lantronix


**T H E I D E A**


## Your Team Shouldn’t Have to Know How OOB Works to Use It


Out-of-band access has always demanded more from users than it should. Learn the portal. Find the device. Know which port it’s on. Memorize the CLI. That overhead slows down every engineer who isn’t the OOB specialist, and in an incident, every minute counts.


Percepxion with MCP removes that friction. Your team connects the AI assistant they already use, Claude, Copilot, or any MCP-compatible client, and from that point on, getting into a device or pulling information about it is just asking for it. The OOB layer is there. It works. They don’t have to think about it.


> “Set it up once. Now everyone on the team can reach any device, in their own words.”


The Percepxion MCP Server is Lantronix’s open-source implementation of the Model Context Protocol, the emerging industry standard for connecting AI assistants to real-world systems.


---


****H O W I T W O R K S****


## A Secure Bridge Between Your AI and Your Fleet


The MCP server runs as a local process alongside your AI client. When the AI needs device data or wants to trigger an operation, it calls a named tool on the MCP server, which validates the request, translates it into a Percepxion API call, and returns the result.


The MCP server enforces read-only mode by default, write commands require an explicit environment flag. All CLI commands dispatched to devices are logged to stderr with device ID and command string for audit purposes. Your credentials never touch the AI client.


---


**T H E P R O B L E M I T S O L V E S**


## Getting Into a Device Shouldn’t Require an Expert


When a device needs attention, the wrong question is “who knows how to navigate the OOB portal?” Every engineer, every NOC analyst, every operations team member should be able to get in, or get answers, without a training course and without a specialist on the phone.


---


**H O W P E O P L E U S E I T**


## The Device They Need. The Answer They’re Looking For. In Their Own Words.


#### Connect to any device, instantly


#### Get answers without navigating anything


#### Anyone on the team can help


Any engineer can reach any device in your network, by name, by location, by what it does. No portal. No jump host. No asking someone which port it’s connected to. Just ask and get connected.


Someone asks what happened on a device. The answer is in a log, an audit trail, or a job record, but finding it requires knowing the system. Now they just ask, and the AI finds it.


Incidents don’t wait for the one engineer who knows the OOB system. With MCP, every team member, NOC analyst, ops engineer, help desk, can take meaningful action on a device without specialized training.


```text
Example prompt
```


```text
"Get me into the Chicago datacenter core switch. I need to check the routing table."
```


```text
Example prompt
```


```text
"The Austin branch firewall went unreachable during last night's maintenance. What changed and when did it drop off?"
```


```text
Example prompt
```


```text
"The lab server in rack 3 is unresponsive. Can you reboot it? I don't have direct access."
```


---


**F O R T H E W H O L E T E A M**


### They Ask About a Device. They Get an Answer. They Never Touch the OOB System.


A NOC analyst gets an alert. A router at a branch site is unreachable. In the old model, they’d escalate to whoever knows the out-of-band system, find the device in the portal, connect to its console, figure out what’s wrong. That takes time nobody has during an incident.


With Percepxion MCP, they open their AI assistant and ask. The AI connects through the OOB layer, pulls the audit log, checks the interface status, and tells them what changed, in plain English, in seconds. The OOB infrastructure did its job. They never had to know it was there.


- Works for any team member, not just the OOB specialist
- No portal login, no device lookup, no syntax to remember
- Full audit trail of every action taken through the AI
- The OOB admin controls what each user can do


---


**F O R T H E O O B A D M I N**


### You Control What Happens. Your Team Gets the Access They Need.


The abstraction doesn’t mean loss of control. The OOB admin configures the MCP server once, setting access policy, defining which operations require write permissions, blocking destructive commands, and connecting credential storage to the team’s existing secret manager.


From that point, every team member benefits. And every action they take through the AI is logged, auditable, and bounded by the policy you set.


- Read-only by default, diagnostic use needs no configuration
- Built-in block list: reload, factory-reset, write erase always blocked
- Configurable permit list to restrict exactly which commands pass
- Every device command logged for audit with device ID and timestamp


---


**C R E D E N T I A L P R O V I D E R S**


### Your Team’s Secret Store. Not a Plaintext File.


For individual use, credentials in a local


` .env`


file are fine. For shared deployments or CI pipelines, the MCP server supports HashiCorp Vault and AWS Secrets Manager as credential backends, no credentials on disk, no rotation coordination.


Switch providers at runtime with


` reconfigure_credentials`


without restarting the server. The active provider is tracked separately from the startup config.


- Three providers:` env` (default),` vault` ,` aws`
- Vault: KV v2 path configurable via env var
- AWS: reads from Secrets Manager by secret name
- Runtime switching without process restart


---


**T O O L R E F E R E N C E**


## 33 Tools. One Connection.


Every tool maps to a named Percepxion API endpoint. Your AI sees the tool description and calls it by name, you see the result.


Device Inventory & Lifecycle


Configuration & Firmware


Logs, Jobs & Security


-


```text
get_device_list
```


Search and paginate the full inventory


-


```text
get_device_details
```


Full properties by device ID or serial


-


```text
import_and_assign_devices
```


Onboard devices to a tenant


-


```text
unassign_devices
```


Remove devices from a tenant


-


```text
reboot_device
```


Reboot via async job group


-


```text
list_device_ports
```


Enumerate serial/device ports


-


```text
get_device_config
```


Read telemetry config before modifying


-


```text
update_device_config
```


Save and optionally apply config changes


-


```text
clone_device_config
```


Copy config from source to target device


-


```text
firmware_compliance_report
```


Fleet-wide compliance vs. target version


-


```text
list_firmware_content
```


Available firmware in Percepxion storage


-


```text
update_firmware_by_smart_group
```


Upload and deploy to a Smart Group


-


```text
send_direct_cli_command
```


Run CLI on a device (policy-controlled)


-


```text
get_device_syslogs
```


Query uploaded syslog files


-


```text
investigate_audit_logs
```


Platform audit records by user or action


-


```text
get_security_telemetry
```


Security-relevant telemetry per device


-


```text
get_job_group
```


Full output of an async operation by ID


-


```text
reconfigure_credentials
```


Switch credential provider at runtime


**G E T S T A R T E D**


## Running in Under Five Minutes


Works with Claude Desktop, Claude Code, GitHub Copilot, or any MCP-compatible client on macOS, Linux, or Windows with WSL.


---


**F A Q**


## Common Questions


Any client that supports the Model Context Protocol spec. This includes Claude Desktop, Claude Code (the CLI), GitHub Copilot in VS Code, Cursor, Cline, and others. The MCP spec is an open standard, as adoption grows, the server works with new clients without modification.


Yes. The MCP server connects to the Percepxion REST API, which requires an active Percepxion account. For Lantronix customers with SLC 9000 console servers, Percepxion is the management platform, this MCP server is the AI interface on top of it. Contact your Lantronix representative for Percepxion access.


The server is designed to be safe to start with and locked down by default. CLI commands are read-only unless you explicitly enable write mode. Destructive operations, reload, factory-reset, write erase, are blocked by a deny list even when write mode is on. Every CLI command is logged. Percepxion’s own role-based access controls apply on top: the AI can only do what your Percepxion account is allowed to do. For production environments, use a dedicated service account with minimum necessary permissions.


Credentials are read by the MCP server process, not by the AI client. The server exchanges username/password for a session token at startup and holds that token in memory. The AI client never receives or sees credentials. Tokens are never written to disk. On a 401 response, the session clears automatically.


Any device managed through Percepxion. This includes SLC 9000 series console servers (the primary platform), SLC 8000 series, and any other Lantronix hardware visible in your Percepxion tenant. The SLC 9000 running firmware 9.7.0+ has the fullest API coverage including AOOB automation capabilities.


Yes. The repository is open source under the MIT license at


[github.com/Lantronix/percepxion-mcp-server](https://github.com/Lantronix/percepxion-mcp-server) . The


` docs/adding-new-tools.md`


file walks through the conventions for adding new tools, each tool is a Python function decorated with


` @mcp.tool()` . Pull requests are welcome.
