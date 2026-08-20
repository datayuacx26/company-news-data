---
schema_version: "1.0.0"
document_id: "bfaa1688e93d3c6319fbfbb42d5bae1779030a22a0cbd6c2a0466b6660d03e06"
company_key: "yelp-inc-common-stock"
company: "Yelp Inc."
source_id: "yelp-inc-common-stock-rss-c1f3492370c5"
canonical_url: "https://engineeringblog.yelp.com/2026/07/migrating-to-graphql-codegen.html"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:18:26.702617+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:ae4a9e8103b0dcd59a9ab978e7d862153291917d185cb323878890ad77644093"
---

# Migrating from Apollo Tooling to GraphQL Codegen at Yelp

# Migrating from Apollo Tooling to GraphQL Codegen at Yelp


- Igor Kusakov, Software Engineer


- Jul 16, 2026


# Introduction


At Yelp, we rely heavily on[GraphQL](https://graphql.org/) and[Apollo](https://www.apollographql.com/) for data loading in our frontend React monorepo. When a developer writes a GraphQL query or mutation inside a React component, the shape of the response is defined by the query itself — but TypeScript has no way to know what that shape looks like at compile time. That’s where code generation (codegen) comes in.


We rely on codegen to read our queries and server schema, then produce TypeScript type definitions for every operation’s input variables and output data. In our codebase, this looks like:


```text
src/components/UserProfile/
├── UserProfile.tsx          ← contains the GetUser query
└── __generated__/
└── GetUser.ts           ← auto-generated types: GetUser, GetUserVariables


```


Developers import these types into their components, and TypeScript catches mismatches — like trying to read a field you forgot to request — at build time rather than at runtime.


This is the story of how we replaced the deprecated tool that generates those types, touching hundreds of packages and thousands of generated files, without any of our developers needing to change a single line of their own code.


# The Old World: Apollo Tooling


Back in 2020, when Yelp first introduced typed GraphQL queries to our frontend codebase, we evaluated two options:[GraphQL Codegen](https://the-guild.dev/graphql/codegen) and[apollo-tooling](https://github.com/apollographql/apollo-tooling) . At the time, both offered roughly equivalent functionality. We chose apollo-tooling as it worked out of the box without any configuration boilerplate.


The workflow was straightforward. We ran` apollo client:codegen` , and it would walk through our source files, find GraphQL queries and mutations, cross-reference them against the schema, and produce` __generated__` type files alongside the code. These files were` .gitignore` ‘d and regenerated at build time, just like any other build artifact.


For years, it worked fine.


But apollo-tooling was reaching the end of life. In April 2023, Apollo[officially announced the deprecation](https://www.apollographql.com/blog/announcement/backend/announcing-the-end-of-life-schedule-for-apollo-cli-service-commands/) of its CLI service commands and recommended that users migrate to GraphQL Codegen.


As an EOL tool, it had[known issues](https://github.com/apollographql/apollo-tooling/issues?q=is%3Aissue%20state%3Aopen%20codegen) that were no longer being addressed. When something went wrong, diagnosing and fixing the issue was difficult since the tool was no longer being maintained.


We needed a supported replacement.


# Evaluating Alternatives


The Client Data team picked up the migration as a project. We considered three alternatives:


1. **GraphQL Codegen** (configured to match our existing output as closely as possible)
2. **[gql.tada](https://gql-tada.0no.co/)** (a newer approach where types are inferred directly from tagged template literals)
3. **GraphQL Codegen’s native mode** (which, like gql.tada, requires changing how queries are written)


Options 2 and 3 were appealing from a “greenfield” perspective, but they would have required changing the` gql` tagged template syntax in every component across our entire monorepo (to a` graphql()` function call syntax) , adjusting our internal tooling, and rewriting existing wrapper functions. Another challenge was that our code explicitly imports and uses generated nested TypeScript types (child objects inside another type’s selection set), which options 2 and 3 did not natively support. We would have to restructure our code to accommodate these changes. For a monorepo with 500+ packages maintained by dozens of teams, that level of disruption was a non-starter.


We chose Option 1: GraphQL Codegen, configured to produce output that was as close to apollo-tooling’s output as possible. The goal was a **transparent migration** — swap the engine, keep the interface.


This decision didn’t come without debate. We had some discussions about whether we should take this opportunity to modernize our approach entirely — adopt component-based file naming, use GraphQL Codegen’s native custom types (GraphQL Codegen introduces custom wrapper types, for example` T | null` is replaced with` Maybe<T>` ), or even move to a single-file-per-component output model, which was the default for GraphQL Codegen. Each of these ideas had merit, but each also came with a migration cost that would have been borne by every team in the company, not just ours. In the end, pragmatism won: do the migration transparently, remove the deprecated dependency, and leave the door open for future improvements.


# The Devil in the Details


With GraphQL Codegen selected, we now needed it to produce output identical to apollo-tooling’s. This turned out to be tricky.


Below are three sample files:


1. A React component that uses generated types
2. A types file generated by apollo-tooling
3. A types file generated by GraphQL Codegen


```text
// A sample source file with a react component:
// ./UserProfile.tsx


import    {    useQuery  ,    gql    }    from    "  @apollo/client  "  ;
import    type    {
GetUser  ,
GetUserVariables  ,
GetUser_user_account  ,    // <- nested type
}    from    "  ../__generated__/GetUser  "  ;


const    GET_USER_QUERY    =    gql  `
query GetUser($id: String) {
user(id: $id) {
account {
since
status
}
}
}
`  ;


export    const    User    =    ()    =>    {
const    {    data    }    =    useQuery  <  GetUser  ,    GetUserVariables  >  (  GET_USER_QUERY  ,    {
variables  :    {    id  :    '  some_id  '    },
});


// Below is a sample usage of a nested type
const    account  :    GetUser_user_account    =    data  .  user  .  account  ;
return    <  p  >  User account status  {  account  .  status  }  </  p  >;
};


```


A generated apollo-tooling types file that we had:


```text
// Apollo-tooling generates files named by the related query or mutation:
// ./__generated__/GetUser.ts


// apollo-tooling includes related enums from the schema:
export    enum    AccountStatus    {
DISABLED    =    '  DISABLED  '  ,
ENABLED    =    '  ENABLED  '
}


export    interface    GetUserVariables    {
id  :    string    |    null  ;
...
}


export    interface    GetUser_user_account    {
readonly    since  :    DateTime    |    null  ;
readonly    status  :    AccountStatus  ;
}


export    interface    GetUser_user    {
readonly    account  :    GetUser_user_account    |    null  ;
}


export    interface    GetUser    {
readonly    user  :    GetUser_user    |    null  ;
}


```


Compare the code above with the raw output of GraphQL Codegen (before the changes described in this article were upstreamed):


```text
// GraphQL Codegen generates files named by the related react source file:
// ./__generated__/UserProfile.ts


// The globalTypes.ts file is created by the codegen and it contains
// all graphql schema definitions along with the codegen custom types:
import    Types    from    '  ../../__generated__/globalTypes  '  ;


// Note the codegen custom types: Exact<>, InputMaybe<> and Scalar[...]
export    type    GetUserVariables     =    Types  .  Exact  <  {
id  :    Types  .  InputMaybe  <  Types  .  Scalar  [  "  String  "  ][  "  input  "  ]  >  ;
...
}  >  ;


// Note: Types.AccountStatus enum below is imported from globalTypes.ts:
export    type    GetUser_user_User_account_Account    =    {    since  :    DateTime  ,    status  :    Types  .  AccountStatus    };


// Note: nested type name is always constructed from field name (user)
// and type name (User), while apollo-tooling uses only field names:
export    type    GetUser_user_User    =    {    account  :    GetUser_user_User_account_Account    };


export    type    GetUser    =    GetUser_user_User  ;


```


Differences Apollo-tooling GraphQL Codegen


**Generated file name** ./__generated__/GetUser.ts ./__generated__/UserProfile.ts


**Includes from the global schema: enums, input types** inline From external file (globalTypes.ts)


**Custom types** none Maybe<>, InputMaybe<>, Exact<>, Scalar\[…\]


**Nested types names** GetUser_user_account GetUser_user_User_account_Account


None of these differences are wrong — they’re just different. But our codebase had thousands of existing import paths pointing to` __generated__/GetUser.ts` , not` __generated__/UserProfile.ts` .


The same was true for all references to nested type names. We couldn’t just swap tools and tell everyone to update their code.


We started with a **“blackbox”** approach: run GraphQL Codegen as-is, then post-process the generated files with custom Node scripts — splitting files, inlining global types, and stripping custom type wrappers. It worked, but we were adding a lot of fragile code that would be hard to maintain.


We also encountered a concurrency issue with GraphQL Codegen that occurs only in large monorepos like Yelp’s frontend. In our monorepo, a GraphQL fragment defined in one package is often imported by another package. Because GraphQL Codegen was not able to distinguish between these packages, it generated output for both.


However, when we run Codegen across the entire monorepo, we do so with multiple concurrent processes. This means that if packages A and B both depend on a fragment from package C, up to three processes may try to generate types for package C simultaneously! With our “black box” version, we occasionally saw random, inexplicable artifacts in the output, which we attributed to this concurrency issue (although we never confirmed it definitively).


So, we tried another approach: we **forked the relevant GraphQL Codegen plugins and modified them directly** to produce the output we needed while fixing the concurrency issue. Specifically, we patched two plugins:


- **` typescript-operations` plugin** : Modified to inline enum and input type definitions from the schema, use native TypeScript types instead of` Scalars\[...\]` , render` T | null` instead of` Maybe<T>` , and compatible nested type names.
- **` near-operation-file` preset** : Modified to name output files by operation name (not component name), avoid importing` globalTypes.ts` , and skip external packages output (the concurrency issue)


These patches were applied to the compiled plugin files and managed as yarn patch files in our repository.


# Upstreaming Our Changes


Maintaining private forks forever was not a future we wanted (that was our “Plan B”). “Plan A” was to upstream our modifications to the official GraphQL Codegen project.


Without any expectations, we opened a[discussion](https://github.com/dotansimha/graphql-code-generator/discussions/10446) on the GraphQL Codegen GitHub explaining our use case and the changes we needed. We also found a Slack channel and reached out to the GraphQL Codegen maintainers directly. We quickly figured out that the GraphQL Codegen team was already working on a[v6.0.0 release](https://github.com/dotansimha/graphql-code-generator/pull/10496) that aligned closely with what we needed. We decided to join forces.


Working with the maintainer,[Eddy Nguyen](https://github.com/eddeee888) , was a genuinely collaborative experience. We coordinated our work through the same Slack channel and GitHub tools. The GraphQL Codegen repository is large and complex, and Claude AI proved invaluable for navigating the codebase—figuring out how things work and finding the right place for each change.


It was mutually beneficial: we got our changes upstreamed, so we don’t have to maintain them in-house, and the GraphQL Codegen team got a large Yelp codebase for testing the new features. Quite often, a newly added feature would break in Yelp’s codebase on edge cases we hadn’t anticipated.


For example, when we implemented apollo-tooling–style names for nested types (e.g.,` GetUser_user_User_account_Account` →` GetUser_user_account` ), we found that apollo-tooling sometimes appended the type name (along with the field name) to disambiguate unions, so we had to replicate that behavior. Another example: when we implemented inline enums, enums for a fragment defined in an external file were missing. Running the new code across Yelp’s large codebase quickly exposed these problems.


Eventually, our patches evolved into properly configurable features that benefit anyone migrating from apollo-tooling, not just Yelp.


We have presented our mutual work at[GraphQLConf 2026](https://graphql.org/conf/2026/schedule/dcd9017f3882e90a2afbdc44893441cd/?name=The%20Biggest%20Change%20To%20GraphQL%20Codegen%20in%2010%20Years%20-%20Eddy%20Nguyen,%20The%20Guild%20&%20SEEK%20&%20Igor%20Kusakov,%20Yelp) in San Francisco, California:


*The GraphQLConf 2026 page with details of our presentation*


*The GraphQLConf 2026 presentation*


# Compatibility Fixes


Even with the GraphQL Codegen modifications, making every package in our monorepo compile cleanly required fixing individual issues across the codebase.


The issues included, among others:


- Nested types were still named differently
- Enums were placed in a different file
- *“Is possibly null”* and *“has any type”* errors — the kind every TypeScript developer knows all too well


In total, there were only a few hundred errors to fix — a manageable number compared with the tens of thousands we initially had. Claude AI was instrumental in fixing them — working through repetitive type mismatches across hundreds of files is exactly the kind of task where AI shines, and doing it manually would have been far more tedious and time-consuming.


We adopted a **“divide and conquer”** approach: the main code generation script maintained a list of packages still using the old apollo-tooling codegen. We’d fix a batch of packages, remove them from the list, and submit a PR. This let us ship incremental progress rather than one enormous, unreviewable change.


This approach also gave us early feedback — does the new codegen work for everyone? If anything went wrong, we could simply add the problematic package back to the list of packages still running on the old codegen. As it turned out, we never needed to do this — but it felt safer having a backup plan.


This was also the part that required the most cross-team coordination. We had to make sure we could identify who owned the code and could review our changes, sometimes having to track ownership across team restructures. Fortunately, people on Yelp’s Slack were very helpful and directed us to the correct channel.


Over the course of the migration, we submitted a series of PRs to fix these compatibility issues incrementally, each covering a batch of packages.


# The Finish Line


The final PR of the migration was a satisfying one. With all packages migrated off the old codegen path, we were able to remove the entire apollo-tooling infrastructure.


The result was a meaningful reduction in code complexity. The build tooling became simpler, the dependency tree became lighter, and the generated types became more consistent.


Most importantly, **the migration was completely transparent to developers.** No one had to update their imports, change their queries, or learn a new tool. The` __generated__` files they’d been importing for years continued to exist at the same paths, containing the same TypeScript types. If we did our job right, most developers didn’t even notice the switch — which was exactly the goal.


# Lessons Learned


**Transparent migrations are worth the extra effort.** It would have been easier for our team to just adopt GraphQL Codegen’s defaults and tell everyone to update their code. But in a monorepo with 500+ packages and dozens of teams, pushing migration work onto every team would have created months of coordination overhead and resistance. By absorbing the complexity ourselves — modifying plugins, submitting incremental PRs — we kept the blast radius to our team only.


**Upstream your changes.** Maintaining private patches is a liability. Every upstream release becomes a merge conflict. Contributing our changes back to GraphQL Codegen was more work upfront, but it means we’re now running on standard, supported code that we don’t have to maintain alone.


**Divide and conquer beats big bang.** The incremental approach — maintaining a shrinking list of packages still on the old codegen and chipping away at it — let us ship progress continuously, get early feedback, and avoid the risk of a single massive PR that blocks on one team’s review.


**Cross-team coordination in a monorepo is a social problem, not just a technical one.** Finding the right Slack channel for a team you’ve never worked with and that may have been restructured and getting a timely review — these are human challenges, not engineering ones.


# What’s Next


With the migration complete, we’re now running on GraphQL Codegen v6 with upstream support for the features we need. Future improvements — like adopting GraphQL Codegen’s native mode, moving to per-component file output, or integrating with newer tools — are now possible without having to first untangle ourselves from a deprecated dependency.


The` __generated__` directories in our monorepo look the same as they did before. Under the hood, everything is different. The finest migrations are the ones where nobody even noticed.


Because our changes were upstreamed, anyone still on apollo-tooling now has an[officially supported migration path](https://the-guild.dev/graphql/codegen/docs/migration/apollo-tooling) — with configuration options designed to make the switch painless. This is something that now benefits not just Yelp but the wider industry. We set out to solve a Yelp problem. The result is something that works for everyone. That’s open source at its best.


[Tweet](https://twitter.com/share)


### Become an Engineer at Yelp


We work on a lot of cool projects at Yelp. If you're interested, apply!


[View Job](https://www.yelp.careers/us/en/c/engineering-jobs?lever-source=engineering_blog)


[Back to blog](https://engineeringblog.yelp.com/)
