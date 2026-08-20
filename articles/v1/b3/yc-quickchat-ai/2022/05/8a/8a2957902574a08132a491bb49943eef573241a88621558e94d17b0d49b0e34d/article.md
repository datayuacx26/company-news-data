---
schema_version: "1.0.0"
document_id: "8a2957902574a08132a491bb49943eef573241a88621558e94d17b0d49b0e34d"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/how-to-add-an-ai-chat-bot-to-drupal-website"
published_at: "2022-05-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:bbf1b5f1c1c438aaa28c65be27dbb5f5162b045af8571caac54d6f3e6d1a5ccf"
---

# How to add an AI Chat Bot to your Drupal website

Alternatively, read documentation at[drupal.org/docs/contributed-modules/quickchat](https://www.drupal.org/docs/contributed-modules/quickchat)


## Where do I start?


1. Create an[account](https://app.quickchat.ai/) and purchase the Quickchat subscription.
2. Build your knowledge base (what do you want your AI to know?).
3. Test it using Preview in the[dashboard](https://app.quickchat.ai/) .
4. Once your AI is ready, submit it for review.
5. If you need a multi-lingual chat bot or a custom solution,[get in touch](https://quickchat.ai/contact) .


## Obtain token for Drupal integration


1. Activate Drupal integration
2. Copy your scenario_id and token


*Copy your scenario_id and token*


## Enable module


Enable the Quickchat module to provide the API client used by the sub-modules. No configuration is needed at this point.


*Enable the Quickchat module*


## Sync knowledge base items


### Configuration


This module will add a new content type quickchat_kb ( *admin/structure/types/manage/quickchat_kb* ) and a view quickchat_kb ( */admin/structure/views/view/quickchat_kb* ) so you can use them as a source to train your chatbots models.


You can configure the module from */admin/config/services/quickchat-api/sync* defining:


- A model name
- A scenario_id
- An API token
- A view name: the view to use as a source of knowledge base items (you can use the one provided by the module or a custom one).
- A view display: the display from the view name selected above.


*Configure Quickchat knowledge base view*


From that screen you can add multiple models, with a different pair of token and scenario_id and view source.


### View


In case you want to use a different view as source of knowledge base items be aware that if you want to split the results into different knowledge base items you need to use the \\n separator. Otherwise, the view results will be considered as only one knowledge base item.


*Configure Quickchat knowledge base view*


### Operation


Once you configured the sync model mappings the next step is to sync the knowledge base items for your chatbot models by going to */admin/content/kb*


There, you can see a list of the configured models:


*List of configured models*


If you click in the title of the model to sync you will see a page similar to this one:


*Model page*


In the preview section you can see the knowledge base items that you are about to sync. The values displayed there are the results of the view and view display that you selected under the model configuration section.


The available actions are Update and Rebuild:


*Knowledge base updated*


The *Update* action will push the knowledge base items to the selected model (scenario_id) and *Rebuild* action will retrain the chatbot with the updated knowledge base items.


*Knowledge base rebuilt*


## Add a Quickchat chatbot


The quickchat_chatbot sub-module adds a new block type chatbot_block where you can set the scenario_id.


*Configure Quickchat scenario_id*


Also, you can add multiple chatbot blocks with a different scenario_id for different type of pages, eg:


*Add multiple chatbots with different scenario_ids*


Once you enabled the block, visit the pages where the block should be available (visibility settings) so you can interact with the chatbot.


## That’s it! 🎉


Your Drupal integration is now ready. If you have any questions or require a more complex solution with more functionality, please do not hesitate to[get in touch](https://quickchat.ai/contact) .
