---
schema_version: "1.0.0"
document_id: "ffb0e23e801a86694d822e3af8af537cd1114eedb8b03ea36821f77a3b6865f8"
company_key: "yc-mindsdb"
company: "MindsDB"
source_id: "yc-mindsdb-news-import-209cdb193474"
canonical_url: "https://mindshub.ai/blog/blend-hybrid-retrieval-with-structured-data-using-mindsdb-knowledge-bases"
published_at: "2025-11-26T00:00:00+00:00"
first_seen_at: "2026-07-22T04:26:30.664731+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:2a2a55a133070d58735a51f0f1ce3388b5c53038a1aa31c1a44a88ec33dfea93"
---

# Blend Hybrid Retrieval with Structured Data using MindsDB Knowledge Bases

This tutorial is a follow-up to[this tutorial](https://mindshub.ai/blog/fast-track-knowledge-bases-how-to-build-semantic-ai-search-by-andriy-burkov) , where we took the first steps in creating and using a MindsDB Knowledge Base feature. In this follow-up project, we will walk through creating a semantic search knowledge base using the famous Enron Emails Dataset. While in the previous tutorial, we simply used an existing dataset, in this one, we’ll preprocess the original dataset by extracting structured attributes (also known as metadata) from it using Named Entity Recognition (NER). We will then create a knowledge base and perform both semantic and metadata-filtered searches.


Before we get our hands dirty, let’s refresh some basics.[Download the webinar code and materials here](https://info.mindsdb.com/e3t/Ctc/W5+113/d58S2n04/MWc75T6rldNW3KxJJ-3xSYHKW1PHNH55z43mJMZjDX45nR3bW8wM7ks6lZ3lkVmdC_97H6rWBW7-qwfg3BZpN7W3r0pn-6_0n6_W47q9bY6MHH-fW5Fq-884mCcw4W4Wtp1T3lr-vMW4K6cj83qc3bTW2R1vjj61n0zGN6l3vjysFVl_W76cGYS2hq9NSW52FV4475HB3TVQMCfm6pVjRgW7RjB-613hTj9V8tFrz2L8DflW96XBHT4Lb8_nW80LxBM1hMn8nW3Ml3xG2qXt6DW6RcBj_3lj_l5W1N6qRy5WYvXPW4f6cmr8wTH6WW6kSq-R8Dqh2rN5drnVWy8q3nW4qDMyd5N9QQ4N6PZg6-2FhQlVFp87G1PtznbVDckpd2xP1n2N4-xf3lxfZbWN4dgRjrkbsw1W7kw82q3KklqcW53kBBy63xBxpW4k4Lkx8PKF3BW1xVlJg4lW08WV1M9Wc5rdX7xW92cDv124CGg3W5MxTkM71sxg3W7t8CDv7CKmJzW1TSL1V7j37MyW50hpBc49xyRcW8g1Fkd3MMVBCW7nBNpW4XfL4WW8WTM-Z7NClb9W6RFtcB2RbHBVN5_C6L6VgFHqW57zG5F1XwwXSf4VN6Xb04?_gl=1*u6xap9*_gcl_au*MjAwNzUyNzYzNy4xNzYwNzI3NDQ1LjE4NzkwNzA2NDMuMTc2MzYzNzUzNS4xNzYzNjM3NTM0) to follow along the tutorial.


## **1. Introduction to Knowledge Bases in MindsDB**


Knowledge Bases (KB) in MindsDB provide advanced semantic search capabilities, allowing you to find information based on meaning rather than just keywords. They use embedding models to convert text into vector representations and store them in vector databases for efficient similarity searches.


In addition to searching for knowledge nuggets using semantic similarity (soft search criteria), MindsDB KBs allow the user to combine both soft search criteria with hard ones called “metadata,” which can be seen as regular relational database table columns.


> In this tutorial, we assume that the user has a free open-source MindsDB instance running in their local environment. Please follow[these steps](https://docs.mindsdb.com/setup/self-hosted/docker) to set it up.


To demonstrate both soft and hard searches in a MindsDB KB, we’ll use the Enron Corpus - one of the largest publicly available collections of corporate emails, containing over 500,000 emails from Enron executives during the years leading up to the company’s collapse in 2001. This dataset is particularly interesting because it contains real business communications, including scandal-related content, making it perfect for demonstrating knowledge base search capabilities.


Named Entity Recognition is the technique we’ll use to automatically extract those structured attributes - such as people, organizations, dates, and locations - from the raw email text. These extracted entities will become the metadata columns in our knowledge base, allowing us not only to search semantically by meaning, but also to filter results using precise, structured criteria like sender, company, or time period.


## **2. Settings Things Up**


#### **2.1 Dependencies Installation**


First, let’s install the dependencies and set up the NER. We will use SpaCy for this, since its pretrained models can automatically extract entities like people, organizations, dates, and locations from the raw email text. Those extracted entities will then be transformed into structured metadata columns, which we’ll store alongside the email content and later use to power rich, metadata-aware queries in our MindsDB knowledge base.


```text
!  pip   install   mindsdb   mindsdb_sdk   pandas   requests   datasets   yaspin   spacy


# Download spaCy English model for Named Entity Recognition
!  python   -m   spacy   download   en_core_web_sm


print  (  "✅ Dependencies installed successfully!"  )
```


#### **2.2 Dataset Selection and Download**


We’ll will download the Enron email’s dataset from[Hugging Face](https://huggingface.co/datasets/snoop2head/enron_aeslc_emails) , which is a large collection of real-world corporate messages from the Enron corpus, paired with their original subject lines and cleaned body text. Each entry includes the email’s metadata (such as sender, recipients, and timestamp) along with the full message content, organized into standard train/validation/test splits so the dataset be uses for tasks like summarization, classification, or downstream NLP experiments.


For our tutorial purposes, we will only use the train fraction of the dataset.


```text
# Download Enron Emails Dataset from Hugging Face
from   datasets   import   load_dataset
import   pandas   as   pd
import   re
from   datetime   import   datetime
import   json


# Load the Enron dataset (536k emails)
print  (  "Downloading Enron emails dataset..."  )
dataset   =   load_dataset(  "snoop2head/enron_aeslc_emails"  ,   split  =  "train"  )
df   =   pd.DataFrame(dataset)


print  (  f  "Dataset shape:   {  df.shape  }  "  )
print  (  "Dataset columns:"  , df.columns.tolist())


def   parse_email_text  (email_text):
"""Parse raw email text to extract subject, body, and metadata"""
if   pd.isna(email_text)   or   email_text   ==   ''  :
return   {  'subject'  :   ''  ,   'body'  :   ''  ,   'from'  :   ''  ,   'to'  :   ''  ,   'date'  :   ''  }


email_text   =   str  (email_text)


# Initialize result dictionary
parsed   =   {  'subject'  :   ''  ,   'body'  :   ''  ,   'from'  :   ''  ,   'to'  :   ''  ,   'date'  :   ''  }


# Extract subject
subject_match   =   re.search(  r  '  Subject:  \s  *  (.  *?  )(?:  \n  |  $)  '  , email_text, re.  IGNORECASE  )
if   subject_match:
parsed[  'subject'  ]   =   subject_match.group(  1  ).strip()


# Extract from
from_match   =   re.search(  r  '  From:  \s  *  (.  *?  )(?:  \n  |  $)  '  , email_text, re.  IGNORECASE  )
if   from_match:
parsed[  'from'  ]   =   from_match.group(  1  ).strip()


# Extract to
to_match   =   re.search(  r  '  To:  \s  *  (.  *?  )(?:  \n  |  $)  '  , email_text, re.  IGNORECASE  )
if   to_match:
parsed[  'to'  ]   =   to_match.group(  1  ).strip()


# Extract date
date_match   =   re.search(  r  '  Date:  \s  *  (.  *?  )(?:  \n  |  $)  '  , email_text, re.  IGNORECASE  )
if   date_match:
parsed[  'date'  ]   =   date_match.group(  1  ).strip()


# Extract body (everything after the headers)
# Look for the end of headers (usually marked by double newline or start of actual content)
header_end   =   re.search(  r  '  \n  \s  *  \n  '  , email_text)
if   header_end:
parsed[  'body'  ]   =   email_text[header_end.end():].strip()
else  :
# Fallback: try to find content after common header patterns
body_start   =   re.search(  r  '  (?:  Subject:  .  *?  \n  .  *?  \n  |  X-  .  *?  \n  )  '  , email_text, re.  DOTALL  )
if   body_start:
parsed[  'body'  ]   =   email_text[body_start.end():].strip()
else  :
parsed[  'body'  ]   =   email_text


# Clean up body text
parsed[  'body'  ]   =   re.sub(  r  '  \n  +  '  ,   ' '  , parsed[  'body'  ])
parsed[  'body'  ]   =   re.sub(  r  '  \s  +  '  ,   ' '  , parsed[  'body'  ])


return   parsed


# Parse first few emails to understand structure
print  (  "Parsing email structure..."  )
sample_emails   =   df.head(  10  )


parsed_samples   =   []
for   idx, row   in   sample_emails.iterrows():
# The dataset might have different column names, let's check
email_content   =   None
for   col   in   df.columns:
if   row[col]   and   len  (  str  (row[col]))   >   100  :    # Find the column with email content
email_content   =   row[col]
break


if   email_content:
parsed   =   parse_email_text(email_content)
parsed[  'email_id'  ]   =   f  "email_  {  idx  :06d  }  "
parsed_samples.append(parsed)
```


```text
Downloading Enron emails dataset...
Dataset shape: (535703, 1)
Dataset columns: ['text']
Parsing email structure...
```


Now let’s print some records to see what’s inside:


```text
df_parsed_sample = pd.DataFrame(parsed_samples)
print("\nSample of parsed emails:")
print("="*100)


for idx, row in df_parsed_sample.head(5).iterrows():
print(f"\nEmail #{idx+1}")
print(f"ID: {row['email_id']}")
print(f"From: {row['from'][:80]}{'...' if len(row['from']) > 80 else ''}")
print(f"To: {row['to'][:80]}{'...' if len(row['to']) > 80 else ''}")
print(f"Date: {row['date']}")
print(f"Subject: {row['subject']}")
print(f"Body Preview: {row['body'][:200]}{'...' if len(row['body']) > 200 else ''}")
print("-" * 80)


print(f"\nSuccessfully parsed email structure!")
print(f"Columns extracted: {df_parsed_sample.columns.tolist()}")
```


```text
Sample of parsed emails:
====================================================================================================


Email #1
ID: email_000000
From:  [email protected]
To:  [email protected]
Date: Mon, 14 May 2001 16:39:00 -0700 (PDT)
Subject: Body:
Body Preview: Here is our forecast
--------------------------------------------------------------------------------


Email #2
ID: email_000001
From:  [email protected]
To:  [email protected]
Date: Fri, 4 May 2001 13:51:00 -0700 (PDT)
Subject: Re:
Body Preview: Traveling to have a business meeting takes the fun out of the trip. Especially if you have to prepare a presentation. I would suggest holding the business plan meetings here then take a trip without a...
--------------------------------------------------------------------------------


Email #3
ID: email_000002
From:  [email protected]
To:  [email protected]
Date: Wed, 18 Oct 2000 03:00:00 -0700 (PDT)
Subject: Re: test
Body Preview: test successful. way to go!!!
--------------------------------------------------------------------------------


Email #4
ID: email_000003
From:  [email protected]
To:  [email protected]
Date: Mon, 23 Oct 2000 06:13:00 -0700 (PDT)
Subject: Body:
Body Preview: Randy, Can you send me a schedule of the salary and level of everyone in the scheduling group. Plus your thoughts on any changes that need to be made. (Patti S for example) Phillip
--------------------------------------------------------------------------------


Email #5
ID: email_000004
From:  [email protected]
To:  [email protected]
Date: Thu, 31 Aug 2000 05:07:00 -0700 (PDT)
Subject: Re: Hello
Body Preview: Let's shoot for Tuesday at 11:45.
--------------------------------------------------------------------------------


Successfully parsed email structure!
Columns extracted: ['subject', 'body', 'from', 'to', 'date', 'email_id']
```


The data looks good, so now, let’s load SpaCy and its pretrained NLP model for English:


```text
import   spacy
from   spacy   import   displacy
# Load spaCy model for NER
print  (  "Loading spaCy model for Named Entity Recognition..."  )
nlp   =   spacy.load(  "en_core_web_sm"  )
try  :
nlp   =   spacy.load(  "en_core_web_sm"  )
print  (  "spaCy model 'en_core_web_sm' loaded successfully!"  )
except   OSError   as   e:
print  (  "Failed to load spaCy model:"  , e)
```


```text
Loading spaCy model for Named Entity Recognition...
spaCy model 'en_core_web_sm' loaded successfully!
```


#### **2.3 Preparing the Dataset for the Knowledge Base**


So far, we have got a raw collection of email messages, but we need a dataset to created a knowledge base from. In this dataset, we want to have natural language texts for soft semantic search and named attributes for hard filtering of data rows.


The first step in preparing a dataset for a KB is cleaning it up and making sure we have a unique ID column. The second step is extracting the named entities from the cleaned records. We will perform both steps in the below cell:


```text
MAX_EMAILS_TO_PROCESS   =   500_000
MIN_BODY_SIZE_CHARS   =   50


import   json
from   tqdm   import   tqdm


def   clean_email_content  (text):
"""Clean and prepare email text for processing"""
if   pd.isna(text)   or   text   ==   ''  :
return   ""


# Convert to string and clean
text   =   str  (text)
# Remove excessive whitespace and newlines
text   =   re.sub(  r  '  \n  +  '  ,   ' '  , text)
text   =   re.sub(  r  '  \s  +  '  ,   ' '  , text)
# Remove common email artifacts
text   =   re.sub(  r  '  -----Original Message-----  .  *  $  '  ,   ''  , text,   flags  =  re.  MULTILINE   |   re.  DOTALL  )
text   =   re.sub(  r  '  ________________________________  .  *  $  '  ,   ''  , text,   flags  =  re.  MULTILINE   |   re.  DOTALL  )


return   text.strip()


def   extract_entities_with_ner  (text, nlp_model):
"""Extract named entities using spaCy NER"""
if   not   text   or   len  (text.strip())   ==   0  :
return   {}


# Limit text length to avoid memory issues
text   =   text[:  5000  ]   if   len  (text)   >   5000   else   text


try  :
doc   =   nlp_model(text)
entities   =   {
'persons'  : [],
'organizations'  : [],
'locations'  : [],
'money'  : [],
'dates'  : [],
'events'  : [],
'products'  : []
}


for   ent   in   doc.ents:
entity_text   =   ent.text.strip()
if   len  (entity_text)   <   2  :    # Skip very short entities
continue


if   ent.label_   ==   "PERSON"  :
entities[  'persons'  ].append(entity_text)
elif   ent.label_   ==   "ORG"  :
entities[  'organizations'  ].append(entity_text)
elif   ent.label_   in   [  "GPE"  ,   "LOC"  ]:    # Geopolitical entities, locations
entities[  'locations'  ].append(entity_text)
elif   ent.label_   ==   "MONEY"  :
entities[  'money'  ].append(entity_text)
elif   ent.label_   ==   "DATE"  :
entities[  'dates'  ].append(entity_text)
elif   ent.label_   ==   "EVENT"  :
entities[  'events'  ].append(entity_text)
elif   ent.label_   ==   "PRODUCT"  :
entities[  'products'  ].append(entity_text)


# Remove duplicates and limit to top entities
for   key   in   entities:
entities[key]   =   list  (  set  (entities[key]))[:  5  ]    # Max 5 entities per type


return   entities
except   Exception   as   e:
print  (  f  "Error processing text:   {  e  }  "  )
return   {}


# Use the real Enron dataset that was loaded earlier
print  (  f  "📧 Working with real Enron dataset:   {  df.shape[  0  ]  }   emails"  )
print  (  f  "Dataset columns:   {  df.columns.tolist()  }  "  )


# Sample a reasonable subset for this tutorial (full dataset is very large)
print  (  "Sampling real Enron emails for processing..."  )
df_sample   =   df.sample(  n  =  MAX_EMAILS_TO_PROCESS  ,   random_state  =  42  ).reset_index(  drop  =  True  )


# Process the real emails and extract entities
print  (  "Processing real Enron emails and extracting entities..."  )
processed_emails   =   []


for   idx, row   in   tqdm(df_sample.iterrows(),   total  =  len  (df_sample),   desc  =  "Processing emails"  ):
# Get the raw email content from 'text' column
email_content   =   row[  'text'  ]


if   not   email_content   or   len  (  str  (email_content))   <   100  :
continue


# Parse the email using the function from Cell 3
parsed   =   parse_email_text(email_content)


# Clean the content
subject   =   clean_email_content(parsed[  'subject'  ])
body   =   clean_email_content(parsed[  'body'  ])


if   len  (body)   <   MIN_BODY_SIZE_CHARS  :    # Skip very short emails
continue


# Extract entities from both subject and content
full_text   =   f  "  {  subject  }   {  body  }  "
entities   =   extract_entities_with_ner(full_text, nlp)


# Create email ID
email_id   =   f  "email_  {  idx  :06d  }  "


processed_email   =   {
'email_id'  : email_id,
'from_address'  : parsed[  'from'  ][:  100  ]   if   parsed[  'from'  ]   else   ''  ,    # Limit length
'to_address'  : parsed[  'to'  ][:  100  ]   if   parsed[  'to'  ]   else   ''  ,          # Limit length
'date_sent'  : parsed[  'date'  ][:  50  ]   if   parsed[  'date'  ]   else   ''  ,        # Limit length
'subject'  : subject,
'content'  : body,
'persons'  :   ', '  .join(entities[  'persons'  ])   if   entities[  'persons'  ]   else   ''  ,
'organizations'  :   ', '  .join(entities[  'organizations'  ])   if   entities[  'organizations'  ]   else   ''  ,
'locations'  :   ', '  .join(entities[  'locations'  ])   if   entities[  'locations'  ]   else   ''  ,
'money_amounts'  :   ', '  .join(entities[  'money'  ])   if   entities[  'money'  ]   else   ''  ,
'dates_mentioned'  :   ', '  .join(entities[  'dates'  ])   if   entities[  'dates'  ]   else   ''  ,
'events'  :   ', '  .join(entities[  'events'  ])   if   entities[  'events'  ]   else   ''  ,
'products'  :   ', '  .join(entities[  'products'  ])   if   entities[  'products'  ]   else   ''  ,
'content_length'  :   len  (body),
'entity_count'  :   sum  (  len  (v)   for   v   in   entities.values())
}


processed_emails.append(processed_email)


# Convert to DataFrame
df_processed   =   pd.DataFrame(processed_emails)


print  (  f  "  \n  ✅ Processed   {len  (df_processed)  }   real Enron emails with entities extracted"  )
```


```text
📧 Working with real Enron dataset: 535703 emails
Dataset columns: ['text']
Sampling real Enron emails for processing...
Processing real Enron emails and extracting entities...


Processing emails: 100%|███████████████████████████████████████████████| 500000/500000 [2:55:44<00:00, 47.42it/s]


✅ Processed 453905 real Enron emails with entities extracted
```


We now have a Pandas dataframe containing, for each email, its text and the extracted attributes. Let’s look at some of them and some stats:


```text
# Show a sample of processed data
print(f"\n📊 Sample of Enron emails processed:")
print("="*120)


for idx, row in df_processed.head(5).iterrows():
print(f"\n📧 Real Email #{idx+1}")
print(f"🆔 ID: {row['email_id']}")
print(f"👤 From: {row['from_address']}")
print(f"👤 To: {row['to_address']}")
print(f"📅 Date: {row['date_sent']}")
print(f"📝 Subject: {row['subject']}")
print(f"👥 Persons: {row['persons'] if row['persons'] else 'None detected'}")
print(f"🏢 Organizations: {row['organizations'] if row['organizations'] else 'None detected'}")
print(f"📍 Locations: {row['locations'] if row['locations'] else 'None detected'}")
print(f"💰 Money: {row['money_amounts'] if row['money_amounts'] else 'None detected'}")
print(f"💬 Content Preview: {row['content'][:200]}{'...' if len(row['content']) > 200 else ''}")
print("-" * 100)


# Show statistics on real data
print(f"\n📈 Real Data Processing Statistics:")
print(f"• Total real emails processed: {len(df_processed)}")
print(f"• Average content length: {df_processed['content_length'].mean():.0f} characters")
print(f"• Average entities per email: {df_processed['entity_count'].mean():.1f}")
print(f"• Emails with persons mentioned: {len(df_processed[df_processed['persons'] != ''])}")
print(f"• Emails with organizations mentioned: {len(df_processed[df_processed['organizations'] != ''])}")
print(f"• Emails with money amounts: {len(df_processed[df_processed['money_amounts'] != ''])}")


# Show some interesting real examples
print(f"\n🔍 Most interesting real emails (by entity count):")
top_emails = df_processed.nlargest(3, 'entity_count')
for idx, row in top_emails.iterrows():
print(f"\n📧 High-entity email from {row['from_address']}")
print(f"📝 Subject: {row['subject']}")
print(f"👥 Persons: {row['persons']}")
print(f"🏢 Organizations: {row['organizations']}")
print(f"💰 Money: {row['money_amounts']}")
```


```text
📊 Sample of Enron emails processed:
========================================================================================================================


📧 Real Email #1
🆔 ID: email_000000
👤 From:  [email protected]
👤 To:  [email protected]
📅 Date: Fri, 10 Dec 1999 08:33:00 -0800 (PST)
📝 Subject: Re: Meter 5892 - UA4 1996 and 1997 Logistics Issues
👥 Persons: Daren J Farmer/HOU, Susan, Meter 5892 - UA4 1996, Mary M Smith/HOU, Susan D Trevino
🏢 Organizations: Volume Management
📍 Locations: UA4
💰 Money: None detected
💬 Content Preview: Susan, I need you to do the research on this meter. You will need to review the various scheduling systems to see how this was handled prior to 2/96. You can also check with Volume Management to see i...
----------------------------------------------------------------------------------------------------


📧 Real Email #2
🆔 ID: email_000001
👤 From:  [email protected]
👤 To:  [email protected]  ,  [email protected]  ,  [email protected]  ,
📅 Date: Fri, 18 Aug 2000 05:03:00 -0700 (PDT)
📝 Subject: DRAFT
👥 Persons: Bcc
🏢 Organizations: None detected
📍 Locations: Rice Village
💰 Money: None detected
💬 Content Preview: Cc:  [email protected]   Bcc:  [email protected]   Remember, the draft is this Sunday at 11:45 am at BW-3 in Rice Village. Please try to be there on time so we can start promptly. -Eric


📧 Real Email #3
🆔 ID: email_000003
👤 From:  [email protected]
👤 To:  [email protected]
📅 Date: Mon, 31 Jul 2000 09:53:00 -0700 (PDT)
📝 Subject: More July CED-PGE
👥 Persons: Susan Fick, Patty
🏢 Organizations: None detected
📍 Locations: None detected
💰 Money: None detected
💬 Content Preview: Patty Could you please forward this to Susan Fick. I don't have her e-mail. LC


📧 Real Email #4
🆔 ID: email_000004
👤 From:  [email protected]
👤 To:  [email protected]  ,  [email protected]  ,  [email protected]  ,
📅 Date: Wed, 13 Dec 2000 07:04:00 -0800 (PST)
📝 Subject: Body:
👥 Persons: None detected
🏢 Organizations: None detected
📍 Locations: None detected
💰 Money: None detected
💬 Content Preview: Attached are two files that illustrate the following: As prices rose, supply increased and demand decreased. Now prices are beginning to fall in response these market responses.


📧 Real Email #5
🆔 ID: email_000005
👤 From:  [email protected]
👤 To:  [email protected]  ,  [email protected]  ,  [email protected]  ,
📅 Date: Tue, 31 Jul 2001 08:28:00 -0700 (PDT)
📝 Subject: El Paso
👥 Persons: Origination El Paso, Tx 77252-2511, Kurt Lindahl Sr., Rob Bryngelson
🏢 Organizations: the ElPaso Corporation, El Paso, Global LNG Division, El Paso Merchant Energy, Business Development
📍 Locations: Houston
💰 Money: None detected
💬 Content Preview: Dear Friends and Colleagues, This note is to inform you that I have joined El Paso Merchant Energy in their Global LNG Division reporting to Rob Bryngelson, Managing Director, Business Development. Pl...


📈 Real Data Processing Statistics:
• Total real emails processed: 453905
• Average content length: 1474 characters
• Average entities per email: 8.1
• Emails with persons mentioned: 383312
• Emails with organizations mentioned: 363549
• Emails with money amounts: 63303


🔍 Most interesting real emails (by entity count):


📧 High-entity email from  [email protected]
📝 Subject: Syncrasy Daily Trader Summary for Wed, Jan 16, 2002
👥 Persons: Data, NC ERCOT(SP, Max, Aquila, Andy Weingarten
🏢 Organizations: Trader Summary, ERCOT(SP, SPP(= SP, Average-Daily Maximum Temperature', MAPP(HP
💰 Money: 37 -1 MAIN(CTR, 50,000, 43 -1 MAIN(CTR, 36 -1 MAIN(CTR, 40 -1 WSCC(RK


📧 High-entity email from  [email protected]
📝 Subject: Iceland Food Festival
👥 Persons: Hotel Klopp, Rich, Mar 1 - National Beer Day, David Rosengarten, Subject
🏢 Organizations: Reykjav?k/K?pavogur, Party, Party Gourmet Dinner, BWI, SCENIC SIGHTSEEING Blue Lagoon
💰 Money: 65, 66, 69, 50, 55


📧 High-entity email from  [email protected]
📝 Subject: True Orange, November 27, Part 2
👥 Persons: Sooners, Jody Conradt, ESPN, Harris, Northwestern
🏢 Organizations: Oregon State, K-State, Texas A&M, SEC, Big East
💰 Money: $1.1 million, $1.9 million, $2.5 million, $1.2 million, 750,000
```


The data looks good, so let’s now save it into a CSV file that we will then load to our knowledge base:


```text
# Save processed real data
df_processed.to_csv('enron_emails_processed_real.csv', index=False)
print(f"\n✅ Real Enron emails saved to 'enron_emails_processed_real.csv'")
```


```text
✅ Real Enron emails saved to 'enron_emails_processed_real.csv'
```


#### **2.3 Connecting to the Vector Store**


When the user creates a MindsDB Knowledge Base, MindsDB chunks all the text fragments into pieces (chunks) and uses an external text embedding model to convert each chunk into an embedding vector. Embedding vectors are numerical arrays that have the following property: if two texts are similar semantically, then their embedding vectors are close to each other in the vector space. This allows us to compare two texts semantically by applying a mathematical operation (like cosine similarity) to two vectors to see how close they are in the vector space.


These embedding vectors need to be stored somewhere. There are various vector databases, including several open-source ones. MindsDB supports ChromaDB by default. However, ChromaDB doesn’t support the “LIKE” operation, which is a standard operation in relational database SELECT queries. We will use LIKE in our tutorial; therefore, we will use a different open-source vector store, PGVector, which is part of the Postgres ecosystem.


For this tutorial, we provisioned a PGVector instance on AWS. You can install it locally too.[Here’s how you can do it](https://www.datacamp.com/tutorial/pgvector-tutorial) .


Let’s create a vector database` enron_kb_pgvector` , which will store knowledge base’s embedding vectors:


```text
#   Drop   an existing pgvector   database   if   it   exists
try  :
print  (  "🗑️  Dropping existing pgvector database..."  )
server  .  query  (  "DROP DATABASE IF EXISTS enron_kb_pgvector;"  ).  fetch  ()
print  (  "✅ Dropped existing database"  )
except   Exception   as   e:
print  (f  "⚠️  Drop error: {e}"  )


#   Create   fresh pgvector   database   connection
try  :
server  .  query  (  """
CREATE DATABASE enron_kb_pgvector
WITH ENGINE = 'pgvector',
PARAMETERS = {
"  host  ": "  c3hsmn51hjafhh  .  cluster  -  czrs8kj4isg7  .  us  -  east  -  1  .  rds  .  amazonaws  .com  ",
"  port  ": 5432,
"  database  ": "  df1f3i5s2jrksf  ",
"  user  ": "  u36kd0g64092pk  ",
"  password  ": "  pc08df7cb724a4ad6b1a8288c3666fa087f1a89c1ba5d1a555b40a8ba863672e4  "
};
"""  ).  fetch  ()
print  (  "✅ Created pgvector database connection 'enron_kb_pgvector'"  )
except   Exception   as   e:
print  (f  "❌ Database connection error: {e}"  )
raise
```


```text
🗑️  Dropping existing pgvector database...
✅ Dropped existing database
✅ Created pgvector database connection 'enron_kb_pgvector'
```


#### **2.4 Uploading the Dataset to MindsDB**


Now let’s connect to our local MindsDB instance and upload the dataset:


> Remember, that in this tutorial, we assume that the user has a free open-source MindsDB instance running in their local environment. Please follow[these steps](https://docs.mindsdb.com/setup/self-hosted/docker) to set it up.


```text
import   mindsdb_sdk
import   re


# Connect to the MindsDB server
server   =   mindsdb_sdk.connect(  'http://127.0.0.1:47334'  )
print  (  "Connected to MindsDB server"  )


# List available databases to confirm connection
databases   =   server.databases.list()
print  (  "Available databases:"  )
for   db   in   databases:
print  (  f  "-   {  db.name  }  "  )


# First drop any knowledge bases
try  :
print  (  "🗑️  Dropping knowledge bases..."  )
server.query(  "DROP KNOWLEDGE_BASE IF EXISTS enron_kb;"  ).fetch()
print  (  "✅ Dropped knowledge base enron_kb"  )
except   Exception   as   e:
print  (  f  "⚠️  KB drop error:   {  e  }  "  )


# Check if df_processed exists and has real data
try  :
print  (  f  "  \n  📊 Checking real processed Enron data..."  )
print  (  f  "Shape:   {  df_processed.shape  }  "  )
print  (  f  "Columns:   {  df_processed.columns.tolist()  }  "  )


if   len  (df_processed)   ==   0  :
print  (  "❌ df_processed is empty. Please run the cell that creates it first."  )
raise   ValueError  (  "No processed data available"  )


df_upload   =   df_processed.copy()
print  (  f  "✅ Using   {len  (df_upload)  }   real processed Enron emails"  )


except   NameError  :
print  (  "❌ Error: df_processed not found. Please run the cell that creates it first."  )
print  (  "Cannot continue without real processed data."  )
raise


def   clean_for_upload  (text):
"""Clean text data for safe upload to MindsDB"""
if   pd.isna(text)   or   text   ==   ''  :
return   ''


text   =   str  (text)
# Remove problematic characters that might cause encoding issues
text   =   re.sub(  r  '  [  ^  \w\s  \-\.\@\,\;\:\!\?\(\)\[\]\/  ]  '  ,   ' '  , text)
# Remove excessive whitespace
text   =   re.sub(  r  '  \s  +  '  ,   ' '  , text)
# Limit length to prevent upload issues
if   len  (text)   >   2000  :
text   =   text[:  1997  ]   +   '...'


return   text.strip()


# Clean the real data for upload
print  (  "Cleaning real Enron data for upload..."  )


# Clean text fields
text_columns   =   [  'from_address'  ,   'to_address'  ,   'date_sent'  ,   'subject'  ,   'content'  ,
'persons'  ,   'organizations'  ,   'locations'  ,   'money_amounts'  ,
'dates_mentioned'  ,   'events'  ,   'products'  ]


for   col   in   text_columns:
if   col   in   df_upload.columns:
df_upload[col]   =   df_upload[col].apply(clean_for_upload)


# Ensure numeric columns are properly typed
for   col   in   [  'content_length'  ,   'entity_count'  ]:
if   col   in   df_upload.columns:
df_upload[col]   =   pd.to_numeric(df_upload[col],   errors  =  'coerce'  ).fillna(  0  ).astype(  int  )


print  (  f  "  \n  📋 Final real Enron dataset for upload:"  )
print  (  f  "Shape:   {  df_upload.shape  }  "  )
print  (  f  "Sample from addresses:   {  df_upload[  'from_address'  ].head(  3  ).tolist()  }  "  )
print  (  f  "Sample subjects:   {  df_upload[  'subject'  ].head(  3  ).tolist()  }  "  )


# Upload to MindsDB
files_db   =   server.get_database(  "files"  )
table_name   =   "enron_emails"


# Delete existing table if it exists
try  :
files_db.tables.drop(table_name)
print  (  f  "Dropped existing table   {  table_name  }  "  )
except   Exception  :
pass


# Upload real Enron data
try  :
print  (  "Uploading real Enron emails to MindsDB..."  )
files_db.create_table(table_name, df_upload)
print  (  f  "✅ Created table files.  {  table_name  }   with real Enron data"  )


# Verify upload with real data
sample_data   =   server.query(  f  "SELECT email_id, subject, persons, organizations FROM files.  {  table_name  }   LIMIT 5"  ).fetch()
print  (  "  \n  ✅ Sample real Enron data uploaded:"  )
for   idx, row   in   sample_data.iterrows():
print  (  f  "📧   {  row[  'email_id'  ]  }  :   {  row[  'subject'  ][:  60  ]  }  ..."  )
print  (  f  "   👥 Persons:   {  row[  'persons'  ]  }  "  )
print  (  f  "   🏢 Orgs:   {  row[  'organizations'  ]  }  "  )


# Check total count
count_result   =   server.query(  f  "SELECT COUNT(*) as total FROM files.  {  table_name  }  "  ).fetch()
total_emails   =   count_result.iloc[  0  ][  'total'  ]
print  (  f  "  \n  📊 Total real Enron emails uploaded:   {  total_emails  }  "  )


except   Exception   as   e:
print  (  f  "❌ Upload failed:   {  e  }  "  )


print  (  "  \n  ✅ Real Enron data upload process completed!"  )
```


```text
Connected to MindsDB server
Available databases:
- files
- movies_kb_chromadb
🗑️  Dropping knowledge bases...
✅ Dropped knowledge base enron_kb


📊 Checking real processed Enron data...
Shape: (453905, 15)
Columns: ['email_id', 'from_address', 'to_address', 'date_sent', 'subject', 'content', 'persons', 'organizations', 'locations', 'money_amounts', 'dates_mentioned', 'events', 'products', 'content_length', 'entity_count']
✅ Using 453905 real processed Enron emails
Cleaning real Enron data for upload...


📋 Final real Enron dataset for upload:
Shape: (453905, 15)
Sample from addresses: [' [email protected]  ', ' [email protected]  ', ' [email protected]  ']
Sample subjects: ['Re: Meter 5892 - UA4 1996 and 1997 Logistics Issues', 'DRAFT', 'More July CED-PGE']
Dropped existing table enron_emails
Uploading real Enron emails to MindsDB...
✅ Created table files.enron_emails with real Enron data


✅ Sample real Enron data uploaded:
📧 email_000000: Re: Meter 5892 - UA4 1996 and 1997 Logistics Issues...
🏢 Orgs: Volume Management
📧 email_000001: DRAFT...
👥 Persons: Bcc
🏢 Orgs: None
📧 email_000003: More July CED-PGE...
👥 Persons: Susan Fick, Patty
🏢 Orgs: None
📧 email_000004: Body:...
👥 Persons: None
🏢 Orgs: None
📧 email_000005: El Paso...
🏢 Orgs: the ElPaso Corporation, El Paso, Global LNG Division, El Paso Merchant Energy, Business Development


📊 Total real Enron emails uploaded: 453905


✅ Real Enron data upload process completed!
```


#### **2.4 Creating a Knowledge Base**


Now, let’s create a knowledge base` enron_kb` using our emails data. We’ll use OpenAI’s embedding model to convert the text into vectors. Note the` storage = enron_kb_pgvector.enron_vectors` parameter which tells MindsDB to use our PGVector vector store. If we omit this parameter, teh default ChromaDB vector store will be used.


```text
#   Drop   existing knowledge base   if   it   exists
server  .  query  (  "DROP KNOWLEDGE_BASE IF EXISTS enron_kb;"  ).  fetch  ()


#   Create   knowledge base   with   pgvector storage
try  :
kb_creation_query   =   server  .  query  (f  """
CREATE KNOWLEDGE_BASE enron_kb
USING
storage = enron_kb_pgvector.enron_vectors,
embedding_model = {{
"  provider  ": "  openai  ",
"  model_name  ": "  text-  embedding  -  3  -  large  "
}},
metadata_columns = [
'subject', 'persons', 'organizations', 'locations',
'money_amounts', 'dates_mentioned', 'events', 'products',
'content_length', 'entity_count',
'from_address', 'to_address', 'date_sent'
],
content_columns = ['content'],
id_column = 'email_id';
"""  )
kb_creation_query  .  fetch  ()
print  (  "✅ Created knowledge base 'enron_kb' with email address and date filtering support"  )
except   Exception   as   e:
print  (f  "❌ Knowledge base creation error: {e}"  )
```


```text
✅ Created knowledge base 'enron_kb' with email address and date filtering support
```


Now let’s insert our email data into the knowledge base:


```text
# Insert the email data into the knowledge base (including the new metadata columns)
from   yaspin   import   yaspin


try  :
with   yaspin(  text  =  "Inserting emails into updated knowledge base..."  ):
insert_query   =   server.query(  f  """
INSERT INTO enron_kb
SELECT email_id,
subject,
persons,
organizations,
locations,
money_amounts,
dates_mentioned,
events,
products,
content_length,
entity_count,
from_address,
to_address,
date_sent,
content
FROM   files.  {  table_name  }
USING
batch_size = 200,
threads = 10,
error = 'skip',
track_column = email_id;
"""  ).fetch()
print  (  "✅ Emails inserted successfully into updated knowledge base!"  )
except   Exception   as   e:
print  (  f  "❌ Insert error:   {  e  }  "  )
```


```text
✅ Emails inserted successfully into updated knowledge base!
```


Let’s see what the data in the KB looks like:


```text
search_query = server.query("SELECT * FROM enron_kb;")
display(search_query.fetch())


search_query = server.query("SELECT count(*) FROM enron_kb;")
display(search_query.fetch())
```


id chunk_id chunk_content metadata relevance distance


0 email_024653 email_024653:content:3of3:1993to2382 Palmer of Caminus Corp on European Markets! ht… {‘events’: None, ‘_source’: ‘TextChunkingPrepr… None None


1 email_024585 email_024585:content:1of1:0to429 Most of you already know, but the move is taki… {‘events’: None, ‘_source’: ‘TextChunkingPrepr… None None


2 email_024902 email_024902:content:1of2:0to997 To facilitate these changes, you received an O… {‘events’: None, ‘_source’: ‘TextChunkingPrepr… None None


3 email_025043 email_025043:content:1of1:0to801 According to our system records, you have not … {‘events’: None, ‘_source’: ‘TextChunkingPrepr… None None


4 email_025044 email_025044:content:1of3:0to996 The attached preliminary comments were finaliz… {‘events’: None, ‘_source’: ‘TextChunkingPrepr… None None


… … … … … … …


676450 email_024824 email_024824:content:1of1:0to585 Cc:[\[email protected\]](https://mindshub.ai/cdn-cgi/l/email-protection#5b36757537342d3e1b3e3529343575383436) ,[\[email protected\]](https://mindshub.ai/cdn-cgi/l/email-protection#b5c6d6dac1c19bc5d4d9d8d0c7f5d0dbc7dadb9bd6dad8) … {‘events’: None, ‘_source’: ‘TextChunkingPrepr… None None


676451 email_024829 email_024829:content:1of3:0to998 20 \[IMAGE\] CO.O.L. Travel Specials \[IMAGE\] Wed… {‘events’: ‘Love Field’, ‘_source’: ‘TextChunk… None None


676452 email_024829 email_024829:content:2of3:999to1998 on either Monday, May 21 or Tuesday, May 22, 2… {‘events’: ‘Love Field’, ‘_source’: ‘TextChunk… None None


676453 email_024829 email_024829:content:3of3:1999to2391 IN 139 - Louisville, KY return to top Featured… {‘events’: ‘Love Field’, ‘_source’: ‘TextChunk… None None


676454 email_024804 email_024804:content:1of1:0to354 FYI — David Leboe in Investor Relations autho… {‘events’: None, ‘_source’: ‘TextChunkingPrepr… None None


676455 rows × 6 columns


count_0


0 676455


As we can see, the data looks very much like a regular relational table. However, the fact that it’s a knowledge base instance rather than a regular database connection, allows us to use a special syntax mixing the semantic similarity with regulare SQL “WHERE” constructs.


You can also notice that the knowledge base contains chunks rather than the original texts of the email messages. Each chunk has its own embedding vectr. This allows finding more granular pieces of content similar to the user’s question.


#### **3. Performing Semantic Searches**


Now that our knowledge base is ready (or being populated), let’s do some Q&A. For convenience, we will setup a utility function` answer_question_about_enron` which will take as input question about the data and the attribute this data is expected to contain such as people names, organizations, locaitons, etc: the attributes thet the NER was supposed to have extracted.


This utility function will combine the inputs into a SELECT query by using the MindsDB syntax. For example, if our question/request is “I need to see emails mentioning fraud.” and we want to only to see emails from “John Smith”, our SELECT query constructed by` answer_question_about_enron` would look like this:


```text
SELECT
id,
chunk_content,
relevance,
metadata
FROM   enron_kb_full
WHERE   content   =   'I need to see emails mentioning fraud.'   AND   persons   LIKE   '%John Smith%'
ORDER BY   relevance   DESC
LIMIT   100  ;
```


The above query will return emails that mention any fraud even if the word “fraud” itself isn’t used in the emails’ texts. This is a soft search. Only those emails will be retuned whose “persons” attribute contains “John Smith”. This is a hard search.


```text
import   openai
import   time
import   json
from   IPython.display   import   display


# Set up OpenAI client (replace with your API key)
client   =   openai.OpenAI(  api_key  =  "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"  )


def   answer_question_about_enron  (question:   str  , persons  =  None  , organizations  =  None  , locations  =  None  , money_amounts  =  None  , subjects  =  None  ):
"""
Answer questions about Enron using the knowledge base with metadata filtering


Args:
question (str): The question to ask
persons (str or list): Person name(s) to filter by
organizations (str or list): Organization name(s) to filter by
locations (str or list): Location name(s) to filter by
money_amounts (str or list): Money amount(s) to filter by
subjects (str or list): Subject keyword(s) to filter by


Returns:
str: Generated answer based on relevant emails
"""


def   create_like_conditions  (column_name, values):
"""Helper function to create LIKE conditions for single values or lists"""
if   values   is   None  :
return   []


# Convert single value to list for uniform processing
if   isinstance  (values,   str  ):
values   =   [values]
elif   not   isinstance  (values, (  list  ,   tuple  )):
values   =   [  str  (values)]


conditions   =   []
for   value   in   values:
conditions.append(  f  "  {  column_name  }   LIKE '%  {  value  }  %'"  )


return   conditions


print  (  f  "🤔 Question:   {  question  }  "  )


# Build WHERE clause with optional filters
where_conditions   =   [  f  "content = '  {  question  }  '"  ]


# Handle persons filter
if   persons:
person_conditions   =   create_like_conditions(  "persons"  , persons)
if   person_conditions:
where_conditions.extend(person_conditions)
if   isinstance  (persons,   list  ):
print  (  f  "🔍 Filtering by persons:   {  ', '  .join(persons)  }  "  )
else  :
print  (  f  "🔍 Filtering by persons:   {  persons  }  "  )


# Handle organizations filter
if   organizations:
org_conditions   =   create_like_conditions(  "organizations"  , organizations)
if   org_conditions:
where_conditions.extend(org_conditions)
if   isinstance  (organizations,   list  ):
print  (  f  "🔍 Filtering by organizations:   {  ', '  .join(organizations)  }  "  )
else  :
print  (  f  "🔍 Filtering by organizations:   {  organizations  }  "  )


# Handle locations filter
if   locations:
loc_conditions   =   create_like_conditions(  "locations"  , locations)
if   loc_conditions:
where_conditions.extend(loc_conditions)
if   isinstance  (locations,   list  ):
print  (  f  "🔍 Filtering by locations:   {  ', '  .join(locations)  }  "  )
else  :
print  (  f  "🔍 Filtering by locations:   {  locations  }  "  )


# Handle money_amounts filter
if   money_amounts:
money_conditions   =   create_like_conditions(  "money_amounts"  , money_amounts)
if   money_conditions:
where_conditions.extend(money_conditions)
if   isinstance  (money_amounts,   list  ):
print  (  f  "🔍 Filtering by money amounts:   {  ', '  .join(money_amounts)  }  "  )
else  :
print  (  f  "🔍 Filtering by money amounts:   {  money_amounts  }  "  )


# Handle subjects filter
if   subjects:
subject_conditions   =   create_like_conditions(  "subject"  , subjects)
if   subject_conditions:
where_conditions.extend(subject_conditions)
if   isinstance  (subjects,   list  ):
print  (  f  "🔍 Filtering by subjects:   {  ', '  .join(subjects)  }  "  )
else  :
print  (  f  "🔍 Filtering by subjects:   {  subjects  }  "  )


where_clause   =   " AND "  .join(where_conditions)


try  :
search_query   =   f  """
SELECT
id,
chunk_content,
relevance,
metadata
FROM enron_kb
WHERE   {  where_clause  }
ORDER BY relevance DESC
LIMIT 100;
"""


print  (  f  "🔍 SQL Query:   {  search_query  }  "  )
search_results   =   server.query(search_query).fetch()


if   len  (search_results)   ==   0  :
return   "❌ No relevant emails found matching your criteria."


print  (  f  "✅ Found   {len  (search_results)  }   results"  )


# Show sample results
for   idx, row   in   search_results.head(  3  ).iterrows():
try  :
metadata   =   json.loads(  str  (row[  'metadata'  ]))   if   row[  'metadata'  ]   and   str  (row[  'metadata'  ])   !=   'nan'   else   {}
print  (  f  "  \n  📧 Result #  {  idx  +  1}  "  )
print  (  f  "🆔 Email ID:   {  row[  'id'  ]  }  "  )
print  (  f  "📊 Relevance:   {  row[  'relevance'  ]  :.4f  }  "  )
print  (  f  "📝 Subject:   {  metadata.get(  'subject'  ,   'No subject'  )  }  "  )
print  (  f  "👥 Persons:   {  metadata.get(  'persons'  ,   'None'  )  }  "  )
print  (  f  "🏢 Organizations:   {  metadata.get(  'organizations'  ,   'None'  )  }  "  )
print  (  f  "📍 Locations:   {  metadata.get(  'locations'  ,   'None'  )  }  "  )
content   =   str  (row[  'chunk_content'  ])
preview   =   content[:  200  ]   +   "..."   if   len  (content)   >   200   else   content
print  (  f  "💬 Content:   {  preview  }  "  )
print  (  "-"   *   60  )
except   Exception   as   e:
print  (  f  "Error processing result:   {  e  }  "  )


# Prepare context for GPT
context_parts   =   []
for   idx, row   in   search_results.iterrows():
try  :
email_context   =   f  """
Email ID:   {  row[  'id'  ]  }
Subject:   {  metadata.get(  'subject'  ,   'No subject'  )  }
Persons mentioned:   {  metadata.get(  'persons'  ,   'None'  )  }
Organizations mentioned:   {  metadata.get(  'organizations'  ,   'None'  )  }
Content:   {  row[  'chunk_content'  ][:  800  ]  }  ...
"""
context_parts.append(email_context)
except  :
continue


context   =   "  \n  ---  \n  "  .join(context_parts[:  8  ])


# Create prompt for GPT
prompt   =   f  """
You are an expert analyst studying the Enron corporate emails dataset. Based ONLY on the following
email excerpts from the Enron corpus, answer the user's question.


EMAIL EXCERPTS FROM ENRON CORPUS:
{  context  }


QUESTION:   {  question  }


Instructions:
- Provide a factual answer based only on the email content provided above
- If the emails mention specific people, organizations, or amounts, include those details
- If the emails don't contain enough information to answer the question, state that clearly
- Reference specific email IDs when making claims
"""


print  (  "🤖 Generating answer using GPT-4..."  )
response   =   client.chat.completions.create(
model  =  "gpt-4o"  ,
messages  =  [
{  "role"  :   "system"  ,   "content"  :   "You are a helpful analyst answering questions about Enron emails. Use only the provided email content and be specific about sources."  },
{  "role"  :   "user"  ,   "content"  : prompt}
],
temperature  =  0.1
)


answer   =   response.choices[  0  ].message.content
print  (  f  "  \n  💡 ANSWER:  \n{  answer  }  "  )
return   answer


except   Exception   as   e:
print  (  f  "❌ Error during search:   {  e  }  "  )
return   f  "Error during search:   {  e  }  "


# Process the three original questions with metadata filtering
print  (  "=== ENRON EMAIL ANALYSIS WITH METADATA FILTERING ===  \n  "  )


print  (  "📋 Question 1:"  )
answer1   =   answer_question_about_enron(
"What concerns did Sherron Watkins express to Ken Lay in her email about Enron''s accounting practices?"  ,
persons  =  [  "Watkins"  ,   "Lay"  ]
)


print  (  "  \n  "   +   "="  *  100   +   "  \n  "  )


print  (  "📋 Question 2:"  )


answer   =   answer_question_about_enron(
"How did David Delainey justify inflating Mariner''s valuation from $250M to $600M in his email to Ken Lay?"  ,
persons  =  [  "Delainey"  ]
)


print  (  "📋 Question 3:"  )


answer   =   answer_question_about_enron(
"How did Tim DeSpain coach Ken Lay on what to tell credit rating agencies about Enron''s financial condition?"  ,
organizations  =  [  "Moody"  ]
)
```


```text
===   ENRON EMAIL ANALYSIS   WITH   METADATA FILTERING   ===


📋 Question   1  :
🤔 Question: What concerns did Sherron Watkins express   to   Ken Lay   in   her email about Enron  ''  s accounting practices?
🔍 Filtering   by   persons: Watkins, Lay
🔍   SQL   Query:
SELECT
id,
chunk_content,
relevance,
metadata
FROM   enron_kb
WHERE   content   =   'What concerns did Sherron Watkins express to Ken Lay in her email about Enron''s accounting practices?'   AND   persons   LIKE   '%Watkins%'   AND   persons   LIKE   '%Lay%'
ORDER BY   relevance   DESC
LIMIT   100  ;


✅ Found   16   results


📧 Result #  1
🆔 Email ID: email_048101
📊 Relevance:   0  .  7053
📝   Subject  : The   key   questions I asked Lay   on   Aug   22
👥 Persons: Sherron S. Watkins, Lay
🏢 Organizations: Enron Corp.
📍 Locations:   None
💬 Content: Sherron S. Watkins Vice President, Enron Corp.   713  -  345  -  8799   office   713  -  416  -  0620   cell
------------------------------------------------------------


📧 Result #  2
🆔 Email ID: email_335299
📊 Relevance:   0  .  6832
📝   Subject  : TEAM   4   -   HR ENERGY COMMERCE SUBPOENA (  1  ) (  01  /  14  /  02  )   AND   (  2  ) (  12  /  10  /  01  )
👥 Persons: Ken Lay, Sherron Watkins, JEDI
🏢 Organizations: BLUE DOG, LJM2,   09   09The, the RAP TEAM, Enron
📍 Locations: V E, electr
💬 Content: Please search your files   and   collect all records   and   documents covered   by   o r relevant   to   the   following   requests: (  1  ) 09All records relating   to   any investigations  /  review of the allegations raised   by   S...
------------------------------------------------------------


📧 Result #  3
🆔 Email ID: email_178480
📊 Relevance:   0  .  6727
📝   Subject  : TEAM   4   -   HR ENERGY COMMERCE SUBPOENA (  1  ) (  01  /  14  /  02  )   AND   (  2  )
👥 Persons: Ken Lay, Sherron Watkins, JEDI, Bcc
🏢 Organizations: BLUE DOG, LJM2,  [email protected]  ,   minutes   ,   09   09The
📍 Locations: V E, electr
💬 Content: (  12  /  10  /  01  ) Cc:  [email protected]  ,   team  .  response  @enron.com Bcc:  [email protected]  ,   team  .  response  @enron.com We remain   in   the process of gathering information sought   by   various governm ental agen...
------------------------------------------------------------
🤖 Generating answer   using   GPT  -  4  ...


💡 ANSWER:
Sherron Watkins expressed concerns   to   Ken Lay about Enron  's accounting practices in her email, stating that she was "incredibly nervous that we will implode in a wave of accounting scandals." This concern was highlighted in an email discussing the broader context of Enron'  s financial issues,   where   it was noted that Andersen, the government,   and   Enron itself had access   to   financial   data   indicating the company  's potential collapse (Email ID: email_446563). Additionally, her concerns were significant enough to prompt investigations and reviews of the allegations she raised in her August memo to Ken Lay, as mentioned in emails discussing subpoenas and document requests (Email IDs: email_335299 and email_178480).


📋 Question 2:
🤔 Question: How did David Delainey justify inflating Mariner''s valuation from $250M to $600M in his email to Ken Lay?
🔍 Filtering by persons: Delainey
🔍 SQL Query:
SELECT
id,
chunk_content,
relevance,
metadata
FROM enron_kb
WHERE content = '  How did David Delainey justify inflating Mariner  ''  s valuation   from   $250M   to   $600M   in   his email   to   Ken Lay?  ' AND persons LIKE '  %Delainey%  '
ORDER BY relevance DESC
LIMIT 100;


✅ Found 100 results


📧 Result #1
🆔 Email ID: email_006062
📊 Relevance: 0.7015
📝 Subject: Mariner
👥 Persons: Delainey, Ken, Kase Lawal, Bcc
🏢 Organizations: un, IPO, Mariner, E P
📍 Locations: None
💬 Content: Cc:  [email protected]  ,  [email protected]   Bcc:  [email protected]  ,  [email protected]   Ken, in response to your note, I am not aware of any official dialogue with Mr. Kase Lawal abou...
------------------------------------------------------------


📧 Result #2
🆔 Email ID: email_400597
📊 Relevance: 0.7015
📝 Subject: Mariner
👥 Persons: Delainey, Ken, Kase Lawal, Bcc
🏢 Organizations: un, IPO, Mariner, E P
📍 Locations: None
------------------------------------------------------------


📧 Result #3
🆔 Email ID: email_275215
📊 Relevance: 0.7015
📝 Subject: Mariner
👥 Persons: Delainey, Ken, Kase Lawal, Bcc
🏢 Organizations: un, IPO, Mariner, E P
📍 Locations: None
------------------------------------------------------------
🤖 Generating answer using GPT-4...


💡 ANSWER:
David Delainey justified inflating Mariner'  s valuation   from   $250M   to   $600M based   on   several factors mentioned   in   the emails. According   to   the content of multiple emails (email IDs: email_006062, email_400597, email_275215, email_372317, email_280761, email_372732, email_332901), the justification included:


1  .   **  Successful Wells  **  : Mariner had enjoyed a series of successful wells that were expected   to   be booked   in   reserve reports   by   the   following   March.
2  .   **  Increases   in   Gas   and   Oil Prices  **  : There were significant increases   in   gas   and   oil prices, which contributed   to   the higher valuation.
3  .   **  Reserve Growth  **  : The reserve growth was a   key   factor   in   the increased valuation.
4  .   **  Current Energy Prices  **  : The current energy prices   at   the   time   supported   the higher valuation.
5  .   **  Future Goals  **  : The goal was   to   demonstrate three   to   four quarters of increasing operating cash flow   and   reserves growth   before   attempting further actions.


These factors collectively contributed   to   the stretch   target   valuation of $600M, which Delainey noted was   not   incredibly   out   of   line   given the circumstances.
📋 Question   3  :
🤔 Question: How did Tim DeSpain coach Ken Lay   on   what   to   tell credit rating agencies about Enron  ''  s financial condition?
🔍 Filtering   by   organizations: Moody
🔍   SQL   Query:
SELECT
id,
chunk_content,
relevance,
metadata
FROM   enron_kb
WHERE   content   =   'How did Tim DeSpain coach Ken Lay on what to tell credit rating agencies about Enron''s financial condition?'   AND   organizations   LIKE   '%Moody%'
ORDER BY   relevance   DESC
LIMIT   100  ;


✅ Found   100   results


📧 Result #  1
🆔 Email ID: email_028284
📊 Relevance:   0  .  6674
📝   Subject  : Yesterday s   Call  : Feedback
👥 Persons: Good Luck, Jeff   P  .  S  ., Cal Ed
🏢 Organizations: LJM, ENE, Moody s, Fastow, SEC
📍 Locations: Citi, Skilling
💬 Content: Ken, Thanks   for   having   the   call   yesterday. I am a believer   in   Enron   and   we are buying your debt. Here s short feedback   on   the   call  . I give the   call   a B  -/  C grade.   If   you want a good example of a compan...
------------------------------------------------------------


📧 Result #  2
🆔 Email ID: email_268144
📊 Relevance:   0  .  6636
📝   Subject  : Moody s Annual Review Meeting
👥 Persons: Jeff McMahon, Stephen Moore   -   Relationship, Foley, Tim, Ben
🏢 Organizations: Sierra Pacific, EBS, International Asset Sales, Wholesale Services, Moody s
📍 Locations: California
💬 Content: Director,   and   Stephen Moore   -   Relationship Manager (our analyst). Diaz   and   Moore are very familiar   with   the Enron credit   profile  . Foley   is   their boss. He apparently   is   the leader of their ratings comm...
------------------------------------------------------------


📧 Result #  3
🆔 Email ID: email_152456
📊 Relevance:   0  .  6528
📝   Subject  : Moody s   and   Standard   Poor s
👥 Persons: John Diaz, Ben, Bcc, Andy, Tim DeSpain
🏢 Organizations: Credit Ratings   -   emphasize, Moody s   Call  :,   Standard   Poor s, EBS, Dhabol
📍 Locations:   None
💬 Content: Cc:   ben  .  glisan  @enron.com,   andrew  .  fastow  @enron.com Bcc:   ben  .  glisan  @enron.com,   andrew  .  fastow  @enron.com Two conference calls have been tenatively scheduled   to   allow you   to   directly discuss Enron s   commit  ...
------------------------------------------------------------
🤖 Generating answer   using   GPT  -  4  ...


💡 ANSWER:
Tim DeSpain, along   with   Andy   and   Ben, coached Ken Lay   on   what   to   tell credit rating agencies about Enron  's financial condition by emphasizing several key assurances. According to email ID: email_152456 and email ID: email_372468, they advised Ken Lay to stress the following points:


1. **Commitment to Maintaining Credit Ratings**: They emphasized that maintaining credit ratings was critical to Enron'  s fundamental businesses, particularly gas   and   power marketing. They noted that   both   counterparties   and   creditors placed significant importance   on   Enron  's consistent rating profile.


2. **Strength of Core Businesses**: They highlighted that Enron'  s core businesses were strong, positioning Enron   as   the   leading   franchise   in   energy marketing. They anticipated continued strength   in   financial performance   from   the commodity groups.


These points were intended   to   assure the credit rating agencies of Enron  's financial stability and commitment to its credit ratings.
```


## **Key Achievements**


1.


**Automated metadata extraction** : By leveraging SpaCy’s NER models, we automatically extracted structured entities (people, organizations, locations) from raw email text, converting unstructured data into a hybrid storage system that supports both semantic and structured queries.


2.


**Hybrid search capabilities** : The knowledge base enables both soft search criteria (semantic similarity through embeddings) and hard search criteria (metadata filtering), allowing for precise and flexible information retrieval. This combination significantly enhances search accuracy and reduces irrelevant results.


3.


**Simplified query interface** : MindsDB abstracts away the complexity of vector databases, embedding models, and similarity calculations behind a familiar SQL interface. The addition of a simple` content` attribute in SQL SELECT statements makes semantic search accessible to anyone familiar with SQL.


4.


**Practical RAG implementation** : By integrating the knowledge base with a chat LLM, we’ve created a Retrieval-Augmented Generation (RAG) system that can answer complex questions by first retrieving relevant context and then generating informed answers, significantly reducing hallucinations.


## **Real-World Applications**


The techniques demonstrated in this tutorial have broad applications beyond the Enron dataset:


-


**Corporate knowledge management** : Search through internal documents, emails, and reports using both semantic queries and metadata filters


-


**Legal discovery** : Find relevant communications filtered by sender, recipient, date range, or mentioned entities


-


**Customer support** : Build intelligent support systems that can search through product documentation and past support tickets


-


**Research analysis** : Query academic papers, research notes, or experimental data with combined semantic and structured filtering


## **Next Steps**


To extend this project, consider:


-


**Expanding entity types** : Extract additional metadata such as monetary amounts, dates, or custom domain-specific entities


-


**Finetuning embeddings** : Use domain-specific embedding models for improved semantic matching in specialized fields


-


**Multi-modal knowledge bases** : Incorporate documents, images, and other file types into your knowledge base


-


**Advanced filtering** : Implement complex boolean logic and date-range queries for more sophisticated searches


-


**Production deployment** : Scale the system to handle larger datasets and concurrent users


## **Conclusion**


In this tutorial, we’ve successfully built a sophisticated question-answering system over the Enron email corpus by combining MindsDB’s Knowledge Base capabilities with Named Entity Recognition. This demonstrates how modern AI tools can transform unstructured text into a queryable, intelligent knowledge base.


For more information on MindsDB Knowledge Bases and advanced features, visit the[official documentation](https://docs.mindsdb.com/mindsdb_sql/knowledge_bases/overview) .


Watch the playback of the live webinar on youtube:
