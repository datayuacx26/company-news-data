---
schema_version: "1.0.0"
document_id: "ada59d12d1678369beabb94d3d0a2603626b05e99dd325647039353b59369b94"
company_key: "palantir-technologies-inc-class-a-common-stock"
company: "Palantir Technologies Inc."
source_id: "palantir-technologies-inc-class-a-common-stock-rss-f87a89a6619a"
canonical_url: "https://blog.palantir.com/osdk-and-mobile-applications-building-with-the-embedded-ontology-668432da6572"
published_at: "2026-04-06T17:16:04+00:00"
first_seen_at: "2026-07-20T03:31:10.454667+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:dc4ba97d45e5047cc7e33a0dda85a834fc5b923e52381d3e87bd155abce0db50"
---

# OSDK and Mobile Applications: Building with the Embedded Ontology

# OSDK and Mobile Applications: Building with the Embedded Ontology


[Palantir](https://palantir.medium.com/?source=post_page---byline--668432da6572---------------------------------------)


9 min read


·


Mar 31, 2026


--


*The Embedded Ontology lets you build powerful enterprise applications for teams that operate at the edge. Run the full, context-rich Ontology locally on the device. The power of Palantir, at the point of action.*


Press enter or click to view image in full size


Traditional enterprise platforms are powerful. They aggregate data, enforce governance, orchestrate workflows, and provide a single pane of glass for an organization to run their business. But that glass is mounted in a climate-controlled office, connected to reliable Wi-Fi, and viewed on a large monitor.


Now ride along with a field service technician. They’re driving between sites, inspecting equipment in mechanical rooms with no cell signal, documenting findings on a tablet while standing next to a roaring HVAC unit, among myriad other tasks. The reality at the edge is fundamentally different: connectivity is intermittent at best, latency is the enemy, and the person holding the device needs to make decisions *right now* , not after a loading spinner resolves.


Traditional mobile architectures — the ones solely built on REST calls and API-first patterns — break down in these environments. Every user action becomes a network request. Every network request becomes a potential failure. Every failure becomes a degraded experience, a workaround, or worse, a missed decision. You can bolt on caching layers and retry queues, but you’re still building on an assumption that the server is reachable. When it isn’t, the app is a shell.


**The Embedded Ontology flips this model entirely.** Instead of treating the mobile device as a thin client that peers into a remote platform, the Embedded Ontology treats the device as a *node in a distributed data system* . A scoped, context-rich projection of your Ontology lives *on the device itself* , complete with the object types, relationships, and actions that the user actually needs. Reads are local. Writes are local. Sync happens when connectivity allows, and conflict resolution is handled automatically on reconnect.


In this post, we’ll quickly walk through the architecture behind the Embedded Ontology, the mental model that makes it work, and the practical steps to build a Progressive Web App (PWA) powered by OSDK that operates seamlessly whether the device is connected or not. We’ll ground it in a simple working example, a field service technician application — but the pattern applies at the edges of any operational organization: warehouses, vehicles, remote sites, and beyond.


## Local & Global Context Through the Ontology


Data is only useful in context. A scanned equipment tag is just a string. But a scanned tag *matched against a local asset registry* , *validated against today’s work orders* , and *logged as an inspection event tied to a specific site and service contract* … that’s actionable intelligence.


The difference between the two is context, and context requires data from multiple sources at multiple scopes. This is the core design principle behind the Embedded Ontology: local data drives real-time decisions; global data provides the operational picture. From a logical perspective, think of the data on a device as existing in two layers:


Press enter or click to view image in full size


Local objects are the device’s working set. They’re created, updated, and queried with millisecond latency, fully independent of connectivity. When a technician logs an inspection finding, the write happens against the local store. No round trip, no spinner, no degraded fallback.


Global context objects are the organizational backdrop. They don’t change frequently, and the device doesn’t write to them, but they’re essential for making local actions meaningful. An` AssetRegistry` tells the technician what equipment is at this site. A` WorkOrder` tells them what they're here to do. A` ServiceContract` tells them what's covered.


**Sync when connected; operate autonomously when not.**


The sync model is straightforward:


1. **On first connection** , the device bootstraps its local store, pulling down the scoped set of objects it needs.
2. **While connected** , changes sync bidirectionally in the background.
3. **When disconnected** , the device continues operating normally. Read operations hit the local store. Write operations (actions, mutations) are captured locally as intents.
4. **On reconnect** , queued writes are reconciled upstream. Conflict resolution & state reconciliation occurs automatically.


The critical insight is that **step 3 is not a degraded mode** . The app doesn’t show a banner that says “You’re offline, some features may be unavailable.” The app just *works* . The technician may not even know whether they’re connected or not, and that’s the point. **Resiliency is the baseline assumption, not a feature.**


## Concepts: The Embedded Ontology


Before we build, let’s establish the mental model. If you’re new to Palantir’s ecosystem, the **Ontology** is the semantic system that brings together data, logic, action, and security primitives. It defines:


- **Object Types** : the nouns in your domain (` Asset` ,` Technician` ,` WorkOrder` ,` InspectionEvent` )
- **Properties** : attributes of those objects (` assetId` ,` status` ,` scheduledDate` ,` severity` )
- **Link Types** : relationships between objects (` InspectionEvent → Asset` ,` WorkOrder → Site` )
- **Actions** : the verbs that mutate state (` logInspection` ,` flagDefect` ,` completeWorkOrder` )


In a typical Foundry deployment, the Ontology runs within the AIP + Foundry service mesh and is queried by a given client. Your application calls the Ontology API, gets back objects, and renders them. This works well when you have reliable connectivity and can tolerate network latency. The Embedded Ontology takes a different approach: instead of querying the Ontology remotely, you define a **scoped projection** , a subset of the full ontology that is materialized *on the device itself* .


## Get Palantir’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


This projection includes:


- **An object set** : only the object types and instances that this device, user, or role actually requires to operate. A field technician’s tablet doesn’t need the procurement team’s vendor objects.
- **Local actions and mutations** : write intents that are captured and executed locally, then reconciled upstream when connectivity is available.


## How This Differs from Caching


You might be thinking: “This sounds like a cache with extra steps.” It’s not, and the distinction matters.


Press enter or click to view image in full size


A cache is a performance optimization. The Embedded Ontology is a **distributed data architecture** . The device has its own local store, its own query engine, and its own action execution layer. It participates in the broader Ontology through sync, and when useful real-time API calls.


## Basic Setup & Development


Let’s build. We’ll walk through the full setup: application setup in Developer Console, scaffolding the PWA, configuring the Embedded Ontology, and implementing the application. We’ll use the scenario where a technician drives to customer sites to inspect, maintain, and repair equipment. They work in environments where connectivity is unreliable. The app needs to surface today’s work orders, let them look up asset details, log inspection findings, flag defects, and sync everything back when they’re on the road or back at the office.


## Prerequisites


- A Foundry environment with Developer Console access
- An ontology with the object types (e.g., WorkOrder) you want to sync locally (or the willingness to create them)
- Node.js and npm installed locally (or use Code Workspaces in Foundry)


### Step 1: Create Your Application


In **Developer Console** , create a new Client-facing application:


1. Select your **Ontology** and the object types your app needs
2. Generate your **TypeScript SDK** , which gives you typed bindings for your object types and actions
3. **Configure website hosting** with a subdomain (you’ll need this for deployment)


*For detailed steps, see*[Create an Embedded Ontology application](https://www.palantir.com/docs/foundry/developer-console/create-embedded-ontology-application/) *in the Foundry documentation.*


### Step 2: Bootstrap with the Embedded Ontology Template


The fastest path to a working scaffold is the pre-configured template:


1. Navigate to **Start developing → Bootstrap in Foundry**
2. Select **Embedded Ontology React** from the template dropdown
3. Generate the repository


This gives you:


- ` @palantir/lohi-ts` pre-installed (Embedded Ontology)
- The **Vite plugin** configured for WebAssembly support
- Example sync patterns ready to customize


### Step 3: Configure Which Objects Sync Locally


This is where you define your **** object set, the scoped projection of the Ontology (i.e., specific Embedded Ontology) that lives on the device. Open` src/client.ts` and specify your sync targets:


```text
import { createClient } from "@palantir/lohi-ts";                                                                                                                                                             import {                                                                                                                                                                                                        InspectionEvent,    ServiceNote,                                                                                                                                                                                                  DefectReport,                                                                                                                                                                                                 WorkOrder,    AssetRegistry,    ServiceContract,    SiteLayout,  } from "@your-app/sdk";   const client = createClient(    import.meta.env.VITE_FOUNDRY_URL,    import.meta.env.VITE_ONTOLOGY_RID,    tokenProvider,    "field-service-ontology",    [      // Reference data — snapshot sync all      AssetRegistry,      ServiceContract,      SiteLayout,       // Filtered snapshot — only open work orders      {      objectType: WorkOrder,      objectSet: (c) => c(WorkOrder).where({ status: "Open" }),      },       // Peering — bidirectional sync for field-created objects      {      objectType: InspectionEvent,      usePeering: true,      },      {      objectType: ServiceNote,      usePeering: true,      },      {      objectType: DefectReport,      usePeering: true,      },  ]  );   export default client;
```


### **Step 4: Implement the Sync Pattern**


```text
import { SyncState } from "@palantir/lohi-ts";  import { useState, useEffect } from "react";  import client from "./client";                                                                                                                                                                                 function FieldServiceApp() {                                                                                                                                                                                  const [ready, setReady] = useState(false);  const [syncError, setSyncError] = useState<string | null>(null);   useEffect(() => {      (async () => {      try {          const state = await client.syncState();          if (state !== "Ready") {          await client.sync();          }          setReady(true);      } catch (err) {          setSyncError("Initial sync failed. Check connectivity and refresh.");      }      })();  }, []);   if (syncError) return <div className="sync-error">{syncError}</div>;  if (!ready) return <div className="sync-loading">Syncing field service data...</div>;   return <FieldServiceWorkflow />;  }
```


Here’s what’s happening:


1. ` **client.syncState()**` checks whether the local store has been bootstrapped. On first launch, it hasn't.
2. ` **client.sync()**` pulls down the configured object types and populates the local store. This is the initial "hydration" of the Embedded Ontology on the device.
3. **Once**` **ready**` **is**` **true**` , all subsequent reads and writes hit the local store. Queries execute with millisecond latency. Actions are captured locally. The device is fully autonomous.
4. **Background sync** handles ongoing reconciliation, pushing local writes upstream and pulling down updates, whenever connectivity is available.


### Step 5: Build the Offline Workflow


With the Embedded Ontology bootstrapped, your application code looks identical whether the device is online or offline. Here’s a greatly simplified inspection workflow:


```text
import {              InspectionEvent,      LogInspection,      AssetRegistry,      WorkOrder    } from "@your-app/sdk";    import type { Client } from "@palantir/lohi-ts";     async function handleInspection(      assetTag: string,      workOrderId: string,      client: Client    ) {      // 1. Look up the asset from the local registry (millisecond latency)      const asset = await client(AssetRegistry)        .where({ assetTag })        .fetchOne();       if (!asset) {        return { status: "UNKNOWN_ASSET" as const, assetTag };      }       // 2. Validate against today's work order      const workOrder = await client(WorkOrder)        .where({ workOrderId })        .fetchOne();       // 3. Log the inspection event locally      await client(LogInspection).applyAction({        assetId: asset.assetId,        workOrderId: workOrder.workOrderId,        siteId: workOrder.siteId,        technicianId: getCurrentTechnician(),        timestamp: new Date().toISOString(),        status: "INSPECTED",        notes: "",      });       // 4. Return result to the UI immediately      return { status: "OK" as const, asset, workOrder };    }
```


Notice what’s *not* here: no network checks, no try/catch around API calls, no fallback logic for offline mode. **The code doesn’t know or care about connectivity.** It queries the local ontology, executes a local action, and returns. The sync layer handles the rest.


### **Step 6: Deploy and Install as a PWA**


Tag a release in your repository to trigger deployment either through the UI or by running a few git commands. For example:


```text
git tag 1.0.0  git push origin 1.0.0 - tags
```


Foundry’s CI builds your application and deploys it to your configured subdomain. Users can then install the PWA directly from their browser:


- Desktop: Look for the install icon in the address bar
- Mobile: Use “Add to Home Screen” from the browser menu


Once installed, the app launches like a native application, full-screen, with its own icon, and fully functional offline using the Embedded Ontology.


Press enter or click to view image in full size


## Wrapping Up


The Embedded Ontology represents a fundamental shift in how we think about enterprise applications at the edge. The device isn’t a window into the platform. **It is the platform at the point of action.**


When a technician inspects equipment in a basement with no signal, they shouldn’t be waiting on a server. When they’re documenting a defect on a rooftop, they shouldn’t be scribbling on paper to enter later. When they’re reviewing an asset’s service history between sites, the lookup shouldn’t depend on a cell signal.


Resilience, speed, and contextual intelligence belong at the edge, and the Embedded Ontology makes this practical, not theoretical. The same typed Ontology that powers dashboards and workflows in Foundry now runs locally on the device, with the same object types, the same actions, and the same relationships. The developer experience is nearly identical. The user experience is dramatically better than conventional approaches.


And this is just the beginning. The trajectory points toward **smarter sync** (adaptive policies based on connectivity patterns and data priority), **AI-augmented local actions** (on-device models that leverage the local ontology for context-aware decisions), and **mesh ontologies** (devices that sync not just with the platform, but with each other). Moving beyond PWAs, the embedded ontology can be augmented with time-series, relational, and object storage, along with local LLM and vector search capabilities for AI workloads. Data flows from ingestion to ontology and ultimately to ROI-generating applications and services are possible.


The edge is getting smarter, and the Ontology is the engine.


*Editor’s Note: This is the second in a series of posts about the Embedded Ontology. See the first post*[here](https://blog.palantir.com/manufacturing-with-the-connected-edge-f8d8871ec437) *.*


*To get started with the Embedded Ontology, visit the*[Foundry documentation](https://www.palantir.com/docs/foundry/developer-console/create-embedded-ontology-application/) *or explore the Embedded Ontology React template in the Developer Console.*
