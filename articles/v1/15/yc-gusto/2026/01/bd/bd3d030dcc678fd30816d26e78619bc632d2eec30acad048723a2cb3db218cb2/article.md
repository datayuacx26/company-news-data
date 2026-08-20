---
schema_version: "1.0.0"
document_id: "bd3d030dcc678fd30816d26e78619bc632d2eec30acad048723a2cb3db218cb2"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/how-to-write-an-oscar-worthy-llm-prompt-your-guide-to-the-prompt-chaining-framework-777d9d7084c6"
published_at: "2026-01-13T00:20:04+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:c5c1967e900cd98ccea0a29a657b4369f558e7caba4aa09eda47f0e9369731e5"
---

# How to Write an Oscar-Worthy LLM Prompt: Your Guide to the Prompt-Chaining Framework

# How to Write an Oscar-Worthy LLM Prompt: Your Guide to the Prompt-Chaining Framework


[Kim H. Nguyen](https://medium.com/@kim.nguyenkhn?source=post_page---byline--777d9d7084c6---------------------------------------)


5 min read


·


Jan 13, 2026


--


We’ve all been there. You type a question into your favorite AI chatbot, excited for a smart, helpful response, and… you get something generic, off-topic, or just plain *wrong* . It feels like a lottery, right? One day you get Shakespeare, and the next, you get a robot struggling to understand basic instructions.


*The problem isn’t the AI’s talent; it’s your direction.*


Press enter or click to view image in full size


Organize your contexts and improve your prompt engineering with prompt-chaining.


Think of an LLM, a Large Language Model, as an incredibly talented, but slightly unfocused, actor. If you walk onto the set and simply say, “Do something good,” you’ll get chaos. But if you step into the role of a **director** and give clear, structured instruction, your actor delivers an Oscar-worthy performance every time.


If there’s one takeaway you need from this post, it’s that you should experiment with structured instruction in an **RTRI** format. It’s a simple, four-step framework that gives your AI everything it needs to know to deliver consistently high-quality outputs. Stop crossing your fingers and start **directing** your results.


## The RTRI Breakdown: Your Director’s Playbook


**RTRI** stands for **Role** , **Task** , **Rules** , and **Input/Output** . Each part serves a specific purpose, turning a vague request into an airtight command. Here’s how each component works and how it maps to your new job as a movie director.


## 1. R: Role (Assigning the Character’s Motivations)


This is the most critical first step. You must tell the LLM **who it is** before it can speak.


**The Director’s Command:** This is like telling your actor, “You are playing a weary detective who hasn’t slept in 48 hours.”


**The Prompting Goal:** You are defining the **persona, expertise, and voice** of the AI. Do you need a history professor? A stand-up comedian? A financial analyst? Defining the **Role** sets the internal filter for all the knowledge the AI has, ensuring the tone and perspective are correct from the very first word.


## 2. T: Task (Explaining the Scene/Goal)


Once the actor knows who they are, they need to know what they are supposed to *do* .


**The Director’s Command:** This is like saying, “Your objective is to confront the suspect and get them to confess by appealing to their family loyalty.”


**The Prompting Goal:** You are clearly defining the **deliverable** . Are you asking for a summary? A list of ideas? A creative story? The **Task** must be a single, unambiguous goal that dictates the content and purpose of the final output. It’s the “why” behind the request.


## 3. R: Rules (Giving the Stage Directions/Tone)


A talented actor can deliver a line a thousand ways. The **Rules** tell them *how* to deliver it.


**The Director’s Command:** This is like giving the stage directions: “You must speak in short, clipped sentences, keep a low, menacing voice, and only use words a fourth-grader would understand.”


## Get Kim H. Nguyen’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**The Prompting Goal:** This section is the **guardrail** for the AI’s creativity. This is where you specify the **format, style, tone, and constraints** . Use bullet points to ensure clarity. Examples of rules include:


- “Keep the response under 500 words.”
- “Write in a friendly, optimistic tone.”
- “The output must be a bulleted list, not a paragraph.”
- “Do not invent statistics; use only the data provided.”


## 4. I: Input/Output (Providing the Script and calling “Action!”)


The actor is ready. They know who they are, what the scene is, and how to behave. Now, give them the material they need to work with!


**The Director’s Command:** “Here is the script for your line — memorize it for tomorrow’s scene.”


**The Prompting Goal:** This is the actual **data** the AI needs to process, and the final specification for how that processed data should look.


- **Input:** This includes the text you want summarized, the list you want categorized, or the article you want analyzed. You can even write “None required” if the AI is generating content from its general knowledge.
- **Output:** Re-confirm the desired format (e.g., “The final output must be a JSON object” or “Start the response with the word ‘Boom!’”).


## The Director’s Cut: How We Used Prompt-Chaining to Create This Post


The **RTRI** format is powerful on its own, but for complex projects like generating this very article, we used a strategy called[Prompt Chaining](http://promptingguide.ai/techniques/prompt_chaining) . Instead of asking the AI to do everything at once, you break the large task into smaller, highly structured **RTRI** requests, passing the output of one step to the next.


Instead of trying to write this whole article in one huge, messy prompt, we broke the task down into a 3-phase workflow:


**Phase 1: Prompt Generation** We didn’t immediately ask the AI to write the blog post. First, we directed the AI to act as a **Prompt Engineer** (the Role) to generate the perfect **RTRI** prompt structure (the Task), based on our basic idea (the Input). Our initial instruction was: *“Help me generate an LLM prompt using the RTRI format… I like the idea of myself playing the role of a movie director…”* This step ensured the foundation was solid before any content was written.


Press enter or click to view image in full size


Phase 1: Generate the prompt.


**Phase 2: Prompt Execution:** We took the highly structured, detailed **RTRI** prompt from Phase 1 and gave it back to the AI. This second, focused prompt contained all the **R, T, R, I** sections: the complete “script”, telling the AI exactly what character to play and what scene to shoot.


Press enter or click to view image in full size


Phase 2: Execute the prompt.


**Phase 3: Refine & Edit:** While the second prompt delivered a fantastic first draft, we reviewed and refined the content to polish the flow and add this final meta-section. This is also where I asked it to generate a slide deck outline for a brown bag presentation.


Press enter or click to view image in full size


Phase 3: Refine & edit.


## Conclusion — that’s a wrap


Imagine giving a full RTRI prompt to an LLM versus just saying, “Write a post about our new feature.” The difference in quality and relevance is night and day!


When you use the RTRI structure, you move from a hopeful guesser to a confident director. You are guiding the LLM not just with *what* to say, but *how* to say it, *who* should say it, and *what shape* the final product should take.


For simple tasks, a single RTRI prompt is all you need. For complex projects, remember the true power lies in Prompt Chaining — breaking down your task into a series of structured RTRI phases, just like we did to create this article.


Ready to direct your first masterpiece? Try using the **RTRI format and Prompt Chaining** today, and let the LLM deliver the consistent, high-quality results you deserve.
