---
schema_version: "1.0.0"
document_id: "9ccdbbb52ff66326a9683bc77703cdd30cd8a999e4bdd0c323b75e06cb905108"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/frontend-interview-prep-guide-2026/"
published_at: "2026-07-29T08:34:09+00:00"
first_seen_at: "2026-07-29T21:44:44.427266+00:00"
fetched_at: "2026-07-29T21:44:45.761634+00:00"
content_hash: "sha256:e630edce91114ac97d5591cbb818b4f92f13d247fdaacf3a9ada0bb79c2d12ea"
---

# Frontend Interview Prep Guide [2026]

A frontend interview in 2026 runs five stages: a recruiter screen, a technical screen, a coding round (take-home or live), a frontend system design round, and a behavioral round. Each stage tests a different signal, and the questions at each one are predictable enough to prepare for. The new variable is AI. Whether you may use an assistant in the coding round is now a per-company policy rather than an industry rule, and guessing wrong is expensive.


Below: the stages, a prep timeline, and the questions that come up, with answers you can say out loud. Still deciding *where* to practice rather than *what* ? Scrimba's roundup of[coding interview prep platforms](https://scrimba.com/articles/best-coding-interview-prep-platforms-2026/) is the companion piece.


## How Are Frontend Interviews Structured in 2026?


A frontend interview is a five-stage pipeline: recruiter screen, technical screen, coding round, frontend system design, and behavioral, each filtering a different signal.


Stage What it tests How to prepare


Recruiter screen (20 to 30 min) Role fit, salary range, timeline, and whether you can describe your work clearly A 90-second project story, a compensation number you can say without flinching, three questions about the team


Technical screen (45 to 60 min) JavaScript and framework depth in conversation, sometimes light coding Rehearse the fundamentals below out loud, not silently on a page


Coding round (60 to 90 min, or a take-home) Whether you can build a working, accessible UI while someone watches Small components on a timer: tabs, a modal, an autocomplete, a paginated table


Frontend system design (45 to 60 min) Architecture judgment: component boundaries, state ownership, data flow, trade-offs The four-part answer shape below, on two or three classic prompts


Behavioral (45 min) Collaboration, conflict, ownership, and how you talk about work that went badly Three stories of two minutes each, one of them a genuine failure


The coding round is where formats diverge most, and geography matters more than people expect. In a January 2026 survey of 400 engineering leaders across the US, India, and China,[Karat](https://karat.com/engineering-interview-trends-2026/?ref=scrimba.com) found take-home projects used by 45% of US organizations but only 20% of Chinese ones.


So **ask the recruiter for the format in writing** . A live 45-minute component build and a three-day take-home reward completely different practice.


## What Has AI Changed About Frontend Interviews?


AI changed the coding round. A minority of companies now hand you an assistant and grade how you use it, while most still prohibit one entirely.


The clearest example is Meta. In July 2025,[404 Media](https://www.404media.co/meta-is-going-to-let-job-candidates-use-ai-during-coding-tests/?ref=scrimba.com) reported on internal Meta communications describing a new interview type in which candidates have access to an AI assistant. The internal message gave two reasons:


> "This is more representative of the developer environment that our future employees will work in, and also makes LLM-based cheating less effective."


Meta began rolling the format out in October 2025. According to[Hello Interview](https://www.hellointerview.com/blog/meta-ai-enabled-coding?ref=scrimba.com) , it runs roughly 60 minutes, replaces one of the two onsite coding rounds, and scores candidates on problem solving, code quality, verification, and communication. You are no longer graded on producing code. You are graded on *judging* it.


Google is running a similar pilot. Per[reporting on an internal document](https://www.tryexponent.com/blog/google-ai-coding-interview?ref=scrimba.com) , it is testing a "code comprehension" round for junior and mid-level roles on selected US teams, where candidates read, debug, and optimize an existing codebase with Gemini available. Interviewers assess prompt engineering, output validation, and debugging.


Now the counterweight, because headlines overstate this. In that same Karat survey, **62% of organizations still prohibit AI use in technical interviews** , and 71% of engineering leaders said AI is making technical skills harder to assess. Permission splits by region too: 38% of US organizations allow it versus 68% in China.


Anthropic publishes the most granular policy. Its[candidate AI guidance](https://www.anthropic.com/candidate-ai-guidance?ref=scrimba.com) allows Claude for research and for refining your own first draft, but not for take-homes or live interviews unless told otherwise. Expect that shape: a rule per stage, not one blanket policy.


So ask. Before any technical round, send the recruiter three questions:


1. May I use an AI assistant in this round, and if so, which one?
2. Is it allowed on the take-home, or only in the live session?
3. Will I be evaluated on how I use it?


## A Realistic Frontend Interview Prep Timeline


Four weeks is enough if you already build things regularly. If React fundamentals still feel shaky, you are looking at months, and Scrimba's[frontend developer roadmap](https://scrimba.com/articles/how-to-become-a-frontend-developer-in-2026-complete-roadmap/) is the better starting point.


1. **Week 1, fundamentals out loud.** Work through the JavaScript questions below and explain each answer to a rubber duck, a friend, or a recording. Reading an answer and delivering one are different skills.
2. **Week 2, build on a timer.** Forty-five minutes, no libraries: tabs, a modal with focus trapping, a debounced autocomplete, a sortable table.
3. **Week 3, React and system design.** Half on React internals, half on two design prompts end to end.
4. **Week 4, rehearsal.** Two mock interviews with a human, then rewrite your behavioral stories based on what you fumbled.


With one week rather than four, keep week 1 and week 4 and drop the rest. *Fluency in the fundamentals plus one honest rehearsal beats broad, silent revision.*


## JavaScript Interview Questions and Answers


JavaScript interview questions cluster around five topics: closures, the` this` keyword, the event loop, promises, and prototypes. Most other questions are variations on these.


**"What is a closure, and where have you used one?"** A closure is a function that keeps access to variables from the scope where it was defined, even after that scope has returned. The follow-up is the loop example:


```text
for (var i = 0; i < 3; i++) {
setTimeout(() => console.log(i), 0);
}
// 3, 3, 3 because var creates one shared binding


for (let i = 0; i < 3; i++) {
setTimeout(() => console.log(i), 0);
}
// 0, 1, 2 because let creates a fresh binding per iteration


```


**"What does` this` refer to?"** It depends on how the function is called, not where it is written. Method calls bind it to the object before the dot, standalone calls bind it to` undefined` in strict mode,` call` ,` apply` , and` bind` set it explicitly, and arrow functions inherit it from the enclosing scope.


```text
const counter = {
count: 0,
start() {
setTimeout(function () { this.count++; }, 100); // this is not counter
setTimeout(() => { this.count++; }, 100);       // arrow keeps counter
}
};


```


**"Explain the event loop."** JavaScript runs one call stack. When it empties, the loop drains the entire microtask queue (promise callbacks,` queueMicrotask` ) before taking a single macrotask (timers, I/O). That ordering is the whole answer; the[MDN event loop reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop?ref=scrimba.com) repays a proper read.


```text
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// 1, 4, 3, 2


```


**"When would you use` Promise.allSettled` over` Promise.all` ?"**` Promise.all` rejects the moment any input rejects, which is right when you need every result.` allSettled` always resolves with a status per promise, which is right when one failing dashboard widget should not blank the page.


**"How does prototypal inheritance work?"** Every object holds an internal link to a prototype, and property lookups walk that chain until they hit a match or reach` null` . ES6` class` syntax is sugar over exactly this, and saying so scores better than reciting the syntax.


Scrimba's[JavaScript Interview Challenges](https://scrimba.com/javascript-interview-challenges-c02c?ref=scrimba.com) (Pro, 2.3 hours, Treasure Porth) is 30 interview-style challenges with worked solutions, and it trains the skill of talking through one aloud.[Frontend Interview Tips](https://scrimba.com/frontend-interview-tips-c01u?ref=scrimba.com) (Pro, 104 minutes, Dylan C. Israel) covers the conceptual side: falsy values,` ==` versus` ===` ,` undefined` versus` null` , spread and rest, plus CSS and Git.


## React Interview Questions and Answers


React interview questions test whether you understand the rules the library depends on: hook ordering, reconciliation keys, state ownership, and when memoization still earns its place.


- **"Why can't hooks be called conditionally?"** React identifies hooks by call order, not by name, so a conditional call shifts every subsequent hook's identity. The[rules of hooks](https://react.dev/reference/rules/rules-of-hooks?ref=scrimba.com) exist because state is stored positionally.
- **"State or props?"** Props flow down and are read-only; state is owned locally and triggers a re-render. The real question underneath is where state should live, and "as low as possible, lifted only when two siblings need it" is the answer they want.
- **"What is reconciliation, and why do keys matter?"** React diffs the new element tree against the previous one and reuses nodes whose type and key match. An index key breaks that the moment the list reorders.


```text
// Reordering reuses the wrong DOM node, and stray input state follows
{todos.map((todo, i) => <Todo key={i} {...todo} />)}


// Stable identity survives reordering, insertion, and deletion
{todos.map(todo => <Todo key={todo.id} {...todo} />)}


```


- **"Do you still need` useMemo` ?"** This separates candidates who track React from candidates who learned it once. The React Compiler reached **1.0 in October 2025** and memoizes automatically at build time, making most manual` useMemo` ,` useCallback` , and` memo` calls unnecessary. They survive as escape hatches. "It depends on whether the compiler is enabled" is a strong 2026 answer.
- **"When is` useEffect` the wrong tool?"** Whenever the value can be derived during render, or the work belongs in an event handler. Effects synchronize with things outside React; they do not compute state from state.


[React Interview Questions](https://scrimba.com/react-interview-questions-c01t?ref=scrimba.com) (Pro, 40 minutes, Cassidy Williams) covers the Q&A side directly: the virtual DOM, JSX, effects, refs, and context. It is a rehearsal, not a course. If the fundamentals are the gap, Scrimba's[guide to learning React](https://scrimba.com/articles/how-to-learn-react/) sets the order.


## CSS, Accessibility, and Browser Questions


These rounds are short, and they are where junior candidates lose ground: the answers are either fluent or absent.


**CSS.** Expect specificity arithmetic, the box model and` box-sizing` , when Grid beats Flexbox (two dimensions versus one), what creates a stacking context, and three ways to center a div.


**Accessibility.** Semantic HTML first, ARIA only when no native element does the job. The[ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/?ref=scrimba.com) puts it bluntly:


> "No ARIA is better than Bad ARIA."


Know that[WCAG 2.2](https://www.w3.org/TR/WCAG22/?ref=scrimba.com) requires a 4.5:1 contrast ratio for normal text and 3:1 for large text, that every interactive control needs a visible focus style, and that a modal must trap focus and return it on close. Scrimba's[Learn Accessible Web Design](https://scrimba.com/learn-accessible-web-design-c031?ref=scrimba.com) (Pro, 96 minutes, Fredrik Ridderfalk) covers contrast, the accessibility tree, form labeling, ARIA roles, and skip links.


**Browser and networking.** "What happens when you type a URL" is a breadth check: DNS, TCP and TLS, request, HTML parsing, the critical rendering path. Also expect CORS preflight, cache headers, and Core Web Vitals. On that last one, *Interaction to Next Paint replaced First Input Delay* in March 2024 ([web.dev](https://web.dev/articles/vitals?ref=scrimba.com) ). Candidates still naming FID date themselves instantly.


If a separate algorithms round is in the loop, that is a different discipline, and Scrimba's comparison of[data structures and algorithms courses](https://scrimba.com/articles/best-data-structures-and-algorithms-courses-2026/) is the place to start.


## Frontend System Design Questions


Frontend system design asks you to architect a feature rather than a backend: component boundaries, state ownership, data fetching, and the trade-offs you consciously accept.


Use the same four-part shape every time:


1. **Clarify requirements.** Scale, devices, offline behavior, accessibility expectations. Two minutes here saves ten later.
2. **Sketch components and data.** What renders what, what data each piece needs, and where that state lives.
3. **Specify the API contract and state strategy.** Pagination style, cache invalidation, optimistic updates, error and empty states.
4. **Name the trade-offs.** Performance budget, bundle size, accessibility, and what you chose not to build.


Two prompts worth rehearsing. *Design an infinite-scroll feed:* cursor pagination over offset, an IntersectionObserver sentinel, list virtualization past a few hundred rows, and a keyboard-reachable "load more" fallback. *Design an autocomplete:* debounce input, cancel in-flight requests with` AbortController` , cache by query, and wire it as a combobox with proper` aria-activedescendant` handling.


One honest caveat: Scrimba's interview courses stop short of frontend system design. The closest thing is the architecture practice from building larger projects in the[Frontend Developer Path](https://scrimba.com/frontend-path-c0j?ref=scrimba.com) , whose final module is a 5.1-hour "Getting hired" section.


## Behavioral Questions


Three prompts cover most behavioral rounds: a project you are proud of, a technical disagreement, and something that went wrong.


Use STAR (situation, task, action, result) as scaffolding, then delete the scaffolding so it does not sound recited. Two minutes per story. For the failure story, name the specific thing you changed afterwards; interviewers listen for the correction, not the confession.


Then reverse it. Ask what code review culture is like, how much of the roadmap is bugs versus features, and what happened to the last person in this role. The last one is unusually informative. Earlier in the process, Scrimba's guide to[landing your first developer job](https://scrimba.com/articles/how-to-land-your-first-developer-job/) covers what comes before.


## Where to Practice


Scrimba's interview material is Pro and splits by need:[JavaScript Interview Challenges](https://scrimba.com/javascript-interview-challenges-c02c?ref=scrimba.com) for coding practice,[React Interview Questions](https://scrimba.com/react-interview-questions-c01t?ref=scrimba.com) for React Q&A, and[Frontend Interview Tips](https://scrimba.com/frontend-interview-tips-c01u?ref=scrimba.com) for common questions plus resume, GitHub, and LinkedIn polish. All three are interactive scrims, so you edit the instructor's code in place rather than watching it.


Pro is **$24.50/month on the annual plan** ($294/year), with student pricing, location-based discounts, and periodic promotions. Free courses include completion certificates, worth knowing before you pay for anything.


## Frequently Asked Questions


### What questions are asked in a frontend interview?


Frontend interviews concentrate on JavaScript fundamentals (closures, this, the event loop, promises, prototypes), framework internals such as React hook rules and reconciliation, CSS layout and specificity, accessibility basics, browser and networking behavior, and one frontend system design prompt. Behavioral questions run alongside.


### Can you use AI in a coding interview in 2026?


It depends on the company. Meta and Google are piloting rounds where an AI assistant is provided and your use of it is graded, but a January 2026 Karat survey found 62% of organizations still prohibit AI in technical interviews. Confirm the policy with your recruiter.


### How long should I prepare for a frontend interview?


Four weeks is realistic if you already build projects regularly: one week on fundamentals, one on timed component builds, one on framework internals and system design, and one on mock interviews. If fundamentals are still shaky, plan in months.


### Do frontend interviews still ask LeetCode questions?


Some do, particularly at larger companies with a shared engineering interview loop. Most frontend-specific rounds favor building a working UI component under time pressure. Prepare both, but weight practice toward component building unless the recruiter says the round is algorithmic.


### What is a frontend system design interview?


A 45 to 60 minute round where you architect a feature such as an infinite-scroll feed or an autocomplete. You clarify requirements, sketch component boundaries and state ownership, define the data-fetching contract, and defend your trade-offs. Very little code gets written.


## Key Takeaways


- Frontend interviews in 2026 run five stages, each testing a different signal, so preparing for all five beats grinding one.
- Whether AI is allowed is a per-company and per-stage policy, so confirm it in writing before every technical round.
- Meta's AI-enabled round scores problem solving, code quality, verification, and communication: the tested skill moved from writing code to judging it.
- A January 2026 Karat survey of 400 engineering leaders found 62% of organizations still prohibit AI in technical interviews. Assume a ban until told otherwise.
- JavaScript questions cluster around closures,` this` , the event loop, promises, and prototypes; React questions around hook rules, keys, state ownership, and whether` useMemo` still earns its place.
- Frontend system design has a repeatable four-part shape: requirements, components and data, API and state, trade-offs. Scrimba's three interview courses are Pro at $24.50/month annually, and none of them covers it.


## Sources


- 404 Media. "Meta Is Going to Let Job Candidates Use AI During Coding Tests." 2025.[https://www.404media.co/meta-is-going-to-let-job-candidates-use-ai-during-coding-tests/](https://www.404media.co/meta-is-going-to-let-job-candidates-use-ai-during-coding-tests/?ref=scrimba.com)
- Hello Interview. "Meta's AI-Enabled Coding Interview." 2026.[https://www.hellointerview.com/blog/meta-ai-enabled-coding](https://www.hellointerview.com/blog/meta-ai-enabled-coding?ref=scrimba.com)
- Karat. "Engineering Interview Trends 2026." January 2026.[https://karat.com/engineering-interview-trends-2026/](https://karat.com/engineering-interview-trends-2026/?ref=scrimba.com)
- Anthropic. "Guidance on Candidates' AI Usage." 2025.[https://www.anthropic.com/candidate-ai-guidance](https://www.anthropic.com/candidate-ai-guidance?ref=scrimba.com)
- Exponent. "Google's AI-Assisted Coding Interview." 2026.[https://www.tryexponent.com/blog/google-ai-coding-interview](https://www.tryexponent.com/blog/google-ai-coding-interview?ref=scrimba.com)
- MDN Web Docs. "The event loop." Accessed July 2026.[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop?ref=scrimba.com)
- React. "Rules of Hooks." Accessed July 2026.[https://react.dev/reference/rules/rules-of-hooks](https://react.dev/reference/rules/rules-of-hooks?ref=scrimba.com)
- W3C. "ARIA Authoring Practices Guide: Read Me First." Accessed July 2026.[https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/](https://www.w3.org/WAI/ARIA/apg/practices/read-me-first/?ref=scrimba.com)
- W3C. "Web Content Accessibility Guidelines (WCAG) 2.2." 2023.[https://www.w3.org/TR/WCAG22/](https://www.w3.org/TR/WCAG22/?ref=scrimba.com)
- web.dev. "Web Vitals." Accessed July 2026.[https://web.dev/articles/vitals](https://web.dev/articles/vitals?ref=scrimba.com)
