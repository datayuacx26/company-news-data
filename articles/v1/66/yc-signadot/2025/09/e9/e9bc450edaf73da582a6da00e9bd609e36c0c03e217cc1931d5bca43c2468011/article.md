---
schema_version: "1.0.0"
document_id: "e9bc450edaf73da582a6da00e9bd609e36c0c03e217cc1931d5bca43c2468011"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/mobile-frontend-preview-with-signadot-sandboxes/"
published_at: "2025-09-16T21:26:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:95c919802b5e994a5dcbdc57c8caeb074c33fd7c7610dc45c3d0a810becc2ace"
---

# Mobile Frontend Preview with Signadot Sandboxes

## Tutorial Overview


This comprehensive guide demonstrates how to use **Signadot Sandboxes** to preview and test backend changes through a mobile frontend without affecting other developers using shared environments.


You’ll learn to set up isolated testing environments that allow your mobile app to interact with experimental backend features while maintaining complete separation from the baseline system.


By the end of this tutorial, you’ll have:


- A fully functional HotROD microservices backend running in Kubernetes.
- Signadot sandboxes for isolated feature testing.
- A modern iOS app that can switch between baseline and sandbox environments.
- The ability to test backend changes safely without impacting other team members.


## Prerequisites


- MacOS.
- Up and Running Kubernetes cluster.
- Xcode for iOS development.
- Basic Kubernetes and iOS development knowledge.
- A Signadot account (free at[app.signadot.com](https://app.signadot.com/) ).


## Part 1: Infrastructure Setup


This section is to demonstrate deploying main infrastructure components, including Hotrod backend services and Signadot operators.


### Step 1: Install Signadot CLI


The Signadot CLI is your primary interface for managing sandboxes and routing configurations. It bridges your local development environment with Signadot’s cloud platform.


You can follow the instructions to install Signadot CLI from this[guide](https://www.signadot.com/docs/getting-started/installation) .


### Step 2: Deploy HotROD Backend Services


HotROD is a sophisticated microservices demonstration application that simulates a ride-sharing platform. It consists of multiple interconnected services that communicate through both HTTP APIs and message queues.


To deploy hotrod backend services, you can follow the steps in this[guide](https://github.com/signadot/hotrod) .


#### **2.1** **Verify Service Deployment**


In this demo, hotrod backend services are deployed in “hotrod” namespace.


Expected services:


- frontend: Web interface and API gateway
- driver: Driver assignment and management
- location: Pickup/dropoff location services
- route: Route calculation & ETA services


Wait for all pods to show running status before proceeding.


#### **2.2 Check Service Endpoints**


**Expected ports:**


- frontend: 8080 - Main API gateway
- location: 8081 - Location service API
- driver:8082 - Driver service API
- route: 8083 - Route calculation API


### Step 3: Install and Configure Signadot Operator


The Signadot Operator runs within your Kubernetes cluster and manages the lifecycle of sandboxes, routing rules, and traffic isolation.


You can follow the instructions[here](https://www.signadot.com/docs/installation/signadot-operator) to install and configure Signadot operator.


#### **3.1** **Verify Operator Installation**


Expected components:


- agent: Cluster management
- routeserver: Traffic routing
- tunnel-proxy: Secure cloud connectivity
- signadot-controller-manager: Sandbox orchestration


Wait for all pods to reach running status.


## Part 2: Creating and Testing Sandboxes


Signadot sandboxes create isolated environments by forking specific services while maintaining connections to baseline services. This allows you to test individual components without affecting the entire system.


### Step 4: Create a sandboxed backend for the iOS app


In order to test out the sandbox feature of Signadot, you will use existing branches that contain two features. After applying the changes, we will test it out on the current iOS application.


#### **4.1 Pull branches and push images**


**ETA in Seconds Feature**


[This feature branch](https://github.com/signadot/hotrod/pull/259/files) modifies the route service to calculate ETA in seconds instead of minutes, simulating a “fast route calculation” feature that provides more granular timing.


In the ETA calculation function, the time unit is changed from time.Minute to time.Second. So Instead of ETAs like “8 minutes” or “15 minutes,” you’ll see “480 seconds” or “900 seconds”


Pull the existing branch with route service changes and push the image.


**Driver License Format Feature**


[This feature branch](https://github.com/signadot/hotrod/pull/260/files) modifies the driver service to add an “SD-” prefix to all license plates..


In the license plate generation function, an “SD-” prefix is added. So instead of license plates like “ABC-T7-12345-XYZ,” you’ll see “ABC-**SD-**T7-12345-XYZ.”


Pull the existing branch with driver service changes and push the image.


#### **4.2** **Sandbox Configuration Structure**


Create a directory for sandbox configurations:


#### **4.3 Create Signadot Sandbox Configurations**


These YAML files tell Signadot which services to fork and which enhanced images to use in each sandbox. Signadot will automatically deploy the base HotROD services if they don’t exist, then create forks with your enhancements.


Create signadot-config/route-fast-eta.yaml:


Create signadot-config/driver-sd-license.yaml:


Create signadot-config/combined-features.yaml


Make sure to replace YOUR_REGISTRY with your Docker registry name.


**Key Concepts:**


- forks: Creates an isolated copy of the driver/route service.
- Custom image: Uses your enhanced version with new features.


#### 4.4 Deploy All Sandboxes


#### 4.5 Verify Sandbox Status


Make sure sandboxes are in Ready status.


### Step 5: Configure Local Development


Set up port forwarding to access services locally. To do that, we will use the \`local connect\` command. \`local connect\` basically establishes a secure tunnel between your local development machine and the Kubernetes cluster where Signadot sandboxes are running. This connection enables proper[DevMesh traffic routing](https://www.signadot.com/docs/guides/request-routing/devmesh) for testing sandbox environments locally.


Make sure that \`$HOME/.signadot/config.yaml\` file exists and is configured.


## Part 3: Mobile App Integration


The HotROD iOS app is a SwiftUI-based ride-booking application that showcases Signadot’s microservice isolation capabilities. It allows users to seamlessly switch between different service environments through a developer mode interface. The app connects to HotROD’s microservices architecture and uses Signadot routing headers to direct API calls to specific sandbox versions, enabling real-time validation of features like fast ETA calculations and branded license plates. Users can book rides by selecting pickup and dropoff locations, choosing from available drivers, and tracking trip status, while developers can toggle between the production baseline and various sandbox configurations to test individual or combined microservice enhancements in isolation.


### **Project Structure Overview**


The HotROD iOS app includes:


**Environment selector** - Switch between baseline and sandbox.


**Location picker** - Location selector.


**Driver List** - Shows drivers list.


‍


In order to access the app code base, run the following commands:


### **Key Integration Components**


**API Service with Routing:**


This code creates HTTP headers that tell Signadot where to route API requests.


**Environment Management:**


Basically, what we are doing here is simply adding a new header in the request with signadot-routing-key that tells the router to route to the forked sandbox service.


### Step 6: Testing Sandbox Scenarios


#### **6.1** **Launch the iOS App**


1. Open the HotROD iOS project in Xcode
2. Build and run on the iOS Simulator
3. Verify the app loads successfully


#### **Scenario 1: Enhanced ETA with Seconds**


Purpose: Validate that only the route service is modified, showing ETA in seconds instead of showing ETA in minutes as in Production basline.


**Setup** :


1. Enable Developer Mode (wrench icon).
2. In Environment Selector, select “Custom Sandbox”.
3. Enter routing key: xxxxx (one displayed from \`signadot sandbox get route-fast-eta\` command).
4. Press the Connect button.


‍


**Test Steps** :


1. Select pickup location: “567 5th Ave”.
2. Select dropoff location: “Central Park”.
3. Enter your name: “Test User”.
4. Wait for drivers to load.
5. In Xcode, write in the debugger console “po request.allHTTPHeaderFields”.
6. You should see two open telemetry header fields
a. tracestate
b. baggage
7. You will see both headers matching the header key you entered in developer mode.


**Expected Results** :


- ETA Format: 2500 sec, 1880 sec, 3576 sec (now in seconds!).
- License Plates: T712345C, T798765C, T754321C (still no SD- prefix).
- Driver Count: 3 drivers available.
- Locations: Same basic locations.


**Key Validation Points** :


- Only ETA changed - This proves route service isolation.
- License plates unchanged - Driver service not affected.


**Screenshot Checklist:**


- Driver list showing ETA in seconds (not minutes).
- License plates still without SD- prefix.
- Environment selector showing custom sandbox.


Production Baseline Screenshot showing ETA in mins:


Custom Sandbox Screenshot showing ETA in seconds:


#### Scenario 2: Driver Service Enhancement (SD License Plates) - Combined with new ETA


**Purpose** : Validate that only the driver service is modified, showing SD- license plates.


**Setup** :


1. Enable Developer Mode.
2. Change routing key to: xxxxx (one displayed from \`signadot sandbox get combined-features\` command).
3. Select this sandbox environment.


**Test Steps** :


1. Select pickup location: “567 5th Ave”.
2. Select dropoff location: “Central Park”.
3. Enter your name: “Test User”.
4. Wait for drivers to load.


**Expected Results** :


- ETA Format: 2500 sec, 1880 sec, 3576 sec (now in seconds!).
- License Plates: SD-T712345C, SD-T798765C, SD-T754321C (now with **SD-** prefix!).


**Key Validation Points** :


- ETA is in seconds.
- License plates have SD- prefix - This proves driver service isolation.
- Same locations - Location service not affected.


## **Conclusion**


You’ve successfully implemented a complete mobile app testing pipeline using Signadot sandboxes. This setup enables:


- Safe feature testing without affecting production
- Isolated environments for parallel development
- Real mobile app validation of backend changes
- Seamless environment switching for comprehensive testing


For advanced use cases and enterprise features, consult the[Signadot documentation](https://www.signadot.com/docs/) and consider reaching out to the Signadot team for specialized support.
