---
schema_version: "1.0.0"
document_id: "3a63cad36521c8057a9568c11bebda534c3ab7530b5ae6a02bf3a3a12775bfcf"
company_key: "yc-automat"
company: "Automat"
source_id: "yc-automat-news-import-800367705320"
canonical_url: "https://runautomat.com/blog/automate-sap-citrix-without-apis"
published_at: "2026-04-13T18:09:17.472+00:00"
first_seen_at: "2026-07-24T17:51:31.960712+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:164937faca2697d29f367ec126656fb9a5b3f781603696f3c166cd0bb774eb8f"
---

# How to Automate SAP and Citrix Without APIs | Automat

Every operations team has them. The systems that run the business but resist every attempt at modernization. SAP GUI with its rigid transaction codes. Citrix virtual desktops that exist behind a pixel stream. AS/400 green screens. Government portals built in 2006 that your team logs into 400 times a day.


These systems share a common trait: no usable API. The only way to interact with them is through the screen, using a mouse and keyboard, the way they were designed to be used by humans.


For years, this meant one of two things. Either you hired people to do the repetitive work manually, or you tried traditional RPA and discovered why it struggles with these environments. Neither option scales well. Both cost too much.


But the technology has caught up. AI agents can now see and interact with any screen interface, including SAP, Citrix, and legacy terminals. This piece explains how it works, what's different from previous approaches, and what to consider before starting.


## **Why traditional RPA struggles with SAP and Citrix**


Traditional RPA tools were designed for web applications. They work by reading the HTML DOM, identifying elements by their CSS selectors or IDs, and replaying scripted actions against those elements.


SAP GUI doesn't have a DOM. It's a native Windows application with its own rendering engine. Traditional RPA tools handle this through SAP-specific connectors or scripting APIs, but these come with significant limitations:


- SAP scripting must be enabled on the server (many IT departments disable it for security reasons)
- Custom transactions, Z-codes, and modified SAP configurations break generic SAP connectors
- Every SAP upgrade or patch can invalidate scripted automation


Citrix is even harder. In a Citrix environment, the application runs on a remote server and streams pixels to the user's device. Traditional RPA can't read the DOM because there is no local DOM. The bot sees a video feed, not an application.


Legacy RPA vendors offer "image recognition" features for Citrix, but these are fragile. They match pixel patterns, and any change in resolution, font rendering, or color scheme breaks the match. Teams running Citrix automations with traditional tools report 30-50% of their maintenance effort goes to fixing image recognition failures alone.


## **How AI computer use changes the equation**


AI computer use works at the visual layer. Instead of reading code or matching pixels, a vision language model looks at a screenshot of the screen and *understands* what's there. It reads text, identifies buttons, recognizes form fields, and understands layout and context.


This is fundamentally different from pixel matching. The AI doesn't care whether a button is 3 pixels to the left of where it was yesterday. It sees the button, reads the label, and clicks it. The same way you do.


For SAP, this means:


- No dependency on SAP scripting being enabled
- Works with custom transactions, Z-codes, and modified configurations
- Survives SAP upgrades without automation rework
- Handles transaction chains that span multiple screens and decision points


For Citrix, this means:


- The bot operates on the pixel stream the same way a human does
- No need for Citrix-specific connectors or image templates
- Resolution changes, font differences, and theme updates don't break anything
- Multi-application workflows within a single Citrix session work natively


For legacy terminals (AS/400, mainframes), this means:


- Green screen text is read visually, not through terminal emulator APIs
- Navigation commands are typed naturally based on screen context
- The AI handles variable screen layouts and conditional menus


## **Real-world examples**


### **Mortgage lending: LOS data entry**


A mortgage lender needed to populate loan origination system (LOS) fields from broker document packages. The LOS had no API. Processors were manually keying data from PDFs into the system, touching each loan file 4-5 times. AI agents now extract data from the documents and enter it directly into the LOS through the screen interface. Processing time dropped from 45 minutes to under 5 minutes per file.


### **Insurance: claims across carrier portals**


An insurance operations team files claims across 8 different carrier portals, each with its own login, navigation, and form structure. Several carriers use Citrix-hosted applications. Traditional RPA required building and maintaining 8 separate automations with carrier-specific selectors. AI agents handle all 8 portals with a single workflow definition because they navigate visually rather than by element ID.


### **Financial services: KYC verification on government websites**


A payments company verifies business identities across government registries in multiple countries. These websites have unpredictable layouts, CAPTCHAs, and frequent redesigns. Traditional RPA broke weekly. AI agents verify the same registries reliably because visual navigation adapts to layout changes automatically.


## **What to consider before starting**


AI computer use is powerful but not magic. Here's what to think through:


- **Latency:** Vision-based interaction is slower per action than direct API calls. For high-frequency, low-complexity tasks, an API integration (if available) will always be faster. AI computer use is best for tasks where no API exists or where the process requires visual judgment.
- **Security and access:** The AI agent needs the same access credentials and permissions as a human operator. Work with your security team to provision service accounts with appropriate access levels.
- **Process stability:** Even self-healing agents benefit from well-documented processes. Share SOPs, screen recordings, or walkthroughs so the automation captures the right business logic, not just the clicks.
- **Compliance:** In regulated industries, audit trails matter. Make sure your platform logs every action the agent takes, with screenshots, for compliance review.


## **Frequently asked questions**


### **Does AI computer use work with SAP S/4HANA or only SAP GUI?**


Both. S/4HANA's Fiori web interface is actually easier to automate than SAP GUI because it's browser-based. But AI computer use handles both interfaces through the same visual approach.


### **What about two-factor authentication on Citrix?**


2FA can be handled through multiple approaches depending on the implementation. Token-based 2FA can be automated. SMS or push-based 2FA may require a human-in-the-loop step for the authentication event, after which the bot resumes the workflow.


### **How does this compare to SAP's own automation tools?**


SAP offers automation capabilities through SAP Build Process Automation (formerly SAP iRPA). These tools work well within the SAP ecosystem but require SAP scripting to be enabled and don't extend to non-SAP applications. AI computer use works across SAP and any other application in the same workflow.


### **Can this handle high-volume SAP transactions?**


Yes. AI agents can run parallelized across multiple SAP sessions, processing hundreds or thousands of transactions per day. The key constraint is SAP licensing (concurrent sessions), not the automation platform.
