---
schema_version: "1.0.0"
document_id: "cd465d3f9ee4f0f6206c569030836eb8127c89fdfc0d6d1854e6b203d0f6afed"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/flutter-sdk-identity-verification-integration-guide/"
published_at: "2026-07-28T16:33:07.798+00:00"
first_seen_at: "2026-07-29T00:27:47.387739+00:00"
fetched_at: "2026-07-29T00:27:49.458663+00:00"
content_hash: "sha256:deaef7f9e46fe673d91c4017e0bb9cdcde5ba4e115b3bbf4fffb818467887c1e"
---

# Flutter SDK: Add Identity Verification to Your App

[Back to blog](https://didit.me/blog/) Blog · July 28, 2026


# Flutter SDK: Add Identity Verification to Your App


A developer guide to adding identity verification to a Flutter app with the Didit SDK: native setup, backend-created sessions, Dart result handling, typed errors, webhooks, testing, security, and release operations.


By Didit


·


July 28, 2026 ·


Updated Jul 28, 2026


A Flutter SDK integration for identity verification should keep permanent credentials and final authorization on your backend while the mobile app launches a native capture flow with a short-lived session token. The Didit Flutter SDK exposes one Dart API over the native iOS and Android verification SDKs, returning typed completion, cancellation, or failure results to the app. The complete decision still belongs to the backend webhook or retrieval flow.


This guide uses only Dart methods and result types verified against the current local SDK source and tests. Native dependency details change across releases, so platform configuration is described by responsibility and linked to the canonical SDK guide instead of copying a version-sensitive Podfile or Gradle block.


## **Key takeaways**


- **Create production sessions on the backend.** Keep the API key off the device and send only the session token needed by the SDK.
- **Use the typed Dart result for user experience, not authorization.**` VerificationCompleted` means the SDK flow ended; inspect status for display and wait for the authoritative backend decision.
- **Handle cancellation, typed failures, and unexpected platform errors separately.** They need different recovery and analytics.
- **Treat native setup as release infrastructure.** iOS privacy keys, near-field communication (NFC) entitlements, deployment targets, Android dependencies, packaging, and permissions must be tested on real devices.
- **Design the whole lifecycle.** Session creation, app handoff, capture, webhook verification, idempotent state changes, review, retries, and observability form one integration.


## **What the Didit Flutter SDK does**


The` didit_sdk` package wraps native iOS and Android SDKs behind a shared Dart interface. It launches the verification user interface as a native full-screen flow and returns when the user completes, cancels, or encounters an error.


The SDK can launch workflows with[ID Verification](https://didit.me/products/id-verification) ,[Liveness Detection](https://didit.me/products/liveness) , and other configured checks. The workflow determines which steps appear; the Flutter call does not hard-code them.


The public Dart surface relevant to the lifecycle is:


```text
DiditSdk.startVerification(token, config: ...)
DiditSdk.startVerificationWithWorkflow(workflowId, vendorData: ..., config: ...)
```


For production, prefer` startVerification` with a backend-created token. The workflow-ID method is simpler but gives the backend less control over advanced parameters.


## **Architecture: backend, Flutter app, SDK, and webhook**


The production flow has four trust boundaries:


Component Owns Must not own


Your backend API key, workflow choice, customer reference, session creation, final customer state Camera interface


Flutter app Handoff request, loading and recovery UI, SDK launch, local analytics Permanent API key or final authorization


Didit Flutter SDK Native capture and configured verification flow Your product entitlement decision


Webhook/retrieval worker Authenticated result ingestion, deduplication, reconciliation Unverified client assumptions


The sequence is:


1. The signed-in Flutter app asks your backend to begin verification.
2. Your backend creates a verification session with the intended workflow and stable internal customer reference.
3. The backend returns the scoped` session_token` to the app.
4. The app passes that token to` DiditSdk.startVerification` .
5. The SDK presents the native flow and returns a typed result for immediate user experience.
6. Your backend receives and verifies the result event, reconciles canonical state, and updates the customer under your policy.
7. The app reads your backend’s customer state before granting access or claiming final approval.


This architecture does not trust an on-device success screen.


For the server-side contract and event boundary, see the[ID verification API integration guide](https://didit.me/blog/id-verification-api-integration-evaluation-guide) .


## **Install the package**


Use the package command rather than copying a version that can become stale:


```text
flutter pub add didit_sdk
```


Then import the public library:


```text
import 'package:didit_sdk/sdk_flutter.dart';
```


Before upgrading, read the changelog and[official Flutter SDK documentation](https://docs.didit.me/integration/native-sdks/flutter-sdk) . Check declared platform requirements against your app and CI images.


Follow the Flutter release instructions for native dependencies; mixing arbitrary versions can create incompatibility.


## **Configure iOS and Android**


### **iOS responsibilities**


Identity capture can use protected hardware and data. Depending on the configured workflow and SDK variant, iOS setup can require:


- an appropriate deployment target;
- camera and microphone usage descriptions;
- photo-library usage description if uploads are allowed;
- NFC usage description and entitlements when chip reading is enabled;
- compatible CocoaPods configuration;
- signing capabilities and provisioning that match NFC use;
- registered custom fonts if an app-specific font is configured.


Missing privacy-purpose strings can terminate an iOS app. Test the exact workflow on a physical device.


NFC support can raise the minimum deployment target or add native dependencies. Choose the SDK variant that matches your workflow and follow the current docs for its Podfile configuration.


### **Android responsibilities**


On Android, check:


- minimum SDK and Java requirements;
- repositories and dependencies added by the plugin;
- camera, network, and NFC manifest entries;
- runtime camera-permission behavior;
- Gradle and Kotlin compatibility;
- packaging rules for native or cryptographic dependencies;
- the` all` ,` core` ,` autodetection` , or` nfc` SDK variant;
- release-build minification and resource behavior.


Your product still needs permission context, denial recovery, accessibility, and support instructions. Test denial, interruption, backgrounding, rotation, and process recreation.


## **Create sessions on the backend**


Your backend should call the session API using a server-side API key. Associate each session with:


- your stable customer identifier;
- the selected workflow;
- environment;
- callback or return behavior where applicable;
- required locale or contact details;
- expected customer details where policy uses them;
- internal correlation and policy metadata.


Never embed the Didit API key in Dart, app assets, readable remote configuration, or a mobile request.


Return only the session token and minimum launch state. Keep it out of analytics, crash reports, logs, clipboard use, and long-term storage.


### **Make start requests idempotent**


A customer can tap twice, lose connectivity after your backend creates a session, or reopen the screen while an attempt is active. Use a stable request identifier and backend logic that returns the existing appropriate attempt rather than creating disconnected duplicates.


Your app’s loading button should block obvious repeated taps, but server-side idempotency remains necessary because clients retry and processes restart.


## **Start verification from Dart**


This complete Dart example uses only the SDK import, method, result classes, session fields, status enum, and error fields verified in the package source:


```text
import 'package:didit_sdk/sdk_flutter.dart';


Future<void> runIdentityVerification(String sessionToken) async {
try {
final result = await DiditSdk.startVerification(
sessionToken,
config: const DiditConfig(
loggingEnabled: false,
),
);


switch (result) {
case VerificationCompleted(:final session):
switch (session.status) {
case VerificationStatus.approved:
print('Flow completed with approved client status.');
case VerificationStatus.pending:
print('Flow completed and still needs a backend decision.');
case VerificationStatus.declined:
print('Flow completed with declined client status.');
}
print('Session ID: ${session.sessionId}');
return;
case VerificationCancelled():
print('The user cancelled the verification flow.');
return;
case VerificationFailed(:final error):
print('SDK error: ${error.type.name}: ${error.message}');
return;
}
} catch (error, stackTrace) {
print('Unexpected platform error: $error');
print(stackTrace);
}
}
```


The example shows the type structure. A real app should update screen state and refresh backend status, never unlock an account from this function alone.


### **Why` VerificationCompleted` is not always approval**


` VerificationCompleted` contains` SessionData` , whose` status` is one of:


- ` VerificationStatus.approved` ;
- ` VerificationStatus.pending` ;
- ` VerificationStatus.declined` .


The SDK flow can finish while the verification remains pending or declined. A human review or asynchronous check can also change backend state after the app call returns. Name your local UI state “flow completed” rather than “identity approved” until your backend confirms the policy outcome.


### **There is no Flutter initialize call**


The verified public Flutter surface exposes no separate initialization method. Do not copy an Android-native initialization pattern into Dart. If Android reports` notInitialized` through the Flutter result, treat it as an integration or native-bridge problem and inspect the package setup.


## **Handle typed errors and recovery**


The SDK’s verified error types are:


Error type Meaning for app policy Safe recovery


` sessionExpired` The token can no longer start the intended session Ask backend for a fresh valid session


` networkError` The native flow could not complete a network operation Preserve context and offer a bounded retry


` cameraAccessDenied` Required camera access is unavailable Explain why it is needed and guide settings or alternate route


` notInitialized` Android native integration or bridge is not ready Log release context and investigate setup


` apiError` The SDK or service returned an API-level failure Retry only when safe; reconcile backend state


` retryBlocked` The flow prevents another automatic attempt Stop the loop and follow backend or support policy


` unknown` The native error did not map to a known Dart type Preserve a safe fallback and correlation data


Native platforms can expose different detail. Keep an` unknown` path.


### **Separate errors from customer outcomes**


A network failure is not a decline; camera denial is not fraud; cancellation is not failed identity. Keep categories separate in:


- user messages;
- retry rules;
- product access;
- support tooling;
- analytics;
- fraud and conversion reporting.


### **Bound retries**


Let the backend decide whether an existing session can continue or a new one is required. Avoid an unlimited loop that repeatedly calls the SDK with an expired or blocked token. Track attempt count and cause without logging the token or identity evidence.


## **Use backend events as the source of truth**


The SDK returns a compact client result. Full evidence and final state arrive through the server-side integration. Your webhook handler should:


1. receive the raw request in the form required by the documented signature scheme;
2. authenticate the event and validate freshness;
3. deduplicate its event identifier;
4. map it to the expected session and customer;
5. prevent older events from overwriting later terminal state;
6. retrieve canonical session state when reconciliation is required;
7. apply your policy and persist the reason;
8. return within the provider’s response budget;
9. process slow downstream work asynchronously.


Assume at-least-once delivery. Duplicate and out-of-order events are ordinary distributed-system behavior. Store the provider event and internal transition separately so an audit can reconstruct both.


The app should poll your backend only for its own product state or use your normal realtime channel. It should not expose a provider API key to retrieve the final record directly.


## **Build a resilient Flutter screen lifecycle**


### **Model explicit local states**


A verification screen can use:


- idle;
- requesting session;
- launching SDK;
- SDK flow open;
- reconciling backend decision;
- awaiting review;
- approved;
- declined;
- recoverable error;
- cancelled.


Persist only what is safe. After process death, ask the backend whether an active or finished session already exists. Do not rely on an in-memory boolean to decide whether to create another attempt.


### **Respect widget lifecycle**


After the awaited call, check` mounted` before` setState` , dialogs, or navigation. Keep business state outside transient UI.


### **Handle backgrounding and cancellation**


Test app switching, screen lock, navigation, and process termination. Define resume, restart, and reconciliation behavior.


### **Design permission recovery**


Explain camera or NFC need. After permanent denial, show settings guidance or an accessible alternate route.


## **Configuration without policy leakage**


The offline-verified Flutter` DiditConfig` surface exposes` languageCode` ,` fontFamily` ,` loggingEnabled` ,` showCloseButton` ,` showExitConfirmation` ,` closeOnComplete` ,` defaultDocumentCamera` ,` defaultLivenessCamera` ,` showDocumentCameraSwitchButton` , and` showLivenessCameraSwitchButton` . The camera fields use` CameraLens.front` or` CameraLens.back` ; all options are typed in Dart and mapped to the native SDKs.


Keep three rules:


1. enable verbose logging only for development or a controlled diagnostic build;
2. do not use UI configuration as a substitute for backend policy;
3. test each supported language, custom font, close behavior, and camera policy on both platforms, including fallback behavior when a requested lens or asset is unavailable.


Workflow composition and product branding belong in the console or backend-managed workflow rather than a maze of mobile feature flags. That keeps iOS, Android, web, and support views aligned.


## **Test the integration**


### **Dart and widget tests**


Wrap the SDK launch behind an application service so screen tests can return:


- completed and approved;
- completed and pending;
- completed and declined;
- cancelled;
- each typed failure;
- an unexpected thrown platform exception.


Assert loading-state cleanup, mounted checks, retry visibility, backend refresh, and analytics categories. Do not put real session tokens in fixtures.


### **Native integration tests**


Run debug and release builds on physical iOS and Android devices. Cover:


- first-time and previously decided permissions;
- supported and unsupported cameras;
- NFC-enabled and non-NFC variants where used;
- low light, blur, glare, and orientation;
- slow, lost, and restored connectivity;
- backgrounding and process recreation;
- cancellation and repeated launch;
- session expiry and retry blocking;
- different locales, font scaling, screen readers, and reduced motion;
- app signing, minification, and production dependency resolution.


An emulator is useful for state and error tests but cannot represent every camera, NFC, biometric, and device-integrity condition.


### **End-to-end backend tests**


Use deterministic sandbox cases for each documented customer state. Replay signed test events, send duplicates out of order, delay review, and reconcile after a simulated missed webhook. Confirm that the app never grants access before your backend state changes.


For biometric test design and attack boundaries, see the[liveness testing guide](https://didit.me/blog/liveness-detection-methods-metrics-testing) .


## **Security and privacy checklist**


Before release, confirm that:


- permanent provider credentials exist only on the backend;
- the app receives a scoped session token over an authenticated channel;
- tokens and evidence are absent from logs, analytics, URLs, and crash reports;
- backend create requests are idempotent and tied to a stable customer reference;
- client completion never directly grants an entitlement;
- webhook signature, freshness, duplicate, ordering, and reconciliation tests pass;
- iOS privacy descriptions and Android permission journeys use clear purpose text;
- NFC capabilities and variants match the workflow and release signing;
- debug logging is disabled for production;
- retention, consent, privacy notice, deletion, and support paths match your role and law;
- SDK, native dependency, OS, and device compatibility are monitored after launch;
- rollback and forced-upgrade decisions have owners.


## **Common Flutter SDK integration mistakes**


### **Shipping the API key in Dart**


Mobile applications cannot protect a permanent server credential. Create sessions on your backend and pass a scoped token.


### **Trusting the completed callback**


The client result is user-interface state. Confirm authoritative status and apply policy on the backend.


### **Inventing methods from another platform**


Flutter does not expose every native SDK method under the same name. Compile against the package and check its public Dart source before writing integration code.


### **Copying stale native configuration**


SDK variants, deployment targets, and package-manager setup change. Follow the documentation for the installed release and record it in your mobile release checklist.


### **Treating every error as decline**


Permission, network, expiry, API failure, cancellation, and customer decision require different recovery and analytics.


### **Testing only on an emulator**


Camera, NFC, permissions, signing, and native dependencies require physical-device and release-build coverage.


## **Using Didit in a Flutter identity workflow**


The Didit Flutter SDK is listed as free. It can launch workflows containing[ID Verification](https://didit.me/products/id-verification) ,[Liveness Detection](https://didit.me/products/liveness) , and other configured checks, while teams manage conditional paths through the[Workflow Orchestrator](https://didit.me/products/workflow-orchestrator) .


Published module rates are available on the[pricing page](https://didit.me/pricing) . The SDK handles the native capture experience; your backend remains responsible for session creation, authenticated result handling, customer state, and product decisions.


## **Frequently asked questions**


### **Which method starts verification?**


For a backend-created production session, call` DiditSdk.startVerification(sessionToken)` . The SDK also exposes` DiditSdk.startVerificationWithWorkflow(...)` for the simpler workflow-ID integration mode.


### **Should the Flutter app contain the Didit API key?**


No. Keep the API key on the backend. The app should receive only the scoped session token required for its verification attempt.


### **Does` VerificationCompleted` mean approved?**


Not necessarily. Its session status can be approved, pending, or declined. Use the result for immediate interface state and confirm the authoritative decision through your backend.


### **How should cancellation be handled?**


Treat it as a distinct user outcome. Preserve backend session state, offer a clear resume or restart path according to policy, and do not label cancellation as fraud or decline.


### **Does the Flutter SDK have an initialize method?**


The verified public Dart API exposes no separate initialization method. Follow the package’s native setup instructions and use the documented start methods.


### **Can Flutter use NFC for identity documents?**


The native SDK can support NFC when the selected package variant, device, iOS or Android configuration, signing capabilities, and workflow all enable it. Follow the current release documentation and test on physical devices.


### **What should the app do while a case is in review?**


Show a truthful pending state, allow the customer to leave safely, and read the final product state from your backend when the authenticated result arrives.


## **Primary references**


- [Didit Flutter SDK documentation](https://docs.didit.me/integration/native-sdks/flutter-sdk)
- [Didit Flutter SDK source repository](https://github.com/didit-protocol/sdk-flutter)
- [Didit session API documentation](https://docs.didit.me/sessions-api/create-session)
- [Didit webhook documentation](https://docs.didit.me/integration/webhooks)
- [Flutter documentation: platform integration](https://docs.flutter.dev/platform-integration)
- [OWASP Mobile Application Security Verification Standard](https://mas.owasp.org/MASVS/)


A strong Flutter SDK integration keeps each boundary explicit: the backend creates the attempt, the app launches a scoped native flow, typed results drive recovery, authenticated server events drive customer state, and real-device tests prove that permissions, lifecycle, native dependencies, and failure paths work outside the happy-path demo.


Keep reading


## Related articles


- [PEP Screening: Definitions, Scope, and Monitoring](https://didit.me/blog/pep-screening-definitions-scope-monitoring/)
- [Enhanced Due Diligence (EDD): Compliance Guide](https://didit.me/blog/enhanced-due-diligence-edd-compliance-guide/)
- [MRZ Explained: Machine Readable Zone Technical Guide](https://didit.me/blog/mrz-machine-readable-zone-technical-guide/)
- [Age Verification Software: Buyer's Guide](https://didit.me/blog/age-verification-software-buyers-guide/)
- [W3C Decentralized Identifiers (DIDs) Specification](https://didit.me/blog/w3c-decentralized-identifiers-dids-specification/)
- [Adverse Media Screening: Process, Tuning, and Risks](https://didit.me/blog/adverse-media-screening-process-tuning-guide/)
