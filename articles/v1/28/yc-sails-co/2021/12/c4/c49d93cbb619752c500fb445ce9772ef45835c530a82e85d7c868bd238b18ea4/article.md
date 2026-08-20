---
schema_version: "1.0.0"
document_id: "c49d93cbb619752c500fb445ce9772ef45835c530a82e85d7c868bd238b18ea4"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/how-I-setup-tailwindcss-and-sails/"
published_at: "2021-12-06T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:42f606558218a97a21b4b15e4043470cc89a50900b16db291deed17738ea004b"
---

# How I setup Sails and Tailwind CSS

I have been using[Tailwind CSS](https://tailwindcss.com/) ever since I grasped the flexibility and power of utility-first CSS frameworks. In this article I will show you how I set up Tailwind CSS in a fullstack Sails project.


After several trials at trying to integrate Tailwind CSS into the asset pipeline of Sails, nothing I came up with felt complete and the DX wasn’t that great.


As always I wanted something super simple for both me and anyone else wanting to integrate Tailwind CSS in their Sails projects.


Fortunately I came across this[tweet](https://twitter.com/adamwathan/status/1467196301238542340?s=20) by Adam Wathan, the creator of Tailwind CSS. The tweet showed how Remix - a fullstack framework - was encouragingt the use of the Tailwind CSS CLI instead of trying to integrate Tailwind CSS into their asset pipeline. Adam further more hinted that he will be recommending this approach going forward.


Frankly, I have thought about this very same approach but I guess I needed the permission of the creator of Tailwind CSS to run with the idea!


## What you will need


For this setup you will need the following:


- A Sails application
- Tailwind CSS
- Tailwind CSS CLI
- [concurrently](https://github.com/open-cli-tools/concurrently) (optional)


## The setup


1. To begin the setup, you will need to have a Sails application, so run:


```text
sails   new   sails-tailwindcss
```


I am assuming that you have the Sails CLI installed.


Then enter into the newly created Sails project after it’s done


```text
cd   sails-tailwindcss
```


Also you can skip the above step if you want to integrate Tailwind CSS in an existing Sails project.


### Remove` assets/styles/importer.less`


This is optional but I am assuming that if you are using Tailwind CSS you wouldn’t need the` importer.less` file. You can remove this file by running the below command in your terminal:


```text
rm   -rf   assets/styles/importer.less
```


### Install Tailwind CSS


Now let’s install TailwindCSS in the Sails project as a dev dependency


```text
npm   i   -D   tailwindcss
```


### Install the Tailwind CSS CLI


Now install the TailwindCSS CLI


```text
npm   i   -g   tailwindcss
```


### Initialized Tailwind CSS


After the above step, initialized Tailwind CSS by running the below command in the root of your Sails project:


```text
tailwindcss   init
```


The above command will create a` tailwind.config.js` file in the root of your Sails project, this file will contain the basic configurations needed for your Tailwind CSS setup to work.


### Edit` tailwind.config.js` to use JIT mode


This is optional but if you are like me and want to live on the edge, you can opt into the[JIT mode](https://tailwindcss.com/docs/just-in-time-mode) for Taildwind CSS.


The JIT feature is still in preview as of the time of writing this article so some details may change when it becomes fully stable.


Your` tailwind.config.js` should look like the below code snippet after you edit it to use JIT mode:


```text
module  .  exports   =   {
mode:   'jit'  ,
purge: [  './views/**/*.ejs'  ],
darkMode:   false  ,   // or 'media' or 'class'
theme: {
extend: {},
},
variants: {
extend: {},
},
plugins: [],
};
```


Notice the mode property is set to` jit` and our purge array is set to purge all` .ejs` files within the` views/` folder and subfolders within it.


### Create a tailwind.css file


Next up we will create a` tailwind.css` file in the root our project. This file will be the input to the Tailwind CSS CLI. After creating this file add the following code:


```text
/* ./tailwind.css */
@tailwind   base;
@tailwind   components;
@tailwind   utilities;
```


### Add a script to compile TailwindCSS and watch for changes


We are almost done! all that’s left is to add a script in our` package.json` to both build and watch for changes in the` ./tailwind.css` file. So your script section in your package.json should look like this.


```text
"scripts"  : {
"start"  :   "NODE_ENV=production node app.js"  ,
"watch:css"  :   "tailwindcss -i ./tailwind.css -o ./assets/styles/tailwind.css --watch"  ,
"test"  :   "npm run lint && npm run custom-tests && echo 'Done.'"  ,
"lint"  :   "./node_modules/eslint/bin/eslint.js . --max-warnings=0 --report-unused-disable-directives && echo '✔  Your .js files look good.'"  ,
"custom-tests"  :   "echo   \"  (No other custom tests yet.)  \"   && echo"
}
```


Notice` watch:css` property is the only addition to the script section.


### Let’s lift!


Alright we are done with the setup! To test it all, you need to run the` watch:css` script in a separate terminal by running:


```text
npm   run   watch:css
```


This will compile your` ./tailwind.css` in the root of your Sails project and then output the final CSS in` ./assets/styles/tailwind.css` . It will also watch for changes in` ./tailwind.css` and update` ./assets/styles/tailwind.css` .


Next lift up your Sails application in a separate terminal.


```text
sails   l
```


And that’s it! You can now use Tailwind CSS in full. If you are using VSCode and you have the Tailwind CSS[IntelliSense extension](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss) installed, you will get autocomplete of all Tailwind CSS classes as you use them! Amazing!


## Trade-off of this approach


Frankly there is no significant trade-off(in my opinion) of setting up Tailwind CSS this way. The only bit I think might be uneasy for some folks is having two terminals open for a Sails project but we will look at how that’s a good thing in a bit.


## Benefits of this approach


Here is what Adam Wathan had to say about using the Tailwind CSS CLI as opposed to integrating Tailwind CSS in the build pipeline of your project.


> The way the JIT engine works is a bit *weird* for a CSS tool, and we have to ask a lot of build tool maintainers in order for it all to work reliably and be efficient. With our CLI, the whole thing is solely our problem, not anyone else’s, and we can optimize much more, too. - Adam Wathan


I think I will go with him on that note and trust the folks who built Tailwind CSS and just use their CLI!


### Other benefits


- Sails auto import` assets/styles/tailwind.css` .[Sails-linker](https://sailsjs.com/documentation/anatomy/tasks/config/sails-linker.js) will auto import CSS, JS within` assets/` folder.
- You can keep the Tailwind CSS CLI running and start and restart your Sails application at will.
- Sails also takes care of minifying` assets/styles/tailwind.css` for production when Sails is running in production mode.


## Using concurrently


Before we wrap up, I’d like to mention that you can avoid having two terminals window open to run your Sails fullstack application by using concurrently - an NPM package to run multiple commands concurrently. Though I don’t recommend this approach but here is how to set it up.


### Install concurrently


```text
npm   i   -D   concurrently
```


### Update package.json


We will add another script called` dev` in the script section so the script section will look like this


```text
"scripts"  : {
"start"  :   "NODE_ENV=production node app.js"  ,
"dev"  :   "  \"  npm run watch:css  \"   \"  node app.js  \"  "  ,
"test"  :   "npm run lint && npm run custom-tests && echo 'Done.'"  ,
"custom-tests"  :   "echo   \"  (No other custom tests yet.)  \"   && echo"
}
```


### Start your Sails app


Now you will run` npm run dev` to start your Sails application when you are developing locally.


The command will both run the` watch:css` script and start your Sails server concurrently!


### Trade-off of this approach


- You will have to start your Sails application with` npm run dev` . I don’t know about you but I like using` sails lift` !
- You always restart the` watch:css` script every time you kill your server.
- The output of the` watch:css` script will pollute Sails log as it keeps updating.


### Benefit of this approach


- You have a single terminal and a single command to run to start your Sails application when developing.


### Note on using concurrently


I personally wouldn’t use concurrently and just have the` watch:css` command run in a separate terminal window in the background and give little to no attention to it. This allows me to focus on the Sails application.


## Conclusion


There you have it, my workflow for using Tailwind CSS in a fullstack Sails application. Let me know what you think about this workflow and if you have any suggestions to make it better do hit me up on[Twitter](https://twitter.com/Dominus_Kelvin) or on the[Sailscasts Discord Server](https://discord.gg/gbJZuNm) .


You can also fork this[repo](https://github.com/sailscastshq/sails-tailwindcss) to quickly get started with this setup.


### Inspirations


- [Adam Wathan’s tweet](https://twitter.com/adamwathan/status/1467196301238542340)
- [https://remix.run/docs/en/v1/guides/styling#tailwind](https://remix.run/docs/en/v1/guides/styling#tailwind)


P.S: If you want a better way to debug your Sails helpers and Waterline queries without using the` sails console` command, check out[guppy](https://guppy.sailscasys.com/) .
