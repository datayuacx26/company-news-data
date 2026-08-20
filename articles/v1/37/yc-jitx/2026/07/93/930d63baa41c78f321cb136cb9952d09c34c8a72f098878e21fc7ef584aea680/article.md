---
schema_version: "1.0.0"
document_id: "930d63baa41c78f321cb136cb9952d09c34c8a72f098878e21fc7ef584aea680"
company_key: "yc-jitx"
company: "JITX"
source_id: "yc-jitx-news-import-aad5bf5f2004"
canonical_url: "https://www.jitx.com/blog/faq"
published_at: null
first_seen_at: "2026-07-22T00:57:30.232566+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:589572c3386f12cfda429d6d969c1e15888ebf0cef9269261c96871f909a95bd"
---

# Duncan Haldane

> What is this?


JITX is a new type of software for automating electrical engineering and circuit board design. Previously, engineers designed electronics by googling for components, reviewing PDFs, building spreadsheets, then drafting and manually reviewing schematics.


JITX users write code to generate designs so hardware developers can get all the benefits of modern software development: speed, reuse, and automated testing (along with actual version control).


> What does it do?


JITX lets you write arbitrary code to generate, optimize and analyze designs, and export them to CAD (KiCAD and Altium right now, we will write more exporters). It is a way to automate your hardware engineering process. We have written solvers to automate low-level work (like component selection, pin assignment and schematic drafting), so users code at a high-level, quickly iterate on designs, and then use existing CAD tools to finalize placement, routing, and export fabrication data.


You also can import existing designs into JITX to perform operations like design checking or component optimization (Or anything you care to do really. We just make it easy to write code for manipulating and checking designs).


> How much is automated?


Comparing against the usual design flows, the input to JITX are engineering requirements (like key component selections and optimization goals) and the output is a schematic and BoM that has been rigorously checked for correctness, and a board with routing rules and an initial component placement. Everything between is automated. This design is exported back to your existing CAD tool for final place and route.


We’re working towards 100% automation for complex boards so as time goes by, users generate designs with more and more automation.


> Does it work?


Yes. We dogfooded these tools as a design services agency for a couple years, taking on real projects and using JITX generate the designs and have them fabricated. Once we were able to ship designs significantly faster than a human could using traditional tools, we productized the technology and opened it up for external use.


Today, customers estimate that they can get designs out between 2.5x and 6.0x faster by using JITX. JITX works well enough to help people try more ideas and hit deadlines with more certainty. Specific areas of design can be automated completely.


> How much coding experience do you need to use JITX?


Surprisingly little. Anyone who has written a python or Matlab script has been able to learn JITX in an afternoon and start generating designs programmatically. We put a lot of work into making the syntax of our programming language intuitive for electrical engineers, which helps. Our IDE helps with the details of managing the fairly simple syntax.


> What are the limits?


There are no limits in terms of schematic pages, pin count, number of layers, board size, via technology etc… The more complex a design the greater the benefit of JITX, so go ahead and generate those 60-page schematics.


> How long does it take to ramp up with JITX?


Once you have an account with JITX, software installation takes 5 minutes, and you can immediately start running code to generate designs. We have a set of tutorials that teach the ins and outs of the programming language that new users complete in 1 to 3 hours.


After that, you can either start writing your own brand new designs, or import your existing designs to refactor them and start generating new versions programmatically.


We have a[shared open-source library of components](https://github.com/JITx-Inc/open-components-database) and utilities to help you get started. Finally we create shared slack channels with all of our customers so JITX engineers can help you build particularly advanced automation.


> Does JITX run locally or in the cloud?


Today JITX runs on your local machine, but hooks into cloud services for things like sourcing real-time component information. Currently supported operating systems are Ubuntu and MacOS. Other OS's are easy to add, so let us know if you need support for something in particular.
