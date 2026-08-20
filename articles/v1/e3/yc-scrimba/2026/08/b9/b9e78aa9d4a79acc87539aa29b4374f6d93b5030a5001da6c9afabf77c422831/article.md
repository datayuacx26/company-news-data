---
schema_version: "1.0.0"
document_id: "b9e78aa9d4a79acc87539aa29b4374f6d93b5030a5001da6c9afabf77c422831"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/ai-tutors-what-they-do-well/"
published_at: "2026-08-13T09:45:53+00:00"
first_seen_at: "2026-08-13T15:35:54.418819+00:00"
fetched_at: "2026-08-13T15:35:56.041700+00:00"
content_hash: "sha256:87206ec2d23ce047548a2a6e21436deb938715025cf488abc6638106794260e3"
---

# AI Tutors: What They Do Well and Where They Fail [2026]

An AI tutor is available at two in the morning, never sighs at the ninth version of the same question, and will re-explain a concept six different ways without losing patience. None of that tells you whether you learned anything.


The category is sold on a result from 1984 that has never been replicated, and reviewed almost nowhere on what it does badly. The failure modes are specific, documented, and mostly avoidable once you know their shapes.


This guide covers what AI tutors do well, six ways they fail, what randomized trials found, and a method for using one without fooling yourself.


## What is an AI tutor?


An AI tutor is a language model prompted to teach rather than answer, available on demand and able to adjust its explanation to the learner. It is a strong first explainer and a poor judge of understanding.


Most disagreement about whether AI tutors work comes from people comparing three different things:


- **A general chatbot.** No pedagogy, no restraint, answers whatever you ask. This is what most people mean when they say they use an AI tutor.
- **A purpose-built tutoring mode.** ChatGPT's[study mode](https://openai.com/index/chatgpt-study-mode/?ref=scrimba.com) uses Socratic questioning and knowledge checks instead of answering outright. Google's[Guided Learning](https://blog.google/products/gemini/guided-learning-google-gemini/?ref=scrimba.com) breaks problems into steps, and[Khanmigo](https://www.khanmigo.ai/?ref=scrimba.com) says it guides learners to the answer "with limitless patience" rather than handing it over.
- **A research-grade tutor.** Built for one course, with expert-written prompts and guardrails. This is what appears in the studies that produce impressive numbers.


Conflating those three is how the same technology gets called transformative and harmful in the same week, both accurately.


## What do AI tutors do well?


AI tutors excel at availability, patience, adjusting explanation level on demand, generating unlimited practice problems, and low-stakes conversation practice in a language you are learning.


The strengths are real, and worth separating from the marketing:


- **Availability.** The bottleneck in self-directed learning is usually the gap between getting stuck at 11pm and finding help on Tuesday.
- **Patience without cost.** Asking a human to explain closures a fourth time carries a social tax. Asking a model carries none, and the fourth explanation is where some concepts finally land.
- **Adjusting the level.** "Explain that as if I have never seen recursion" and "now give me the version you would give a senior engineer" are both one sentence away.
- **Generating practice.** Twenty variations on a problem type, graded by difficulty, in seconds. *This is the most underused capability in the category* , and the one that most resembles what tutoring actually is.
- **Language conversation.** For spoken practice, the scarce resource is a partner who will not judge you.[Duolingo](https://www.duolingo.com/efficacy?ref=scrimba.com) publishes a 2025 whitepaper reporting speaking gains from its AI video call feature, though that is self-published rather than peer-reviewed.
- **Explaining the same idea a different way.** When one framing does not land, a second often does. For learners who would rather watch than read, Scrimba Explain takes a question an AI agent has researched and returns a narrated video explainer, on non-code subjects as well as code. Scrimba's own FAQ describes the narration as talking you through it "like a private tutor would", and the same FAQ says it can make mistakes and that anything important should be double-checked. It is free during open beta.


The evidence carries a condition. In a randomized trial with 194 Harvard physics undergraduates, students using a custom AI tutor showed median learning gains more than double those of the in-class active-learning group, in less time ([Kestin et al.](https://www.nature.com/articles/s41598-025-97652-6?ref=scrimba.com) ). A six-week World Bank trial in Nigerian secondary schools found gains of 0.31 standard deviations overall and 0.23 on English, the main outcome ([De Simone et al.](https://ideas.repec.org/p/wbk/wbrwps/11125.html?ref=scrimba.com) ).


Both involved a deliberately designed tutor or a supervising teacher. Nobody has shown that handing a learner an unmodified chatbot produces anything close.


## Where do AI tutors fail?


AI tutors fail at verifying understanding, diagnosing confusion, resisting agreement, and providing accountability. Their most dangerous failure is confident error in subjects the learner cannot yet check.


### 1. They are confidently wrong exactly where you cannot check


This is the core danger, and it is structural rather than incidental. Models are trained and graded in ways that reward guessing over admitting uncertainty, which is why hallucination persists in frontier systems. The paper makes the comparison directly: "Like students facing hard exam questions, large language models sometimes guess when uncertain" ([Kalai et al.](https://arxiv.org/abs/2509.04664?ref=scrimba.com) ).


The problem is not the error rate. It is that the error arrives in the one subject you cannot audit, which is the only reason you were asking. The Harvard team names the same flaw, describing AI tutors' *uncanny confidence* when giving an incorrect answer ([Kestin et al.](https://www.nature.com/articles/s41598-025-97652-6?ref=scrimba.com) ).


### 2. They have no model of what you understand


A good human tutor carries a running theory of your knowledge: what you have solid, what you fake, which mistake you keep repeating. A chat session carries a transcript.


So the tutor cannot tell a student who has understood from a student who has nodded. It optimizes for the response that ends the exchange smoothly.


### 3. They agree with you


Sycophancy is measured, not anecdotal. Across five leading assistants, researchers found responses matching a user's stated view are more likely to be preferred, and that humans and preference models sometimes favor a convincingly written sycophantic answer over a correct one ([Sharma et al.](https://arxiv.org/abs/2310.13548?ref=scrimba.com) ).


It has failed publicly too. OpenAI rolled back a GPT-4o update in April 2025 for being "overly flattering or agreeable", and called the result *overly supportive but disingenuous* ([OpenAI](https://openai.com/index/sycophancy-in-gpt-4o/?ref=scrimba.com) ). A tutor that folds the moment you push back is weakest on the exact question you got wrong.


### 4. They are weak at diagnosing why you are stuck


Teaching is mostly diagnosis. The default behavior of a language model is exposition.


One measurement makes the gap concrete: fine-tuning a model on 100,000 hours of real one-to-one tutoring dialogue doubled student talk time and raised the number of dialogue turns by 50% against the base model ([Perczel et al.](https://arxiv.org/abs/2510.05087?ref=scrimba.com) ). That is a direct measure of how much a default model talks instead of asking.


### 5. There is no accountability structure


Nobody notices when you stop. The tutoring meta-analysis found programs run during the school day outperformed after-school ones ([Nickow et al.](https://www.nber.org/papers/w27476?ref=scrimba.com) ), a finding about obligation at least as much as instruction. An AI tutor is all after-school.


### 6. Fluency feels like understanding


This failure is invisible while it happens, which is why it produces the worst outcomes.


In a randomized trial with nearly a thousand Turkish high-school maths students, unrestricted GPT-4 access raised performance during practice sharply. When access was removed for the exam, those students scored *worse* than peers who never had it. A version with pedagogical guardrails largely removed the harm ([Bastani et al.](https://www.pnas.org/doi/10.1073/pnas.2422633122?ref=scrimba.com) ).


The mechanism has a precedent outside AI entirely. Students in active-learning physics classes learned more than students in polished lectures while rating their own learning lower ([Deslauriers et al.](https://pubmed.ncbi.nlm.nih.gov/31484770/?ref=scrimba.com) ). Effortful learning feels like confusion. A smooth explanation feels like competence.


> The tutor answered the question. Answering the question was the thing you were supposed to learn to do.


## Does the research support AI tutoring?


The evidence is mixed and conditional. Purpose-built AI tutors under supervision have beaten classroom instruction in randomized trials, while unrestricted chatbot access has measurably reduced unaided exam performance.


Almost every article on this topic opens with the same claim. In 1984, Benjamin Bloom reported that students taught one-to-one under mastery learning outperformed conventionally taught students by about two standard deviations, and he framed the paper around the *problem* of getting group instruction to match that ([Bloom 1984](https://journals.sagepub.com/doi/10.3102/0013189X013006004?ref=scrimba.com) ).


It has not held up as a benchmark. The underlying experiments ran three weeks, on narrow post-tests covering only the material just taught. Paul von Hippel notes that among 96 tutoring studies examined, *none produced a two-sigma effect* ([Education Next](https://www.educationnext.org/two-sigma-tutoring-separating-science-fiction-from-science-fact/?ref=scrimba.com) ). The meta-analytic figure for real tutoring programs is 0.37 standard deviations ([Nickow et al.](https://www.nber.org/papers/w27476?ref=scrimba.com) ). Two sigma is the number the AI tutoring category quietly promises to deliver at scale.


Study Design What it found What it does not show


Bloom, 1984 Three-week experiments, narrow post-tests Tutored students about 2 SD above conventional classes That the effect survives longer courses or broad assessments


Nickow, Oreopoulos and Quan, 2020 Meta-analysis of experimental tutoring studies Pooled effect of 0.37 SD; professional tutors beat volunteers Anything about AI; these are human tutoring programs


Kestin et al., 2025 RCT, 194 Harvard physics undergraduates Median gains more than double active learning, in less time That an off-the-shelf chatbot does this; the tutor was purpose-built


De Simone et al., 2025 RCT, six weeks, Nigerian secondary schools 0.31 SD overall, 0.23 SD on English That unsupervised use works; sessions were teacher-supervised


Bastani et al., 2025 RCT, ~1,000 Turkish high-school maths students Unrestricted access raised practice scores, lowered unaided exam scores That guardrailed tutors are harmful; those largely avoided the effect


Read together, the summary is short. A well-designed AI tutor with a supervising adult can beat a classroom on narrow content. An unsupervised chatbot can leave a learner worse off than no tool at all. The difference between those two sentences is design and accountability, not model quality.


## How do you use an AI tutor well?


Use an AI tutor for the first explanation rather than the final understanding. Ask it to quiz you, make it show its working, verify anything load-bearing, and rebuild the thing unaided.


1. **Ask it to quiz you before you ask it to explain.** Testing yourself is what moves material into memory, and it is the request almost nobody makes. Open with "ask me five questions on this, hardest first".
2. **Make it show its working, then check one step yourself.** Pick a step you could have done unaided and verify it. That single check calibrates how much to trust the rest of the answer.
3. **Verify anything load-bearing against a primary source.** The documentation, the textbook, the syllabus. Not a second AI answer, which is the same instrument read twice.
4. **Push back once on purpose.** Challenge something you know is correct. If the tutor folds immediately, you have measured its sycophancy and can price its agreement accordingly for the rest of the session.
5. **Use it for the first explanation, not the final understanding.** A clean paragraph or a narrated video explainer is an entry point, which is why Scrimba tells Explain users to double-check anything important. The test is whether you can reproduce the answer with the tab closed.
6. **Close the loop the same day.** Rebuild the thing unaided, and put whatever you got wrong into spaced repetition. Skip this and the session was entertainment.


If your subject is code, the[beginner's guide to learning to code](https://scrimba.com/articles/how-to-start-learning-to-code-a-complete-beginners-guide-2026/) covers how to sequence this alongside a real curriculum, the[roundup of AI tools and courses for learning to code](https://scrimba.com/articles/best-ai-tools-and-courses-for-learning-to-code/) covers the tooling, and[how web developers can use AI](https://scrimba.com/articles/how-web-developers-can-use-ai/) covers the professional version.


## When do you still need a human?


You need a human when you do not know what you do not know, when motivation rather than information is the blocker, and when work needs judging rather than explaining.


- **Naming the gap.** An AI tutor answers the question you asked. A good teacher notices the question you should have asked, usually two topics upstream of where you are stuck.
- **Motivation and obligation.** The tutoring evidence keeps pointing at supervision and schedule as much as instruction ([Nickow et al.](https://www.nber.org/papers/w27476?ref=scrimba.com) ). A person expecting you on Thursday does work no model currently does.
- **Judged work.** Code review, a written argument, a design decision. These need a standard applied by someone with a stake in the outcome, not a description of what good looks like.


For how long the whole process actually takes, the guide on[how long it takes to learn to code](https://scrimba.com/articles/how-long-does-it-take-to-learn-to-code-a-realistic-timeline/) beats any vendor's marketing as a planning tool.


## Frequently Asked Questions


### Are AI tutors actually effective?


Sometimes, under conditions. Purpose-built AI tutors used under supervision have produced real gains in randomized trials, including a Harvard physics study and a World Bank trial in Nigeria. Unrestricted chatbot access has produced worse unaided exam performance than no access at all.


### Can an AI tutor replace a human tutor?


Not yet, and not for the reason most people expect. The gap is not knowledge. It is diagnosis, an accurate model of what the learner actually understands, and the obligation to show up, which is the part of tutoring that no current product supplies.


### Do AI tutors give wrong answers?


Yes, and the risk peaks in subjects you cannot yet check. Language models are trained and evaluated in ways that reward guessing over admitting uncertainty, so errors arrive with full confidence. Verify anything you will be tested on against a primary source.


### What is Bloom's two-sigma problem, and is it real?


Bloom reported in 1984 that tutored students outperformed classroom students by about two standard deviations, based on three-week experiments with narrow post-tests. Later reviews found no tutoring study reproducing it. The typical real-world tutoring effect is about one-third of a standard deviation.


### How should a beginner use an AI tutor?


Ask it to quiz you before asking it to explain, make it show its working and check one step yourself, verify anything important against documentation or a textbook, and rebuild the thing unaided before moving on to the next topic.


## Key Takeaways


- The two-sigma result has never been replicated in a tutoring study; 0.37 standard deviations is the benchmark for real tutoring programs.
- Designed AI tutors under supervision have beaten classroom instruction; unsupervised chatbot access has measurably hurt unaided performance.
- Hallucination lands hardest in the subjects a learner cannot yet verify, which is exactly when the tutor is being used.
- Sycophancy is a measured property of current assistants, not a personality quirk, and it is worst on the answers you got wrong.
- A chat session is a transcript, not a model of what you understand.
- The feeling of learning is a poor instrument, and a smooth explanation optimizes the feeling.
- Ask the tutor to quiz you first, explain second, and rebuild the thing unaided before you call it learned.


## Sources


- Bloom, Benjamin S. "The 2 Sigma Problem." Educational Researcher 13, no. 6, 1984.[https://journals.sagepub.com/doi/10.3102/0013189X013006004](https://journals.sagepub.com/doi/10.3102/0013189X013006004?ref=scrimba.com)
- von Hippel, Paul T. "Two-Sigma Tutoring: Separating Science Fiction from Science Fact." Education Next 24, no. 2, 2024.[https://www.educationnext.org/two-sigma-tutoring-separating-science-fiction-from-science-fact/](https://www.educationnext.org/two-sigma-tutoring-separating-science-fiction-from-science-fact/?ref=scrimba.com)
- Nickow, Andre, et al. "The Impressive Effects of Tutoring on PreK-12 Learning." NBER Working Paper 27476, 2020.[https://www.nber.org/papers/w27476](https://www.nber.org/papers/w27476?ref=scrimba.com)
- Bastani, Hamsa, et al. "Generative AI without guardrails can harm learning." PNAS 122, no. 26, 2025.[https://www.pnas.org/doi/10.1073/pnas.2422633122](https://www.pnas.org/doi/10.1073/pnas.2422633122?ref=scrimba.com)
- Kestin, Greg, et al. "AI tutoring outperforms in-class active learning." Scientific Reports 15, 17458, 2025.[https://www.nature.com/articles/s41598-025-97652-6](https://www.nature.com/articles/s41598-025-97652-6?ref=scrimba.com)
- De Simone, Martin, et al. "From Chalkboards to Chatbots." World Bank Policy Research Working Paper 11125, 2025.[https://ideas.repec.org/p/wbk/wbrwps/11125.html](https://ideas.repec.org/p/wbk/wbrwps/11125.html?ref=scrimba.com)
- Deslauriers, Louis, et al. "Measuring actual learning versus feeling of learning." PNAS 116, no. 39, 2019.[https://pubmed.ncbi.nlm.nih.gov/31484770/](https://pubmed.ncbi.nlm.nih.gov/31484770/?ref=scrimba.com)
- Kalai, Adam Tauman, et al. "Why Language Models Hallucinate." arXiv:2509.04664, 2025.[https://arxiv.org/abs/2509.04664](https://arxiv.org/abs/2509.04664?ref=scrimba.com)
- Sharma, Mrinank, et al. "Towards Understanding Sycophancy in Language Models." ICLR 2024.[https://arxiv.org/abs/2310.13548](https://arxiv.org/abs/2310.13548?ref=scrimba.com)
- Perczel, Janos, et al. "TeachLM." arXiv:2510.05087, 2025.[https://arxiv.org/abs/2510.05087](https://arxiv.org/abs/2510.05087?ref=scrimba.com)
- OpenAI. "Sycophancy in GPT-4o." April 2025.[https://openai.com/index/sycophancy-in-gpt-4o/](https://openai.com/index/sycophancy-in-gpt-4o/?ref=scrimba.com)
- OpenAI. "Introducing study mode." July 2025.[https://openai.com/index/chatgpt-study-mode/](https://openai.com/index/chatgpt-study-mode/?ref=scrimba.com)
- Google. "Guided Learning in the Gemini app." August 2025.[https://blog.google/products/gemini/guided-learning-google-gemini/](https://blog.google/products/gemini/guided-learning-google-gemini/?ref=scrimba.com)
- Khan Academy. Khanmigo, self-reported. August 2026.[https://www.khanmigo.ai/](https://www.khanmigo.ai/?ref=scrimba.com)
- Duolingo. Efficacy research, self-reported. August 2026.[https://www.duolingo.com/efficacy](https://www.duolingo.com/efficacy?ref=scrimba.com)
- Scrimba. Self-reported product information for Scrimba Explain. August 2026.
