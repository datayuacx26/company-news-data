---
schema_version: "1.0.0"
document_id: "c4332ee03573e21cd56d3d2641eea75d4fdff557c270d39722e43410be37f536"
company_key: "yc-sapling-ai"
company: "Sapling.ai"
source_id: "yc-sapling-ai-rss-3361843b7812"
canonical_url: "https://blog.sapling.ai/javascript-spelling-and-grammar-checkers/"
published_at: "2022-07-22T07:32:53+00:00"
first_seen_at: "2026-07-25T22:15:25.567776+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:ce968c25230b123907c6ac4530dd830b6fdce399df7d9dbe60748773c1405eec"
---

# JavaScript Spelling and Grammar Checkers

This article describes how to use Sapling as a JavaScript spelling and grammar checker, and presents a couple other commercial or open-source JavaScript grammar checker alternatives. Different libraries may be better suited for your needs depending on project requirements and budget. For each option, you can walk through detailed installation instructions as well as a quickstart piece of code for each library and a demonstration of how to use it. Comparisons are grouped by whether your use case is web/frontend based or backend based.


### Sapling


Sapling is a cloud-based service that checks grammar with deep neural nets that have been trained on millions of English sentences. It uses contextual cues of sentences to suggest edits instead of human written rules. Additionally, Sapling supports full spelling and grammar checking for more than[10 different languages and spell checking for more than 30 different languages](https://sapling.ai/multilingual) .


For enterprise use cases, Sapling is HIPAA compliant, SOC2 compliant, and offers options for no-data retention or on-premise/self-hosted models so that processed text data can stay in a geographical region or your compute environment for regulatory purposes.


You can register an account and get a rate-limited api key for testing or personal use for free:[https://sapling.ai/docs/api/api-access](https://sapling.ai/docs/api/api-access)


Contents


Toggle


##


— Frontend JavaScript Libraries —


JavaScript is used in almost all modern browsers and websites to enable specific functionality and interactivity. It is paired as the most popular scripting language along with CSS and HTML that specify what things to render to a screen. Using JavaScript in a web front-end typically involves pulling a user’s text to check from HTML textarea elements and div container elements marked as content editable. As more tools and apps are being offered in the web browser, using JavaScript to mark up spelling and grammar errors for text and writing applications is becoming more common.


##


Sapling (Frontend)


Sapling has an open source npm installable JavaScript library for node. It is compatible with popular JavaScript frameworks like React, Angular, Vue, Ember, Svelte and compatible with TypeScript, a programming language that transpiles into JavaScript. For frontend usage, it can also be imported directly in an HTML webpage through a script tag.


### Installing Sapling for node/npm


- Register for an account at[Sapling.ai](https://sapling.ai/user/register) .
- After registering and signing in, generate a development API key in your[dashboard](https://sapling.ai/user_settings) .
- Install the Sapling SDK:


```text
npm install @saplingai/sapling-js
```


If you don’t have node or npm, you can install it from here:[https://nodejs.org/en/download/](https://nodejs.org/en/download/)


### Sapling usage in node/npm


```text
import { Sapling } from "@saplingai/sapling-js/observer";


Sapling.init({
key: '<YOUR_API_KEY>',
endpointHostname: 'https://api.sapling.ai',
editPathname: '/api/v1/edits',
statusBadge: true,
mode: 'dev',
});


const editor = document.getElementById('editor');
Sapling.observe(editor);
```


### Sapling usage in HTML page


Sapling’s JavaScript SDK can be imported directly as a script in an html webpage. See here for more details:[https://sapling.ai/docs/sdk/HTML/quickstart](https://sapling.ai/docs/sdk/HTML/quickstart)


```text
<script src="https://sapling.ai/static/js/sapling-sdk-v1.0.2.min.js"></script>


<div contenteditable="true" id="check-space" sapling-ignore="true">Lets get started!</div>


<script type="text/javascript">
Sapling.init({
key: '<API_KEY>',
endpointHostname: 'https://api.sapling.ai',
editPathname: '/api/v1/edits',
statusBadge: true,
mode: 'dev',
});


const contentEditable = document.getElementById('check-space');
Sapling.observe(contentEditable);
</script>
```


See here for more details around frontend usage:[https://sapling.ai/docs/sdk/JavaScript/frontend_documentation](https://sapling.ai/docs/sdk/JavaScript/frontend_documentation)


Sample React app:[https://github.com/saplingai/sapling-samples/tree/master/javascript/react-app](https://github.com/saplingai/sapling-samples/tree/master/javascript/react-app)


Sample Angular app:[https://github.com/saplingai/sapling-samples/tree/master/javascript/angular-app](https://github.com/saplingai/sapling-samples/tree/master/javascript/angular-app)


##


Perfect Tense (Frontend)


Perfect Tense is an AI based commercial grammar checking service. It can be used as an HTTP API or script tag imported JavaScript library directly into a web page. The product is marketed as useful as a medical spell checker and is HIPAA compliant. The product is not self-serve and there is no API documentation publicly available. You can reach out here to get started:[https://www.perfecttense.com/api](https://www.perfecttense.com/api)


##


Nanospell (Frontend)


[Nanospell](https://www.javascriptspellcheck.com/) is a spell checker that supports 20 languages and is available as TinyMCE and CKEditor plugins. There is a free version of the software that periodically has a “free version” popup. A paid version removes this popup with a one time license that can be purchased ranging from $299 to $1499 depending on whether or not the library will be distributed, or used on a server.


### Nanospell Usage


```text
<script type='text/javascript' src='/JavaScriptSpellCheck/include.js' ></script>
<script type='text/javascript'>$Spelling.SpellCheckAsYouType('myTextArea')</script>
<textarea name="myTextArea" id="myTextArea" cols="30" rows="4">
This this is a simple exampl of Spell-checking As-You-Type using javascript spellcheck.
</textarea>
<input type="button" value="Spell Check in a Dialog" onclick="$Spelling.SpellCheckInWindow('myTextArea')" />
```


##


Grammarly (Frontend)


Grammarly is one of the most popular individual grammar checkers out there, with popular integrations for browsers and mobile keyboards. With the integrations, users can get direct spelling and grammar checking. They have a closed beta that needs to be applied to for their SDKs that support React, Vue and web editors. Documentation for the SDK is public, but access to it is restricted. Grammarly’s SDK is HTML DOM based and does not work without a webpage. You can apply for the closed beta here:[https://developer.grammarly.com/](https://developer.grammarly.com/)


##


— Backend JavaScript Libraries —


JavaScript is also used in back-end servers, with the most popular runtime environment being Node.js. Other environments like RingoJS and Deno also support the usage of JavaScript for server-side scripting. Backend grammar checking involves directly passing in text and getting edit suggestions in response through a JavaScript data structure, without any interaction with HTML DOM elements or a user interface. This is the preferred way to batch process large amounts of text or multiple files at the same time.


##


Sapling (Backend)


### Installing Sapling for node/npm


- Register for an account at[Sapling.ai](https://sapling.ai/user/register) .
- Install the Sapling SDK:


```text
npm install @saplingai/sapling-js
```


### Sapling Usage in node


```text
import { Client } from "@saplingai/sapling-js/client";


const apiKey = '<YOUR_API_KEY>';
const client = new Client(apiKey);
client
.edits('Lets get started!')
.then(function (response) {
console.log(response.data);
})


```


##


LanguageTool (Backend)


[LanguageTool](https://languagetool.org/http-api/#/default) is a rule-based LGPL Java grammar checker. If you are distributing commercial software with LanguageTool, make sure that the language tool library is linked separately and end users have the ability to load in their own.


You can use LanguageTool either through an HTTP API calling a backend hosted by the LanguageTool company, or you can run the grammar checker backend yourself and call with through Python bindings. The LanguageTool hosted backend API costs money for commercial use and scales with usage. There is a free/trial version that you can try with restrictions around usage and limits corrections to 30 misspelled words.


There are several JavaScript bindings for LanguageTool,[gramma](https://www.npmjs.com/package/gramma) being one of the more popular ones.


### Installing LanguageTool


This step is optional and is only required if you want to keep all text processing local or avoid paying for LanguageTool’s hosted backend.


1. Download LanguageTool:[https://languagetool.org/download/LanguageTool-stable.zip](https://languagetool.org/download/LanguageTool-stable.zip)
2. Install Java[https://www.java.com/en/download/help/download_options.html](https://www.java.com/en/download/help/download_options.html)
3. Launch the server:


```text
java -cp languagetool-server.jar org.languagetool.server.HTTPServer --port 8081 --allow-origin
```


### Installing gramma JavaScript Client


```text
npm i gramma -g
```


### Using LanguageTool / gramma


```text
const gramma = require("gramma")


gramma
.check("Some text to check.", {
api_url: "http://localhost:8081/v2/check",
api_key: "SOME_API_KEY",
language: "en-US",
})
.then(console.log)
```


##


Hunspell (Backend)


[Hunspell](https://github.com/hunspell/hunspell) is a popular open source spell checker that supports a variety of different languages. Based on an older spellchecker called MySpell, the name comes from originally being used to check Hungarian spelling. It is the default spell checker integrated into web browsers like Chrome and Firefox, so you very likely have used it before. If you are looking for an open source spell checking library that is very widely used and actively maintained, Hunspell is a good choice.


Hunspell is a C++ library, but bindings for JavaScript can be installed through npm. Hunspell is licensed under 3 separate licenses: GPL/LGPL/MPL. The availability of the MPL license makes this easier to integrate into commercial products compared to GPL or LGPL only licensed software. Just make sure to open-source any changes made to the core library.


Nodehun is a JavaScript interface for Hunspell, and is released under the MIT license. It packages Hunspell directly, so there is nothing else that needs to be installed.


### Installing Nodehun


```text
npm install nodehun
```


##


Using Hunspell / Nodehun


```text
const { Nodehun } = require("nodehun");
const fs          = require('fs')


const affix       = fs.readFileSync('./node_modules/nodehun/examples/dictionaries/en_US.aff')
const dictionary  = fs.readFileSync('./node_modules/nodehun/examples/dictionaries/en_US.dic')


const nodehun     = new Nodehun(affix, dictionary)


nodehun.spell('color').then(correct => { console.log(correct) }) //  true
nodehun.spell('clor').then(correct => { console.log(correct) }) // false
```


##


Building Your Own


Grammar checkers can be incredibly complicated to build, train and maintain. On the other hand, building a spell checker in JavaScript that takes text and suggests spelling corrections for words can be done in less than 100 lines of code. Existing open source spell checkers can also easily be extended. The developer community does not produce many libraries around doing machine learning in JavaScript, in part because the language is optimized towards web usage and portability instead of performance. It would be much easier to build a system like this in another scripting language like[Python](https://blog.sapling.ai/?p=562) .


### **Building a Spelling Checker**


The simplest algorithms look up individual words against a word dictionary. Words that are not in the dictionary are marked as incorrect, and suggested alternatives are computed based on edit distance (the number of characters that need to change) compared to a dictionary word. Words that are lower in edit distance are more similar, and presumed to be more likely to be the intended word.


Peter Norvig, a prominent AI computer scientist, has a blog post detailing how to build a spelling corrector, with an example in Python:[https://norvig.com/spell-correct.html](https://norvig.com/spell-correct.html)


The example has been ported into JavaScript by various authors:


- [https://blog.astithas.com/2009/08/spell-checking-in-javascript.html](https://blog.astithas.com/2009/08/spell-checking-in-javascript.html) (53 lines)
- [https://github.com/past/speller/tree/master](https://github.com/past/speller/tree/master) (72 lines)
- [https://stoi.wordpress.com/2012/12/31/jspell/](https://stoi.wordpress.com/2012/12/31/jspell/) (92 lines)


### **Building a Grammar Checker**


Grammar checkers require either maintaining a database of rules for matching against or enough data to train an effective machine-learning language model. Nowadays the most effective models are based off of neural nets, but statistical models can also be trained. Both the training and maintenance of your own grammar checker can be expensive. This route is preferable only if you are interested in developing you or your team’s expertise in natural language processing and are willing to invest the time.


##


Conclusion


The best JavaScript spelling and grammar checker will depend on your use case. Other existing spelling and grammar checking APIs were not included in this overview if they did not provide JavaScript support. You can visit[this](https://blog.sapling.ai/javascript-spelling-and-grammar-checkers)[page](https://blog.sapling.ai/python-spelling-and-grammar-checkers/) for a comparison of Python spelling and grammar checkers.


Library Javascript SDK Pros Cons


SaplingFrontend +Backend – Self-serve option
– Multiple language support – Costs money


Perfect Tense Frontend – Self-serve option – Costs money


NanoSpell Frontend – No grammar checking


Grammarly Frontend – Wide adoption – Costs money


LanguageTool Backend – Self-serve option
– Multiple language support – Costs money or hosting resources


Hunspell Backend – Self-serve option
– Multiple language support
– Wide adoption – No grammar checking


Build your own Frontend + Backend – ML expertise as a competitive advantage – Engineering cost of training and maintenance
