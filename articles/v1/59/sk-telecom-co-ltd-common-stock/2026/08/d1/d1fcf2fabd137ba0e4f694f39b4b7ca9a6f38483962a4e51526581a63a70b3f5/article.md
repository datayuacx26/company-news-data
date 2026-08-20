---
schema_version: "1.0.0"
document_id: "d1fcf2fabd137ba0e4f694f39b4b7ca9a6f38483962a4e51526581a63a70b3f5"
company_key: "sk-telecom-co-ltd-common-stock"
company: "SK Telecom Co. Ltd."
source_id: "sk-telecom-co-ltd-common-stock-rss-953576b46343"
canonical_url: "https://news.sktelecom.com/en/3213"
published_at: "2026-08-05T23:29:01+00:00"
first_seen_at: "2026-08-06T00:10:52.239292+00:00"
fetched_at: "2026-08-06T00:10:54.071375+00:00"
content_hash: "sha256:cf7545057110c11659ca2d3c2cce5103500f4440b276116aa3332c9986524a41"
---

# [MGAIC 2] “AI That Thinks Twice: The Core Edge for SKT’s Sovereign AI Model” – Interview with MIT Professor Yoon Kim

## Recommended


- [Insight \[MGAIC 1\] “SKT’s move to develop its own AI foundation model is a highly strategic move” – Interview with MIT Professor Pattie Maes 2026.04.21](https://news.sktelecom.com/en/2993)


- Facebook
- X
- Email
- Copy URL
- Print


Professor Yoon Kim at MIT


SK Telecom is working closely with the MIT Generative AI Impact Consortium (MGAIC), a global industry–academia research consortium led by Massachusetts Institute of Technology, to drive AI innovation.


MGAIC serves as a cross-industry AI collaboration platform bringing together global leaders such as The Coca-Cola Company, OpenAI, and SKT. Through more than 60 active projects, the consortium is generating tangible advancements in AI across multiple industries.


Through the MGAIC series, the SKT Newsroom highlights a range of collaboration cases between SKT and MIT.


This edition features Professor Yoon Kim of MIT’s Department of Electrical Engineering and Computer Science. His research team will introduce a new approach that enables AI agents to understand long contexts more accurately while significantly reducing computational cost.


We spoke with Professor Kim about the significance and background of this research, the motivation behind it, and his perspective on Korea’s sovereign AI strategy.


## Learning at test time to enable AI to solve difficult task


**Q1. Could you share the background and objectives of your research conducted in collaboration with SK Telecom?**
The research project (funded as part of MIT Generative AI Impact Consortium (MGAIC)) will focus on enhancing capabilities of LLMs through “test-time training” (TTT) and related techniques. The motivation comes from a shift in how the field thinks about compute. For most of the last several years, progress came primarily from scaling up training. More recently it’s become clear that spending additional compute at inference time also produces reliable gains. But nearly all of that inference-time compute is currently spent generating more tokens, while the model itself stays frozen. TTT asks a different question: what if some of that compute went into adapting the model to the problem in front of it?


**Q2. Test-Time Training (TTT) may be an unfamiliar concept to many readers. Could you explain how it works and why it matters?**
Test-time training (TTT) encompasses a broad class of techniques for training/adapting (parts of) a trained model to particular “test” instances in order to solve a task. Ordinarily, a language model is trained once and then deployed as a fixed system. Instead of answering with a fixed model, in TTT the system briefly trains itself on material relevant to the specific problem it has been given, and then answers. This is certainly not a panacea, and there are real costs to doing it. But it’s an interesting lens for thinking about how to convert compute at test time into model capability, which I think is one of the central questions in the field right now.


**Q3. What does it mean for an AI system to understand long contexts, and how could long-term memory capabilities shape the competitiveness of AI agents in both consumer and enterprise settings?**
It’s worth separating two things that get bundled together under “long context.” The first is retrieval: given a large body of text, can the model find the relevant piece? Current models do this reasonably well. The second is integration: can the model maintain a coherent understanding across the whole thing, for example noticing that a passage contained in page 400 contradicts page 30? That is much harder, and a model that can theoretically “process” a million tokens and still not have understood them in this second sense. In both consumer and enterprise settings, having systems that can understand long context in this sense would be crucial for developing useful AI agents.


**Q4. If commercialized, what are the most significant changes this research could bring for industries and end users?**
The project is aimed at fundamental algorithms rather than any specific commercial application, so I’d be cautious about predicting product impact. But if the underlying techniques work, this could enable LLMs to more efficiently tackle difficult tasks, particularly in reasoning-heavy domains, and could thus improve a host of LLM-based applications.


## A Strong Collaboration Model for Advancing Inference in Sovereign AI Foundation Models


**Q5. This project is being conducted in collaboration with SKT as part of MGAIC. What is the significance of this partnership, and how do you view SK Telecom’s AI strategy from a research perspective?**
SKT’s participation in MGAIC is significant in both directions. It lets MIT researchers ground their work by learning from industry teams building systems that are actually on a path to real-world deployment. And it lets industry researchers collaborate with academics who are better positioned to explore novel and foundational ideas without near-term constraints. Academia is better suited to exploration and industry to exploitation, and progress in AI increasingly requires both. Partnerships of this kind are, in my view, going to be crucial.


**Q6. SKT recently unveiled its new proprietary AI foundation model, A.X K2. How could this research enhance the competitiveness of SK Telecom’s foundation model?**
Techniques developed through the project could be of particular use in scaling “test-time compute” in LLMs trained by SKT. Concretely, researchers have found that it is possible to scale compute at inference to continually improve LLM performance across a range of tasks, in particular “reasoning”-related tasks (e.g., in answering math questions). TTT techniques developed through the proposal could increase the rate at which such test-time compute can be traded off for greater capabilities.


**Q7. As Korea prepares for the next evaluation of its Sovereign AI foundation Model Project, what key developments should the industry watch for, and what are your expectations for SK Telecom’s upcoming model?**
Korea has made significant progress in developing foundation models through the Sovereign AI Foundation Model project. That said, I think it would be accurate to say the models developed so far remain meaningfully behind frontier systems from the US and China. Thus, I think evaluators should pay close attention to how much the gap has closed on the core benchmarks that global model developers actually use. In my opinion this deserves more priority than assessing whether a participating model contains novel or flashy ideas. Catching up to an existing frontier is largely a matter of executing a known recipe extremely well, and that is the most important thing to verify at this stage.


**Q8. Which technology areas should countries pursuing sovereign AI, including Korea, prioritize to remain competitive?**
Insofar as AI has become crucial not only economically but for national security, it makes sense that sovereign nations want to build their own AI infrastructure and ecosystems. I do however worry that framing the goal around sovereignty pushes toward overcompetition rather than collaboration. With that caveat, in the current landscape where capabilities are driven largely by scaling relatively simple algorithms, I think what matters most is training researchers and engineers who can work with large systems at scale. Compute can be bought, and data centers can be built, but that kind of experience takes much longer to accumulate.


**Q9. How do you expect today’s Transformer-centric AI ecosystem to evolve in the future?**
It’s honestly hard to predict. It is possible that Transformers are “enough”, and refining/scaling the current recipe will lead to continual improvements. I am skeptical that this will be the case, and continue think that we will need fundamentally new ideas. One of these ideas may be architectural alternatives to the Transformer that could bring about a significant leap in efficiency/capabilities. For example, some recent open-weight frontier systems from several Chinese companies employ ideas that are (in a way) different from ordinary Transformers (although they are still “Transformer-like” in other ways). We should also not forget that Transformers are only a part of the current AI recipe, and other aspects of the recipe (e.g., data, objective, optimization, etc.) will also likely need much more work.


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3YrN2J6dzI3YkNpVS9pQURzNEN6NEg5QlNVWDIrU2hjd2d0dGRRK2tyUmk4eWRJN253eWlQanNFVXFtVldUSHdsaHpOVFpJVzNMNDdkVEFCbUVxbEptRWZDZDFDQ3hOTXJNQWU4WmJMVmp3VkQvUzlxVnF2eWF0UDBuUkkzTXpZYlE4RnZ1dWhKMVZweEs1U241cVhIUnQwZVUxVzZtdzdWY0MzM2NJN0hqbz0=


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3YrN2J6dzI3YkNpVS9pQURzNEN6NEg5QlNVWDIrU2hjd2d0dGRRK2tyUmk4eWRJN253eWlQanNFVXFtVldUSHdsaHpOVFpJVzNMNDdkVEFCbUVxbEptRWZDZDFDQ3hOTXJNQWU4WmJMVmp3VkQvUzlxVnF2eWF0UDBuUkkzTXpZS3Z0VHN0a210QlJXcUxibUtzQ090UT09


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3YrN2J6dzI3YkNpVS9pQURzNEN6NFBzQTdJdkVpTUF2a2VCbUtUaFVQdSt2L3ZDU3V0ZGd0anNJRnAyaWdGYm1GbEZiWnd5Qnd1Qy9SR0Y4bEd6NTR6TnAvcjhHTVJSQ3FEVzlpR3JjLzVtZk9hR0EzSk10T1RTQUlQSWRkWnh3ZWpWb0VOVXZId001WjUrOFpKNWZFZGJaa1N5M09PYkd1ZzlJYldJWkVnWT0=


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3YrN2J6dzI3YkNpVS9pQURzNEN6NFBzQTdJdkVpTUF2a2VCbUtUaFVQdSt2L3ZDU3V0ZGd0anNJRnAyaWdGYm1GbEZiWnd5Qnd1Qy9SR0Y4bEd6NTR6TnAvcjhHTVJSQ3FEVzlpR3JjLzVtZk9hR0EzSk10T1RTQUlQSWRkWnh3cU5RZzRTa0xJLzg2UW85SXJsbE5NUnpBNnllVkluT2dHN3BzdmxJMldETTRMZklwTzkvdzdxQVdvendITnQwanhIM0VYTytMa25PZWZMMmFsRWpsQlE9PQ==


/dl.php?key=THRJTjZRZzBqblN2K1p5WWxTRjJRT2tGb0dvb3VzSlhSTFRKYmpuT3hkLzBKOFd0M3hsVThNekdlNDlIMUVJZ3YrN2J6dzI3YkNpVS9pQURzNEN6NFBzQTdJdkVpTUF2a2VCbUtUaFVQdSt2L3ZDU3V0ZGd0anNJRnAyaWdGYm1GbEZiWnd5Qnd1Qy9SR0Y4bEd6NTR6TnAvcjhHTVJSQ3FEVzlpR3JjLzVtZk9hR0EzSk10T1RTQUlQSWRkWnh3QzFNY2gxeWkrZDgxcTBrdHpLOWJXQXg2d0VnY3YvdzdRNEg3Nm9DMGZ5c0NLeHJFWmdENjA4Yk5rTEZkRlprY2lsMk16RUE1YWdvWnRvRHQvNm4yZUE9PQ==


[HOME](https://news.sktelecom.com/en) >


[Insight](https://news.sktelecom.com/en/category/insight)


- [#AI](https://news.sktelecom.com/en/tag/ai)
- [#MGAIC](https://news.sktelecom.com/en/tag/mgaic)
- [#MIT](https://news.sktelecom.com/en/tag/mit)
- [#skt](https://news.sktelecom.com/en/tag/skt)
- [#SKTelecom](https://news.sktelecom.com/en/tag/sktelecom)
- [#Sovereign AI](https://news.sktelecom.com/en/tag/sovereign-ai)
