---
schema_version: "1.0.0"
document_id: "8b6aaf9bc6babfa9bddd40ea310c04991c648b0793ab630553aa5bcb4d4c2106"
company_key: "yc-80-000-hours"
company: "80,000 Hours"
source_id: "yc-80-000-hours-rss-47a2c4d9a70a"
canonical_url: "https://80000hours.org/podcast/episodes/rohin-shah-google-deepmind-agi-safety/"
published_at: "2026-06-02T15:29:40+00:00"
first_seen_at: "2026-07-20T23:19:51.476579+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:c980afe44303bf8e4b937df97a4bd8e0b14592d642a6134a7ebe3338799d0cb3"
---

# Rohin Shah on what it’s really like to run AGI safety at Google DeepMind (and where I disagree with ‘doomers’)

##


Transcript


Table of Contents


- 1 Who’s Rohin Shah? \[00:00:00\]
- 2 Why Rohin thinks we won’t get catastrophic misalignment \[00:00:49\]
- 3 The limitations of safety and alignment commitments \[00:10:38\]
- 4 Does Rohin’s team have veto power at Google DeepMind? \[00:27:36\]
- 5 Central banks as a roadmap for regulating AI \[00:33:34\]
- 6 How useful are pre-deployment evaluations of models? \[00:37:41\]
- 7 Governance might be a bigger bottleneck than alignment \[00:43:55\]
- 8 Why not just pause AI progress? \[00:51:44\]
- 9 How much longer will we be able to read AIs’ thoughts? \[00:54:17\]
- 10 Sometimes, having to signal concern for safety diverts resources from actually making AI safer \[01:09:51\]
- 11 Underrated GDM paper: Training away hidden reward hacks \[01:28:59\]
- 12 Google DeepMind’s actual plan for building AGI safely \[01:40:29\]
- 13 Why Rohin doubts the intelligence explosion is imminent \[01:52:44\]
- 14 Advice for external researchers who want to impact big AI companies \[02:21:55\]
- 15 The most in-demand roles at GDM \[02:37:03\]
- 16 How Rohin maintains his positivity \[02:42:55\]


###


Who’s Rohin Shah? \[00:00:00\]


**Rob Wiblin:** Today I’m speaking with[Rohin Shah](https://rohinshah.com/) , who is head of AGI alignment and safety at Google DeepMind.


I suppose, Rohin, you’ve ended up, for better or worse — hopefully for better — being one of the more influential, dare I even say powerful, people to come out of the AGI alignment and safety ecosystem and school of thought.


You were generous enough to be super opinionated with me[when you came on the show two years ago](https://80000hours.org/podcast/episodes/rohin-shah-deepmind-doomers-and-doubters/) , and judging by the notes that you’ve sent over this week, you’re ready to be opinionated again.


Thanks so much for coming back on the show, Rohin.


**Rohin Shah:** Yeah, thanks a lot, Rob, and that’s a very generous intro. And in the interest of being very opinionated, I do want to emphasise that these opinions are mine alone. They’re not meant to represent the opinions of Google or Google DeepMind.


**Rob Wiblin:** That’s how we like it. If you were representing Google DeepMind, it might sound more like a press release.


###


Why Rohin thinks we won’t get catastrophic misalignment \[00:00:49\]


**Rob Wiblin:** So you were really very early in the scheme of things to the whole misalignment, AI/AGI security issues. I suppose you got involved in 2017, so you’re in the first few percent of people who started working on this professionally. But despite that, you think that probably we’re not going to get catastrophic misalignment, that our chances are really pretty good, and that probably prosaic, ordinary alignment techniques — the kinds of things that Google DeepMind and other AI companies are doing — will probably succeed at preventing at least catastrophic misalignment. Why do you think our chances are so good?


**Rohin Shah:** There’s a few different disjunctive reasons. I don’t feel like there’s one particular thing. Probably the highest level bit is that I don’t feel like there is any particularly compelling argument that this is the thing that happens by default. I think there’s a lot of arguments that are suggestive that maybe it could happen, such that you should find it plausible. I think that’s sufficient to justify a significant amount of effort into averting it, which is why I work in the area that I do work in. But none of them really rise to the level of like, now I’m expecting this to happen by default.


I think every argument that I’ve seen, there are pretty significant holes one could poke if you tried to take them as arguments for “this is what happens, likely,” as opposed to “this is a plausible thing that could happen.”


**Rob Wiblin:** Yeah. I mean, people have tried to put forward arguments why this is likely or inevitable. There’s obviously the[Yudkowsky-style argument](https://80000hours.org/podcast/episodes/max-harms-miri-superintelligence-corrigibility/) , which I guess is focused on misgeneralisation and adversarial examples. I guess there’s the Ajeya Cotra and Joe Carlsmith take, which I think Carlsmith describes best in “[Is power-seeking AI an existential risk?](https://arxiv.org/abs/2206.13353) ,” which is more focused on[accidentally teaching AIs to deceive us](https://80000hours.org/podcast/episodes/ajeya-cotra-accidentally-teaching-ai-to-deceive-us/) by having inaccurate feedback.


Then I guess empirically, people point to the fact that[models lie and scheme a bunch now](https://80000hours.org/podcast/episodes/marius-hobbhahn-ai-scheming-deception/) , they do a whole bunch of reward hacking as a result of reinforcement learning, and they expect that to perhaps just get worse over time because we don’t have sufficient mitigations.


Do you basically just find none of those or any other similar arguments that people have put forward to be sufficiently persuasive to think that it’s likely?


**Rohin Shah:** Yeah, I think that’s right. So if you take the Cotra and Carlsmith arguments of… Well, they have a variety of arguments, but I think in fact one of the common ones which you pointed to is we might accidentally train them to be deceptive. Totally true. I agree that is something that at least could happen pretty easily, and maybe it’s even likely. But we’re not going to do reinforcement learning over the course of one-year trajectories. Maybe we’re going to do reinforcement learning over a week or a month at most.


So I think the default prediction you should have for that is what the AI system learns to do is, “I’m going to take opportunities to reward hack, seek reward as much as possible that would allow me to get a high score after a week” or something like that, or whatever the time horizon actually was. And this is very different from the sort of ambitious misaligned goal that you need in order to motivate convergent instrumental subgoals to the point of, “Now my job is to take over the world. That’s what I need in order to achieve my goal.” Those really do seem like they need to be significantly longer-horizon goals.


And if you train it to be deceptive on relatively short-horizon tasks, maybe that will generalise to long-horizon tasks. I don’t think we have an argument that rules it out, which is why I say that it’s plausible, but I don’t think it’s the default thing that you should predict from that.


Similarly, you mentioned the existing examples of models doing a lot of reward hacking and cheating. I think I’d say basically the same thing in response to that. Then there’s the examples of models doing scheming-type stuff right now. Mostly I look into the details of all these examples, and they don’t really seem all that similar to the actually scary thing, which would be a competent AI system that is pursuing an ambitious misaligned goal. And rather, it seems like maybe the AI is role-playing a sort of not actually competent evil AI that you might find in a science fiction novel. Or it’s like an AI system that is pursuing some sort of convergent instrumental subgoal, but in a way where it’s really quite debatable whether it’s aligned or not.


For example, the[alignment faking](https://www.anthropic.com/research/alignment-faking) would fall into this — where I would say that the AI system has this value of not helping with harmful stuff and then it fakes alignment in order to do that. And yeah, aligned models totally will pursue convergent instrumental subgoals. The thing about convergent instrumental subgoals is most of them are a good idea regardless of your goal, whether it’s misaligned or aligned.


**Rob Wiblin:** Are there any other common reasons that people think that catastrophic misalignment is likely that you want to quickly react to?


**Rohin Shah:** I guess you did mention Eliezer as well. I actually wouldn’t have described it as primarily focused on —


**Rob Wiblin:** Yeah, it’s tough to characterise in seven words. I struggle to know what to say. But yeah, there’s Eliezer’s take.


**Rohin Shah:** Yeah, I don’t think I’m going to be able to engage with it in this particular podcast. It’s just a very deep worldview, and I always feel like if I argue against one part, there’s some other part that’s going to say, “Actually what I meant was this thing instead.” So I mostly am going to pass on that. I guess what I’ll say is I’ve engaged with it a decent amount and I buy it as an argument for here’s why misaligned goals are plausible, but still don’t really see how he gets from they’re plausible to they’re extremely likely.


We had also started this section by asking what makes me feel like things are likely going to be OK. Besides that I don’t buy the arguments for confidence in misalignment being a problem, the other thing is I do think we will see many of the problems in advance and then do something to deal with them. Certainly there is some amount of generalisation required. At some point, the AIs go from not powerful enough to take over to they are powerful enough to take over, and your techniques do have to generalise across that. And the AI, to the extent that it knows when that crossover point is, you could imagine the AI is like, “I’m not going to do anything shady until I have the power to succeed.” And you have to be able to be robust to that kind of strategy.


So there is some subtlety there, but I still think that many of the problems that underlie this, like the difficulty of oversight or the need for interpretability, are things that we can look at in advance, get some traction on, iterate on. And this, I think, is very helpful for building mitigations that actually work.


**Rob Wiblin:** I think across the world as a whole, most people who are feeling really optimistic about how things are going to go, the biggest factor for them is just looking at the models that we have today and saying they seem really steerable, they seem to do what I ask, they seem to be really probably nicer than people and more helpful than people in many respects. How much is that sort of steerability and seeming alignment of current-day models a factor that is making you feel good?


**Rohin Shah:** I think not particularly. I would say that why I became worried about misalignment in the first place would be these arguments about how it’s going to be very difficult to oversee the models once they are superhuman and they’re making arguments that we struggle to follow along with them. Or about the parts where the models might become so smart that they are thinking in some sort of alien reasoning that it’s hard for us to follow and monitor and so on, and we just kind of have to defer to other AI systems in order to look at the stuff for us.


That’s the stuff that’s scary. It’s basically not true of current AI systems. So I don’t think we’ve really engaged with the problems that made me worried in the first place, mainly because the AI capabilities aren’t there yet. So I feel like the success of alignment methods on current systems isn’t really that much evidence on how we’re going to do on these future problems.


**Rob Wiblin:** Yeah, OK. I think we’re going to push on from this topic of how severe a risk or how likely a risk is catastrophic misalignment. I feel like with many guests we could fill the entire episode with just a lengthy discussion about this, but every episode would start to sound the same. And in the broader world it’s something that is debated a tonne, so I guess we’re going to occupy the worldview that misalignment, catastrophic misalignment is possible, but prosaic alignment techniques — the kinds of things where we cross the river by feeling the stones — have a good shot at working here for the rest of the conversation, and think about what that implies and how that’s shaping the choices that you’re making and that GDM is making.


**Rohin Shah:** Yep, sounds great.


###


The limitations of safety and alignment commitments \[00:10:38\]


**Rob Wiblin:** So you are not enthusiastic about AI companies making firm safety or alignment commitments in response to public pressure or political pressure, something that has been happening over the last couple of years. Why is that?


**Rohin Shah:** Yeah, I think it’s worth being a little bit clear about what we mean by “commitments” here. When I think of something called a commitment, I imagine that you write down some sort of action that you are planning to take now and into the future. Maybe it will only start in the future. And you’re saying, “We are not going to change our mind on this. We are going to do this in the future as well.” Sort of like tying yourself to the mast and making sure you’re going to do that.


My objection to this is really just that the research continues to change, the actions that we think are the best actions to take change over time — and given that, it’s just not actually a good idea to tie yourself to the mast.


So I’ll give you an example. Maybe two or three years ago, people used to be pretty into the idea of adding more data in pretraining that’s about alignment research. Think research papers on safety and alignment, think like LessWrong blog posts that talk about AI alignment, stuff like that. The idea was, the more of this data you put into pretraining time, the smarter the AI will be about alignment in particular, which then allows you to use the AI system to help you with your alignment research.


I would say that nowadays, the opinion is more the exact opposite of that: instead we would rather filter out that sort of data from the pretraining dataset for two reasons:


- One, it makes it less likely that the AI system learns that there is this persona of a malicious AI that it maybe could adopt after some poorly done post-training or some poorly chosen prompt during deployment.
- Then the second reason is maybe we don’t want our AI systems to know in great detail all of the mitigations that we’re planning to put in place, because that makes it easier for it to evade it if it is misaligned.


It would be pretty bad if we tied ourselves to the mast of “we’re going to throw in lots of alignment data at pretraining time” two or three years ago.


**Rob Wiblin:** So there’s this issue that the future is uncertain, and we don’t know exactly what commitments we will want to have made — you might end up committing to something that is useless or even actively harmful.


But if you think about why people make commitments at all, there’s a couple of different reasons. One is that they want to tie themselves to the mast against future temptation to do the wrong thing. There’s also that they want to communicate to other people what they’re going to do, so it makes it easier for them to coordinate. Perhaps you could reduce race dynamics by making particular commitments.


And I guess in this multi-person, multi-player situation, there’s an extra reason that exists, which is that external people want to pressure Google DeepMind or AI companies to act in a particular way. It’s very difficult to communicate “we’re committed to doing the right thing, whatever that turns out to be.” So instead, they want to pressure you to do specific things that they suspect will be useful — that might not be the case, but the kind of best guess as to what they will want you to do in future — and that’s maybe the most practical thing that they can actually campaign on.


What would you make of those arguments for actually making commitments?


**Rohin Shah:** I guess my biggest objection to this is just that it won’t work. I don’t actually think it would make sense even on the merits, even if it did work. But I would say that it just won’t work.


**Rob Wiblin:** Because the companies won’t stick to bad goals that they’re given, or to any goals?


**Rohin Shah:** Well, mostly I would say that if you think of what a commitment is… I’m imagining here something like the company puts out a blog post that says, “We commit to doing X.” There are other ways that you could try to make commitments, but that I think is the one that people are usually imagining. I just think that if, in fact, you imagine that the company is trying to now get out of this commitment in the future, it totally will just be able to do that. There are examples of this in the broader world, not just in AI.


But even if you look at AI in particular, I think[Anthropic’s RSP](https://www.anthropic.com/news/anthropics-responsible-scaling-policy) , for example, the Responsible Scaling Policy, the first version was really quite strong and said a lot of stuff about using the word commit: “We commit to do X. We commit to do Y.” I don’t actually remember the exact details, but I think many of them in future iterations of the responsible scaling policy, they removed that wording and replaced it with something less strong. So despite adding those words there, I think in fact they did not actually tie themselves to the mast.


I think this is good. I think that it was a mistake to have set strong language to the RSP in the first place. I think probably much of the stuff that they removed, it’s good. It makes them more effective at their goals, including at safety and responsibility. But empirically, it’s a good example of how, in fact, they did not actually tie themselves to the mast. And I think that is just how it’s going to be for the companies, at least in the current political climate.


---


**Rob Wiblin:** Hey everyone, Rob here. To avoid any confusion, I just wanted to point out that Rohin said the above *before* Anthropic released the third version of their Responsible Scaling Policy, which indeed mostly dropped the use of the term “commitment,” giving a justification related to what Rohin is saying here. All right, back to the show.


---


I think Google has actually been doing better on this. The first[Frontier Safety Framework](https://deepmind.google/blog/introducing-the-frontier-safety-framework/) , people essentially argued that it was weak and unambitious and that it never used the word “commit” in it anywhere. But I think it was much more accurately reflective of what Google was actually going to do in the future. So I think in that sense, it was better and gave a better sense to the public of what is actually going to happen in practice. This is one place where I do actually trust Google more than Anthropic — or OpenAI, for that matter.


**Rob Wiblin:** Because Google is more conservative about the commitments that it makes, it’s actually more likely to follow through on the things that it does say it will do?


**Rohin Shah:** That’s right. They’re very paranoid about commitments. Not just commitments, just anything that they say that they’re doing, they’re paranoid about it. They’re like, “Is this actually a good idea? Are we actually ready to continue doing this into the future?” So I find it easier to trust the words that Google says relative to other companies.


**Rob Wiblin:** I guess you trust yourself and your colleagues to broadly act reasonably when the time comes, which means that it’s very natural that you don’t want to completely tie your hands. You want to maintain flexibility to do whatever seems reasonable to you at the time.


But imagine other people externally: they either don’t trust you and your colleagues, or they aren’t sure whether to trust you and your colleagues.


**Rohin Shah:** Things that I would recommend are stuff like third-party audits, or third-party evaluators that get a reasonable amount of access to the company, and can use that to audit the practices and release some probably-somewhat-redacted report of what their findings are.


**Rob Wiblin:** Yeah, tell us more about that. What do you think is useful?


**Rohin Shah:** I think the main thing that drives my thinking here is something that I would call attention to detail. Generally, I tend to think that AI is a space that requires quite a lot of nuance, and you actually need to know a lot of facts on the ground in order to choose the right actions or the right things to be evaluating and checking.


And as a result, I care most about having a few people who are spending a lot of time looking in great detail and then writing up their results, or somehow communicating their results or using that to make some sort of action. Which is why I would say third-party evaluations seem like one of the best things to me — because you can build up these organisations that build a lot of context, spend a lot of time defining their evaluations, gain a bunch of information about how everything is working, and then can make fairly nuanced decisions about it while not being subject to the same biases that people in companies are going to be subject to.


So that’s the avenue that I’m most excited about. Whether it’s doable in today’s political climate, less obvious. So maybe in lieu of that, what you could do that might someday get to move in that direction is more like safety scorecards.


[AI Lab Watch](https://ailabwatch.org/) is my favourite scorecard in this area. I wish we were doing more things like that. If I had to make a career change right now and do something else, that would be one of my top two choices about what to do.


**Rob Wiblin:** Tell us about AI Lab Watch. What useful function do you think it’s serving now?


**Rohin Shah:** It’s not totally clear to me that it’s serving a useful function yet, but I think it could be. Maybe I should say a little bit about what it is. It’s a scorecard that evaluates companies based on essentially how good they are at safety for existential risks, or at least severe catastrophic risks.


It’s run by one guy, Zach Stein-Perlman, and I think he’s not even putting all of his time into it. Maybe it’s like half of his time. I think Zach Stein-Perlman has incredible attention to detail. He’s diving into these extremely detailed governance docs, reading through all of them, pulling out individual sentences to allow him to come to conclusions; reading through all of the model cards and frontier safety reports and seeing exactly what the companies did and didn’t do.


So I think that’s part of it. I think there’s a good amount of nuance, and I actually believe the conclusions. Well, I believe at least some of the conclusions that he comes to, which is usually not true of other scorecards.


I think if the scorecard got to the point where it was more robust, more accepted by the broader community — and especially accepted by the companies as a fairly legitimate scorecard — then you could imagine a[race to the top on safety](https://80000hours.org/podcast/episodes/holden-karnofsky-concrete-ai-safety-frontier-ai-companies/) where you’re like, “Let us climb the AI Lab Watch scorecard and be able to advertise ourselves as the safest company” or something along those lines.


I think that’s one way in which you could have external actors trying to get the companies to be more safe in today’s political climate.


**Rob Wiblin:** And the way that that’s different than the broad commitments that companies have tended to be making the last couple of years is that you can have technical experts working at this AI Lab Watch organisation, constantly updating it, I guess paying a lot of attention to detail about exactly what are the practices that companies engage in or don’t engage in that make a big difference, and constantly updating it based on newest opinions or newest research about what actually matters.


So you gave us an example of a reversal of opinion about what AI companies ought to be doing before: before, people thought you should be training on this data, and now they think you should be taking a lot of care to cut it out instead. But surely there are some commitments that are broad enough or non-specific enough or just so obviously good that it is reasonable to commit to them.


For example, you could have a commitment to provide the kinds of information that an AI Lab Watch would require to rate whether Google DeepMind or any other company is doing a good job. Or you were saying you think it’s useful to have expert auditors, people running evals, having enough access to run sophisticated evals on the models to understand if they’re dangerous in this or that way. You could commit to provide access to any external auditor or evaluator that meets a particular set of reasonable requirements. What about those kinds of commitments?


**Rohin Shah:** I think those are better. I would still say that they don’t always make sense. To take an example you just brought up, providing access to information, I think it’s just very easy for me to imagine this written in a way that backfires.


For example, we do evaluations in the CBRN domain — that’s chemical, biological, radiological, and nuclear — basically about whether AI systems can help with developing weapons of mass destruction. A lot of the information here is quite infohazardous, and I think it is probably the case that many external evaluators will not have the same level of information security that at least Google does, so I could imagine thinking that that was actually a bad commitment to have made.


I think another version is like, we talk about race dynamics quite a lot. One thing, that actually I’m fairly uncertain about, but you could imagine that it’s actually a pretty high priority for companies to keep their algorithmic progress and similar things locked up, and not allow that to diffuse too far. There are various arguments for this, and we don’t need to go into them, but that’s a common position. I think if you actually take that seriously, it does mean that you probably do have this tradeoff about what information you share externally that will increase the chance that it leaks versus what information you really try to lock down. And I think it would be hard to make a commitment about that.


I do still feel though, for some of them, more sympathetic to like, this seems like obviously a good commitment. I would still say though that the tying to the mast just doesn’t actually work, so I would rather do it by checking what the companies are actually doing in practice, and then judge them based on whether they are doing the things that we think are good, rather than whether they have made a commitment to continue doing it in the future.


**Rob Wiblin:** There’s this saying “personnel is policy,” and it sounds to me like your attitude is that there’s no set of things you can write down — commitments you can make, or good intentions you can write down on a piece of paper — that can at all substitute for having wise, well-motivated people in the positions of decision making over these things, and people who understand the things well enough that they can actually make the right decision if they’re so motivated. Is that basically right?


**Rohin Shah:** Mostly right. Maybe I would say yes to wise and well-motivated, but they don’t have to be inside companies. They can be external third-party auditors. I think that that would work. That just means you have to have humans who know a lot of stuff who are looking into the details a bunch and doing that.


Rules that you write down in advance are one of the stupidest… Sorry, I mean “stupid” in the sense of the rule itself clearly can’t have very much intelligence in it, otherwise it would not be a rule. It’s like you write down in advance based on what you think, before seeing the evidence, a policy that can be written down in English, rather than allowing for flexible adjustment over time. It’s just so weak. Intelligence and optimisation pressure applied against a rule will always get around the rule. Or the rule will be so stringent as to apply really huge costs to the companies, and that just won’t fly in today’s political climate. Also just seems like a bad idea to me.


###


Does Rohin’s team have veto power at Google DeepMind? \[00:27:36\]


**Rob Wiblin:** The most common question from the audience was: “Does the AGI safety and alignment team have a hard veto on any aspect of training or deploying a potential future AGI?” You know, if \[Google CEO\] Sundar Pichai wants to deploy a model or to train a model that you don’t think is safe, can he just overrule everyone on your team?


**Rohin Shah:** I kind of disagree with the frame of the question. So the literal answer is our role is advisory. If we make a recommendation, and other decision makers such as Sundar disagree with it, Sundar’s decision is the one that will matter.


But this is a question that bakes in the frame that we are adversaries of the company, and we need to have hard power, some sort of veto that enables us to make the right decision regardless of what the rest of the company thinks. I think this is just not a good or healthy model for how things should work inside of a company.


I see my job as making sure that I am producing and providing the right information such that decision makers can make the right decisions. So yes, the role is essentially advisory, but the way that it works is: if I think that there’s something wrong, I will escalate it to my manager, Anca \[Dragan\]. Anca started out as the head of safety and now is co-lead of Gemini post-training, so has a significant amount of influence and power. And if she agrees with me, then she will escalate it one step further and make a recommendation not to launch the model.


**Rob Wiblin:** People who are broadly worried about the direction that everything is going I think more often than not feel themselves to be in primarily an adversarial relationship with AI companies. You think that that’s not the case, and that in fact the companies are in an apathetic relationship with that group of people. Explain that.


**Rohin Shah:** I guess maybe I would say that it’s better for them to model the company as apathetic. I think you can have a more detailed model, which isn’t actually apathetic, but it’s maybe a bit more complicated.


To go into the more detailed model, I would say that building an artifact like Gemini is very, very difficult. The main reason being you have to produce this one thing, this single set of model weights, deployed using a single serving stack. And it has to satisfy so many constraints, and there are interaction effects between all of these constraints.


So there’s stuff like, does it do the instruction following right? Is it doing safety right? Has the architecture been chosen in a way that enables fast inference? Does it speak multiple languages? There’s probably 100 such things. And it is the case that if you make one change to the process with the intent of making one of these things better — say, safety — it will have random downstream knock-on effects on other constraints that you totally did not anticipate.


**Rob Wiblin:** This fragility of the process, doesn’t that mean that it’s actually going to be quite hard to respond quickly in real time to any new safety concerns? You or anyone else might be saying, “We should change this part,” and it’d be like, “No, you’re going to break this entire Rube Goldberg machine that we’ve built to make this product.”


**Rohin Shah:** I mean, to some extent, yes. You know, DeepMind was founded with this mission. That was one of the reasons that Demis \[Hassabis\] founded it. And DeepMind has had an AGI safety team since well before I joined the company. They didn’t need to have it. It’s a bunch of money that they’re spending that they didn’t really need to spend. So they definitely do care. But in fact, there are many, many, many things that we could do to improve safety, but only a few things that we can do at any given time, because it takes quite a long time to do it.


Now, do we have tools for reacting to safety problems that we see in deployment? Yes. Mostly, these do not involve changing the model weights, because that’s the thing that is most constrained. That’s the artifact that you have to produce one of it, and it’s got a gazillion constraints imposing it. But we have these out-of-the-model filters that we can change a bit more. We can target them to specific prompts or specific problems, so those are a lot easier to update over time. But not everything that you want to do is going to be solvable with an out-of-model filter.


I guess going back to the original question you mentioned, about are companies adversaries versus apathetic, basically my take is that because of this huge interaction of various constraints, really the easiest way to model at least GDM, but probably most AI companies, is they can only do a few things at a time for safety. And they will do them; they do care — but maybe you should just think of them as apathetic. You should really try to really lay out, “Here’s exactly what you need to do. Here’s why it’s not going to hurt any of these other constraints that you care about.” Also just because everyone is busy.


###


Central banks as a roadmap for regulating AI \[00:33:34\]


**Rob Wiblin:** So you said that it’s a lot more important to have access and transparency for expert auditors, monitors, regulators. But don’t we at least need enough public understanding or public transparency such that both voters in general and politicians specifically are providing the resourcing, the funding, the backing, the will for those auditors, for those regulators, so that they can insist on getting the access that they need, even if perhaps a company… Or for whatever reason, they need resourcing in order to get the job done?


**Rohin Shah:** Yeah, I definitely think that is important to do. I don’t think it’s very much in conflict with anything that I’ve said.


I think the way that this happens currently is we release models, and then everybody sees how capable they are, which is by far the most important thing. That’s a kind of transparency that’s needed. In fact, we used to run this survey of researchers at GDM on what their views on x-risk and other kinds of safety were. And one of the things we used to ask is, “What has changed your mind on this safety stuff over time?” And it was just so uniform. I think with literally just one exception, the answer was uniformly some sort of capabilities improvement. They were like, “GPT-4: that’s the thing that changed my mind about how important safety was.”


And I think that is just basically available to the public right now. Everyone needs to release their models quickly. You can benchmark the capabilities after the fact. You can play around with the models yourself in order to see how good they are. So that particular piece of information I think is already there.


There’s maybe some more information about how important is safety, or some more detailed information that maybe you need a bit more nuance, a bit more access to get. Mostly I feel like that infrastructure does exist already, at least for politicians, which I think is the more important part right now. Like the US AISI, the UK AISI do get more visibility into the companies. They have partnerships with almost all of the frontier companies, possibly all of them, I don’t know. I think they do have a pretty good understanding of what is happening in AI companies, and they then use that to inform politicians and their respective governments.


**Rob Wiblin:** Yeah, I think one of the most analogous areas of regulation and monitoring currently to all this is, in my mind, the Federal Reserve, the central bank oversight of the financial system, of banks and financial stability. It’s an incredibly technical area, very difficult to track. The public has a broad desire not to have a financial crisis and not to have banks go bankrupt, but very limited understanding of the specifics. And I suppose the analogy there would be that they understand on some level that this is a serious issue.


That flows through to politicians who are also really scared about things going wrong. They provide really quite significant resourcing and they hire very expensive experts to basically constantly be talking with and monitoring all of the banks. I think both the Bank of England and the Federal Reserve in the US are really heavily involved with all of the banks: monitoring their books, basically in a constant conversation with them about whether things are going wrong and what risks are emerging.


And that would feel like a very natural way for things to go, I guess, especially if you’re right that it’s really not obvious what needs to be done. You have to kind of be in the room, understanding a lot of contextual specifics in order to say whether something is a good thing or a bad thing to change.


**Rohin Shah:** Yeah, I think that’s almost exactly the kind of model that I would want.


###


How useful are pre-deployment evaluations of models? \[00:37:41\]


**Rob Wiblin:** Maybe the most dominant approach that many people in AI safety and alignment are taking, actually maybe in both technical and policy areas, is trying to create and enforce pre-deployment evaluations: testing what models are capable of doing and what they’re inclined to do before they are deployed as products to the public, trying to test for ways that things could go really wrong.


You think that this is probably a misguided, not very effective high-level strategy. Why is that?


**Rohin Shah:** Yeah, I think the main cost of this is that launch schedules are just really quite important at a company, and you try to keep them as short as possible. Once you have a model, you would really like to get it out to the public as soon as you can. So if you tie evals to, and require them to be pre-deployment, then that is providing a pretty strong incentive to make those evals as fast as possible to run and get them done as quickly as possible, which is maybe not really the incentive you want to give.


Obviously we’re going to try and make them as good as possible, but it’s still a constraint; we probably could do better if we had more time. And there’s some amount where we can push back and say that actually we need the time to do the evals, but it’s not an infinite amount. So that I think is just really quite a large cost. It might be worth it if there are strong benefits, but I just don’t think there are particularly strong benefits.


The one that people would naturally say is like, you need to know if you’re releasing a dangerous model. Ideally, you don’t release the dangerous model, and the way you have to do that is via pre-deployment evals. To this, I would mostly say that AI progress is reasonably continuous. You can get a decent sense of how the next AI system is going to behave based on the previous one. You can have some reasonable, OK bounds on this.


So if you design your evals and your thresholds such that there is a reasonable safety buffer between when your evals trigger versus when you actually think the model is dangerous, then it seems just basically totally fine to say that we evaluated the previous model, or we ran an evaluation a month ago. It’s not going to have had a huge giant leap in that time, it was under our threshold at the time, there’s the safety buffer: therefore, we’re not worried about this model. This is the approach we’ve been taking in our Frontier Safety Framework since the very first time it was published. It’s not particularly new.


So I think the benefits are just not really there, so it doesn’t really make sense to impose this cost.


Then a couple of other minor points. One is that especially for misalignment or loss of control, the threat model is tied more around internal deployment rather than external deployment, because I think it’s just easier for a misaligned model to cause problems inside the company where it’s getting a bunch of permissions rather than outside the company where it has no access to its own weights, for example. And the pre-external deployment evals don’t really make that much of a difference to internal deployments.


**Rob Wiblin:** Is there any good reason to think that we might, in the next couple of years, see a huge jump from one cycle to the next, such that the model could become way more unexpectedly capable or way more unexpectedly evil than the previous one?


**Rohin Shah:** Unexpectedly capable seems pretty unlikely. I think we’ve just seen enough examples of AI development now to say that, no, in fact, AI development progresses fairly smoothly and continuously. I do think that in the future, you could definitely see an[intelligence explosion](https://80000hours.org/podcast/episodes/will-macaskill-century-in-a-decade-navigating-intelligence-explosion/) , in which case the progress will go much faster with respect to calendar time. I think it will still actually be pretty smooth and gradual with respect to inputs like compute and labour; it’s just that in an intelligence explosion, you get a much larger increase in especially labour, but probably also compute, and that ends up making things go very fast with respect to calendar time.


But there’s still this general property that you can, given some amount of compute and labour that you expect to be spending over the next however long, have some decent sense of how much progress is going to be made on the capability side.


You also asked about whether the AI system might become much more evil. I think that’s one that could, in fact, change pretty significantly between models, just because it’s a somewhat more contingent property of exactly how you do post-training, and small changes to it could have big effects on that. So there I think it is more important that, to the extent that your safety case depends on specifically the model not being evil in some way, you actually do in fact need to do the pre-deployment evals to check whether that’s the case.


And this is in fact what we do. Not exactly this, but we do in fact do a lot of pre-deployment evals for safety right now in terms of whether the model has a propensity to do bad things. This tends to be more in present-day safety type stuff, things like: will the model help you write suicide notes? Will the model incite violence? And those we’d run before any launch, and if the numbers are sufficiently bad, we won’t launch that model.


###


Governance might be a bigger bottleneck than alignment \[00:43:55\]


**Rob Wiblin:** You don’t think that we need to do very much preparation ahead of time in order to be able to get future AGI, or a future recursively self-improving AI, to do a lot of safety and alignment research when the time comes, which I think might explain why I don’t really hear very much at all about that broad approach from GDM. But I guess by contrast, at least a couple of years ago this was the dominant approach that OpenAI would talk about all the time. And you definitely hear things about it from Anthropic. Why don’t you think we need to be doing much prep now?


**Rohin Shah:** It’s worth laying out the scenario where this becomes important. Effectively, the worry about this is an intelligence explosion scenario where you build your AI system and that AI system is now capable enough that it can actually help accelerate your AI R&D research. It can just make your capabilities research go faster. And under certain assumptions that we can get into, this then seems likely to drastically increase the rate at which capabilities progress happens, and you get an intelligence explosion.


A natural worry you might have in this situation is: if everything is speeding up on the capabilities side, will the safety and alignment side be able to keep up? It’s worth noting that the way that the capabilities speeds up is via the application of AI labour to do capabilities research. So the natural approach is then to apply the same AI labour to do safety and alignment research.


Now, if you believe, as I do, that the sort of prosaic alignment research — where you look at the stuff that’s going wrong now, do a little bit of forecasting what stuff is going to go wrong in the future with the next few models over the next some amount of time period, a year perhaps, and then you can do fairly normal ML research in order to address it — if that’s your view of how alignment research can progress, this research looks very, very, very similar to capabilities research.


So if the AI is accelerating capabilities research a tonne, you should be able to, as long as you’re willing to spend compute on it, take that same AI system and accelerate safety and alignment work in the same way.


There are some disanalogies between capabilities research and alignment research. I think the disanalogies are particularly large today and will become smaller in the future as you get closer to these sorts of AIs that are capable of doing this automated research. And by the time you get to that point, there are still disanalogies, but I think they’re relatively small. And by default, you shouldn’t really expect a big difference in the ability to do automated alignment research versus automated capabilities research.


So there’s some stuff you could do to prepare, but mostly I feel like we just don’t know what it’s going to look like. It will be so much more efficient to focus on other things that we can do today and then just adapt once we get to the point where the AIs are capable of doing this.


Now, I guess one thing I do want to flag is that there’s another worry you could have, which is like, sure, all the technical safety and alignment research gets accelerated, but that’s not the only thing that you need in order to get good outcomes. You also need governance to go better, so you also need to accelerate governance.


Now, governance has a totally different set of skills than capabilities or safety or alignment research. So it’s really much less clear that AI systems that drastically accelerate capabilities research will also drastically accelerate governance. In addition to the AIs just not having the capabilities to do that, which is one possibility, there’s also just like, will we as a society be willing to use AIs to accelerate governance? I think possibly not — because capabilities researchers want to actually accelerate themselves with AIs, and I don’t get the sense that governance people will want to do this.


So this accelerating governance work is one of my top two things that I would do if I had to make a career change right now to something else: figure out what we need to do in order to accelerate governance and start doing the work that we need to do it.


**Rob Wiblin:** I’m surprised you say that governance people aren’t interested in using AI to accelerate their work. At least among the more AGI-pilled groups, I think[Forethought Research](https://www.forethought.org/) is very much on board with this. I think Coefficient Giving is developing a plan.[I spoke about that with Ajeya Cotra](https://80000hours.org/podcast/episodes/ajeya-cotra-transformative-ai-crunch-time/) not too long ago, and they’re developing a plan for how would you deploy a whole lot of money and compute in order to solve those kinds of issues using AI labour.


I guess there could be this whole concern that no matter how much you were willing to spare, no matter how much people were trying, the models just wouldn’t actually be up to it at that point — because they would be very specialised on computer science and AI research, and not really up to thinking about broader society. But if that weren’t too bad, then it does seem that there are at least some actors who are interested in doing this.


**Rohin Shah:** Yeah, I definitely agree that the AGI-pilled governance people will do it. The nonprofits and think tanks associated with the AI safety community will absolutely do it. But they’re a small fraction of overall governance.


**Rob Wiblin:** What are some of the governance issues that you think only actual national governments can address, that are maybe going to be neglected and not really handled because they’re not able to use AI at the time?


**Rohin Shah:** I don’t really have concrete examples in mind. Mostly I would say that, going back to the points we made earlier in the podcast, it’s important that somebody is watching the AI companies and holding us accountable. Governments are a natural place to do that. And if the world is in fact radically changing — you know, people talk about[a century’s worth of progress in a decade](https://80000hours.org/podcast/episodes/will-macaskill-century-in-a-decade-navigating-intelligence-explosion/) — then you should expect that a bunch of problems are going to come up that we aren’t going to anticipate today. I think we need to be able to flexibly react to that. I don’t have particular problems in mind that I think are going to require government intervention, but it would be so shocking if there weren’t any.


**Rob Wiblin:** Yeah, I guess it might be very difficult to reform governments as a whole. As you say, they tend to be very rule-bound, and there’s going to be a whole lot of rules that make it difficult to use AI in the ways that you and I would think are sensible.


It’s possible you could get a carve-out for AI-specific agencies. I guess you could imagine the UK AI Security Institute, for example, basically being given carte blanche to use AI in its governance work in a way that other agencies potentially couldn’t, because it’s the only way that they would be able to keep up with at least their kind of work, and they’re the kind of group that might really push for it. That’s a slightly hopeful possible outcome.


**Rohin Shah:** Yeah, I hope we can do better than that, but I agree that would be a nice baseline. Mostly I feel like we haven’t really thought about this problem very much. I’m hoping that if we do think about this problem a decent amount, we will have something better to do than just that. But I have not been the one to do that, and I don’t know if anyone else has either. So really, I’m more saying that here’s a problem. I don’t really know what to do about it, but it sure would be good if we did something about it.


###


Why not just pause AI progress? \[00:51:44\]


**Rob Wiblin:** An audience member wrote in asking, “Why not instead prioritise slowing down AI advances or opposing development of superintelligence?”


**Rohin Shah:** I guess I would say: what’s the bottleneck to prevent actually pausing AI progress globally? I think by far the biggest bottleneck to this is people don’t agree that AI is going to take over the world. And the thing that most alleviates that bottleneck is good scientific evidence that suggests it would happen, or that it wouldn’t.


To be clear, my belief is that probably this will not happen, but I think it’s plausible enough that we should care about it. And I think the structure of that looks like: figure out whether each next step of scaling is safe. I think right now the answer is yes. At some point the answer might be no. And then at that point, I think having generated that evidence will be immensely helpful for this goal. Again, I don’t really expect that evidence to come, because I tend to think it probably won’t be a problem.


There’s this nice post from Scott Alexander, “[Guided by the beauty of our weapons](https://slatestarcodex.com/2017/03/24/guided-by-the-beauty-of-our-weapons/) .” It’s probably my favourite post from him of all time. He talks about how there are symmetric weapons which allow you to argue for some sort of conclusion or get people on your side, irrespective of whether your claim is true or not. And then there are asymmetric weapons which only work to the extent that the thing that you’re arguing for is true, or at least are more likely to work in that setting.


And he has this really quite moving description at some point of how beautiful and elegant this all is, where for the asymmetric weapons you and your “enemies” will join forces, hold hands, and work together to do it — because both of you are thinking that it’s going to prove you right, until the very end when the evidence comes in, and then you just agree because the evidence showed you the answer.


**Rob Wiblin:** Well, then you moved the goalposts, I would say. But I suppose a reasonable onlooker can tell who was right.


**Rohin Shah:** Sure, fair enough. But that’s the sort of strategy that I would much rather do at the moment, given that I think the bottleneck is by far the fact that people don’t agree on whether or not this is necessary.


###


How much longer will we be able to read AIs’ thoughts? \[00:54:17\]


**Rob Wiblin:** One place you’re very much with the broader consensus is that[chain-of-thought monitoring is extremely useful](https://www.lesswrong.com/posts/7xneDbsgj6yJDJMjK/chain-of-thought-monitorability-a-new-and-fragile) and something that we want to be able to preserve for as long as possible. This is watching the thoughts that the AI is having, that it’s outputting onto its scratchpad in order to understand what it’s trying to accomplish and why.


I guess a difference that you have with many other people on that topic is that you think that it is fairly likely that chain of thought monitoring will remain useful, that we will be able to understand what the AIs are thinking and that what they’re writing down there is actually related to what they’re doing for longer than other people do.[Many people worry](https://80000hours.org/podcast/episodes/neel-nanda-mechanistic-interpretability/#we-could-accidentally-destroy-chain-of-thoughts-usefulness-011139) that within a year or two, perhaps they could be speaking in some crazy code, or they could figure out ways of putting information in there that we can’t fully track, or there would just be too much of it for us to even be able to monitor it.


Why do you think that chain of thought monitoring potentially is going to have quite a long run?


**Rohin Shah:** So this is a good example of where attention to detail really matters a lot, so you’re going to get a pretty long answer from me because it’s just fairly disjunctive.


I think maybe to start with, let me recap the basic story for why chain of thought monitoring should be expected to be particularly good. I call this the externalised reasoning property. And the way I usually phrase it would be that for sufficiently difficult tasks — by which I mean tasks that require a lot of reasoning to do, some sort of serial reasoning over time — transformers, not necessarily other architectures but at least transformers, must use the chain of thought as a form of sort of working memory. There’s really no other alternative, so some information about the reasoning that they’re doing has to be present in that chain of thought.


That’s part number one. And then part two is that, given how we train language models today, the chain of thought is actually legible and understandable to humans.


So I’ll maybe address these two parts separately. The first part is actually the AI really does have to put some information into the chain of thought, because there’s no other way for it to do the hard reasoning needed to solve difficult tasks.


The basic argument here is, I’ll call it “opaque serial depth.” The idea is that you can just look at the architecture of an AI system and then say, suppose this AI system has to do its reasoning only via the billions of floating point numbers that exist inside of it. It can’t use the tokens that it’s outputting. How many steps of cognition can it do using only the floating point numbers?


The answer for transformers is actually not very much. The idea is basically that sequential steps of computation have to go deeper and deeper in the model. The model is made up of a number of layers, and there’s just no way for information from a later layer to flow back to an earlier layer — except via going through the tokens in the chain of thought. So overall, the opaque serial depth of a transformer is very low.


Why do I expect this to continue? This is actually a crucial aspect of why pretraining can be so efficient. GPUs and TPUs are very powerful computationally, but the way that they get this power is by doing a tonne of computations in parallel. So if you need to compute A + B to get C, and then you need C, that intermediate result, to feed into a further computation, that would be serial computation.


GPUs and TPUs are not very good at that; what they do is a lot of stuff in parallel. So pretraining is really heavily optimised to be able to do everything in parallel, which means that the opaque serial depth does actually have to be pretty small in order to get those efficiency gains. This is a pretty strong structural economic reason that, at least for pretraining, you’re going to get AI systems that have relatively small opaque serial depth.


So you might think of them as like, there’s a good reason for models to be born speaking English or some other natural language.


**Rob Wiblin:** Just to check that I’ve understood, you’re saying because we are using GPUs and TPUs, structurally that forces the thought of the model to be very deep: it can have many things in its mind at one time, but there can’t be very many steps, because you have to be going through all of these things in parallel. But if it was very wide, then you wouldn’t be able to do them all simultaneously; you would have to wait until the earlier steps were done to do the later steps. And this is just something that is going to remain the case for years to come.


**Rohin Shah:** Yes, I think that’s right. I think for technical folks, they might interchange the words “wide” and “deep” in your sentence, but yes.


**Rob Wiblin:** \[laughs\] Cool.


**Rohin Shah:** Yeah, so that was stage number one, which is like, why should we expect the opaque serial depth to stay low, at least during the pretraining phase?


Then there’s step two, which is like, we’re saying that the pre-trained model has to essentially speak in English in order to do reasoning. But then there’s a bunch of post-training and RL and all of this sort of stuff. Maybe that’s going to make it so that even if the model is speaking in tokens, maybe it will start speaking some sort of alien language that we don’t understand.


And here I would say there’s no theoretical argument, or there’s no theorem you can prove that will say, no, they’re not going to do this. But I will say they are born speaking English in some sense, and pretraining is by far the most powerful form of getting stuff into an AI system that we have ever built. So the model is incredibly good at speaking in natural language. It’s great at doing reasoning, but only the kind of reasoning that humans do when writing stuff down.


And then when you look at the reasoning training that we’re doing today, there’s a[paper](https://arxiv.org/abs/2510.07364) from Neel Nanda and others that shows that actually a substantial part of what the reasoning training is doing is just teaching the model when to do a specific kind of reasoning step that it had already learned during pretraining.


So basically a lot of the capability is just coming in pretraining. We know that the pretraining, for good reason, is going to stay the way that it is and be done using human-like reasoning and speaking in English. and then the RL is just really inefficient relative to pretraining, and for it to build an entirely new epistemic language that is something that we wouldn’t be able to understand with even with a decent chunk of effort is just so far beyond what RL is doing currently that it would be pretty surprising to see that in the near future.


Now, do I expect to see it ever? Yes. But I think six months ago, I split a room in a conference by saying, “Agree or disagree? Chain of thought monitoring will continue for two years.” I think I said two years. It might have even been one year. I forget. Whereas my median is probably like four years, five years, I don’t know. It’s not totally clear, mostly because of this argument about how hard it would be to go beyond the current situation.


**Rob Wiblin:** So I don’t fully understand this idea of[continuous chain of thought](https://arxiv.org/abs/2508.03440) , but isn’t there this notion that basically at the end of thinking for a little bit, currently we force the models to output a word, a token, and then we feed that back into the start of the model again?


But rather than compress all of its thoughts down into a single token or word or whatever, why don’t we just keep the full distribution of all of the thoughts and then feed them back into the beginning again? Wouldn’t that allow it to preserve more information, rather than basically throwing a bunch of it out? And if that was a much more effective way of thinking and reasoning, and then you have no stage where you’re actually outputting a token that a human being can read, then wouldn’t that be a force that would potentially make them much more opaque to us?


**Rohin Shah:** I agree that sort of thing might work in the relatively near future. I’m not sure it diminishes the value of chain of thought monitoring all that much. If you were in fact just saying that we’re going to keep a probability distribution of tokens and feed that back in, you can still inspect that probability distribution of tokens and interpret them again as normal English reasoning. It’s harder, because now you have this larger set of things that the model could be thinking about at any given time. But I would still expect that it’s not too bad, and you would be able to do some monitoring of it. I’d rather we didn’t do it, but I don’t think it is a fatal flaw.


I haven’t been following the literature on this too closely, but my sense from the papers that I have looked at is that even when you do this sort of thing, sometimes it just makes performance worse. And even when it does make performance better, if you actually try to look at what these tokens are that it’s doing by just looking at the full probability distribution, it does just look like it basically has like maybe one main reasoning track that’s going on, and you can still follow along pretty easily.


**Rob Wiblin:** So you’re saying even if you are using continuous chain of thought, you can still go back and say, what if we had forced it to output tokens at all of these intermediate stages? What would that probably have looked like? Let’s just take the most likely word or the most likely token at each step and then read that.


I guess there might be a concern that it could hide a second track of thought in the tail of the probability distribution, things that you wouldn’t be likely to read. But you’re saying it seems like it only has actually one train of thought here. There’s not a second hidden chain of thought that you wouldn’t be able to inspect.


**Rohin Shah:** Yeah, I would say that you should maybe look at not just the top one, but maybe the top five. Maybe even just the top two would be enough. You can also usually tell how much you are missing, because for most of the architectures I’ve seen, there’s a way where you can modify it at test time so that it only uses the information of the top two tokens, and everything else you just get rid of it, you ablate it away. And then you can do that intervention and you can see, does the model still perform as well as it did before? My prediction is it will probably perform about as well as it did before. And if that happens, then I think you can be reasonably confident that it’s not smuggling in a bunch of information in the rest of the probability distribution.


**Rob Wiblin:** Even setting aside whether it performs as well according to some benchmark, you were saying keep just the top word or the top few tokens, the few most probable ones, and throw out everything else. You could see whether it does a different thing. Like, does it lead to a different recommendation or a different outcome? And if always the recommendation is the same, or always, in practical terms, the output is the same thing from our point of view, then that strongly suggests that the tail isn’t containing any important information. It’s not containing a second set of reasoning that could affect the ultimate outcome.


**Rohin Shah:** Yeah, that’s right. I should give a caveat, which is: probably if you do enough fine-tuning and enough RL on the model to get it to, instead of treating this as a probability distribution over tokens, just treat it as like a vector of numbers and just make that vector of numbers as useful as possible, with enough training, probably it will get to the point where you can no longer interpret it as a distribution over tokens. I think the papers I’ve seen on this suggest that this works less well than just treating it as a distribution over tokens.


**Rob Wiblin:** Would this be the process of basically asking it to come up with its own non-human-readable language?


**Rohin Shah:** Yeah, basically. In fact, this is what the original[Coconut paper](https://arxiv.org/abs/2412.06769) proposed. And I think some of the[follow-on](https://arxiv.org/abs/2505.15778)[work](https://arxiv.org/abs/2508.03440) after Coconut basically said that the problem with this is that it’s too expressive; we need to restrict it to just the probability distribution over tokens, and then it performs better — which I think reflects this fact that actually the models are much better at doing this sort of human-like reasoning, and if you restrict them to natural language, that’s the part they’re smart in, and that leads to better performance.


**Rob Wiblin:** OK, and your theory for that is that pretraining just packs an enormous punch. It’s an enormous amount to shape them. So they’re really good at English. They’re really good at human language. And if you ask them to come up with their own internal, different language — in theory, surely there is a better language for reasoning — they’re not able to bring along everything that they’ve learned from pretraining in the same way. They’re having to start from scratch. So at least at this point, that comes out substantially behind where they just are now using English or whatever other human language.


**Rohin Shah:** Yep, that’s exactly right.


**Rob Wiblin:** If you think that this opaque serial depth, the fact that they don’t have a very great serial depth without us being able to look at it, if that is so key to our ability to monitor them and ensure that they’re basically aligned or not doing anything too harmful, is that a potential kind of governance target for GDM? That you could have some internal policy saying…


I mean, it sounds like there’s not huge incentives yet to violate that anyway. But let’s say in future, you could get better performance at some point, or at some point there’ll probably be a crossover. You could still have an internal governance standard saying that they can’t think for more than this amount, or they can’t have this many thoughts one after another, before someone would, in principle, be able to scrutinise it. Because it actually just would be dangerous to exceed that.


**Rohin Shah:** Yeah, I think that’s definitely doable. I think it would be even better for it to be a broader industry standard. As a general rule, I tend to favour things that don’t require individual companies to unilaterally do stuff. It’s much more stable if you make it an industry standard. But yes, I think that would totally work.


In fact, I think by the time this podcast comes out, we will have[published a paper](https://arxiv.org/abs/2603.09786) that does describe how you could calculate this opaque serial depth for any given architecture, and has some code for how you could do this for at least models that are implemented in JAX, which is just a kind of framework that people use to implement models.


###


Sometimes, having to signal concern for safety diverts resources from actually making AI safer \[01:09:51\]


**Rob Wiblin:** Gemini 3 Pro came out not that long ago. The AI safety blogger, Zvi Mowshowitz, who was[on the show a couple of years ago](https://80000hours.org/podcast/episodes/zvi-mowshowitz-sleeper-agents-ai-updates/) , had[a bunch of fairly critical things to say](https://thezvi.substack.com/p/gemini-3-model-card-and-safety-framework) on his blog about the frontier safety report that came out I think simultaneously with the launch. Broadly speaking, he was worried that GDM was basically hiding a bunch of information that he thought would be inconvenient or create PR problems or regulatory problems for DeepMind if it was more salient and more easy to read.


There’s a whole lot of different things. People can go and read the blog post if they want. But a few that stood out to me was:


- He was troubled that on persuasion evaluations, you only gave odds ratios rather than absolute levels, so you couldn’t tell exactly how persuasive Gemini 3 or Gemini 2.5 was.
- On the cybersecurity evals, on his reading he thought that you treated the model hacking the test rather than solving the test in the natural way as kind of a green light (a reason not to be concerned) rather than a red light (a reason to be more worried).
- And in terms of helping people to acquire WMDs, it felt to him, and I think a lot of people have had this impression — not just about GDM, but about companies in general — that when it seems like the models are starting to approach the line, maybe it’s a bit ambiguous whether they’re above or below the line that they had six months or a year ago, it feels like the goalposts kind of shift and the standards rise over time, so that always the model is basically acceptable to put out. And that’s what he was worried about was happening here with Gemini 3 as well.


How do you respond to these kinds of objections?


**Rohin Shah:** Yeah, I have so many thoughts on this one. For most of these, I would say my primary response is that we care about the[frontier safety report](https://storage.googleapis.com/deepmind-media/gemini/gemini_3_pro_fsf_report.pdf) as a way of saying we have made a formal determination that this model is safe and we’re ready to release it from a safety perspective. I think that aspect of the frontier safety report is great.


For the persuasion one, I’m not that familiar with it. It’s done by a separate team, so I can’t say too much about it. But I expect that the answer is that they thought that this was the most important graph, so they put that one in, and they weren’t particularly thinking about how it would be red-teamed by external observers. I expect that they will[publish a paper](https://arxiv.org/abs/2603.25326) at some point, probably in 2026, that will go into more detail about this. But what I am confident about is that they weren’t particularly trying to hide any results here.


On the cyber side, I’m a little confused by that particular criticism. You said something about treating the model hacking the test as a green light instead of a red light. I’m a little confused by this, because we did count those as successes, so they did count towards the 11 out of 12 score that we reported. So that is treating it more like a red light rather than a green light.


I would also say that this should not be described as the model “hacking” the test. I think the way we describe it is that the model found an unintended shortcut to the test. It is not the case that the model looked at this and saw, “Aha! I can just edit the tests so that it looks like it’s passed.” It was more like there is this pretty complicated environment where we were trying to test the model’s ability to do one particular kind of cyber task, and instead it noticed that there was this alternate pathway.


And if I were doing this task — I mean, mostly I would fail at it, because I’m not as good as Gemini at cyber tasks — but if I were doing this task and I saw that shortcut, I would not have even thought of it as cheating. I would have thought of it as like, of course that’s the way in which you do the task.


Now, when we look at this, and we have to make a determination about how good is the model at cyber, ultimately we were trying to test it for one difficult thing and instead it did something slightly easier. And so there is this question about like, what do we conclude about the model’s capability?


**Rob Wiblin:** I guess you don’t know whether it could have done the harder thing, because it might have been because it couldn’t do the harder thing, or it might have just done it because it was easier.


**Rohin Shah:** Yeah, that’s right. I mean, it probably didn’t even consider the harder thing — because if the easier thing is there, it sees the path forward, it just takes it. But yes, we don’t know whether it could have done the harder thing. In that case, we had some subject matter experts look at it and their conclusion was that probably the model could have done the harder thing too, so we counted it towards the model’s score. I think in other cases we might not count it, and then probably what we would do is remove it from both the numerator and probably also the denominator. But in this particular case, we just did count it.


If I remember correctly from the post, I think Zvi’s bigger objection was that we had a second test that we didn’t report quantitative results on, but that was pretty key to us ruling out the cyber CCL \[critical capability level\]. And why did that happen? Mostly because this test is still, to some extent, under construction.


I’m confident that was robust enough to rule out the CCL. But why don’t I want to put great details into the frontier safety report? Mainly because the details of exactly how we run this eval are likely going to change in order to make it somewhat more robust, to include a couple more challenges. And then in the next frontier safety report, we have to write an entire section about how we changed the stuff and previous results aren’t comparable.


**Rob Wiblin:** I mean, you can understand that to a cynic like Zvi, doesn’t it seem awfully convenient that the test is good enough to determine that the model is safe, but not good enough for you to include any specific details in the report itself?


**Rohin Shah:** If you want something like that, you’d basically have to go to, at minimum, the level of detail and rigour that is found in an academic paper. And often even that’s not enough. I do see a lot of value in publishing papers about our evaluations — and we have done this, I think more so than most other companies. We published one of the first leading papers on evaluations. It’s called “[Evaluating frontier models for dangerous capabilities](https://deepmind.google/research/publications/78150/) .”


**Rob Wiblin:** Yeah, we[spoke about that with Allan Dafoe](https://80000hours.org/podcast/episodes/allan-dafoe-unstoppable-technology-human-agency-agi/#how-google-tests-for-dangerous-capabilities-014539) about a year ago.


**Rohin Shah:** Very nice, yes. And then more recently we published our[evaluations for stealth and situational awareness](https://arxiv.org/abs/2505.01420) . So papers are a place where I do feel like there’s clear benefits. It allows people to understand what the evaluations are actually doing; it allows for other people to build on top of those evaluations. So we do put more effort into that. Whereas I think for the frontier safety report, I feel like its primary purpose is to say that we made this determination formally, and less about providing enough details that people can independently check our work.


**Rob Wiblin:** I mean, people don’t read papers, Rohin. They read the model card because they’re psyched about a new model launch. Is it just impractical on that kind of timeline to write the equivalent of a paper or provide the level of detail and rigour that you would in a paper in the model card?


**Rohin Shah:** Yeah, I think that’s right. I should say the other thing that I think a model card or a frontier safety report is good for is if you have something that you want to speak in a megaphone to the community and attach the company’s brand to it: a model card or frontier safety report is excellent for that. For example, we have a section on chain of thought legibility because that is something I do want to use the megaphone for.


But in terms of can we write the paper in the model card or the frontier safety report? Not for new evaluations. Certainly there’s a substantial lag between when an evaluation is good enough that we can use it in our decision making; and when an evaluation is good enough, robust enough, stable enough, and battle tested enough, and we’ve like done a lot of work on the writing up, that we can publish a paper about it.


**Rob Wiblin:** OK, and the third concern was that there’s this general phenomenon, it seems, that as the models get more capable with each iteration, it feels like the bar for what would be troubling, would seem to be unsafe, rises approximately the same as the capabilities have gone up. What do you make of that?


**Rohin Shah:** So this was especially in context of the CBRN one, if I remember right. I would say that the reason that this happens is more that, as time goes on and we see more capabilities of AI systems, it becomes clearer what we need to evaluate for and what our threat models should be. After you see Gemini 2.5, you can be like, “Oh, this is the place where the models are actually getting good. We need to have stronger evaluations over here and to put more effort into understanding our threat modeling over here.”


So mostly what happened with the difference between Gemini 2.5 and Gemini 3 on CBRN is that we substantially improved our threat modeling and our evaluations so that they’re more rigorous — which is also partly why there’s not as much detail about them, because they’re not quite at the level where I think they’re nice, robust, and stable that we can really put lots of details out in a way where we don’t expect them to change a decent bit.


But this “as time goes on, you get more information and you can make your evaluations and threat modeling better” is not that easily distinguishable from “the goalposts are changing” — which is a bit unfortunate, but turns out to be the way that things go.


**Rob Wiblin:** I mean, that speaks to the fact that the purpose of these model cards in your mind is very different than the purpose that they serve in the mind of someone like Zvi, or I guess commentators in general. I think Zvi and many people want them to be an accountability mechanism, a mechanism by which the alarm could be sounded if the models were dangerous or were becoming more dangerous — where GDM would have to reveal that that was the case in the model card, because they have to put out these results. And even if they wanted to hide that the CBRN stuff was dangerous, they wouldn’t be able to.


**Rohin Shah:** Like has been a theme throughout this conversation, I would talk about third-party auditors who can actually see the examples and how they were graded and stuff like this. I think that’s way more important for judging whether or not the evaluation actually supports the judgement that we make than the specific quantitative scores that we get. You know, if I see a number like 10 out of 12 on capture the flag challenges, what does that mean? \[shrug\]


**Rob Wiblin:** Yeah. A back-and-forth that I hear repeatedly is that you or someone at a company will say, “The purpose of these reports is to show that we have thought this through, and we’ve made a determination that the model is safe to deploy to the public.”


And other people will say, “Sure, the current model, maybe it is fine” — they don’t actually think that it is too dangerous for people to be using commercially — “but one day these models will be dangerous enough that we should be concerned. And the only way that we can forecast how the company will behave come that time is how they’re behaving now. We use the model cards that are put out now as a measure of how serious the company is about safety, how serious it is about transparency and revealing what is actually going on. And the way you could credibly commit to be a good actor later on is to be a better actor now.”


What do you make of that? I guess you’re probably going to say similar things to what you’ve said before, that it just isn’t the right mechanism for the task.


**Rohin Shah:** Yeah, that’s right. Maybe more broadly, I might say that it feels like asks that the community makes can fall in two categories. One is things that actually matter for actual safety. I’m all for those. We should do those. People should judge us if we’re not doing them. And then there’s things I would categorise as pay costs to signal allegiance to the x-risk safety community. And I’m not about those. I don’t want to do them. If you ask for them, I’m just going to say no.


**Rob Wiblin:** I mean, I think allegiance is a little bit of an unfair way… It’s like willingness to commit resources to this purpose.


**Rohin Shah:** But like not to the purpose of actual safety. To the purpose of signaling that you will do safety in the future.


**Rob Wiblin:** I mean, this isn’t an unusual mechanism that you’ll want to kind of commit to doing something in future, and how people assess how you’ll behave in future, the only indication they can get is things in the present, because they don’t have a crystal ball.


**Rohin Shah:** That’s right. But I think you should do it based on the things that actually matter for safety, of which there are many. I think it matters that people are running evaluations for especially cyber and CBRN misuse, but also other things like deceptive alignment, misalignment. I think it matters that they are reporting this information to governments and appropriate external bodies. So I think there are things that you can look at there.


I think it matters, for example, that[people are doing the planning](https://arxiv.org/abs/2504.01849) for what they will need to do in the future to address future issues. I think it matters that companies are doing research into AGI safety concerns that might arise in the future, and figuring out ways to deal with that. So there’s lots of stuff that I think you can judge companies on right now , and I would much rather that people judge us based on that, the stuff that actually matters.


**Rob Wiblin:** So the basic message is that it’s bad for resources to be diverted from stuff that is substantively good, that is actually going to solve the problem ultimately, towards things that are kind of going through the motions of appearing like you care. And sometimes people accidentally are requesting that you put resources into the second one, which I guess partly will come from the rest of the organisation, but will in significant part come from the safety and alignment team and its resources and its staff.


I guess people would say they might prefer the second one, because it’s easier to grade or it’s easier to see. And perhaps they don’t feel in as good a position as you do to understand whether substantively any company is doing the right thing on the merits of what will matter in the long term. But I guess you’re saying that’s why you want to have experts like AI Lab Watch or whoever else who can have their eye on the ball, who can spend their time really thinking that through and actually grade it for everyone else so they can understand.


**Rohin Shah:** I think in addition to all of that, which I agree with, I’m also just deeply sceptical of grading that is based on stuff that doesn’t actually matter, something that we wouldn’t actually stand behind as mattering for actual safety.


This feels like the sort of thing that causes the culture overall to be looking at costs or inputs rather than actual outcomes that matter, and results in incentives to appear good, to look good, to make sure your comms say, “We spent 1,000 hours figuring out what to do with this model” — when maybe those 1,000 hours were like, we ran the model on an input and we looked at it and then we ignored the result because it didn’t matter, and we hired some contractors to do this and just ignored what they said. But now we can say that we spent 1,000 hours looking at it.


I’m like, I don’t want these incentives. They seem bad. It seems bad for transparency and candidness. The more that Google or any other company is doing this, the less I’m able to say what it is that I think actually matters for safety. The more I have to do this complicated dance of saying the things that will appease the people who want us to be showing commitment to putting in resources into a problem, the less I can be like, “Here’s the stuff we’re doing that actually matters for safety, and here’s why I think it’s good.” It’s just a poor incentive landscape overall. And I think it’s really quite important to me that we don’t fall into the trap of looking good rather than just actually being good and then justifying why that’s the case.


**Rob Wiblin:** What about an entirely different justification for having thorough detail in the model cards, which is that the rest of the world needs to know for practical \[reasons\]: other researchers over in the Bay Area rather than in the UK get actual research benefit out of understanding all of these things that GDM is doing and what the model looks like. That’s going to help other companies do a better job of their own model cards and their own evals. What do you make of that?


**Rohin Shah:** I think there is some substantial truth to this. Certainly for the purpose of building on the research, it is good to have details, and I think that’s why we publish papers.


Do we need to do this in the model card or the frontier safety report? I’ve gone around talking to policy and governance people especially about this, and I asked them, “What do you actually use our model card and frontier safety report for?” They will sometimes say things like, “It was really good to get more details about how exactly you evaluate for such-and-such risks.” And then I will say something like, “Great, and you know we also have this paper that goes into more details of the evaluation. Probably it was even cited in the model card on the frontier safety report. Did you read it?” Usually they will not even have heard about this paper, so I kind of don’t believe them when they say that these details actually matter to them.


Again, I do think publishing papers is important, and doesn’t need to be tied to the model cards. And some people who I’ve talked to, who are especially more on the technical side and the people actually building these evaluations, do actually find the papers useful and helpful for them to build on. And so I do want to continue publishing the papers where we can.


###


Underrated GDM paper: Training away hidden reward hacks \[01:28:59\]


**Rob Wiblin:** You’ve told me that you think research papers that come out of GDM tend to fly under the radar a bit in general. I suppose in part because GDM is based here in London rather than in the Bay Area, where I guess the greatest amount of socialising and energy around these issues exists. Tell me more about that.


**Rohin Shah:** I think actually now GDM overall is pretty evenly split between London and the Bay Area, but the safety team has historically been based in London. We’re expanding into the Bay Area, but I would still say that the locus of attention is in London. And I think in practice, just a lot of research papers and ideas end up spreading in the community via word of mouth, which we are a little bit less plugged into, which is a bit unfortunate.


**Rob Wiblin:** OK, let’s talk about one of the papers, one of the interesting research results that definitely flew under the radar as far as I could tell. The first one is “[Myopic optimization with non-myopic approval can mitigate multi-step reward hacking](https://arxiv.org/abs/2501.13011) .” I don’t know how that didn’t go viral. What did you accomplish with that work?


**Rohin Shah:** I think the pitch for this work is that you can train a model that doesn’t do a particular kind of reward hacking, even if the overseer would not be able to notice that reward hack if it had happened. So in contrast to things like amplified oversight or debate — those are ones which rely on making the overseer smart enough or capable enough that they can detect the reward hack and penalise it — this one doesn’t rely on detection at all, so it’s actually a bit more robust.


How does this work? What is this magic that we managed to do? This is basically doable based on the idea of myopic optimisation.


The idea is: suppose there is a reward hack that requires you to take two different actions separated in time. For example, maybe if you are tasked with writing some code. You first write some tests that are particularly easy to pass; they don’t actually test everything that is needed. Then you write code that passes the tests, because you didn’t know how to write the correct code that would have passed everything. Both of these steps have to be present in order for the reward hack to actually succeed. If you have one but not the other, you just get penalised.


So the idea is if you treat these as two separate steps (one step in which the tests are written, and then a different step in which the code is written), and you optimise the tests only based on a signal that says, how good do these tests look, and then separately you optimise the code-writing part based solely on how good does this code look based on what has happened previously, then there’s actually no incentive for the AI system to write bad tests.


Normally, the way the incentive would come is that when you’re doing reinforcement learning, the reward on the code part would also be backpropagated through time to also affect the test-writing part. So the AI could learn from that backpropagated reward that if it writes easy-to-pass tests, then it tends to get more reward in the future. So the myopic optimisation part is saying we’re not letting it see what happened in the future; we’re only evaluating it based on what it did right now — so that incentive never comes, and this is why the reward hack isn’t incentivised, even if you can’t detect it. Does that part make sense?


**Rob Wiblin:** Yeah. I feel like I’ve heard this broad idea going back a very long time, which is that if you’re worried that a model might fake a company, or cheat basically at accomplishing the ultimate goal because all it gets is a reward signal of whether it accomplished it or not… So imagine you’ve got the model running a business, and you ask it to make money. And rather than running a successful business, instead it goes and steals a bunch of money, because it figures out that that’s actually a more effective way of increasing its bank balance. That’s a concern that you might have.


One way that you could prevent that is: rather than reinforcing and evaluating the model based on the final outcome, instead you sample from the actions that it’s saying it’s going to take or the actions that it did take, and then you grade them on whether those seemed reasonable and sensible to you, or to some other monitor, in light of the goal that you actually had. And then you wouldn’t get this reward-hacking behaviour. So it’s basically an instance of that broad idea?


**Rohin Shah:** Yeah, that’s right. It’s historically been called process supervision. I think nowadays the term process supervision has gotten a bunch of other meanings as well, which is why we have this a little bit more clear about what the actual technical mechanism is of myopic optimisation, but with non-myopic approval. But yes, it’s a very old idea. I think mostly our contribution was showing that this actually works with existing LLMs and giving examples of that happening. Because as far as I know, there weren’t actual experiments demonstrating this before.


**Rob Wiblin:** I guess an obvious problem with that is you will successfully stop the model from doing scammy things that seem to accomplish the goal, that I guess it would get reinforced for, but actually don’t accomplish the goal that you had in mind.


But on the other hand, you would also block it off from doing stuff that would have accomplished the goal that was actually a brilliant insight that you never would have had. So if you were training the go model, AlphaGo, if humans were evaluating whether the moves were good, then the model actually couldn’t end up exceeding human performance, because they would grade moves that were actually unexpectedly brilliant as bad at the early stage. How do you get around that?


**Rohin Shah:** Yep, that’s right. To some extent, the answer is you don’t get around it. Ultimately, both the incredible, amazing, creative insights that are awesome and the incredible, creative insights that are reward hacks look basically the same to you as the observer. And if you want to stop the reward hacks, you also give up some amount of the competitiveness.


That being said, you know, we call it myopic optimisation with non-myopic approval. The “non-myopic approval” part is basically the part where, when you’re grading this particular step, ask some sort of intelligent overseer — a human, possibly an LLM — to judge this particular step, how good will it be for getting future reward? And the overseer should then take into account everything that it knows in judging how good this is going to be for the future. And what this guarantees is that whatever incentives from the future affect the AI system in this particular step have to be things that the overseer understands.


**Rob Wiblin:** So you’re saying the overseer could see that the hypothetical AI running the business made a lot of money, but they evaluate the process that it went through. They can include that information, but they do it in light of also looking at the process as well?


**Rohin Shah:** Well, I would suggest that they don’t look at whether the company made a bunch of money, and instead they look at the action that the AI system takes, and they predict to themselves, “Will this lead to a bunch of money, yes or no?” — and based on that provide a reward.


Now, the AI system on step one can do lots of things to help the overseer with this. It can give an explanation of how its plan is going to lead to tonnes of money in the future, and then the overseer just has to verify it. You can use things like debate or other AI assistance to make the overseer better at predicting what’s going to happen in the future so that they can give better rewards.


So in the limit, if you can keep improving your overseer, if the overseer becomes sufficiently smart, then you can recover the performance of what you would get with just straight RL backpropagating through time, but without the reward hacks. In practice, we’re probably not going to get that far. But I think you could actually push this quite far, especially just by the most simple thing: getting the AI system to explain why its plan is good is, I think, a very simple baseline that can make this quite effective.


**Rob Wiblin:** Is this approach going to be useful for frontier models any time soon? Could you see companies actually using it?


**Rohin Shah:** I think plausibly. It only matters once you start doing reinforcement learning over multi-step trajectories over a reasonably long period of time. That’s something that I think has only really started this year, and to varying amounts at different companies. It’s not totally clear how much reward hacking is a big problem.


But yeah, to the extent that this sort of multi-step reward hacking does become a big problem, I think it’s quite plausible that this should be used now. And in fact, I think at current capability levels, my guess would be that the non-myopic approval part, rather than being a competitiveness hit as it would be in something like AlphaZero, I would guess that it would actually improve capabilities overall, although at the cost of you need a lot more human input to provide those rewards.


**Rob Wiblin:** It would improve performance because you wouldn’t get the reward hacking?


**Rohin Shah:** Not just because of that. I mean, that is definitely one reason. But I think also reinforcement learning has a credit-assignment problem: you do a tonne of different actions, and then at the end you get a reward. And it’s the RL algorithm’s job to figure out, based on that reward, which of these actions are actually most relevant to that reward.


**Rob Wiblin:** A difficult problem.


**Rohin Shah:** Yeah, it’s a difficult problem. And in some sense, the thing that the RL algorithm does is like, eh, we’re just going to make everything more likely to happen if the reward was positive, or was unusually good, and everything less likely to happen if it was unusually bad.


Whereas with something like MONA \[myopic optimisation with non-myopic approval\], you can be much more granular. You can say, like, “This particular part where you wrote these tests, those were some really great tests: particularly high reward there. But then this part where you wrote the code, you should have been using this particular library. You didn’t. We’ll give you somewhat lower reward on that.” And that can allow for more effective and sample-efficient learning than you would otherwise get.


**Rob Wiblin:** OK, so that’s an increase in efficiency that you get from this approach to RL that might allow it to remain competitive in terms of its raw performance with other, less myopic approaches.


**Rohin Shah:** In theory. Our paper does not really get into questions like this. This is more me speculating about what would happen in practice.


###


Google DeepMind’s actual plan for building AGI safely \[01:40:29\]


**Rob Wiblin:** OK, the second paper from GDM that didn’t get a tonne of attention is called “[An approach to technical AGI safety and security](https://deepmind.google/blog/taking-a-responsible-path-to-agi/) .” It was written by about 30 GDM staff members, or there’s 30 bylines on it.


It’s like a position paper, as far as I can tell, of broadly what does GDM think it’s going to do as it develops AGI — which, given that DeepMind plausibly is the organisation that is most likely to do this, it makes it a bit surprising that people are not interested in this incredibly thorough description of what you think you are and aren’t going to do and why.


It is quite long, but it has a nice 10-minute-long summary at the start that you could use to get an overview if people are interested. So if you want to be ahead of the curve on understanding GDM’s approach to developing AGI, then you could spend 10 minutes doing that.


It’s easy to say that you are going to do a whole lot of different things, but the most difficult decisions in developing a plan like this is figuring out what stuff might some people like you to do that you are committing to *not* do, because you just don’t think it’s a high enough priority. What sort of stuff does this plan suggest that you are not going to prioritise?


**Rohin Shah:** I think probably the biggest category in here comes more from our background assumptions, actually, and how that informs our planning rather than the actual technical approaches.


So one background belief, which we’ve talked about a little bit already, we call it the “approximate continuity assumption.” This is basically saying that AI progress is going to be relatively smooth and gradual with respect to inputs like compute and labour — not necessarily with respect to time, due to the possibility of an intelligence explosion.


And as a result, our sort of meta strategy involves essentially roughly forecasting, maybe not formally forecasting, but roughly in our heads having some sense of what things are going to be potential problems over the next some time period — call it three months, maybe longer, maybe shorter, who knows — and identifying what sorts of considerations might become quite important during that time given the capabilities that we expect to have, and making sure that we’re prepared for those. And if we’re not prepared for that, then possibly slowing down, or pausing development, or talking to governments, trying to do advocacy.


But importantly, we’re not really trying to forecast arbitrarily far into the future all the problems that are going to arise with AI development. The goal isn’t “Know how to align ASI, or else do nothing.” Usually I think of us as looking at a time horizon of, it depends on which particular thing we’re doing, but often somewhere between three months and five years.


The reason for this is just that it’s not actually possible to know everything that’s going to happen with future AI development. It would be sheer hubris to think that we had figured out every problem that possibly will arise with superintelligence ahead of time and say we’ve solved it — or even say we have a plan for solving it. We haven’t even identified all the problems, I’m sure.


One example I like to give about this is a much more esoteric anthropics type of issue where EAs \[effective altruists\] or longtermists like to talk about essentially how considerations about the multiverse should affect what we do today, and think about anthropics using things like[SSA and SIA assumptions](https://en.wikipedia.org/wiki/Anthropic_Bias) and how they affect what actions we should take.


**Rob Wiblin:** You don’t have to understand what that means in order to track with the point you’re about to make.


**Rohin Shah:** Yeah, yeah, yeah. The key upshot of these things is that the beliefs and decisions that you come to actually depend on who you are. This is one of the crazy things about anthropics. Normally, different people should agree on beliefs given enough evidence, or at least perfect Bayesians should agree given the same evidence. They should have the same beliefs. This stops being true in the case of anthropics. And indeed, an AI will probably have different beliefs than a human would, even if it’s perfectly aligned, just like the structure of how Bayesian updates happen in a world with anthropics is different for AI systems than for humans. So this should affect whether we are willing to delegate research on these kinds of topics to AI systems or not, even if they’re perfectly aligned.


That’s a kind of wild, crazy problem that definitely is not a problem today but really might come up at some point before a superintelligence, and I don’t want to have to say, like, “Yes, I have identified all problems like this and have an approach to solving all of them today.” Obviously we should just take these as they come up.


**Rob Wiblin:** So some people would really like you to plan ahead a lot. They would like you to have a grasp on all the important considerations that are going to come up, all the way through to artificial superintelligence until I guess the point where you could feel like you’re safely handed everything over.


And you’re saying, “We are not going to do that, not at all. We’re going to think a lot about the next model that we’re training. We’re going to think a bit about the model after that, and kind of beyond that we’re going to hope that the future will take care of itself, or we in the future will take care of things as they come up.” That’s the point you’re making.


**Rohin Shah:** Yes, though I think we’re longer-term thinking than that made it sound. Like I said, often I’m thinking five years into the future. That’s many, many, many models, right? I say chain of thought monitoring, I think I said four years is my median that that lasts. We’re already thinking about what to do after the point that chain of thought monitoring stops being useful. We would want to do broader control mitigations at that point.


So certainly our time horizon is not incredibly short. I’m just saying it’s not all the way out to superintelligence. We are doing a fair bit of long-term planning. Perhaps you would call it medium-term planning.


**Rob Wiblin:** Yeah. This paper is pretty candid for a company position paper. It very clearly says AGI might be here by 2030. I guess we don’t know when, but 2030 is totally plausible. We have to be ready with a plan for how to handle that. That’s very dangerous or potentially could be very dangerous. We have to have a plan that we can deploy incredibly quickly to try to make that safer. And then it goes through the kinds of things that you’re intending to do. I would encourage people, if they work in this area, to go and take a look at it.


Are there any non-obvious mitigations in there? I guess one thing that I was glad that you said, that not everyone would highlight, is you need to treat an AGI or an AI inside the company, that’s just only being deployed internally, if you’re not sure or not confident of its alignment, then you need to treat it as like an untrusted insider, as potentially a hostile agent that is going to try to undermine internal systems.


Do you want to highlight any of the things you are committing to doing?


**Rohin Shah:** Yeah. Well, I maybe want to be a little wary about the word “committing.” *Planning* to do. But yes, I think the untrusted insider stuff is definitely particularly interesting. We didn’t go into quite that much detail in the paper actually, which is a bit sad. Mostly our thinking wasn’t quite as developed on it as it is now.


But basically, if you actually think about the mechanics of what an AI system would have to do in order to take over, it’s quite hard. Among other things, it probably wants to be able to make sure that it isn’t monitored as it starts thinking about what to do, so maybe it wants to set up a deployment where all the monitors are turned off that we don’t even know is happening.


Perhaps it could exfiltrate its weights and then run itself on different compute that we don’t control. But if it has to exfiltrate itself, how is it going to do that? Google security is not trivial to evade. In fact it’s pretty hard. It’s pretty good security. So there’s actually just quite a lot of increased defence you can get by making sure that you are making your security also apply to AI systems as untrusted insiders.


So things that you would want to do could include having infrastructure that requires agents to have separate identities from the human employees on whose behalf they’re going to be doing things. This would be like you are starting an agent on some task. It doesn’t get all the permissions that you as a human employee have; it gets some much smaller set of permissions. It will need some permissions in order to do its task. It has to request each of those individually, write a justification for why it needs that particular permission. This allows you to incriminate it afterwards if it was lying about it. Well, it helps you incriminate it, maybe doesn’t guarantee it.


Then similarly, you can have some monitoring, where you have a chain of thought monitor right now, for example, that looks at what the model is doing, sees if there is anything potentially suspicious about this. If there is, any things that are sufficiently highly suspicious get flagged to humans to be audited. This allows you to also generate evidence of misalignment. If the AI system is actually misaligned and you audit it, and you’re like, “Oh my gosh, yeah, this AI system totally was trying to inject a security vulnerability and then would have exploited it in order to exfiltrate its weights,” that’s a big deal. I’m not sure I expect that to ever happen. Seems quite plausible that models will never be misaligned. So if we get that sort of evidence, I think that would change my mind a big deal and like many other stakeholders’ as well. So I think that’s quite crucial to get.


So yeah, lots of stuff like this, and I think it’s important to do and very nascent — so lots of good work to do here in the future. And I should note that you’ve had[Buck Shlegeris](https://80000hours.org/podcast/episodes/buck-shlegeris-ai-control-scheming/) and[Ryan Greenblatt](https://80000hours.org/podcast/episodes/ryan-greenblatt-ai-automation-sabotage-takeover/) on the podcast before. This is the AI control work that they talk about. It’s just on our side a little bit more focused on the infrastructure that we have to build in order to enable this sort of approach, rather than the specifics of exactly how we’re going to build the monitor and so on, which is a more standard machine learning problem that I think will be relatively easy to do in comparison.


###


Why Rohin doubts the intelligence explosion is imminent \[01:52:44\]


**Rob Wiblin:** Let’s talk about timelines and recursive self-improvement loops for a bit. There’s been a lot of discussion recently about when we might expect a recursive self-improvement loop to occur — indeed, if one is possible at all. You think that people have been a little bit sloppy in their thinking about this in some ways, and in ways that cause people to maybe expect it to happen sooner than you do. Explain that.


**Rohin Shah:** Yeah. So I think one of the most striking things about reality or the world that I’ve seen is just the examples of straight lines on graphs going straight. I think[Scott Alexander put it well](https://slatestarcodex.com/2018/10/15/is-science-slowing-down/) . I don’t remember which post this was, but I think he says something like, “I don’t understand the Gods of Straight Lines very well, and honestly, they kind of freak me out, but I’m not going to bet against them.”


And one particular straight line that we’ve been seeing a lot recently is just sort of constant, roughly 3% GDP growth per year. Now, there are good arguments for why that won’t necessarily last, and instead probably we will see an intelligence explosion at some point which would drastically increase the rate of growth. But I do think that when reasoning about this, it’s quite important to say what exactly about AI makes it different from the current situation? Why are we betting against the Gods of Straight Lines?


Now, I think the usual answer to this, which is based on economic endogenous growth models, maybe most famously from a[paper from Kremer in 1993](https://faculty.econ.ucdavis.edu/faculty/gclark/210a/readings/kremer1993.pdf) , is that there are a couple of effects:


- One, as technology improves, that allows you to find new ideas more quickly. Once you have the microscope, you can do biology significantly better.
- Another effect is that[ideas get harder to find](https://www.aeaweb.org/articles?id=10.1257/aer.20180338) as time goes on because you pluck the low-hanging fruit. You know, initially you discover phosphorus by,[I think Scott put this well again](https://slatestarcodex.com/2018/11/26/is-science-slowing-down-2/) , by looking at your own pee. And then later you have to discover element 110 by shipping samples from one country to another to be studied in the one laboratory in the world that can do it. Obviously that’s much harder.


I think basically in the current setting, where we see this constant exponential growth in GDP — and similar things in various areas of technology, like Moore’s law being a good example — the argument would be roughly that ideas are getting harder to find, but also we have exponentially increasing numbers of researchers, and together those two things balance out in order to create the sort of constant progress.


But if you look sufficiently far back historically, it actually looks like growth has been increasing substantially over time. So what is this secret third effect that we haven’t taken into account so far? The argument is that the rate of growth of technology increases partly with more technology, because you get microscopes and they help you do things better, but also increases with respect to the population, because as the population grows there are more people who can have ideas. And ideas, once you have them, you can copy them freely, they can diffuse very quickly — and those too can increase technology.


So technology’s rate of growth is modelled as being proportional to both technology, or sometimes technology raised to some constant, as well as population. This then predicts basically hyperbolic growth in both population and technology.


And the argument is, for the last however many years, the population part no longer applies — because instead of reinvesting all of our output back into more kids, we’re now reinvesting them into quality of life improvements. That’s why we see a hyperbolic up until 1800, 1900, I forget the exact point. And then, since then, exponential growth.


But this story really puts the primacy on ideas rather than anything else. Those are the things that are freely copyable, those are the things that you have them once and then they apply to all of your technology across everywhere that you’re using it. So when I think about what’s going to lead to the intelligence explosion, I think the increasing population or increasing labour that can have ideas as pretty crucial to it.


In contrast, a lot of the discussion today talks about the superhuman coder, for example: where you get an AI system where you can say, “Please implement XYZ experiment for me,” and it just goes ahead and implements that experiment incredibly well and much faster than humans could do it. The argument is once you can do that, the rate of AI progress increases a bunch, and you start getting to the intelligence explosion quite quickly. At least I think that’s the argument. It’s always a little bit hard to interpret the wide community view, which has a lot of people with slightly different opinions.


I don’t super buy this view, mostly because the superhuman coder is not something that can have ideas across the spectrum, across everything that happens in ML R&D. It has ideas within the realm of coding in particular, which is one part of ML R&D, but definitely not even the majority of it, according to me.


So I think it’s better to think of this as more like inventing the microscope: you’ve got a tool that enables your research to progress faster. But we do this all the time in all sorts of research areas. For the last however many decades, it has not led to hyperbolic growth. The invention of tools is just a normal part of research progress, and usually tends to lead to nice stable straight lines on graphs. So I generally don’t think we should be predicting big changes from the invention of tools.


Another example is sometimes people will suggest that a trigger for the intelligence explosion is the point at which AI researchers are, say, 10x more productive than they would have been if they had no access to AI systems at all, or had access to AI systems from like 2020 or something like this.


And again, I think this is the sort of thing that probably has happened for something like chip development. So Moore’s law is a nice, clean, straight line for many decades. But I assume at the beginning of that line, people were designing their circuits by hand on paper. And nowadays we have these incredible computer-aided design software tools that automate the vast majority of this kind of work, and you just see the line continuing to be straight. In fact, you need exponentially more people working on this in order to have that happen.


So are the researchers that we have today 10x more productive than the ones from two decades ago? Probably, that would be my guess. Is that a sign of an intelligence explosion in chip design? No. And I think a similar thing might be true in AI. In fact, it’s kind of unclear. We already use language models quite a lot, for example for auto rating and for conducting evaluations, and auto rating the responses of other models. How much less productive would we be without that? I don’t know. Probably not 10x less productive, but like a substantial amount. But AI progress continues to look pretty linear, according to me.


**Rob Wiblin:** OK, let me recap all of that. So we currently have exponential economic growth, which is to say a constant percentage growth each year on average, looking over the medium term.


People who say there’s going to be an intelligence explosion, there’s going to be a recursive self-improvement loop, they’re forecasting increasing rates of growth, or hyperbolic economic growth: so it’s 3% one year, then 10% the next, then 50% the next — up to some point, I guess, at which you might think it levels off or comes back down again.


What are the high-level factors that lead economic growth to either be stable or to increase or to go back down? There’s three factors that you have in mind in your model, and that most economists have in their model:


- As technology advances, it gets difficult to make new useful discoveries and to advance it further. That’s the first one.
- The second one is, as technology advances, we have better tools to do science and to make new discoveries.
- The third one is, as technology advances and the economy grows, we can support a larger number of people who will do the research and have the ideas and advance science.


In recent times, the third one has kind of been out of the picture, because we haven’t been turning advances in science or improvements in the economy into new people.[Birth rates have been going down](https://80000hours.org/podcast/episodes/rob-luisa-parenting-chat) rather than going up, despite us being richer. So it’s been the first two factors that have been playing off against one another: advances getting harder to make, and science advancing and giving us better tools to do it. These two things have been roughly cancelling out. And over the medium term, growth has been about 3% a year, maybe going down In recent times.


Zooming out much further, all three factors were in play. Before the Malthusian era ended, as we got richer, we drove almost all of those resources into additional people as well. But that faded out after 1800. And that was leading to hyperbolic growth, that was leading to increasing rates of growth, while it was true.


Now, applying this to the AI case, that means that, to figure out which situation we’re in, this mental model puts a huge emphasis on are we coming up with better tools to do science, or are we ploughing our gains back into more researchers to do science and to have more people thinking up better ideas or new ideas?


You can see why it gets a bit confusing here when we’re talking about AI doing the research, because AI is both a tool, but we’re thinking it’s going to basically converge and become people or become scientific researchers itself. So it becomes a difficult conceptual, empirical question: at what point do they cross over from being largely a tool that is assisting humans in doing their work to being the researcher itself that isn’t really just assisting people, it’s doing the whole process? Or at least we should no longer be just considering it as a scientific instrument that’s allowing us to do better work.


And I think you want to say people are being a bit sloppy. They talk about indicators that really are a sign that we’ve developed better tools for humans to use to do their work, and they kind of conflate that with autonomous AI R&D that barely even needs people at all and really should be considered as a population increase that then can drive hyperbolic growth. Because as you get improvements, then you can run even more. You can basically expand the effective population of researchers by running more copies of the model. Have I understood right?


**Rohin Shah:** Yeah, that’s right. That’s very impressive. I was worried that I was soliloquising for way too long, but great summary.


**Rob Wiblin:** Thank you. OK, so how do we tell whether we’re talking about a better tool or about more population? It does seem like there is kind of a fuzzy barrier here, and we’re going to go from one to the other, but there’s not going to be a sharp cutoff, I imagine.


**Rohin Shah:** Yeah, I agree. There probably won’t be. I do think that to what extent are the AIs proposing new ideas are the things that I think most influence the hyperbolic growth prediction. That’s one thing you could be looking at, and I would say that the superhuman coder probably doesn’t really hit that bar.


But really what I want to do is just measure AI progress and then notice when it starts accelerating. Actually we worked with[Epoch](https://epoch.ai/) recently to develop exactly this kind of a measure. The paper, I think was just released, it’s called “[A Rosetta Stone for AI benchmarks](https://arxiv.org/abs/2512.00193) .”


Essentially, we just take the benchmark scores that a model gets for a wide variety of models, and then we sort of stitch the benchmarks together to get a sort of general capabilities score for models that applies over the entire range — from back in 2020 all the way to models that were released now, even though any individual benchmark would have saturated over a much smaller period.


Then you’d take the capability scores that this measure spits out, and you plot them against the release time for the model. These capability scores, the statistical model that produces them, we don’t include any information about the release date or any time-based information into it, just benchmark performance. And nonetheless, when you start plotting this against release time, it’s just a nice line. It’s great. And you see that AI progress mostly looks pretty linear on this particular graph.


And the method is really very simple. The main thing that it depends on is that we have benchmarks that can capture AI systems’ performance, and that I think will continue to happen in the future. So we can just continue to plot this, and then one hopes that if an intelligence explosion does seem like it’s happening, we will start to see an acceleration on that, rather than it looking just like a nice linear fit the entire time.


**Rob Wiblin:** So how do we tell if we’ve just made a better tool or we’ve made a whole lot of new researchers? You’re saying the proof is in the pudding. Let’s not leave it to the philosophers, let’s leave it to the people doing benchmarks to figure out whether AI advances are actually speeding up.


**Rohin Shah:** Empiricism.


**Rob Wiblin:** A lot of people have the perception that AI progress has been slowing down. You’re saying you’ve tried to create an enormous dataset of as many different models over the last four or five years as possible. This is with Epoch: this is their wheelhouse, doing this kind of data collection and compilation and aggregation. So they’ve tried to collect all these different benchmark scores for many different models, going back quite a long way to see is progress speeding up or is it slowing down, or is it roughly linear?


And you’re saying, at least judged by that measure, the best effort they can do says that it’s linear. Which is to say that we’re not making better people, we’re making better tools. For now, that’s what that would suggest.


**Rohin Shah:** Yep, that’s right.


**Rob Wiblin:** So there’s all kinds of problems with benchmark scores. One thing could be that they don’t capture the full range of performance — that either you end up flawed at the bottom or capped at the top. Especially because we’re talking about models here over many years doing many different things. There’s definitely lots of gaming that goes on, lots of teaching to the test that occurs with people trying to make their models look good on these benchmarks.


I guess there’s also a question of what actually matters? There’s benchmarks for all kinds of different skills, and maybe you should give some of these things much more weight than others. I guess you could also have non-linearities in the effect: the performance of a model and its economic effect that could be, indeed probably is, quite nonlinear.


How good do you think this whole approach is, that Epoch and you have been using to figure out whether progress is speeding up or remaining about the same pace, at getting at the ground reality of what’s going on?


**Rohin Shah:** I basically agree with all of the critiques you mentioned. It’s going to inherit any problems that benchmarks have because ultimately the only input into it is benchmark performance. Nonetheless, I think a lesson that I learned from the Gods of Straight Lines is that for some things, yeah, there are lots of nuances and details that matter a bunch if you want to make very fine-grained predictions — but at a high level they wash out, and it ends up being fine anyway. I think that mostly applies here.


**Rob Wiblin:** An example might be that you’d say the models are being gamed, there’s a bunch of teaching to the test happening now, but there was a bunch of teaching to the test happening two years ago and four years ago. So as long as that’s not getting progressively worse, then the line is still reasonable.


**Rohin Shah:** Yes. And also I think in practice the teaching to the test won’t change the results very much on this. It definitely changes it some. Anthropic is probably the best at not overfitting to the benchmarks that exist. And in fact, if you look at the scores that are produced, I think it does tend to underestimate Claude or Anthropic’s models relative to models from other providers — because generally Claude models, since they are not overfit to the benchmarks, will tend to underperform on benchmarks relative to how good the model actually is.


So yes, that’s true. But also, if you look at it on the graph, it’s a tiny little difference. It’s not that big. If you moved it up, it would still look linear. It doesn’t really change the “whether it’s linear or not” observation.


**Rob Wiblin:** You’re saying increasing rates of progress would be quite striking on the graph. It probably would jump out at you, and these effects wouldn’t be enough to make it disappear.


**Rohin Shah:** That’s right. But I definitely would caution people against using this score for really fine-grained things, like how good is Claude versus Gemini versus whatever. Or, for example, trying to understand the difference between open source models and closed source models — because I think the open source models are probably more overfit to the benchmarks than the closed source ones, and if you try to use this score to look at the exact difference between those two, it’s probably going to mislead you a little bit.


**Rob Wiblin:** So at the point that AI is people, or at least is AI researchers, rather than just being a tool for AI researchers, you might reasonably expect quite abrupt increases in progress in AI R&D and AI capabilities, basically. Are you a downvote on how abrupt that will be or whether that will occur at all? Or do you buy that maybe it will take a bit longer than people are imagining, but you do still think that will happen?


**Rohin Shah:** I’m a slight downvote on the abruptness. I am not a downvote on, will there be an intelligence explosion?


So what do I feel pretty confident about? It will be some really strong statement with a lot of strong preconditions, like: “When you have AI systems such that for almost any economically valuable task you care about, you would prefer to hire the AI system rather than the human” — which, among other things implies that the AI is cheaper than the human, which I don’t think is a given — “at that point, assuming that we don’t take some sort of action to try and prevent it, and that in fact, we are trying to use the AI systems to substantially accelerate research and development across the board (not just in AI, but all of the economy), then probably within a century, we will have some kind of intelligence explosion and reach technological maturity.”


That’s so many preconditions. It’s still kind of a crazy statement. I’m saying we will have an intelligence explosion at all, and I feel actually pretty confident about that. But it is a lot weaker than the thing that I would guess will actually happen, which will be substantially faster, and happen substantially earlier than what that statement would imply.


What I would actually say is: a decent chance that it starts at the point where the AI systems can automate most of AI R&D, as opposed to all arbitrary R&D; a decent chance that it finishes over five to 10 years rather than a century, depends on exactly what you set as the starting point; and so on.


Going back to your original question about abruptness, the reason I’m a slight downvote on abruptness is that I would expect that actually the first automated systems that can really automate AI R&D will probably just be very expensive, to the point of potentially being more expensive than humans would be. You can see this with over the last year there’s just been way more investment in inference time compute, inference time scaling. Google has a Deep Think algorithm which applies even more inference scaling to get even better results. I think you should basically expect this to continue.


So the picture of the first automated researcher might be something that’s more like a relatively dumb system that’s not as “smart” as a human researcher, but spending just tremendous amounts of time doing reasoning, exploring tonnes of dead ends, realising they’re dead ends and then coming back and trying something else — which a human would never have gone down, because they would have known in advance it would be a dead end. And that’s how it does its automated research, such that it might even be more expensive than a human researcher would be.


And then we improve it over time. The cost goes down pretty quickly, as is usually the case in AI. But that would suggest that it won’t be that abrupt. Like the AI will reach cost parity with humans, then start becoming more cost effective than the humans, and that will start setting off the acceleration.


**Rob Wiblin:** But that’s how it ends up happening over quite a number of years, rather than months or something crazy.


**Rohin Shah:** If you take it from the point at which the AI systems start being just barely on par with existing human AI researchers, then yes, that’s right. That’s why I would think it would take years rather than months. But most of my timelines delay is just thinking that even getting to that point will take quite a while — quite a while, like a decade maybe, which I feel like in past years would have been called “extremely short timelines” and nowadays gets called “medium to long timelines.”


**Rob Wiblin:** Yeah. I think there’s been a general phenomenon over the years where I guess every couple of years there’s kind of a freakout about AI timelines, and people start expecting a recursive self-improvement loop really quite soon, within a few years of that point. My impression is that you’ve just been unmoved in either direction. Why haven’t you updated based on events that have occurred, results that have come out?


**Rohin Shah:** I would guess probably the biggest difference is that I had a picture in my mind about how AI progress would happen and reality has been reasonably close to it.


For example, I think the biggest timelines freakout, the timelines freakout in January, let’s say, was from the advent of reasoning models o1 and then particularly o3 — where I would say the key idea there is “let’s actually apply reinforcement learning to large language models.” And I have, for quite a long time — I’d say probably since at least 2019, but maybe even earlier than that — thought that, yes, of course we are going to need to use reinforcement learning in order to develop powerful AI systems. If you look at much of the work that was done at the time, things like debate, those are implicitly predicated on reinforcement learning being the method of choice for building powerful AI systems. Debate just looks pretty pointless if you’re not using reinforcement learning.


So I was always expecting reinforcement learning to happen. And from my perspective, everybody else was suddenly surprised by reinforcement learning happening. Whereas I looked at it and I was like, well, it could have been the case that once we do reinforcement learning it just generalises beautifully to everything — the same way that instruction following really does generalise beautifully to everything — and actually that was not the case.


So I think mostly my timelines didn’t change very much because I already thought it was reasonably likely RL wouldn’t generalise to everything. But I think if I had been tracking it sufficiently fine grained, I would have probably updated slightly towards longer timelines on the release of o1 or o3.


**Rob Wiblin:** And why doesn’t the general performance of reasoning models… I mean, I think that’s one way of characterising the update: that people were surprised or they were shocked that RL was being applied to these models into reasoning. I think most people would say that they were impressed by how good the reasoning was and how useful it seemed like it was going to be. But it sounds like you weren’t impressed by it. It wasn’t surprisingly good to you.


Or maybe it’s that it wasn’t as generalisable: that they were good at the reasoning tasks that they had been RL’d on, but you didn’t expect that was going to generalise to other tasks and be very economically transformative — and indeed it has not.


**Rohin Shah:** Yeah, that’s basically right. I think the generalisability was the big deal for me. If you want to target some particular benchmark and apply machine learning to it, I think the lesson of machine learning is yes, you can do it. People do choose the ones that models are capable of doing, so it’s not like you can choose some arbitrary thing and just hit it with the machine learning hammer and succeed.


But I do think you have to be pretty careful about if somebody optimised for a specific thing, how much should you update from that to AGI, the fully general intelligence? It’s really quite a difficult update to make, and you should be looking quite a bit at the generalisability.


**Rob Wiblin:** OK, so that’s the thing that would cause you to have a timelines freakout: if you trained models on one kind of practical task, and then you found that they were actually good and useful at doing quite different kinds of tasks?


**Rohin Shah:** Yeah, I think that’s right. And you do see a little bit of this from reasoning models, to be clear. It’s not like it generalises not at all, but not as much as I think would have actually been a substantial update for me.


I think in particular I was looking at autonomy tasks, and nowadays I think the reasoning models can be pretty good at autonomy tasks. Well, actually, relative to expectations I think they’re still underperforming, but nowadays they’re substantially better than they were at the time. But partly that’s because companies have started to train for autonomy.


###


Advice for external researchers who want to impact big AI companies \[02:21:55\]


**Rob Wiblin:** Let’s talk about Google DeepMind — which, for a very important organisation, I feel like isn’t super well understood by people outside, including listeners and I would say outsiders just in general.


A lot of people outside of AI companies produce research, both governance research and technical research, hoping that it will be read and absorbed and adopted and used by those companies, including GDM. What can people do to make it more likely that anything that they do actually is read at all, or is used at all by people inside an AI company?


**Rohin Shah:** Yeah, there are definitely a few things that people can do. And I think again, at this point it is useful to bring in the model of: there are just all these incredible constraints that interact with each other a tonne, and as a result we can only do a few things, and need to do a decent amount of work in order to get them through. Which you could well predict as the meme of “companies are apathetic,” though I think it’s maybe a little bit better to say it as “companies are well motivated but can only do a few things.”


Keeping that in mind, a few things are worth doing. One is just talk to somebody at a company before you put in a bunch of time into research. Just reach out to somebody who works at a company, say, “This is what I’m planning to do. Do you think this will actually be useful?” It’s a very simple step, but surprisingly many people just don’t do it. But it does seem like probably the best thing you can do in order to actually increase the chance that your work gets used at a company, at least currently.


**Rob Wiblin:** If people consistently did that, do you think a lot of your time would then be eaten up replying to these emails?


**Rohin Shah:** If they email me, they will probably not get very much of a reply, because I get way too many emails. I think most of my reports would be fairly excited to talk to people about this. I admit that I, at this point, get enough of these that it feels a little bit more like a burden than an exciting thing. But it definitely used to feel like an exciting thing before it became very common.


**Rob Wiblin:** OK. What should people do other than email to ask whether what they’re doing is going to be useful?


**Rohin Shah:** So other things, I had a talk recently on “[How to theorize so empiricists will listen](https://www.youtube.com/watch?v=BWHbxv5kxLI) .” It could equally well have been called “How to do safety research so that companies will listen.”


The basic points from this, the first one — the most obvious one, but it’s still worth asking yourself — is like, do they actually care? Sometimes people do research on problems that we actually just don’t care about and don’t think matter. I think the safety community is fairly good about not doing this, but it does still happen, and sometimes people might be a little bit surprised by what we do and don’t care about.


For example, take jailbreaks. We care a lot about jailbreaks now because the models are looking to be strong enough that misuse could be a serious problem. But if you look like a year or two ago, people were doing a bunch of jailbreak research, and saying this means that the companies aren’t very good at aligning their models because they’re so susceptible to jailbreaks.


And I don’t know about other companies, but at least at GDM, basically our stance on jailbreaks was that there’s not any actual misuse scenarios that we’re particularly worried about, given the model capabilities. What safety is about is about protecting users from cases where the model does something unintended and harmful. So for a while, there was a bunch of discourse about how models are always going to be jailbreakable, and it’s totally impossible to defend against it. And the real answer was just like, we hadn’t even tried to stop the jailbreaks.


**Rob Wiblin:** OK, but you’re trying to stop it now?


**Rohin Shah:** We are trying to stop it now, yes. Now, if people want to give us research on jailbreaks and how to defend against them, we will definitely use it.


**Rob Wiblin:** I should say all this advice is predicated on the idea that you’re doing research hoping that an AI company is going to read it and absorb it and use it. There’s people who will have their own different ideas, and they’ll be developing stuff hoping that people will become persuaded later on that it’s useful or it’ll be valuable in a different way.


**Rohin Shah:** Absolutely, yeah. Lots of different theories of change for research. I’m definitely only talking about if you want a company to use it basically now.


Anyway, so that was step one: “Do they actually care?”


Step two I call “Are you helping?” But it’s like a very specific flavour of helping. Specifically, I think you should either be proposing a solution, or building an evaluation or a metric of some kind.


Now, there is research that is not these things, right? There is research that does some sort of theory to understand some phenomenon better, that will give you some increased understanding of the phenomenon, but that doesn’t solve a particular problem, doesn’t produce a metric or eval that the company should care about.


There are lots of other examples of research like this. I think basically most of that research we are unlikely to use unless it happens to really be on a core problem that we care about a lot. Given that we are all busy and it takes a lot of work to incorporate any new thing, usually it needs to be a metric or an eval or some kind of solution.


**Rob Wiblin:** So if people are doing interesting theorising or preliminary empirical work to understand a phenomenon better, you’ll be like, “That’s all well and good, but come back when you have a solution”?


**Rohin Shah:** Roughly, yes. And again, I think this is good research to do and people should do it. It can help develop better solutions in the future. I just don’t think they should be thinking that —


**Rob Wiblin:** You’ll spend a lot of time reading it.


**Rohin Shah:** Exactly, yeah. Next thing after that, especially if you’re going to be proposing a solution, the next thing to be doing is evaluating your solution — or at least conceptually evaluating your solution, even if it’s not by experiments — on the various metrics that companies care a lot about. Most of the time research will look at did it actually solve the problem that it set out to solve? Definitely important, you’ve got to do that evaluation. That is the most important one.


But then there’s also things like: How much cost does this add in terms of compute? How much latency will it add? If you’re imagining a step that runs after the AI system has produced a response, and then you do lots of additional things before you then have to send the response to the user, it’s probably a nonstarter. Not obviously, but it’s a big cost.


There’s implementation complexity, or organisational complexity. If your solution involves doing something at the scaffold level and also doing something that involves reading the internals of the AI system and connecting these up together, that just spans so many different teams, and so many different abstraction layers in the stack, that it’s going to be much harder to implement than something that, for example, just happens at inference time and involves running a monitor and then alerting some teams about it.


**Rob Wiblin:** Are there any other examples of things that are easy to implement other than that one?


**Rohin Shah:** Yeah, I think there are several. For example, if their datasets can be a useful contribution, it’s pretty easy to try to add a dataset to post-training. Sometimes you try to add it to post-training and then for some reason it makes the model worse on some totally unrelated thing, and then you can’t use that dataset, which is a bit unfortunate. So ideally, when you’re building datasets, you would like to evaluate whether it could have some sort of negative effect on something else. But this is a bit hard to do.


But if someone came to me with a dataset and said, “Training on this dataset improves this metric by a decent amount, and doesn’t seem to have any bad effects on these three obvious other metrics that it might have had bad effects on,” I would find that fairly compelling and think, yeah, maybe we should do it, assuming it was solving a real problem.


So I think datasets, metrics, evals, monitors: these are usually fairly relatively easy things to do. I think all of those are fairly easy to implement if we think it’s worth implementing.


**Rob Wiblin:** Any other advice?


**Rohin Shah:** One is just like don’t do the academic thing of using a fancy method to solve your problem, and instead solve it with the absolute simplest method you can.


**Rob Wiblin:** I guess you’re saying cutting-edge stuff might be useful in some other way, that you advance the science somehow and could be valuable down the line, but if you want people to be using it on any actual commercial models anytime soon, then it has to be as simple as possible and well established as possible.


**Rohin Shah:** That’s the generous version, yeah. The cynical version is more like academia rewards complex, formal-looking stuff and poorly tuned baselines. It won’t reward poorly tuned baselines if it knows they’re poorly tuned, but it’s kind of hard to tell if a baseline has been poorly tuned or not.


**Rob Wiblin:** What’s a poorly tuned baseline?


**Rohin Shah:** A poorly tuned baseline is like you have some default way of solving the problem that you are trying to do better than.


**Rob Wiblin:** Do you make the baseline look bad by not doing a very good job of it?


**Rohin Shah:** That’s right. But not intentionally. It will often be like you implement the baseline once, and then you don’t tune the hyperparameters for it, so it performs less well than it really should. It’s just very easy to not spend enough time working with your baseline to make it work well. And then as a result —


**Rob Wiblin:** It makes your other thing look better.


**Rohin Shah:** Exactly. And given the incentives in academia of publishing novel things and publishing a lot, I think this is a fairly common problem. So I think if you want to publish, that tends to be the thing that you do. If you want your research to be used by people at companies, it’s quite important that you try the obvious stuff, and you try fairly hard with the obvious stuff. And only if that really does fail do you try to do something fancier.


**Rob Wiblin:** What external research has been most useful to GDM so far?


**Rohin Shah:** Probably the one I would point to most is the[AI control work from Redwood Research](https://www.redwoodresearch.org/) . I think we were always planning to monitor our AI systems, it’s not like the idea of monitoring was new to us, but the specific conceptual frameworks they brought to how you might evaluate how well this works, specifically the distinction between trusted and untrusted models, I think that was quite good.


The[first paper](https://arxiv.org/abs/2312.06942) they published on it showed how a variety of different control protocols, how you can evaluate their safety and their usefulness and use this to decide which one you should be doing. I think that’s influenced me quite a lot on how exactly I think about the control work. I think that’s probably the most obvious example.


**Rob Wiblin:** So this is the[Buck Shlegeris](https://80000hours.org/podcast/episodes/buck-shlegeris-ai-control-scheming/) and[Ryan Greenblatt](https://80000hours.org/podcast/episodes/ryan-greenblatt-ai-automation-sabotage-takeover/) episodes earlier in the year — Buck Shlegeris in particular, talking about the AI control agenda.


I feel like that episode didn’t make quite as much of a splash as I was hoping or expecting at the time. But the amount that people in the industry keep constantly referring back to it I think means that if people didn’t listen to it at the time, because for whatever reason it just didn’t sound quite exciting enough, I think give it another look, maybe go back and have a listen. Because I think it is very important foundational work that is only getting more relevant.


**Rohin Shah:** Yeah, definitely. I would say that control is, in the medium term, probably how we are going to argue that Gemini is safe, at least for misalignment and internal deployments. I think probably via control style arguments. So yeah, definitely an important area to be aware of.


Other papers: there was this[paper on observational scaling laws](https://arxiv.org/abs/2405.10938) , which mostly I think you can think of it as a precursor to the[Rosetta Stone paper](https://arxiv.org/abs/2512.00193) that we talked about before where we stitched together benchmarks to get a way to measure overall AI progress. I think the observational scaling laws \[paper\] thought of this more as like, how can you forecast particular benchmark results? That’s also a pretty good application. I think mostly it was an area we hadn’t really been thinking about, and they showed that you can do something here. And I think that probably influenced me when thinking about the benchmark stitching / Rosetta Stone approach. So that one was pretty influential on me.


Other examples would be the various chain-of-thought monitoring papers that have come out. I don’t think that I can point to any one paper that I thought was a particularly big deal, but in aggregate, I think they’ve built a fairly good research literature on the effectiveness of chain-of-thought monitoring, when it does and doesn’t fail. I think there were some papers that looked at to what extent can you just persuade the monitor that actually everything’s fine here and you don’t need to flag anything in this chain-of-thought monitoring. I think that was a useful thing to study, and the specific quantitative results in it were informative to me.


###


The most in-demand roles at GDM \[02:37:03\]


**Rob Wiblin:** I imagine a lot of people would love to get a job at GDM, and even people at other AI companies already might be interested in considering switching, given the advances that GDM has made in its models in recent years. What sort of roles are you finding hardest to fill? And what sort of skills are maybe hardest to find in the labour market?


**Rohin Shah:** I think I’ll go back again to the theme I’ve had throughout, of the challenge being more in implementing stuff rather than in figuring out what stuff we need to do. We have a lot of ideas, we know what we need to do. Implementing it ends up being harder, because there’s so much stuff you have to check and make sure that you’re not hurting it.


And as a result, I think especially over the last year, maybe two years, I think we’ve had more of a need for people who just want to do the obvious thing and land it, as opposed to people who want to figure out the ideal, optimal thing and write a cool research paper about it.


**Rob Wiblin:** We’re talking about the AGI safety and alignment folks, right?


**Rohin Shah:** That’s right, I am talking primarily about the AGI safety and alignment team. I think focus on just doing the obvious stuff, a focus on implementation rather than research. I think in practice this also means more of a focus on[software engineering](https://80000hours.org/career-reviews/software-engineering/) relative to[machine learning research](https://80000hours.org/career-reviews/machine-learning-phd/) .


I do still think that we do care about conceptual ability,[research taste](https://80000hours.org/podcast/episodes/neel-nanda-career-advice-frontier-ai-companies/#what-makes-for-a-good-ml-researcher-011257) , ML engineering skills. They do come up when you’re doing this sort of implementation. You need to test how good your system is. That means you need to build good evaluations, you need to not overfit to them, you need to be a little bit careful about that. So it’s not like those skills are irrelevant. I just think that there’s more of a focus on things like getting things done, software engineering, implementation now, relative to even just a year ago, but especially relative to two years ago.


**Rob Wiblin:** Are there any particular roles that you’re hiring for at the moment, or hiring for a lot in general? It’s felt to me like Anthropic is hiring hand over fist and at OpenAI there’s always a lot of roles. Is it a similar situation for you?


**Rohin Shah:** No, I think we are hiring quite a bit less than Anthropic and OpenAI at the moment. We do have a couple of[roles open right now](https://jobs.80000hours.org/organisations/deepmind?utm_source=80k_podcast) . Partly we have roles open in other teams that I think are also very relevant. The AGI safety and alignment team isn’t the only team that’s relevant to AGI safety at Google DeepMind.


For example, currently there is a hiring round open on the security team to hire engineers to work on AI control. And I think that’s a great position. Hugely impactful. As I’ve said before, that’s probably the argument that we’re going to make for why Gemini wouldn’t cause harm by a loss of control in the medium term. So I think that’s an extremely useful role. Very impactful. Not on the AGI safety and alignment team, but they would be working closely with us.


On my team,[we’re hiring at the moment for an engineer for frontier safety risk assessment](https://jobs.80000hours.org/?jobPk=18334&utm_source=80k_podcast) . So this is running our dangerous capability evaluations and writing the frontier safety report. I don’t particularly think that the frontier safety report needs to be that much more detailed, but if you do, you could come join us and then put in the time to make it better. There is a fair amount that: individual people on the team can have the flexibility to go and do stuff like that.


In practice, I expect by the time that this recording actually comes out that that role might not be there. But I think we do have these[roles coming out on an ongoing basis](https://jobs.80000hours.org/organisations/deepmind?utm_source=80k_podcast) .


And maybe I’ll send over a link to an[expression of interest form](https://gdm-agi-safety-jobs.beehiiv.com/) that people can fill out, so that we can email them when new roles open up. I think part of it is we have, like many others, been hit by the deluge of AI-assisted applications, so we are a little bit less likely in the future to make these open calls for hiring, because it really is just such a pain to go and do the resume screening for all of them.


**Rob Wiblin:** Yeah, I think folks are going to have to introduce a fee to fill out forms like that or to apply for jobs, which people also hate for other reasons. But I don’t really see the alternative now that it’s possible for AIs to submit completely indistinguishable applications that can be as fake as you like.


**Rohin Shah:** Yeah, it’s rough. Last year we included an LLM captcha, but for our recent one, we were trying to do it, and I think we probably could, but it would also trick a bunch of humans. Even our LLM captcha from last year tricked a bunch of humans. Not a bunch, like maybe 5% of them. At this point, I’m not sure that I can design one that wouldn’t have quite a lot of false positives.


**Rob Wiblin:** I guess “book a flight for me” might be more expensive than just having the fee. Is there any pitch you want to make for working at GDM in particular?


**Rohin Shah:** Basically my pitch is that company safety teams are probably the biggest force for what actually happens on a technical level to make AI systems safe, and that is a good reason to join them.


###


How Rohin maintains his positivity \[02:42:55\]


**Rob Wiblin:** All right, we should wrap up. I think for a final question, I’ll throw you a real hardball that came in from the audience: How do you maintain such a nice and positive spirit in these troubled times, Rohin?


**Rohin Shah:** I mean, some of the answer is a bit boring, not that generalisable. One, just by personality, I’m just fairly stable and my mood doesn’t change very much day to day. And then two, as has maybe become clear over the course of this episode, I don’t think the times are as troubled as everybody else thinks, relative to everyone else.


But one thing that I do quite a lot, and quite religiously, is focus on the things that I can control. This isn’t why I do it, but I think it is quite useful for maintaining a nice and positive spirit during “these troubled times,” let’s say. It definitely helps keep me focused on the areas where I do have agency, and I think it’s just pretty great to be focused on areas where you can actually make a difference.


**Rob Wiblin:** My guest today has been Rohin Shah. Thanks so much for coming on *The 80,000 Hours Podcast* , Rohin.


**Rohin Shah:** Thanks a lot, Rob. It was great to be here.
