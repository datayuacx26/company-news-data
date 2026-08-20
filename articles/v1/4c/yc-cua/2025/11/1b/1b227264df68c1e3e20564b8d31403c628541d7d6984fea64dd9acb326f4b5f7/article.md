---
schema_version: "1.0.0"
document_id: "1b227264df68c1e3e20564b8d31403c628541d7d6984fea64dd9acb326f4b5f7"
company_key: "yc-cua"
company: "Cua"
source_id: "yc-cua-news-import-c869e0198521"
canonical_url: "https://cua.ai/blog/introducing-cua-cli"
published_at: "2025-11-18T00:00:00+00:00"
first_seen_at: "2026-07-21T15:30:37.690309+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:aabcf7fe70d64e37184c3ecacb21ccfab8cb46adcbe600ccd0eb73dd137add4a"
---

# Introducing the Cua CLI: Manage Cloud Sandboxes from Your Terminal

If you've been using our Cloud Sandboxes, you've probably been managing them through the web dashboard - clicking through forms to create instances, copying credentials, manually starting and stopping sandboxes. It works, but it's not exactly built for power users like yourself.


Today we're launching the **Cua CLI** : a command-line interface that brings the full power of our Cloud Sandbox platform to your terminal. Create, manage, and connect to Linux, Windows, or macOS sandboxes in seconds—all from a single command.


## What You Can Do


The Cua CLI handles everything you need to work with Cloud Sandboxes:


**Authentication**


- Browser-based OAuth login with automatic credential storage
- Direct API key support for CI/CD pipelines
- Export credentials to` .env` files for SDK integration


**Sandbox Management**


- Create sandboxes with your choice of OS, size, and region
- List all your sandboxes with status and connection details
- Start, stop, restart, and delete sandboxes
- Open remote desktop (VNC) connections directly in your browser


**Two Command Styles** The CLI supports both flat and grouped command structures—use whichever fits your workflow:


```text
bash   # Grouped style (explicit & clear)  cua sb ls  cua sb create --os linux --size small --region north-america  cua sb vnc my-sandbox     # Flat style (quick & concise)  cua ls  cua create --os linux --size small --region north-america  cua vnc my-sandbox
```


Both styles work identically. The CLI shows grouped commands in help by default, but all flat commands remain available for backwards compatibility.


## Installation


One command installs everything (includes Bun runtime + Cua CLI):


```text
bash   # macOS/Linux  curl -LsSf https://cua.ai/cli/install.sh | sh     # Windows  powershell -ExecutionPolicy ByPass -c "irm https://cua.ai/cli/install.ps1 | iex"
```


Or install via npm if you prefer:


```text
bash   npm install -g @trycua/cli
```


## Getting Started


Authenticate with your Cua account:


```text
bash   # Interactive browser login (recommended)  cua auth login     # Or provide your API key directly  cua auth login --api-key sk-your-api-key-here
```


Create a sandbox:


```text
bash   cua sb create --os linux --size small --region north-america  # Sandbox created and ready: my-sandbox-abc123  # Password: secure-password-here  # Host: my-sandbox-abc123.sandbox.cua.ai
```


List your sandboxes:


```text
bash   cua sb list  # NAME                 STATUS    HOST  # my-sandbox-abc123    running   my-sandbox-abc123.sandbox.cua.ai  # test-windows-456     stopped   test-windows-456.sandbox.cua.ai
```


Open a remote desktop:


```text
bash   cua sb vnc my-sandbox-abc123  # Opens your browser to the VNC interface with password pre-filled
```


## SDK Integration


Export your API key to a` .env` file for seamless SDK integration:


```text
bash   cd my-project  cua auth env  # Wrote /path/to/my-project/.env
```


Then use it with our Python or TypeScript SDKs:


```text
python   from computer import Computer     computer = Computer(      os_type="linux",      provider_type="cloud",      name="my-sandbox-abc123",      api_key="your-api-key"  # Or load from .env  )     await computer.run()
```


## Sandbox Sizes & Regions


Create sandboxes in the size and region that fits your needs:


**Sizes:**


- ` small` - 2 cores, 8 GB RAM, 128 GB SSD
- ` medium` - 4 cores, 16 GB RAM, 128 GB SSD
- ` large` - 8 cores, 32 GB RAM, 256 GB SSD


**Regions:**


- ` north-america`
- ` europe`
- ` asia-pacific`
- ` south-america`


**OS Options:**


- ` linux` - Ubuntu with XFCE desktop
- ` windows` - Windows 11 with Edge and Python
- ` macos` - macOS (preview access)


## Example Workflows


**Quick Testing Environment**


```text
bash   # Spin up a sandbox, test something, tear it down  cua sb create --os linux --size small --region north-america  # ... do your testing ...  cua sb delete my-sandbox-abc123
```


**Persistent Development Sandbox**


```text
bash   # Create a sandbox for long-term use  cua sb create --os linux --size medium --region north-america     # Stop it when not in use (data persists)  cua sb stop my-sandbox-abc123     # Start it again when needed  cua sb start my-sandbox-abc123
```


**CI/CD Integration**


```text
bash   # Provision sandboxes in your pipeline  export CUA_API_KEY="sk-your-api-key"  cua auth login --api-key "$CUA_API_KEY"  cua sb create --os linux --size large --region north-america     # Run your tests with the Cua Computer SDK  python run_tests.py     # Clean up  cua sb delete my-test-sandbox
```


## Command Aliases


We've added aliases for common commands to speed up your workflow:


```text
bash   # List aliases  cua list    # or: cua ls, cua ps, cua sb list     # VNC aliases  cua vnc     # or: cua open, cua sb vnc
```


## FAQs


**Can I use this in scripts and CI/CD?**


Yes. All commands support non-interactive mode with` --api-key` flags, and the CLI exits with proper status codes for scripting. The flat command style (` cua list` ,` cua create` ) is particularly useful for quick scripts.


**Where are my credentials stored?**


API keys are stored in` ~/.cua/cli.sqlite` using a local SQLite database. They never leave your machine. Use` cua auth logout` to clear stored credentials.


**What happens to passwords in the output?**


Passwords are hidden by default in` cua list` for security. Use` cua list --show-passwords` to display them when needed.


**Can I manage sandboxes created through the web dashboard?**


Yes. The CLI and dashboard share the same API. Any sandbox you create in the dashboard will show up in` cua list` , and vice versa.


**How do I update the CLI?**


If you installed via script:


```text
bash   curl -LsSf https://cua.ai/cli/install.sh | sh
```


If you installed via npm:


```text
bash   npm install -g @trycua/cli@latest
```


## What's Next


We're actively iterating based on feedback. Planned features include:


- SSH key management for secure sandbox access
- Template-based sandbox creation
- Batch operations (start/stop multiple sandboxes)
- Custom sandbox configurations
- Snapshot management


If there's a feature you need, let us know in[Discord](https://discord.gg/cua-ai) .


## Need Help?


- **Documentation** :[https://cua.ai/docs/cua/reference/cloud-cli/commands](https://cua.ai/docs/cua/reference/cloud-cli/commands)
- **Installation Guide** :[https://cua.ai/docs/cua/reference/cloud-cli](https://cua.ai/docs/cua/reference/cloud-cli)
- **Discord Community** :[https://discord.gg/cua-ai](https://discord.gg/cua-ai)


Get started at[cua.ai](https://cua.ai/) or check out the[quickstart guide](https://cua.ai/docs/cua/guide/get-started/using-agent-sdk) .
