---
schema_version: "1.0.0"
document_id: "83da6b8d6c0abcf34cb118db0fb6fbc6b7433acc0d49423a3fe42800f8c9baff"
company_key: "yc-veryfi-inc"
company: "Veryfi, Inc."
source_id: "yc-veryfi-inc-news-import-dba8fa8c07ed"
canonical_url: "https://www.veryfi.com/engineering/flutter-wrapper-for-lens/"
published_at: "2022-11-23T05:40:01+00:00"
first_seen_at: "2026-07-24T06:07:32.032518+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:a67c2ed61be3894bb6971b04a7d320f64a87dbfaaba6d2b0f442bbee23b2e429"
---

# Flutter Integration with Veryfi Lens for Your Mobile App

## Get Structured Data in Seconds


Magic, who doesn’t like a bit of magic? As a child, magic was always in the back of my mind — the fantasy of something grander. One could bring stories to life using just our thoughts and a sprinkle of pixie dust. The power of injecting zest into something that I yearned for. Wouldn’t it be cool to have magical powers — of course, it would.


As we travel through life and mature into adulthood, some forget the power of magic and magical thinking. But that power is still lurking and waiting to be released through creative and contrarian thinking. When we match this with newly acquired superpowers like software development, you become an unstoppable magician that can bring any idea to life using code.


> “Time is Galleons, little brother,”
>
>
> *– Fred Weasley, Harry Potter and the Order of the Phoenix, Chapter 4*


In the Order of the Phoenix book, after Harry’s arrival at the headquarters of the Order, he meets the Weasley twins, who have just passed the Apparition test. The twins used this apparition method to get from one part of the house to a different one. When they appear in Ron and Harry’s room, their scared younger brother shows his annoyance: “It would have taken you about thirty seconds longer to walk down the stairs.” said Ron. “Time is Galleons, little brother,” said Fred.


The source of this proverb *“Time is Galleons”* can be found in the muggle proverb “Time is Money”. Time is precious, just like money, and should not be wasted.


## Bookkeeping… Yikes!


Bookkeeping is a time-consuming and somewhat tedious task of collection, labeling/categorization, extraction of data into an accounting suite, and matching transactions. If you specialize in something other than accounting, then bookkeeping is a hair-puller of an experience. Yet we all have to do it for statutory tax obligations. Your time is money. Say Hello to Veryfi and melt those pains away and get your bookkeeping done in seconds using day-1-ready software that automates your bookkeeping tasks.


## Veryfi Lens


One of those tools in the Veryfi toolbox is a product called Veryfi Lens for financial document capture. It is a framework (precompiled code) to capture and pre-process bills, receipts, invoices, w2s, credit cards et al.. in seconds so you can focus on your core product.


Veryfi Lens works on both iOS and Android and comes with a wealth of plugins like Flutter (a open-source mobile UI software development kit).


## Magical Tools Just For You


The Veryfi Flutter plugin takes advantage of all the magical stuff Flutter does in the background to create a native mobile application with only one codebase. You start with a few simple spells (outlined below), and walla — it’s done.


## It’s Show Time!


The following assumes you have followed the keys setup and platform-specific configuration in Veryfi’s hub documentation found here:[​https://hub.veryfi.com/lens/docs/flutter/](https://hub.veryfi.com/lens/docs/flutter/)


and have added Veryfi Lens Flutter wrapper to your project dependencies:


*pubspec.yaml* :


```text
dependencies:


veryfi:
git:
url: https://[USERNAME]:[PASSWORD]@repo.veryfi.com/shared/lens/flutter-plugin-veryfi-lens.git
ref: [VERSION]
```


Now we are ready to start.


### **Spell 1:** Credentials setup


```text
Map<String, dynamic> credentials = {
'clientId': 'XXXX',
'userName': 'XXXX',
'apiKey': 'XXXX',
'url': 'XXXX'
};
```


### **Spell 2:** Settings


```text
Map<String, dynamic> settings = {
'blurDetectionIsOn': false,
'showDocumentTypes': true
};
```


### **Spell 3:** Lens Initialization


```text
Veryfi.initLens(credentials, settings);
```


### **Spell 4:** Lens events handling


```text
Veryfi.setDelegate((LensEvent eventType, Map<String, dynamic> response) {
//Do something with the event type and response.
});
```


### **Spell 5:** Showing the Lens Camera


```text
Veryfi.showCamera();
```


**Note:** There is also a Github Demo App[​https://github.com/veryfi/veryfi-lens-flutter-demo](https://github.com/veryfi/veryfi-lens-flutter-demo) in case you need a running example to see how a real app looks using Veryfi’s Lens Flutter wrapper.


Abracadabra! Magic powers acquired.


Enjoy!


Sebastian Giraldo
*Mobile Software Engineer at Veryfi*
