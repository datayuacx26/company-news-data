---
schema_version: "1.0.0"
document_id: "3c562d934d3b92dca7400dcb70e270e3c3363e6c532c36a8d95bf414f0460d60"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/the-journey-to-a-safer-frontend-why-we-removed-react-fc-095ff0b3e2e4"
published_at: "2026-02-09T17:01:01+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T22:21:12.159869+00:00"
content_hash: "sha256:bde62fe418ea50841dab71d3f5860b6d2f58f1bec6aebc42181de335b571b663"
---

# The Journey to a Safer Frontend: Why We Removed React.FC

# The Journey to a Safer Frontend: Why We Removed React.FC


[Arda Örkin](https://ardaorkin.medium.com/?source=post_page---byline--095ff0b3e2e4---------------------------------------)


5 min read


·


Feb 9, 2026


--


Press enter or click to view image in full size


Person in parachute over mountains during daytime


Sometimes, engineering improvements don’t begin with a grand roadmap. They start with a small moment of curiosity.


One afternoon, while working on a React component, I noticed something strange.


When I typed my component props directly, TypeScript flagged an unused prop. But when I wrapped the same component in React.FC, the warning vanished.


That didn’t feel right. TypeScript is supposed to *protect* us from mistakes — not silently ignore them.


I opened up a TypeScript playground and tried a few experiments. Sure enough, React.FC was changing how the compiler treated props. It was as if TypeScript was politely saying, “Don’t worry, everything’s fine,” even when it wasn’t.


That small hunch kicked off a journey that eventually touched thousands of files and changed how we write components across Gusto’s frontend.


## A Familiar Pattern–with Hidden Costs


In Gusto, many of our React components followed a familiar pattern:


```text
type ButtonProps = {    label: string    onClick?: () => void  }   const Button: React.FC<ButtonProps> = ({ label, onClick }) => (    <button onClick={onClick}>{label}</button>  )
```


This convention made sense years ago. React.FC looked clean and convenient, and it was widely recommended in early TypeScript + React guides


But as I started digging deeper, the cracks started to show.


React.FC does a few things behind the scenes that can quietly mislead developers:


- It **allows invalid default prop values** without error.
- It **hides unused props** , so TypeScript doesn’t warn you about dead code.
- It **breaks type inference for generics** , making reusable components harder to type.


Individually, these issues are subtle. Together, they add up to technical debt that TypeScript is supposed to help us avoid.


In practice, this meant we were quietly accumulating technical debt.


## The Moment It Clicked


The problem became obvious when I compared two versions of the same component — one typed with React.FC, one without:


```text
// With React.FC — no warning  const Example: React.FC<{ used: string; unused: string }> = ({ used }) => {    return <div>{used}</div>  }   // Without React.FC — correct warning  const Example = ({ used }: { used: string; unused: string }): React.ReactNode => {    return <div>{used}</div>  }
```


In the first case, TypeScript didn’t complain that the **unused** variable wasn’t being used. In the second, it did exactly what we’d expect.


That’s when it clicked: our type system was only as good as the types we chose to trust. And in this case, **React.FC** was making promises it couldn’t keep.


## From a Conversation to a Codebase-Wide Change


I shared the discovery in a developer chat. At first, it was a casual “today I learned” post — the kind that might get a few emoji reactions and then fade away. But this one sparked a real discussion. Other engineers chimed in, sharing similar frustrations and half-remembered TypeScript quirks.


It quickly became clear that we weren’t alone — and that we could actually do something about it.


This is a good moment to pause and talk about **Gusto’s engineering culture** , because it played a critical role in what happened next. At Gusto, turning an idea into action rarely feels like a solo effort. Collaboration isn’t just an abstract value — it’s supported by concrete structures that make it easy to surface ideas and get meaningful feedback.


We have guilds, Architecture Decision Records (ADRs), Tech Spec reviews, recurring office hours, and open Slack channels that create clear paths for discussion. If you want to raise a frontend-specific concern, you can bring it to the Web team’s office hours. If the change touches shared infrastructure or conventions, there are established channels and forums where those conversations naturally happen. And if an idea needs broader alignment, it can be formalized through an ADR and reviewed openly.


That environment made all the difference here. What started as a casual observation didn’t stall at “interesting, but risky.” Instead, it quickly found collaborators, skeptics, and champions — people who helped pressure-test the idea and shape it into something actionable.


Within days, we decided to see how far the problem went. The answer: very far. Our main frontend repository contained thousands of components typed with **React.FC** . Fixing this would mean touching nearly every package.


It was ambitious — maybe even a little reckless. But we had the tools, the support, and the shared understanding that the payoff — real, enforced type safety — would be worth it.


## Automating the Change


**Step 1: Rewrite Component Declarations**


## Get Arda Örkin’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


We wrote a transformation script that scanned the entire codebase for components defined with **React.FC** . For each one, it:


- Extracted the props type from **React.FC<Props>**
- Inlined that type into the component’s parameter
- Added a clear return type **(React.ReactNode)**


```text
// Before  const Card: React.FC<CardProps> = ({ title }) => <div>{title}</div>   // After  const Card = ({ title }: CardProps): React.ReactNode => <div>{title}</div>
```


The result was simpler, stricter, and easier for TypeScript to reason about.


**Step 2: Enforce Explicit Return Types**


Next, we ensured every exported component explicitly declared its return type. If a component returned JSX but didn’t specify a type, we added one automatically:


```text
// Before  export function Modal({ isOpen }: ModalProps) {    return isOpen ? <div>Open</div> : null  }   // After  export function Modal({ isOpen }: ModalProps): React.ReactNode {    return isOpen ? <div>Open</div> : null  }
```


This gave every component a clear contract — one TypeScript could enforce.


## The Cleanup Ripple Effect


Once the migration ran, something unexpected happened: the compiler began surfacing errors we didn’t know existed:


- **Unused props** that had quietly survived through multiple refactors
- **Default values with invalid types**
- **Inconsistent component definitions** across projects


By removing one abstraction, we exposed dozens of small issues that had gone unnoticed. What started as a typing cleanup turned into a meaningful improvement in overall code health.


## Keeping It That Way


After the migration, we wanted to make sure React.FC didn’t creep back in. So we added a linter rule to block it entirely:


```text
"@typescript-eslint/no-restricted-types": [    "error",    {      "types": {        "FC": {          "message": "Avoid React.FC. Use explicit prop types and return values instead."        }      }    }  ]
```


Now the rule enforces the convention automatically.


## What We Gained


By the end of the project:


- **Over 5,000 files updated**
- **Dozens of frontend packages standardized**
- **Dozens of hidden bugs fixed**
- **Our type system became more predictable and trustworthy**


But the real win wasn’t just technical. It was cultural.
This project reminded us that engineering excellence often comes from *paying attention* — from noticing when something feels off and following that thread until you understand it fully.


## Conclusion


Removing **React.FC** wasn’t about being pedantic — it was about precision.
By choosing explicit, predictable typing over hidden convenience, we gained stronger guarantees and a cleaner foundation for the future.


Sometimes, progress isn’t about adopting the newest tool — it’s about questioning the ones we’ve quietly accepted.


## Further Reading


- [Removed React.FC from Create React App templates (facebook/create-react-app#8177)](https://github.com/facebook/create-react-app/pull/8177)
- [React + TypeScript Cheatsheets: Function Components](https://github.com/typescript-cheatsheets/react#function-components)


Gusto is hiring! If you enjoy solving difficult problems and multiplying your impact, check out our careers page at[gusto.com/careers](https://gusto.com/careers) .
