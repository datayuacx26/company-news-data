---
schema_version: "1.0.0"
document_id: "2417dd72b6b6644225bbba3d166c45781991e34dc92e2ada6c32aa4fb492e306"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/creating-a-no-code-auto-clicker/"
published_at: "2025-05-08T00:00:00+00:00"
first_seen_at: "2026-07-24T09:27:21.536976+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:0097a0ceac5adba2298e74e182cab41b3f7fe94362087f21c6a9b3795fc1a2a4"
---

# Creating a No-code Auto Clicker in Minutes

[← All blog posts](https://axiom.ai/blog)


# Creating a No-code Auto Clicker in Minutes


May 8, 2025


Creating an auto clicker with axiom.ai can allow you to use the tool to automatically click a button, or element, multiple times. There are many reasons why you may wish to automate this - the primary reason being to avoid a repetitive strain injury! Automatic clickers can help solve a wide range of use cases with browser automation.


## What is an auto clicker bot?


Exactly what it says on the tin, an auto clicker bot is a piece of software that is designed to automatically perform a click action - in this article we are going to be concentrating on auto clicking within a browser environment. They are useful for applications where you may need to click multiple times where you may not wish to do this manually.


## How to create an auto clicker bot


To get started, we are going to be utilising[axiom.ai](https://axiom.ai/) to create a bot that can perform the clicks for us. If you haven't already signed up, you can do so to take advantage of the free trial that's on offer - from our testing for this article, we believe you should be able to create this auto clicker bot within the trial period.


Create a new automation, or navigate to the automation you want to add this functionality to. Ensure that there is a[Go to page](https://axiom.ai/docs/no-code-tool/reference/steps/go-to-page) step added to your automation to ensure that there are elements loaded to be able to click them. We will be using the[Click element](https://axiom.ai/docs/no-code-tool/reference/steps/click-element) step to perform the action on the webpage once it has loaded.


### Using the 'Jump' method


The first step after the "Go to page" step that you will want to add is the[Click element](https://axiom.ai/docs/no-code-tool/reference/steps/click-element) step, this step will allow you to click the element on the page itself. Add the "Click element" step and follow the instructions in the step to select the element that you wish to click.


Once the step has been set up as you require, add a[Jump to another step](https://axiom.ai/docs/no-code-tool/reference/steps/jump-step) step to your automation, you'll want to set this up to "jump back" to the "Click element" step that you previously created. You'll be able to configure the maximum number of jumps that this step will perform - consider the impact on runtime this may have if set to a larger umber.


*Note, if you change the order of steps in the future, you will need to update the "Jump to another step" step with the new step number.*


### Using the 'Loop' method


axiom.ai offers a[Loop through data](https://axiom.ai/docs/no-code-tool/reference/steps/loop) step that is normally used to loop through rows of data within a data token - this allows you access to the individual rows of data. However, this can also be used to loop and perform repetitive tasks as a little "hack".


To get started, create a Google Sheet and add content to the number of iterations that you would like to iterate through. For example, set rows A1-A10 with the numbers 1-10. The content in this case does not matter once the number of rows with content matches the number of iterations that you'd like to perform. Read this data into your automation using a[Read data from a Google Sheet](https://axiom.ai/docs/no-code-tool/reference/steps/read-data-from-a-google-sheet-step) or[Read data from Excel](https://axiom.ai/docs/no-code-tool/reference/steps/read-data-from-excel) step and setting the data token into the "Loop through data" step.


Inside of the "Loop through data" step you can then add the "Click element" step that will perform the click action.


### Using the 'Code' method


What! Code in a no-code article? I know, but hear me out. Using the built-in tools with axiom.ai it's quite easy to use a small about of code to achieve big things, like an auto clicker. This is an alternative option that would use the[Write Javascript](https://axiom.ai/docs/no-code-tool/reference/steps/write-javascript) step to write code that can be used to repeatedly click the button or element. You'll need to know the selector for the element for this method - see our article[The Best Custom CSS Selectors to Use for Automations](https://axiom.ai/blog/best-custom-css-selectors-for-web-scraping) for some tips and tricks.


To get started, create a` for` loop within your code, setting it tot he desired number of iterations, for example:


```text
const   iterations   =   10  ;


for   (  var   i   =   0   ; i   <   iterations ; i  ++  ) {
// Click code
}


```


From here, there are two options that you can take to click the element. Both are written in Javascript, however, one takes advantage of the Puppeteer library. The second option will require that you run your automation locally using the desktop application.


```text
// Pure JS
document.  getElementById  (  "#id"  ).  click  ();


// Puppeteer
await   page.  click  (  '<selector>'  );


```


Using Puppeteer has a few advantages, such as being able to click an element based on text or XPath selectors, for example:


```text
// Click based on text
await   page.  click  (  "a:has-text('Buy now')"  );


// Click based on XPath
await   page.  click  (  'xpath=//input[@id="search-input"]'  );


```


## Additional configuration


These auto clickers on their own are great, however, there may be times when you need to make changes to them in order for them to meet your requirements.


### Adding delays


Delays can be added to all of the above methods, this can be useful if you are waiting for something else to happen, or if you are looking to avoid a site detecting your bot.


For the no-code options, simply add a[Wait](https://axiom.ai/docs/no-code-tool/reference/steps/wait) step after your “Click element" step. Configure this to your desired delay and this will cause a delay between clicks.


The coded option is a bit more involved and does require modification to the code, we’ve included a full example below:


```text
var   i   =   1  ;            // rather than 0
const   delay   =   3000  ;       // milliseconds, change as required
const   iterations   =   10  ;


function   clickLoop  () {
setTimeout  (  function  () {
page.  click  (  '#example'  );
i  ++  ;


if   (i   <   iterations) {
clickLoop  ();
}
}, delay)
};


clickLoop  ();


```


*Note, you may need to also add a “Wait” step after the “Write Javascript” step that equals the total time your loop will take. For the example above, we would add a wait of 31000 (delay * iterations + 1000 (to account for any loading)).*


### Exiting a loop


If you are using your automation to monitor a page for changes, there may not always be a need to complete the entire loop. These are various methods of exiting a loop early in this instance.


**No-code options**


Both options for the no-code methods will make use of[Conditional steps](https://axiom.ai/docs/no-code-tool/reference/steps/#control-flow) to determine when to leave the loop. We won’t be reviewing these steps in this article but we would recommend checking out the documentation on them.


When using the “jump” method, we’ll want to add a “Conditional step”, such as an “If/else” step, to your automation before the “Jump to another step” step. Configure this conditional step as you require, and then add another “Jump to another step” inside the conditional step - you’ll want to set this to “jump” to the step after the original “Jump to another step”.


For the “loop” method, as of the time of writing, we are developing an “End loop” step that can be inserted into a conditional step that can be used to exit your loop.


**Code options**


For the “code” method of looping, you can use an if statement within your loop, adding the break keyword will allow you to break out of the loop, for example:


```text
for   (  var   i   =   0  ; i   <   10  ; i  ++  ) {
if   (i   ===   10  ){
break  ;
}
}


```


## Wrapping up


Performing repetitive tasks by hand can be time consuming, which is exactly where an auto clicker bot comes in. This can be used for a wide range of applications, such as automatically refreshing an element on a page to check if there are updates to the page that you are automating. The ease of development of these no-code auto clicker bots means that there is less friction when creating these, and the code option means that you can expand the functionality to ensure that it meets your requirements.


We look forward to hearing from you on how you have used axiom.ai to create an auto clicker bot, feel free to join us over in our[community](https://reddit.com/r/axiom_ai) .


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- What is an auto clicker bot?
- How to create an auto clicker bot
- Additional configuration
- Wrapping up


By


[Karl Jones](https://axiom.ai/authors/karl-jones) , Technical writer


Karl is a technical writer with axiom.ai with a computer science background and 10+ years of customer support experience. In his spare time he…
