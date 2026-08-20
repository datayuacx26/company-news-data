---
schema_version: "1.0.0"
document_id: "e5feef5ec9c4d044612d9023ab0cec4e07591da7e4683e437311acbe96f170d2"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/js-ts-support-for-hotglue-transforms"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:7b5e55075136d46059b3061a0b89b621375f954f3b11382586ca31875af2b3aa"
---

# JavaScript & TypeScript Support for Hotglue Transforms

We’re excited to share that Hotglue now supports writing transformation scripts in JavaScript or TypeScript in addition to Python!


# Why we’re adding JS/TS support


Since we started Hotglue we have been huge supporters of the Python ecosystem:


- libraries like Pandas and Dask make it extremely easy to work with large volumes of data in a performant way,
- the[Singer.io](http://singer.io/) framework and connectors are also all built in Python, and
- the surrounding Python ecosystem has great SDKs and bindings for all sorts of things like dealing with CSV and Parquet files


We still believe strongly that the Python ecosystem is ideal for working with integration data and there is a huge benefit to using these modern libraries (including newer ones like Polars).


However, many of our users are not familiar with Python.


The goal of Hotglue has always been to give engineering teams the ability to avoid some of the heavier data engineering and maintenance work that goes into supporting integrations. Often that means the engineers using Hotglue are core product engineers that spend most of their time building in JavaScript/TypeScript. This made using Hotglue’s data transformation layer pretty daunting, as it required switching to a different, unfamiliar language.


That’s why we felt that giving users the ability to choose which language they write their transformation scripts could actually reduce the learning curve and make it easier to get value from Hotglue.


# How it works


If you’re a current Hotglue user you’ve likely used our[gluestick](https://github.com/hotgluexyz/gluestick) library, which provides some utility functions to make it easy to work with your integration data using[pandas](https://pandas.pydata.org/) .


We’ve built a new version of the same library called[gluestick-ts](https://github.com/hotgluexyz/gluestick-ts) (available on[npm](https://www.npmjs.com/package/@hotglue/gluestick-ts) ), built on[polars](https://pola.rs/) . This makes it super easy to load your job data into a polars dataframe and apply your logic. We’ve also built a basic starter script available on GitHub:[https://github.com/hotgluexyz/etl-scripts-ts](https://github.com/hotgluexyz/etl-scripts-ts)


The breadth of what you can do with the transformation layer is quite large, but to illustrate we’ve put together a little demo working with data from Salesforce:


Here’s the script used in the example for reference:


` import


*


as


gs


from


'@hotglue/gluestick-ts'


;


import


pl


from


'nodejs-polars'


;


const


ROOT_DIR


=


process


.


env


.


ROOT_DIR


??


'.'


;


const


INPUT_DIR


=


\`


${


ROOT_DIR


}


/sync-output


\`


;


const


OUTPUT_DIR


=


\`


${


ROOT_DIR


}


/etl-output


\`


;


async


function


runEtl


(


)


{


console


.


log


(


'Starting ETL process...'


)


;


// Create Reader instance


const


input


=


new


gs


.


Reader


(


INPUT_DIR


,


ROOT_DIR


)


;


// Get all available streams


const


availableStreams


=


input


.


keys


(


)


;


console


.


log


(


'Available streams:'


,


availableStreams


)


;


// Read the raw input data with catalog types


const


accountsDf


=


input


.


get


(


"Account"


,


{


catalogTypes


:


true


}


)


;


const


contactsDf


=


input


.


get


(


"Contact"


,


{


catalogTypes


:


true


}


)


;


// Merge the dataframes


let


outputDf


=


accountsDf


.


join


(


contactsDf


,


{


"leftOn"


:


"Id"


,


"rightOn"


:


"AccountId"


,


"how"


:


"inner"


,


"suffix"


:


"_contact"


}


)


;


// Only select the columns we need: Id, FirstName, LastName, Email, AccountId, Name


outputDf


=


outputDf


.


select


(


\[


"Id"


,


"FirstName"


,


"LastName"


,


"Email"


,


"Name"


\]


)


;


// Combine the FirstName and LastName columns into a single column called "FullName"


outputDf


=


outputDf


.


withColumn


(


pl


.


concatString


(


\[


pl


.


col


(


"FirstName"


)


,


pl


.


col


(


"LastName"


)


\]


,


" "


)


.


alias


(


"FullName"


)


)


.


drop


(


\[


"FirstName"


,


"LastName"


\]


)


;


// Export the data to the format defined by environment variables


gs


.


toExport


(


outputDf


,


"Contacts"


,


OUTPUT_DIR


)


;


console


.


log


(


'ETL process completed'


)


;


}


// Run the ETL if this file is executed directly


if


(


import


.


meta


.


url


===


\`


file://


${


process


.


argv


\[


1


\]


}


\`


)


{


runEtl


(


)


.


catch


(


error


=>


{


console


.


error


(


'ETL process failed:'


,


error


)


;


process


.


exit


(


1


)


;


}


)


;


}


export


{


runEtl


}


;


`


As you can see, the syntax is pretty simple, and thanks to polars the performance is blazing fast! We’re excited to see what you build with this.


If you’re interested in trying it out,reach out or read more in the docs:


- [https://docs.hotglue.com/transformation/javascript-scripts](https://docs.hotglue.com/transformation/javascript-scripts)
- [https://docs.hotglue.com/transformation/gluestick-ts/overview](https://docs.hotglue.com/transformation/gluestick-ts/overview)


Thanks for reading! :)


TABLE OF CONTENTS


- Why we’re adding JS/TS support
- How it works


RECOMMENDED BLOGS


[Optimizing Microsoft SQL insert speed in Python: pymssql vs. pyodbc vs. bcp Learn how we optimized our Microsoft SQL target by using bcp to boost insertion speed and switching from pymssql to pyodbc.](https://hotglue.com/blog/optimizing-microsoft-sql-insert-speed-in-python)


[How to build a public Shopify app Learn how to build a public Shopify app. This guide walks through authentication, app setup, and how to use hotglue to simplify the entire process.](https://hotglue.com/blog/how-to-build-a-public-shopify-app)


[QuickBooks New API Fees Explained + How to Monitor & Minimize Usage Starting July 28, 2025, QuickBooks is launching its new QuickBooks App Partner Program, which introduces paid tiers and API fees. Without careful monitoring, this new pricing model can get expensive fast.](https://hotglue.com/blog/how-to-lower-your-coreplus-quickbooks-api-usage)
