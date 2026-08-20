---
schema_version: "1.0.0"
document_id: "627e94239fafd78b941365b9b8a9a866c96e07d494a27c50e1823685ccc4b82d"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/synthetic-data-generation-instructor-typescript"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:0a0cda2bd0117d2c3df114542a7b527421f089fecea194731c836f5e493ca066"
---

# Synthetic Data Generation for Q&A Tasks

\[Joschka Braun\]([https://joschkabraun.coml](https://joschkabraun.coml/) 2, 2024


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting)


Synthetic data are a great way to bootstrap test data for an AI application if no real production data are available or cannot be used. In this tutorial, we will use[Instructor in TypeScript](https://instructor-ai.github.io/instructor-js/) to generate synthetic data for a question-answering task. Instructor is designed to simplify generating structured responses from LLM APIs. It does that by patching the respective model provider API client and using features such as tool use, function call or JSON mode. It will then use user-specified Zod schemas to generate structured responses from the model.


To install instructor, zod and openai, run the following command:


```text
pnpm   add   @instructor-ai/instructor   zod   openai


```


Using instructor amounts to three steps:


1. Patch the OpenAI client with Instructor
2. Define a Zod schema for the structured response
3. Call the` chat.completions.create` method with the Zod schema as the` response_model`


Patching the OpenAI client is easy with Instructor:


```text
import   Instructor   from   "@instructor-ai/instructor"  ;
import   OpenAI   from   "openai"


const   oai   =   new   OpenAI  ({
apiKey:   process  .  env  .  OPENAI_API_KEY   ??   undefined  ,
organization:   process  .  env  .  OPENAI_ORG_ID   ??   undefined
})


const   client   =   Instructor  ({
client:   oai  ,
mode:   "TOOLS"
})


```


Next we will define a Zod schema for the question-answer pairs and require that the answer is a number (to simplify using it to test our application).


```text
import   {   z   }   from   "zod"


const   QuestionAnswerPair   =   z  .  object  ({
question:   z  .  string  ().  describe  (  ""  ),    // optionally use descriptions to steer the synthetic data generation
answer:   z  .  number  ().  describe  (  ""  )
})


```


Note, we can optionally specify descriptions for each attribute which help steering the synthetic data generation and will be used in the prompt. In order to generate a list of question-answer pairs, we define a schema for the list:


```text
const   QuestionAnswerPairsSchema   =   z  .  object  ({
pairs:   z  .  array  (  QuestionAnswerPair  )
});
type   QuestionAnswerPairs   =   z  .  infer  <  typeof   QuestionAnswerPairsSchema  >


```


Finally, using Instructor to generate the synthetic data is as simple as calling the` chat.completions.create` method and specifying the Zod schema as the` response_model` . We will get back a response of type` QuestionAnswerPairs` and can save that as our Q&A dataset.


```text
const   generateQuestionAnswerPairs   =   async   (  count  :   number  ,   topic  :   string  )  :   Promise  <  QuestionAnswerPairs  >   =>   {
return   await   client  .  chat  .  completions  .  create  ({
messages:   [{  role:   "user"  ,   content:   `Generate   ${  count  }   question answer pairs on   ${  topic  }  .`  }],
model:   "gpt-4o"  ,
response_model:   {  schema:   QuestionAnswerPairsSchema  ,   name:   "QuestionAnswerPairs"  },
temperature:   0.0  ,    // increase the temperature to get more creative responses
max_tokens:   1024  ,
})
}


```


This will generate` count` Q&A pairs on the topic` topic` and return them as a structured response. To customize this to your use case, you will want to adapt this prompt to include information on your use case.


## ​


Fully Working Code


Below you can see the fully working code:


```text
// pnpm add @instructor-ai/instructor zod openai
import   Instructor   from   "@instructor-ai/instructor"  ;
import   OpenAI   from   "openai"
import   {   z   }   from   "zod"
import   *   as   dotenv   from   "dotenv"  ;


dotenv  .  config  ();


const   oai   =   new   OpenAI  ({
apiKey:   process  .  env  .  OPENAI_API_KEY   ??   undefined  ,
organization:   process  .  env  .  OPENAI_ORG_ID   ??   undefined
})


const   client   =   Instructor  ({
client:   oai  ,
mode:   "TOOLS"
})


const   QuestionAnswerPair   =   z  .  object  ({
// can optionally use descriptions which will be used in the prompt
question:   z  .  string  ().  describe  (  ""  ),
answer:   z  .  number  ().  describe  (  ""  )
})
const   QuestionAnswerPairsSchema   =   z  .  object  ({
pairs:   z  .  array  (  QuestionAnswerPair  )
});


return   await   client  .  chat  .  completions  .  create  ({
model:   "gpt-4o"  ,
temperature:   0.0  ,
max_tokens:   1024  ,
})
}


const   main   =   async   ()   =>   {
const   count   =   5  ;
const   topic   =   "soccer"  ;


const   qaPairs  :   QuestionAnswerPairs   =   await   generateQuestionAnswerPairs  (  count  ,   topic  )
console  .  log  (  qaPairs  )
}


main  ().  then  (()   =>   console  .  log  (  "Done!"  ));


```
