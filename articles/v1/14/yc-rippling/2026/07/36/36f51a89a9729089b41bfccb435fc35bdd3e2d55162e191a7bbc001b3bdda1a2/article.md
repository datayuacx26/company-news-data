---
schema_version: "1.0.0"
document_id: "36f51a89a9729089b41bfccb435fc35bdd3e2d55162e191a7bbc001b3bdda1a2"
company_key: "yc-rippling"
company: "Rippling"
source_id: "yc-rippling-news-import-758faf74b02a"
canonical_url: "https://www.rippling.com/blog/react-native-upgrades"
published_at: null
first_seen_at: "2026-07-22T11:59:36.706542+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:e123b6bd3a06b2bd0921868b0bc910b8b880ba690c355a7c8fc12cd810454ed3"
---

# Managing and scoping React Native upgrades

At Rippling, our journey with[React Native](https://reactnative.dev/) has been a key part of building mobile applications that are fast, reliable, and feature-rich. With more than five years of experience using React Native, we've harnessed its ability to share code across platforms and enable collaboration between web and mobile engineers, allowing us to deliver seamless product integrations efficiently.


The React Native community moves quickly, with frequent updates and releases. Staying up-to-date with these rapid changes is essential for maintaining the security, performance, and compatibility of our apps—[Rippling Mobile](https://apps.apple.com/us/app/rippling-hr-it-finance/id1231325957) and[Rippling Kiosk](https://play.google.com/store/apps/details?id=com.people.timeclock) . Initially, we upgraded every year, but we've since transitioned to semi-annual major upgrades, reflecting a more structured approach to managing the fast-paced evolution of React Native. While these upgrades can be challenging, they offer substantial benefits, such as access to new APIs, improved developer tools, and optimized performance.


In this blog, we’ll take you behind the scenes at Rippling, sharing our strategies for scoping and managing React Native upgrades. From allocating dedicated resources to overcoming specific challenges, we’ll provide insights into how we balance the disruption of an upgrade with its long-term advantages, making sure our apps continue to deliver exceptional experiences for employees.


## Rippling’s mobile apps: A brief overview


Rippling has two mobile applications, each serving distinct purposes for different user groups:


**App name**


****Audience & Purpose****


****Features****


****Store Availability****


**Rippling Mobile App**


**Employees** - Offers a unified platform for employees to manage various HR, Finance, and IT-related tasks.


- **Time and Attendance** : Track work hours and attendance patterns


- **Payroll** : View salary details and manage pay-related information


- **Spend Management** : Expense reimbursements


- **Time Off** : Apply for and manage leaves


- **Benefits** : Explore and manage employee benefits


Google:[Play Store](https://play.google.com/store/apps/details?id=com.people.rippling)


Apple:[App Store](https://apps.apple.com/us/app/rippling-hr-it-finance/id1231325957)


**Rippling Kiosk App**


**Hourly employees of companies** - A dedicated app for clocking in and out of shifts, ensuring streamlined time tracking for hourly workers.


- **Clock in/out** Pin Based


- **Manage shifts**


Google:[Play Store](https://play.google.com/store/apps/details?id=com.people.timeclock)


Apple:[App Store](https://apps.apple.com/us/app/rippling-time-kiosk/id1540084564)


Both apps undergo **200+ releases annually** and integrate with **more than 10 products** , showcasing the scale and complexity of the systems we manage. With a mix of 53% focused on bug fixes, performance, and stability improvements, and 47% introducing new features, these frequent releases are key to maintaining app reliability and driving innovation. Over the years, Rippling has consistently embraced updates in React Native to stay aligned with the evolving ecosystem. These frequent updates and integrations highlight the dynamic nature of our mobile platform and our commitment to continuously improving the user experience, ensuring both apps remain performant, reliable, and up to date with modern development standards.


## Why Rippling upgrades React Native


These upgrades are essential for ensuring our apps remain secure, high-performing, and aligned with the latest technology standards. Here are the key reasons why we prioritize regular upgrades, along with a look at how they’ve directly benefited our engineering efforts:


**1. Access to new features:** Upgrading unlocks new features and performance optimizations that directly enhance our apps. For example, during our upgrade to React Native 0.70, we saw significant improvements with the introduction of[Hermes](https://reactnative.dev/docs/hermes) as the[default JavaScript engine](https://reactnative.dev/blog/2022/07/08/hermes-as-the-default) . This change reduced our JavaScript bundle load time by more than 50%, improving app launch speeds and delivering a noticeably smoother user experience.


**2. Security improvements:** With each upgrade, we ensure that our apps are protected against vulnerabilities through critical security patches. This proactive approach helps safeguard sensitive user data and meets compliance standards expected by businesses using Rippling.


**3. Bug fixes and stability enhancements:** New versions address existing bugs and stability issues, minimizing downtime and making troubleshooting easier. For example, during the upgrade from 0.72 to 0.73, we benefited from[fixes to previously reported crashes](https://github.com/facebook/react-native/blob/main/CHANGELOG.md#fixed-42) in edge-case scenarios, leading to a more stable application.


**4. Compatibility with third-party libraries:** Upgrades help us maintain compatibility with the third-party libraries we rely on. As we upgraded to React Native 0.72, the improved support for TypeScript allowed us to better integrate type safety into our codebase, reducing runtime errors and making our development process more robust.


**5. Support for new React, iOS, and Android versions:** Each major upgrade aligns our apps with updated React versions, the latest Android and iOS operating systems, and essential development tools like Android Studio and Xcode. For example, upgrading to React Native 0.72 ensured our apps were optimized for the latest macOS and Xcode upgrades, making it easier for engineers to maintain and enhance the product.


**6. Fulfilling policy requirements:** The mobile app ecosystem is constantly evolving, with Apple and Google frequently updating their policies. Regular upgrades help us meet these requirements seamlessly. For instance, our recent upgrades ensured that our apps complied with the latest iOS App Store and Google Play Store submission guidelines. Failure to keep up with these updates could lead to serious consequences, including app rejection, removal from the stores, revenue loss, and damage to user trust. By staying compliant, we not only avoid these risks but also ensure the continued availability of our app, maintaining both our user base and brand reputation.


**7. Future-proofing through end-of-life awareness:** Staying informed about[React Native’s End-of-Life (EOL) policies](https://endoflife.date/react-native) is vital. Running an unsupported version risks exposing apps to security vulnerabilities and integration issues with third-party libraries. Regular upgrades, such as our recent transition from 0.70 to 0.73, ensure that our apps remain compliant, secure, and easy to maintain, while also benefiting from community support and updated resources.


## Understanding the upgrade process


Upgrading React Native isn’t just about updating a library—it’s a comprehensive process that touches multiple layers of your application. At Rippling, we recently upgraded from[React Native 0.72 to 0.73](https://reactnative.dev/blog/2023/12/06/0.73-debugging-improvements-stable-symlinks) , which provides a practical example of the considerations and adjustments involved.


**1. JavaScript code:** One of the first steps in the upgrade was reviewing our JavaScript codebase. The new version introduced changes to APIs, requiring us to replace deprecated methods and adjust functionality. For instance, we refactored parts of our code to align with stricter type-checking enforced by the updated TypeScript version, ensuring smoother runtime behavior and reducing errors.


**2. Native code:** This upgrade required updates to our native Android and iOS codebases. With the new version of React Native, support for the latest iOS and Android operating systems was introduced. We adjusted files like build.gradle for Android and Podfile for iOS to ensure compatibility, enabling our apps to leverage platform-specific enhancements and policies.


**3. Third-Party dependencies:** Several third-party libraries we rely on, such as **React Navigation and Detox** needed updates to stay compatible with React Native 0.73. During the process, we resolved conflicts arising from outdated dependencies and ensured that their latest versions worked seamlessly within our app ecosystem.


**4. Configuration files:** Key configuration files like AndroidManifest.xml and Info.plist required minor changes to meet the latest requirements. For instance, we updated permissions and configurations to comply with the latest Android and iOS store submission guidelines, ensuring our apps met policy standards.


**5. CI/CD pipelines:** Our CI/CD pipelines also underwent modifications. The upgrade introduced updated build requirements, prompting us to adjust our workflows in Bitrise and GitHub Actions. This ensured that builds were efficient and aligned with the new version’s dependencies and testing frameworks.


**6. Static analysis tools:** As part of our commitment to maintaining high code quality, we upgraded static analysis tools like **TypeScript** , **ESLint** , and **Prettier** . These upgrades enabled us to use new rules and improved error detection, particularly beneficial for catching issues during the early stages of development.


**7. Other development tools:** We also updated our debugging tools and IDEs, such as **Android Studio** and **Xcode** , to their latest versions. These updates offered better support for React Native 0.73, improved performance during development, and provided access to new debugging features that simplified troubleshooting.


### Key lessons learned from the past upgrades:


-


**Plan for dependencies:** Upgrading third-party libraries alongside React Native is critical to avoid compatibility issues.


-


**Test incrementally:** Running tests throughout the process helped us catch bugs early and minimized disruption to ongoing development.


-


**Optimize CI/CD:** Updating CI/CD pipelines early in the process ensured seamless integration and deployment once the upgrade was complete.


-


**Communicate clearly:** Having one engineer dedicated to the upgrade allowed us to minimize disruption to feature development while maintaining clear communication with the broader team.


By following a structured approach, we ensured that the transition was smooth, enabling Rippling Mobile and Rippling Kiosk to take full advantage of React Native 0.73’s new features and improvements. This upgrade was not just a technical necessity—it was an investment in the future scalability and performance of our apps.


## Adopting a gradual approach


At Rippling, we've found that adopting a gradual approach is one of the most effective strategies for managing React Native upgrades. Instead of tackling the entire application at once, we break the process into smaller, manageable steps. This incremental method allows us to:


-


**Mitigate risks:** Address potential issues in a controlled manner by upgrading specific features or components step by step


-


**Maintain functionality:** Ensure that the application remains operational throughout the upgrade process, minimizing disruption to end users


-


**Facilitate testing and validation:** Focus on smaller, isolated changes that are easier to test and validate, ensuring stability and compatibility


-


**Integrate enhancements seamlessly:** Gradual upgrades make it easier to incorporate and leverage new React Native features and improvements without overwhelming the development process


This approach has proven invaluable in managing complexity and delivering a more resilient application. By prioritizing stability and user experience, a phased strategy helps us maximize the benefits of upgrading while minimizing disruptions.


## Scoping the Upgrade


Before beginning the React Native upgrade process, it’s critical to scope the changes comprehensively. This step ensures that the transition is well-planned and minimizes disruptions to your application. Here’s how we approach scoping at Rippling:


### **1. Reviewing the changelog**


-


**Action:** Analyze the[official React Native changelog](https://reactnative.dev/versions) for all versions included in the upgrade path.


-


**Purpose:** Gain a clear understanding of changes, deprecated APIs, and new features that might impact your app.


-


**Benefit:** This helps anticipate the necessary modifications and plan the upgrade steps effectively.


**2. Consulting the React Native blog**


-


**Action** : Explore the[React Native blog](https://reactnative.dev/blog/) to stay updated with announcements, insights, and detailed upgrade guides from the core team.


-


**Purpose** : Gain a deeper understanding of major updates, migration strategies, and the latest features to better align your upgrade process with React Native's best practices.


-


**Benefit** : Leverage expert guidance to streamline the upgrade, minimize risks, and effectively utilize new enhancements in your application.


### **3. Using upgrade helper tools**


-


**Tools:** We leverage the[React Native Upgrade Helper](https://react-native-community.github.io/upgrade-helper/) and[Expo Native Project Upgrade Helper](https://docs.expo.dev/bare/upgrade/) to streamline our upgrade process.


-


**Functionality:** These tools highlight the differences between the current and target versions, simplifying the identification of required changes. Using the Upgrade Helper, we can quickly pinpoint necessary code adjustments and configurations for JavaScript and Native code.


-


**Expo CNG (Continuous Native Generation) adoption** : To further simplify and accelerate our upgrade process, we’re planning to adopt[Expo CNG](https://expo.dev/blog/what-is-continuous-native-generation) (Continuous Native Generation) soon. Expo CNG introduces an innovative approach by providing a continuous integration system that handles the generation of native code. This helps eliminate manual processes traditionally involved with managing native code changes, reducing friction and increasing the speed of upgrades. By leveraging Expo CNG, we expect even smoother native updates, quicker iterations, and better alignment with modern development practices.


Adopting Expo CNG is a strategic step forward, not only improving our internal development workflows but also contributing to the Expo ecosystem. By relying on Expo’s powerful infrastructure, we can more easily manage native dependencies, reduce the risk of incompatibilities, and keep both of our mobile apps performing optimally with minimal overhead.


### **4. Updating dependencies**


-


**Process:** Audit all third-party libraries and confirm compatibility with the target React Native version.


-


**Tools:** Use npm outdated or yarn outdated to identify outdated packages.


-


**Action:** Check each dependency’s repository for migration guides and updates.


### **5. Evaluating custom code**


-


**Scope:** Review any custom code or patches applied to the React Native framework.


-


**Outcome:** Ensure customizations align with the updated framework and plan necessary adjustments.


### **6. Reviewing security implications**


-


**Focus:** Examine security updates or requirements in the target version.


-


**Action:** Verify compliance with new security practices to safeguard user data.


### **7. Mapping features and impact**


-


**Approach:** Create a detailed list of app features and identify those impacted by the upgrade.


-


**Outcome:** This enables prioritization and planning for potential adjustments or testing needs.


### **8. Prioritizing upgrades**


-


**Strategy:** Tackle critical features and high-impact areas first, leaving less essential components for later phases.


-


**Result:** This ensures that vital functionality remains intact throughout the upgrade process.


### **9. Assessing performance metrics**


-


**Baseline:** Gather pre-upgrade performance data to compare with post-upgrade metrics.


-


**Objective:** Identify and address any regressions introduced during the update.


### **10. Establishing rollback procedures**


-


**Plan:** Document a rollback strategy to revert to the previous version if significant issues arise.


-


**Impact:** This reduces downtime and ensures a quick recovery if unexpected problems occur.


### **11. Team communication**


-


**Engagement:** Involve the team in planning discussions to gather input on potential challenges.


-


**Alignment:** Make sure everyone understands the upgrade strategy and their responsibilities.


By following this structured approach to scoping, we ensure a smoother upgrade process that minimizes risks, reduces surprises, and prepares the team to tackle challenges effectively. This proactive planning lays the foundation for a successful React Native upgrade while maintaining stability and performance.


## Compatibility with third-party libraries: A major risk


One of the most significant risks we encountered was third-party library compatibility. Many of our dependencies were not immediately compatible with the latest React Native version, which introduced delays and additional troubleshooting steps. To manage this risk:


-


**Early compatibility assessment:** We reviewed each dependency and checked for compatibility information or recent updates on their repositories.


-


**Temporary forks and custom patches:** For critical libraries lacking official updates, we created temporary forks and modified the code ourselves. This helped us keep the upgrade on track while waiting for library maintainers to release official patches.


-


**Exploring alternatives:** When certain libraries seemed unlikely to receive updates soon, we researched alternative solutions that were compatible with the new React Native version.


Dependency compatibility is a crucial risk factor during upgrades. Approaching it with a proactive strategy minimized disruptions, allowing us to maintain the app’s functionality throughout the process.


## Effort and team Involvement in RN upgrades


At Rippling, the RN upgrade process typically requires **six to eight engineer weeks** , depending on the scope and complexity of the upgrade. The effort is primarily led by **a single dedicated engineer** who drives the upgrade process while collaborating with other team members as needed. This ensures minimal disruption to ongoing feature development, as most of the team continues to focus on their regular work. Cross-functional input is occasionally required to address issues in specific product modules and to verify that the upgrade does not introduce regressions.


## Automating the upgrade process


At Rippling, we prioritize efficiency in our upgrade workflows by automating key steps of the React Native upgrade process. Leveraging tools and scripts helps us streamline the process, reduce manual effort, and minimize errors.


Some of the tools we use or recommend include:


-


[React Native Upgrade Helper](https://react-native-community.github.io/upgrade-helper/) : A community-driven tool that provides a detailed, side-by-side comparison of changes required for upgrading to a specific version of React Native. This helps ensure we don’t miss critical updates in configuration files or dependencies.


-


[Expo Native Project Upgrade Helper](https://docs.expo.dev/bare/upgrade/) : For projects using Expo, this tool offers a similar experience by highlighting the necessary adjustments to seamlessly transition to newer versions.


-


**Custom scripts:** We have developed in-house scripts to automate repetitive tasks, such as dependency checks and static analysis tool upgrades, ensuring a consistent approach to upgrades.


By automating much of the upgrade process, we save time, reduce the risk of human error, and ensure a smoother transition to the latest version. This approach also frees up our engineering resources to focus on delivering new features and improving user experience.


## Maintaining backward compatibility


At Rippling, ensuring backward compatibility during React Native upgrades is a critical part of our process. We aim to provide a seamless experience for our users, avoiding disruptions or issues when transitioning to the upgraded version. Here’s how we approach this:


**1. Reviewing breaking changes:**


-


We meticulously analyze the[React Native release notes](https://github.com/facebook/react-native/releases) to identify potential breaking changes.


-


Any APIs, modules, or configurations impacted by the upgrade are flagged for review and adjustment.


**2. Feature flags:**


-


We implement feature flags to isolate new features or changes. This allows us to enable or disable specific functionalities dynamically, minimizing user impact if issues arise.


**3. Testing strategy:**


-


**Automated testing:** Comprehensive test suites ensure that existing functionalities are validated against the updated React Native version.


-


**Manual testing:** Edge cases and user-specific scenarios are manually tested to catch any discrepancies not covered by automation.


By prioritizing backward compatibility, we uphold our commitment to delivering a reliable and user-friendly experience while embracing the improvements offered by new React Native versions.


## Debugging


Debugging during the upgrade process is a crucial phase that requires a meticulous and systematic approach. As you implement updates, you may encounter unexpected behaviors or errors that disrupt functionality. To efficiently address these issues, use a combination of tools that offer powerful debugging capabilities and insights into your app's performance.


### Debugging tools


-


**Flipper** (GitHub:[Flipper](https://github.com/facebook/flipper) ): All-in-One Tool (Deprecated). Previously used for debugging at Rippling, but we have since transitioned to other tools listed below


-


**Network inspection** : **Proxyman** (Website:[Proxyman](https://proxyman.io/) ). Used to inspect and analyze network requests and responses, ensuring the app communicates correctly with external services


-


**UI inspection** : **React DevTools UI Inspector** (Documentation:[React DevTools UI Inspector](https://reactnative.dev/docs/0.73/react-devtools) ). Helps analyze the app's UI hierarchy and components


-


**JavaScript debugging** : **Chrome Debugger** for efficient JavaScript debugging, helping identify issues in the logic and execution flow


-


**Performance monitoring** : **Performance Monitor** (Documentation:[React Native Performance Monitor](https://reactnative.dev/docs/0.73/debugging#performance-monitor) ). Used to track and address performance bottlenecks in the app


### Clearing caches


Sometimes, issues arise from outdated cache files during the upgrade process. Clearing various caches can help resolve these problems and ensure consistency:


-


**Metro Bundler Cache** : Run npx react-native start --reset-cache to reset the Metro cache.


-


**npm Cache** : Use npm cache clean --force to clear the npm cache.


-


**Yarn Cache** : Clear the Yarn cache by running yarn cache clean.


-


**Android Build Cache** : For Android, run cd android && ./gradlew clean to clear the build cache.


-


**iOS Derived Data** : Navigate to ~/Library/Developer/Xcode/DerivedData and delete the contents to clear Xcode's derived data.


### Community resources


In addition to the tools mentioned, consulting community resources such as forums and GitHub issues (e.g., React Native and Expo repositories) can provide valuable insights into common upgrade-related pitfalls. By systematically debugging and clearing caches, you can address issues more efficiently, ensuring a smoother transition to the new version of React Native and enhancing your app’s stability and performance.


## Testing and validation


Thorough testing and validation play a critical role in ensuring the success of your React Native upgrade. A comprehensive testing strategy should include both automated and manual testing to ensure stability and a seamless user experience.


### Automated testing


To cover all critical functionality, we utilize various tools in Rippling testing suite:


-


**Static checks** : We use tools like **ESLint** for linting, **unimported** to detect unused code, **TSC** for type errors, and checks for cyclic dependencies to maintain code quality.


-


**Screenshot testing** : We leverage **react-native-owl** (GitHub:[react-native-owl](https://github.com/FormidableLabs/react-native-owl) ) for automated screenshot testing, ensuring that the UI remains consistent across updates.


-


**Unit testing** : For isolated function and logic testing, **Jest** is our go-to framework. It helps us verify that individual components and functions are working as expected.


-


**End-to-end testing** : **Detox** (GitHub:[Detox](https://github.com/wix/Detox) ) is used for runtime end-to-end tests, ensuring the entire flow of the app works seamlessly in real-world scenarios.


### Manual testing


In addition to automated testing, we perform manual testing to validate the overall user experience and identify any potential regressions that may have been missed by automation. This combination of tools and methods ensures that your app remains stable and that users enjoy a smooth, error-free experience after the upgrade.


## Continuous integration and deployment


At Rippling, **Continuous Integration (CI)** and **Continuous Deployment (CD)** pipelines are an integral part of our overall development workflow. These pipelines automate key steps like building, testing, and deploying applications, ensuring a seamless, reliable, and efficient process for all code changes, including React Native upgrades. This approach minimizes manual intervention, reduces the risk of errors, and maintains consistent application stability and performance.


We use[Bitrise](https://bitrise.io/) as our primary CI/CD platform, which allows us to:


-


**Automate builds** : Every code change, including React Native version upgrades, is automatically built to ensure compatibility and identify potential issues early in the process.


-


**Run tests** : Bitrise integrates with our testing framework to execute unit, integration, and end-to-end tests, ensuring that the application maintains functionality and performance post-upgrade.


-


**Streamline deployments** : Upgraded builds are deployed automatically to testing environments, and eventually to production, following successful validation.


By leveraging Bitrise, we maintain a consistent upgrade workflow that enhances both **Rippling Mobile** and **Rippling Kiosk** apps' stability and performance. This automation accelerates the deployment process, ensuring that users benefit from the latest features and improvements with minimal delay.


## Phased rollout to reduce risk of breaking changes


At Rippling, we adopt a **phased rollout strategy** to ensure a smooth and stable transition during React Native upgrades. This dual-approach strategy minimizes risks by tailoring deployment methods based on the type of changes being introduced.


For **native changes** , we utilize the **native phased-release capabilities** offered by the App Store and Google Play. These allow us to roll out updates to a percentage of users incrementally, ensuring that any issues with native components, such as updates to Android or iOS code, configuration files, or third-party libraries, can be identified and addressed early. This approach is particularly useful for updates that cannot be handled via OTA tools, such as changes to native modules or platform-specific optimizations.


For **JavaScript-only changes** , we leverage **OTA (Over-The-Air) updates** using tools like[Expo Updates](https://docs.expo.dev/versions/latest/sdk/updates/) . Previously, we relied on[CodePush](https://github.com/microsoft/react-native-code-push) for OTA releases, which allowed us to gradually release changes to user subsets. With the transition to Expo Updates, we’ve gained access to a more robust feature set:


-


**Flexible rollout controls:** Gradual deployments to targeted user groups help us monitor app behavior and collect feedback before broader rollouts.


-


**Enhanced rollback mechanisms:** Issues can be quickly mitigated with streamlined rollback capabilities, ensuring minimal disruption for users.


-


**Improved analytics:** Enhanced tracking provides detailed insights into performance and user engagement during phased rollouts.


-


**Cross-platform efficiency:** Expo Updates simplifies OTA updates for both Android and iOS platforms, keeping the user experience consistent.


By combining these two strategies, we ensure that **Rippling Mobile** and **Rippling Kiosk** apps maintain high performance and stability during updates. This approach enables us to balance the speed and flexibility of OTA updates with the rigor and safety of native phased releases, ensuring minimal disruption while maximizing the benefits of React Native upgrades.


## Monitor for issues


Post-deployment monitoring is a critical step in the React Native upgrade process. Even with comprehensive testing, some issues may only surface in the production environment. At Rippling, we rely on a robust monitoring stack to proactively identify and resolve problems, ensuring a seamless user experience after each upgrade.


-


**Error monitoring with Sentry** : We use[Sentry](https://sentry.io/for/react-native/) to track both **JavaScript** and **native crashes** . This allows us to capture detailed error reports, including stack traces and user context, helping us quickly pinpoint and resolve issues. Sentry’s real-time alerts ensure that our team is notified immediately when a problem arises, reducing response time.


-


**Logging and performance tracking with Datadog** : For deeper insights into application behavior, we leverage[Datadog](https://docs.datadoghq.com/integrations/rum_react_native/) . It helps us track logs, monitor application performance, and identify bottlenecks or anomalies in the system. With Datadog, we gain visibility into critical metrics like API response times, memory usage, and user interactions, enabling us to maintain high performance and stability.


By combining Sentry and Datadog, we create a **comprehensive monitoring ecosystem** that covers all aspects of our application, from JavaScript code to native modules. This proactive approach allows us to catch and address potential issues before they impact our users, ensuring a smooth post-upgrade experience for both **Rippling Mobile** and **Rippling Kiosk** apps.


## Update documentation


After completing the upgrade, we at Rippling ensure that all internal documentation is updated to reflect the changes. This allows future developers to easily understand the rationale behind the upgrade, as well as any new features or modifications. Keeping the documentation thorough and up-to-date is key to maintaining project continuity and facilitating a smoother transition for anyone working on the codebase in the future.


## Conclusion


Upgrading your React Native application may seem daunting, but with the right approach, it can be a seamless process that unlocks significant benefits. At Rippling, we've honed our strategy to ensure smooth transitions while minimizing disruptions. Here’s a recap of key takeaways:


-


**Adopt a gradual approach:** Break the upgrade into smaller, manageable steps to reduce risks and improve stability.


-


**Scope effectively:** Clearly identify necessary changes and prioritize them to focus on what's most impactful.


-


**Leverage automation:** Use tools like Bitrise and Expo Updates to streamline upgrade, testing, and deployment processes.


-


**Maintain backward compatibility:** Safeguard existing functionality to ensure a seamless experience for all users.


-


**Prioritize testing and validation:** Implement robust testing strategies, including automated and manual testing, to identify issues early.


-


**Utilize CI/CD pipelines:** Integrate upgrades into your CI/CD processes for efficient testing and reliable deployment.


By adopting these practices, you can not only minimize the challenges associated with upgrading but also maximize the value of new features, security patches, and performance enhancements that each new React Native version brings. Upgrading isn’t just a maintenance task—it’s a critical step toward keeping your application future-proof and delivering the best possible experience to your users.


We're hiring engineers!


[See open roles](https://www.rippling.com/careers/open-roles)


Disclaimer


Rippling and its affiliates do not provide tax, accounting, or legal advice. This material has been prepared for informational purposes only, and is not intended to provide or be relied on for tax, accounting, or legal advice. You should consult your own tax, accounting, and legal advisors before engaging in any related activities or transactions.
