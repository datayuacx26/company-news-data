---
schema_version: "1.0.0"
document_id: "ca84a4ec1f4120c20c8ec430d9984ae32e5d72012bc85e40b4a1cb54088ab5bb"
company_key: "yc-metalware"
company: "Metalware"
source_id: "yc-metalware-news-import-ec8258eac460"
canonical_url: "https://www.metalware.com/blog/automating-firmware-analysis/"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-22T04:09:46.862890+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:faee972cfedf5998624d296c26ce69eaf0c83c967d087d554437557c489aa272"
---

# How We're Automating Firmware Fuzzing

Fuzzing firmware is hard. The binaries are stripped, the architectures exotic, and there’s often no OS abstractions to lean on. AFL++ won’t know what to do with a 64KB blob that talks over memory mapped FDCAN. At least not without a harness.


At Metalware, we’ve been working on a way to change that. You upload a binary and our platform figures out what it is, bootstraps an emulator, and fuzzes it for defects. All that without requiring source code or hardware.


## Initial configuration


When you upload a binary, the first thing we do is determine whether it’s an architecture we support. We do this by scoring each[ISA](https://en.wikipedia.org/wiki/Instruction_set_architecture) based on the number of functions it can successfully disassemble.


For dense ISAs such as Thumb (used in Cortex-M, which is the focus of our work), most byte sequences will disassemble successfully, meaning this heuristic won’t always work. To that end, we employ a second heuristic that looks for the[Interrupt Vector Table](https://developer.arm.com/documentation/107565/0101/Use-case-examples/Generic-Information/What-is-inside-a-program-image-/Vector-table) (IVT) at the head of the binary.


The file format matters too, because it determines how much the binary can tell us about itself. For stripped binaries, we also have to infer the base address. We enumerate candidate addresses by looking for absolute pointers in the code, and then score each candidate based on how many string references and function entry points it explains.


## Configuration agent


We then provide the inferred configuration to our **Configuration Agent** , which adjusts the configuration until the firmware executes successfully.


The agent iterates on the configuration using feedback from the emulator. Once it has found a configuration that works, it kicks off fuzzing.


## Fuzzing runtime


Our emulator intercepts reads to MMIO and DMA and uses the fuzzer to return values. How we identify DMA buffers at runtime deserves its own blog post, but in short, we rely on the observation that DMA buffers are generally configured by writing a RAM pointer to a MMIO location. The RAM pointee can be either the DMA buffer or a descriptor that points to one \[[Source 1](https://arxiv.org/abs/2007.01502) ,[Source 2](https://www.usenix.org/system/files/usenixsecurity25-scharnowski.pdf) \].


MMIO is an easier case as it tends to live in standardized regions, meaning we can simply hook all the reads there. For interrupts, we inject interrupts at regular intervals based on their enable status and their likelihood to generate new coverage.


## Crash Analysis


The artifacts of fuzzing are crashes. Since missing memory regions manifest as crashes, we need a mechanism to discern legitimate crashes (bugs) from issues with our emulator’s configuration.


To this end, we use a two-pronged approach:


- A **backslicer** , which performs backward taint analysis on the crash to determine whether it meets the properties that make it a missing memory region.
- A **root-cause analysis (RCA)** **Agent** , whose job is to look at the binary under a decompiler and the crashing program trace to classify the issue.


The reason for the two-pronged approach is to maximize determinism and speed. The **RCA agent** operates on the order of minutes, whereas the **backslicer** usually takes a few seconds.


### Backslicer


We’ve observed that *nearly* all crashes caused by missing memory regions share a small set of properties that can be identified via backward taint propagation:


- The crash is due to an invalid load or store address rather than a control-flow transfer. In a small number of cases, the firmware will try to JUMP into an unmapped bootloader, which is an open problem we’re working on.
- The address is solely derived from ROM constants. This is because monolithic firmware is not relocatable — all addresses are baked into the binary at compile time.
- The crashing instruction happens outside an interrupt context. This helps distinguish unmapped memories from initialization errors.
- The address was not derived from non-stack memory.
- No[CFI](https://en.wikipedia.org/wiki/Control-flow_integrity) violations have occurred. This helps avoid misclassifying memory corruption bugs, which can redirect execution into random places.


If a crash meets the above properties, we label it a missing memory region and report it to the **Configuration Agent** for remediation.


If it does not, we use the **RCA Agent** as a fallback. The agent classifies the issue, generates a formal description for deduplication, and produces a human-readable explanation.


## Future


In this post, we focused on how to get the firmware running well enough to start interacting with peripherals. As the binary executes, our system adapts to the environmental assumptions the binary reveals along the way.


But that is only one part of the problem. Reaching meaningful states requires correctly modeling missing memory regions (not just mapping them), dealing with complex network stacks, and choosing snapshotting strategies that focus on *interesting* code.


We’ll cover more of these techniques in future posts. Stay tuned.
