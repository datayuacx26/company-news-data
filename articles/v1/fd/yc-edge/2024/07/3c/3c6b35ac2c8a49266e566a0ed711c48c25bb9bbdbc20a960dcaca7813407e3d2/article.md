---
schema_version: "1.0.0"
document_id: "3c6b35ac2c8a49266e566a0ed711c48c25bb9bbdbc20a960dcaca7813407e3d2"
company_key: "yc-edge"
company: "Edge"
source_id: "yc-edge-rss-21208923c582"
canonical_url: "https://blog.withedge.com/p/uspto-issues-ai-invention-guidance"
published_at: "2024-07-18T14:42:12+00:00"
first_seen_at: "2026-07-25T01:55:11.618560+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:236809a7ae19fbd46bcd2b1f0ad4b6c6e368ef8027156c274b9f77aec15a718c"
---

# USPTO issues AI Invention Guidance

# USPTO issues AI Invention Guidance


### USPTO dropped some guidance on AI inventions. Here's what you need to know


[Evan Zimmerman](https://substack.com/@evanzimmerman)


Jul 18, 2024


As AI is exploding in the field, so are new patent applications. Lots of practitioners are having their clients ask them to file applications, but this is a new area for most practitioners. They need guidance. Especially because in an


*Alice/Mayo* world there is a fear that many of these ideas may be abstract.


USPTO came out with more information on this topic just a few days ago. Specifically, there is now


[new guidance](https://www.federalregister.gov/documents/2024/07/17/2024-15377/2024-guidance-update-on-patent-subject-matter-eligibility-including-on-artificial-intelligence) in the Federal Register as well as


[three new examples](https://www.uspto.gov/sites/default/files/documents/2024-AI-SMEUpdateExamples47-49.pdf) of inventions to help patent practitioners clarify patentable subject matter. But what does it mean?


This week on Nonobvious, we are going to briefly digest this USPTO guidance on AI subject matter patents and what they mean for practitioners.


Thanks for reading! Subscribe for weekly patent news.


## USPTO AI Guidance Analysis


USPTO provides several non-limiting examples of non-abstract ideas in its guidance, including:


1.


An ASIC for a neural network


2.


A system for analyzing livestock health that uses radios and machine learning and specifies which variables and data are used


3.


A treatment method for treating the disease NAS-3


There are also references to a number of examples from case law, including a specific RFID encoding. None of these examples should be particularly surprising to practitioners who have spent a lot of time with AI. First, these are all applications rather than artificial intelligence methods on their own. One is a hardware invention built for artificial intelligence; the next is a system (not a method!) that combines radios. Also of note, the livestock invention specifies the specific variables and data relating, which relate to movement. And the last is a treatment method, which is common in the life sciences.


1


The guidance emphasizes the importance of proving a true improvement, such as an improvement to a computer in a technical field. The guidance is firmly set in the


*Alice/Mayo* two-step analysis. Readers of Nonobvious will already know a number of these tips from last year’s


[AI patent guide](https://blog.withedge.com/p/drafting-patents-for-ai-tips-for) .


Yet, the guidance is unable to avoid some of the ambiguity of the abstract idea concept. Examiners are also supposed to evaluate whether the method represents an abstract idea or mental process. But the well-known complaint with this aspect is that mathematics and computation are a key ingredient in any computer invention; arguably, all code is just executed bytecode executed mathematically by a machine. So there remains a question as to where the line is in “abstraction.” USPTO tries to help by providing three examples.


They are:


-


Example 47: An artificial neural network that detects anomalies. The first, which is eligible, recites an ASIC with specific limitations to the neural network in question. The second claim, which is a method for training and using the neural network, is ineligible because it recites a very abstract idea while


*not connecting it to the practical application* . The third claim, however, is valid because it applies the neural network to the specific application.


-


Example 48: An algorithm for analyzing speech. The first claim is ineligible as abstract and just math. The second claim, which is dependent on the first claim, is eligible because it specifically improves speech separation and is not “directed to” the abstract idea exception. And the third claim, a computer readable medium, integrates the second claim into a practical application.


-


Example 49: A method for treating fibrosis using a machine learning method that applies several known treatments. There are two broadly similar claims, but the first one is ineligible as abstract for failing to connect to the practical application while the second one is. The main difference is that the second claim specifies a specific treatment rather than merely suggesting, as the first claim does, an “appropriate treatment.”


I find Claim 3 of Example 47 the most interesting. Here it is:


> \[Claim 3\] A method of using an artificial neural network (ANN) to detect malicious network packets comprising:
>
>
> (a) training, by a computer, the ANN based on input data and a selected training algorithm to generate a trained ANN, wherein the selected training algorithm includes a backpropagation algorithm and a gradient descent algorithm;
>
>
> (b) detecting one or more anomalies in network traffic using the trained ANN;
>
>
> (c) determining at least one detected anomaly is associated with one or more malicious network packets;
>
>
> (d) detecting a source address associated with the one or more malicious network packets in real time;
>
>
> (e) dropping the one or more malicious network packets in real time; and (f) blocking future traffic from the source address.


This is an extremely limiting claim. It does not require you to describe the training data (interestingly!) but does require specifying a particular machine learning algorithm. it also describes the specific actions the neural network and system as a whole are supposed to take based on the neural network’s predictions. This suggests to practitioners to try and tie neural networks to specific, but extremely broad, applications (you could plausibly define gradient descent to capture nearly all machine learning methods today, including transformers)


2


and to tie it to deterministic outcomes, like another algorithm or a physical device.


Despite these features, this claim has much in common with some of the example


*ineligible* claims. For example, Claim 1 of Example 48 has no practical example but does go into great detail into the specific model, describing the use of Fourier transforms. Similarly Claim 2 of Example 48 seems to me to be just as abstract as Claim 1, so I am not quite sure why USPTO feels this is more directed towards an improvement. USPTO’s


*Alice/Mayo* analysis emphasizes aspects from brief specification that describe how these additional steps are an improvement over using deep neural networks with Fourier transforms and specifically attach these improvements to the specific application, so perhaps there is a lesson in using the specification to help explain the non-abstractness of the claim. Similarly, Claim 1 of Example 49 describes a machine learning method that is applied to the life sciences. In fact, although it is less specific as to the machine learning method, it is more specific as to the types of data it uses (SNPs), but completely unspecific in the particular action taken (dropping offending packets vs applying an “appropriate treatment”).


I am a known critic of the vagueness of the abstraction rules. But my takeaways are these:


-


If you don’t have a practical application for your machine learning method, don’t bother


-


There is a sort of menu of abstraction: the machine learning method, the data used, the actions taken, and you need to pick from this menu but don’t necessarily need to have every one in your claims


-


The specification is important, and needs to tie your methods back to improvements and practical applications


-


Try to not do pure algorithm claims in the form of methods, though it is possible to get those granted


## Meet the Author


Are you at NAPP today? Come see Evan! We’re exhibiting


[Edge](https://withedge.com/) and he’d love to see you and show you the best patent software in the business. Even if you can’t come, reach out and he’ll give you a demo.


[Try Edge](https://withedge.com/contact-us)


## Weekly Novelties


-


Dennis Crouch also put out analysis of the USPTO guidance on PatentlyO (


[PatentlyO](https://patentlyo.com/patent/2024/07/guidance-eligibility-inventions.html) )


-


Some have accused pharmaceutical companies of watering down a bill to fight patent thickets (


[State News](https://www.statnews.com/pharmalot/2024/07/17/drug-patent-reform-affordable-prescriptions-for-patients-act-big-pharma-lobbying/) )


-


One analyst claims that the patent cliff’s biggest losers will be Merck, Bristol Myers Squibb, and Amgen (


[Fierce Pharma](https://www.fiercepharma.com/pharma/firepower-aplenty-and-patent-cliffs-ahead-time-right-ma-activity-report) )


-


The EFF claimed victory in efforts to limit a proposed rule that would have limited discretionary denials of IPRs (


[EFF](https://www.eff.org/deeplinks/2024/07/victory-eff-supporters-beat-uspto-proposal-wreck-patent-reviews) )


-


In


*SIPCO, LLC v. Jasco Products Company, LLC* another fun case about a small typo with big consequences (


[Law360](https://www.law360.com/ip/articles/1857925/patent-ruling-shows-a-minor-typo-can-lead-to-a-major-loss) )


-


Sanofi and Regeneron convinced the new UPC court to overturn a key Amgen patent, marking the first major invalidation of the UPC (


[Juve](https://www.juve-patent.com/cases/sanofi-and-regeneron-overturn-key-amgen-patent-at-upc/) )


-


The UK announced that it is beginning a six-month pilot for a new digital patent system; the first application filed under this new system was done by the firm Murgitroyd (


[Law360](https://www.law360.com/articles/1858508/uk-debuts-streamlined-digital-patent-service) )


1


That said, before


*Mayo* , diagnostic methods were often patented too, so I wouldn’t hang my hat on this one.


2


I know one practitioner who will take a random matrix method, like Brownian motion, and always add his own definition. Examiners never argue with it, but he will define it so broadly as to capture nearly all other random matrix operations, even unrelated ones like hidden Markov chains.
