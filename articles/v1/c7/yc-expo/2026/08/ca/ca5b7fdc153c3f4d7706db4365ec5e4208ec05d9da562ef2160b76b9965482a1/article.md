---
schema_version: "1.0.0"
document_id: "ca5b7fdc153c3f4d7706db4365ec5e4208ec05d9da562ef2160b76b9965482a1"
company_key: "yc-expo"
company: "Expo"
source_id: "yc-expo-rss-ee8e6cc345e2"
canonical_url: "https://expo.dev/blog/convex-is-a-backend-for-expo-apps"
published_at: "2026-08-10T22:30:00+00:00"
first_seen_at: "2026-08-10T22:45:02.069285+00:00"
fetched_at: "2026-08-10T22:45:04.469794+00:00"
content_hash: "sha256:438b77f17cc67d8a0d46a8164fa9c6aaf8a9d761a1422ad1313ac0c8f5deddef"
---

# Convex is now a one-command backend for Expo apps

*This is a guest post from Wayne Sutton, Developer Community Builder at Convex. Run one EAS CLI command to provision and connect a reactive, fully typed Convex backend to your Expo app. No separate signup, no dashboards, no copy-pasted tokens.*


…


Here's a moment I see all the time when I'm out talking to developers. You've built the app. The screens work, navigation is smooth, the demo looks great. Then someone asks where the data lives, and you remember you still need a backend. So you go pick a database, make an account somewhere, create a project, copy a deployment URL out of one dashboard, paste it into your environment config, set the same variable again for your build profiles, and hope you got all of it right before the next person runs the project.


The folks at Expo have told me that gap, the one between "I have an app" and "I have an app with a backend," has been the single biggest missing piece in the Expo experience for years. So we worked with them to close it. You can now connect[Convex](https://www.convex.dev/) , the reactive database and backend platform I work on, to your Expo app with one command:


Terminal


` -`` eas


integrations:convex:connect`


That command does the provisioning and the wiring for you. No separate Convex signup flow during onboarding, no OAuth redirect, no juggling two dashboards. You end up with a **fully typed, reactive backend behind your Expo app** and an environment that's already configured for local dev and for your builds. That's the part I'm proud of. It feels less like bolting on a third-party service and more like a feature that was always there.


## What Convex gives you


[Convex](https://www.convex.dev/) is a backend platform with a realtime database, server functions, file storage, search, and scheduling, plus type-safe client libraries. You write backend functions in TypeScript, and the client subscribes to them. When the underlying data changes, your` useQuery` results update on their own. No manual refetching, no cache invalidation code, no REST endpoints to hand-roll.


The types are generated from your backend code, so the query you call in a component and the function you wrote in the` convex/` folder stay in sync. Rename a field on the server and TypeScript tells you where the client breaks. I've watched a lot of developers hit that for the first time at meetups, and it's usually the point where they lean in.


## When to reach for this


Use it when your Expo app needs to store and read data that changes while people are looking at it: chat, presence, live scores, collaborative lists, anything where a screen should update the moment the data does. Convex's subscription model fits that shape well, and you get it without standing up sockets or polling loops.


It's also the fastest path when you're early, and this is the crowd I care about most. You have an idea and an Expo project, and you want a real backend today instead of a week from now. One command gets you there, and you can grow into functions, scheduling, and file storage as you need them.


If your app is a thin client over an existing internal API, or your data is fully static, you probably don't need this. Reach for it when live data is the point.


## How to connect Convex to your Expo app


You'll need an[Expo account](https://expo.dev/signup) , the[EAS CLI](https://docs.expo.dev/eas/cli/) installed with` npm install -g eas-cli` , and an Expo project linked to EAS with` eas init` .


From your project directory, run:


Terminal


` -`` eas


integrations:convex:connect`


The command prompts for a deployment region, a project name, and a team name when it needs one. It only asks for a team name when it has to create a new Convex team connection. You can also pass everything up front:


Terminal


` -`` eas


integrations:convex:connect --region


aws-us-east-1 --team-name "your-team"


--project-name "your-app"


`


Under the hood, one command:


- Installs the` convex` package with` npx expo install convex`
- Creates a Convex team connection for your EAS account, or reuses one you already have
- Creates a Convex project and deployment for this Expo app
- Writes` CONVEX_DEPLOY_KEY` and` EXPO_PUBLIC_CONVEX_URL` to your` .env.local`
- Sets` EXPO_PUBLIC_CONVEX_URL` as an EAS environment variable across your production, preview, and development environments
- Emails an invite to your verified address so you can claim the Convex team and open its dashboard


That last point is worth calling out. The invite is asynchronous, so you don't have to stop and accept it to keep working. Start the Convex dev server right away:


Terminal


` -`` npx


convex dev`


This creates the local` convex/` directory if you don't have one, generates the typed API files, and keeps your functions in sync with your deployment while it runs.


Then wrap your app in a provider. For an Expo Router project, edit` app/_layout.tsx` :


```text
import     {     ConvexProvider  ,     ConvexReactClient     }     from     'convex/react'  ;     import     {     Stack     }     from     'expo-router'  ;
const   convex   =     new     ConvexReactClient  (  process  .  env  .  EXPO_PUBLIC_CONVEX_URL  !  ,     {      unsavedChangesWarning  :     false  ,     }  )  ;
export     default     function     RootLayout  (  )     {        return     (          <  ConvexProvider   client  =  {  convex  }  >            <  Stack     /  >          <  /  ConvexProvider  >        )  ;     }
```


Add a query function in the` convex/` folder:


```text
import     {   query   }     from     './_generated/server'  ;
export     const   get   =     query  (  {      args  :     {  }  ,        handler  :     async   ctx   =>     {          return     await   ctx  .  db  .  query  (  'tasks'  )  .  collect  (  )  ;        }  ,     }  )  ;
```


And call it from a screen:


```text
import     {   api   }     from     '@/convex/_generated/api'  ;     import     {   useQuery   }     from     'convex/react'  ;     import     {     Text  ,     View     }     from     'react-native'  ;
export     default     function     Index  (  )     {        const   tasks   =     useQuery  (  api  .  tasks  .  get  )  ;
return     (          <  View  >            {  tasks  ?.  map  (  task   =>     (              <  Text   key  =  {  task  .  _id  }  >  {  task  .  text  }  <  /  Text  >            )  )  }          <  /  View  >        )  ;     }
```


Change a row in the Convex dashboard and the list on your device updates. That's the whole loop.


## Set it up with an agent


A lot of the developers I work with build with AI agents now, so I tested this that way too. Because it's a single command with predictable flags, it works well as an instruction to a coding agent. Paste something like this:


> In my Expo project, run` eas integrations:convex:connect` to provision and connect a Convex backend. Then add a` ConvexProvider` in` app/_layout.tsx` using` process.env.EXPO_PUBLIC_CONVEX_URL` , write a sample` tasks` query in the` convex/` folder, and render it with` useQuery` on the home screen.


The agent has everything it needs. No dashboard clicks to hand off, no secrets to read back to you.


## Managing the integration


A few commands let you inspect or change the link later:


```text
eas   integrations:convex:project    eas   integrations:convex:dashboard    eas   integrations:convex:team    eas   integrations:convex:team:invite
```


If you unlink with` eas integrations:convex:project:delete` or` eas integrations:convex:team:delete` , EAS removes its own integration metadata. It does not delete anything on Convex, so your data and deployments stay put.


## A few things to know


The command emails a team invite rather than logging you into Convex during the flow, so full dashboard access waits until you claim it. Region choices are` aws-us-east-1` and` aws-eu-west-1` at launch. And` EXPO_PUBLIC_CONVEX_URL` is a public variable by design, since the client needs it to reach your deployment; keep secrets in Convex environment variables or server functions, not in your app bundle.


## Where to start


- One command provisions and connects a Convex backend, with your env vars already set for local dev and builds.
- You get a reactive, fully typed database where` useQuery` updates on its own when data changes.
- Unlinking is safe: it removes EAS metadata, not your Convex resources.


If you have an Expo project already, run` eas init` , then` eas integrations:convex:connect` , and you'll have a backend before your coffee's cold. The full walkthrough lives in the[Using Convex guide](https://docs.expo.dev/guides/using-convex/) , and the[Convex overview](https://docs.convex.dev/understanding/) covers how documents, functions, and subscriptions fit together.


Come tell me what you build. I'm around in the[Convex Discord](https://convex.dev/community) and I read the[Expo Discord](https://chat.expo.dev/) too. I want to hear where this saves you time and where it snags.
