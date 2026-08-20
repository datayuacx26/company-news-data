---
schema_version: "1.0.0"
document_id: "ed508208038ea3f307b4bd126b827f2613813cabdf0e82d58782e1c36b359405"
company_key: "yc-todesktop"
company: "ToDesktop"
source_id: "yc-todesktop-news-import-b82ee277d06d"
canonical_url: "https://www.todesktop.com/blog/posts/using-tailwind-for-styling-with-todesktop-builder"
published_at: "2024-01-04T11:33:00+00:00"
first_seen_at: "2026-07-24T04:14:47.478048+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:17e0c6954acd1a93025c7c51e824295784b1af875343be944ba1e93130076a57"
---

# Using Tailwind for styling with ToDesktop Builder

At ToDesktop, we've always had the[ability to add Custom CSS to apps built with ToDesktop](https://www.todesktop.com/docs/recipes/add-custom-css-to-your-desktop-app) . We came up with a super simple solution that works quite well. ToDesktop adds a` todesktop` class to the html element on each page of your app. You can use this to identify when your app is being run as a desktop app.


```text
.announcement   {
background-color  : hotpink;
/* other styles... */
}


/* Hide the announcement in the desktop app */
html  .todesktop    .announcement   {
display  : none;
}


```


While this approach suits many apps, an increasing number of developers are now opting for styling tools like Tailwind.


## Adding Tailwind support


[Peer](https://twitter.com/peer_rich) from[Cal.com](https://cal.com/) reached out and asked if we could add support for tailwind modifiers like` todesktop:hidden` and` mac:rounded-lg` to quickly style up components without resorting to a` global.css` file.


This was a great idea and I was keen to see if we could make it work.


Modifiers in Tailwind control when a particular utility should be applied. For example, the` hover:` variant applies a utility on hover. I needed to find out if we could introduce custom variants like` todesktop:` that would activate only when the app is running inside the ToDesktop environment. So, for example` todesktop:text-xl` would only apply the` text-xl` class when the app is running on ToDesktop.


## Creating a Tailwind plugin


It wasn't immediately obvious how to add support for these modifiers. I was able to find plenty of examples for creating plugins for themes/compononents/utilities but nothing for adding support for modifiers. I stumbled on a blog post from[Charlie Gerard](https://twitter.com/devdevcharlie) that showed me how to[create a variant plugin](https://dev.to/devdevcharlie/writing-a-tailwindcss-variant-plugin-2oe2) .


I wanted this to be backwards compatible with all previous versions of ToDesktop Builder so I needed to rely on the` todesktop` class that we already add to the` html` element.


So, the plan is pretty simple:


1. Define a custom` todesktop:` variant
2. This variant should modify the selector so that it only applies when the` html` element has the` todesktop` class


To achieve this, the following code can be used:


```text
const   plugin =  require  ( 'tailwindcss/plugin'  );


plugin  ( function   ( { addVariant, e }  ) {
addVariant  ( 'todesktop'  ,  ( { modifySelectors, separator }  ) =>   {
modifySelectors  ( ( { className }  ) =>   {
return    `html.todesktop . ${e( `todesktop ${separator}  ${className}  `  )}  `  ;
});
});
});


```


## Adding support for macOS, Windows and Linux


Next, I expanded the plugin to include other ToDesktop-specific scenarios, like differentiating between macOS, Windows and Linux environments (` mac:` and` windows:` and` linux` variants). This would allow developers to tailor their app's UI to each platform's idiosyncrasies.


Here are the classes that are added to the` html` element when the app is running on ToDesktop:


Class name Platform (operating system) target


` .todesktop-platform-win32` Windows


` .todesktop-platform-darwin` macOS


` .todesktop-platform-linux` Linux


In hindsight, I should have used` windows` instead of` win32` and` mac` instead of` darwin` . But this decision was made a long time ago and I didn't want to break backwards compatibility. But this Tailwind plugin allows us to use more friendly names so I created a mapping from the ToDesktop class names to the Tailwind variants:


```text
const   platformMap = {
darwin  :  'mac'  ,
win32  :  'windows'  ,
linux  :  'linux'
};


```


Now, I can iterate over this map and add variants for each platform:


```text
Object  . keys  (platformMap). forEach  ( ( platform  ) =>   {
const   variant = platformMap[platform];
addVariant  (variant,  ( { modifySelectors, separator }  ) =>   {
modifySelectors  ( ( { className }  ) =>   {
return    `html.todesktop-platform- ${platform}   . ${e( ` ${variant}  ${separator}  ${className}  `  )}  `  ;
});
});
});


```


## Real world example


Previously, ToDesktop customers had to add a` global.css` file to their app (or resort to inline` <style />` tags) to make style changes when the app was running on ToDesktop.


Here's an example from Cal.com. They have a header component that previously looked like this:


```text
< style  >
html  .todesktop    aside    header   {
margin-top  : - 12px  ;
flex-direction  : column-reverse;
-webkit-app-region: drag;
}
</ style  >


< header    className  = "items-center justify-between md:hidden lg:flex"  >
<!-- ... -->
</ header  >


```


But, now you can use the` todesktop:` variant! Peer was quick to jump on this and he added support for our new variants to his app. Cal.com is open source so you can take a look at the[PR where Peer added support for the new variants was added over on GitHub](https://github.com/calcom/cal.com/pull/12991/files) .


Now, Peer was able to **remove the global styles** and his header component looks like this:


```text
< header
className  = "todesktop:-mt-3 todesktop:flex-col-reverse todesktop:[-webkit-app-region:drag] items-center justify-between md:hidden lg:flex"
>
<!-- ... -->
</ header  >


```


## Use it with ToDesktop Builder today


Our Tailwind plugin is open-source and can be used with any app built with ToDesktop Builder.[Check it out and install it over on GitHub](https://github.com/ToDesktop/tailwind-variants) .
