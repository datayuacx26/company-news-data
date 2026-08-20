---
schema_version: "1.0.0"
document_id: "48f33b55f035258ab316eb542faea6e5351e350a1aa5610bb134e1a610443dc6"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/introducing-the-new-and-improved-widget"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:c6d91f071184b3292d59913f4ad1b3a64ec894bbded094d27001b5c7d3170e2c"
---

# Introducing the new and improved widget

The Hotglue widget is a core part of Hotglue’s product offering – it provides engineers with an embeddable frontend experience they can drop in their product to allow users to link and manage their integrations securely. The widget bakes in features like credential validation and handling of OAuth workflows, saving engineers headaches and valuable dev time.


Today we’re excited to announce the all-new **Hotglue Widget** ! This was a ground-up rebuild of the widget, including both the classic popup widget and inline connections component. The new version introduces:


- A modern redesign
- Prefetching for faster load times
- A client-side TypeScript SDK


Check out the demo below:


# Why we rebuilt the hotglue widget from the ground up


We want to minimize the time it takes for customers to launch their first integration, while still giving power users the flexibility to fully customize their Hotglue implementation.


The building blocks behind our new TypeScript-first, sleek, and simplified components can be easily taken apart to create your own custom implementation. For every drop-in component that you can use to test out the waters, we expose client-side functions and customization options that can take your integrations to the next level.


Given this goal, the new widget has been rebuilt for speed, flexibility, and a seamless fit with your product’s look and feel. You’ll notice shorter load times, smoother transitions, and a more responsive feel throughout, all while keeping the deep customization you expect.


We’re excited to keep expanding our toolkit to help users launch and scale integrations even faster!


# How to try out the new widget


## React


Install via npm for React:


` npm


install


@hotglue/widget@beta


`


From there, it’s easy to embed the inline Connections component or launch the widget popup:


` import


{


useHotglue


,


Connections


}


from


"@hotglue/widget"


;


export


default


function


WidgetLauncher


(


)


{


// Initialize the Hotglue widget


useHotglue


(


{


tenantId


:


"MY-TENANT-ID"


,


environmentId


:


process


.


env


.


NEXT_PUBLIC_HOTGLUE_ENV_ID


,


apiKey


:


process


.


env


.


NEXT_PUBLIC_HOTGLUE_PUBLIC_KEY


,


}


)


;


return


(


<


div


>


<


Connections


/


>


<


/


div


>


)


;


}


`


## Vanilla JS


Or use the CDN distribution with:


` <script src="\[<https://hotglue.com/widgetv3.js>\](<https://hotglue.xyz/widgetv3.js>)"></script>


`


With the new widget, offering seamless integrations for your customers only takes a couple lines of code:


` <


html


>


<


head


>


<


script src


=


"\[<https://hotglue.com/widgetv3.js>\](<https://hotglue.xyz/widgetv3.js>)"


>


<


/


script


>


<


script


>


const


hotglue


=


new


Hotglue


(


{


environmentId


:


'<ENV ID>'


,


apiKey


:


'<PUBLIC API KEY>'


}


)


;


// Open the widget popup


function


openWidget


(


)


{


hotglue


.


openWidget


(


)


;


}


<


/


script


>


<


/


head


>


<


body


>


<


button onclick


=


"openWidget()"


>


Open


Widget


<


/


button


>


<


/


body


>


<


/


html


>


`


[Check out the docs](https://docs.hotglue.com/widget-v3/overview) to learn more about the new widget and how to use it!


TABLE OF CONTENTS


- Why we rebuilt the hotglue widget from the ground up
- How to try out the new widget
- React
- Vanilla JS


RECOMMENDED BLOGS


[JavaScript & TypeScript Support for Hotglue Transforms Hotglue now supports writing transformation scripts in JavaScript or TypeScript in addition to Python!](https://hotglue.com/blog/js-ts-support-for-hotglue-transforms)


[Optimizing Microsoft SQL insert speed in Python: pymssql vs. pyodbc vs. bcp Learn how we optimized our Microsoft SQL target by using bcp to boost insertion speed and switching from pymssql to pyodbc.](https://hotglue.com/blog/optimizing-microsoft-sql-insert-speed-in-python)


[How to build a public Shopify app Learn how to build a public Shopify app. This guide walks through authentication, app setup, and how to use hotglue to simplify the entire process.](https://hotglue.com/blog/how-to-build-a-public-shopify-app)
