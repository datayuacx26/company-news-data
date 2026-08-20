---
schema_version: "1.0.0"
document_id: "658de5f4b48376a0e6cbef2ba5d0e6f67e0d5600d0f7cb2d2443e2771944da74"
company_key: "yc-axiom-ai"
company: "Axiom.ai"
source_id: "yc-axiom-ai-news-import-c535b3e44cdf"
canonical_url: "https://axiom.ai/blog/creating-a-reddit-notification-bot/"
published_at: "2025-04-17T00:00:00+00:00"
first_seen_at: "2026-07-22T21:24:24.051508+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:0842b6e2761c4a4c7e24fb3fdaf169b00ec9150ea906c45cf3313d9882762016"
---

# Creating a Reddit Notification Bot

[← All blog posts](https://axiom.ai/blog)


# Creating a Reddit Notification Bot


April 17, 2025


We had a problem here at axiom.ai, we wanted to be sure that our teams were notified of new posts within our community so that we could review them, help where we can, or simply collect feedback from all of you.


Originally, we had a Zapier workflow that would detect new posts in the subreddit, and then trigger an axiom.ai automation that would then trigger a Slack workflow. Unfortunately, due to changes with Zapier, this stopped working as expected which meant that we missed a few posts.


Let’s look into how we solved this issue and simplified this process purely with axiom.ai and Google Sheets.


## The problem to solve


We will start by defining the problem that we are looking to solve with axiom.ai. Our team wanted to be notified when new posts go live into our community. This would ensure that we can get back to those who need help, and collect any feedback that has been posted there so we can feed it back into our development process.


## Solving the problem


Enter axiom.ai and Google Sheets. To solve this problem we are going to want an automation that can automatically get the data from the subreddit, and then check if there is new data on the subreddit. If there is new data, we then want the automation to let the team know.


Note: this will only track the newest ‘new’ post.


### Setting up your Google Sheet


Before getting started, we would recommend setting up a Google Sheet that will be used to keep track of the most recent post. We created a sheet with three columns and we will be continuing this guide with the assumption that you have done the same. You can see the sample below of how we have it set up.


Subreddit Latest Post Date of last check


r/axiom_ai No current post


The first step in your automation should be a Read data from a Google Sheet that reads in the data that you are using, we set the “First cell” input to “A2”, and the “Last cell” input to “B2” - at this stage we only need the subreddit name, and the latest post URL.


### Getting the subreddit data


The first step that we need to do is to be able to get the data from the subreddit. It would be possible to directly scrape this from the page but we have found that a more reliable method is to do this using the RSS feed that is provided for each subreddit. In our case, the URL we use is the following:


> [https://www.reddit.com/r/axiom_ai/.rss](https://www.reddit.com/r/axiom_ai/.rss)


You can replace the subreddit name with the subreddit that you’d like to scrape. We are going to use the subreddit name that we have read in from the Google Sheet to make the automation more dynamic. You’ll need to set up your Go to page step to do the following:


You can also skip the step of adding in the data token and insert the full URL manually. To the human eye, this looks like a jumble of information, we will look at how we can extract the data next.


### Deciding if there is new content


To determine if there are new posts, we will first need to read the content of the page, and then compare it to the URL of the latest post that we have stored in our Google Sheet. To do this we will need to make use of custom code within the Write Javascript step. To get started, add the following steps to your automation:


1. A Try/catch step - to catch any errors
2. Inside of the “try” section of this step, add a Write Javascript step.


Once this has been finished, you can insert the code below into the Write Javascript step:


```text
// Parse the text that we have retrieved from the 'pre' tag containing the RSS data.
const   parseRss   =   (  rssText  )   =>   {
const   parser   =   new   DOMParser  ()
return   parser.  parseFromString  (rssText,   'text/xml'  )
}


// Extract the individual items from the data into rows to be used in your automation.
const   extractData   =   (  xmlDoc  )   =>   {
// Grab the individual 'entry' items
const   items   =   xmlDoc.  getElementsByTagName  (  'entry'  )
var   entries   =   []


for   (  let   i   =   0  ; i   <   items.  length  ; i  ++  ) {
const   author   =   items[i]
.  getElementsByTagName  (  'author'  )[  0  ]
.  getElementsByTagName  (  'name'  )[  0  ].textContent
const   link   =   items[i].  getElementsByTagName  (  'link'  )[  0  ].  getAttribute  (  'href'  )
const   title   =   items[i].  getElementsByTagName  (  'title'  )[  0  ].textContent


entries.  push  ([author, link, title])
}
return   entries
}


const   preBody   =   document.  getElementsByTagName  (  'pre'  )[  0  ].innerText
const   parsedRss   =   parseRss  (preBody)


return   extractData  (parsedRss)


```


This code will return the author, link and title of the latest post that has been created on the subreddit. This will be stored in the code-data data token that can then be used in later steps of your automation. This code can be extended to take more values out of the code of the page - feel free to copy it and play around with it for yourself!


Once we have this data, we will need to decide if the post is a new post, or if it’s an existing post. To do this, we will want to check the URL of the latest post against the URL of the post that you have stored in your Google Sheet.


To get started, add an “If condition is true, run steps” step to your automation. The data to check will be the URL that has been stored in your Google Sheet - this should be in Column B of your data that was imported in the “Read data from a Google Sheet” step. We will want to compare this to Column B from the` code-data` data token that was exported from the “Write JavaScript” step. It’s important to note that we want the steps inside of the “If condition” step to run if these do not match - enable the “Reverse condition” option.


Now, any steps that you include within the “If condition” step will run when the URL that is stored within the Google Sheet does not match the URL of the latest post.


You can add steps to notify your team of any new posts, this could be done using a Send an Email step, the Trigger a webhook step, or simply just logging this into a Google Sheet. We decided to go ahead and send this onto a Slack channel using our guide on How to automate Slack with axiom.ai.


### Logging the status of the automation


As previously mentioned, the post URL will need to be logged each time a new post has been picked up by the automation. Now that we have confirmation that a new post has been created, we will want to replace the URL of the last post that the team was notified about and replace this with the URL of the latest post. These steps will need to remain inside the “If condition” step that we created above.


First, we will want to replace the URL within the Google Sheet. To do this, create a new “Write data to a Google Sheet” step. Configure this to use the same sheet that you used previously, and set the data to write to be the code-data data token, using Column B, when prompted. The step should be set to “Add to existing data” and “B2” should be entered in the cell.


Next, we are going to log the last time that the automation ran and successfully detected a new post - first, add a “Date and time” step and configure this with the time format that you wish to use. Then, create a new “Write data to a Google Sheet” step. Configure this to use the same sheet that you used previously, and set the data to write to be the date-and-time data token. The step should be set to “Add to existing data” and “C2” should be entered in the cell.


## Wrapping up


In this article, we explored the problem of automating Reddit notifications using axiom.ai and Google Sheets. We walked through the steps involved in setting up the automation, from reading data from a Google Sheet to getting the subreddit data and comparing it to the latest stored post. We also demonstrated how to use custom code to parse the RSS feed and extract the individual items from the data.


By leveraging the power of axiom.ai and Google Sheets, we were able to create a reliable and efficient automation that helps our team stay informed about new posts in our community. This solution allows us to be more responsive to our users and collect valuable feedback that we can use to improve our products and services.


[Join discussion on r/axiom_ai](https://reddit.com/r/axiom_ai)


###### On this page


- The problem to solve
- Solving the problem
- Wrapping up


By


[Karl Jones](https://axiom.ai/authors/karl-jones) , Technical writer


Karl is a technical writer with axiom.ai with a computer science background and 10+ years of customer support experience. In his spare time he…
