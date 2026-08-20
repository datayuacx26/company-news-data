---
schema_version: "1.0.0"
document_id: "0bdf1cf7ac184843381c40abc3c6aa6c267d1e87613aeb999389a41827865836"
company_key: "yc-testrigor"
company: "testRigor"
source_id: "yc-testrigor-news-import-fd5ad855febb"
canonical_url: "https://testrigor.com/blog/how-to-automate-testing-of-ai-features/"
published_at: "2024-05-20T17:01:29+00:00"
first_seen_at: "2026-07-22T16:09:13.277797+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:da09b54cece8d555d87cfc32d2399a36f3e1ee9ac626956ff0f12c904a1e0877"
---

# AI Features Testing: A Comprehensive Guide to Automation

Pragya Yadav


- [AI Testing Tool](https://testrigor.com/blog/category/ai-testing-tool/)
- [Automated Testing](https://testrigor.com/blog/category/automated-testing/)


**Weekly Newsletter**
Receive weekly testRigor newsletters packed with insights on test automation, codeless testing, and the latest advancements in AI.


To grow your product, you might have already released or will probably release LLM-backed features for your customers in 2024. This is required because knowing customers’ real-time sentiment is crucial in understanding their buying decisions and your business expansion.


With Large Language Models (LLMs), you can gather valuable insights from customer actions in real time and use this information to increase customer satisfaction and business further. For example, with this real-time sentiment analysis, you can reach out to a dissatisfied customer within seconds rather than making them wait for days.


LLMs help your business provide personalized customer experience, real-time customer support, product and service development, emotional intelligence, community management, and social media.


Now comes the question: **How to test LLMs?** Testing LLMs can be tricky, even with manual testing. The reason is simple: you need to know what precisely the input should be and what to expect while testing.


Let’s take it a step further and find out how to automate testing of advanced AI features such as LLMs.


## How to Perform Automation Testing of LLMs


LLMs’ output is often (or always) different, which makes testing them challenging. So, how do you automate those tests? Is there a way to ensure a small typo fix in a prompt doesn’t accidentally derail the whole feature?


The solution is to use another LLM’s intelligence to test LLM-based applications. LLMs such as testRigor make it exceptionally easy to do so. testRigor is a[generative AI](https://testrigor.com/generative-ai-in-software-testing/) -based automation testing tool that lets you perform testing of complex test scenarios using plain English or any other natural language (English, Spanish, Portuguese, German, French) commands.


### Testing AI and LLMs using testRigor


testRigor is an[AI agent](https://testrigor.com/ai-agents-in-software-testing/) that works on NLP, ML, and generative AI. Its simplicity, ease of use, and advanced capabilities to test complex test scenarios are the vision behind testRigor. Know in-depth about[AI in software testing](https://testrigor.com/ai-in-software-testing/) .


Let’s see a few examples to understand how testRigor can be the best companion for testing your LLMs.


#### Example 1: Test positive user sentiment in a chat


You can verify whether the customer chat has a positive message or not using testRigor’s AI as below:


```text
check that "chat" "contains a positive message" using ai
```


#### Example 2: Test the window is chat and the restaurant has positive reviews


You can verify that the window is a chat window, and the meaning of the last message is that this restaurant has positive customer feedback online.


```text
check that "chat" "is a chat and the last message is equivalent to 'this restaurant has a lot of positive reviews'" using ai
```


#### Example 3: Test true and false natural language statements


You can utilize testRigor’s Vision AI to validate a statement about the webpage as below:


```text
check that statement is true "page contains testRigor logo"
```


#### Example 4: Test the positive growth of a graph


You can test that a graph’s image on the webpage shows positive growth. This is possible using testRigor’s Vision AI:


```text
check that page "contains an image of graph of positively growing function" using ai
```


All of the above testRigor’s commands will invoke AI to analyze the page/screen and do complex validations that were only possible to do manually previously. testRigor makes testing advanced AI features such as LLMs extremely easy and straightforward.


Here is a testRigor test case with all the above commands in plain English:


Read[how to create end-to-end tests using testRigor](https://testrigor.com/blog/how-to-do-end-to-end-testing-with-testrigor/) in natural languages.


## testRigor’s Capabilities


- **Email, Phone Call, and SMS Testing:** Use simple English commands to test the[email](https://testrigor.com/blog/how-to-do-email-testing-using-testrigor/) ,[phone calls](https://testrigor.com/docs/language/#phones) , and[SMS](https://testrigor.com/docs/language/#sms) . These commands help validate[2FA scenarios](https://testrigor.com/blog/how-to-do-sms-2fa-and-phone-call-testing-using-testrigor/) , with OTPs and authentication codes being sent via email, phone calls, or via phone text.
- **Reusable Rules (Subroutines):** You can easily create functions for the test steps that you use repeatedly. You can use the Reusable Rules to create such functions and call them in test cases by simply writing their names. See the example of[Reusable Rules](https://testrigor.com/blog/how-to-use-reusable-rules-or-subroutines-in-testrigor/) .
- **Global Variables and Data Sets:** You can import data from external files or create your own global variables and data sets in testRigor to use them in[data-driven testing](https://testrigor.com/blog/how-to-do-data-driven-testing-in-testrigor-using-testrigor-ui/) .
- **2FA, QR Code, and Captcha Resolution:** testRigor efficiently manages the 2FA,[QR Code](https://testrigor.com/blog/how-to-test-qr-codes-using-testrigor/) , and[Captcha resolution](https://testrigor.com/blog/how-to-do-captcha-resolution-using-testrigor/) through its simple English commands.
- **Table Handling** : testRigor simplifies table handling and testing with its easy natural language commands. You don’t need to worry about the DOM anymore. Read:[How to work with tables using testRigor?](https://testrigor.com/blog/how-to-work-with-tables-using-testrigor/)
- **File Upload/ Download Testing:** Execute the test steps involving file download or[file upload](https://testrigor.com/blog/how-to-do-file-testing-using-testrigor/) without the requirement of any third-party software. You can also validate the contents of the files using testRigor’s simple commands.
- **Database Testing:** Execute[database queries](https://testrigor.com/blog/how-to-do-database-testing-using-testrigor/) and validate the results fetched.


[Read the documentation with examples](https://testrigor.com/docs/language/#check) to learn more about the testRigor’s powerful capabilities.


### Why use testRigor for LLM and AI Testing?


Here are few of the many benefits that testRigor provides:


- **Quick Test Creation** : Create tests using[testRigor’s generative AI](https://testrigor.com/generative-ai-in-software-testing/) feature; just provide the test case title/description, and testRigor’s generative AI engine will automatically generate most of the test steps. Tweak a bit, and the plain English (or any other natural language) automated test cases will be ready to run.
- **Eliminate Test Maintenance:** There is no maintenance nightmare because there is no reliance on implementation details. This lack of XPath and CSS dependency ensures ultra-stable tests that are easy to maintain.
- **Import Existing Manual Test Cases:** You can import and refine your manual test cases with reusable steps (subroutines). Import your existing manual test cases from test management tools such as TestRail, PractiTest, Zephyr, etc. Read:[Import test cases from TestRail for execution](https://testrigor.com/testrail-import-test-cases-for-execution/) .
- **Codeless Testing** : testRigor eliminates the need for programming language knowledge by converting English test scripts into actual code internally using advanced Natural Language Processing ([NLP](https://testrigor.com/blog/natural-language-recognition-for-software-testing/) ). You can also use our test recorder to record UI actions and create the test cases easily in plain English, meaning learning to code is unnecessary.
- **Everyone in Team Tests:** Product managers can review test cases; testers and your business analysts, sales, and marketing teams can write and execute test cases using testRigor.
- **Shift Left Testing:** Leverage the power and advantages of[shift left testing with testRigor](https://testrigor.com/blog/shift-left-testing/) . Create test cases early, even before engineers start working on code using[Specification Driven Development (SDD)](https://testrigor.com/blog/how-specification-driven-development-works/) .
- **Single Tool for Every Testing Need** : testRigor enables you to test web,[mobile (hybrid, native)](https://testrigor.com/blog/how-to-do-mobile-testing-using-testrigor/) ,[API](https://testrigor.com/blog/how-to-do-api-testing-using-testrigor/) , and[desktop apps](https://testrigor.com/blog/how-to-do-desktop-testing-using-testrigor/) with minimum effort and maintenance.


testRigor offers an ‘ **AI-driven Test Automation Engineer** ‘ certification for free.[Get your certificate today!](https://testrigor.com/certification/)


To try testRigor, just register, and you can start test automation immediately with no learning curve![Sign up here](https://testrigor.com/sign-up/) .


You're 15 Minutes Away


From Automated Test Maintenance and Fewer Bugs in Production


Simply fill out your information and create your first test suite in seconds, with AI to help you do it easily and quickly.


Achieve More Than **90% Test Automation**


Step by Step **Walkthroughs and Help**


**14 Day Free Trial** , Cancel Anytime


“We spent so much time on maintenance when using Selenium, and we spend nearly zero time with maintenance using testRigor.”


Keith Powe


VP Of Engineering - IDT


[Start testRigor Free](https://testrigor.com/sign-up/)


[Request a Demo](https://testrigor.com/request-demo/)
