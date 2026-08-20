---
schema_version: "1.0.0"
document_id: "95fac6114f878e5dde1947b65bc6159b407d0a65ce8387cd9a97012ac44c6169"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/react-app-internationalization/"
published_at: "2026-08-15T00:00:00+00:00"
first_seen_at: "2026-08-15T15:41:58.644603+00:00"
fetched_at: "2026-08-15T15:41:59.466191+00:00"
content_hash: "sha256:de44bbc397c191b189c71980c767685392df65df23e258664c8b60bcbda2ff48"
---

# Adding Internationalization to a React App

Adding internationalization to a React app means externalizing every user-facing string into per-language files and rendering them through a translation layer instead of hardcoding text in JSX.


If you’ve ever shipped a build where a raw` t('main.header')` turned up on a customer’s screen, or watched a German string blow out a button that looked fine in English, you already know the setup isn’t the hard part. The wiring takes an afternoon; the locale-specific edge cases take the rest of the sprint. The production-standard way to do this is[react-i18next](https://react.i18next.com/) , the React binding for the i18next framework. Standardize on react-i18next: it’s hooks-based, supports namespaces and lazy loading, works with server-side rendering, and rides the largest i18next plugin ecosystem. Reach for[react-intl](https://formatjs.github.io/docs/react-intl) only if you’re committed to ICU message syntax. This guide covers the correct current setup, then the five things that bite in production: interpolation, pluralization, locale-aware number/date formatting, right-to-left layout, and SSR.


## Key Takeaways


- Set` interpolation.escapeValue: false` in your i18next config because React already escapes values before rendering; leaving i18next’s escaping on double-escapes your strings.
- In current i18next, plural keys use CLDR/Intl suffixes (` _zero` ,` _one` ,` _two` ,` _few` ,` _many` ,` _other` ), and the legacy` _plural` suffix belongs to the old JSON v3 format; the selecting variable must be named` count` .
- Number and date formatting depend on region, not just language, so qualify locales (` en-US` ,` ar-EG` ) and format with i18next’s Intl formatters via` {{value, number}}` and` {{date, datetime}}` .
- Load translations from JSON files with` i18next-http-backend` and a` loadPath` ; inlining them with` require()` ships every language in your main bundle and kills lazy loading.
- On Next.js, don’t hand-roll SSR i18n:` next-i18next` v16 wires up both the App Router and Pages Router in one package.


## How do you set up react-i18next?


Install the core framework, the React binding, and two plugins that handle detection and file loading. Four packages carry the setup, each with a distinct job:


Package Version line Purpose


[i18next](https://www.npmjs.com/package/i18next) 26.x Core engine: lookup, interpolation, plurals, formatting


[react-i18next](https://www.npmjs.com/package/react-i18next) 17.x React binding:` useTranslation` ,` Trans`


[i18next-browser-languagedetector](https://github.com/i18next/i18next-browser-languageDetector) 8.x Detects the user’s language


[i18next-http-backend](https://www.npmjs.com/package/i18next-http-backend) 4.x Loads translation JSON over HTTP


Note one caveat:[i18next-http-backend v4 requires native fetch](https://github.com/i18next/i18next-http-backend) . Node ≥ 18, all modern browsers, Deno, and Bun ship fetch by default. On older runtimes, supply a ponyfill or stay on v3.


```text
npm   install   i18next   react-i18next   i18next-browser-languagedetector   i18next-http-backend
```


Create` src/i18n.ts` and initialize once:


```text
import   i18n   from   '  i18next  '  ;
import   Backend   from   '  i18next-http-backend  '  ;
import   LanguageDetector   from   '  i18next-browser-languagedetector  '  ;
import   {   initReactI18next   }   from   '  react-i18next  '  ;


i18n
.  use  (  Backend  )
.  use  (  LanguageDetector  )
.  use  (  initReactI18next  )
.  init  ({
fallbackLng  :   '  en  '  ,
supportedLngs  :   [  '  en  '  ,   '  es  '  ,   '  ar  '  ],
load  :   '  languageOnly  '  ,
backend  :   {   loadPath  :   '  /locales/{{lng}}/{{ns}}.json  '   },
interpolation  :   {   escapeValue  :   false   },
});


export default   i18n  ;
```


Set` interpolation.escapeValue: false` because React already escapes values before rendering; leaving i18next’s escaping on double-escapes your strings. The` loadPath` matters: inlining resources with` require()` (a Create React App / Webpack-era pattern) ships every language in your main bundle and defeats lazy loading. Import the config once at your entry point, before rendering:` import './i18n';` in` main.tsx` .


## Externalize strings with the useTranslation hook


Translations live in per-language JSON files under` public/locales/<lng>/translation.json` , and components read them through the` useTranslation` hook’s` t` function. Replace every hardcoded string with a key lookup.


```text
{   "  main  "  : {   "  header  "  :   "  Welcome to the app!  "   } }
```


```text
import   {   useTranslation   }   from   '  react-i18next  '  ;


export default   function   Header  ()   {
const   {   t   }   =   useTranslation  ();
return   <  h1  >{  t  (  '  main.header  '  )  }</  h1  >  ;
}
```


Nested keys (` main.header` ) and namespaces organize large string sets. For copy that contains inline markup or links, the plain` t()` call breaks the JSX. Use the[Trans component](https://react.i18next.com/latest/trans-component) instead, which interpolates React elements into a translated sentence while keeping the markup in your component, not your JSON.


```text
<  Trans   i18nKey  =  "  main.docs  "   components  =  {  {   docsLink  :   <  a   href  =  "  https://react.i18next.com/  "   />   }  } />
```


## How do you switch and detect languages?


Change the active language with` i18n.changeLanguage(lng)` ; every component using` useTranslation` re-renders automatically. A switcher is just buttons or a` <select>` calling that method:


```text
const   {   i18n   }   =   useTranslation  ();
<  select
value  =  {i18n  .  resolvedLanguage}
onChange  =  {  (  e  )   =>   i18n  .  changeLanguage  (  e  .  target  .  value  )  }
>
<  option   value  =  "  en  "  >English</  option  >
<  option   value  =  "  ar  "  >العربية</  option  >
</  select  >
```


Detection is handled by the language-detector plugin, which checks sources in a fixed order: query string (` ?lng=en` ), a cookie,` localStorage` , the browser` navigator` , then the` <html lang>` attribute. It stops at the first supported match. It caches the resolved language to` localStorage` , so returning users keep their choice, and a manual` changeLanguage` call updates that cache too.


## The five things that bite


Most i18n bugs live outside the happy path. These are the failure modes that pass local QA and only surface in a user’s locale.


**Interpolation.** Inject dynamic values with` {{var}}` syntax and pass them as the second argument:` t('greeting', { name })` against` "Hello, {{name}}"` . React’s escaping plus` escapeValue: false` keeps this XSS-safe.


**Pluralization.** English needs two plural forms and Arabic needs six, which is exactly why you never hand-write` if (count === 1)` . Pass` count` to` t()` and let` Intl.PluralRules` pick the key. Define forms with[CLDR suffixes](https://www.i18next.com/translation-function/plurals) :` _zero` ,` _one` ,` _two` ,` _few` ,` _many` ,` _other` . The variable must be named` count` .


```text
{
"  messages_one  "  :   "  You have one message  "  ,
"  messages_other  "  :   "  You have {{count}} new messages  "
}
```


The old` _plural` suffix is legacy JSON v3. i18next[streamlined its plural suffixes](https://www.i18next.com/misc/migration-guide) to match the ones used by the Intl API when it introduced the JSON v4 format. Since v24, the Intl API is mandatory: if your runtime lacks` Intl.PluralRules` you have to polyfill it, because the old fallback to v3 plural handling is gone and` compatibilityJSON` no longer accepts` 'v3'` .


**Number and date formatting.** Format with i18next’s built-in Intl formatters:` {{value, number}}` and` {{date, datetime}}` , with options like` {{value, number(style: percent)}}` . Because formatting depends on region, qualify your locales (` en-US` ,` ar-EG` ) so numerals and date order stay consistent across browsers.


**Right-to-left.** For RTL languages, set the document direction from` i18n.dir()` on every language change so the whole layout reflows without per-component CSS:


```text
useEffect  (()   =>   {
const   apply   =   (  lng  :   string  )   =>   {
document  .  documentElement  .  lang   =   lng  ;
document  .  documentElement  .  dir   =   i18n  .  dir  (  lng  );
};
i18n  .  on  (  '  languageChanged  '  ,   apply  );
return   ()   =>   i18n  .  off  (  '  languageChanged  '  ,   apply  );
}, [  i18n  ]);
```


Read` i18n.dir()` inside the` languageChanged` handler, not synchronously mid-switch: after` changeLanguage()` ,` i18next.language` reflects the new language only once resources have loaded.


**SSR.** Don’t hand-roll server-side i18n on Next.js.[next-i18next v16](https://www.npmjs.com/package/next-i18next) is a thin layer over i18next and react-i18next that takes care of the Next.js-specific wiring: middleware, the server/client split, and resource hydration. It supports the App Router (Server Components, Client Components, middleware) and the Pages Router, with` getT()` for Server Components and` useT()` for Client Components. Wrap client trees in` <Suspense>` rather than assuming` window` exists. These locale-specific defects (a raw key like` main.header` rendered to the user, RTL padding clipping text, or fallback-language text leaking into a translated screen) are precisely the ones that pass default-locale QA and only show up when you watch a real session in the target locale, which is where session replay earns its keep.


## Scale with namespaces and key extraction


As string counts grow, split translations into namespaces and load them per route with` useTranslation('dashboard')` , so each page fetches only its own JSON and bundles stay small. Once strings sprawl across the codebase, reach for automated tooling:[i18next-cli](https://github.com/i18next/i18next-cli) is the official, all-in-one command-line tool that handles key extraction, code linting, locale syncing, and type generation, and a translation management system such as Lokalise, Phrase, or Crowdin coordinates translators once real localization begins.


You now have a correct react-i18next setup and a map of the advanced concerns. Wire up the config, externalize your strings, then reach for namespaces when bundles grow and` next-i18next` when you render on the server. Verify exact package versions against npm at install time, since i18next’s core and bindings ship frequently.


## FAQs


What is the difference between i18next and react-i18next?


i18next is the core framework that handles the actual translation logic: key lookup, interpolation, pluralization, and formatting. react-i18next is the React binding layered on top, providing hooks like useTranslation, the Trans component, and automatic re-rendering when the language changes. You install both: i18next does the work, react-i18next connects it to your components. react-i18next requires a modern i18next peer, so keep them on compatible majors.


Why is my translation key showing as literal text instead of the translated string?


A raw key like main.header rendering to the user means the lookup failed to resolve, almost always because the JSON file for that language or namespace never loaded. Common causes: a loadPath that does not match your file location, a namespace not registered, the i18n config not imported before render, or a key that does not exist in the file. Check the network tab for a failed request to your locales path, and confirm the key exists in the correct language file.


Do I still use the _plural suffix for plural keys in i18next?


No. The _plural suffix belongs to the legacy JSON v3 format. Current i18next uses CLDR/Intl word suffixes that match Intl.PluralRules: _zero, _one, _two, _few, _many, and _other. English uses two forms (_one and _other) while Arabic uses all six. The variable selecting the form must be named count and must be present, since there is no fallback if count is missing. If Intl.PluralRules is unavailable you must polyfill it: since v24 there is no fallback to the old v3 plural handling, and compatibilityJSON no longer accepts v3.


Do I need to store translations in JSON files or can I inline them in the config?


You can inline translations via the resources option, but for anything beyond a trivial app you should load them from JSON files with i18next-http-backend and a loadPath such as /locales/{{lng}}/{{ns}}.json. Inlining every language with require() ships all translations in your main bundle and defeats lazy loading, so users download strings for languages they never use. File-based loading fetches only the active language and namespace on demand. Note that i18next-http-backend v4 requires native fetch, meaning Node 18 or newer.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
