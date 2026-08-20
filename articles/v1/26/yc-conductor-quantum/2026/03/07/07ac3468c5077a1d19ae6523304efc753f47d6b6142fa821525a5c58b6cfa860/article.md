---
schema_version: "1.0.0"
document_id: "07ac3468c5077a1d19ae6523304efc753f47d6b6142fa821525a5c58b6cfa860"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/introducing-coda-mcp-quantum-tools-for-ai-agents"
published_at: "2026-03-16T20:31:11+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T21:58:45.685125+00:00"
content_hash: "sha256:03f60ca6fa91a8296e6d2cf4f062faae511c26f3a115d41731fae0111ce30404"
---

# Introducing Coda MCP: quantum tools for AI agents

# Introducing Coda MCP: quantum tools for AI agents


### Your AI agents can now use quantum computers


Mar 16, 2026


Coda is Conductor’s natural language interface for quantum computers. Describe what you want to do, and Coda turns that intent into an executable program, simulates it when you want fast iteration, runs it on real systems when you are ready, then returns results you can inspect.


Today we are launching


**Coda MCP** , our Model Context Protocol (MCP) server that lets any MCP-compatible agent call quantum compute as a tool.


### At a glance


-


One MCP server for quantum execution, simulation, and transpilation


-


Multi-provider QPU access across IBM, AQT, IQM, IonQ, and Rigetti


-


Cross-framework transpilation, including IBM Qiskit and NVIDIA CUDA-Q


-


Available now for anyone with a Coda account


## One interface, multiple backends


Quantum workflows tend to fragment across providers and SDKs. Coda MCP is built to keep that from happening: one interface, consistent tools, and the flexibility to switch targets without rewriting everything.


Right now, that means live access to:


-


Providers:


**IBM, AQT, IQM, IonQ, and Rigetti**


-


Hardware:


**6+ QPUs** totalling


**1000+ qubits**


With more hardware providers coming soon. If you’re not sure which one to choose, we’ve included a


[QPU (quantum processing unit) leaderboard](https://www.conductorquantum.com/coda#qpu-leaderboard) to help.


## Write once, run anywhere


Quantum work rarely stays inside one SDK. Coda MCP includes a transpile tool so you can prototype in one framework and execute in another while keeping results comparable.


Frameworks include:


-


Qiskit


-


NVIDIA CUDA-Q


-


Cirq


-


PennyLane


-


Braket


-


PyQuil


-


OpenQASM


## Simulation you can iterate on


When you are iterating on a circuit or debugging an experiment, simulation is the fastest feedback loop.


Coda also supports simulations up to 34 qubits supported by the


[NVIDIA cuQuantum libraries and NVIDIA CUDA-Q](https://developer.nvidia.com/cuquantum-sdk) platform for hybrid quantum-classical computing.


In practice, this is how you and your coding agent debug quickly, benchmark, and run parameter sweeps before you spend queue time on hardware.


## Small utilities that make real workflows smoother


Coda MCP includes workflow utilities you will end up needing anyway:


-


Export to OpenQASM 3 (


` to_openqasm3` )


-


Estimate resources like qubit count, depth, and gate counts (


` estimate_resources` )


-


Split larger circuits for distributed execution (


` split_circuit` )


It also includes paper search and retrieval tools (


` search_papers` ,


` get_paper` ) so an agent can ground an experiment in the literature, then immediately test an idea in simulation or on hardware.


## Why MCP matters here


Agents can already write quantum code. What has been missing is a reliable, repeatable path from that code to execution across real backends.


Coda MCP provides that path. It lets an agent plan an experiment, run it end to end on simulators or QPUs, collect results, and iterate, all through a simple interface.


Under the hood, the loop is simple: generate or import a circuit, transpile to the target framework, simulate for fast iteration, execute on a QPU when you are ready, then pull results back into the conversation for analysis and next steps. All of these steps are completely handled by Coda.


## Get started


Docs:


[https://pypi.org/project/coda-mcp/](https://pypi.org/project/coda-mcp/)


### If you want to try Coda first


If you just want to get a feel for Coda’s natural language interface, you can use the web chat at


[coda.conductorquantum.com](https://coda.conductorquantum.com/) .


Once you are ready to give an agent access to the same capabilities, connect via Coda MCP below.


### If you want to connect an MCP compatible agent


Coda MCP works with MCP clients that support stdio transport, including Claude Desktop, Claude Code (CLI), VS Code, Cursor, Zed, and more.


First,


[generate an API token in Coda](https://coda.conductorquantum.com/) (


` Settings → API` ) and set it as


` CODA_API_TOKEN` . If you need to override the API endpoint, you can set


` CODA_API_URL` as well.


Install the server using one of the supported options:


```text
pip install coda-mcp


coda-mcp  # runs the server
```


Then add the server to your MCP client.


Claude Desktop uses these config paths:


-


macOS:


` ~/Library/Application Support/Claude/claude_desktop_config.json`


-


Windows:


` %AppData%\\Claude\\claude_desktop_config.json`


Example Claude Desktop config:


```text
{


“mcpServers”: {


“coda”: {


“command”: “/path/to/uvx”,


“args”: [”coda-mcp”],


“env”: {


“CODA_API_TOKEN”: “your-token-here”


}


}


}


}
```


Claude Code (CLI):


```text
claude mcp add coda -e CODA_API_TOKEN=your-token-here -- uvx coda-mcp
```


OpenAI Codex (CLI):


```text
codex mcp add coda --env CODA_API_TOKEN=your-token-here -- uvx coda-mcp
```


Tip: in the Codex terminal UI, run


` /mcp` to confirm the server is connected.


Cursor: add the same


` mcpServers.coda` entry to


` ~/.cursor/mcp.json` or


` .cursor/mcp.json` in your project.


Zed: add a


` context_servers.coda` entry to


` ~/.config/zed/settings.json` .


## Once connected, try:


-


“Coda, create a 2 qubit Bell circuit in Qiskit, simulate it, then export to OpenQASM 3.”


-


“Coda, transpile this Qiskit circuit to CUDA-Q and run a GPU backed simulation.”


-


“Coda, list available QPUs and submit this circuit to IonQ or Rigetti.”


-


“Coda, compile the same circuit for two targets and compare depth, gate counts, and output distributions.”


## Access


Coda MCP is available now for anyone with a Coda account.


-


To try


[Coda](https://www.conductorquantum.com/coda) in the browser, use the chat interface at


[coda.conductorquantum.com](https://coda.conductorquantum.com/) .


-


To connect an agent, generate a token, add the MCP server, and run your first job.


If you are building agent workflows and want a specific backend or capability prioritized, reach out. We are actively expanding coverage and improving the developer experience.


– Brandon, Joel and Ray, Conductor Quantum


Thanks for reading! Subscribe for free to receive new posts and support our work.
