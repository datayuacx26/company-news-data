---
schema_version: "1.0.0"
document_id: "c6be05abddb85df9179237e84e977871aa7255eafef4b94fc6e1db29c8953244"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/08/09/introducing-the-nutanix-mcp-server-part-1-getting-started-introduction/"
published_at: "2026-08-10T05:32:23+00:00"
first_seen_at: "2026-08-10T05:48:45.337624+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:89787cf414ef87ff0bec8b193d2fab037f17038ac9e5cdbfbc76bbc4f57445e8"
---

# Introducing the Nutanix MCP Server – Part 1 – Getting Started/Introduction

## Introduction


In August 2026, Nutanix released the Tech Preview of our MCP (Model Context Protocol) Server. The Nutanix MCP Server bridges the gap between agents such as Claude Code, Cursor and Nutanix Cloud Platform (NCP) environments.


Whilst the[docs/quickstart.md](https://github.com/nutanix/ntnx-api-mcp-server/blob/main/docs/quickstart.md) installation guide covers high-level steps, this article is intended as a slightly more detailed example of that installation, followed by additional usage examples.


## Technical Preview Disclaimer


This is an open source project. Please note the following before using it:


- **Non-Production Only** : This project is in a tech preview state. It is **not** designed, tested, or supported for production workloads.
- **Expect Changes** : As a Tech Preview, breaking changes may occur. We encourage you to experiment, test, and share your feedback in non-production environments!


## Assumptions


This article makes the following assumptions.


1. Readers following the steps in this article have an existing connection to a Prism Central deployment, along with appropriate credentials for that deployment. For the demos outlined in this article, Nutanix IAM-managed read-only access to virtual machines is sufficient.
2. Readers following the steps in this specific article have an existing installation of Claude Desktop and Claude Code. These examples are for demonstration purposes only and do not constitute a Nutanix recommendation or endorsement of Claude Code as the most suitable local coding agent.


## Requirements


The goal of this article and demo is to achieve the following outcomes.


1. Show some installation screenshots that supplement the instructions shown in[quickstart.md](https://github.com/nutanix/ntnx-api-mcp-server/blob/main/docs/quickstart.md) .
2. Demonstrate integrating the Nutanix MCP Server into Claude Code
3. Show environment-specific configuration for the local MCP server to connect to Prism Central
4. Demonstrate high-level MCP Server command usage
5. Show examples of additional commands beyond those shown in the[quickstart.md](https://github.com/nutanix/ntnx-api-mcp-server/blob/main/docs/quickstart.md) document


## Project Setup


This section can be skipped if you already have a dedicated project directory. This demo uses the path` ~/projects/ntnx-api-mcp-server-main` , although your project directory may be different.


### Directory Setup


1. Change to the` ~/projects/ntnx-api-mcp-server-main` directory
2. Create a Python >=3.11 virtual environment (venv) in the` .venv` subdirectory
3. Activate the new virtual environment


Create and activate the Python virtual environment


### Installation Prerequisites


The Nutanix MCP server is provided with a` pyproject.toml` file. This file contains information about all the required project dependencies and allows Python to install verified package versions with no user input.


1. Install the prerequisites as documented (` pip install -e .` ). This will load and install package dependencies from` pyproject.toml` . This article assumes use of` pip` .


1. **Note** : The use of` pyproject.toml` allows alternate Python package managers, if required. Example:` uv` instead of` pip` .


Install Python dependencies from` pyproject.toml`


### Verify commands


Before continuing, we will now carry out a quick test to ensure the` nutanix-mcp` command is available.


1. Run the following:
` which nutanix-mcp`
2. If you used the same directory structure as that shown in[quickstart.md](https://github.com/nutanix/ntnx-api-mcp-server/blob/main/docs/quickstart.md) , the output will appear similar to the following example:


Verifying location of the` nutanix-mcp` command


### Credentials


The[quickstart.md](https://github.com/nutanix/ntnx-api-mcp-server/blob/main/docs/quickstart.md) documentation indicates the project root requires a new file named` .env` . This can be created using any preferred method; methods for Mac OS X, Linux and Windows are shown below.


**Note:** In production environments, we recommended using Prism Central “User Account of type Service Account” with dedicated API keys. Dedicated API keys provide programmatic access for scripts and services and allows authentication and authorization of API activity through Nutanix IAM Authorization Policies (ACPs).


For simplicity, this demo shows the use of HTTP Basic Authentication.


The following commands will create a file named` .env` in the current directory and set the contents as shown. Replace the environment-specific settings with those matching your requirements.


#### Mac OS X/Linux (POSIX shells)


```text
cat << EOF > .env
# Required
PC_HOST=pc.demo.lab
PC_PORT=9440


# Choose one auth method (not both):
# PC_API_KEY=your-api-key
# - OR -


PC_USERNAME=admin
PC_PASSWORD=nutanix/4u


# TLS (set to false if your PC has a trusted certificate)
PC_INSECURE=true
EOF
```


#### Windows PowerShell


```text
@"
# Required
PC_HOST=pc.demo.lab
PC_PORT=9440


# Choose one auth method (not both):
# PC_API_KEY=your-api-key
# - OR -


PC_USERNAME=admin
PC_PASSWORD=nutanix/4u


# TLS (set to false if your PC has a trusted certificate)
PC_INSECURE=true
"@ | Set-Content .env
```


**Note** : The Nutanix MCP server GitHub repository ships with` READ_ONLY_MODE` set to` true` . This prevents any expected modifications to connected environments. If your deployment is intended for modification of the connected environments (VM creation, for example),` READ_ONLY_MODE` must be set to` false` .


### MCP Initialization


With all commands verified, we can initialize our new MCP project.


1. Initialise the project using` nutanix-mcp init` . If a successful connection is made to Prism Central, this will download the OpenAPI specs from Prism Central. The example below includes messages that may appear in the event an artifact download fails.


```text
{
"mode": "init",
"artifact_mode": "pc_compatible",
"discovered": 23,
"processed": 23,
"success": 18,
"skipped": 3,
"failed": 2,
"skipped_reasons": {
"missing_version_or_namespace": 3
},
"failed_reasons": {
"http_error": 2
},
"duration_ms": 103421
}
```


## Coding Agent Connection


This demo shows the use of Claude Desktop on Mac OS X. The included[quickstart.md](https://github.com/nutanix/ntnx-api-mcp-server/blob/main/docs/quickstart.md) documentation includes equivalent steps for Cursor, if required.


### Method 1: Direct File Edit for Claude Desktop


1. Locate the` nutanix-mcp` command. This can be done as follows:


```text
which nutanix-mcp
```


**Note** : The use of an alternate Python package manager may prevent the` which` command from returning a full path to` nutanix-mcp` . For example, the` uv` package manager, by default, will create a virtual environment in the` .venv` directory. In that situation, the` nutanix-mcp` binary path will be` .venv/bin/nutanix-mcp` . Adjust your commands accordingly, if not using` pip` for Python package management.


1.


1. If using the project path as shown so far in this article, this resolves to:
` /Users/<username>/projects/ntnx-api-mcp-server-main/.venv/bin/nutanix-mcp`
2. Edit the Claude Desktop configuration to use the new MCP Server installation. By default, this requires editing the following file:
` ~/Library/Application Support/Claude/claude_desktop_config.json`
3. The contents of this file must be modified to include the following JSON block, making the appropriate environment-specific changes where necessary:


```text
"mcpServers": {
"nutanix-v4-mcp": {
"command": "/Users/<username>/projects/ntnx-api-mcp-server-main/.venv/bin/nutanix-mcp",
"args": [
"serve-stdio"
],
"env": {
"PC_HOST": "pc.demo.lab",
"PC_PORT": "9440",
"PC_USERNAME": "admin",
"PC_PASSWORD": "nutanix/4u",
"PC_INSECURE": "true",
"ARTIFACTS_DIR": "/Users/<username>/projects/nutanix/ntnx-api-mcp-server-main/artifacts"
}
}
},
```


### Method 2: Claude Code config using` mcp` command


For this demo, the Claude Code configuration can be configured *using* Claude Code. Within Claude Code, the` mcp` command can accept similar info to what was used in the previous step with Claude Desktop.


1. Start and open Claude Code
2. Collate the following information:


1. Location of the` nutanix-mcp` command. This assumes you have a Python 3 virtual environment created and activated as per previous steps:


```text
which nutanix-mcp
```


In our demo environment, this returns:
` /Users/<username>/projects/ntnx-api-mcp-server-main/.venv/bin/nutanix-mcp`


1. PC Central FQDN or IP address
2. Prism Central credentials i.e. username + password. **Note** : API key credentials are outside the scope of this demo.
3. Artifacts directory. Unless configured otherwisex, this demo’s artifacts directory will be as follows:
` /Users/<username>/nutanix/ntnx-api-mcp-server-main/artifacts`
4. Build the` mcp` command as follows:


```text
mcp add nutanix-v4-mcp "<nutanix-mcp path>" serve-stdio --env PC_HOST="<pc_fqdn>" PC_PORT="9440" PC_USERNAME="<pc_username>" PC_PASSWORD="<pc_password>" PC_INSECURE="true" ARTIFACTS_DIR="<artifacts_dir>" --scope local
```


**Note** :` nutanix-v4-mcp` is the generic name for the Nutanix v4 MCP server within this demo. If required, this can be altered to suit your requirements.


## Using the Nutanix MCP Server


This completes the Claude Desktop and/or Claude Code configuration.


### Documented Examples


A number of basic usage examples are included in[docs/quickstart.md](https://github.com/nutanix/ntnx-api-mcp-server/blob/main/docs/quickstart.md) . For completeness, screenshots of those examples are shown here.


#### List Virtual Machines


Command:


` List the first 5 virtual machines in my Nutanix cluster.`


Example response from “List the first 5 virtual machines in my Nutanix cluster.”


#### List Recovery Points


Command:


` Show me the 5 most recent recovery points available in Nutanix.`


Example response from “Show me the 5 most recent recovery points available in Nutanix.”


### Additional Example


Now that we’ve seen two basic examples of using Claude Code with the Nutanix MCP Server, let’s expand by combining the Nutanix v4 MCP Server with Nutanix v4 API and Odata filters:


#### Filter Virtual Machines by IP Address


In the first example, we use the existing capabilities of the Nutanix MCP Server to collect a virtual machine “list”, However, the Nutanix v4 APIs do not currently support filtering virtual machines machines and, in this example, we need to find out if any virtual machines are using the IP address` 10.x.x.103` . Let’s see how Claude Claude handles this, now that it knows how to work with Nutanix virtual machines.


Example of using the Nutanix MCP Server and Claude to find which VM is using a specific IP address


Notable points:


1. The` nutanix-v4-mcp` MCP server (“the server”) **does** know how to list virtual machines
2. The server **does** understand the Nutanix v4 APIs support Odata filters
3. The server **does** understand the Nutanix v4 APIs do not currently support complex filters for use with VM IP addresses
4. Because the server understand these points, it can build a list of VMs and look at the VM details to obtain the required information
5. With Claude Code’s conversation memory still in place, i.e. **without** using` /clear` , we can then ask questions as follows:
` If the Nutanix v4 APIs do not currently support complex filters using virtual machines IP addresses, how did you figure out that a single VM uses that IP address?`


In this example, that natural-language query returned a long response containing a couple of key points:


First, client-side filtering is the only way to get each VM’s IP address:


Use client-side filtering to get the VM with the requested IP address


And then, later in the response:


Extrapolating an answer from the Nutanix MCP Server response combined with what this Claude projects about the Nutanix v4 APIs


## Conclusion


The Nutanix MCP Server can be used today via the[dedicated Nutanix GitHub repository](https://github.com/nutanix/ntnx-api-mcp-server) .


This concludes the introductory guide to setting up and using the Nutanix MCP Server. By successfully configuring the server and exploring the initial examples, you have established a foundation for more efficient, agent-driven infrastructure management. While this document focused on code assistants, the MCP server can be used with AI Agents that support MCP servers. As you move forward, you can leverage these integrations to perform more complex operations, utilize OData filters for specific data retrieval, and incorporate Nutanix resources directly into your development and operational workflows.


As you continue to explore, we encourage you to:


- Experiment with advanced prompts: Your prompts can be multi-part that could help you solve multi-step problems, frontier models can help you do advanced tasks like planning for major changes in your data center. Try automating audit reports for your deployment.
- Give feedback to Nutanix: During your exploration with the Nutanix MCP Server, please report any issues or suggestions to <GitHub>
- Explore the broader ecosystem: The Nutanix MCP Server is one of the initial steps of a larger push towards intelligence that will help enable automation. Keep an eye on further updates regarding Nutanix Agentic AI solution.
