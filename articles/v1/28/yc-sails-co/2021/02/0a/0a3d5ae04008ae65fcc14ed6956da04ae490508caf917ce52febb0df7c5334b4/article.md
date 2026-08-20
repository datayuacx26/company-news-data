---
schema_version: "1.0.0"
document_id: "0a3d5ae04008ae65fcc14ed6956da04ae490508caf917ce52febb0df7c5334b4"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/how-to-configure-eslint-and-standard-js-in-a-sails-project/"
published_at: "2021-02-04T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:3122b06c8bc265a2be35658c1773c64cd708e0aa930f99a4cbd8950f578b8df1"
---

# How to configure ESLint and Standard JS in a Sails project

## ESLint


Eslint is a popular linting tool for JavaScript. It is used to find problems via static analysis across your codebase and report them to you. ESlint also allows you fix these errors automatically when it can. It is highly customizable and you can basically tell it what type of syntax of things in your code to flag as errors which is perfect for enforcing code standards.


## Standard JS


Standard JS on the other hand is an opinionated linter, formatter and JavaScript style guide. What this means is that by default when you are using Standard JS it comes built in with its opinions of how you should write JavaScript.


This is a good thing as the opinions are well researched and adhere to several JavaScript best practices. However, there are scenarios where you will like to override some of the default rules in Standard JS while still reaping the other benefits it provides. Hence…


## ESLint + Standard JS


Since ESLint is highly customizable and Standard JS comes baked in with a sophisticated JavaScript style guide, linting and formatting, we can have the best of both worlds by utilizing both of them together.


We can use ESLint to override the defaults in Standard JS that doesn’t adhere to the style we want.


Now let’s see how we will set this up in a Sails project.


## Starting on a clean slate


We will be assuming a brand new` sails new` generated Sails project for this article but do note you can also follow this setup for an already existing codebase.


With that said, let’s start off by running the following command in our terminal in the location of your chosen:


```text
sails   new   sails-eslint-standardjs-starter   --no-frontend
```


After the project has been created navigate into it by running:


```text
cd   sails-eslint-standardjs-starter
```


Sails comes with it’s own eslint configurations, however we won’t be needing this so we can delete the` .eslintrc` file by running:


```text
rm   .eslintrc
```


You can also delete this file any way you want.


Next we will begin a fresh initialization of ESLint by running:


```text
npx   eslint   --init
```


This setup will start off by asking you a couple of questions.


The very first one is how you would like to use ESLint. Use the arrow key to select the option **To check syntax, find problems, and enforce code style** .


Next it will ask you the type of module your project uses, select **CommonJS (require/exports)** .


Next it will ask you which framework your project use; select **none of the above** if Sails is not on the list then hit enter


It will then ask you where you code runs, select **Node** with your space bar and make sure to deselect the **Browser** option by also with the Space bar key.


It will ask you how you like to define a style for your project, select **Use a popular style guide** . You will then have to choose from a list of style guide, chose **Standard**


finally it will ask what standard you want your config file to be in, chose JavaScript


It will then check for peer dependencies ESLint will need to setup your project based on the choices you have given and then prompt you to allow you install them via NPM, Type in Y if you agree.


When it’s done installing, open up your project in your editor and let’s do some more setups tailoring it to our Sails project needs


## .eslintrc.js


Open up` .eslintrc.js` which is your ESLint config file. You will see a bunch or red squiggly lines if you are on VS Code.


So what happened here? The linting config file is not passing linting! Well the thing is since we said we are using Standard JS as our style guide, Standard flags the config file as Standards do not approve of having quotations in an objects property.


If your editor is configured to fix linting errors on Save, you can simply hit **cmd + s** if you are on a Mac or **ctrl + s** if you are on Windows and that will fix the problem


With that fixed, we need to add some configurations specific to Sails. For example our models in Sails are globally injected, Standard JS will complain if we don’t tell it to allow this. So let’s do this. In the` rules: {}` section of your config file add the following:


```text
'no-undef'  : [  'off'  ],
```


We also need to tell ESLint to approve of the Sails generated comments by adding this rule:


```text
'no-irregular-whitespace'  : [  'off'  ]
```


Overall your rules sections should look like this:


```text
rules  : {
'no-undef'  : [  'off'  ],
'no-irregular-whitespace'  : [  'off'  ],
},
```


Great! still a couple more configurations to go.


We also need to tell ESLint (and inevitably Standard JS) to allow globals as we have a couple of them in Sails. To do this, in the globals section of your configuration file add the following entry so it look like this:


```text
globals  : {
Atomics  :   'readonly'  ,
SharedArrayBuffer  :   'readonly'  ,
Promise  :   true  ,
sails  :   true  ,
_  :   true
},
```


And we are done for this file.


Next we need to tell ESLint not to lint` app.js` as we wouldn’t be editing this file. To do so just add` app.js` in .eslintignore.


## package.json


We also need to be able to run ESLint to check for errors and also to fix auto-fixable problems.


For this we will modify the script section of our package.json and editing the` lint` script to be:


```text
"lint"  :   "eslint ."  ,
```


We will also add another script to fix errors:


```text
"lint:fix"  :   "eslint --fix ."  ,
```


So overall our script section in package.json should look like this:


```text
"scripts"  : {
"start"  :   "NODE_ENV=production node app.js"  ,
"lint"  :   "eslint ."  ,
"lint:fix"  :   "eslint --fix ."  ,
},
```


## Lint-staged


To really enjoy the benefit of linting, it will be nice to lint files before we commit them so we don’t commit files with typos or that basically fail our coding standards. To achieve this we will be using[lint-staged](https://www.npmjs.com/package/lint-staged) a package that allows us to run our linter on files that are staged in git.


To do this let’s make our project a Git repository as lint-staged assumes a git environment. Do this initializing git with:


```text
git   init
```


Then proceed to installing and setting up lint-staged by running:


```text
npx mrm lint  -  staged
```


The above command will install and configure[husky](https://www.npmjs.com/package/husky) and lint-staged.


If you look at your` package.json` you will see these two sections now added:


```text
"husky"  : {
"hooks"  : {
"pre-commit"  :   "lint-staged"
}
},
"lint-staged"  : {
"*.js"  :   "eslint --cache --fix"
}
```


So now when you try to commit your changes to git, lint-staged will run your linter for you. If the linting report errors, the commit will be aborted. Pretty cool eh?


## Pro tip


You can also have lint-staged to run your tests before a commit is made by editing the` precommit` section of the` package.json` to look like this:


```text
"pre-commit"  :   "lint-staged && npm test"
```


> You can see how to set up testing for a Sails project in[this article](https://blog.sailscasts.com/testing-sails-applications-with-mocha-and-super-test)


## Conclusion


In this article we setup linting for a Sails project using ESLint and Standard JS. We took it a step further by automating the linting of our files before we commit them by using lint-staged.


You can find the[example project](https://github.com/sailscasts/sails-eslint-standardjs-starter) we used on GitHub
