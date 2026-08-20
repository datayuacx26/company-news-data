---
schema_version: "1.0.0"
document_id: "4d493476bfb6358527ebc29e6220c1ae0a2337ab4e0e0ff76b04b5427f309474"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/writing-tests-with-llms/"
published_at: "2025-08-27T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:3c6fa7233a0f4b7bc10b02153bb5fe4b9042067ef6a18520a9c9b71a9038766d"
---

# Writing Tests With LLMs

[← All blog posts](https://axiom.ai/blog)


# Writing Tests With LLMs


August 27, 2025


Large-language models (LLMs) are fantastic at writing code, this means that you can take full advantage of them when you are building out your projects. LLMs such as ChatGPT, Gemini, Claude or Le Chat are great at helping you write code - not to mention Cursor or Github Copilot that are specifically designed to help you with code. Whether you are following a standard project timeline or a test-driven development framework, writing tests is essential to ensure that your scripts work as you expect them to.


## Writing the script


We're not going to be looking at writing scripts in this article, but we are going to introduce a script that we are going to use to ask an LLM to help us write tests for. We're going to stick with JavaScript for this article, and we are going to be using Gemini, but this will work for all LLMs. We're going to use a snippet that uses Puppeteer to fills a string into the Google search bar, this will connect to an external browser, go to Google.com, and then fill in the text before closing the browser session.


```text
import   puppeteer   from   'puppeteer-core'


const   start   =   async   ()   =>   {
console.  log  (  'start'  )
const   browser   =   await   puppeteer.  connect  ({
browserWSEndpoint:   '<ENDPOINT>'  ,
})
console.  log  (  'browser connected'  )
try   {
const   page   =   await   browser.  newPage  ()
await   page.  goto  (  'https://google.com/'  )


// Locate the search bar on the page
await   page.  locator  (  'aria/Search'  ).  fill  (  'Testing axiom.ai'  )


await   new   Promise  ((  resolve  )   =>   {
setTimeout  (()   =>   {
resolve  ()
},   5000  )
})


await   page.  close  ()
await   browser.  close  ()
console.  log  (  'browser closed'  )
}   catch   (e) {
alert  (e.message)
await   browser.  close  ()
}
}


start  ()


```


## Prompting the LLM


There are a few things that the LLM will assume when you give it code to write tests from - for example, if you copy in JavaScript code, it's going to assume that you are going to want JavaScript output. However, being specific even where it may seem redundant is recommended to increase your chances of getting a right answer from the beginning. Recommended things to mention in your prompt:


- The programming language you are using
- The testing framework that you want to use
- Any area of the code that you want it to specifically concentrate on


We used the following prompt when prompting the LLM to generate the tests:


> Generate Jest unit tests in JavaScript using the following script:


Followed by the code that we included above.


## The result


Gemini provided us with two tests that can be run to test the code, including the set up before and after the tests run. These tests include:


- should successfully navigate and fill the search bar
- should handle connection errors gracefully


We're going to break down the code into smaller segments, but the full code can be found at the end of the article


### The setup


Step one within the tests is to set up the` beforeEach` and` afterEach` functions within your tests - these functions are called before and after each test is run within the script. These steps can be used to set up mock objects to test your code against. IN this instance we need to set up a mock page, browser, puppeteer instance and timers.


```text
let   mockPage
let   mockBrowser
let   mockPuppeteer
let   mockLocator


beforeEach  (()   =>   {
// Mock the locator first
mockLocator   =   {
fill: jest.  fn  ().  mockResolvedValue  (),
}


// Mock the page object and its methods
mockPage   =   {
goto: jest.  fn  ().  mockResolvedValue  (),
locator: jest.  fn  ().  mockResolvedValue  (mockLocator),   // Return the mock locator
close: jest.  fn  ().  mockResolvedValue  (),
}


// Mock the browser object and its methods
mockBrowser   =   {
newPage: jest.  fn  ().  mockResolvedValue  (mockPage),
close: jest.  fn  ().  mockResolvedValue  (),
}


// Mock the Puppeteer module itself
mockPuppeteer   =   {
connect: jest.  fn  ().  mockResolvedValue  (mockBrowser),
}


// Mock the global setTimeout to prevent the test from waiting 5 seconds.
jest.  useFakeTimers  ()
})


afterEach  (()   =>   {
// Restore the timers after each test
jest.  useRealTimers  ()
})


```


## The tests


We're going to look at one of the two tests generated, the one that tests the main functionality of the script that we are testing - whether or not it fills in the Google search bar. In this case we are testing to confirm that the` fill` command has been called successfully:


```text
test  (  'should successfully navigate and fill the search bar'  ,   async   ()   =>   {
// Wrap the logic in a mock for the global alert function if needed, but since
// the original `alert()` is not standard for Node.js, we should change it to `throw`.
try   {
// Run the main automation function with our mocked puppeteer.
await   runAutomation  (mockPuppeteer)


// Fast-forward time to skip the setTimeout call.
jest.  advanceTimersByTime  (  5000  )


// Assertions to check if the correct methods were called.
// Check that the script connected to the browser.
expect  (mockPuppeteer.connect).  toHaveBeenCalledTimes  (  1  )
expect  (mockPuppeteer.connect).  toHaveBeenCalledWith  (
expect.  objectContaining  ({
browserWSEndpoint: expect.  any  (String),   // We don't need to check the full string
})
)


// Check that a new page was opened.
expect  (mockBrowser.newPage).  toHaveBeenCalledTimes  (  1  )


// Check that it navigated to the correct URL.
expect  (mockPage.goto).  toHaveBeenCalledTimes  (  1  )
expect  (mockPage.goto).  toHaveBeenCalledWith  (  'https://google.com/'  )


// Check that it located the search bar.
expect  (mockPage.locator).  toHaveBeenCalledWith  (  'aria/Search'  )


// Check that it filled the search bar with the correct text.
expect  (mockLocator.fill).  toHaveBeenCalledTimes  (  1  )
expect  (mockLocator.fill).  toHaveBeenCalledWith  (  'Testing axiom.ai'  )


// Check that the page and browser were closed correctly.
expect  (mockPage.close).  toHaveBeenCalledTimes  (  1  )
expect  (mockBrowser.close).  toHaveBeenCalledTimes  (  1  )
}   catch   (e) {
// If an error is thrown, the test will fail.
// This is how we test the happy path.
throw   e
}
})


```


As you can see from the script above, this essentially mimics the actions of the original script - using the mock objects to fill in the gaps of the puppeteer instance that does not exist within the scripts. We can see that the script is testing to confirm that the specific functions that we called in the original script are also being called successfully within the test scripts, such as` mockPage.locator` to locate the searchbar, or` mockLocator.fill` to fill the text into the search bar once it's been found.


## Wrapping up


While the tests and the script above are very basic, the idea remains, allowing LLMs to take a review your code and then output testing scripts for them can speed up development time rapidly. We do still recommend that you have an understanding of the tests that are being output by these LLMs as they can make mistakes and assumptions about the code that you should be able to highlight or manually modify yourself. Even if you decide against using the code that has been generated, this can be used as inspiration when it comes to writing your own code manually.


For junior developers, this can be an extremely helpful learning exercise to understand the structure of the tests within your language of choice, meaning that you can develop your skills to be able to include tests within your projects.


## Full code


The following is the full code provided by Gemini, this has not extensively been tested so we would recommend testing before blindly coping this into your code.


```text
const   start   =   async   ()   =>   {
console.  log  (  'start'  )
const   browser   =   await   puppeteer.  connect  ({
browserWSEndpoint:   '<ENDPOINT>'  ,
})
console.  log  (  'browser connected'  )
try   {
const   page   =   await   browser.  newPage  ()
await   page.  goto  (  'https://google.com/'  )


// Locate the search bar on the page
await   page.  locator  (  'aria/Search'  ).  fill  (  'Testing axiom.ai'  )


await   new   Promise  ((  resolve  )   =>   {
setTimeout  (()   =>   {
resolve  ()
},   5000  )
})


await   page.  close  ()
await   browser.  close  ()
console.  log  (  'browser closed'  )
}   catch   (e) {
alert  (e.message)
await   browser.  close  ()
}
}


// We will use Jest to run these tests.
// This is the beginning of our test suite.
describe  (  'Puppeteer Automation Script'  , ()   =>   {
// Before each test, we'll set up our mocks.
let   mockPage
let   mockBrowser
let   mockPuppeteer
let   mockLocator


beforeEach  (()   =>   {
// Mock the locator first
mockLocator   =   {
fill: jest.  fn  ().  mockResolvedValue  (),
}


// Mock the page object and its methods
mockPage   =   {
goto: jest.  fn  ().  mockResolvedValue  (),
close: jest.  fn  ().  mockResolvedValue  (),
}


// Mock the browser object and its methods
mockBrowser   =   {
newPage: jest.  fn  ().  mockResolvedValue  (mockPage),
close: jest.  fn  ().  mockResolvedValue  (),
}


// Mock the Puppeteer module itself
mockPuppeteer   =   {
connect: jest.  fn  ().  mockResolvedValue  (mockBrowser),
}


// Mock the global setTimeout to prevent the test from waiting 5 seconds.
jest.  useFakeTimers  ()
})


afterEach  (()   =>   {
// Restore the timers after each test
jest.  useRealTimers  ()
})


// Wrap the logic in a mock for the global alert function if needed, but since
try   {
// Run the main automation function with our mocked puppeteer.
await   runAutomation  (mockPuppeteer)


// Fast-forward time to skip the setTimeout call.
jest.  advanceTimersByTime  (  5000  )


// Assertions to check if the correct methods were called.
// Check that the script connected to the browser.
expect  (mockPuppeteer.connect).  toHaveBeenCalledTimes  (  1  )
expect  (mockPuppeteer.connect).  toHaveBeenCalledWith  (
expect.  objectContaining  ({
})
)


// Check that a new page was opened.
expect  (mockBrowser.newPage).  toHaveBeenCalledTimes  (  1  )


// Check that it navigated to the correct URL.
expect  (mockPage.goto).  toHaveBeenCalledTimes  (  1  )
expect  (mockPage.goto).  toHaveBeenCalledWith  (  'https://google.com/'  )


// Check that it located the search bar.
expect  (mockPage.locator).  toHaveBeenCalledWith  (  'aria/Search'  )


// Check that it filled the search bar with the correct text.
expect  (mockLocator.fill).  toHaveBeenCalledTimes  (  1  )
expect  (mockLocator.fill).  toHaveBeenCalledWith  (  'Testing axiom.ai'  )


// Check that the page and browser were closed correctly.
expect  (mockPage.close).  toHaveBeenCalledTimes  (  1  )
expect  (mockBrowser.close).  toHaveBeenCalledTimes  (  1  )
}   catch   (e) {
// If an error is thrown, the test will fail.
// This is how we test the happy path.
throw   e
}
})


test  (  'should handle connection errors gracefully'  ,   async   ()   =>   {
// Mock a connection error.
mockPuppeteer.connect.  mockRejectedValue  (  new   Error  (  'Connection Failed'  ))


// Use a spy to see if browser.close was called.
const   mockBrowserCloseSpy   =   jest.  spyOn  (mockBrowser,   'close'  )


// Expect the function to throw an error, since we've replaced the alert().
await   expect  (  runAutomation  (mockPuppeteer)).rejects.  toThrow  (
'Connection Failed'
)


// Even on failure, the browser should attempt to close.
expect  (mockBrowserCloseSpy).  toHaveBeenCalledTimes  (  1  )
})
})


```


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- Writing the script
- Prompting the LLM
- The result
- The tests
- Wrapping up
- Full code


By


[Karl Jones](https://axiom.ai/authors/karl-jones) , Technical writer


Karl is a technical writer with axiom.ai with a computer science background and 10+ years of customer support experience. In his spare time he…


> "Does exactly what I need it to do!"


Gene L


##### "Axiom has completely transformed how I handle browser automation. The no-code interface is intuitive and powerful."


John Smith


##### "Axiom has saved me countless hours. The automation possibilities are endless."


Robert Taylor


##### "The visual builder is a dream come true. No more writing complex scripts!"


Lisa Anderson


##### "The support team is amazing, and the product just keeps getting better. Worth every penny."


Emma Davis


##### "The best investment we've made in our automation workflow. Simple yet powerful."


Jennifer Brown


##### "The best browser automation tool I've used. The AI features are game-changing for complex workflows."


Sarah Johnson


##### "Finally, a tool that makes browser automation accessible without compromising on power. Highly recommended!"


Michael Chen


Fast to build. Easy to run and scale. Outstanding support.


Get started in minutes.


[Download the chrome extension](https://chromewebstore.google.com/detail/axiom-browser-automation/cpgamigjcbffkaiciiepndmonbfdimbb?hl=en-GB)
