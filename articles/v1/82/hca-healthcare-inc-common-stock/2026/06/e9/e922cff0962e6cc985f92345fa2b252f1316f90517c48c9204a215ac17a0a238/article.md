---
schema_version: "1.0.0"
document_id: "e922cff0962e6cc985f92345fa2b252f1316f90517c48c9204a215ac17a0a238"
company_key: "hca-healthcare-inc-common-stock"
company: "HCA Healthcare Inc."
source_id: "hca-healthcare-inc-common-stock-rss-adadc5499810"
canonical_url: "https://hcahealthcaretoday.com/2026/06/12/mis-quarterly-executive-insights-from-leading-ai-transformation-at-hca-healthcare/"
published_at: "2026-06-12T20:22:32+00:00"
first_seen_at: "2026-07-20T03:32:36.294382+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:7f35201c3bda7de9833561408a0a2328d9423d210aeb120ada40e6960a56ed32"
---

# MIS Quarterly Executive: Insights from Leading AI Transformation at HCA Healthcare

> Dr. Michael Schlosser, MD, MBA, recently joined George Westerman of the MIT Sloan School of Management to discuss how HCA Healthcare is approaching AI innovation, scaling new technologies and driving transformation across the organization.
>
>
> This below article was originally published in *MIS Quarterly Executive, Volume 25, Issue 2 (2026)* , and is republished here with permission. Read and download the full article on *[MIS Quarterly Executive](https://aisel.aisnet.org/misqe/vol25/iss2/2/) .*


Leading large-scale AI-driven transformation is never easy, especially in a highly regulated industry like healthcare. But the Digital Transformation and Innovation group at HCA Healthcare is delivering a steady drumbeat of innovative value for employees and patients. In this interview, Dr. Michael Schlosser, who leads enterprise-wide digital transformation and innovation for HCA Healthcare, describes the mechanisms he and his team use to engage top leaders, rank-and-file employees and even regulators to produce transformative AI-powered innovations quickly and safely.


## Introductory comments


HCA Healthcare is one of the leading providers of healthcare services in the United States. As a comprehensive, patient-focused learning health system, the organization operates 189 hospitals and approximately 2,600 ambulatory sites of care — including surgery centers, freestanding emergency rooms, urgent care clinics and physician practices — across 19 U.S. states and in the United Kingdom. Supported by a workforce of over 320,000 colleagues and a network of roughly 44,000 active and affiliated physicians, HCA Healthcare uses the scale of its operations, which include roughly 47 million annual patient encounters, to innovate in areas like data-driven sepsis treatment, clinical research and graduate medical education. Dr. Michael Schlosser, MD, MBA, is the senior vice president and chief transformation officer at HCA Healthcare. He leads the enterprise’s Digital Transformation & Innovation (DT&I) function, driving large-scale change across clinical, operational and administrative domains. He also leads HCA Healthcare’s Clinical Informatics organization and Data Office, overseeing enterprise strategy for electronic health records, AI and data platforms.


## The interview


#### The role of organizational size


**George Westerman:** HCA Healthcare is a giant organization in a highly regulated sector. Large organizations can often be inertial, and regulation can create even more inertia. How is HCA Healthcare’s size a benefit for innovation?


Dr. Mike Schlosser, HCA Healthcare Senior Vice President and Chief Transformation Officer


**Dr. Michael Schlosser:** Big organizations can certainly have a lot of inertia, and fear of regulatory actions can make it worse. But it doesn’t have to be that way. Since our founding in 1968, HCA Healthcare has prioritized and embraced innovation to constantly address the needs of those we serve and to support our clinicians. Our scale gives us almost a superpower for innovation. With 189 hospitals and approximately 2,600 sites of care, we have vast amounts of data that can help us understand how patients flow through our healthcare system, informing insights on treatments and outcomes. These insights from 47 million annual patient encounters support our digital and AI strategy.


We have myriad care settings where we can test ideas. We have Innovation Hubs where we can evaluate innovative models of care and technology solutions. And then, once we find something that works, we have an operational infrastructure to responsibly scale those innovations across the entire organization. That’s not just technical infrastructure. We also have experienced teams at the regional and hospital level who are accustomed to implementing new solutions. So, when I go to a hospital, I have leaders I can tap into. They know what I need from them in helping to lead this change. And the way we manage the process produces a lot of clear evidence that can help support appropriate regulatory considerations.


To me, that is the most exciting part of HCA Healthcare. I see a lot of innovation in the broader healthcare space that is challenged to move beyond the pilot phase. But at HCA Healthcare, we have the ability to pilot something, see that it works and then tap into our operational infrastructure to understand how these solutions perform in real-world settings. This allows us to help advance the industry in going from great ideas and great technology to meaningfully impacting patient care at scale.


#### Improving nurses’ jobs with AI-powered innovation


**George Westerman:** You’ve talked publicly about some of the AI-powered innovations you’ve launched: ambient AI to document doctor visits, optimizing medical device inventory, maternal-fetal health monitoring, early sepsis detection and others. I found one of your innovations especially fascinating: Nurse Handoff. Can you tell us more about that?


**Dr. Michael Schlosser:** The idea for Nurse Handoff came directly from our nurses. They have identified it as a key pain point. The process of handing the care of a patient off to the next incoming shift and ensuring the next shift knows everything they need to know — that nothing falls through the cracks — is a critical, high-impact time in healthcare delivery.


Heather Steele, digital transformation and innovation operations manager, discusses a digital tool designed to support our care teams with Chesaney Leach, director of the medical-surgical unit at HCA Healthcare’s TriStar Hendersonville Medical Center.


The tools at their disposal have not always fully supported this process. Historically, much of the responsibility has rested on nurses to manage the handoff. After a long shift, each nurse needs to pick through the chart and think of their experience with each patient and their conversations with doctors. They need to decide what nuggets of information are most important and then compile them into a clear summary for the next care team. The next team needs to absorb that information quickly so that they can give each patient the care they need.


Attempts in the past to have the electronic health record (EHR) system generate summaries have missed the mark; they provided either too little or too much information to be effective. What’s needed is to train software to think like a nurse: to understand what are those nuggets, what are the things that matter and what are the things that don’t? Large language models (LLMs) were among the first technologies that showed promise in supporting this capability. This isn’t just summarization; it’s about understanding clinical context and thinking like a care team member.


Working alongside bedside nurses, it took about a year to refine the model structure to do this well. It involved extensive input and validation from our care teams — who do handoffs every day. We’d produce an output and the nurses would provide detailed feedback — the good, the bad and the ugly. Then we would go back and fine-tune the architecture, adjust the retrieval augmented generation (RAG), change the type of model we’re using and do it again and again until we got the metrics we were looking for.


We were measuring things like the cohesiveness of the summary, accuracy, conciseness and the usefulness. We kept iterating until those metrics reached consistently strong performance thresholds into the high 90s. Then we focused on usability. The nurses didn’t want just a long string of text. They needed to interact with it in a way that was easy to consume.


We have the solution running in eight hospitals and plan to expand it more broadly in the future. We believe this approach has the potential to enhance the safety and efficiency of the handoff process and better support our care teams.


#### Dealing with AI non-determinism and hallucinations


**George Westerman:** High 90s in accuracy is amazing, but what do you do about the other few percent where it’s not quite right? When you’re talking about patient care, can you afford to be wrong?


**Dr. Michael Schlosser:** Human-in-the-loop is a really important part of the process here. To me that will always be the case for any LLM-driven solution. These technologies are designed to support, not replace, clinical judgement. It will never be 100%, so you have to build systems that make it easy for the end user to be vigilant and to review and validate outputs.


A big part of that is providing the right education, training and guidance so nurses understand their role in overseeing what this model is doing. They know the tools are designed to be highly reliable, but they also understand the importance of remaining attentive. It’s not enough to explain why LLMs can make mistakes — we also need to design the software so it’s really easy to give feedback. For example, clicking on a word to flag that it may be inaccurate or unclear.


That feedback and data are immediately captured and reviewed to support ongoing improvement. Right now, that aggregation and analysis still includes human review, but our data scientists are building automation to aggregate that information and pass it back to the model in an automated way.


#### The importance of shared understanding in leading transformation


**George Westerman:** Your background is fascinating. You’re a surgeon, but you’ve also worked as a chief medical officer (CMO) and as a regulator, and you’ve even gained experience working with medical devices. You’ve done extensive academic training in medicine, and in engineering as an undergraduate, and even earned an MBA. How does that help you in leading transformation?


**Dr. Michael Schlosser:** Being a transformation leader was never part of my long-term plan, but I think my background set me up perfectly for this role. I got to experience all the different aspects of the healthcare delivery system, which has helped me better understand the complexities involved in this kind of innovation. When you look at just one part of the healthcare world, without understanding how decisions connect to all of the other complexity that is healthcare, you can miss the mark. You can deliver a solution that works at a single point but doesn’t really connect with the rest of the way healthcare is delivered.


**George Westerman:** Are there times when being a surgeon or a former CMO gives you a kind of “secret handshake” that other people might not have?


**Dr. Michael Schlosser:** There is a credibility aspect in the clinical space. People need to know that I understand their worldview. People will look to me and say: “You’re not just a finance person who’s trying to improve the bottom line of the company. We can trust that you’ve thought about the doctors and the patients and the nurses in this.” So, if my background gives me any kind of “superpower,” it’s that people know I understand their worldview.


#### Organizing for AI transformation


**George Westerman:** That takes me to the structure of your team. What does the Digital Transformation and Innovation group look like?


**Dr. Michael Schlosser:** We have three teams. The first is the product team that focuses on solutions. Each product is really focused on solving a specific problem, and this team helps figure out the best solution to that problem. They are people with clinical or business backgrounds as well as technical expertise, such as data scientists and AI engineers.


Then there’s the implementation team. They do change management, training, education and on-the-ground implementation. These are the people who figure out how to take a good idea that works in one unit and scale it across an entire organization. They’re involved from the very beginning. So, as we’re finding solutions, we start thinking about the scaling considerations from day one. It’s an approach that has been very successful.


The third team is our data organization. All AI strategy requires a data strategy. The AI is only as good as your data. I was intentional about standing up a chief data officer and a Data Office and placing it within the transformation organization because we depend on the quality of our data. This includes our Chief AI Office, which has the AI experts and engineers.


Then we drew two lines. On one side of the first line is IT, which has software engineering, platforms, cloud and cybersecurity — basically all of the infrastructure and systems that we rely on to do this work. HCA Healthcare’s CIO, Chad Wasserman, and I work hand in hand on all of this. It’s an incredibly important partnership.


The other line is with the business. All of my product teams have business unit sponsors who bring us ideas, provide context and make sure that the solutions are really going to meet the needs. They help us measure the value and understand the impact the solutions will have. Each sponsor has a day job running part of HCA Healthcare. But they spend a little bit of their time with us, making sure that we’re focused on the right solutions. They also have a very important role to play when it comes to scaling. Because they’re there for the whole journey of developing the solution, they’re well-positioned to help us drive the adoption and measure the impact.


#### Project selection and value tracking


**George Westerman:** Let’s talk about your innovation process. In a company this size, with so many different moving parts and different interests involved, how do you decide what projects to pursue?


**Dr. Michael Schlosser:** To me, this is the million-dollar question. There is a lot of low hanging fruit in healthcare; we have a lot of opportunities to be better. So where do you start? We think of it as a bottom-up and top-down approach.


The top-down is important because these things take time, they take energy and they’re usually expensive. If the leadership of the organization doesn’t believe that we’re focused on the most important strategic priorities, it’s hard for our efforts to get traction. So, we have a governance committee that conducts quarterly business reviews. The committee includes the most senior leaders across the organization, including our CEO and our CFO. It makes sure we are aligned on the biggest strategic priorities for HCA Healthcare. For example, supporting our healthcare workforce — ensuring we have the right people trained and developing future leaders — and aligning our innovation agenda to that strategy. Another strategic priority is ensuring that we are thoughtful stewards of our resources. As the healthcare landscape continues to evolve, how do we focus on using resources efficiently while maintaining high quality care? This governance structure helps us align with the key priorities of the entire organization.


The bottom-up comes from many different inputs. Three hospitals are designated as Innovation Hubs. They get to live in the innovation space in addition to providing everyday care delivery. They’re a valuable source of ideas. It’s not that the other 186 hospitals don’t have strong ideas as well, but knowing how to turn an idea into something that we can actually implement is an art that takes some learning. The Innovation Hubs tend to be very good at recognizing what ideas can really take root and go fast and far.


All our business unit sponsors also have structures they use to get feedback. For example, our nursing organization has forums where they’re constantly engaging nurse leaders in the field. They’re hearing about what their pain points are and what they’re seeing on a day-to-day basis.


We’ve created a structured process to filter all that information and ensure we’re focusing on the right things on a quarterly basis. It’s not like we change direction every quarter — these are mostly refinements. But sometimes an entirely new idea will lead us to stand up a new team to pursue it.


**George Westerman:** As you do this filtering process for ideas, how much does the financial business case — the ROI or potential value — fit into the process? And how do you estimate that?


**Dr. Michael Schlosser:** We have a small but mighty value-tracking team with biostatisticians and others who are deeply skilled in data analysis. We attach that team to every idea. Even in the discovery phase, they help the team do some “napkin math” about the impact of an idea. We call it value tracking because it’s not just financial — it includes patient safety, quality, experience and operations. We are interested in measuring all those impacts and improving all those areas. Regardless of what the kind of value is, this team can help size the opportunity.


With 189 hospitals, we often see variation in how challenges are addressed. If one hospital is doing really well with a new solution, and another is not seeing the same results with a traditional approach, that is useful input for the value-tracking team. It helps us understand what broader impact could look like if performance is improved across the system.


Showing value is critically important for maintaining momentum and funding. If you can’t consistently demonstrate how an effort will improve outcomes that HCA Healthcare prioritizes, then it’s difficult to sustain support. And it should be hard because we need to ensure we are investing in solutions that create value for our patients, our care teams and our organization.


That’s easy for me to say, but it’s hard to do. Getting to the data that describes these problems is often not easy, but our Data Office is helping us get better. Sometimes the data tells a different story than expected — we may believe an idea will have a significant impact, but it doesn’t show up that way in the data. In those cases, we have to reassess and adjust our approach, rather than continuing forward without evidence because we’ve fallen in love with the solution. That takes discipline.


#### Ensuring responsible AI use


**George Westerman:** Beyond considering the potential value of an innovation, what practical guardrails do you consider to ensure safe and responsible outcomes?


**Dr. Michael Schlosser:** When we started this journey a couple of years ago, we established our Responsible AI Committee. It sits at the senior level of the organization, and includes me, our chief legal officer, chief privacy officer, ethics and compliance leaders, and others. We created a set of guiding principles for what responsible use of AI would look like. They include areas such as transparency, privacy and security, and robustness.


Grace Vero, RN, women & children’s services at HCA Healthcare’s TriStar Hendersonville Medical Center, provides care and support to a patient while monitoring maternal and fetal well-being.


And then there’s the topic of bias. A maternal fetal wellbeing model is not going to be useful for male patients—that’s not bias; it’s an intentional design decision. But you always have to ask, going in, who is the population that may be exposed to this and does our training dataset adequately represent that entire population? In the maternal-fetal health example, we were working with GE Healthcare on a model, and the Food and Drug Administration (FDA) asked some good follow-up questions. Was the model trained well enough to be able to handle the smaller subpopulations we may see less frequently? It was good feedback that caused us to go back and do additional work to ensure they were all handled well.


Our governance process reviews each application using our framework. Before we can test, turn on or even study an AI solution, it has to go through this governance process where we ask questions to ensure we’ve thought about all of the safety, security, privacy and robustness issues that could come up. And then we design mitigating strategies. There’s always some kind of risk, but how do we ensure that we’re protecting ourselves and our patients from that risk? We answer those questions before we even start the process.


**George Westerman:** After you launch an application, do you go back periodically to ensure it’s still following your framework guidelines?


**Dr. Michael Schlosser:** Continuous governance is core to our governance model — it’s not a one-time check. Models drift, populations evolve and risk profiles change. Usually, there’s a feedback loop where we’re collecting data and working with a QA team. This isn’t “fire and forget”; it’s lifecycle management to ensure performance, fairness and compliance over time.


#### Scaling AI pilots


**George Westerman:** You mentioned earlier that one of HCA Healthcare’s strengths is that, once you have a pilot that’s working, you’re able to scale it really well. For many organizations, this is a real challenge. Especially with AI, moving from the pilot phase to scaled adoption is very difficult. What are some key lessons people can learn from how you do that?


**Dr. Michael Schlosser:** Change management is often the number one failure point. You can sometimes skip it in a pilot because everyone’s excited. You have this contained environment, the AI is cool and flashy, and so everyone leans in. You can forget the fact that, at scale, adoption can vary significantly.


An example is ambient documentation, where an AI can listen to a doctor/patient visit and write the first draft of notes for the medical record. You have these early adopters who say: “This is amazing — it saves me all this time, and it’s so cool.” But you have thousands of doctors who like their current workflows. They’ve developed patterns that work for them. For them, this represents something new to learn and adapt to, which can introduce additional complexity alongside caring for patients. A new process is an emotional and psychological effort for them.


We learned that you have to start change management from the very beginning. Even with your early adopters, you need change management teams to ask questions like: What is the change impact here? Why might adoption be challenging? What might we encounter as we scale?


The lessons involve both products and people. First, we learned that you sometimes need to refine the product or interface to make adoption easier. And sometimes it’s about how you train or motivate people as you roll a solution out. You need to start that from the very beginning, and then you never stop. When you go to your first wave of rollout and you don’t see the adoption rate that you expected, you study that and you learn from it. What did we miss? What can we do better? And then you adjust and apply those learnings in the next phase.


The second lesson is that these AI solutions are inherently iterative. Forget the idea that you turn the solution on and move on. AI models are living, breathing things. You have to adapt your implementation plan to realize that it’s probably never going to be done. You’ll always be learning and evolving. That’s actually positive, because AI is adaptable. The product is improving as you roll it out. It learns and gets better as more people use it. But your infrastructure has to support that. If your rollout method is designed for something static, it will struggle with AI models.


The third lesson is to fail fast. One way we fail is when we fail to recognize that the product is not actually working. We want it to be the solution so badly that we keep pressing forward when the data in front of us is telling us it’s time to stop and go in a different direction. It’s important to be willing to adjust direction quickly when needed.


And a final point is the operational infrastructure that I mentioned earlier: The people who know how to be part of the change process at all levels. They understand that, by working with us throughout the process, we can develop solutions that work for their teams, not just for leadership or technical teams. Their partnership makes the adoption processes smoother and better. That’s an additional benefit that we have in HCA Healthcare that others might need to build for more effective scaling of new tools.


#### AI and the future of healthcare


**George Westerman:** As AI becomes more mainstream, how do you see healthcare changing in the next 5-10 years for HCA Healthcare and its patients?


**Dr. Michael Schlosser:** I’m an AI optimist. I’m incredibly bullish that the next 5-10 years are going to be an amazing time in healthcare. If we point our resources and our energy at solving the right problems, we have the opportunity to create a healthcare system where doctors and nurses get to spend more time focused on patients.


For example, some estimates suggest that between 30-50% of clinicians’ time is now spent on administrative burden: documentation, dealing with payers or other processes not directly adding value to the patient. We might have a 50% bigger workforce than we think we have, if only we could reduce that administrative burden. If we focus our energy on letting doctors be doctors and nurses be nurses, we may find the capacity in our system to care for more patients. This can be achieved through AI helping them get access to data and insights — not just journal articles and publications but also patterns and insights derived from our massive data sets.


It’s like a superpower where machine intelligence and human intelligence partner together to make every decision better than either one could have done on their own in the past. If we can bring that future to life, patients could benefit from a better experience and more timely care. We can move people through their healthcare journeys in a more effective way, which may help expand access for more patients who need care.


In parallel, patients will have access to tools so they can engage with healthcare in a better way. They can become more educated and more informed. Over time, emerging and AI-enabled tools will become another touchpoint for patients to access healthcare information. Ideally, this will support healthier behaviors and more informed decision-making, while the healthcare system will remain an essential source of care and guidance.


## Insights from HCA Healthcare for innovating with AI


Dr. Schlosser and his colleagues have created a disciplined system for AI innovation at HCA Healthcare. This system uses rigorous methods, rapid iteration, strong partnership and evidence-based decision-making to support innovation in a high-stakes environment where precision and safety are critical. It engages people at all levels to identify, create and scale innovations using a collaborative evidence-based process. In so doing, HCA Healthcare’s DT&I group has helped evolve how the organization approaches innovation across both operational and patient-care settings.


The following six insights gained from Dr. Schlosser’s experience can be useful for any company seeking to innovate with AI, even in highly regulated environments.


#### 1. Start by choosing problems that are worth solving


Healthcare organizations face many opportunities for improvement, making project selection critical. As Dr. Schlosser notes, “There is a lot of low-hanging fruit in healthcare; we have a lot of opportunities to be better. So where do you start?” Problems need to be big enough to be worth solving and implementable at scale. HCA Healthcare screens ideas with employees from Innovation Hub hospitals who excel at “recognizing what ideas can really take root and go fast and go far.” The value-tracking team pinpoints potential benefits, and the governance committee ensures that projects align with major strategic priorities. As Dr. Schlosser explains, “If the leadership of the organization doesn’t believe that we’re focused on big strategic problems, it’s hard for our efforts to get traction.”


#### 2. Focus on adoption and scaling from the beginning


The Expanse team conducts a Wave 1 training session at HCA Houston Healthcare Northwest Hospital, preparing colleagues to use HCA Healthcare’s modern electronic health record system.


One of HCA Healthcare’s most distinctive practices is embedding adoption and scalability into the innovation process early: “As we’re finding solutions, we start thinking about the scaling challenges from day one.” This approach proved essential for solutions like ambient documentation, where enthusiastic early adopters masked the reality that thousands of other doctors might not want to change their established work patterns. HCA Healthcare engages with eventual adopters throughout every stage of the innovation process so the designs fit with their workflows. Then it proves the solutions work in a few hospitals before scaling more broadly. Later adopters who may be reticent can gain encouragement from seeing people like them using the solutions, rather than from a technical person who may not know their jobs. This systematic consideration of adoption throughout the innovation process helps avoid the “death by pilot” that many other healthcare organizations experience.


#### 3. Who leads the innovation is less important than how they lead


While Dr. Schlosser’s background as a surgeon, CMO and regulator provides valuable context, he emphasizes that his “superpower” isn’t the credentials themselves but rather the credibility and understanding that background affords: “People need to know that I understand their worldview.” His varied experience enables him to “connect solutions with the broader realities of how healthcare is delivered.” But the real lesson is the need to build knowledge and trust across the organization, whether gained through diverse career experiences or through intentional learning and relationship-building. This enables transformation leaders to introduce innovations that work for the entire organization, not just for technical or administrative functions.


#### 4. Discipline is essential


HCA Healthcare’s success with AI is rooted in discipline throughout the development process. After the Nurse Handoff idea passed the company’s rigorous investment process, the team spent a year refining the Nurse Handoff model through repeated iterations until performance metrics reached acceptable levels. This evidence-based discipline includes knowing when to stop: “Sometimes the data starts to tell the wrong story. …You have to say, ‘We’re not seeing it, so we need to pivot.’” Value tracking happens from the discovery phase onward, with a specialized team helping to size opportunities and measure actual impact. This prevents teams from “falling in love with the solution” without sufficient evidence and ensures that resources focus on initiatives that deliver meaningful value.


#### 5. Focus on value and performance


HCA Healthcare’s “small but mighty” value-tracking team embeds value measurement into every innovation from conception through scaling. Crucially, value is not purely financial; it also includes patient safety, quality and experience. HCA Healthcare’s scale enables sophisticated value analysis that compares high- to low-performing units and adopters to non-adopters. This comprehensive focus on value maintains momentum and funding by consistently demonstrating impact while also providing discipline. “If you can’t consistently tell the value story of how this is going to improve an outcome that HCA Healthcare cares about, then it’s hard to get support,” Dr. Schlosser said. “And it should be hard because we shouldn’t be spending money on things that aren’t creating value.”


#### 6. Find ways to make regulation work for you


Rather than viewing regulation as an inhibitor to innovation, HCA Healthcare treats it as a valuable input that strengthens the innovation process. The regulatory process pushes teams to be more thorough, transparent and rigorous in their validation — all qualities that lead to better, more trustworthy AI solutions. The maternal-fetal health monitoring example illustrates this perspective. When the FDA asked detailed questions about whether the model adequately represented important patient subpopulations, HCA Healthcare saw it as “good feedback that caused us to do additional work and ensure they were all handled well.” HCA Healthcare’s Responsible AI Committee ensures that projects at all stages consider not only health and safety risks but also risks to adoption and scalability. This encourages teams to consider mitigation strategies early and establish feedback loops for quality assurance, helping HCA Healthcare move forward with appropriate rigor and confidence.


## Concluding comments


HCA Healthcare’s approach to AI transformation demonstrates that success at scale requires more than advanced technology. It demands disciplined processes, strategic focus and deep organizational commitment. By treating the company’s size as an asset and a source of strength, HCA Healthcare leaders built an organizational infrastructure that can pilot innovations quickly, validate them rigorously and scale them systematically across 189 hospitals. Its success stems from fundamental practices that any organization can adopt: starting with problems worth solving, embedding change management from day one, combining top-down strategy with bottom-up insights, maintaining strict discipline around value measurement, and viewing governance and regulation as tools for building better solutions rather than obstacles to overcome. Together, these practices help ensure that innovations work across the system. As Dr. Schlosser envisions a future where “machine intelligence and human intelligence partner together to make every decision better than either one could have done on their own,” HCA Healthcare’s transformation journey offers a practical roadmap for healthcare organizations — and indeed any large enterprise — seeking to harness AI’s potential while managing its risks responsibly.


## About the authors


#### Dr. Michael Schlosser


Dr. Michael Schlosser, MD, MBA, is senior vice president and chief transformation officer at HCA Healthcare. He leads the enterprise’s Digital Transformation & Innovation (DT&I) function, driving large-scale change across clinical, operational and administrative domains. In this role, he oversees eight transformation domains and more than 20 product teams developing solutions that span supply chain, revenue cycle, nursing care delivery, physician documentation and beyond. He also leads HCA Healthcare’s Clinical Informatics organization, with the chief health informatics officer reporting to him, and directs the enterprise strategy for electronic health records (EHRs), AI and data platforms.


#### George Westerman


George Westerman is a principal research scientist and senior lecturer at the Massachusetts Institute of Technology’s Sloan School of Management. His research and teaching help executives understand the transformative potential of new technologies and the steps they can take to build innovative capability in their organizations. A pioneer in the study of digital transformation, he has co-authored three award-winning books and numerous journal articles for academic and leadership audiences. His most recent work helps clarify the complex challenges of driving AI-powered transformation in large organizations.


> Download the full article[here.](https://aisel.aisnet.org/misqe/vol25/iss2/2/)
