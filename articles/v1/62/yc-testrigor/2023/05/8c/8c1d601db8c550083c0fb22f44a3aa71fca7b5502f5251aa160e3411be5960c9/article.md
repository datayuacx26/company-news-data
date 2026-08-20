---
schema_version: "1.0.0"
document_id: "8c1d601db8c550083c0fb22f44a3aa71fca7b5502f5251aa160e3411be5960c9"
company_key: "yc-testrigor"
company: "testRigor"
source_id: "yc-testrigor-news-import-fd5ad855febb"
canonical_url: "https://testrigor.com/blog/servicenow-testing/"
published_at: "2023-05-25T22:34:18+00:00"
first_seen_at: "2026-07-22T16:09:13.277797+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:4ade9c5908d900db2462e1b79126c41bb5508b34d1c95e206b1ab3717756ae36"
---

# ServiceNow Testing

Heather Brooks


- [ERP Testing](https://testrigor.com/blog/category/erp-testing/)


**Weekly Newsletter**
Receive weekly testRigor newsletters packed with insights on test automation, codeless testing, and the latest advancements in AI.


“ *Approximately 85% of Fortune 500 companies use ServiceNow* .” ([source](https://www.servicenow.com/blogs/2023/servicenow-joins-fortune-500-list.html) )


## What is ServiceNow?


A well-known name in the IT service industry, ServiceNow is a cloud-based platform specializing in automating and managing digital workflows for enterprise operations. Founded in 2004, ServiceNow has grown to become a leading provider of IT service management (ITSM) solutions. Still, its offerings have expanded far beyond just ITSM to encompass various areas of enterprise operations. It is designed to help organizations improve operational efficiencies by streamlining and automating routine work tasks.


### The ServiceNow Store


Along with its out-of-the-box products, ServiceNow also includes an app store. This is a repository of pre-built applications and integrations developed by both ServiceNow and independent software vendors. These solutions address specific industry needs, extend core functionalities, and solve unique business challenges.


If you’re planning to add your app to this ServiceNow Store, you need to do the following:


- **Register** as a ServiceNow Partner. You can also apply as an individual developer, though you might miss out on official store listing and monetization features.
- **Develop** your app using ServiceNow’s tools and platforms. Test your app thoroughly before publishing it.
- **Consider user feedback** and incorporate it and community insights during development.
- **Publish your app,** if you’re a partner, and upload your app through the Partner Portal. For individual developers, share your app in the ServiceNow Community.


## Development in ServiceNow


The out-of-the-box products that ServiceNow offers may not fit into your existing system. Hence, you might need to customize it in one of the following ways.


- **Custom app development** : For highly specific needs requiring extensive customization and unique functionalities.
- **Customizations** : For optimizing existing functionalities or integrating with other systems within your organization.
- **App Store app development:** For creating broadly usable solutions with potential revenue generation across a wider audience.


### Configuring v/s Customizing ServiceNow


The easier approach would be to configure ServiceNow using the built-in options and settings to adapt ServiceNow to your specific needs without changing the underlying code.


Here are some examples of configuration.


- **Creating new roles and users:** Define access levels and permissions for different users.
- **Modifying existing forms and fields** : Add, remove, or change forms’ fields to capture the needed data.
- **Creating new workflows and business rules:** Automate tasks and processes based on your specific requirements.
- **Customizing dashboards and reports** : Tailor the information displayed to suit your needs.


If you cannot achieve what you want through configuration, then you can customize ServiceNow. For this, however, you will need coding experience.


Here are some examples of customization.


- **Creating new widgets and UI elements** : Design custom interfaces for specific tasks or data visualization.
- **Developing custom integrations** : Connect ServiceNow with other systems and applications.
- **Modifying core functionalities:** Change the way ServiceNow works at a deeper level.


### Technologies Needed to Customize ServiceNow


The primary programming language used for customizing ServiceNow is *JavaScript* . This is true for various aspects of customization, like


- **Client scripts** : These scripts run on the client side (user’s browser) and are used to manipulate the user interface, validate data, and perform other actions.
- **Server-side scripts** : These scripts run on the ServiceNow server and can interact with the database, send emails, and perform other backend tasks.
- **Business rules:** These are used to automate processes and decisions based on certain conditions. They are also written in JavaScript.
- **UI Macros:** These are reusable components that can be used to build custom user interfaces. They primarily use JavaScript with HTML and CSS for styling.


While JavaScript is the core language, some additional technologies are involved in ServiceNow customization.


- **Glide system:** This JavaScript API provides access to ServiceNow data and functionality.
- **UI Framework:** The user interface of ServiceNow is built using AngularJS, so understanding this framework can be beneficial for complex UI customizations.
- **REST APIs:** These APIs can be used to integrate ServiceNow with other systems and applications. Other languages like Python or Java might be used depending on the integration. However, you may not find enough documentation for working with a programming language other than JavaScript for ServiceNow.


### ServiceNow Tools and Platforms for Customization


ServiceNow offers robust tools and platforms to support customization across various levels, catering to no-code and code-based needs. Here’s a breakdown.


#### No-code and low-code tools


- **Studio:** A drag-and-drop interface for building customized applications, workflows, and user interfaces without writing code. It includes features like


- Form Editor: Designs and modifies forms to capture specific data.
- Workflow Editor: Automates tasks and processes visually.
- UI Builder: Creates custom dashboards and reports.


- **App engine:** Builds enterprise-grade applications with pre-built components and low-code capabilities.
- **Flow designer** : Automates basic tasks and processes without coding.
- **Configuration Management Database (CMDB):** Extend and customize the CMDB using built-in features and integrations.


#### Coding-based Tools


- **Script editor** : Uses JavaScript code for server-side scripts, client scripts, UI Macros, and business rules.
- **IntegrationHub** : Integrates ServiceNow with other systems and applications using pre-built connectors or custom code.
- **Plugin Development Kits (PDKs):** Develop custom plugins to extend ServiceNow functionality with advanced features.
- **ServiceNow Mobile Development Kit (MDK):** Build custom mobile applications for ServiceNow.


#### Platforms


- **Now Platform:** The underlying platform offering core functionalities and APIs for development.
- **ServiceNow Developer Portal** : Provides resources, documentation, and tools for developers.
- **ServiceNow Community** : Connect with other developers and experts for help and learning.


## ServiceNow Automated Testing


Whether you are customizing ServiceNow products, building integrations, or creating apps for the ServiceNow app store, you must perform ServiceNow testing. You can use the following approach to do this.


- Unit testing
- Integration testing
- End-to-end testing


### ServiceNow Unit Testing


Unit testing constitutes the first level of ServiceNow application testing in the[test automation pyramid](https://testrigor.com/blog/test-automation-pyramid-done-right/) . This stage involves examining individual units or components of the application in isolation from the rest of the system to verify that each unit operates as expected. By[catching defects early](https://testrigor.com/blog/minimizing-risks-the-impact-of-late-bug-detection/) in the development cycle, unit testing can save time and resources by preventing more severe bugs further down the line.


You have a wholesome ServiceNow testing framework/tools for unit and integration testing. The ServiceNow testing tools that work best in these cases are as follows.


#### **ServiceNow Unit Testing Tools**


**Jasmine:** It is an open-source testing framework utilized within ServiceNow for unit testing. It is written in JavaScript and offers a straightforward syntax for outlining tests and assertions. It is frequently used in conjunction with other JavaScript testing tools and frameworks and can be employed for both front-end and back-end testing.


The unit test cases are housed in the “ **Client Test Scripts** ” section of the **ServiceNow Test Management** module. You can access this section by navigating to “Test Management” -> “Test Suites” -> “Client Test Scripts”. From there, you can create a new test script, which lets you write your test cases using the Jasmine framework.


Below is a sample code utilizing Jasmine.


```text
describe('My ServiceNow Application', function() {


var myApp = new MyApp();


describe('Functionality Tests', function() {
it('should define the application', function() {
expect(myApp).toBeDefined();
});


it('should have a function to create a new record', function() {
expect(myApp.createNewRecord).toBeDefined();
});


it('should have a function to update an existing record', function() {
expect(myApp.updateRecord).toBeDefined();
});
});
});
```


## ServiceNow Integration Testing


This form of testing is typically done after unit testing and before[end-to-end testing](https://testrigor.com/blog/how-to-do-end-to-end-testing-with-testrigor/) to ensure that individual components operate correctly when integrated as a system. This integration is crucial in ServiceNow as you want to ensure your system’s existing modules work smoothly with your chosen ServiceNow modules.


You will find recommendations for ServiceNow testing tools, including ATF and IntegrationHub for integration testing.


#### **ServiceNow Integration Testing Tools**


**ServiceNow’s Automated Test Framework(ATF):** The Automated Test Framework (ATF) is ServiceNow’s testing framework. Built directly into the platform, it is well-equipped for testing all ServiceNow application aspects and supports various testing types. With ATF, users can create and execute automated tests that simulate interactions between ServiceNow and other systems via web services or APIs.


Consider the following integration test script designed to log an incident in a ServiceNow application.


```text
var testSuite = 'My Integration Tests';
var testCaseName = 'My Test Case';
var testCase = new sn_atf.Testcase();


testCase.initialize(testCaseName, testSuite);


testCase.addStep(new sn_atf.Step('Log in to ServiceNow', function() {
sn_atf.utils.login('admin', 'password');
}));


testCase.addStep(new sn_atf.Step('Create a new incident', function() {
var gr = new GlideRecord('incident');
gr.initialize();
gr.short_description = 'Test Incident';
gr.description = 'This is a test incident created by ATF';
gr.insert();
assert.ok(gr.getUniqueValue(), 'Incident record created successfully');
}));


testCase.run();
```


## ServiceNow End-to-End Testing


You might want to do end-to-end testing with ServiceNow for many reasons. Maybe the ServiceNow modules you integrated with have been updated, or you want to check if the integration is working correctly in the first place, or if you’ve created a custom app, you might want to ensure its behavior.


Through[end-to-end testing](https://testrigor.com/blog/integration-testing-vs-end-to-end-testing/) , you can test an entire application from start to finish, including all components and integrations, to ensure that the application works as intended. Test scenarios created are run in real-world-like simulations, which may involve interactions with external services or databases.


Since end-to-end testing is done through the UI or web browser, you needn’t worry too much about underlying code or the application’s code restricting you. This is provided; you opt for tools that are agnostic to such details.


Here are some test automation tool options for end-to-end testing.


### ServiceNow E2E Testing Tools


#### **Selenium**


Selenium is a popular web automation tool for end-to-end application testing. It requires you to code test cases in any programming language like Java, Python, Ruby, etc.


Here’s a sample test case created in Selenium.


```text
WebDriver driver = new ChromeDriver();


driver.get("https://<yourinstance>.service-now.com");


WebElement username = driver.findElement(By.id("user_name"));
WebElement password = driver.findElement(By.id("user_password"));


username.sendKeys("your_username");
password.sendKeys("your_password");


driver.findElement(By.id("sysverb_login")).click();
driver.get("https://<yourinstance>.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D-1");


WebElement shortDescription = driver.findElement(By.id("incident.short_description"));
WebElement description = driver.findElement(By.id("incident.description"));


shortDescription.sendKeys("Test Incident");


description.sendKeys("This is a test incident created via Selenium");


driver.findElement(By.id("sysverb_insert")).click();


WebElement incidentNumber = driver.findElement(By.className("outputmsg_text"));


assert incidentNumber.getText().contains("INC");


driver.quit();
```


Selenium depends heavily on the XPath or the IDs of the elements. So, any change in the DOM element property causes the test case to fail. Also, the code complexity increases when more scenarios are created in Selenium, making code debugging difficult. So, with all these[disadvantages](https://testrigor.com/blog/why-selenium-sucks-for-end-to-end-testing/) , most organizations prefer other ServiceNow automated testing tools.


#### **testRigor**


If you want to focus on creating quality test cases rather than worrying about coding them, then[testRigor](https://testrigor.com/blog/how-to-do-end-to-end-testing-with-testrigor/) is your pick. You can even find the[testRigor app](https://store.servicenow.com/sn_appstore_store.do#!/store/application/1b7086e887b76550ad4ea8683cbb3571/1.0.1?referer=%2Fstore%2Fsearch%3Flistingtype%3Dallintegrations%25253Bancillary_app%25253Bcertified_apps%25253Bcontent%25253Bindustry_solution%25253Boem%25253Butility%25253Btemplate%25253Bgenerative_ai%25253Bsnow_solution%26q%3DtestRigor&sl=sh) in the ServiceNow Store as an official app for automation testing.


This revolutionary[AI agent](https://testrigor.com/ai-agents-in-software-testing/) understands plain English statements and conducts tests from an end-user’s perspective. This innovative approach to automation allows your organization to achieve multiple key objectives simultaneously.


- **Plain English tests:** Enable your existing employees to build test automation, regardless of their technical proficiency. As testRigor is a[generative AI](https://testrigor.com/generative-ai-in-software-testing/) -driven, no-code solution, setting up the environment and building tests becomes more seamless and efficient than traditional testing tools. Here is an[All-Inclusive Guide to Test Case Creation in testRigor](https://testrigor.com/blog/all-inclusive-guide-to-test-case-creation-in-testrigor/) .
- **Ultra-stable test scripts** : Ensure your automation system is robust enough to withstand the release of new versions. This is very important for ServiceNow as they constantly update and release newer versions. Unlike other systems, testRigor does not rely on the detailed specifics of the implementation such as CSS/XPath locators. This results in unparalleled stability, saving countless hours each week.
- **Cross-platform testing** : Create a collaborative platform for[cross-platform testing](https://testrigor.com/blog/cross-platform-testing-web-and-mobile-in-one-test/) , including mobile app testing, which can be useful for testing ServiceNow mobile integrations.
- **Minimum maintenance** : Negligible test maintenance is possible with this tool, thanks to its use of AI. Whenever there are UI and requirement changes, testRigor has the capability to adapt the test scripts accordingly, making the[maintenance effort near-zero](https://testrigor.com/maintenance/) .
- **Powerful integrations** : testRigor offers[built-in integrations](https://testrigor.com/integrations/) with popular CI/CD tools like Jenkins and CircleCI, test management systems like Zephyr and TestRail, defect tracking solutions like Jira and Pivotal Tracker, infrastructure providers like AWS and Azure, and communication tools like Slack and Microsoft Teams. You can import your manual test cases from test management tools or copy-paste them in testRigor, and with small tweaks you are ready with test automation. Here are the guides for[TestRail](https://testrigor.com/testrail/) ,[Zephyr](https://testrigor.com/zephyr/) ,[ALM](https://testrigor.com/alm/) ,[PractiTest](https://testrigor.com/practitest/) .
- **Accessibility and Exploratory Testing** : You can efficiently perform[accessibility testing](https://testrigor.com/accessibility/) through testRigor. Read here[how to build an ADA-compliant app](https://testrigor.com/blog/how-to-build-an-ada-compliant-app/) . This intelligent tool helps you run exploratory testing with its AI-powered features. Read how to perform[exploratory testing with testRigor](https://testrigor.com/automate-exploratory-testing/) .
- **AI Features Testing:** Using testRigor, you can test chatbots (LLMs), and[AI features](https://testrigor.com/blog/how-to-automate-testing-of-ai-features/) such as negative/positive intentions, true/false statements, sensitive information, etc.


Creating a free account with testRigor is effortless as it is a cloud-based platform. You can also avail of the enterprise versions based on your needs, as these versions tend to include some features that aren’t available in the free version.


### How to use testRigor for ServiceNow Testing?


As a ServiceNow QA tester, its very straightforward and easy to test applications using testRigor. We will go through the step-by-step procedure for the same. Let’s get started.


**Step 1** : Log in to your testRigor app with your credentials.


**Step 2** : Set up the test suite for the website testing by providing the information below:


- **Test Suite Name:** Provide a relevant and self-explanatory name.
- **Type of testing:** Select from the following options: **Desktop Web Testing** , **Mobile Web Testing** , **Native and Hybrid Mobile** , based on your test requirements.
- **URL to run test on:** Provide the ServiceNow application URL you want to test.
- **Testing credentials for your web/mobile app to test functionality which requires user to login:** You can provide the app’s user login credentials here and need not write them separately in the test steps then. The login functionality will be taken care of automatically using the keyword ***login*** . Filling out this field is optional and not mandatory.
- **OS and Browser:** Choose the OS Browser combination on which you want to run the test cases.
- **Number of test cases to generate using AI:** You can simplify your test creation further by opting to generate test cases based on the App Description text. This feature works on[generative AI](https://testrigor.com/generative-ai-in-software-testing/) .
- **App Description** : Provide the description of the Application you are testing.


**Step 3** : Click on **Create Test Suite.**


**Step 4** : To create a new custom test case, click **Add Custom Test Case** .


**Step 5** : Next step is to provide the test case **Description** and add the test steps as below:


**Step 6** : As mentioned in Step 2, the **‘login’** keyword will automatically log you into the application using the credentials provided during the test suite creation.


**Step 7** : In this end-to-end test case, we plan to save employee data. Since this is a common and repetitive test step, we can create a plain English ‘ **Reusable Rule** ‘ in testRigor for it, similar to a subroutine in programming. Once the rule is created, we can use the rule name in any test case.


We have created a reusable rule, ‘ **save employee detail** ‘, as shown below. Read:[How to create Reusable Rules](https://testrigor.com/how-to-articles/how-to-use-reusable-rules-or-subroutines-in-testrigor/) .


**Step 8:** Now, let’s use the name of the reusable rule, ‘ **save employee detail** ,’ in our test case to execute all the associated test steps. If any application or requirement changes occur, you can update the reusable rule accordingly, and the changes will automatically reflect in all test cases that utilize it.


```text
login
save employee detail
```


In a nutshell, the ‘ **save employee detail** ‘ test steps are working as:


**Step 8.1:** click “ALL EMPLOYEES”


**Step 8.2** : The employee name (Bow Ruggeri) is saved as a stored value (variable): **Employee Name.** Read:[How to use variables in testRigor?](https://testrigor.com/how-to-articles/how-to-use-variables-in-testrigor/) Here is the testRigor command to click using the variable’s value.


```text
click on the stored value "Employee Name"
```


**Step 8.3** : click “Save Contact”


**Step 8.4** : You can assert a check using plain English command as follows and you may read:[How to perform assertions using testRigor?](https://testrigor.com/how-to-articles/how-to-perform-assertions-using-testrigor/) to know more.


check that page contains “Contact saved You can find this contact quickly in your saved list.”


**Step 8.5** : click “Back to previous”


**Step 8.6** : click “SAVED”


**Step 8.7** : check that page contains stored value “Employee Name”


**Step 9** : Now, we have employee data saved using the reusable rule ‘ **save employee detail** ‘. Similarly, you can add the remaining test steps to save the employee data. The complete test case will look like this:


Click ‘ **Add and Run** ‘ to start the test execution.


Here is a step-by-step guide to help you perform[end-to-end testing using testRigor](https://testrigor.com/how-to-articles/how-to-do-end-to-end-testing-with-testrigor/) .


## ServiceNow Testing Scenarios


As a ServiceNow tester, you would like to cover these scenarios:


- **Incident Management Testing:** Validate the lifecycle of incidents from creation to closure.
- **Change Management Testing:** Ensure workflows for change requests, approvals, and rollbacks work as expected.
- **Problem Management Testing:** Test the creation, root cause analysis, and resolution of problems.
- **Service Catalog Testing:** Verify the request, approval, and fulfillment process for catalog items.
- **Role-Based Access Testing:** Test role-based access control for data and functionality.
- **Data Migration Testing:** Test data migration accuracy and consistency between systems.
- **CMDB (Configuration Management Database) Testing:** Validate accuracy, updates, and impact analysis of configuration items (CIs).


## Conclusion


The core of ServiceNow’s appeal lies in its highly flexible platform, which supports extensive customization and scalability to meet the needs of large and complex organizations. Its ability to automate workflows across various departments, not just IT, helps break down silos within organizations, facilitating a more integrated and efficient approach to service management.


A robust platform like ServiceNow needs vital QA processes to ensure the best services to your customers. You can guarantee that if you balance out ServiceNow manual and automated testing as and when required by employing powerful and intelligent tools for the same.


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
