---
schema_version: "1.0.0"
document_id: "111466b8b9f0b8161bfa9448dcaeb06b54a125f16528d72726b93f95bb82e9dd"
company_key: "yc-metalware"
company: "Metalware"
source_id: "yc-metalware-news-import-ec8258eac460"
canonical_url: "https://www.metalware.com/blog/svd-prediction/"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-22T04:09:46.862890+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:8e66f8276e54fb948e6370f422150d2496dd57b29d253ec3efd8cd5be46a3013"
---

# Automating Peripheral Map Discovery on Monolithic Firmware

We don’t always know the MCU of the firmware we receive. Knowing the MCU tells you what peripherals it has and where they live in memory. The peripherals are the entire attack surface on monolithic firmware. Can we recover the peripheral map from the firmware alone?


Malaschonok’s[2026 Ortellius paper](https://www.ndss-symposium.org/wp-content/uploads/sdiotsec26-7.pdf) used MMIO fingerprinting and[SVD](https://arm-software.github.io/CMSIS_5/SVD/html/svd_Format_pg.html) databases to deterministically find the best SVD for the firmware. SVDs, or System View Description files, describe the peripherals, registers, and bitfields of the MCU. The method returned a ranking of SVDs where the top rank identified the MCU family 57% of the time and the best available memory map 44% of the time across 42 binaries \[1\]. A[2024 Binary Ninja blog post](https://binary.ninja/2024/08/12/sidekick-in-action-analyzing-firmware.html) mentioned using their LLM with MMIO fingerprints for peripheral identification. “While not always 100% accurate in device identification, Sidekick’s guesses were consistently close and often accompanied by convincing rationales” \[2\].


My hypothesis was simple: feeding both Malaschonok’s SVD ranking results and the firmware to an LLM would achieve SVD accuracy good enough for automation. In other words: slap an LLM on it.


Results were promising with my prototype. I wanted to make sure I was feeding the LLM the best possible information (garbage in, garbage out) and investigated whether the ranking algorithm and/or SVD database could be improved.


Malaschonok’s method for SVD scoring was to add the Jaccard similarity of MMIO reads to the Jaccard similarity of MMIO writes. I found three issues with this scoring method:


- the Jaccard index favored SVDs with fewer registers.
- some SVDs inaccurately labelled their access types (some STM32 clones ranked higher than ST STM32 SVDs).
- some SVDs were more holistic than others. For example, not all Cortex-M SVDs contained entries for the private peripheral bus. Those that did ranked higher.


Scoring SVDs based on the number of matching MMIO accesses in the peripheral region fixed the above issues.


A lot of SVDs also don’t follow the official spec.[Nordic](https://github.com/nordicsemi/nrfx/tree/master/bsp/stable) , for example, strictly uses lowercase instead of camelCase for register access types. I used[svd-rs](https://github.com/rust-embedded/svd/tree/master/svd-rs) with validation disabled for parsing. I also pulled SVDs from more sources, including TI’s IDE using[tixml2vsd](https://github.com/dhoove/tixml2svd) to convert the TIXML to SVDs.


For LLM augmentation, I gave medium-effort Claude Opus 4.6 a one-paragraph system prompt (shown below), the top deterministic SVD rankings, and access to bash. A lot of the Ortellius firmware included the MCU in the name of the file, so I renamed the firmware under test to firmware.bin to prevent cheating.


> You are selecting the matching SVD file for a Cortex-M firmware binary. You have SVD rankings showing which peripheral maps best match the binary’s MMIO accesses. Your job is to pick the right SVD. Use tools to disambiguate when the top rank has multiple candidates or the top ranks are close.


Adding tool hints to the prompt improved results:


> bash can inspect the firmware binary at $FIRMWARE (e.g. strings $FIRMWARE | grep -i , xxd $FIRMWARE | head -8).


Results with the Ortellius dataset (42 binaries):


Method Stripped Binaries Exact Family Wrong


Ortellius Paper N/A 36% 57% 43%


**Deterministic only** **N/A** **50%** **81%** **21%**


+ LLM No 48% 90% 10%


+ LLM with tool hint No 67% 95% 5%


+ LLM with tool hint Yes 50% 86% 14%


The largest gain, +24pp in MCU family accuracy, came from the SVD ranking and database updates. The LLM added another +14pp on top of that when strings were available. The LLM improvement on stripped binaries was much smaller, only +5pp.


The LLM traces showed it was able to improve results by:


- matching board names, chip names, SDK errors/logs, and build-paths to MCUs
- analyzing headers, inferring the RAM size from the SP location and the IRQ count from vector-table entries


It’s not all roses and sunshine working with LLMs. Downsides are:


- Execution time (minutes vs milliseconds)
- Non-determinism
- Extra complexity
- Extra dependency
- Hallucinations (picking nonexistent SVDs)
- Cost (~10 cents per binary)


For now, we’re shipping the deterministic MMIO fingerprinting solution. If we encounter real-world issues, we’ll look into improving the MMIO scanning mechanism and/or bringing back the LLM.


**Citations**


\[1\] Malaschonok, D. (2026) ‘Identifying Microcontroller Architecture Through Static Analysis of Firmware Binaries’, *SDIoTSec 2026* . Available at:[https://www.ndss-symposium.org/wp-content/uploads/sdiotsec26-7.pdf](https://www.ndss-symposium.org/wp-content/uploads/sdiotsec26-7.pdf) .


\[2\] Miller, B., Bryant, T. and Knudson, B. (2024) *Sidekick in action: Analyzing firmware* , *Binary Ninja Blog* . Available at:[https://binary.ninja/2024/08/12/sidekick-in-action-analyzing-firmware.html](https://binary.ninja/2024/08/12/sidekick-in-action-analyzing-firmware.html) .
