---
schema_version: "1.0.0"
document_id: "a4724ef238c7589df02a7bf2fdb12db1003764fac43f6ebf3d148a83201e82f0"
company_key: "yc-oneleet"
company: "Oneleet"
source_id: "yc-oneleet-news-import-d01376dbbc95"
canonical_url: "https://www.oneleet.com/blog/crafting-a-custom-windows-auto-updater-for-go-powered-desktop-apps"
published_at: "2024-09-09T00:00:00+00:00"
first_seen_at: "2026-07-24T07:26:35.695106+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:f0f7171c2466fdc7615f4ccd6a75c72924b3560ffe20ca3b9dfb7a010a7ad94e"
---

# Crafting a Custom Windows Auto Updater for Go-Powered Desktop Apps

*Seamless Updates, Uncompromised Privacy: Our Journey with the Oneleet Agent for Windows*


We developed the Oneleet Agent as a Go-based desktop application that provides a continuous, privacy-respecting monitoring solution for end-user devices. Like any such tool, we needed auto updates built into the app. While the macOS version smoothly integrates with the renowned Sparkle framework for auto-updates, we hit a roadblock with the Windows version. After exploring and testing multiple options, none met our high standards. So, we took on the challenge and crafted our own custom auto updater for the Windows version, ensuring seamless updates without compromising on performance or privacy.


## Existing Solutions


When we set out to implement an auto-update feature for Oneleet Agent's Windows version, our first step was to evaluate the existing solutions available for Go-based desktop applications.


Here’s a brief overview of some of the solutions we found:


-


[Go-Update](https://github.com/inconshreveable/go-update) - This library offers basic update functionality but falls short when it comes to supporting more complex update scenarios and rollback features. Despite its initial promise, it hasn't seen updates in nearly a decade. Its intended successor,[Equinox](https://equinox.io/) , showed potential but was unfortunately discontinued in 2021. It still has active forks such as[https://github.com/minio/selfupdate](https://github.com/minio/selfupdate) .


-


[Getdown](https://github.com/threerings/getdown) - Originally written for Java applications, Getdown offers a rich set of features for application updates. However, integrating it with Go posed significant challenges, and we encountered compatibility issues that made it less than ideal for our specific requirements.


-


[Squirrel.Windows](https://github.com/Squirrel/Squirrel.Windows) - Popular in the Electron community, Squirrel.Windows provides an excellent framework for updating Windows applications. Despite its strengths, it’s heavily tailored for Node.js and Electron, making integration with Go applications cumbersome and not straightforward.


-


[Omaha](https://github.com/google/omaha) - Used for updating applications like Chrome, this is an updater framework from Google. While the client-side implementation of the Omaha protocol is open source and available for use, the server-side implementation remains closed source. This limitation posed a significant barrier, as a reliable and customizable server infrastructure is crucial for managing updates effectively.


-


[WinSparkle](https://winsparkle.org/) - Inspired by the Sparkle framework for macOS, WinSparkle aims to provide a similar auto-update experience for Windows applications. It’s a robust and well-documented solution.


Apart from the above solutions, something that we also looked into was[The Update Framework (TUF)](https://theupdateframework.io/) . TUF is renowned for its robust security features, designed to safeguard software update processes from a variety of attacks, including those targeting integrity and authenticity. By employing strong cryptographic techniques, it ensures that only authorized updates are applied, thereby maintaining a high level of trust. However, TUF's strength in security can become a drawback in the context of desktop applications that require seamless user experiences. Desktop apps often necessitate auto-restarts, installation wizards, and real-time feedback during updates, which TUF's security-centric model can complicate. The stringent checks and balances, while excellent for preventing tampering and unauthorized changes, can interfere with the smooth, user-friendly update flows typically expected in desktop environments, making it less ideal for applications prioritizing ease of use and minimal user intervention.


## Choosing WinSparkle


After evaluating various options , we decided to go with WinSparkle. The primary reason for this choice was our existing use of the Sparkle framework for the macOS version of the Oneleet Agent. Sparkle's reliability and user-friendly update mechanism had already proven effective for our needs, and thus made WinSparkle a natural fit for our Windows port.


To integrate WinSparkle with our Go-based application, we leveraged CGO to write bindings for WinSparkle. CGO allows Go packages to call C (and C++) code, which enabled us to bridge the gap between our Go application and the WinSparkle library. This approach allowed us to maintain consistency across platforms while using a familiar and robust framework for auto-updates.


### Initial Success


Initially, everything worked seamlessly. We successfully integrated WinSparkle into the Oneleet Agent, ensuring that our users on both macOS and Windows received smooth, automated updates. This solution met our requirements for a reliable update system without compromising on performance or security.


## Challenges with Broken Updates


However, when v1.0.0 of the Oneleet Agent for Windows was released, it started receiving broken updates, even though the signed updates were correctly created on our end. The issue appeared to be related to the Go bindings we had implemented, but with the pressing need to ship the next version, we didn't have the luxury of diving into the technical details of what was breaking (something that we will perhaps write a blogpost on at a later date). This unexpected problem forced us to reconsider our approach and look for a more reliable solution.


## Developing Our Own Auto Updater from Scratch


Faced with the challenges of broken updates and the pressing need to ensure seamless updates for our users, we decided to take matters into our own hands. The solution was clear: develop our own custom auto updater from scratch.


Starting this journey was no small feat. We began by outlining the core requirements for our auto updater:


-


**Reliability** : The updater had to be robust and capable of handling all update scenarios without failure.


-


**Security** : Ensuring that only properly signed and verified updates could be applied was paramount.


-


**Seamless Integration** : The updater needed to work flawlessly with our existing Go-based application as well as the native Windows experience to deliver a user-friendly experience.


With these goals in mind, we started designing the architecture of our auto updater. We broke down the project into several key components:


-


**Update Server** : A secure server to host and manage update files, ensuring that updates are distributed safely and efficiently.


-


**Client Integration** : Developing a client-side component that could check for updates, download them, verify their integrity, and apply them seamlessly.


-


**Security Mechanisms** : Implementing robust signing and verification processes to ensure the authenticity of update files.


-


**User Interface** : Creating a user-friendly interface to notify users of available updates and guide them through the update process.


By focusing on these components, we aimed to build an auto updater that would not only resolve the issues we faced with WinSparkle but also provide a superior update experience for our users.


The development process was intensive, requiring meticulous attention to detail and rigorous testing. Yet, the prospect of having a tailor-made solution that met all our requirements kept us motivated and driven.


In the following sections, we’ll delve into the specifics of each component and share insights from our development journey.


## Update Server


To manage and distribute updates securely and efficiently, we chose to host a JSON file containing the latest version and other essential metadata. The JSON structure would include fields such as` version` ,` releaseDate` ,` downloadUrl` , and` checksum` , providing necessary information for the client to verify and apply updates seamlessly.


## Client Integration


The updater code was written in Go itself, designed to poll the update server at regular intervals to check for the latest version. When a new version is detected, it downloads and verifies the update installer against the provided checksum. Once verified, the updater notifies the user of the available update, ensuring a seamless and secure update process.


## Security Mechanisms


Security is the most important part of any auto updater. We implemented several key mechanisms to ensure the integrity and authenticity of updates:


-


**SHA256 Checksum** : Each new version is accompanied by a SHA256 checksum, allowing the updater to verify the integrity of the downloaded file.


-


**Minisign** : We use[minisign](https://jedisct1.github.io/minisign/) to sign the new version, ensuring that updates are authorized and untampered. Minisign is a compact, fast, and easy-to-use tool for signing files and verifying signatures. We chose minisign for its simplicity and strong security features, which include:


-


**Ease of Use** : Minisign's straightforward command-line interface makes it easy to integrate into our release process.


-


**Strong Cryptography** : Minisign uses modern cryptographic algorithms to provide robust security.


-


**Small Footprint** : With a minimal binary size, minisign fits well within our lightweight update framework.


-


**Client-Side Verification** : The updater performs client-side verification of both the checksum validation as well as the minisign signature before applying any update, adding an extra layer of security to the process.


## User Interface


Once the auto updater downloads and verifies a new update, the user interface plays a crucial role in ensuring a smooth user experience. After successful verification, the user receives a notification informing them about the available update. The notification provides clear instructions and a prompt to initiate the update process. Users can then easily launch the installer from this notification to update their Oneleet Agent. This seamless integration ensures that users are always up-to-date with the latest features and security enhancements, without any hassle.


## Conclusion


Developing a custom auto updater for the Oneleet Agent on Windows was a challenging yet rewarding journey. By starting from scratch, we ensured that our updater met all our specific requirements: reliability, security, seamless integration, and cross-platform consistency. Leveraging robust security mechanisms like SHA256 checksums and file signature via Minisign, we crafted a solution that provides our users with a smooth and secure update experience without compromising on their privacy. Our commitment to delivering high-quality updates without compromising on user privacy and security remains steadfast.


If you're interested in trying out the Oneleet Agent, get in touch with us atsupport@oneleet.com .
