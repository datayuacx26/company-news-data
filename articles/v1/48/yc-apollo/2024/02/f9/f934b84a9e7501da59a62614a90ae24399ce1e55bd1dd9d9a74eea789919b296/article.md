---
schema_version: "1.0.0"
document_id: "f934b84a9e7501da59a62614a90ae24399ce1e55bd1dd9d9a74eea789919b296"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/fun-with-ai-generating-graphql-schemas-in-discord"
published_at: "2024-02-07T13:06:54+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:d95920b6a8eeae2e793406cbe167af77a9a9009d584ee6f293b4dc82e0aae125"
---

# Fun with AI – Generating GraphQL Schemas in Discord

Over the past two years, the emergence of AI-driven tools has significantly transformed the tech landscape. Among these innovations, tools designed specifically for code generation—such as Copilot, CodeLlama, and others—have garnered particular attention from developers.


According to a survey conducted by GitHub, 92% of U.S.-based developers are already using AI coding tools both in and outside of work[(source)](https://github.blog/2023-06-13-survey-reveals-ais-impact-on-the-developer-experience/) .


These tools offer immense value for tasks like rapid prototyping and brainstorming, serving as powerful allies in the development process. However, despite their advanced capabilities, a notable gap exists in their ability to generate high-quality GraphQL schemas.


I’m sure most, if not all of us, wish to craft the finest GraphQL schema possible—even if it’s just for a prototype. If you’ve watched my[“Let’s Build a Supergraph Using ChatGPT” talk at GraphQL Summit](https://www.youtube.com/watch?v=p4jP2xnImAM) , you might know some of the ways you can steer tools like ChatGPT to improve the output of the generated code.


But let’s be real. The results aren’t always perfect, and it almost feels like ChatGPT clings onto some less-than-desirable practices! Consider this scenario: when I request ChatGPT to construct a schema for a note-taking application, this is the outcome:


```text
type Query {
getNotes: [Note]
getNoteById(id: ID!): Note
getTags: [Tag]
}


type Mutation {
createNote(title: String!, body: String!): Note
createTag(name: String!): Tag
addTagToNote(noteId: ID!, tagId: ID!): Note
}


type Note {
id: ID!
title: String!
body: String!
tags: [Tag]
}


type Tag {
id: ID!
name: String!
notes: [Note]
}


```


Clearly, this schema has room for improvement. It includes generally discouraged practices like utilizing ‘get’ in query fields and lacking important elements like pagination. It also overlooks important components such as mutation payloads and input types, essential for successful schema evolution.


During my presentation, I shared our experience with fine-tuning GPT-3.5 for optimized GraphQL schema generation. We did notice improvements on schema generation from the standard models, but it wasn’t flawless. On numerous occasions, the model failed to meet our set guidelines for schema generation or it was generating schemas that were less than ideal.


So we’ve been working more to make the output much better and make sure it would follow the best practices as often as possible, and we are finally ready to share it with you all!


To try it is pretty simple![Join our Discord server](https://discord.gg/SzeywawCa6) , go to the **#ai-schema-design** forum and post a message describing the schema you’d like to create, for example:


**Title:** Note taking application


I’m building a note taking application. Users can create notes, and each note has a title and a body. Users can also create tags, and each tag has a name. Users can add tags to notes, and notes can have multiple tags.


Then after submitting the bot will create a schema based on your inputs! The more specific you are the better the schema will be 😊And you’ll be able to keep iterating with the bot to improve the schema or even to see some operation that you could run with your schema


Here’s an example of the schema generator in action:


I hope this will be useful for your schema design journey, and remember we’ll always be happy to help you with your schemas, so feel free to ping me or any other Developer Advocate in[our Discord server](https://discord.gg/SzeywawCa6) .


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick)
