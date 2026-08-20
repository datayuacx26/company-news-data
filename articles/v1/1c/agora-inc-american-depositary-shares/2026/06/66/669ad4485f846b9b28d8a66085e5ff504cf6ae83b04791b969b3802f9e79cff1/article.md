---
schema_version: "1.0.0"
document_id: "669ad4485f846b9b28d8a66085e5ff504cf6ae83b04791b969b3802f9e79cff1"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/building-conversational-ai-interfaces-with-agora-agent-ui-kit-complete-beginner-to-pro-guide"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:b51c15d564fc11ac462ff161a62cc7ca49ec42405e1d91546423590502cd4078"
---

# Building Conversational AI Interfaces with Agora Agent UI Kit (Complete Beginner-to-Pro Guide)

The way users interact with applications is changing fast.


From static UIs to **real-time, conversational, and multimodal experiences** , modern apps are expected to feel more human — supporting voice, chat, video, and even emotional intelligence.


But building such systems from scratch is complex.


👉 That’s where **Agora Agent UI Kit** comes in.


In this guide, you’ll learn:


- What Agora Agent UI Kit is
- How its architecture works
- Step-by-step setup (beginner friendly)
- How to build voice, chat, and real-time AI interfaces


## What is Agora Agent UI Kit?


Agora Agent UI Kit is a **React component library** built for creating AI-powered user interfaces such as:


- Conversational chat apps
- Voice assistants
- Video-based AI agents
- Biomarker-driven UI (emotion & wellness)


It provides a **plug-and-play UI layer** , so you don’t have to build everything from scratch.


*It includes modular components for voice, chat, video, and biomarker-based AI interfaces.*


## Architecture Overview


Below is how the system is structured:


## Key Idea


The UI kit is divided into **modular entry points** :


- **Base**
Core UI components (chat, voice, video, layout)
👉 No additional dependencies required
- **RTC**
Handles real-time audio and video interactions
👉 Requires` agora-rtc-react`
- **Session**
Connects UI directly to AI agents and manages conversations
👉 Requires` agora-agent-client-toolkit` and` agora-agent-client-toolkit-react`
- **Thymia**
Enables biomarker and emotion-based UI features
👉 Requires` agora-rtm-sdk`


This ensures:


- Smaller bundle size
- Flexibility
- Easy scaling


## Why Use Agora Agent UI Kit?


Building conversational AI UIs manually involves:


- Managing real-time streams
- Handling state and transcripts
- Designing consistent UI


This library abstracts all of that.


## Benefits


- Faster development
- Built-in voice support
- Real-time communication
- Modular architecture
- Tailwind-based styling


## Beginner Step-by-Step Guide


Let’s build your first AI interface.


## Step 1: Create a React App


```text
npx create-next-app@latest my-ai-app
cd my-ai-app
```


## Step 2: Install the UI Kit


```text
npm install agora-agent-uikit
```


‍ *React and ReactDOM are required peer dependencies.*


## Step 3: Setup Tailwind CSS


```text
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```


Update config:


```text
export     default   {
content  : [
'./src/**/*.{js,ts,jsx,tsx}'  ,
'./node_modules/agora-agent-uikit/dist/**/*.{js,mjs}'  ,
],
};
```


## Step 4: Build Your First Voice UI


```text
import   { useState }   from     'react'  ;
import   { MicButton, AgentVisualizer }   from     'agora-agent-uikit'  ;
export     default     function     App  (  )   {
const   [micState, setMicState] = useState(  'idle'  );
return   (
<  div     className  =  "flex flex-col items-center justify-center h-screen gap-6"  >
<  AgentVisualizer     state  =  "ambient"   />
<  MicButton
state  =  {micState}
onClick  =  {()   =>
setMicState((s) => (s === 'idle' ? 'listening' : 'idle'))
}
/>
</  div  >
);
}
```


You now have a working voice UI


## Step 5: Add Real-Time Audio


```text
npm install agora-rtc-react
import   { MicButtonWithVisualizer, MicSelector }   from     'agora-agent-uikit/rtc'  ;
```


## Step 6: Connect to AI Agent


```text
npm install agora-agent-client-toolkit agora-agent-client-toolkit-react
import   { ConversationalAIProvider }   from     'agora-agent-client-toolkit-react'  ;
<  ConversationalAIProvider     config  =  {{     channel:   '  demo  ' }}>
<  YourApp   />
</  ConversationalAIProvider  >
```


## Step 7: Add Biomarker UI


```text
npm install agora-rtm-sdk
import   { ThymiaPanel }   from     'agora-agent-uikit/thymia'  ;
<  ThymiaPanel     biomarkers  =  {{     happy:     0.7   }}   isConnected   />
```


## How Everything Works Together


## Customization


Tailwind-based styling:


```text
<MicButton className=  "ring-2 ring-blue-500"   />
```


## Components Overview


## Voice


- MicButton
- AgentVisualizer


## Chat


- SessionTranscript
- ChatInput


## Video


- VideoGrid
- Avatar UI


## Advanced


- ThymiaPanel


All components are modular and reusable.


## Development & Testing


```text
pnpm test
pnpm build
pnpm typecheck
```


Includes a demo Next.js app for reference.


## Final Thoughts


Modern AI apps are becoming:


- Voice-first
- Real-time
- Emotion-aware


Agora Agent UI Kit helps you build all of this — fast.


Instead of worrying about infrastructure, you can focus on:


👉 Creating meaningful AI experiences


## When Should You Use It?


Use this if you’re building:


- AI copilots
- Voice assistants
- Chat apps
- Telehealth platforms
- Emotion-aware systems


‍
