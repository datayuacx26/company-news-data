---
schema_version: "1.0.0"
document_id: "b32c71b18afcf9b93e048dddec0aea1ebb7884fecf4ffb791687049e2103d372"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/scrape-hacker-news"
published_at: "2024-10-23T12:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:99b6813b33ea51027026daa03d1aff05fa21bfbac905f14dca0f8d4497d6a61a"
---

# How to scrape a website using Browserbase, Puppeteer, OpenAI and Trigger.dev

## What you'll build


In this tutorial, you'll create a Trigger.dev task that scrapes the top 3 articles from Hacker News using[Browserbase](https://browserbase.com/) and[Puppeteer](https://pptr.dev/) , summarizes them with[ChatGPT](https://platform.openai.com/) and sends a nicely formatted email summary to yourself every weekday at 9AM using[Resend](https://resend.com/) .


## Before you begin…


Check out this 4 minute video overview of this tutorial to get an idea of what we'll be building.


## Prerequisites


- Create a[Trigger.dev account](https://cloud.trigger.dev/) and setup a[new project](https://trigger.dev/docs/quick-start)
- Create a[Browserbase account](https://browserbase.com/)
- Install[Puppeteer](https://pptr.dev/guides/installation) on your local machine
- Create an[OpenAI account](https://platform.openai.com/)
- Create a[Resend account](https://resend.com/)


### Warning


When web scraping, you MUST use a proxy to comply with our terms of service. Direct scraping of third-party websites without the site owner’s permission using Trigger.dev Cloud is prohibited and will result in account suspension.


## Configure your environment variables


Login to each of the services and grab the API keys. Add them to your local` .env` file so we can run a local test of our task later on.


`
BROWSERBASE_API_KEY: "<your Browserbase API key>"


OPENAI_API_KEY: "<your OpenAI API key>"


RESEND_API_KEY: "<your Resend API key>"


`


## Install Puppeteer


Before you can run your task locally, you need to install Puppeteer on your local machine. Check out the[Puppeteer installation guide](https://pptr.dev/guides/installation) for more information.


`
npm i puppeteer


`


## Write your task code


Create a new file called` trigger/scrape-hacker-news.ts` in the` trigger` folder in your project and add the following code below.


The best way to understand how the following two tasks work is by following the comments, but here's a quick overview:


1. The parent task` summarizeHackerNews` is set to run every weekday at 9AM using the` cron` property.
2. It connects to Browserbase to proxy the scraping of the Hacker News articles.
3. It then gets the` title` and` link` of the top 3 articles on Hacker News.
4. Next, it triggers a child task called` scrapeAndSummarizeArticle` for each of our 3 articles using the` batchTriggerAndWait` method. You can learn more about batching in[the docs](https://trigger.dev/docs/runs-and-attempts#batchtriggerandwait) .
5. The child task,` scrapeAndSummarizeArticle` , scrapes the content of each article using Puppeteer and summarizes it using ChatGPT.
6. The parent task waits for all of the child tasks to complete before continuing.
7. Finally, the parent task sends an email summary to you using Resend and React Email using the 'summaries' it has generated from the child tasks.


Ensure you replace the placeholder email addresses with your own.


trigger/


scrape-hacker-news.ts


`
import { render } from "@react-email/render";


import { logger, schedules, task, wait } from "@trigger.dev/sdk";


import { OpenAI } from "openai";


import puppeteer from "puppeteer-core";


import { Resend } from "resend";


import { HNSummaryEmail } from "./summarize-hn-email";


const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });


const resend = new Resend(process.env.RESEND_API_KEY);


// Parent task (scheduled to run 9AM every weekday)


export const summarizeHackerNews = schedules.task({


id: "summarize-hacker-news",


cron: {


pattern: "0 9 * * 1-5",


timezone: "Europe/London",


}, // Run at 9 AM, Monday to Friday


run: async () => {


// Connect to Browserbase to proxy the scraping of the Hacker News articles


const browser = await puppeteer.connect({


browserWSEndpoint: \`wss://connect.browserbase.com?apiKey=${process.env.BROWSERBASE_API_KEY}\`,


});


logger.info("Connected to Browserbase");


const page = await browser.newPage();


// Navigate to Hacker News and scrape top 3 articles


await page.goto("https://news.ycombinator.com/news", {


waitUntil: "networkidle0",


});


logger.info("Navigated to Hacker News");


const articles = await page.evaluate(() => {


const items = document.querySelectorAll(".athing");


return Array.from(items)


.slice(0, 3)


.map((item) => {


const titleElement = item.querySelector(".titleline > a");


const link = titleElement?.getAttribute("href");


const title = titleElement?.textContent;


return { title, link };


});


});


logger.info("Scraped top 3 articles", { articles });


await browser.close();


await wait.for({ seconds: 5 });


// Use batchTriggerAndWait to process articles


const summaries = await scrapeAndSummarizeArticle


.batchTriggerAndWait(


articles.map((article) => ({


payload: { title: article.title!, link: article.link! },


idempotencyKey: article.link,


}))


)


.then((batch) =>


batch.runs.filter((run) => run.ok).map((run) => run.output)


);


// Send email using Resend


await resend.emails.send({


from: "Hacker News Summary <[\[email protected\]](https://trigger.dev/cdn-cgi/l/email-protection) >",


to: \["[\[email protected\]](https://trigger.dev/cdn-cgi/l/email-protection) "\],


subject: "Your morning HN summary",


html: render(<HNSummaryEmail articles={summaries} />),


});


logger.info("Email sent successfully");


},


});


// Child task for scraping and summarizing individual articles


export const scrapeAndSummarizeArticle = task({


id: "scrape-and-summarize-articles",


retry: {


maxAttempts: 3,


minTimeoutInMs: 5000,


maxTimeoutInMs: 10000,


factor: 2,


randomize: true,


},


run: async ({ title, link }: { title: string; link: string }) => {


logger.info(\`Summarizing ${title}\`);


const browser = await puppeteer.connect({


});


const page = await browser.newPage();


// Prevent all assets from loading, images, stylesheets etc


await page.setRequestInterception(true);


page.on("request", (request) => {


if (


\["script", "stylesheet", "image", "media", "font"\].includes(


request.resourceType()


)


) {


request.abort();


} else {


request.continue();


}


});


await page.goto(link, { waitUntil: "networkidle0" });


logger.info(\`Navigated to article: ${title}\`);


// Extract the main content of the article


const content = await page.evaluate(() => {


const articleElement = document.querySelector("article") || document.body;


return articleElement.innerText.trim().slice(0, 1500); // Limit to 1500 characters


});


await browser.close();


logger.info(\`Extracted content for article: ${title}\`, { content });


// Summarize the content using ChatGPT


const response = await openai.chat.completions.create({


model: "gpt-4o",


messages: \[


{


role: "user",


content: \`Summarize this article in 2-3 concise sentences:\\n\\n${content}\`,


},


\],


});


logger.info(\`Generated summary for article: ${title}\`);


return {


title,


link,


summary: response.choices\[0\].message.content,


};


},


});


`


## Create your React Email template


Install[React Email](https://react.email/docs/introduction) and the` @react-email/components` package:


`
npm i @react-email/components


`


Create a new file called` summarize-hn-email.tsx` in your project and add the following code. This is currently a simple but nicely styled email template that you can customize to your liking.


summarize-hn-email.tsx


`
import {


Html,


Head,


Body,


Container,


Section,


Heading,


Text,


Link,


} from "@react-email/components";


interface Article {


title: string;


link: string;


summary: string | null;


}


export const HNSummaryEmail: React.FC<{ articles: Article\[\] }> = ({


articles,


}) => (


<Html>


<Head />


<Body style={{ fontFamily: "Arial, sans-serif", padding: "20px" }}>


<Container>


<Heading as="h1">Your Morning HN Summary</Heading>


{articles.map((article, index) => (


<Section key={index} style={{ marginBottom: "20px" }}>


<Heading as="h3">


<Link href={article.link}>{article.title}</Link>


</Heading>


<Text>{article.summary || "No summary available"}</Text>


</Section>


))}


</Container>


</Body>


</Html>


);


`


## Do a test run locally


Once you've written your task code, you can do a test run locally to make sure everything is working as expected. Run the Trigger.dev` dev` command to start the background worker:


`
npx trigger.dev@latest dev


`


Next, go to the Trigger.dev dashboard and click` Test` in the left hand side menu **(1)**


. Choose` DEV` from the environment options at the top of the page **(2)**


, select your task **(3)**


, click` Now` to ensure your task runs immediately **(4)**


, and then click the` Run test` button to trigger your test **(5)**


.


You should see your task run and an email sent to you with the Hacker News summary.


It's worth noting that some Hacker News articles might not be accessible. The tasks will each attempt 3 times before giving up and returning an error. Some reasons for this could be that the main content of the article isn't accessible via the` article` HTML element or that the page has a paywall or the Hacker News post links to a video file. Feel free to edit the task code to handle these cases.


## Deploy your task to the Trigger.dev cloud


Once you're happy with your task code, you can deploy it to the Trigger.dev cloud. To do this, you'll first need to add Puppeteer to your build configuration.


### Add Puppeteer to your build configuration


trigger.config.ts


`
import { defineConfig } from "@trigger.dev/sdk";


import { puppeteer } from "@trigger.dev/build/extensions/puppeteer";


export default defineConfig({


project: "<project ref>",


// Your other config settings...


build: {


// This is required to use the Puppeteer library


extensions: \[puppeteer()\],


},


});


`


### Add your environment variables to the Trigger.dev project


Previously, we added our environment variables to the` .env` file. Now we need to add them to the Trigger.dev project settings so our deployed task can access them. You can copy all of the environment variables from your` .env` file at once, and paste them all into the` Environment variables` page in the Trigger.dev dashboard.


### Deploy your task


Finally, you can deploy your task to the Trigger.dev cloud by running the` trigger.dev@latest deploy` command.


`
npx trigger.dev@latest deploy


`


## Run your task in Production


Once your task is deployed, it will run every weekday at 9AM. You can check the status of your task in the Trigger.dev dashboard by clicking on the` Runs` tab in the left hand side menu.


If you want to manually trigger your task in production, you can repeat the steps from your local` DEV` test earlier but this time select` PROD` from the environment options at the top of the page.
