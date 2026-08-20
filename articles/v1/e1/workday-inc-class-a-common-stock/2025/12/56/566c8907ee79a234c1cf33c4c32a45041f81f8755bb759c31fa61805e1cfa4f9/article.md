---
schema_version: "1.0.0"
document_id: "566c8907ee79a234c1cf33c4c32a45041f81f8755bb759c31fa61805e1cfa4f9"
company_key: "workday-inc-class-a-common-stock"
company: "Workday Inc."
source_id: "workday-inc-class-a-common-stock-rss-1edd291cea4c"
canonical_url: "https://medium.com/workday-engineering/adopting-kotlin-multiplatform-a-practical-guide-to-unifying-codebases-1b333436061e"
published_at: "2025-12-18T20:32:48+00:00"
first_seen_at: "2026-07-20T04:35:52.231186+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:ef9b8eb5aa632c5306ad7fb5f7b7f74c7d703041cb43dbcf46d63d0d49e6a70e"
---

# Adopting Kotlin Multiplatform: A Practical Guide to Unifying Codebases

Android


IOS


Kotlin Multiplatform


# **Adopting Kotlin Multiplatform: A Practical Guide to Unifying Codebases**


[Ahmed Mohamed](https://medium.com/@ahmedadeltito?source=post_page---byline--1b333436061e---------------------------------------)


9 min read


·


Dec 18, 2025


--


*What if you could build features once and run them natively on both Android and iOS, without the compromises of traditional cross-platform frameworks? At Workday, we have begun doing exactly that. In this article, we explore the workings of our transition to Kotlin Multiplatform (KMP), detailing how we architected our shared mobile system and managed complex repositories to boost efficiency while maintaining the high-quality native experiences our customers expect.*


By[Ahmed Mohamed](https://medium.com/@ahmedadeltito) with contributions by Brendan Innis


## Introduction


For years, mobile development at **Workday** was a tale of two platforms. We often found ourselves duplicating effort, time, and resources to build the same features for both Android and iOS. The dream was always a single, unified codebase, but early cross-platform solutions often came with compromises in performance, user experience, and access to native features.


Kotlin Multiplatform (KMP) entered this landscape not as another framework, but as a flexible technology that empowered us to share code where it makes sense, without sacrificing the quality and the performance of the native experience our users expect.


In this article, we share **Workday’s journey** with KMP. We’ll cover the core benefits we’ve observed, the scalable project structure we adopted, our implementation strategies, and the best practices we learned through our journey.


## When to Use Kotlin Multiplatform


Press enter or click to view image in full size


source:[https://www.jetbrains.com/help/kotlin-multiplatform-dev/multiplatform-reasons-to-try.html#7-with-kotlin-multiplatform-you-can-start-sharing-your-code-gradually](https://www.jetbrains.com/help/kotlin-multiplatform-dev/multiplatform-reasons-to-try.html#7-with-kotlin-multiplatform-you-can-start-sharing-your-code-gradually)


At Workday, we appreciate that Kotlin Multiplatform doesn’t demand an all-or-nothing commitment. Its greatest strength is its flexibility, allowing us to adopt it incrementally. We started small, looking for parts of our application where sharing logic provided the most value without disrupting our existing flows.


We found KMP ideal for code that needs to be identical and reliable across platforms. This includes foundational elements like **data models** , **networking calls** , **analytics tracking** , and **the complex business rules** that define how the Workday app functions. By sharing this core logic, we reduced the amount of code we had to write and eliminated a major source of platform, ensuring a more unified experience for our customers.


## Handling Platform-Specific Code


While we aim to maximize code sharing, some functionalities — like accessing device hardware — require platform-specific APIs. KMP provides patterns for this, we carefully choose between the **expect/actual** mechanism and **Dependency Inversion** .


### Approach 1: The expect/actual Mechanism


The pattern is a unique KMP feature that is great for defining platform-specific implementations within the shared module itself. It allows you to declare an API “contract” in your common code and requires each platform-specific source set (e.g., *androidMain* , *iosMain* ) to provide a concrete implementation. The flow is straightforward:


1. You first declare an *expect* function in *commonMain* . This declaration acts as a contract, defining an API without providing an implementation.
2. Then, in *androidMain* and *iosMain* , you provide the *actual* platform-specific implementations. The Kotlin compiler ensures that every *expect* declaration has a corresponding *actual* implementation for each target platform, guaranteeing that your code will compile and run correctly.


**Example *expect* declaration in *commonMain* :**


```text
// in commonMain/kotlin/com/workday/shared/Platform.kt  package com.workday.shared   expect fun getDeviceModel(): String
```


**Example *actual* implementation in *androidMain* :**


```text
// in androidMain/kotlin/com/workday/shared/Platform.kt  package com.workday.shared  import android.os.Build   actual fun getDeviceModel(): String =       "Android ${Build.VERSION.RELEASE} (${Build.MODEL})"
```


**Example *actual* implementation in *iosMain* :**


```text
// in iosMain/kotlin/com/workday/shared/Platform.kt  package com.workday.shared  import platform.UIKit.UIDevice   actual fun getDeviceModel(): String =       "iOS ${UIDevice.currentDevice.systemName()} ${UIDevice.currentDevice.systemVersion}"
```


This approach allows your shared *commonMain* code to call *getDeviceModel()* without having any direct knowledge of the underlying platform, keeping your core logic clean and platform-agnostic.


### Approach 2: Dependency Inversion with Interfaces


For larger features or when integrating with our existing native codebases, we prefer **Dependency Inversion** . Instead of coupling *commonMain* to platform specifics via *expect/actual* , we define an interface in *commonMain* and inject the platform-specific implementation from the native app. This follows the **Dependency Inversion Principle** , allowing your KMP logic to depend on an abstraction, not on concrete platform details. The flow is straightforward:


1. **Define an Interface in *commonMain*** : The contract our shared logic uses.
2. **Implement the Interface in Native Code** : Our native Android and iOS apps provide concrete implementations.
3. **Inject the Implementation** : When initializing the KMP module, we pass the implementation.


**Example Interface in *commonMain* :**


```text
// in commonMain/kotlin/com/workday/shared/DeviceInfoProvider.kt  package com.workday.shared   interface DeviceInfoProvider {      fun getDeviceModelInfo(): String  }   // Example of a ViewModel using the interface  class SharedViewModel(      private val deviceInfoProvider: DeviceInfoProvider  ) {      fun getDeviceGreeting(): String =          "Hello from your ${deviceInfoProvider.getDeviceModelInfo()}!"  }
```


**Example Implementation in the Android App (e.g., *androidApp/src/…* ):**


```text
// In your Android app's DI module or Activity  import com.workday.shared.DeviceInfoProvider  import android.os.Build   class AndroidDeviceInfoProvider : DeviceInfoProvider {      override fun getDeviceModelInfo(): String =          "Android ${Build.VERSION.RELEASE}"  }   // When creating the ViewModel  val deviceInfoProvider = AndroidDeviceInfoProvider()  val viewModel = SharedViewModel(deviceInfoProvider)  val greeting = viewModel.getDeviceGreeting() // "Hello from your Android 14"
```


**Example Implementation in the iOS App (e.g., *iosApp/src/…* ):**


```text
// In your Swift code (e.g., in a helper class)  import shared // Assuming 'shared' is the name of your KMP module   class IOSDeviceInfoProvider: DeviceInfoProvider {      func getDeviceModelInfo() -> String {          return "\(UIDevice.current.systemName) \(UIDevice.current.systemVersion)"      }  }   // When creating the ViewModel  let deviceInfoProvider = IOSDeviceInfoProvider()  let viewModel = SharedViewModel(deviceInfoProvider: deviceInfoProvider)  let greeting = viewModel.getDeviceGreeting() // "Hello from your iOS 17.0"
```


This approach allows our shared code to remain completely ignorant of the platform it’s running on, making it easier to test and integrate into our mature native applications.


## Choosing a Distribution and Delivery Strategy


Once you have a shared module, a critical decision is how to deliver it to your native applications. This choice fundamentally shapes your team’s workflow and release cycles. There are several popular strategies, each with distinct advantages.


### Strategy 1: Distributing as a Versioned Library


For iOS, we utilize the **Umbrella Framework** pattern. We bundle our shared modules into a single XCFramework. This simplifies consumption for our iOS app, allowing it to treat the KMP code as just another third-party dependency.


**Advantages:**


- **Decoupled Release Cycles:** The shared code team can release new versions on their own schedule, and native app teams can upgrade when ready.
- **Stability and Clarity:** Native teams consume a stable, versioned artifact, and dependency management is explicit.


**Disadvantages:**


- **Slower Iteration:** Making a change that spans from the shared code to the native UI requires two separate PRs and a new library release, which can create friction.


### Strategy 2: Building from Source (Git Submodules/Subtrees)


For local development and often for Android, we support building from source. This allows developers to make changes in the shared code and see them reflected immediately in the sample apps, enabling a rapid feedback loop.


**Advantages:**


- **Unified Workflow:** Developers can make changes to both the platform-specific code and the shared KMP code in a single pull request. This is a significant advantage for iterating on features quickly.
- **No Publishing Overhead:** Eliminates the need to publish and consume versioned artifacts for internal development.


**Disadvantages:**


- **Tightly Coupled:** The shared code and native app versions are locked together, which can be less flexible than a decoupled library approach.


The best approach depends on your team’s size, structure, and existing workflows. While versioned libraries offer stability, many teams find that building from source provides a more agile and developer-friendly workflow for rapid feature development.


## A Layered Approach to Testing


Our testing strategy at Workday treats KMP modules as libraries, meaning the consumer applications are ultimately responsible for integration and end-to-end testing. This creates a **clear separation of concerns** and ensures that testing is focused and effective. This can be visualized as a testing pyramid:


1. **Unit Tests (Base of the Pyramid):** We write unit tests in *commonMain* that run on the JVM. This ensures our core business logic is correct.
2. **Integration Tests (Middle Layer):** We write integration tests on each native platform (Android and iOS). These tests verify the integration points with the KMP module, ensuring that the *actual* implementations behave as expected.
3. **End-to-End Automation Tests (Top of the Pyramid):** Finally, after a new version of a KMP library is integrated into the apps, we run a full suite of E2E automation tests. This validates the entire user flow and confirms that the shared logic works correctly within the context of the full application.


## Real-World Example: Workday’s Shared Architecture


Currently, we are implementing these principles as part of our **Shared Mobile Architecture** . Our objective is to centralize business logic and state management across Android and iOS, creating a unified core that integrates seamlessly and safely with our existing mature native frameworks (referred to as “Legacy Frameworks”)


Press enter or click to view image in full size


The sequence diagram above maps out how a WidgetEdit moves through the Workday Shared Architecture during its lifecycle


### Architecture Overview


Our architecture follows **Clean Architecture** principles combined with **Unidirectional Data Flow** . We utilize the **Port/Adapter pattern** to interface with our legacy code, allowing us to adopt KMP without a complete rewrite.


The architecture is divided into distinct layers:


**Native Presentation Layer (Platform-specific):**


- **Native UI** : Implemented in SwiftUI for iOS and Jetpack Compose for Android.
- **Native ViewModels** Handle platform-specific UI concerns and bridge the gap to the shared KMP code.


**KMP Presentation Layer (Shared):**


- **SharedWidgetViewModel** : A generic *ViewModel* that resides in the shared module. It publishes domain actions and observes domain models.
- **DomainModelToUiStateMapper** Converts internal domain models into platform-agnostic UI state.


**Domain Layer (Shared Core Business Logic):**


- **DomainProcessor** : The central hub that manages state and business logic.
- **DomainAction** : Represents user intents.
- **DomainModel** : Pure Kotlin data classes representing the business entities.


**Data Layer (Shared):**


- **DataStoreHandler** : Processes updates and manages data flow.
- **PlatformDataStore (Port)** : An interface defining the contract for data operations that need to interact with the platform.


**Platform Layer (Native Implementations):**


- **AndroidDataStore / iOSDataStore (Adapters)** : These implement the *PlatformDataStore* interface. They act as *adapters* , calling into our existing “Legacy Frameworks” to fetch or update data, ensuring our shared code remains isolated from legacy constraints.


### Unidirectional Data Flow in Action


We enforce a strict one-way flow of data to ensure predictability:


- **Actions flow DOWN** :
Native UI → Native ViewModel → Shared ViewModel → Domain → Data → Platform.
- **State flows UP** :
Platform → Data → Domain → Shared ViewModel → Native ViewModel → Native UI.


This ensures that the “Domain” layer remains the single source of truth.


### **Repository Structure at Workday**


To manage our KMP code effectively alongside our massive existing native applications, we use a **three-repository structure** connected via **git subtree** .


Press enter or click to view image in full size


**KMP Repository (Central)** : This is the home of our shared code. It contains:


- **Feature Modules** : Individual KMP modules for each feature.
- **Umbrella Module** : A special module that aggregates all feature modules into a single framework for iOS consumption.


**Android Repository** : This is our main Android monorepo. It pulls in the KMP repository as a **git subtree** .


- Our Android build gradle files can depend directly on the specific KMP feature modules they need, treating them just like any other module in the project.


**iOS Repository** : This is our main iOS monorepo. It also pulls in the KMP repository as a **git subtree** .


- It utilizes a **KMP Xcode Project** which wraps the **Umbrella module** . This allows the iOS app to consume all shared functionality as a single unified framework, simplifying the integration and avoiding complex dependency graphs on the iOS side.


Using *git subtree* allows us to build from source within our native apps. We can make changes to both the platform-specific code and the shared KMP code in a single pull request, without the friction of publishing versioned artifacts for every change during development.


## Conclusion


Adopting Kotlin Multiplatform is more than just a technical decision; it’s a strategic move that fundamentally improves how teams build and maintain software. As we’ve seen, KMP provides significant advantages, from the development efficiency of a single, shared logic layer to the high-quality, native performance it enables. It offers a flexible and pragmatic approach to platform-specific code with patterns like **expect/actual** and **dependency inversion** , and it can be integrated into your team’s existing workflow, whether through **versioned libraries** or by **building from source** .


If you’re considering KMP, our advice is to start today, but start small. The journey to a unified codebase doesn’t require an immediate ‘big bang’ rewrite; the platform’s true strength lies in its modularity. Begin with a single new feature or a small proof-of-concept, allowing your team to iterate, learn, and witness the benefits of shared logic firsthand.


In our experience, it’s a practical, achievable, and highly rewarding path to a more efficient and unified development future.
