---
schema_version: "1.0.0"
document_id: "e3cdd93ed562efd9212c624af5c3984a8f25f06c0548c9dd390590ab9f239427"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/collaborative-pre-merge-testing-for-multi-pr-features/"
published_at: "2025-08-28T17:22:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:2bfd0f5e9b7e4189abbf74d3602718303b4568adc136ea71a4eb2d6f66696258"
---

# How to Test Features Spanning Multiple Pull Requests

When new features span multiple microservices, testing becomes a major challenge. Coordinated changes across separate pull requests (PRs) must be validated together before merging. This tutorial provides a hands-on guide to building an automated system using Signadot that creates unified, ephemeral preview environments for all PRs related to a single feature epic.


This technical guide is a comprehensive, step-by-step resource for:


- Setting up a collaborative pre-merge testing environment with Signadot and HotROD
- Troubleshooting real-world issues
- Automating ephemeral preview environments with GitHub Actions
- Implementing and testing multi-service features such as “Dynamic Surcharges”


**Note:** The configuration files and code referenced in this guide can be found in the following repository:
[https://github.com/signadot/examples/tree/main/collaborative-pre-merge-testing-for-multi-PR-features](https://github.com/signadot/examples/tree/main/collaborative-pre-merge-testing-for-multi-PR-features)


## **1. Prerequisites and Baseline Environment Setup**


### **1.1 Deploy the HotROD Demo Application**


**Refer to the**[HotROD README](https://github.com/signadot/hotrod) **for installation steps.**


The HotROD YAMLs will automatically add the necessary annotations for Signadot integration. Follow the installation instructions in the HotROD README file.


` git clone https://github.com/signadot/hotrod.git cd hotrod`
*# Follow the installation instructions in the HotROD README*


### **1.2 Verify the Baseline**


Connect to your cluster and access the application:


` signadot local connect --cluster=<your-cluster-name>`


Then access the frontend using the in-cluster URL:[http://frontend.hotrod.svc:8080](http://frontend.hotrod.svc:8080/)


Request a ride by selecting a pickup and dropoff location. A successful request will display a car on a map, confirming the baseline is working.


Baseline HotROD App Baseline HotROD App Baseline HotROD App Baseline HotROD App


## **2. The Scenario: Implementing “Dynamic Surcharges”**


This scenario demonstrates a real-world example: implementing a “Dynamic Surcharges” feature in the HotROD demo app, which requires changes to both the backend (route service) and frontend services. All work for this feature is tracked under the identifier EPIC-42.


### **2.1 Backend Change: route Service**


Add a new gRPC endpoint to the route service to calculate the surcharge.


**Edit services/route/route.proto:**


**Implement the server logic in services/route/server.go:**


**Add the client method in services/route/client.go:**


### **2.2 Frontend Change: frontend Service**


Modify the frontend service to call the new GetSurcharge endpoint and display the result to the user.


**Update the frontend server in services/frontend/server.go:**


**Update the React frontend in services/frontend/react_app/src/pages/home.tsx:**


**Update the UI template in services/frontend/templates/index.html:**


### **2.3 Build New Docker Images**


**When you create the sandboxes, they need to use a new image - the one that has the code changes for the new feature.**


After making the code changes, build new container images:


*# Generate Go code from proto files*
` cd hotrod make generate-proto`


*# Build the React frontend*
` make build-frontend-app`


*# Build the Docker image with all changes*
` make build-docker`
*# Tag and push the image*
` docker tag signadot/hotrod:epic-42-surcharge-only-linux-amd64 <your-dockerhub-username>/hotrod:epic-42-surcharge-only docker push <your-dockerhub-username>/hotrod:epic-42-surcharge-only`


**Note:** Replace` <your-dockerhub-username>` with your actual Docker Hub username or registry path.


## **3. Manual Workflow: Unified Preview with Signadot**


### **3.1. Create Sandboxes for Each Service**


Create` route-surcharge-sandbox.yaml:`


Create` frontend-surcharge-sandbox.yaml:`


Apply the sandboxes:


**Important:** In your cluster, there will be 2 versions of the frontend and route deployments (baseline & sandbox).


### **3.2. Understanding the Testing Use-Case**


Before discussing RouteGroups, it’s important to understand the testing scenarios we need to cover. In our “Dynamic Surcharges” feature, we have:


- **fe1** : Baseline frontend (original version)
- **fe2** : New frontend with surcharge feature
- **r1** : Baseline route service (original version)
- **r2** : New route service with surcharge API


The following combinations can be tested:


1. **fe1 → r1** (both baseline) - This is just testing the baseline versions to ensure nothing is broken
2. **fe1 → r2** - To ensure changes in r2 haven’t broken fe1. Here you can select the sandbox corresponding to r2 and use fe1
3. **fe2 → r1** - This is expected to fail as fe2 depends on new APIs in r2, but you can test whether fe2 handles this gracefully. Here you select the sandbox corresponding to fe2 in the chrome extension **‍**
4. **fe2 → r2** - This is where we test the new feature end-to-end and here’s where RouteGroups are used as detailed in the next section. RouteGroups combine multiple sandbox contexts into one with its own unique routing key


### **3.3. Create a RouteGroup**


Create` epic-routegroup.yaml:`


Apply it:


### **3.4. Test the Unified Preview**


- Install the[Signadot Chrome Extension](https://chrome.google.com/webstore/detail/signadot/)
- Log in and select the epic-42-preview RouteGroup
- Access the application using the RouteGroup URL:[https://hotrod-frontend—epic-42-preview.preview.signadot.com](https://hotrod-frontend--epic-42-preview.preview.signadot.com/)
- Request a ride; you should see


‍ **Dynamic Surcharge Applied: $1.25**


New Version with Dynamic Surcharges


*New Version with Dynamic Surcharges*


Sandbox Selection in Chrome Extension


*Sandbox Selection in Chrome Extension*


Signadot Dashboard(Cluster) Signadot Dashboard showing cluster


Signadot Dashboard(Sandboxes) Signadot Dashboard showing sandboxes


Signadot Dashboard(Route Group) Signadot Dashboard showing routegroup


## **4. Automation: GitHub Actions Integration**


### **4.1. Overview**


Automate the creation and management of Signadot sandboxes and RouteGroups for every pull request and epic using GitHub Actions. This enables fully automated ephemeral preview environments for collaborative testing.


**Professional Note:** The automation workflows and templates provided here are based on Signadot’s official patterns and best practices. They are designed to work when correct parameters, secrets, and valid container images are supplied. However, as with any CI/CD automation, users should validate these workflows in their own environment and adapt as needed for their specific use case and infrastructure.


### **4.2. Add Required GitHub Secrets**


In your repository settings, add the following secrets:


- ` SIGNADOT_API_KEY` (your Signadot API key)
- ` SIGNADOT_ORG` (your Signadot organization name)
- ` SIGNADOT_CLUSTER` (your Signadot cluster name)


### **4.3. Workflow 1: Create Sandbox on PR**


Create` .github/workflows/create-pr-sandbox.yml:`


### **4.4. Workflow 2: Link Epic via PR Comment**


Create` .github/workflows/link-epic-preview.yml:`


**Screenshots:** User commenting /epic EPIC-42 on a PR User commenting /epic EPIC-42 on a PR


GitHub Action workflow running GitHub Action workflow running


Preview URL response in PR Preview URL response in PR


## **Conclusion**


This tutorial demonstrated how to build collaborative pre-merge testing for multi-PR features using Signadot. We successfully implemented a Dynamic Surcharges feature across multiple microservices, created isolated sandboxes, unified them through RouteGroups, and established automated workflows. The approach provides isolated testing environments for each PR while enabling unified preview of integrated features. This methodology scales from manual testing to full automation, making it suitable for teams testing complex, multi-service features in production-like conditions.
