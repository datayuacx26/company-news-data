---
schema_version: "1.0.0"
document_id: "e5c6441b395a88f07fd2d4eb6a87b14862afd7b9bb9e87ef174b559219f6d9e4"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/profile-guided-optimization"
published_at: "2026-01-30T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:22:54.577865+00:00"
content_hash: "sha256:99dd96a4b4600dc703a38872b452cb30054ad6018168cdcb854b65fb37078387"
---

# Profile-Guided GPU Kernel Optimization

Profiling in our CLI broke a theory-only plateau: 11.65× on the Kimi Delta Attention kernel.


Frontier models are surprisingly capable kernel optimizers out of the box, but most agentic coding environments like Claude Code don’t provide anything other than bash for interacting with accelerator hardware.


By adding an` ncu` subcommand to our Wafer CLI, our agent was able to break the plateau of theory-based optimizations for the Kimi Delta Attention kernel, bringing up the initial 9x speedup over the torch.compile baseline to 11.65x speedup within the same run.


Profiling revealed to the agent that the kernel was only launching 64 blocks on a 145-SM GPU – just 0.04 waves per SM and 6.25% achieved occupancy. This inspired the agent to rewrite the kernel to vectorize register usage and add manual unrolling, resulting in the final 11.65x speedup. Without profiler data, the agent had been trying optimizations based on theory alone.


> In the earlier kernel, each thread stored its state as 128 independent floats and iterated over them in a long loop


```text
float   S_row  [  128  ];
#pragma   unroll   16
for   (  int   col   =   0  ; col   <   128  ;   ++  col) {
S_k_row   +=   S_row  [col]   *   k_shared  [col];
}
```


> In the optimized version, the same 128 values are still kept entirely in registers but packed into 32` float4` s


```text
float4   S_row  [  32  ];
#pragma   unroll
for   (  int   i   =   0  ; i   <   32  ; i  ++  ) {
int   base_col   =   i   *   4  ;
S_k_row   +=   S_row  [i].x   *   k_shared  [base_col];
S_k_row   +=   S_row  [i].y   *   k_shared  [base_col   +   1  ];
S_k_row   +=   S_row  [i].z   *   k_shared  [base_col   +   2  ];
S_k_row   +=   S_row  [i].w   *   k_shared  [base_col   +   3  ];
}
```


While the agent did the above optimization, it didn’t attempt to modify the launch configuration to solve the low occupancy issue that it found in the data. We’re working on ways to improve the CLI to more effectively allow the agent to traverse and act on the raw information from the profiler.


For more information, you can review the trace yourself at[https://app.wafer.ai/traces](https://app.wafer.ai/traces) . Contact us if you have any ideas or feedback.


## What’s Next


Agents write better code when they can read logs, write test scripts, and use tools to understand what their code actually does. We’re applying this principle to kernel programming too: give agents the tools to see what the hardware is doing, to let them iterate better and faster.
