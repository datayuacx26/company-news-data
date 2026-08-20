---
schema_version: "1.0.0"
document_id: "ed8083bde9bfb2c3a45df4d2a96a643621860659a985de79de86437b704b752c"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-migrate-from-telepresence-to-signadot-a-technical-guide/"
published_at: "2025-08-27T14:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:65599fa68e748c7a378333667448eee3daedbc2b36f442d3a85c43d0b138557a"
---

# How to Migrate from Telepresence to Signadot: A Technical Guide

If you’re a Telepresence user, you’re already familiar with the power of connecting your local development environment to a remote Kubernetes cluster. It’s a game-changer for speeding up the “inner loop” of coding and debugging. But as your team and application scale, you may find yourself hitting the limits of what a local-only development tool can do. The challenges of broken staging environments, unreliable automated tests, and coordinating changes across multiple services often remain.


Migrating to Signadot is more than a tool-for-tool replacement; it’s a strategic upgrade to your entire development workflow. Signadot provides a comprehensive testing platform that not only enhances your local development experience but also solves the “outer loop” problems of team collaboration and automated testing.


This guide provides a practical, phased approach to migrating from a Telepresence-centric workflow to the Signadot platform, ensuring a smooth transition that delivers immediate value and unlocks powerful new capabilities for your team.


### **The Foundational Step: One-Time Platform Setup**


Before you begin, you’ll need to set up the Signadot platform components. This is a straightforward, one-time process that typically takes less than a day.


1. **Install the Signadot Operator:** The operator is a Kubernetes-native component that manages Sandboxes, request routing, and other resources within your cluster. You can install it via Helm with a few simple commands provided in the Signadot dashboard.
2. **Install and Configure the Signadot CLI:** The CLI is your primary interface for managing Sandboxes from your local machine. Installation is available via direct download or Homebrew. Once installed, you’ll configure it with your organization name and an API key from the dashboard.


With these components in place, you’re ready to start the phased migration.


### **Phase 1: Replicate and Enhance Your Local Development Workflow**


The first goal is to replicate your existing Telepresence workflow with Signadot, meeting you where you are today while providing immediate improvements.


The Telepresence Way:


With Telepresence, you use telepresence connect to establish a network tunnel to the cluster and telepresence intercept to route traffic for a specific service to your local machine.


The Signadot Upgrade:


Signadot achieves the same outcome but with a more powerful and isolated approach using Sandboxes.


1.


Connect to Your Cluster:
Run signadot local connect to establish a secure tunnel between your machine and the cluster. This allows you to access cluster services by their DNS names, just like with Telepresence.


2.


Define Your Local Sandbox:
Instead of an imperative intercept command, you declaratively define your local development environment in a simple YAML file. This file specifies which remote service’s traffic should be routed to your local port.


3.


Apply and Run:
Apply the Sandbox definition with` signadot sandbox apply -f local-sandbox.yaml` . Now, start your service locally in your favorite IDE. Signadot will automatically route requests intended for the remote payment-service within your isolated Sandbox to your local instance on port 8080.


4.


Testing Your Local Service:
Once your Sandbox is active, you can direct traffic to your local service in a couple of ways:


- **API Testing:** Use tools like curl or Postman to send requests. You’ll need to include the Sandbox’s unique routing key as a header (e.g., Baggage: sd-routing-key=) to ensure the request is routed correctly to your local machine.
- **Web UI Testing:** For testing end-to-end flows from a web frontend, the **Signadot Chrome Extension** is the easiest method. Simply install the extension, select your active Sandbox from the dropdown, and it will automatically inject the necessary routing headers into all outgoing requests from your browser.


**The Immediate Benefit:** Your local service is now part of an isolated, high-fidelity Sandbox. You’re no longer intercepting traffic on a shared cluster where other developers’ changes could interfere with your work. This provides a more stable and predictable development experience from day one.


### **Phase 2: Upgrade to Automated PR Previews**


This is where you move beyond the capabilities of Telepresence and start solving team-wide problems. Instead of just testing locally, you can automate the creation of preview environments for every pull request.


1.


Define a PR Sandbox:
Create a Sandbox template that forks a deployment in the cluster (a “shadow deployment”) rather than connecting to a local workload. This template can be parameterized to use the image built for your specific PR.


2.


Integrate with Your CI/CD Pipeline:
In your CI workflow (e.g., GitHub Actions), add a step that runs after your image is built. This step uses the Signadot CLI to create a Sandbox for the PR.


**The Immediate Benefit:** Every pull request now gets its own isolated, shareable preview environment that spins up in seconds. This finally solves the “broken staging environment” problem. Developers can share the preview URL with teammates, product managers, and QA for review. By using the


**Signadot Chrome Extension** and selecting the PR’s Sandbox, stakeholders can interact with the new feature in a live, production-like context before code is merged, dramatically improving collaboration.


### **Phase 3: Unleash the Full Power of the Platform**


With preview environments automated, you can now layer on comprehensive testing capabilities that are out of scope for Telepresence.


1. Run Automated E2E Tests:
Your existing test suites (Cypress, Playwright, Postman, etc.) can be pointed at the Sandbox endpoints generated by your CI pipeline. Because each PR runs in an isolated Sandbox, you can run your full test suite in parallel without flaky results caused by environment contention.
2. Test Across Multiple Services with RouteGroups:
For complex features that span multiple PRs, you can use RouteGroups to combine several Sandboxes into a single, unified testing environment. This allows you to validate the complete, integrated feature before merging any of the constituent PRs.
3. Handle Stateful and Asynchronous Workflows:
Leverage Signadot’s native support for complex architectures. Use resource plugins to spin up temporary, isolated databases for testing schema migrations safely. Utilize the built-in isolation for asynchronous systems like Kafka and SQS to reliably test event-driven flows end-to-end.


### **From Telepresence to Signadot: A Conceptual Cheat Sheet**


Signadot vs Telepresence — The Upgrade Table :root{ —bg:#fff; —fg:#111; —muted:#6b7280; /* gray-500 */ —line:#e5e7eb; /* gray-200 */ —accent:#111827; /* gray-900 */ —badge:#111827; —x:#ef4444; /* red-500 */ } body{font-family:system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;background:var(—bg);color:var(—fg);margin:24px} .table-wrap{max-width:1100px;margin-inline:auto;border:1px solid var(—line);border-radius:12px;overflow:hidden;box-shadow:0 1px 4px rgba(0,0,0,.04)} table{width:100%;border-collapse:separate;border-spacing:0} thead th{ background:#f9fafb; text-align:left; font-weight:700; padding:14px 16px; border-bottom:1px solid var(—line); font-size:14px; line-height:1.2; } tbody td{ padding:16px; vertical-align:top; border-bottom:1px solid var(—line); font-size:14px; } tbody tr:last-child td{border-bottom:none} .muted{color:var(—muted)} .code{font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,“Liberation Mono”,“Courier New”,monospace;background:#f3f4f6;border:1px solid var(—line);padding:2px 6px;border-radius:6px} .badge{display:inline-block;font-size:12px;font-weight:600;color:#fff;background:var(—badge);padding:2px 8px;border-radius:999px} .x{color:var(—x);font-weight:800} /* Make it mobile-friendly */ @media (max-width:720px){ thead{display:none} table, tbody, tr, td{display:block;width:100%} tbody tr{border-bottom:1px solid var(—line)} tbody td{border:none;padding:10px 16px} tbody td::before{ content:attr(data-label); display:block; font-size:12px; text-transform:uppercase; letter-spacing:.04em; color:var(—muted); margin-bottom:4px; font-weight:600; } }


Use Case / Concept


Telepresence


Signadot


The Upgrade


**Local Development**


telepresence intercept


signadot sandbox apply -f <file.yaml>


Moves from an imperative command to a declarative, version-controllable definition of your isolated dev environment.


**PR Previews**


✖ N/A


Automated via CI/CD


Shifts testing “left,” enabling review and validation on every PR in a stable, isolated environment.


**Automated E2E Tests**


✖ N/A


Native CI/CD Integration


Allows reliable, parallel execution of tests against real dependencies, eliminating flaky tests.


**Cross-PR Testing**


✖ N/A


RouteGroups


Enables true end-to-end testing of complex features spanning multiple services and PRs before merging.


### **Conclusion: A Strategic Move for Scalable Development**


Migrating from Telepresence to Signadot is a natural evolution for growing engineering teams. By following this phased approach, you can smoothly transition your developers to a more powerful workflow, starting with an enhanced version of the local development loop they already know. Ultimately, you’ll unlock a comprehensive testing platform that improves collaboration, increases release quality, and accelerates your team’s ability to ship features with confidence.
