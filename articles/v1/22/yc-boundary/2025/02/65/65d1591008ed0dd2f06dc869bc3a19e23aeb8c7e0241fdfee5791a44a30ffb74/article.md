---
schema_version: "1.0.0"
document_id: "65d1591008ed0dd2f06dc869bc3a19e23aeb8c7e0241fdfee5791a44a30ffb74"
company_key: "yc-boundary"
company: "Boundary"
source_id: "yc-boundary-news-import-b6810cf62b42"
canonical_url: "https://boundaryml.com/blog/react-next-js-integration"
published_at: "2025-02-11T00:00:00+00:00"
first_seen_at: "2026-07-24T21:11:53.929220+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:3c6e51781d2f9128392b58f95356aaa8dcdc6584aaf9e3c653019c6b271ed604"
---

# Full stack BAML with React/Next.js

BAML provides first-class support for React and Next.js applications through automatically generated type-safe hooks and server actions. Let's look at how easy it is to use BAML React hooks to handle the streaming automatically, giving your users real-time generation experience with zero extra code.


Every generated hook in your React code will have a code lens that shows the underlying prompt! This gives you full transparency from your frontend components all the way to the AI prompt, making debugging a breeze.


## Example Usage


Here's a simple example of using BAML with React/Next.js. Once you define your BAML function, BAML will automatically generate a type-safe React hook for you.


```text
// baml_src/story.baml
class Story {
title string @stream.not_null
content string @stream.not_null
}


function TellStory(input: string) -> Story {
client "openai/gpt-4"
prompt #"
Tell me a story.


{{ ctx.output_format() }}


{{ _.role("user") }}


Topic: {{input}}
"#
}


```


Generate react hooks from the BAML function.


```text
$ npx baml-cli generate


```


Use the generated hook in your React component.


```text
// src/app/StoryComponent.tsx
'use client';


import { useTellStory } from '@/baml_client/react/client';


export function StoryComponent() {
// Auto generated hook from BAML function
const tellStory = useTellStory();


return (
<div>
<button
onClick={() => tellStory.mutate("a cat in the hat")}
disabled={tellStory.isLoading}
>
Generate Story
</button>


{tellStory.data && <div>{tellStory.data.title}</div>}
{tellStory.data && <div>{tellStory.data.content}</div>}
</div>
);
}


```


## Reference Documentation


For complete API documentation of the React/Next.js integration, see:


### Core Concepts


- [Generated Hooks](https://docs.boundaryml.com/ref/baml_client/react-next-js/use-function-name-hook) - Auto-generated hooks for each BAML function


### Hook Configuration


- [HookInput](https://docs.boundaryml.com/ref/baml_client/react-next-js/hook-input) - Configuration options for hooks
- [HookOutput](https://docs.boundaryml.com/ref/baml_client/react-next-js/hook-output) - Return value types and states
- [Error Types](https://docs.boundaryml.com/ref/baml_client/errors/overview) - Error handling and types


## Next Steps


- Check out the[BAML Examples](https://github.com/BoundaryML/baml-examples/tree/main/nextjs-starter) for more use cases
