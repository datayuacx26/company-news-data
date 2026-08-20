---
schema_version: "1.0.0"
document_id: "eb31da9c3024865684ce144b57019236cf0783a15db4f357cd2bc660ee92b5e6"
company_key: "super-micro-computer-inc-common-stock"
company: "Super Micro Computer Inc."
source_id: "super-micro-computer-inc-common-stock-rss-44766a8a0f56"
canonical_url: "https://learn-more.supermicro.com/data-center-stories/spec-cpu-2026-benchmark-suites"
published_at: "2026-06-11T22:40:29+00:00"
first_seen_at: "2026-07-20T04:35:54.594017+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:3fd5a46e9bebc7ad0514b5a17487a3aa46b163e21d14e8ba86938a94b6748405"
---

# SPEC CPU 2026 Benchmark Suites Released: See Supermicro's Strong Results

# SPEC CPU 2026 Benchmark Suites Released: See Supermicro's Strong Results


Written by:[Supermicro Experts](https://learn-more.supermicro.com/data-center-stories/author/supermicro-experts) |


3 min read


[AI](https://learn-more.supermicro.com/data-center-stories/tag/ai?hsLang=en)


The


[Standard Performance Evaluation Corporation (SPEC)](https://www.spec.org/) officially released the SPEC CPU 2026 benchmark suites, representing the largest update to industry-standard CPU testing in almost a decade. Stepping up from the previous CPU 2017 suite, the 2026 version expands the benchmark count from 43 to 52. It more than doubles the number of lines of code and heavily incorporates modern open-source workloads. Crucially, it broadens testing domains to cover AI orchestration, electronic design automation (EDA), complex databases, and deep graph analytics.


The SPEC benchmark suite for CPU results shows both the performance of the CPU for:


- Integer Speed (INT Speed): Measures the time it takes for a system to execute a single, sequential task that relies heavily on integer math. It consists of 13 benchmark testing tasks, such as compilation, cryptography, and compression.
- Integer Rate (INT Rate): Measures the total amount of integer workloads a system can process simultaneously. It contains 14 benchmarks designed to test multiprogramming capabilities and the performance of heavily parallelized, multithreaded integer operations.
- FP Speed (Floating-Point Speed): Measures single-task performance for complex mathematical calculations, such as physics simulations and fluid dynamics. It features 13 benchmarks stressing computing throughput and DRAM/cache performance.
- FP Rate (Floating-Point Throughput): Measures aggregate throughput for simultaneous, heavy mathematical calculations. It contains 12 benchmarks pushing multi-core and multi-threaded floating-point hardware.


Each of these benchmarks can be run with different numbers of CPUs, but here we will focus only on the 1-socket and 2-socket results.


**Integer Speed:**


- 1-socket results – Supermicro holds the top two spots using the[CloudDC Server](https://www.supermicro.com/en/products/system/clouddc/1u/as-1116cs-tn) with AMD EPYC™ 9755 (128 cores) and 9965 (192 cores) CPUs, respectively1.


- 2-socket results – Supermicro holds the top spot and 4 of the top 6 positions. The highest-performing result is a[Hyper system](https://www.supermicro.com/en/products/system/hyper/2u/as-2126hs-tn) with AMD EPYC 9755 CPUs (256 cores)2.


**Integer Rate:**


- 1-socket results – Supermicro holds the top 2 results using a[Supermicro SuperBlade®](https://www.supermicro.com/en/products/superblade/module/sbi-612ba-1ne34-lcc.php) using a single Intel® Xeon® 6990E+ (288 cores) CPU. The second-place score also uses the Intel Xeon 6990E+ processor in a Hyper server3.
- 2-socket results – Supermicro holds the top spot using a Hyper server with the Intel Xeon 6990E+ (total of 576 cores) CPU. The Peak Result is 14+ % faster than the 2nd place result4.


**Floating Point Speed:**


- 1-socket results – Supermicro holds two of the top three positions using a CloudDC server with the AMD EPYC 9965 (192 cores) and 9755 (128 cores) CPUs, respectively5.
- 2-socket results – A Supermicro Hyper server with the AMD EPYC 9755 CPU (total of 256 cores) was within 5% of the fastest system6.


**Floating Point Rate:**


- 1-socket results - Supermicro owns the top three positions for this benchmark. The fastest system is a Hyper system using the Intel Xeon 6990E+ (288 cores) CPU, and the 2nd-place result is the same processor on the SuperBlade system. The 3rd place result is also a SuperBlade system with the Intel Xeon 6980E+ (264 cores) CPU7.
- 2-socket results – Supermicro has the highest performance among the top two server vendors, based on the Q4 CY25 IDC results*. Using a Hyper server with two Intel Xeon 6980P processors, totaling 576 cores, Supermicro attained the top position8.


Visit[www.spec.org](http://www.spec.org/) for more information.


* *Source: IDC Worldwide Quarterly Server Tracker, 2025Q4*


*References:*


1. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00227.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00227.html) and[https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00233.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00233.html)
2. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00231.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00231.html)


3. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00265.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00265.html)


and[https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00261.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00261.html)
4. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260518-00252.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00261.html)
5. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00234.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00234.html) and[https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00228.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00228.html)
6. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260210-00232.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00262.html)
7. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00262.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260518-00253.html) and[https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00266.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00266.html) and[https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00258.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00258.html)
8. [https://spec.org/cpu2026/results/res2026q2/cpu2026-20260518-00253.html](https://spec.org/cpu2026/results/res2026q2/cpu2026-20260519-00258.html)


### Subscribe to Data Center Stories


By clicking subscribe, you consent to allow Supermicro to store and process the personal information submitted above to provide you the content requested.


You can unsubscribe from these communications at any time. For more information on how to unsubscribe, our privacy practices, and how we are committed to protecting and respecting your privacy, please review our[Privacy Policy](https://www.supermicro.com/en/about/privacy-policy) .
