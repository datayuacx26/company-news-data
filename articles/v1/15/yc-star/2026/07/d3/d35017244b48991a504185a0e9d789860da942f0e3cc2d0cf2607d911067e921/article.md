---
schema_version: "1.0.0"
document_id: "d35017244b48991a504185a0e9d789860da942f0e3cc2d0cf2607d911067e921"
company_key: "yc-star"
company: "Star"
source_id: "yc-star-news-import-45723d8e2a4c"
canonical_url: "https://buildwithstar.com/blog/ai-sprite-animations"
published_at: null
first_seen_at: "2026-07-24T02:51:00.792401+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:cd605f5452b3b4d0980194fe745e3efd08e695720c77815502d4c5ff88064f32"
---

# How Star Creates Sprite Animations with AI

## The Problem


Sprite animations are the lifeblood of 2D games. A character that walks, attacks, jumps, and idles requires dozens of carefully drawn frames. Traditionally, this means hiring a pixel artist, waiting weeks, and spending hundreds of dollars per character.


For a platform where anyone can make a game in 2 minutes, that doesn’t work.


We needed a way to generate sprite animations from a text prompt, instantly, and have them actually work in a game. No sprite sheet editors. No frame-by-frame hand-drawing. Just describe your character and what it should do.


## Step 1: You describe what you want


You type something like:


- “A knight with a sword, walk cycle”
- “A robot, idle animation”
- “A cat wizard casting a spell”


The AI understands what you mean. It knows “walk cycle” means legs moving and arms swinging. It knows “idle” means subtle breathing. It knows “attack” means a weapon swing.


You can pick from common types—walk, run, idle, attack, jump, death, hurt, and cast—or describe something completely custom.


## Step 2: The smart prompt


Behind the scenes, we take your description and construct a very specific prompt for Gemini 3.1 Flash (via the Nano Banana 2 model on fal.ai).


The prompt asks for a **sprite sheet** : a grid of frames arranged in a square. For a 4-frame animation, that’s a 2x2 grid. For 9 frames, 3x3. For 16 frames, 4x4.


The key insight


Gemini can’t generate transparent backgrounds. If you ask for “transparent background,” you get weird checkerboard patterns or inconsistent results. So we don’t ask. We ask for a **bright green background** (#00FF00), like a Hollywood green screen. This gives us a clean, consistent result every time.


The prompt also includes specific instructions about grid layout, frame ordering (top-left is frame 1, read left-to-right), style (pixel art by default), and a negative prompt to avoid blurry results, grid lines, and character inconsistency.


## Step 3: Gemini generates the sprite sheet


Gemini 3.1 Flash generates a single image containing all animation frames arranged in a grid. For a 4-frame fireball animation, you get a 1024x1024 image with four 512x512 frames—your character in different casting poses, all on a bright green background.


For higher frame counts (9 or 16 frames), we request a 2048x2048 image to maintain quality per frame. The whole generation takes a few seconds.


Raw output from Gemini 3.1 Flash: a 2x2 grid of fireball casting frames on #00FF00 green.


## Step 4: The green screen pipeline


This is where the magic happens. We run the generated image through a multi-stage processing pipeline:


Letterbox detection


Sometimes Gemini adds white or gray borders around the image. We scan inward from each edge, detect uniform light-colored bands, and convert them to green so they get removed in the next step.


Green screen removal


We process every pixel. Pure green pixels become fully transparent. Semi-green pixels at character edges get “despilled”—we calculate how green they are, reduce opacity proportionally, and remove the green color cast. This preserves smooth anti-aliased edges without a green fringe.


The result


A clean sprite sheet with true transparency. No green halos. No jagged edges.


Green screen in, transparency out. Same sprite sheet, before and after processing.


## Step 5: Frame extraction


We slice the sprite sheet into individual frames using the grid dimensions. A 2x2 grid on a 1024x1024 image gives us four 512x512 frames.


But we’re not done. Sometimes Gemini adds faint dark borders between grid cells. We handle this with a two-pass algorithm:


1. Extract every frame and detect dark borders at the edges
2. Calculate the maximum border thickness across all frames
3. Trim all frames by the same amount so they stay consistent
4. Resize back to target dimensions


This ensures your animation plays back smoothly with no visual popping between frames.


The 4 individual frames extracted from the sprite sheet, ready for animation.


## Step 6: Game-ready in milliseconds


Everything gets uploaded to cloud storage: the full sprite sheet (for games that want one image) and every individual frame as a separate PNG (for frame-by-frame control).


Each animation gets metadata: frame count, FPS, loop behavior, and URLs for every frame. The game code animates it with just a few lines:


```text
// Animate in the game loop
let elapsed = 0;
game.loop((dt) => {
elapsed += dt;
const frameIndex = Math.floor((elapsed * 8) % frames.length);
ctx.drawImage(frames[frameIndex], x, y, 64, 64);
});
```


That’s it. Text prompt to playable animation.


The Secret Sauce: Reference Images


Generate a character image first, then generate animations—and they’ll look like the **same character** . The system passes your original image to Gemini as a reference, maintaining visual consistency across walk, idle, attack, and every other animation.


Without this, each animation might look like a different character. With it, your wizard looks like *your* wizard in every frame of every animation.


Type your prompt, @mention a character for consistency.


Seconds later: a playable fireball animation, ready to use in your game.


## Why This Matters


Before this pipeline, AI-generated games had two options for character animation:


1.


No animation.


Static sprites that slide around the screen. Functional, but lifeless.


2.


CSS/code-based animation.


Rotating, scaling, or color-shifting a static image. Better than nothing.


3.


Real sprite animations from text.


Characters that actually walk, run, attack, and idle with frame-by-frame animation. The kind that makes a game feel like a game.


Option 3 takes seconds, not weeks.


## Key Takeaways


- 1


Gemini 3.1 Flash generates sprite sheet grids from text prompts in seconds.


- 2


A green-screen pipeline converts AI-generated images into game-ready transparent sprites.


- 3


Reference images keep your character looking consistent across all animations.


- 4


You don’t need to understand any of this. Just say “make a knight game” and it happens.


[Try the Sprite Generator](https://buildwithstar.com/assets)


Or[make a full game](https://buildwithstar.com/) with animated characters.


Related guides:


- [How to Make a Game with AI (No Coding)](https://buildwithstar.com/blog/make-game-with-ai)
- [Claude vs ChatGPT for Making Games](https://buildwithstar.com/blog/claude-vs-chatgpt-games)
