---
schema_version: "1.0.0"
document_id: "b0258d2587500eb278f5bef9ddad7c4d8e66603e837d4d188eae683a7d114642"
company_key: "yc-vibeflow"
company: "VibeFlow"
source_id: "yc-vibeflow-rss-0e70e82ed51f"
canonical_url: "https://vibeflow.ai/blog/ai-doesnt-have-bad-taste-you-have-no-system/"
published_at: "2026-03-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:58.355770+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:dd2885cd476590e5c2691ada016db143e8513dbe677e16448557e30ab27a8741"
---

# AI Doesn't Have Bad Taste. You Have No System.

If you’ve ever prompted an AI to build a UI, you’re probably familiar with this kind of result:


Purple gradients that nobody asked for, sparkle icons, a card shadow that matches nothing. You open a page you haven’t touched in a week and it looks like a different product.


The fix for better design isn’t a better model, it’s a design system: a set of reusable components, patterns, and standards that keep your product visually consistent as it scales. Here’s why that matters.


## Why AI-generated UIs are inconsistent


AI makes 200–300 visual micro-decisions per component: padding, colors, border radius. It has no memory between sessions, so it guesses fresh every time. Without a design system to constrain those decisions, by session 10 your app looks like three different products.


It’s not picking bad values, it’s picking *plausible* ones. Your system uses` --space-200` for 8px, the AI writes` padding: 12px` . Looks fine. Just doesn’t match anything else.


Your instinct is to over-describe everything in the prompt. That makes it worse. You can’t cover every decision, and the AI fills in the gaps. Randomly without a design system, consistently with one.


## Easy fixes


### 1. Use a “golden screen”


One perfect screen becomes the reference for everything else.


Build one page that nails your design system. Colors, typography, spacing, components, all correct. Every time you generate a new page, reference it:


```text
Create a settings page that matches the style and design language
of this page.
```


AI is good at pattern matching. A visual reference beats abstract documentation. Teams go from “this looks AI-generated” to “wait, AI made this?” with nothing but a golden page. It also works as a quality gate: hold every new page against it. Does it feel like the same app? If not, your system has gaps.


### 2. Name your tokens well


Call a color` #3b82f6` and the AI has no idea what it’s for. It picks a slightly different blue every time. Call it` color-primary` and it knows exactly what to do. Name your spacing` button-padding` instead of` space-4` and it goes on buttons, not randomly on cards. The more specific the name, the less the AI guesses.


Vague names lead to guessing. Specific names lead to consistency.


### 3. The real fix: agentic design systems


[Brad Frost](https://bradfrost.com/blog/post/agentic-design-systems-in-2026/) , the person who literally wrote *Atomic Design* , just coined the term **Agentic Design Systems** : design systems built not for designers, but for AI agents.[Google Stitch](https://developers.googleblog.com/en/stitch-a-new-way-to-design-uis) is doing something similar, baking design tokens directly into Gemini 3 so generated screens stay on-brand.


Point to the system, don’t describe every pixel.


Most people paste their design system into a prompt. Sometimes the AI follows it, sometimes it ignores half of it. A document in the context window is a suggestion. A[skill](https://agenticskills.io/) is a constraint. A good design system skill has three things:


1. **Tokens as data** : your colors, spacing, typography in a format the agent can parse (not prose, actual values)
2. **Components as templates** : real code patterns for buttons, cards, inputs, not descriptions of what they should look like
3. **Guidelines as constraints** : rules like “always use 4px spacing grid” or “never use more than 3 font sizes per page”


Structure it this way and the AI stops interpreting.


## How we do it in VibeFlow UI


You don’t need months to build a design system.[VibeFlow UI](https://ui.vibeflow.ai/) is your design system manager. You can now create a system in minutes, import components from anywhere, and use it as a skill for any AI agent.


### 1. Create a design system


[VibeFlow UI](https://ui.vibeflow.ai/design-systems) lets you build your own design system from scratch. You set the color palette, typography scale, font pairings, spacing tokens, border radius, shadows, and component guidelines.


Manage your components, each with a live preview.


You can also import components from anywhere:


- **From Figma** : paste a component URL, get production React + Tailwind
- **From any website** : the[VibeFlow Clipper](https://chromewebstore.google.com/detail/vibeflow-clipper/cifpbjfodcpokoghghagledfleocmben) extension lets you select and clip any section of any live site
- **From screenshots** : upload a screenshot, AI converts it to a component
- **From conversation** : describe what you want and the Vibe Designer builds it following your system


Import from screenshots, websites, Figma, prompts, or code.


### 2. Generate UIs with your system applied


Prompt the page you want in[VibeFlow UI](https://ui.vibeflow.ai/) , apply your design system, and it builds the UI with your tokens already in place.


Pick “Design System” and your tokens are applied to every generated screen.


### 3. Use it from your editor via MCP


If you live in Claude Desktop or Cursor, you don’t need to open a browser. VibeFlow UI exposes a full MCP server. Your agent can create design systems, add components, and export skills directly from the terminal.


```text
Look at my app in src/components and create a design system
that matches what I already have.
```


The agent scans your existing code, calls VibeFlow UI’s MCP tools, and builds a design system that matches your current UI. Tokens, components, guidelines, all extracted from what you’ve already built.


### 4. Export as a skill for any agent


When you select a design system in the VibeFlow UI chat, it’s not pasted into context. It’s loaded as a **skill** that the agent calls when building components. Your tokens, patterns, and guidelines become constraints, not context window noise.


You can do the same from other agents. Download your design system as a SKILL.md file and use it in any project:


Export as a Claude Skill or JSON manifest for any tool.


```text
your-project/
├── .agents/
│   └── skills/
│       └── my-design-system/
│           └── SKILL.md    ← your exported design system
├── src/
└── ...
```


Create once. Use everywhere: Cursor, Claude Code, or any agent.


## Designers aren’t cooked. The handoff is.


Something AI changed about design: designers used to finish a mockup and then spend days going back and forth with developers over padding and font weights. That doesn’t happen anymore.


What hasn’t changed is taste. AI has seen millions of designs, but it doesn’t have a point of view, which is why every AI-generated app looks the same. That’s still on you. And now instead of spending your time policing pixels, you spend it defining the system that everything else follows.


Teams shipping the best products won’t be the ones with the best prompts. They’ll be the ones with the best systems.


[VibeFlow UI](https://ui.vibeflow.ai/) makes that easy.
