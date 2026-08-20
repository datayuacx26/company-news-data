---
schema_version: "1.0.0"
document_id: "fdb96f411a3ee0d3000dd28e748796689637a11b318d3dbda42957ddf357160a"
company_key: "yc-motion"
company: "Motion"
source_id: "yc-motion-rss-f628c3046538"
canonical_url: "https://engineering.usemotion.com/moving-off-of-typescript-e7bb1f3ad091"
published_at: "2025-09-17T14:05:42+00:00"
first_seen_at: "2026-07-20T23:21:05.244128+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:01eed244e2787d1117d29f6068b489def0847feccf07711eabc9cd9e5aaa3d82"
---

# Moving off of TypeScript

# Moving off of TypeScript


[Chander Ramesh](https://c5r.medium.com/?source=post_page---byline--e7bb1f3ad091---------------------------------------)


7 min read


·


Sep 17, 2025


--


## We Love You, TypeScript


For nearly five years now, Motion has operated in a large TypeScript monorepo. At its peak, it was roughly ~2.5 million lines of code after excluding comments, node_modules, etc. To manage this, we used Vercel’s rather excellent[Turborepo](https://turborepo.com/) build system.


This is not a blog post hating on TypeScript — quite the opposite! Motion would likely not ** even have survived until today without it. Motion has pivoted[over twenty times](https://www.linkedin.com/posts/harryqi_when-chander-joined-motion-35-years-ago-activity-7371578882313027584-xOVx?utm_source=li_share&utm_content=feedcontent&utm_medium=g_dt_web&utm_campaign=copy) . TypeScript let us quickly validate ideas, and most importantly, let all our developers operate in a truly full stack manner. We could quickly and easily share code between the frontend and the backend, we unified our development setup greatly, and of course, we took advantage of the extensive and lively ecosystem.


Our frontend is written in React. Mobile apps? React Native. Desktop app? Electron. Infrastructure as code?[Believe it or not, TypeScript](https://engineering.usemotion.com/replacing-clickops-with-pulumi-d21f3e80b851) . We were (and still are!) *huge* TypeScript fans.


But over time, things started to change.


The dream of sharing code *everywhere* never really fully materialized. From the beginning, there were problems sharing code between React Native and the rest of the ecosystem. We had a different version of React and Tailwind from the rest of the web app, so many core libraries were simply never shared. When we *did* manage to share code, developers would often forget mobile entirely when making changes to shared libraries, resulting in a frequent “who broke the mobile app” hide and seek game. Eventually,[we had to abandon React Native for certain screens where performance was critical](https://engineering.usemotion.com/we-thought-react-native-was-the-answer-until-our-app-hit-190-cpu-f0e849073334) .


Over time, we dealt with all the typical pain points of a growing TypeScript codebase. The language server frequently crashed, CI builds took *forever* (our somewhat optimized pipeline regularly still takes ~20+ minutes). A large portion of this will eventually be[solved by the move to TS-Go](https://github.com/microsoft/typescript-go) — but this is the exact problem with TypeScript. There’s a *lot* going on, and I have no doubt that *eventually* things will be great. But in the meantime, as a rapidly growing and scaling startup, we are not in control of our own destiny.


We’re waiting for the (incredibly talented) TypeScript compiler team to make fundamental changes. We’re waiting for the (really passionate) biome team to support certain features so we can[re-enable basic lint rules without nuking our CI times](https://eslint.org/docs/latest/rules/no-return-await) .


We had to[wait for Zodv4](https://github.com/colinhacks/zod/issues/5064) to come out just so that our monorepo would even compile because with v3 we were running into[max recursion depth issues on the TypeScript compiler](https://zod.dev/v4?id=100x-reduction-in-tsc-instantiations) ! For that matter, why do we need Zod at all? This ceremony is only required because TypeScript doesn’t have runtime types in the first place.


The story doesn’t get any better in the ORM side. As we found out the hard way, Prisma *deletes the entire table* if your[where statement evaluates to undefined](https://github.com/prisma/prisma/issues/20169#issuecomment-2295402176) . (We ran into many, *many* other bugs along the way.)[Drizzle](https://orm.drizzle.team/) seems better, and we sponsored them for many months, but it’s not even 1.0 yet. Who’s to say it won’t also have bugs for the first couple years after launch?


The ecosystem is large, vibrant, and passionate. It’s full of people who genuinely want the best for the web and are trying their best. But at the end of the day, as a CTO, I cannot rest the fate of my company in someone else’s hands. The gnawing feeling of helplessness every time we ran into one of these issues was eating away at me — especially when these problems are for the most part solved in many mature ecosystems!


It’s no accident that Java and .NET rule the enterprise world. Runtime types are a given. Relatively fast compile times are mandatory for codebases that are millions of lines long. They both have an ORM that has matured over *decades* , which means for basic CRUD operations developers simply don’t need to think at all: there is only *one* real choice. With multi-threading back on the table, we don’t need to spin up complex workflow orchestration solutions to keep a single-threaded event loop free. And ultimately, this means developers can focus on solving *novel* problems instead of finding increasingly clever solutions to decades-old use cases.


Is it boring? Yes. But solving CRUD operations at scale shouldn’t be exciting. It should be boring and safe. It’s 2025 —we as an industry should know how to scale CRUD apps to hundreds of thousands of queries per second safely and reliably without bugs, without performance bottlenecks, and without “excitement”.


The nail in the coffin came when we developed[our agentic workflow system](https://www.usemotion.com/ai-employees/ai-executive-assistant) : we needed to have a code block that could execute arbitrary JavaScript. And, for obvious security reasons, that couldn’t be executed in a backend container that also ran JavaScript. This coincided with a major (yet to be publicized) new initiative in the company.


## But We Can’t Stay


So if we needed to introduce another language for arbitrary code execution anyway… Could we just start the new initiative in the new language? But that then begged the question of *which* language.


## Get Chander Ramesh’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Two years ago in a now infamous offsite in Chicago, we had a similar fork in the road moment. TypeScript does not and will never have a truly sound type system, and even back then we were having TypeScript fatigue. And while a majority of backend developers wanted to move to a new language, the ensuing flame war between the language choices ultimately led to us remaining on TypeScript. This time, I had learned my lesson: no polls! (Just kidding — in truth, I did discuss it with six or so engineers, but I didn’t want to have a truly open forum.)


The criteria were that I wanted a garbage collected, statically typed language that was performant and maximized productivity. Given that, the only two choices I seriously considered were C Sharp and Java. They both have large, proven ecosystems with extremely mature software.


In the end, I decided on C Sharp, even though I had never used it professionally because I genuinely believe Motion will be far more productive:


1. Entity Framework


As mentioned above, many of the frustratingly simple problems we dealt with was the lack of a very robust and performant ORM. Most B2B software centers around basic CRUD operations, and we at Motion are no exception. After[playing around with it myself](https://github.com/c10r/freetool) , I was absolutely blown away by Entity Framework. At Motion we long struggled with dealing with soft deletes — especially with nested models. But with EF Core’s global query filters,[it’s literally a single line of code.](https://learn.microsoft.com/en-us/ef/core/querying/filters?tabs=ef10#basic-example---soft-deletion) There’s many more examples of really advanced use cases ([table splitting](https://learn.microsoft.com/en-us/ef/core/modeling/table-splitting#table-splitting) ,[concurrency tokens](https://learn.microsoft.com/en-us/ef/core/modeling/table-splitting#concurrency-tokens) , and[navigation properties](https://learn.microsoft.com/en-us/previous-versions/dotnet/netframework-4.0/bb738520(v=vs.100)?redirectedfrom=MSDN) ). But the thing that impressed me the most was how natural the[change tracking](https://learn.microsoft.com/en-us/ef/core/change-tracking/) was. Unlike in Prisma or Drizzle where we had to plumb the transactions through to every method in the repository layer, EFCore operated this way *by default* . Operations naturally just mutated the context, and saving the context would transactionally commit all changes. At any time, you can simply create a new context (or save the old one).


2. Similarity to TypeScript


These two languages were created by the same person, so it’s only natural that they look and feel very similar. As[many people](https://medium.com/@namratabagh20/how-i-realized-c-and-typescript-are-more-alike-than-i-thought-ec54759e4f1d) are starting to realize (heck,[Microsoft even advertises this](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/tips-for-javascript-developers) ), the language constructs are exceedingly similar. Both have async/await, similar` =>` anonymous function syntax, nullable types, and a lot more. Perhaps the best website comparing and contrasting the two languages[is this one](https://typescript-is-like-csharp.chrlschn.dev/) by our very own Senior Staff engineer


[Charles Chen](https://medium.com/u/61db58b8ec88?source=post_page---user_mention--e7bb1f3ad091---------------------------------------)


! The familiarity was quite important because it meant the velocity hit during this transition would be minimized.


3. Ecosystem


C Sharp is the[fifth most popular programming language](https://survey.stackoverflow.co/2025/technology) (excluding SQL, HTML, and Bash). While it’s less popular than TypeScript or JavaScript, I generally don’t find continued benefits linearly with the size of the ecosystem. Rather, it’s much more similar to logarithmic growth. After a certain point, it plateaus and there isn’t more marginal utility to the extra library, especially when the quality of many of the libraries quite frankly isn’t that high. In that sense, it’s more of a binary test: is the ecosystem *large enough* — not *is this the largest ecosystem* . For our use cases, it passed the test with flying colors. C Sharp has a rich history of[event sourcing](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing) , world class support for the[actor model](https://learn.microsoft.com/en-us/dotnet/orleans/overview) , and really great[message buses and more](https://github.com/jasperfx) .


4. AI Coding


C Sharp and .NET play *very* well with AI coding tools. In general, I’ve found that the stronger the guardrails, the further AI can go without human intervention. When I was playing around with F#, which has a *much* stricter compiler than Javascript or Python, I consistently found I could send Claude on longer, more complex tasks without oversight and simply trust the compiler to catch most common issues. This[Jane Street podcast](https://youtu.be/4llyKNb_lak?si=Vk-HmPT3El_nT3CD) also briefly touches upon this same phenomenon: the stronger the guard rails, the longer AI can go. I think it’s no surprise that Kotlin and Elixir, both very functional,[consistently perform better with code generation](https://youtu.be/4llyKNb_lak?si=Vk-HmPT3El_nT3CD) (C Sharp is top 5 though!).


Strictness of the compiler aside, C Sharp has *hundreds* of well established and very performant lint rules thanks to the very well document[Roslyn compiler API](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/) . One of those lint rules mandates that all public methods and fields have structured XML documentation around them, which is a[well-known happy path for AI models](https://fireflower.ai/blog-full/strategic-use-of-xml-tags-in-prompts) . Combined with the sheer volume of well crafted examples from throughout the .NET ecosystem, my very unscientific, vibes-based findings are that AI models do indeed write better C Sharp code than Typescript code given a similar amount of prompting effort.


## Conclusion


.NET is not “cool”, and for good reason. For many years it was Windows-only, and it had a reputation of being quite archaic. As Google took Silicon Valley (and the web) by storm, technologies from Mountain View became the de-facto standard.


But .NET Core has been quietly and steadily evolving. This is most definitely *not* the C Sharp of yore. It’s modern, Linux-first, and open source. It has blazing fast performance and a rock-solid, mature runtime.


I recognize this is a contrarian take. But if you’re the type of developer who has an open mind and prefers some against-the-grain type of thinking, then I think Motion is the right place for you.[We’re hiring](https://www.usemotion.com/careers) !


P.S. We’re[not the only ones](https://tracebit.com/blog/why-tracebit-is-written-in-c-sharp) realizing just how productive C# can be!
