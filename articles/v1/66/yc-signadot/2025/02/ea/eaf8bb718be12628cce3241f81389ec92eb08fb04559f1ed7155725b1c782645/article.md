---
schema_version: "1.0.0"
document_id: "eaf8bb718be12628cce3241f81389ec92eb08fb04559f1ed7155725b1c782645"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-run-scalable-performance-tests-using-grafana-k6-and-signadot/"
published_at: "2025-02-04T19:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:b326149a5e96061862e245b9a07d718e8199c7b382e7824447137dfdc0e93ab5"
---

# How to Run Scalable Performance Tests Using Grafana/K6 and Signadot

**Isolate Workloads, Parameterize Tests, and Optimize Performance at Scale**


## Introduction


Performance testing in pull requests is often overlooked due to the complexity of setting up full-scale environments and the challenge of obtaining reliable results in shared clusters. **Signadot** simplifies this process by providing an efficient and scalable solution for running performance tests without the need for extensive infrastructure provisioning. With **Signadot Sandboxes** , developers can seamlessly fork only the necessary services, such as HotROD’s location service, into isolated environments, eliminating the overhead of full-scale deployments. Additionally, the **Signadot Job Runner Group** automates the execution of **Grafana K6** test scripts as jobs. This allows teams to conduct repeatable load tests while dynamically adjusting parameters such as URLs, virtual users (VUs), and duration. This streamlined approach ensures that performance evaluations take place in a controlled, production-like environment. As a result, teams receive rapid feedback, reduce infrastructure costs, and gain confidence in microservice scalability—all while maintaining efficient pull request workflows. This guide walks through, step by step, how Signadot implements this concept**.**


**What You’ll Learn**


- Deploying and isolating microservices with Signadot Sandboxes.
- Configuring Job Runner Groups for parallel test execution.
- Designing dynamic K6 scripts with run-time parameters.
- Analyzing results to validate scalability and reliability.


**Prerequisites**


- A Kubernetes cluster with[Signadot installed](https://www.signadot.com/docs/overview) .
- kubectl configured for cluster access.
- Basic familiarity with Kubernetes, YAML, and JavaScript.


## ‍ **Deploy the HotROD Demo Application**


**‍** HotROD is a microservice-based application designed for testing distributed systems. Deploy it to your cluster with the following commands:


This deploys HotROD with the devmesh overlay in the hotrod namespace.


Next, We’re going to Introduce the the delay in the location service API and rebuild the image to simulate delayed response.


Run following commands to build location service API delay response docker image


## **Create an Isolated Sandbox Environment to simulate delay response in next PR release**


Signadot Sandboxes allow you to fork services for isolated testing without impacting the baseline environment.


### **Define the Sandbox Configuration**


Create` location-sbx.yaml` to fork the location-service deployment:


### **Apply the Sandbox**


Note: If you haven’t already installed Signadot CLI on your computer, please go to this link and install the Signadot CLI:[https://www.signadot.com/docs/getting-started/installation/signadot-cli](https://www.signadot.com/docs/getting-started/installation/signadot-cli)


This will create sandbox for location service api.


Optional: Scale replicas for load testing by adding a customPatch to the sandbox YAML. Please go to this link to have some idea on how to create custom patches for sandboxes[https://www.signadot.com/docs/reference/sandboxes/spec#patch](https://www.signadot.com/docs/reference/sandboxes/spec#patch)


## **Configure Job Runner Groups for Scalable Execution**


Job Runner Groups enable distributed test execution across multiple pods, ideal for large-scale load testing.


### Define the Job Runner Group


Create job-runner.yaml to configure a group named k6-perf-tests:


### Apply the Jobrunner group


This will create Jobrunner group on Signadot. You can see it from Signadot GUI


## **Design a K6 Load Test Script with Dynamic Parameters**


K6 scripts can leverage environment variables to decouple test logic from configuration, enabling reusable tests across environments.


Key Features of the Script


- Dynamic URL: The target endpoint is passed via __ENV.URL.
- Configurable Load: Virtual users (__ENV.VUS) and duration are adjustable at runtime.
- Thresholds: Built-in checks validate response status and latency.


## **Create a Parameterized Job Template**


This job template is designed to dynamically route HTTP request traffic to a specified target destination—either baseline or sandbox—for performance benchmarking. The workload, derived from the location-service-api, enables comparative analysis of an upcoming pull request by evaluating key performance metrics.


**Execute the test with custom parameters:**


**injectRoutingKey :**


- disabled: Requests are routed to the baseline as no routing key is injected.
- auto: Requests are routed to the sandbox with the routing key injected by a proxy.


**Note:** **traficManager.injectroutingKey won’t work with HTTPS traffic. If you are running your workload under HTTPS, Please go to this URL**[https://www.signadot.com/docs/tutorials/testing/e2e-with-cypress#injecting-routing-context](https://www.signadot.com/docs/tutorials/testing/e2e-with-cypress#injecting-routing-context) **. It will demonstrate how to inject routing keys to your HTTP request.**


## **Analyze Test Results**


After execution, K6 outputs metrics such as:


- Requests per second (RPS)
- Latency distributions (p95, p99)
- Error rates


Test result summary of baseline deployment: **location-service-api**


Test result summary of sandbox created from Signadot: **location-service-api** ‍


Use these metrics to:


- Identify performance bottlenecks under load.
- Validate if the service meets SLA requirements (e.g., response times < 500ms).
- Compare results across different replica configurations.


As you can see The comparison between the baseline and the proposed change ( sandbox ) reveals significant performance degradation in the upcoming version. Below are the critical observations:


**1. Response Time Violations** ‍


Baseline (Current Workload):


- All checks passed: Status 200 (100%) and response time <500ms (100%).
- Average HTTP request duration: 303.94 ms (well within the 500ms threshold).
- p95 latency: 357.3 ms (compliant with SLA).


Upcoming Change:


- Critical failure: Response time exceeded 500ms for 100% of requests.
- Average HTTP request duration: 10.3 seconds (20x higher than baseline).
- p95 latency: 10.35 seconds (severely breaches SLA).


**2. Throughput and Iteration Efficiency** ‍


Baseline:


- Completed 1,089 iterations at ~9 iterations/second.
- System efficiently handled 20 virtual users (VUs) with stable performance.


Upcoming Change:


- Completed only 135 iterations at ~1 iteration/second (88% drop in throughput).
- Severe bottleneck observed, even with the same VU capacity (20 VUs).


**3. Root Cause Indicators**


**‍** Latency Distribution:


-


The upcoming change shows uniformly high latency (min: 10.2s, max: 10.39s), suggesting a systemic delay (e.g., introduced artificial latency, inefficient code, or resource contention).


-


HTTP request waiting time (avg: 10.3s) dominates total duration, indicating backend processing bottlenecks.


Failed Threshold Automation:


- The automated check for response time failed entirely, highlighting the urgency to address the regression.


**Best Practices for Effective Testing**


1. **Isolate Critical Tests:**
Use dedicated sandboxes for mission-critical services to avoid shared-baseline interference.
2. **Parameterize Early:**
Design scripts with environment variables (e.g., __ENV.URL) for reusability.
3. **Start Small, Scale Gradually:**
Begin with low VUs and incrementally increase load to identify breaking points.
4. **Automate Thresholds:**
Define pass/fail criteria in K6 scripts to automate result interpretation:


‍**Additional Recommendations:**‍


- **Explore Comprehensive Test Scenarios:** Utilize advanced features from the[K6 documentation](https://grafana.com/docs/k6/next/javascript-api/) to expand your testing scenarios. This includes options for custom metrics, complex thresholds, and integration with third-party tools.
- **Refine Test Parameters Iteratively:** Experiment with different virtual user configurations, durations, and script complexities to thoroughly evaluate system performance under various conditions.
- **Leverage Visualized Metrics:** Consider integrating K6 outputs with Grafana for detailed visualizations, helping teams identify trends and bottlenecks more effectively.


## Conclusion


By integrating K6 with Signadot Sandboxes and Job Runner Groups, engineering teams can:


- Run performance tests in isolated environments with dynamic parameters.
- Scale tests efficiently across distributed pods.
- Validate micro-service scalability and reliability before deployment.


## Next Steps:


- Integrate this work-flow into CI/CD pipelines to catch regressions early.
- Explore[Signadot’s Advanced Custom Patches](https://www.signadot.com/docs/reference/sandboxes/advanced/custom-patches) for complex testing scenarios.
- Monitor trends over time to proactively address performance degradation.
