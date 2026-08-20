---
schema_version: "1.0.0"
document_id: "1496974d5fe260242519a65b8d468420701a69b53e6d5bcdd134d88b2b318dc0"
company_key: "yc-fume"
company: "Fume"
source_id: "yc-fume-rss-5fd00bac91aa"
canonical_url: "https://www.fumedev.com/blog/era-of-abundance"
published_at: "2025-01-16T12:00:00+00:00"
first_seen_at: "2026-07-20T23:23:55.513253+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:c605d846d54ab553dd20feeef982d2fb0205f6d120de9e014aa959c3f6e6b22f"
---

# Era Of Abundance - The Big Misconception on AI Value

It seems like every once in a while there is a big flare up on X (fka Twitter) about 'AI replacing knowledge workers.' This is usually in response to a new model or product that has impressive benchmark scores in areas like coding, math or general reasoning. Some think the AI will soon replace all of us and "everything is going to look so much different in the next 6 months". Others say that LLMs (or any other for of autoregressive model) will eventually hit a "wall" before making any significant economic impact. As some who tried to build an AI SWE day and night for almost a year now, I think both of these camps have major flaws in their thought process and most likely outcome is a more pleasant third option: AI will remove the notion of scarcity in our business workflows.


Before, moving on with my prediction on how the abundance era will look like, I want to first explain why I think AI agents will not replace real human work except for some adhoc grunt work here and there (probably not more than 10% of any actually valuable occupation). I think nothing summarizes the idea that I’m going against as good as this billboard ad campaign from our YC W24 batch mates Artisan (10/10 campaign btw.)


The notion that we can replace human employees with AI agents is very enticing. After all, most knowledge works comes down to keyboard taps and mouse clicks. Why can’t I replace my software engineers with AI agents if they can write and run code? Why can’t I hire an AI account executive if it can send emails and join calls? OpenAI o3 scored 96% on AIME (a competitive math exam), it’s definitely 'smarter' than most of us.


From my experience of trying to get AI agents work in the real world context, I can spot 2 major flaws that these coveted benchmarks hide.


First is the lack of **context** . I know that this is very vague but there isn’t really a good way to explain this without specific examples. The problem itself is the inability to generalize problems across businesses. That’s why even after years of university education, humans still have to be onboarded for months during which they ingest the necessary context around their work to be actually useful. The context is REALLY hard to represent in words and humans are very good at consuming trillions of tokens worth of data and distill them into a mental model over time. Funny enough, my father who is a curtain wholesaler and has nothing to do with AI made me realize this. Seeing what I was working on, he told me his job line would never be replaced by AI. I was skeptical. Deciding on which curtains to buy from manufacturers, arranging transportation and storing them in a warehouse can all be done by emailing all day (this quite literally my father’s job) however the mental model that my father uses is a very distilled version of the 20+ years he lived and I don’t think even if we recorded all of his conversations, emails and documents into an infinite LLM context, we’d have an AI agent that performs nearly as good as a human. Humans are good at generalizing past experiences onto new open ended problems, LLMs are not. Benchmarks like ArcAGI was marketed as evaluating on the spot learning, I would strongly disagree but that’s a whole different conversation.


What deceives people is that the benchmarks create a bubble for AI to solve a very contained problem. Real life is order of magnitude more complex and requires prior context to solve real economically valuable problems.


Second, is AIs inability to follow exact instructions. I’m not here to argue these models are just stochastic next token predictors. However, you sure are going to get a stunning image from Midjourney or a good looking website from Claude with a sentence long prompt but have you ever tried getting the EXACT result you want to get out of them? Here is an example (I’m going to use an image-gen model for ease of explanation):


I used the Dall-E model built into ChatGPT with the prompt: "draw a red leather book on top of a turtle. the turtle is standing on top of cowboy hat and the hat has a rocket engine under it. the background is the blue mosque in istanbul, turkiye." The result is very impressive yet not exactly correct. The model of capable of generating each piece of my prompt separately but still fails miserably to follow exact instructions. If I’m using this model casually, yes I’ll be happy with this result and post on X: "AI is coming for all digital artists." But the reality is if I needed this image in a business context and hired a digital artist from Fiverr, this image would never pass.


Same happened for Fume (the AI SWE I spent a year building) all the time. For example, a customer was using it to generate unit tests - the classic software task that is said to be automated by AI. But since they were going to actually use the code generated, they asked it put all mock data into a separate directory for easier maintenance. Simple enough right? Well, Fume would start out while obeying this simple instruction. A few test case in, it would most certainly hit an error and had to debug during which it would straight out ignore the exact instruction we provided and go back to writing bad code.


I believe this inability to follow exact instructions is nothing related to intelligence but more so with this (now more unpopular than ever) slide on autoregressive LLMs from Yann LeCun:


When you provide an exact instruction, the tree of correct answers is extremely narrow. Within a long agent loop, every iteration of LLM generation poses a possibility of diverging from this already narrow tree of correct answers. Therefore LLM agents are miserably fail at following exact instructions in complex tasks.


Now that I explained why I think the "AI replacing humans" hysteria is not likely to become real anytime soon, I can move on with how I think AI will impact businesses in the forseeable future. If you look closely there are many business workflows that are designed with "intelligence scarcity mindset." For knowledge workers, cost equals to capability (most likely represented by their hourly rate) multiplied by time. If the expected impact of a task is lower than their cost, they don’t do it. Let’s take software development for example (I assume most of the audience to read this far to be at least somewhat technical), when an engineer publishes a PR a more senior engineer reviews it. But the reviewer’s cost per time is high, therefore they don’t actually test the changes made. They just make sure the code written complies with the general architecture and standards because that is the only work worth their time in the long run. Some amount of testing is expected of the junior engineer who submits the PR but all the edge cases are only tested in a 'staging' environment where the changes made in a certain timeframe is aggregated and tested by various methods that are far less costly than any of the engineers' time.


The testing is done on aggregate not because it’s the best practice. Quite contrary, testers miss a lot of edge case when testing many changes at once; however, this workflow yields a good enough result where the company is allocating the scarce resources to their best ability. Now, image what would the company do if the intelligence was abundant. They’d probably want all of the edge cases to be tested for every PR. Detecting edge cases on individual changes is far more reliable than looking for mistakes on change aggregates. It’s also much easier to fix as the search area for the mistake is smaller. So you could easily imagine a software team that tests every PR throughly as if it’s immediately going into production, if intelligence was free.


This is the exact kind of change I imagine AI to bring: transform workflows from scarcity to abundance mindset. We optimized our workflows to minimize cost for a good enough outcome and AI can unlock so much value we pass on because it’s simply not worth our time. If AI agents were able to automate meaningful software engineer work end-to-end, I’d be one of the first people to embrace it but the reality is it cannot. And the bottleneck is not intelligence. Therefore, I don’t partake in the "AI replacing humans" fear every time the models score higher on arbitrary math test but this also does not mean I’m not excited for the change AI will bring to the way we create economic output. There are trillions of dollars worth of value to unlock thanks to the abundance of intelligence AI will provide.


I think the best way to demonstrate this is unfolding hidden steps in our workflows which we previously passed on. A sales team might unfold extra value with a tool like Artisan but I don’t see any of them replacing a knowledge worker with it.


**An important note here: Technology is moving faster than ever and therefore, it’s really hard to extrapolate on what’s going to happen in a longer time frame than 10 years. I don’t think I, Sam Altman, or anyone else can make an educated guess on where this technology will be in 10 years and how it’s going to affect us. So, these predictions are for the next few years time frame. For what it’s worth, we might either be sipping martinis on Mars while AI overlords take care of everything else for us or keep living our mostly same lives in the next 10-50 years** **While proof-reading, I realized I use AI and LLMs interchangeably. I thought of fixing it first but then decided to leave as is since the public discourse is mostly around these AR-LLMs**
