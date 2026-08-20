---
schema_version: "1.0.0"
document_id: "f722eb91bed1236ab6c05928a3d3c44cb4e12c52de6981e1c6f90385ade7a2f3"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2021-04-19-designingmarketingemailsviapredefinedmod/"
published_at: "2021-04-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:cdd88d46d406d70e34f023492508fb6514fbe633f5a363c3447c87fc55f9941d"
---

# Designing Marketing Emails via predefined Modules in Salesforce Email Studio

Designing Marketing Emails via predefined Modules in Salesforce Email Studio


When techies hear about email marketing and designing HTML emails, they typically roll their eyes and think of a very boring field of work. In this article, we hopefully can explain to you why this is wrong from our perspective. Our team built a solution that our marketers use to easily design HTML emails with predefined modules within our email marketing platform[Salesforce Email Studio](https://www.salesforce.com/products/marketing-cloud/email-marketing/?d=www.exacttarget.com/) , which is part of the[Salesforce Marketing Cloud](https://www.salesforce.com/products/marketing-cloud/) (SFMC). This article gives you an overview of our approach as well as why and how we built such predefined modules for Email Studio on our own.


## Automating Repetitive Work


From a technical perspective, designing HTML emails seems to be tedious - and for most techies this is probably true. Even in 2021, the main technical challenge when building HTML emails is still fighting the technical limitations of the different email applications and devices with varying screen sizes. Email applications such as Microsoft Outlook, Mozilla Thunderbird, or Apple Mail still only render old-fashioned HTML 4.01 properly, so the designs have to be based on HTML tables, inline CSS, and other techniques that frontend developers nowadays consider as layout hacks. For more details about the technical challenges, we refer, for example, to[Top 5 HTML Email Development Challenges](https://medium.com/@info.mailerstock/top-5-html-email-development-challenges-f94bb5fd421c) or[An Introduction To Building And Sending HTML Email For Web Developers](https://tech.trivago.com/post/%5Bhttps://www.smashingmagazine.com/2017/01/introduction-building-sending-html-email-for-web-developers/%5D(https://www.smashingmagazine.com/2017/01/introduction-building-sending-html-email-for-web-developers/)) .


Besides that, the real challenge is a good conceptual solution which has to fit the business requirements of your marketers while still being able to scale in many different dimensions. At trivago, for example, we send out different personalized emails in 30+ languages to several millions of recipients with a comparably small individual overhead.


In email marketing, you are typically confronted with different types of campaigns and single send-outs your marketers would like to do. Some are highly repetitive and some are more ad hoc. For example, highly repetitive send-outs at trivago are our retargeting emails based on user actions on the trivago hotel search. In the background, we set up highly automated workflows to deliver high numbers of personalized emails to our customers. Such highly repetitive emails are typically HTML email templates once developed and rarely changed. When it comes to automating repetitive work, automating such highly repetitive send-outs should be your first priority.


However, our marketers also run campaigns and send-outs on an ad hoc basis such as for special occasions like public holidays or for advertising new features of our products. Such emails often have to be designed individually (see also: figure above). Supporting your marketers properly in creating those emails is much more challenging though. The solution that we present in this article is mostly concerned with ad hoc up to semi-automated emails.


## Modularizing Email Content


In order to better automate ad hoc emails and their send-outs, the normal approach is to come up with a module-based email design. Designing HTML emails based on predefined modules is not new to the world. For designing HTML emails, there are diverse tools, desktop clients, and ready-to-be-used SaaS solutions. All solutions typically have possibilities to build reusable modules. Also, our email marketing platform SFMC, respectively Salesforce Email Studio, supports that - later in this article, we show you how we implemented our solution in SFMC. Furthermore, there are a lot of providers which create email templates for you on a per-request basis. We also make use of such providers for one time send-outs or campaigns where we do not have predefined modules or do not want to create further predefined modules. This is a classical make-or-buy decision based on our framework presented in the previous sections.


Building modules for a module-based design approach requires you to think about what modules you should build and how you prioritize your efforts. We typically start to think about the re-usability and complexity of the email content and its design elements. Here, the complexity is mostly driven by the complexity of the HTML and inline CSS as well as responsiveness of the design elements.


When you look at typical (marketing) emails, you can see that they follow some common structure (see, e.g.:[Understanding Email Layout and Structure](https://chamaileon.io/resources/understanding-email-layout-and-structure/) ): a header, a content section, and a footer. While the header is not always used, the content section and the footer are essential. The content section contains the actual content, and the footer typically contains obligatory legal information which has to be in emails.


While the header and footer can be considered as static, the content section requires more attention. The content section is often composed of simple elements such as buttons, images, text blocks, etc. (i.e., individual design elements). Although you can build all emails out of such individual design elements, you should consider building more complex reusable modules. You have to build a responsive design for all the different email applications and devices anyway, and considering the positioning per module is often much better than always reinventing the wheel for each email design.


When trivago does email marketing, we typically advertise deals for hotels, or for what we call alternative accommodations such as apartments or vacation rentals (see, e.g.:[trivago announces integration of HomeAway into its hotel search platform](https://ir.trivago.com/news-releases/news-release-details/trivago-announces-integration-homeaway-its-hotel-search-platform) ). As you can probably imagine, these deals are dynamic content compared to a quite static footer. With every send-out, the deals change.


However, our email recipients always care about the same key information: This includes details about the accommodation such as a picture, the name, the location, the price per night, etc. So, we can easily build a reusable module out of similar deals that we typically promote. A very common deals element is a predefined module with two such deals next to each other, our so-called Two Deals module (see: figure above). For every send-out, we just have to adapt the hotel and alternative accommodation data or get it dynamically via an API.


We have many more predefined modules. A few of them are depicted in the figure above based on their complexity and re-usability in different emails. In combination with individual design elements (buttons, images, and text blocks), our marketers can build emails individually while still producing proven HTML that looks equally good in all email applications.


## Integrating our Predefined Modules in Salesforce Email Studio


As mentioned, we are using SFMC and Salesforce Email Studio for our email marketing. Besides other tools, SFMC also provides a way to build and use predefined modules when designing HTML emails. For that, SFMC has the drag & drop editor[Content Builder](https://help.salesforce.com/articleView?id=mc_ceb_content_builder.htm&type=5) . Salesforce refers to Content Builder as a library of content for your SFMC account. In doing so, content created via the Content Builder as predefined modules - Salesforce calls them content blocks - can be used all over your organization for different channels such as in emails in Salesforce Email Studio or in similar channels such as for SMS, MMS, or push notifications.


In order to integrate with the Content Builder, we used an SFMC framework for the[Content Builder](https://help.salesforce.com/articleView?id=mc_ceb_content_builder.htm&type=5) , the[Content Builder Block Standard Development Kit (SDK)](https://trailhead.salesforce.com/en/content/learn/modules/content-builder-block-sdk/meet-the-content-builder-block-sdk) . The modules or so-called content blocks that you build have to be wrapped by specific functions of the Content Builder Block SDK via a “plugin mechanism”. In doing so, the content blocks are draggable & droppable into the Content Builder Editor’s drawing area (see also:[Content Builder Block SDK Description](https://trailhead.salesforce.com/en/content/learn/modules/content-builder-block-sdk/meet-the-content-builder-block-sdk) ).


When you drag and drop the content block into the drawing area of the Content Builder Editor, you are able to select the content block and get a configuration panel on the left side of the Content Builder Editor. Via that configuration panel, you can make the content block dynamic and interactive for your marketers when designing the HTML emails.


## A Small Example


To be able to integrate custom content blocks with the Content Builder, you have to at least provide an HTML page making use of the Content Builder Block SDK in a JavaScript block (see also:[installation of the Content Builder Block SDK](https://github.com/salesforce-marketingcloud/blocksdk#installation) ). Within the JavaScript block, you have to define the callback functions for the plugin mechanism. The callback functions are invoked by the Content Builder Editor when pressing buttons or dragging and dropping the content block into the drawing area. The HTML page URL needs to be integrated into your SFMC account as an App/Installed Package. Afterwards, you can make use of the content block in Content Builder Editor.


At trivago, we preferred to build a Vue.js application instead of exposing a simple HTML page per content block. Building a Vue.js application we were able to ease our development by reusing a couple of boilerplate code. The Vue.js application exposes Vue.js components via routes per content block. The Vue.js components are essentially the different content blocks. The application is hosted in our data center. An example content block as a Vue.js component is shown in the code listing below.


```text
<  template  >
<!-- This is the HTML for the configuration panel -->
<  div   id  =  "workspace"   class  =  "slds-card"  >
<  h2  >Content Block Example</  h2  >
<  input   id  =  "text-input"   type  =  "text"   v-model  =  "imageUrl"   @change="getContent"   @input="getContent"   value  =  ""   />
</  div  >
</  template  >


<  script  >
const BlockSDK: require("blocksdk");


const sdk: new BlockSDK(
["blocktester.herokuapp.com", "localhost", "marketingcloudapps.com"],
true
);


export default {
name:   "ContentBlockExample"  ,
data  () {
return {
content  :   ""  ,
imageUrl  :   "https://imgma.trivago.com/image/upload/v1562688258/..."  ,
};
},
methods: {
generateHtml  () {
/**
* Generates HTML with the dynamic content in it.
* The HTML is a paragraph containing the value of
* the text input as text and as an image.
*/
return   `<p>${  this  .  imageUrl  }<br><img src="${  this  .  imageUrl  }" alt="This is an example image"></p>`  ;
},
getContent  () {
/**
* Generates the content block and sets the data on users input
*/
sdk.  setData  ({
content:   ""  ,
imageUrl:   this  .imageUrl,
});
sdk.  setContent  (  this  .  generateHtml  ());
},
},
};


</  script  >
```


The first part of the Vue.js component is the HTML for the configuration panel of the content block that appears on the left side of the Content Builder Editor when clicking on the content block in the drawing area. The code is wrapped by the` template` tags. In the configuration panel, there is a text input for an image URL (` id="text-input"` ; it is referenced via` imageUrl` in the rest of the content block code). When changes to the value of the text input happen, the function` getContent()` is invoked. This function is part of the JavaScript block surrounded by the` script` tags.


In the script block, the Content Builder Block SDK is initialized first. The further parts are wrapped by an ECMAScript block (` export default` ). First, the data object which will be used to exchange data between the different functions and the Content Builder Editor is initialized. Next, the methods implementing the content block’s functionality are defined.


Most essential for the content block’s functionality is the` getContent()` function. The` getContent()` function first uses the SDK’s setData function which retrieves the current value from the configuration panel’s text input and writes it to the data object. Afterwards, the SDK’s` setContent({...})` function is invoked to write the updated email HTML to render the content block in the Content Builder Editor’s drawing area newly with updated values. We moved the HTML content generation to the` generateHtml()` function which is directly putting the HTML into the` setContent({...})` function.


Although the given example does not contain full functionality that we have in our content blocks such as our Two Deals module, it shows the way it works. The Content Builder Block SDK provides further functions you can use. For more information, we refer to the documentation. Making use of the Content Builder Block SDK, we built predefined modules for our marketers to design proper HTML ad hoc emails easily via drag & drop in the Salesforce Content Builder Editor.
