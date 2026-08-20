---
schema_version: "1.0.0"
document_id: "deaa4e174a4d124a7a17344b3bb2deddbd6449e1744f4aa8ad4d1ea1b9b7fd92"
company_key: "yc-quindar"
company: "Quindar"
source_id: "yc-quindar-rss-c6f333da4943"
canonical_url: "https://quindar.space/better-confidence-in-planning-and-flight-dynamics/"
published_at: "2026-06-22T16:10:00+00:00"
first_seen_at: "2026-08-10T02:21:35.833292+00:00"
fetched_at: "2026-08-10T02:21:38.860446+00:00"
content_hash: "sha256:548fb16f0e85479820084e42fa03c932ff7394b6f1a599f0bd94eb08a4800b40"
---

# Better confidence in planning and flight dynamics

Blog


June 22, 2026


Mission teams need more than isolated updates. They need a clearer operating picture.


With Quindar v0.35, we are expanding visibility and control across the workflows that shape day-to-day mission operations: flight dynamics, visibility generation, scheduling, asset awareness, and mission coordination.


This release brings together six updates designed to help teams operate with more confidence and less friction.


**Better confidence in planning and flight dynamics**


Quindar v0.35 adds covariance to predictive ephemeris, giving teams a clearer view of how uncertainty evolves as a spacecraft state is propagated forward. Instead of looking at position and velocity alone, operators can better understand how much confidence to place in the displayed trajectory and when a prediction may become operationally unreliable once it extends beyond the OD arc.


That added context is also available in the outputs teams rely on downstream. Quindar can propagate and store covariance alongside state for each predictive ephemeris timestamp and surface that information in ephemeris files, exports, and API responses. That helps teams carry flight dynamics context into the workflows where they review predictions, plan operations, and assess confidence over time.


**More accurate visibility generation and scheduling**


When valid OEM ephemeris is available, Quindar can now use it as the default source for visibility generation. That gives operators managing spacecraft a more accurate view of access opportunities than TLE-only propagation can provide on its own. When OEM is missing or stale, the system automatically falls back to TLE-based generation, while preserving visibility into the source used.


That combination of accuracy and fallback matters operationally. Teams get more trustworthy visibility calculations when higher-fidelity data is available, while retaining continuity when it is not. Quindar also improves the Visibilities experience itself by differentiating analysis from visibilities more clearly and surfacing the source behind each run.


Quindar v0.35 also introduces native scheduling support for AnySignal ground stations. Operators can discover AnySignal stations, generate visibilities, and schedule contacts within the workflows they already use. Instead of adding another disconnected process, this extends Quindar’s common operating picture to another part of the ground network teams rely on.


**Stronger operational awareness inside the asset view**


With v0.35, Quindar continues to refine the Asset Page into a more unified mission operations interface by bringing tickets, key flight dynamics context, contact scheduling, and task re-queueing closer to the work itself. That means operators can view tickets and selected flight dynamics details directly on the Asset Page, schedule both test and contacts over a longer period from the timeline, and quickly append or prepend executed tasks back into spacecraft queues.


For multi-spacecraft operators, mission operations teams, and flight dynamics teams, that reduces context switching, improves situational awareness during live contacts, and makes recovery and repeat-task workflows faster and more efficient.


**A more capable Portal experience for mission collaboration**


Portal is evolving from a request-management tool into a more capable mission operations platform.


Previously, teams had to rely on external file-sharing tools, manual approval chains, and separate mission contexts just to coordinate routine operational work. With v0.35, organizations can manage multiple missions and spacecraft within a unified Portal experience, securely share mission files, task definitions, and operational resources, and surface mission-specific spacecraft and scheduling options based on shared SGLs and mission context.


Just as importantly, authorized users can now work more directly inside Portal. They can select tasks, assign those tasks to upcoming contacts or directly to spacecraft queues, and schedule contacts without routing everything through a separate request workflow. For organizations operating multiple spacecraft and missions with trusted partners, that means less manual coordination, better cross-mission visibility, and a more scalable way to operate independently inside Portal.


**Built for modern space operations**


Taken together, these updates reflect the role Quindar is built to play in modern space operations.


Quindar is the future of mission management. v0.35 strengthens the common operating picture mission teams rely on by improving visibility, tightening operational context, and reducing friction across planning, coordination, and execution.


For teams working to stay mission-ready, that kind of clarity matters.
