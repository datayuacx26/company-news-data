---
schema_version: "1.0.0"
document_id: "2759d5ec77f743fbece104b9ff7e5aaa247efdd4490f0fe20e5594417d38421a"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/industry-insights/ai-threat-modeling/"
published_at: "2026-07-14T07:00:00+00:00"
first_seen_at: "2026-07-22T07:05:37.626494+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:380a28d1e34d095a3eabceaddd99403dbbc9db3e3f72f45ed40297e55a39cbb9"
---

# Threat modeling with AI: Enhancing cybersecurity workflows

The ubiquity of AI is a phenomenon that we encounter every day. You just have to drive down Route 101 through San Francisco to see dozens of billboards advertising AI products. In our daily lives, we see AI in medicine, in shopping, and even in[laundering our clothes](https://www.samsung.com/us/home-appliances/laundry/all-in-one-washer-dryer-combo/) . As with any new technology, there is a sinister side, and criminals, of course, are[using AI to commit crimes](https://openai.com/index/disrupting-malicious-uses-of-ai-by-state-affiliated-threat-actors/) .


At Okta, our business is to combat cybercriminals, whether they are aided by AI or not. Threat modeling is one of the cybersecurity techniques we use to strengthen our products against attackers. It is a natural place for us to apply AI.


## Understanding cybersecurity threat modeling


Threat modeling may seem exotic, but it is actually straightforward. We assess potential attacks, their likelihood and impact, and how to mitigate them. It’s something many of us do in other contexts without realizing it.


### A practical example of threat modeling and mitigation


I recently bought a new e-bike. Figuring out how to prevent it from being stolen is a threat modeling exercise. I park the bike in my garage, so that’s one scenario I need to analyze. Where we live, it’s unlikely a thief will break into our garage; something that might not be true if I lived in an apartment building in a large city, where garage access might be easier. The likelihood that my teenage children will leave the garage door open, however, is very high. For me, it’s enough to use a simple U-lock so that a would-be thief can’t grab the bike and ride away. On the other hand, if I’m using my bike in downtown San Francisco, I might want an angle-grinder-resistant lock to deter the most aggressive thieves.


Fill out the form to access this content.


[Source](https://upload.wikimedia.org/wikipedia/commons/9/96/Stolen_bicycle.jpg) /[License](https://creativecommons.org/licenses/by/4.0/deed.en)


## How Okta approaches threat modeling


At Okta, we do the same exercise with our software. We meditate on how an attacker could bypass our defenses and how we can strengthen those defenses.


We have a codified, but evolving, process for doing threat modeling at Okta. We diagram how data flows through the system and how the various components interact. We carefully enumerate the possible ways that an attacker could compromise our system and how we can protect ourselves. After writing our threat model, we submit it for internal peer and security-expert review, just as we do for code. We have just started using AI as part of our threat modeling process, and below I’ll share some early findings.


## Using Claude AI for threat modeling


Okta’s new[Device-Bound Single Sign-On (SSO) feature](https://www.okta.com/blog/product-innovation/device-bound-sso-safer-faster/) uses a specific[private key](https://hackernoon.com/public-key-cryptography-simply-explained-e932e3093046) to sign a payload sent to Okta’s back-end. Signing the payload with this key enables Okta to verify specific details about the end user’s machine.


I asked[Claude by Anthropic](https://claude.ai/) to help me build the threat model for the macOS version of the device-bound SSO feature. We have written several technical specs for the feature, including a high-level architectural document, implementation details for both Windows and macOS clients, and a threat model for a Windows client.


### Crafting the AI prompt


I provided all of this as context and used the following fairly simple prompt:


The Resources folder contains a description of the Special Key feature that we are building. In the Resources directory, there is technical specification for the feature for both macOS and Windows. That file is called SpecialKeySpec.pdf. There is also a spec that is specific for macOS, which is called macOSSpecialKeySpec.pdf.


You are a cybersecurity expert who is going to develop a threat model for this feature using the STRIDE framework.


I would like the following output:


* a level 0 context diagram built using the Mermaid format;


* a sequence diagram built using the Mermaid format;


* an enumeration of the threats that you find with suggested mitigations written using the Markdown format.


### Analyzing the AI threat model results


Threat modeling can feel overwhelming, but using AI effectively reduces the angst one feels when staring at an empty Google Doc. Claude had no such angst and quickly cranked out a list of about 20 threats. The output resembled what I would expect from a clever 12-year-old with an interest in cybersecurity: a lot of enthusiasm, some good ideas, but a general lack of nuance.


### Strengths: Where AI excels in threat modeling


Claude shines in getting the ball rolling and doing the busy work.


Claude showed prowess in writing first drafts of the diagrams we use in threat modeling. We diagram using the[Mermaid markup language](https://www.mermaidchart.com/) , and Mermaid diagrams involve a fair amount of boilerplate. Having Claude bootstrap the process gets you to a working diagram more quickly than if starting from scratch or even copying and pasting an existing diagram. On balance, the sequence diagram and data flow diagrams were decent. Since I spent so little time on initial scaffolding, I had more time to focus on editing and refining the diagrams to make them more precise.


Example of a data flow diagram


Fill out the form to access this content.


Example of a sequence diagram


Fill out the form to access this content.


Claude did well listing the threats related to compromising our private key. Claude also did a good job of enumerating[machine-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attacks. Such an attack occurs when someone or something eavesdrops on your internet traffic between your system components.


Finally, Claude listed several attacks that could occur if a malicious actor compromised one of our software components. I’m confident that we wouldn’t have missed these had we started from scratch, but it saved us a lot of time.


Fill out the form to access this content.


[Source](https://commons.wikimedia.org/wiki/File:%D0%A5%D0%B0%D0%BB%D0%B4%D0%BB%D0%B0%D0%B3%D0%B0.jpg) /[License](https://creativecommons.org/licenses/by-sa/4.0/deed.en)


### Limitations: Less useful AI security suggestions


Claude’s output, however, often lacked nuance. It identified multiple threats related to the leaking of non-secret data. For example, we have a session identifier, which is a unique string that is not at all secret. We didn’t specify that in our prompt, however, and Claude repeatedly raised the disclosure of that identifier as a problem.


Other threats simply didn’t make sense, and we could ignore them entirely. Claude enjoined us to replace HTTPS with XPC, something that is not actually practical. Claude wrote a couple of threats about password disclosure, but there are no passwords involved in the data flow.


## Conducting a threat model gap analysis


After spending several hours editing Claude’s initial list of threats and set of diagrams, I submitted my draft to our threat modeling workshop. There, experienced security modelers critiqued the design, providing valuable insights I incorporated directly into the model. I then went back to Claude and asked it to take my semi-finished model and analyze it for gaps, again using the Windows threat model for context.


Claude was again obligingly verbose and gave me a long list of possible issues. Most were not relevant, but there were a few gems. For example, because we had hardened a particular system component well, I hadn’t considered the threats that could occur without that hardening.


A good rule of threat modeling is to assume a particular threat has no mitigations and consider the outcome. I had ignored that rule, but Claude did not.


Claude provided a couple of other useful insights: one about encrypting inter-process communication and another about session hijacking. I probably would have eventually incorporated these as a result of cross-referencing the Windows threat model, but having “another pair of eyes” saved me time.


## Key takeaways on using AI in cybersecurity workflows


There are, of course, risks when using AI for threat modeling. For starters, you have to ensure that the AI tools aren’t exfiltrating your data. Even if you have secured your AI toolchain so that malicious results aren’t an issue, the results may be nonsensical. You have to do your own work just as diligently as ever. It’s easy to get a few good results from AI—a well-written email or a nicely coded function, for example—and assume that the results are always golden. Likewise, Claude can’t be the only one doing the brainstorming: the fact that Claude didn’t mention a particular threat doesn’t mean you can ignore it.


AI does a great job at getting things going. It produces a flurry of ideas. Some of those ideas are useful for building the final model, some prompt the human threat modeler to think of other things, and some can be ignored. AI can avoid cognitive biases, such as ignoring certain threats because we assume a part of the system is safe.


At Okta, we clearly see value in AI for threat modeling, but[our jobs are still secure](https://www.okta.com/blog/company-and-culture/human-judgment-vs-ai-averages/) (as threat modelers, anyway). AI remains solidly in the realm of a useful tool in our workflow, not the driver or owner of the workflow itself.


Want to see how other teams are putting these technologies to work? Discover how Okta employees[leverage AI to drive innovation](https://www.okta.com/blog/company-and-culture/okta-ai-innovation-revolution/) .


About the Author


[Francis Kelly Principal Engineer, Okta Device Access Francis Kelly is a Principal Engineer with the Okta Device Access team. He's responsible for designing and implementing Okta's Device Access features for the macOS platform, including Desktop Multifactor Authentication and Desktop Password Sync. He's been working in software development for over 30 years and in cybersecurity for over 10. He graduated from Yale University with a degree in math.](https://www.okta.com/blog/author/francis-kelly/)
