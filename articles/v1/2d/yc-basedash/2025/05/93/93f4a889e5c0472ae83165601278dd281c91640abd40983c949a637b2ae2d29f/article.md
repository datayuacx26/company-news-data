---
schema_version: "1.0.0"
document_id: "93f4a889e5c0472ae83165601278dd281c91640abd40983c949a637b2ae2d29f"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-we-built-light-mode-without-tailwind-s-dark-class/"
published_at: "2025-05-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:00c056b29cbb4f41dcf457d9a1848c7249eb783aef2e1c55b9c980837406beba"
---

# How We Built Light Mode Without Tailwind’s dark: Class

We recently added support for light mode in Basedash. We’re on Tailwind v4, but we chose not to use Tailwind’s built-in dark: class approach. Instead, we built our own theming system using CSS variables, global state, and some lightweight utilities.


This post breaks down how our system works, how it avoids common pitfalls like flickering, and why we think this approach is better for scaling and maintainability.


---


## **CSS Variables as the Foundation**


At the core of our system are CSS variables. We define our entire color palette using CSS custom properties, scoped under either the .dark or .light class. For example, we define dark mode values under .dark, and light mode values under .light. Here’s what that looks like in practice:


```text
:root,
.dark {
--theme-1: rgba(60, 80, 255, 1);
...
}


.light {
--theme-1: rgba(6, 29, 224, 1);
...
}
```


We then use Tailwind’s @theme directive to expose those variables as utility classes like text-bdc-theme-1 or bg-bdc-neutral-2. With this approach, we were able to write our components once using these semantic class names, and the right colors were applied based on the current theme.


This pattern means we don’t need conditional class logic like text-gray-900 dark:text-white. Instead, our components stay clean and theme-agnostic. We preferred to do it this way since it makes it easier to scale and introduces far less friction in the event we ever want to support more themes beyond light and dark.


---


## **Preventing Theme Flicker on Load**


One of the most common issues with theming in React apps is a flash of the wrong theme on initial load. This happens when the browser renders HTML before React has a chance to apply the theme. To avoid that, we inject a script directly into the of our document. This script runs immediately—before hydration—and determines which theme to apply.


The script checks for a saved preference in local storage (handled by our global state), falls back to the user’s system preference via matchMedia, and defaults to dark mode if neither is available. Once it determines the correct mode, it adds either a .light or .dark class to the element, ensuring the page renders with the right colors from the very beginning.


We borrowed the general idea from Josh Comeau’s excellent[post on dark mode](https://joshwcomeau.com/react/dark-mode/) and tailored it to our setup.


---


## **Global State Management**


We use[Legend-State](https://legendapp.com/open-source/state/v3/intro/introduction/) to store theme information in global state. There are two separate pieces of data we manage:


- theme: This is the user’s actual preference—‘light’, ‘dark’, or ‘system’. It’s persisted to localStorage automatically via Legend-State.
- colorMode: This represents the theme that’s currently being shown. It’s either ‘light’ or ‘dark’, and it’s computed at runtime based on the HTML class. We don’t persist this value since it’s determined dynamically on page load.


On initial hydration, we read the current mode from the class on and use that to update colorMode:


```text
const hasLightMode = document.documentElement.classList.contains('light');
const hasDarkMode = document.documentElement.classList.contains('dark');


if (typeof window !== 'undefined' && (hasLightMode || hasDarkMode)) {
globalState$.colorMode.set(hasLightMode ? 'light' : 'dark');
}
```


This separation allows us to differentiate between what the user *wants* (theme) and what the app is *doing* (colorMode), which comes in handy for conditionally rendering assets or animations.


---


## **Keeping Everything in Sync**


We wrote a small useThemeSynchronization hook that keeps both the DOM and global state in sync. This hook makes sure that:


1. The right class is applied to the element.
2. System-level preference changes are detected and reflected in the app.
3. The colorMode global state is kept up to date whenever theme changes.


If the user sets their theme preference to ‘system’, we listen for OS-level changes using matchMedia and update accordingly. If they explicitly choose ‘light’ or ‘dark’, we just apply that directly. This keeps the UI responsive to changes without needing to refresh or re-render unnecessarily.


---


## **Forcing Dark Mode on a Component (Even in Light Mode)**


A nice side effect of our CSS variable setup is that we can opt any section of the DOM into a specific theme by simply wrapping it in a div with a .light or .dark class. Tailwind will resolve the color variables based on the nearest matching class in the DOM tree.


We use this in our welcome screen, where we always want the card to render in dark mode—even if the app is in light mode. Since it’s wrapped in a .dark container, it picks up the dark theme values without any extra logic.


```text
<html class="light">
...
<div class="dark">
<!-- This section renders using dark theme styles -->
</div>
</html>
```


This kind of flexibility would be hard to pull off cleanly using Tailwind’s dark: modifiers alone.


---


## **WebGL Animations That Match the Theme**


We use[Unicorn Studio](https://unicorn.studio/) for WebGL-based animations in our app. Each animation is defined by a JSON file. The only reliable way to support both themes is to have separate JSON files for light and dark versions.


Rather than pass the correct file explicitly every time, we simplified the API. We just pass a name prop into the component, and internally we resolve the right asset using colorMode.


```text
const projectSrc = colorMode === 'light'
? srcMap[name].light
: srcMap[name].dark;
```


This keeps component code clean and lets us centralize asset switching logic in one place.


---


## **Why We Skipped Tailwind’s dark: Mode**


Tailwind’s dark: modifier is totally fine for smaller projects or quick themes. But in our case, it introduced complexity we didn’t want to deal with.


Duplicating classes (text-black dark:text-white) throughout the codebase becomes hard to manage at scale. It’s also rigid—if you want to support more than two themes, or theme a component differently from the rest of the page, it starts to break down.


By using CSS variables and defining theme scopes with .light and .dark, we made our code simpler and more flexible. It’s easy to add new themes, avoids unnecessary re-renders, and eliminates the flash of incorrect styling on load. Most importantly, it keeps our component code clean.


---


Let us know if you’re building something similar or thinking of trying this approach—we’re happy to share more details.
