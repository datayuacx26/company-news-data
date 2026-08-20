---
schema_version: "1.0.0"
document_id: "d04b90fc1249386ed66472baf324ccc02a94cfda2a52182b8aafe0b7fc98484e"
company_key: "stmicroelectronics-n-v-common-stock"
company: "STMicroelectronics N.V."
source_id: "stmicroelectronics-n-v-common-stock-rss-811b8a53447b"
canonical_url: "https://blog.st.com/matter-commissioning-st25da-c/"
published_at: "2026-06-17T12:00:44+00:00"
first_seen_at: "2026-07-24T02:26:40.894294+00:00"
fetched_at: "2026-07-28T21:10:58.759142+00:00"
content_hash: "sha256:c64f6dccedb996e4e731962bcca24eda727d66a1965052165a113dccb6940328"
---

# ST25DA-C, 1st fully compliant Matter 1.6 device already at the center of NFC commissioning for many industry giants

*Editor’s Note: We have updated this blog post to reflect the release of Matter 1.6*


**ST is announcing today that the ST25DA-C is the first fully compliant Matter 1.6 device on the market, ensuring that a secure NFC device can serve to commission Matter end products over NTL (NFC Transport Layer).** Put simply, we expect a slew of Matter devices to use NFC to commission nodes to their network, as an alternative to Bluetooth LE (BLE). Under the hood, the ST25DA-C shares DNA with STSAFE, our Secure Element, while adding NFC capabilities for Matter contactless commissioning. As a result, it can store and manage keys as well as accelerate certain cryptographic operations. This is ST’s first[ST25D device](https://www.st.com/en/nfc/st25-dynamic-nfc-tags.html) with embedded security, thus enabling a whole new type of interaction with Matter products.


## What does it mean for the Matter world?


Using NFC commissioning to provision a smart light bulb


The uniqueness of the ST25DA-C means companies can start working on taking advantage of NFC commissioning. They can design their products, add the NFC tag, and work toward getting the certifications. This has been met with enthusiasm from many in the industry because starting work on this sooner means device makers have greater assurances that their product will be ready on time and can grab early market shares. **Some of the largest companies in this industry are already using the ST25DA-C in preparation for certification and market release.** Indeed, beyond offering just a component, we are also helping with testing operations, thanks to our work with the CSA.


In addition to being a member of the CSA and having ST employees participate in the standardization process, we had direct involvement in writing the specification and the tests to meet the requirements of the NFC commissioning feature for the Matter 1.6 certification. **This unique position enabled ST to become the first fully compliant commissionee, meaning we provide the device that commissioners will use to add nodes to their smart home platforms.** Put simply, the work that took place behind the scenes will help drive adoption and make NFC commissioning a reality for more users faster.


## Why the CSA was looking into NFC Commissioning?


### Limitations of QR Codes and BLE


A man using NFC for Matter Commissioning


In a nutshell, commissioning is the process that securely adds an end device to a network. **As a result, it requires communication with the gateway and an exchange of keys to guarantee the network’s integrity.** In the case of Matter, most end devices use a QR Code or a passcode that the user must enter on a mobile device. That smartphone or tablet must then use a Bluetooth LE connection to talk to the end node to verify that code, check credentials, install a certificate on that end device, and then share connection information to enable it to connect to the gateway.


While this process serves many use cases well, it is not without potential issues, chief among them being the impracticality of using a QR code or, more rarely, a passcode. **Using BLE for commissioning is also impossible when external power is unavailable.** In this instance, the device needs to be powered on before starting the Matter commissioning process, which is inconvenient for a switch installed in a wall fixture or a ceiling-mounted video camera. It can also be impossible when connecting many devices in a smart building, for instance, where the power grid is not yet in place.


### Limitations of NFC onboarding


Using a ST25DA-C and NFC to commission a Matter device


To solve the shortcomings of QR codes or passcodes, the Matter protocol has supported NFC onboarding since launch, and every major version brings updates that make it even more practical. **In essence, an NFC device compensates for the inherent limitations of a QR code or passcode, since it stores that code in its memory.** Instead of a sticker that may be damaged or lost, the NFC tag stores the code that starts the onboarding process. It also means users no longer have to reach behind a device to read a small set of numbers. Tapping it with a smartphone is far more expedient. An end node could even use a dynamic passcode, thanks to NFC’s capabilities, for even greater security.


However, until Matter 1.5, NFC could only be used for onboarding, meaning a Bluetooth LE connection was still required to commission a device. That meant using BLE to exchange encryption keys and perform the handshakes needed to join the Thread or Wi-Fi network. Before today, NFC was just a replacement for QR Codes, which meant users still faced issues with power requirements, among other things. As we explained, installing light fixtures in a building under construction means there is no power at all, and smart bulbs don’t come with batteries. That’s why the people behind Matter looked for an alternative.


## How is the ST25DA-C implementing NFC commissioning?


### The move to NFC commissioning


The ST25DA-C


In 2021, the[CSA announced partnering with the NFC Forum](https://www.st.com/content/st_com/en/campaigns/matter-commissioning-with-nfc-z21.html) to “\[improve\] experiences in areas like commissioning and proof of possession.” **We can therefore expect NFC Commissioning to take on a more significant role, as the consortium behind Matter announced it was exploring “tap to commission” and other ways to make the process more straightforward and intuitive.** In fact, ST, as a CSA member, has been actively shaping the protocol to achieve that goal. Indeed, the ability to tap to commission creates a user experience that’s fast, simple, intuitive, and very stable. Indeed, there are far fewer unpredictable variables that could disrupt the onboarding process, which explains why the industry has high hopes for tap-to-commission.


### The jump to ST25DA-C for security and NFC commissioning


At its core, the ST25DA-C is an NFC tag with the processing power to support the Matter commissioning process, including advanced cryptographic operations. It also means it can store more than just code, but also certificates and keys. When a smartphone taps the ST25DA-C, the tag initiates the cryptographic verification process using only the electrical current generated by the contactless technology. At the end of this interaction between the ST25DA-C and the mobile application, the product is pre-commissioned with the network credentials and ready for joining the operational network when the main power is turned on.


Matter NFC commissioning with the ST25DA-C Commissioning a smart thermostat


Interestingly, the computational power inside the ST25DA-C is so significant that additional secure functions can be operated (secure storage, secure channel, digital signature, and potentially more at a later date). **A little-known fact is that the ST25DA-C hardware is certified Common Criteria EAL6+, one of the highest assurance levels for such a product, making it the most secure companion for a main microcontroller.** Another benefit of such a strong solution is that we will begin the SESIP Level 3 validation process and confidently plan to receive the certification soon. Although Matter doesn’t require this, we decided to invest in it to provide greater assurances to our IoT partners.


Matter logo


The ST25DA-C is the smallest available device (2 mm x 3 mm ultra-thin DFN8 package) allowing engineers to use it in small IoT systems. To minimize its footprint, this Type 4 NFC component comes with a 78-pF tuning capacitor, allowing the use of a highly integrated antenna, while engineers can still expect to work well with most smartphones and other mobile devices. Hence, the overall package can fit in light bulbs, smoke detectors, and other smart devices with very limited space. **Additionally, we provide reference designs, development boards, and example applications to facilitate integration into a final product, lower the time to market, and democratize Matter 1.6 NFC commissioning.**


- [Check out the ST25DA-C](https://www.st.com/content/st_com/en/campaigns/matter-commissioning-with-nfc-z21.html)
