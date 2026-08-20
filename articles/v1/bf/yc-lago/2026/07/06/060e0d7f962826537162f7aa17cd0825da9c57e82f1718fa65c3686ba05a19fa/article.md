---
schema_version: "1.0.0"
document_id: "060e0d7f962826537162f7aa17cd0825da9c57e82f1718fa65c3686ba05a19fa"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/we-killed-our-motion-design-job"
published_at: "2026-07-21T00:13:11+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:4afcfb8680676fb76f7b75050710845e99ff79314ba375a76b6f0c9cf9b50c4a"
---

# We killed our motion design job

We never hired a motion designer. Now we never will.


Every major Lago feature now ships with a launch film. A coding agent produces it through a skill we wrote. The skill creates three artifacts: a storyboard, an animatic, and the final film. A human approves the first two. The expensive render only starts after both gates pass.


Six videos in, feedback dropped from a dozen rounds to about one. The marginal cost is now one prompt, two approvals, and thirty minutes of unattended rendering.


We turned motion design into a build step.


## The launch gap


A product team that ships weekly produces more features than marketing can launch.


Every feature gets a changelog entry. The big ones get a blog post. Almost none get a film. Film has the worst cost structure in marketing: an external studio, weeks of back and forth, and a budget that reserves it for two launches a year.


It is also the asset that works. It autoplays in the feed. It shows the product doing the thing instead of claiming it.


A changelog entry says: “you can now hold a subscription until its first payment succeeds.” A twenty-second film shows the click, the incomplete state, the payment landing, and the status flipping to active. Our buyers are engineers and finance teams. They trust what they can see.


The best launch asset was the one we produced least.


## The Linear question


Our reference was[Linear's launch films](https://www.linkedin.com/posts/new-agent-assisted-project-updates-1-from-ugcPost-7473410837597036545--8ak/) . No voiceover. No stock footage. No abstract 3D shapes. The camera moves through the real product, one action at a time. Twenty seconds later, you understand the feature.


Linear has a design team producing those films.


We asked a different question: how close could we get with no budget beyond AI tokens?


## Prompts don't compound


The first attempt was a one-off prompt to a coding agent. It got surprisingly far.


We spent a day giving feedback: slow the transitions, use the real icons, make the text bigger, stop inventing screens. The video got good. Then the next feature arrived. The conversation was gone. The agent started from zero and repeated every mistake.


A prompt does not compound. A skill does.


We moved every correction into a versioned instruction set: the motion grammar, design tokens, camera engine, storyboard schema, validators, and workflow. It lives in a repo next to our code. It gets reviewed like code. Every film improves the next one.


The skill does not output a video. It outputs three artifacts in sequence. Each one is a gate.


Everything upstream of approval is cheap to change. Everything downstream is mechanical.


## The closed palette


The naive way to make AI do motion design is to ask it to imagine your product. The result looks like AI: screens that almost match the app, icons that never existed, and data that reads like lorem ipsum wearing a suit.


We inverted the constraint. The film may only use real pixels.


The skill pulls the feature's Figma frames at 3x resolution and films them with a virtual camera: zoom into the action, whip-pan between screens, hold the payoff. Frame metadata provides the coordinates for every element. When a payment lands or a status changes, the skill patches that exact region. If a required screen does not exist in Figma, it asks for one.


Inventing UI is forbidden by construction.


The same rule applies to the design system. Colors, typography, radii, shadows, and SVG paths come from our published package. Gradient backgrounds come from our brand files. Custom elements are composed from design-system primitives.


This creates a closed palette. The agent cannot pick the wrong blue, a lookalike icon, or an off-brand background. Those options do not exist in its world.


## Approve twice, render once


A finished film is the most expensive artifact to disagree with. The two human gates sit before the expensive step.


### The storyboard


The storyboard is a machine-readable file called` storyboard.json` . A TypeScript schema makes it the contract for every project. A shot looks like this:


```text
{
"shotNumber"  :     3  ,
"startFrame"  :     310  ,
"endFrame"  :     480  ,
"cameraFraming"  :     "locked on the subscription overview"  ,
"focalObject"  :     "status chip"  ,
"userAction"  :     "the customer's card payment succeeds"  ,
"productReaction"  :     "Incomplete dissolves to Active, chip pops"  ,
"informationRevealed"  :     "payment is the activation trigger"  ,
"readingHold"  :     {     "startFrame"  :     400  ,     "endFrame"  :     480  ,     "density"  :     "readable"     }  ,
"eyeTraceDestination"  :     "status chip, upper left"  ,
"transitionType"  :     "same-screen"  ,
"requiredSourceAsset"  :     "figma:7855:9040 subscription.details (active)"  ,
"cropSafety"  :     "reframable"  ,
"advances"  :     [  "Y"  ]  ,
"reviewFrames"  :     {     "arrival"  :     318  ,     "settled"  :     420  ,     "transitionMidpoint"  :     387     }
}
```


The file opens with one narrative contract: because the user does X, Lago changes Y, producing Z business outcome. X, Y, and Z are named story axes. Every shot must advance one. A shot that advances none gets deleted.


The header also fixes the flow, frame budget, sound mode, and what should feel different from the previous film. Durations, easings, springs, and blur amounts come from motion tokens. Deviations require an exception with evidence, risk, and validation.


Before a human sees it, a validator catches bad frame ranges, reading holds that are too short, missing review frames, and shots that advance no axis.


The machine argues about arithmetic. The human argues about the story.


### The animatic


The animatic is a complete render. Low fidelity on purpose.


The skill builds the skeleton in[Remotion](https://www.remotion.dev/) , where the video is a React component and every frame is a function of time. Grey placeholders stand in for screens. Only four things are real: the camera, cuts, focal markers, and timing. Diagnostics show shot numbers and frame counters.


Nothing is there to admire. That is the point.


The animatic answers the expensive questions early: does the rhythm work, does the eye land in the right place, and is the story understandable on mute? Approval is recorded in the storyboard. The pipeline refuses to continue without it.


Then production surfaces replace the placeholders. The camera does not change. The timing does not change. The skeleton already passed.


This animatic is automatically produced by our AI skill and must be validated before it starts building the film (Purchase Order feature)


## The shipping gate


The final build emits its own audit trail.


A timeline lists every animation interval and reading hold. An asset manifest records every source, approval, and raster scale. Scripts check the timeline against the motion grammar, scan for banned motion sources, and validate every asset.


A contact sheet renders the declared review frames: arrival, settled state, interaction, transition midpoint, payoff, and end card. A human can scan the full film in one minute.


The encoded masters are checked for resolution, frame rate, duration, and audio. A seven-category scorecard runs. The film ships at 92/100 or it does not ship.


The output is not one file. It includes the 16:9 master, a silent master, 1:1, 4:5, and 9:16 recompositions, a poster frame, a preview, and a checksum manifest. The alternate formats use focal coordinates instead of blind crops.


Thirty minutes. Unattended.


The final result.


## Taste is a spec


The rules were not designed upfront. They were earned one correction at a time: one motion at a time, no intro card, the camera does every transition, screens bleed off the frame instead of floating like cards, never show a screen the design team did not draw.


Six videos later, feedback dropped from a dozen rounds to about one.


The model did not get smarter. Our taste moved from someone's head into a versioned file.


When the output is wrong, we fix the rule, not the video. The next film inherits the correction.


## Why a billing company cares


We compete for developer attention against companies whose brand budgets we will not match. Our lane is velocity and proof.


Velocity means every major feature gets a film. Proof means the film shows the real product.


Product marketing no longer decides which features deserve motion. The long tail that used to ship silently gets the same treatment as the flagship.


It also matches how we build software. We open-sourced our billing engine so buyers could inspect it. Our marketing pipeline now has the same property. The grammar, templates, and design tokens are files anyone on the team can read, run, and improve.


## What it cannot do


It cannot invent screens. That is a feature. If the design frame does not exist, the film waits.


It does not have taste. It has our taste written down, plus two human gates. Judgment is the part we did not automate.


It is also model-dependent. A weaker model produces competent films. A strong one gets the details right.


Linear made launch films a bar. We made them a build step.
