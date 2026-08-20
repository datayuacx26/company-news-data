---
schema_version: "1.0.0"
document_id: "9d82a8ba42d5c16cddb0571f060b2db301bfca6dd6c1a5d1c0dd268cba79d314"
company_key: "system1-inc-class-a-common-stock"
company: "System1 Inc."
source_id: "system1-inc-class-a-common-stock-news-import-6b4ffda1ae56"
canonical_url: "https://system1.com/blog/system1aichatbot"
published_at: null
first_seen_at: "2026-07-24T03:07:20.281825+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:368a637c7079a5548cac0872e743fdb134ddc100f2ce3eaf014857c90838afa5"
---

# System1 : Unlocking Information at System1: Deploying an AI-powered Slack Chatbot Using Snowflake

*This blog post was originally published on[Snowflake’s Medium .](https://medium.com/snowflake/efficiently-unlocking-information-at-system1-deploying-a-slack-chatbot-with-snowpark-container-53fa5b524ee8)*


**Authors:**


**[Gregory Baker](https://www.linkedin.com/in/gregory-baker-1575902/)** , System1 Data Engineering Manager


**[Zachary Reardon](https://www.linkedin.com/in/zack-reardon-45887321b/) ,** System1 Data Engineer


[Puneet Lakhanpal](https://www.linkedin.com/in/puneet-lakhanpal-3b49266/) , Snowflake Field CTO, Data Science


# Executive Summary


##### Listen to an AI-generated audio summary 👆


This blog highlights how System1 deployed a Slack Bot to enable bilateral communication on information stored in Confluence docs across a number of engineering, management, and science teams. The Slack bot was deployed on[Snowpark Container Services](https://docs.snowflake.com/en/developer-guide/snowpark-container-services/overview)


and uses Retrieval Augmented Generation (RAG) techniques with Snowflake’s Cortex managed service LLM[(llama 3.1–70b)](https://docs.snowflake.com/en/user-guide/snowflake-cortex/llm-functions#availability)


and Snowflake vector store embeddings[(nv-embed-qa-4)](https://docs.snowflake.com/en/sql-reference/functions/embed_text_1024-snowflake-cortex)


. This architecture is shown in Figure 1.


Figure 1: Reference Architecture


# Problem Statement


At its core System1 provides cohesive solutions to digital marketing by leveraging cutting-edge AI and Machine Learning technology. Key to this is the Responsive Acquisition Marketing Platform (RAMP), a closed-loop platform that integrates seamlessly with advertising networks to efficiently monetize internet traffic by allocating ads to the appropriate high intent customers.


Maintaining, optimizing, and improving RAMP requires bilateral input across a number of engineering, management, and science teams. Therefore, intra-team communication is essential and is often taken care of with dedicated Slack channels.


While these channels have been effective in providing designated lines of communication on a team-to-team or project-by-project basis, there are a few noticeable drawbacks.


- Questions directed towards individuals or teams can typically lead to delays between when a question is asked and an answer is provided, this is especially impactful in instances where the answer is contained within pre-existing documentation.
- Channels are left susceptible to cases where open questions can be lost or buried and an answer is not guaranteed.
- Users may direct questions and/or answers to individuals when the requested information could be helpful for others outside of the conversation or channel.


As long as information is recorded, there should be a way to get that information into the hands of whoever needs it. Fortunately many of the workspaces that companies use, including Confluence which we will use as an example throughout this piece, allow for this information to be extracted and transported quickly and efficiently using an API. But how do we know what someone wants?


LLMs have proven to be effective at interpreting and answering these questions. However, the likelihood of an off-the-shelf LLM regurgitating terminology specific to your company or spitting out specifics from your last meeting notes is obviously pretty low. We can solve for this using Retrieval-Augmented Generation (RAG), a process wherein we provide the pre-trained model with additional company-specific content that it can reference to help curate an appropriate answer.


This piece will cover the process of extracting content from documents within a Confluence space, providing useful content to a LLM for reference, and relaying a suitable answer to a user’s question in Slack. By gathering related information from Confluence and generating answers on the fly using an LLM we can improve on the current approach to intra-team communication in several ways.


- Users can get their questions answered almost immediately without worrying about them getting lost or ignored.
- Users can obtain information contained in Confluence without having to sift through folders and individual documents.
- Everyone is able to see the Slack bot’s response in the channel regardless of whether they were the one asking the question or not. Since the bot pulls information from company documentation, the answer is often pertinent or useful to everyone in the channel.


# Chunking, Vectorizing, and Storing Documents


First, we must consolidate the information contained within company documentation. In this example we’ll demonstrate the process of collecting, chunking, vectorizing, and finally storing pages within a requested Confluence space.


Since the Atlassian Python API includes a Confluence module, the process of programmatically accessing Confluence documents is relatively quick and painless.


You can begin by downloading the API using the console command pip install atlassian-python-api. The Confluence object, required for access to the Confluence workspace, can then be created as so.


> ```text
> from   atlassian  import   Confluence
>
>
> # connecting to Confluence
> confluence_object = Confluence(
> # adjust parameter strings to match your own credentials
> url= "https://company.atlassian.net"  ,
> username= "user@company.com"  ,
> password= "password"  ,
> )
> ```


Note: The parameters passed in here are examples, you should replace the hostname, username, and password with you or your company’s credentials. Consider using a secrets manager to store credentials such as these and accessing them programmatically for increased security.


In order to collect the documents in the hypothetical “Documentation” Confluence space, we can iterate through them and append to a list. It is important to note that the iteration is necessary since Confluence API call returns are limited to 100 pages at a time.


> ```text
> # collecting all documents in specified space
> collected_docs = []
>
>
> start =  0
> pages =  True
> while   pages:
> pages = confluence_object.get_all_pages_from_space(
> space= "Documentation"  ,  # Replace with the name of your Confluence space
> start=start,
> limit= 100  ,
> status= None  ,
> expand= "space,body.storage,version"  ,
> content_type= "page"  ,
> )
> if    len  (pages) ==  0  :
> pages =  False
> else  :
> start +=  100
> collected_docs += pages
> ```


The idea is to land the collected documents in a dedicated Snowflake table. Be sure to create a table ahead of time using the SnowSQL CLI or Snowsight UI. Alternatively, you can create the table as needed using an additional query in Snowpark. In this example, a table has been created ahead of time. Extracting the necessary fields and converting our list of collected documents to a pandas dataframe will make it easier to insert the data into Snowflake down the road. We also recommend using BeautifulSoup to clean the content body.


> ```text
> import   pandas  as   pd
>
>
> # selecting desired fields
> docs_to_insert = []
> for   doc  in   collected_docs:
> docs_to_insert.append(
> [
> doc[ "id"  ],
> doc[ "title"  ],
> doc[ "version"  ][ "when"  ],
> doc[ "version"  ][ "by"  ][ "displayName"  ],
> doc[ "body"  ][ "storage"  ][ "value"  ],
> ]
> )
>
>
> # example of conversion into pandas dataframe
> pandas_df = pd.DataFrame(
> docs_to_insert,
> columns=[
> "ID"  ,
> "TITLE"  ,
> "UPDATED_AT"  ,
> "AUTHOR"  ,
> "TEXT"  ,
> ],
> )
> ```


Now that the data is standardized in a pandas dataframe, there is just one more step to consider before we can insert into our Snowflake table. Separating the content out into chunks helps to ensure that each unit of context we feed to the model will not exceed a certain character length. We used langchain’s RecursiveCharacterTextSplitter to break the content bodies down into 1400 character chunks with 100 character overlap, however there are many ways to approach this. Try playing around with different character limits, overlaps, or other text splitters to find which works best for you.


Once your pandas dataframe is separated out into the desired chunks on a row-by-row level, we can insert this data into our Snowflake table. The Snowpark pandas API makes this process incredibly easy, just create a snowflake session and use the session.write_pandas operation like shown.


> ```text
> from   snowflake.snowpark  import   Session
>
>
> # connecting to Snowpark session
> connection_parameters = {
> # adjust parameter strings to match your own credentials
> "ACCOUNT"  :  "account_name"  ,
> "USER"  :  "username"  ,
> "PASSWORD"  :  "password"  ,
> "DATABASE"  :  "DATABASE_NAME"  ,
> "SCHEMA"  :  "SCHEMA_NAME"  ,
> }
>
>
> snowpark_session = Session.builder.configs(connection_parameters).create()
>
>
> # writing dataframe to Snowflake table
> snowpark_session.write_pandas(
> pandas_df,  "SNOWFLAKE_VECTOR_STORE_TABLE"  , auto_create_table= False
> )
> ```


Note: The credentials and parameters passed in our examples, please replace with your own credentials. If you have not created your Snowflake table yet, just set auto_create_table to True.


To vectorize these chunks and finish out our vector store add a “VECTORIZED_CHUNK” or similarly named column to our Snowflake table. Make sure the data type for this new column is VECTOR (check out[https://docs.snowflake.com/en/sql-reference/intro-summary-data-types](https://docs.snowflake.com/en/sql-reference/intro-summary-data-types)


to see all available data types). Our chunk column can then be vectorized and populated into this new column using a simple SQL query in Snowflake.


> ```text
> snowpark_session.sql( """
> UPDATE SNOWFLAKE_VECTOR_STORE_TABLE
> SET VECTORIZED_CHUNK = SNOWFLAKE.CORTEX.EMBED_TEXT_1024('nv-embed-qa-4',CHUNK)
> WHERE VECTORIZED_CHUNK IS NULL
> ;"""
> )
> ```


Now our vector store has been populated. You can also consider populating this table regularly if your Confluence space has frequent document additions or changes. In our case we’ve found it helpful to ingest documents that have been updated after the prior ingest. Scheduling and monitoring abilities can be added and managed via Airflow or another orchestrator.


# LLM and RAG-assisted Answer Generation


With the reference data in place, the next stage is to build the answer generator. The base LLM we used is llama 3.1–70b, however Snowflake Cortex offers a number of models to select from.


Since our use case involves asking the bot questions and returning answers that reference the data in our vector store, we can begin by directing the LLM towards an understanding of its use case. Something like…


> ```text
> prompt =  f"""
> 'You are an assistant that extracts information from the context provided.
> Answer the question asked to you.
> The context may be helpful in generating an appropriate answer.
> The context may include information which is unique to the company.
> Please answer clearly and naturally.'
> Context:  {context}
> Question:  {question}
> Answer:
> """
> ```


Notice that “context” and “question” represent variables. We will insert the question asked by the user in place of the “question” variable and “context” will consist of chunks determined to be similar to the question asked. All new information provided to the base LLM will be provided within this prompt text. This technique does not require any fine-tuning or re-training of the base model.


Let’s choose to arbitrarily extract the 20 most similar chunks to the question to feed as context. When implementing your own RAG process, test different numbers of chunks to see which performs best. To calculate the similarity between the question asked and the chunks in our vector store, we will use the Snowflake VECTOR_COSINE_SIMILARITY function. We can convert the output to a pandas dataframe with to_pandas.


> ```text
> retrieve_chunks =  f"""
> SELECT CHUNK FROM
> VECTOR_COSINE_SIMILARITY(vectorized_chunk,SNOWFLAKE.CORTEX.EMBED_TEXT_1024('nv-embed-qa-4',?)) as similarity, chunk
> FROM EXAMPLE_SNOWFLAKE_TABLE
> LIMIT ?
> ;"""
>
>
> df_context = snowpark_session.sql(
> retrieve_chunks, params=[question,  20  ]
> ).to_pandas()
> ```


From this dataframe we can create the context value that will be fed into the prompt.


> ```text
> context_length =  len  (df_context) -  1
>
>
> context =  ""
>
>
> for   i  in    range  ( 0  , context_length):
> context += df_context._get_value(i,  "CHUNK"  )
>
>
> context = prompt_context.replace( "'"  ,  ""  )
> ```


This can all be pieced together into a single function that takes a question and returns an answer using Snowflake CORTEX.COMPLETE.


> ```text
> def    answer_question  ( question  ):
>
>
> prompt =  f"""
> 'You are an assistant that extracts information from the context provided.
> Answer the question asked to you.
> The context may be helpful in generating an appropriate answer.
> The context may include information which is unique to the company.
> Please answer clearly and naturally.'
> Context:  {context}
> Question:  {question}
> Answer:
> """
>
>
> retrieve_chunks =  """
> SELECT CHUNK FROM
> FROM EXAMPLE_SNOWFLAKE_TABLE
> LIMIT ?
> ;"""
>
>
> df_context = snowpark_session.sql(
> retrieve_chunks, params=[question,  20  ]
> ).to_pandas()
>
>
> context_length =  len  (df_context) -  1
> context =  ""
> for   i  in    range  ( 0  , context_length):
> context += df_context._get_value(i,  "CHUNK"  )
> context = context.replace( "'"  ,  ""  )
>
>
> complete =  """
> SELECT SNOWFLAKE.CORTEX.COMPLETE('llama3.1–70b', ?) AS response
> ;"""
>
>
> df_response = snowpark_session.sql(complete, params=[prompt]).collect()
>
>
> response =  ""
> response += df_response[ 0  ][ "RESPONSE"  ]
>
>
> return   response
> ```


We have created a function using Snowpark that will take a question as input, select the 20 most similar chunks from our vector store as context, and reference that context to help generate an answer. You can try feeding different questions to this function and printing the answer. We now have a RAG-assisted LLM capable of answering questions using company documents. We don’t yet have a Slack chatbot.


# Relaying to Slack Bolt App


Ultimately, what we want is for a question asked in a Slack channel to be fed to the answer generation function we created above, and the response to be returned as a reply to that initial message. A Slack bot is an effective means to accomplish this.


You can create your own Slack app at[https://api.slack.com/apps](https://api.slack.com/apps)


.


When configuring your app to act as a chatbot, there are a few important selections to make.


- Create an App-Level Token with scope connections:write. This will generate a token that will be useful later.


- Make sure Socket Mode is toggled on. This will allow the app to communicate with Slack without using public HTTP endpoints. Socket Mode alongside the Bolt framework, which we will use here, is also incredibly easy to use and implement.


- Enable Event Subscriptions and subscribe to the app_mention bot event. This will allow our bot to be notified when a Slack message includes the bot’s handle.


- Add the necessary Bot Token Scopes including app_mentions:read and chat:write. These will give your bot permission to read content from and write content to Slack.


Once your app is set up in the Slack API UI, you can install (or request to install) it to your company workspace.


Next, we can create our Bolt app in Python.


The app will use the slack_bolt library and will be configured to handle Slack mentions of your app’s name. We can create the app by providing the Bot User OAuth Token. This can be found under OAuth & Permissions.


> ```text
> from   slack_bolt  import   App
> from   slack_bolt.adapter.socket_mode  import   SocketModeHandler
>
>
> # replace slack_bot_token with the Bot User OAuth Token
> app = App(token= "slack_bot_token"  )
> ```


Note: As mentioned before, consider storing tokens such as the Bot User OAuth Token in a secrets manager.


In order for the app to interact with Slack each time its handle is mentioned within a channel, we will create an “app_mention” event. Each time this event takes place, our answer_question function that we defined above will be called.


> ```text
> @app.event( “app_mention”  )
> def    mention_handler  ( body, say  ):
> message_id = body[ "event"  ][ "ts"  ]
> answer = answer_question(question)
> say(answer, thread_ts=message_id)
> ```


To start running the Bolt app we’ll define the handler and start. On handler creation, we’ll feed the app we created as well as the App-Level Token we generated earlier to SocketModeHandler.


Consolidating all of this down into a simple script yields:


> ```text
> from   slack_bolt  import   App
> from   slack_bolt.adapter.socket_mode  import   SocketModeHandler
>
>
> app = App(token= "slack_bot_token"  )  # replace with the Bot User OAuth Token
>
>
> @app.event( "app_mention"   )
> def    mention_handler  ( body, say  ):
> message_id = body[ "event"  ][ "ts"  ]
> answer = answer_question(question)
> say(answer, thread_ts=message_id)
>
>
> # replace slack_app_token with the App-Level Token
> handler = SocketModeHandler(app,  "slack_app_token"  )
> handler.start()
> ```


Execution of this in Python starts and runs the Bolt app which will generate an answer to a question from Slack whenever the app’s name is mentioned.


Each time a Slack user mentions the app’s name, the message, as well as similar chunks from our vector store, will be fed into the base LLM which has been instructed to extract information from those chunks. The information will then be referenced by the model as it generates and returns an answer. That answer will then be populated back over to Slack as a reply to the initial question that was asked.


# Deployment of Slack Bolt App on Snowpark Container Services


Snowpark Container Services (SPCS) is a managed, container orchestration platform hosted within Snowflake. We can host and orchestrate containers via a concept called Services. These are hosted on compute pools, which comprise an elastic compute layer of managed compute, including CPU or GPU virtual machines.


For deploying the Slack app on SPCS, we start off by defining a docker file and building/pushing the docker image into SPCS.


> ```text
> FROM python:3.11-slim
> WORKDIR /app
> COPY ./requirements.txt /app/requirements.txt
> RUN pip install - upgrade pip && \
> pip install -r requirements.txt
> COPY ./main.py /app/main.py
> CMD ["python main.py"]
> ```


We can build, tag and push this image into SPCS using the commands below.


> ```text
> export DOCKER_BUILDKIT=1
> docker build - platform linux/amd64 -t $(SS_SERVICE_NAME) -f Dockerfile .
> docker tag $(SS_SERVICE_NAME) $(IMAGE_REGISTRY)/$(SS_SERVICE_NAME)
> docker push $(IMAGE_REGISTRY)/$(SS_SERVICE_NAME)
> ```


Next, we define a YAML specification file and put it into a Snowflake stage either using SnowSight UI or SnowSQL. Within this YAML file, we can provide the image registry url definition of the container that we want to use and also pass in slack bot and app tokens as environment variables.


> ```text
> spec:
> containers:
> -    name:    bot
> image:    /<database>/<schema>/<image_repo>/<container_name_with_tag>
> env:
> slack_bot_token:    "XXX"
> slack_app_token:    "XXX"
> ```


Lastly, we will create a SPCS compute pool and a service to deploy the Slack Bolt App on SPCS.


> ```text
> CREATE   COMPUTE POOL IF  NOT    EXISTS   SLACK_BOT_POOL
> MIN_NODES  =    1
> MAX_NODES  =    1
> INSTANCE_FAMILY  =   CPU_X64_M;
>
>
> create   service SLACK_BOT_SERVICE
> in   compute pool SLACK_BOT_POOL
> from    @specs
> specification_file =  'slackbot.yaml'
> external_access_integrations  =   ( <  EXTERNAL_ACCESS_INT_IF_REQUIRED >  );
>
>
> /  /  check   service status
> CALL    SYSTEM  $GET_SERVICE_STATUS(SLACK_BOT_SERVICE);
> /  /  check   service logs
> CALL    SYSTEM  $GET_SERVICE_LOGS(SLACK_BOT_SERVICE,  '0'  ,  'bot'  , 1000  );
> ```


This completes the setup of the Slack bot on SPCS.


# What’s Next?


Regardless of which workspace or workspaces you or your company prefers to use, you can use a similar process to populate your vector store. Thus the most apparent next step would be to add ingestion support for new sources. Most workspaces have APIs similar to Atlassian that can be used to fetch documents and their content. By creating a pandas dataframe from each new source, we can simply populate our Snowflake tables using Snowpark’s write_pandas feature as we did with Confluence. More sources allow for more data within the vector store and more information for our LLM to extract to help answer more company questions.


Improvement isn’t only about expanding the available data the LLM has on hand, by modifying the prompt fed to the model to fit our needs we can tune the output to fit the desired audience. For instance, if an engineering team’s line of questioning in Slack typically involves looking for helpful SQL queries to fit their needs, the model can be told to use the context provided to help generate an answer that includes a SQL query. If the audience isn’t as technical, the model can be explicitly told not to produce code as part of the answer or it can be told to answer at a high-level business standpoint. Prompt engineering like this can be tricky to nail down but can be helpful in tuning the behavior of a model to better match its use case.


From a DevEx standpoint, the quick-fire Q&A approach rendered by a Slack chatbot such as this can also reduce the cognitive load exerted on developers. On the spot information extraction can speed up the debugging process significantly. Dense technical documentation can be parsed and summarized almost immediately. Engineers can spend less time answering questions or pointing each other to code and spend more time doing what they do best, engineering. While it isn’t necessarily everyone’s favorite thing to do, maintaining and updating documentation pays dividends when it comes to RAG. A model is only as good as the data provided to it. Everyone stands to gain from well-kept documentation, from the business to our RAG chatbot to the developers who work with it.


The above examples mention ways that we can make the chatbot more performant and suitable for different business use cases, but they are by no means the only ones. As long as the AI field continues to develop, System1 will look for ways to use these developments in novel and effective ways. Snowpark Container Services and Snowflake Cortex make it easy to implement and scale these new technologies, helping us stay on top of our data and keep pace with ever-evolving AI innovations.


### Next post


[MapQuest is back! Here’s How We Protect Your Privacy with New Private Maps App](https://system1.com/blog/privatemapsbymapquest)


#### System1


System1
