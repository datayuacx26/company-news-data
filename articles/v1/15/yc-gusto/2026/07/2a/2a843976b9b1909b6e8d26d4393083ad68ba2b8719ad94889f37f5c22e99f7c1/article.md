---
schema_version: "1.0.0"
document_id: "2a843976b9b1909b6e8d26d4393083ad68ba2b8719ad94889f37f5c22e99f7c1"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/eval-driven-design-systems-8f781dc2dacb"
published_at: "2026-07-27T19:37:21+00:00"
first_seen_at: "2026-07-27T21:28:00.212983+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:cb2d55c1288020a6f0de5c971538da37d12749eb7036b12db1e6a14af5c67a6e"
---

# Eval-Driven Design Systems (Part 1)

# Eval-Driven Design Systems (Part 1)


[Jaywritescode](https://medium.com/@jaywritescode?source=post_page---byline--8f781dc2dacb---------------------------------------)


4 min read


·


1 day ago


--


Press enter or click to view image in full size


From plausible to production-ready: schema-grounded code replaces hallucinated props with verified, type-safe components.


```text
<Button size="lg" intent="confirm" icon="check" />
```


Three things are wrong. One line of code.` size=”lg”` .` intent=”confirm”` : that prop doesn’t exist anywhere in our schema.` icon=”check”` . None of these are obscure edge cases. All of them are documented in the design system we own and maintain.


This is what every AI coding assistant does when it meets a private design system. It writes the average of every button it has ever seen. The average is wrong.


For the past several months, we’ve been building toward something we call eval-driven design systems: making our design system legible to AI, then actually measuring whether AI uses it correctly. Here’s what we found, and what we’d recommend if you maintain a design system of your own.


## Why our design system was invisible to AI


AI-assisted coding is how engineers write UI now. That’s just the world.


The models behind those assistants were trained on the public internet: millions of Material-UI buttons, thousands of Chakra and Mantine and Tailwind buttons. They’ve seen zero` <Button>` components from our design system, because our design system is private. When an engineer asks for a button, the model writes the average. Which looks exactly like every public design system, and nothing like ours.


This pattern plays out quietly across engineering teams. Sometimes the assistant bypasses the design system entirely, dropping in raw HTML and Tailwind for a component the team already has. Sometimes it uses the right component with invented props, like the example above. Sometimes it gets the JSX right but misses a content guideline or an accessibility wrapper. Either way, the codebase drifts away from the design system one PR at a time. And no team can review every prop in every PR.


We don’t think of this as an AI problem. We think of it as a design system problem.


A design system exists to encode patterns so engineers don’t have to rediscover them. If the largest population of UI authors (AI assistants) can’t read what we’ve encoded, the system’s value collapses for that audience. AI-legibility is a quality bar now, the same way accessibility is. You can’t fix this by banning AI from your workflow. That ship has sailed. You fix it by making the system legible to the tools writing the code.


For us, that meant two things: schemas, and evals.


## Schemas as agent-readable contracts


TypeScript types tell a model that variant is` ‘primary’ | ‘secondary’ | ‘tertiary’ | ‘link’` . That’s useful. But types don’t tell the model which one to pick for a confirmation action. They don’t say a button label should start with a verb, or that buttons longer than three words are usually doing too much. They don’t carry the rule that tone=”monochrome” is only legal on certain variants. They don’t link to the Figma file.


## Get Jaywritescode’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


None of that fits into a type. So the model fills the gap with the average of its training data, which for our design system is wrong.


We treat schemas as something stronger than types. Each component in our design system has a Zod schema with a .meta() payload structured as` .meta({ examples, contentGuidelines, accessibilityGuidelines, importStatement, figmaUrl })` , a place to put everything a model needs to use the component correctly:


- ` *examples*` : real code snippets validated against the same schema. A model that copies an example is using a guaranteed-correct snippet. A model that invents one is guessing.
- ` *contentGuidelines*` : voice rules and character limits for visible text. This is why “Submit & continue” gets flagged on a primary button before a designer ever sees it.
- ` *accessibilityGuidelines*` (for components that need them): when ARIA props are required, what aria-label should contain, which patterns trip screen readers.
- ` *importStatement*` : the exact import path. Sounds trivial. It killed an entire category of hallucinations on its own. Models love inventing from` @gusto/design-system/button` when no such submodule exists.
- ` *figmaUrl*` : for downstream tools chained with design context.


For components like Button and Select, we use Zod unions with typed .extend() branches so that illegal prop combinations are unrepresentable, not just discouraged:


```text
const buttonShared = z.object({    size: z.enum(['lg', 'md', 'sm']).default('md'),    // …other shared props  });   export const buttonSchema = z.union([    buttonShared.extend({      variant: z.enum(['primary', 'ghost']).optional(),      tone: z.enum(['critical', 'neutral']).optional(),    }).strict(),    buttonShared.extend({      variant: z.enum(['secondary', 'outline']).default('secondary'),      tone: z.literal('critical').optional(),    }).strict(),  ]).meta({    importStatement: "import { Button } from '@acme/design-system';",    examples: [    { code: '<Button variant="secondary">Save changes</Button>' },    // …more validated examples    ],    // …also: contentGuidelines, figmaUrl, usageGuidelines  });
```


A model reading this schema knows two things at once: there’s no arm of the union that lets a neutral tone pair with an outline variant, and there’s a working example it can copy verbatim. No guessing required.


The schemas are published in a form any MCP-capable AI client can fetch. IDE assistants, agents, and code-review bots all read the same source. We pair that publication with a short prompt telling the model the right discipline: copy from .meta().examples. Don’t invent.


But a contract is useless if you don’t enforce it. Even with perfect schemas, a model’s output can drift if not held accountable. In our next post, we move from defining the rules to scoring the reality: we’ll cover how we use Braintrust to score every prop, measure our results, and turn these schemas into an eval-driven design system.


In our next post, we’ll cover how we score every prop, measure our results, and turn these schemas into an eval-driven design system.


[Jay Johnson](https://medium.com/@jaywritescode) *works on the Builder Enablement team at Gusto. The team maintains the internal design system powering Gusto’s web products, including the schemas and evals described in this post.*


*If you maintain a design system and want to make it AI-legible, we’re hiring on Builder Enablement and platform teams:*[gusto.com/about/careers](https://gusto.com/about/careers)
