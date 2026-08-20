---
schema_version: "1.0.0"
document_id: "03a480a1291a985d62c658350d0a1bf9e5dc93877c03cd0bdb49e59bb83d57b0"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/generating-bot-prompt-instructions-with-chatgpt"
published_at: "2023-01-23T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:cd8410b4eb16fa55f337d19548e9ff7e195e0aefe4dd6f2502e2050a7884318d"
---

# Generating bot prompt instructions with ChatGPT

If you have a scenario where you need to have your customer answer a standard set of questions, one way to do this is with a chatbot.


For example, say you’re a customer-facing agent working at a company that manufactures spacesuits. Below is a hypothetical conversation you might have with a customer:


Customer Heya, I need a spacesuit


You Great, we'd love to help. May I have our chatbot ask you a few standard questions, so we can get started?


Customer Of course!


You @SpaceSuitor


request suit details


SpaceSuitor $$$ Hello! What is your height?


Customer 110cm


SpaceSuitor What is your weight?


Customer 60kg


SpaceSuitor Are you right-handed?


Customer no


SpaceSuitor Are you planning any long-range spacewalks?


Customer yes


SpaceSuitor Are you planning on carrying weapons?


Customer yes


SpaceSuitor Thank you! $$$


You Thanks for this - our next printing cycle starts in two hours, so you can pick up your spacesuit this afternoon.


Customer Is a delivery option available?


You Of course! Mind if I have our bot collect delivery details?


Customer No problem


You @SpaceSuitor


request delivery details


SpaceSuitor $$$ Hello! What is your layer and quadrant?


... ...


To configure such a chatbot with Fogbender, you’ll need two things:


1.


A ChatGPT account - you can create one here:[https://chat.openai.com/chat](https://chat.openai.com/chat)


2.


A script with a list of questions that follow the rules below:


- Branching via “yes” and “no” answers only. For example, in the dialogue above, if the Customer answered “left” to the question “Are you right-handed?”, the bot would have replied “Sorry, I don’t understand ‘left’. I only understand ‘yes’ and ‘no’.”
- If you expect a multi-message answer to a particular question, you can indicate this in parentheses, along with a list of words the bot would use to move onto the next question (see example below)
- We use multiples of` =` instead of the more common` -` in conjunction with tabs/indentation to indicate nesting because many word editors turn consecutive` --` characters into m-dashes and because indentation may get collapsed during copy-pasting


Please use the following script as a model for yours:


```text
= What's your name?    = What year were you born?    = What are the cities and countries where you lived for more than a year? (Can have multiple responses; stop word: next, continue, or done)    = Do you like films?    = (if yes:)         == What are some of your favorite films? (Can have multiple responses; stop word: next)    = (if no:)         == How come? (Can have multiple responses; stop word: next)         == Do you know anyone else who doesn't like films?    = Do you play any musical instruments?    = (if yes:)         == Which ones? (Can have multiple responses; stop word: next)    = (if no:)         == Did you ever want to play one?         == (if yes:)           === Which one?    = Do you have a favorite non-dessert food?    = (if no:)         == How about a favorite non-dessert beverage?         == (if yes:)           === Which one?    = What's your second favorite US state?
```


Once you’ve got your script ready:


1. Enable and name your bot in the[Fogbender ai settings tab](https://fogbender.com/admin/-/-/settings/ai)
2. While there, add a new prompt and give your command a name (e.g. “request suit details” in first example)
3. Paste the ChatGPT instructions (below) to a new text document
4. Replace` PASTE YOUR SCRIPT HERE` in the instructions (below) with your script
5. Start a new ChatGPT chat, paste instructions, wait for ChatGPT to generate the JSON, copy the JSON
6. In Fogbender ai settings, paste the JSON into the “Instructions” textarea of your bot prompt


(NOTE: the JSON that ChatGPT produces might be malformatted - if that’s the case,click “Prettify JSON” in Fogbender after pasting)


```text
Consider the following set of questions:
Questions set 1: ###    = What is the OS of your device or system?    = Which device are you using?    = What is the Fogbender SDK version that are you using?    = Do you have a URL of a video showing the problem?    = (if yes:)        == What is the URL?    = (if no:)         == Please provide a clear and concise description of what you expected to happen. (Can have multiple responses; stop word: next)         == Please provide a clear and concise description of what's currently happening. (Can have multiple responses; stop word: next)         == Please provide the steps to reproduce the behavior. (Can have multiple responses; stop word: next)    = Did you try kicking a tire?    = (if yes:)         == Which tire?    = (if no:)         == Would you mind giving kicking a tire a shot?         == (if yes:)           === Make sure to kick the rear right one    = Do you have any additional commentary or context that you would like to provide about the problem? (Can have multiple responses; stop word: next or no)    ###
And the corresponding JSON structure:
JSON: ###    [         {           "question_id": 0,           "question_text": "What is the OS of your device or system?"         },         {           "question_id": 1,           "question_text": "Which device are you using?"         },         {           "question_id": 2,           "question_text": "What is the Fogbender SDK version that are you using"         },         {           "question_id": 3,           "question_text": "Do you have a URL of a video showing the problem?",           "on_affirmative": [             {               "question_id": 4,               "question_text": "What is the URL?"             }           ],           "on_negative": [             {               "question_id": 5,               "question_text": "Please provide a clear and concise description of what you expected to happen.",               "stop_words": ["next"]             },             {               "question_id": 6,               "question_text": "Please provide a clear and concise description of what's currently happening.",               "stop_words": ["next"]             },             {               "question_id": 7,               "question_text": "Please provide the steps to reproduce the behavior.",               "stop_words": ["next"]             }           ]         },         {           "question_id": 8,           "question_text": "Did you try kicking a tire?",           "on_affirmative": [             {               "question_id": 9,               "question_text": "Which tire?"             }           ],           "on_negative": [             {               "question_id": 10,               "question_text": "Would you mind giving kicking a tire a shot?",               "on_affirmative": [                 "question_id": 11,                 "question_text": "Make sure to kick the rear right one"               ]             }           ],         },         {           "question_id": 11,           "question_text": "Do you have any additional commentary or context that you would like to provide about the problem?",           "stop_words": ["next", "no"]         }    ]    ###
Can you build a similar JSON structure (with pretty formatting) for the following questions?
Questions set 2: ###         PASTE YOUR SCRIPT HERE    ###
```
