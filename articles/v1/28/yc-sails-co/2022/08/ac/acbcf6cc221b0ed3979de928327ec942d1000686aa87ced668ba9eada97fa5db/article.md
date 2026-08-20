---
schema_version: "1.0.0"
document_id: "acbcf6cc221b0ed3979de928327ec942d1000686aa87ced668ba9eada97fa5db"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/how-i-setup-end-to-end-test-in-sails-with-playwright/"
published_at: "2022-08-26T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:c52eac95326d389bf58d2dc7da909033a774705f1e35e7e80cf6c195d8557dfa"
---

# How I set up end-to-end testing in Sails with Playwright

Playwright is a Node.js library created by Microsoft for end-to-end testing. In this article I will take you step by step on how I setup Playwright for end-to-end testing in a Sails.js codebase. Let’s go!


## Init playwright


The[Playwright docs](https://playwright.dev/docs/intro://) has really done a good job in making the installation and setup of Playwright super easy by providing both an` init` NPM command and a VS Code extension to get started with Playwright. I decided to use the` init` command because I like the CLI 🙂. So I ran:


` npm init playwright@latest`


I got prompted with a series of questions like:


- Do you want to use TypeScript or JavaScript?. I chose JavaScript!
- Where to put your end-to-end tests? tests or e2e. I went with e2e for this one because I already have some functional and unit tests in my codebase powered by Japa.
- Add a GitHub Actions workflow?. I chose yes for this as because I want to be able to run e2e tests via GitHub actions workflow in CI.


After I answered the above questions, the` init` command proceeded to install the Playwright Test -` @playwright/test` dependency - which is the playwright test runner, it also downloaded the browsers and touched my codebase with a config file and it added an` e2e` folder in my project root.


The` init` command also created an example test file within the` e2e` folder and a` test-example` folder with a file filled with useful test scenarios which I used to get a better sense of authoring e2e tests with Playwright.


## Specifying the base URL


The next thing I did was to setup a` baseURL` for my entire end-to-end tests. This will allow me not to repeat myself when visiting pages.


To do this, I opened` playwright.config.js` and then within the` use` object in the config, I uncommented the` baseUrl` property and I updated it to my development URL for testing i.e` localhost:3333` And that’s it! Now I can use` await page.goto('/')` within my tests.


## Setting up the local dev server


I love how convenient Playwright made setting up the dev server of your application before running the tests. All I had to do was, in the same` playwright.config.js` file, I found a commented out section concerning setting up a local dev server and I made it to look like this:


```text
// playwright.config.js
webServer  : {
command  :   'sails_environment="test" sails lift'  ,
port  :   3333  ,
timeout  :   120   *   1000  ,
reuseExistingServer  :   !  process.env.  CI  ,
},
```


You can see I’m telling Playwright the command to use to run my dev server which is` sails_environment="test" sails lift` and I’m specifying the port the dev server will be listening on, the timeout and whether or not to reuse existing server.


Now you might be wondering why I’m telling Playwright that the server is listening in on port` 3333` when the default Sails dev server port is on` 1337` . If you will notice the` sails_environment="test"` line in the` command` section of the` webServer` object, that will make Sails to use the test environment which I’ve already defined earlier on in my codebase when I[set up unit and functional tests with Japa](https://blog.sailscasts.com/testing-your-sails-apps-with-japa/) .


## First test


And that’s it! Set up is all done and I went ahead to author my first Playwright test by renaming` example.spec.js` to` home.spec.js` and then I modified the content to like this:


```text
// ./e2e/home.spec.js


// @ts-check
const   {   test  ,   expect   }   =   require  (  '@playwright/test'  )


test  (  'homepage has Sailscasts in title and Give Sailscasts a try link linking to the pricing page'  ,   async   ({
page,
})   =>   {
await   page.  goto  (  '/'  )


// Expect a title "to contain" a substring.
await   expect  (page).  toHaveTitle  (  /  Sailscasts  /  )


// create a locator
const   getStarted   =   page.  locator  (  'text=Give Sailscasts a Try'  ).  first  ()


// Expect an attribute "to be strictly equal" to the value.
await   expect  (getStarted).  toHaveAttribute  (  'href'  ,   '/pricing'  )


// Click the get started link.
await   getStarted.  click  ()


// Expects the URL to contain pricing.
await   expect  (page).  toHaveURL  (  /  .  *  pricing  /  )
})
```


You can see I went straight for the money on this one by testing if a user can click on the “Give Sailscasts a Try” link and gets navigated to the` /pricing` page 🤑


## Run the tests


Now to run the test, in my terminal I ran:


` npx playwright test`


The above command will ran all tests in headless mode i.e without opening the browsers and just executes in the terminal.


Just to see what headed mode looked like, I ran:


` npx playwright test --headed`


I also ran:


` npx playwright show-report`


To see a webpage with a[report](https://playwright.dev/docs/test-reporters#html-reporter) on my test.


## Conclusion


And that’s it! I love how easy Playwright was to setup and how fast the tests were. Also it was really simple authoring the tests.


I plan on writing more tests and though I already have a test runner with Japa and Playwright let’s you use[third-party test runners](https://playwright.dev/docs/test-runners) , I think I might just stick with this setup as both Japa and Playwright played nicely well together.


Let[me](https://twitter.com/Dominus_Kelvin) know if you give Playwright a try in your codebase.
