---
schema_version: "1.0.0"
document_id: "0b53230159ed7dc9fb0aff4a224e3973f182649bebd81b8bf7d3c920d038fadb"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/claude-science-bionemo-agent-toolkit/"
published_at: "2026-06-30T17:00:38+00:00"
first_seen_at: "2026-07-19T06:54:21.753056+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:623269d7550715b310e315b90326b1a0f24aa47f9ae9ce3cd46de01127c86316"
---

# NVIDIA BioNeMo Agent Toolkit Brings Accelerated AI to Life Sciences Researchers in Claude Science

Life sciences has entered an era of computational scale, and for more than a decade, NVIDIA has built the full GPU-accelerated computing stack — spanning hardware, frameworks, libraries, models, microservices and domain-specific tools — to help researchers run more sophisticated workflows and iterate faster.


This week, Anthropic announced[Claude Science](https://www.anthropic.com/news/claude-science-ai-workbench) , an AI workbench for science research that lets scientists converse with agents in natural language to run their work end to end.


Claude Science


integrates with


[NVIDIA BioNeMo Agent Toolkit](https://nvidianews.nvidia.com/news/nvidia-launches-bionemo-agent-toolkit-giving-ai-agents-the-tools-to-accelerate-scientific-discovery) as a resource that


scientists can access


within their workflow. The toolkit packages NVIDIA-accelerated capabilities as callable skills, enabling Claude Science to select the appropriate tool, prepare valid inputs and execute the workflow — all while connecting to NVIDIA compute resources deployed anywhere. This brings NVIDIA’s accelerated models, libraries and NVIDIA NIM microservices directly into the same environment where the rest of the research happens.


The world’s largest pharmaceutical companies use NVIDIA technologies to advance AI-enabled research across drug discovery, genomics, medical imaging, molecular design and protein engineering. Today, 18 of the top 20 pharmaceutical companies use


[NVIDIA BioNeMo](https://github.com/NVIDIA-BioNeMo) , underscoring the breadth of its role across the ecosystem.


## **Advancing the Agentic Era of Scientific Discovery**


Claude Science lets scientists use natural language to move their research from intent into action, without manually configuring models, endpoints, or software environments. NVIDIA BioNeMo Agent Toolkit extends that with access to accelerated workflows and models like Evo 2, Boltz-2 and OpenFold3, so the analyses that benefit from acceleration run faster.


A scientist begins by describing a research task, such as analyzing a genomic sequence, predicting a protein structure or designing a potential binder, in natural language. Claude Science interprets the request and orchestrates the work through preconfigured domain-specialized agents that know established workflows across genomics, proteomics, single-cell analysis, cheminformatics and clinical research.


BioNeMo Agent Toolkit gives these agents the context needed to connect each step with an appropriate NVIDIA scientific capability. Each skill includes information about its purpose and required inputs, helping agents prepare and execute the workflow and return outputs for review.


The result is an iterative loop between scientific reasoning and accelerated computational work. Scientists can inspect outputs, refine their questions and determine the next step while staying focused on the science.


One powerful example is generating better inhibitors of common cancer targets. In this workflow, a scientist starts with a known cancer-causing antigen mutation and asks Claude to design numerous potential inhibitors. Claude Science integrated with BioNeMo Agent Toolkit and NVIDIA NIM microservices accelerates high-throughput inhibitor prediction, optimization and validation.


## **A Scientific Foundation Built for Agents**


AI agents reason, plan and use tools to complete tasks. In life sciences, those tools are often specialized computational workflows.


An autonomous AI scientist agent doesn’t reason in isolation. It may need to fingerprint a library of compounds, cluster promising hits, generate conformers for top candidates, analyze genomic context and compare perturbation responses before recommending the next experiment.


Each step relies on a scientific tool, and the agent can only work as fast as those tools run.


NVIDIA BioNeMo Agent Toolkit gives scientific agents the accelerated tools they need to operate at the speed of science. It includes:


- [NVIDIA Parabricks](https://docs.nvidia.com/clara/parabricks/latest/overview.html) accelerates genomic analysis from hours to minutes, so an agent can integrate genomic context into a decision in near real time.


- [RAPIDS-singlecell](https://rapids-singlecell.readthedocs.io/en/latest/) , developed by scverse, compresses a 1.3-million-cell preprocessing and clustering workflow from 52 minutes to 25 seconds, so single cell analysis becomes part of the reasoning loop rather than an offline batch of jobs.


- [nvMolKit](https://github.com/NVIDIA-BioNeMo/nvMolKit?ncid=so-link-338451) accelerates cheminformatics operations like similarity search and conformer generation by up to 3,000x, so an agent iterating across a massive chemical space gets results at the speed of thought.


- NVIDIA BioNeMo


[open models](https://github.com/NVIDIA-BioNeMo#models) deliver core biomolecular capabilities accelerated by NVIDIA libraries, so an agent has a purpose-built scientific model for each step of a workflow.


- BioNeMo


[NIM microservices](https://github.com/NVIDIA-BioNeMo#optimized-inference-and-deployment) package those models as enterprise-ready inference endpoints — containerized microservices with the full accelerated software stack pre-integrated and tuned for high-performance inference — so an agent can call a single stable application programming interface for production deployment.


NVIDIA BioNeMo Agent Toolkit is open and harness-agnostic, allowing the same scientific skills to work across agent frameworks and research platforms. The toolkit and its skills are available now through[NVIDIA developer resources](https://developer.nvidia.com/industries/healthcare?size=n_12_n&sort-field=featured&sort-direction=desc) and


[GitHub](https://github.com/NVIDIA-BioNeMo/bionemo-agent-toolkit) .


Scientists can access BioNeMo-powered workflows through Anthropic’s Claude Science, which is entering public beta today. As part of the public beta, Anthropic is inviting researchers to provide feedback on additional domain specialists and integrations they need.
