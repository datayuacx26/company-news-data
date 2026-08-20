---
schema_version: "1.0.0"
document_id: "f162844061862407420105f8620961915afddd14dc25339e332bcafcd58d5ff0"
company_key: "ww-international-inc-common-stock"
company: "WW International Inc."
source_id: "ww-international-inc-common-stock-rss-f4e3db081dfb"
canonical_url: "https://medium.com/ww-tech-blog/data-science-capstone-projects-8273a437c8db"
published_at: "2023-05-15T18:03:42+00:00"
first_seen_at: "2026-07-20T23:20:13.695109+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:666642ba846b2e0b8f4cb33ebecc7c8958432afe170c178a92019bbde1b6f909"
---

# Data Science Capstone Projects

# Data Science Capstone Projects


[Betsey Corey](https://medium.com/@betsey.corey?source=post_page---byline--8273a437c8db---------------------------------------)


10 min read


·


May 15, 2023


--


This spring semester the Data Science team at WeightWatchers welcomed back a group of Business Analytics Masters students from Columbia University to participate in two capstone projects. WW proposed two topics, Voice Controlled Food Tracking, and Food Image Generation, in return eight students selected WeightWatchers and were split between the two topics to research and ultimately present their findings back to our organization at the end of their spring semester.


The project entailed weekly meetings between the students and the Data Science project lead and mentor to ensure guidance around project parameters and to create a sense of community and connectivity throughout. With a visit to our headquarters to attend one of our global town hall meetings and feel a sense of community, the community that we were founded on, 60 years ago!


At the conclusion of the project, the students shared back their findings internally with our tech and product organization and at our virtual hackathon. The article below highlights one of the team’s research around Image Generation with Stable Diffusion. Thank you to the students for their research and efforts, we enjoyed having you and look forward to welcoming future capstone students from Columbia again. Please read on to hear more about the project in their own words.


*Betsey Corey, Director, Strategic Talent Programs & Partnerships*


## **Image Generation with Stable Diffusion**


One of our favorite parts of the WeightWatchers app is the recipe recommendation section, where one can browse 12,000+ delicious (and healthy) meals. Each recipe is accompanied by beautiful, high-resolution images, which are often professionally created by WW. We tried to generate such images through[latent diffusion models](https://huggingface.co/blog/stable_diffusion) by simply giving the model a prompt, for example, “pasta puttanesca”. The recent release and rapid progression of diffusion models inspired this project and we sought to use stable-diffusion-v2–1 to generate pictures that resembled professional images as closely as possible.


**Implementing the model & initial results**


Getting things set up and generating the first images is very straightforward. We found running the model on a local or cloud GPU to speed things up considerably, especially when generating many images. At its most basic implementation, stable-diffusion-v2–1 only needs a prompt with a random seed being generated every time the model is run.


When experimenting with different recipes and food items, the model generated pictures of widely differing quality. Some prompts, such as full meal names (e.g., “pasta puttanesca with basil, black olives and tomatoes”) worked considerably better than single foods (e.g., “an apple”). Further, since most models generate a random seed each time the model is run, the same prompt will lead to vastly different outputs. This makes a rigorous evaluation more challenging. Here are some initial examples of images we generated:


Press enter or click to view image in full size


(results for “clean pink table with a white round plate of chopped salad with sweet red pepper garlic clove \[…\] string beans in the middle”)


Press enter or click to view image in full size


(results for “apple”)


While these results are not terrible, they are still far from the professional benchmark we are comparing them to. The most noticeable deficiency at this stage was the appearance of unwanted items, such as hands, fingers, or cutlery, appearing on pictures despite the fact that our prompts did not include them. This led us to explore options to constrain the model or further specify instructions.


**Improving the output**


After some research and quite a bit of trial and error, we implemented the following measures to improve the outputs. First, we fed a list of negative prompts to the model, i.e. items that should be excluded from appearing. In addition, we varied the degree of complexity or detail of the prompts. Simply prompting with “Apple” yields different results than “high-resolution, shiny, centered Apple”. We tried to systematically determine whether complexity and detail in the prompt improved the images. We varied the seed and complexity of prompts for a given food item and created numerous images. Below, results for “banana” and “olive oil” are shown. Note that the complexity scale is arbitrary and subjective. Low complexity is something like “banana” while level 5 complexity is more akin to: “DSLR food photograph of normally shaped croissant, in a circular blue plate, white napkin background, light coming from the side, natural lighting, masterpiece, 4K, 85mm f1.8”.


Press enter or click to view image in full size


Press enter or click to view image in full size


This analysis showed that depending on the food, differing complexity levels of the prompts yielded different image qualities. To generate a realistic Apple or Banana, giving details such as “DSLR food photograph” and “high-resolution” proved to be effective. For items such as olive oil or salt, simplicity in the prompt was key.


The last avenue we have so far pursued is to use[ControlNet](https://github.com/lllyasviel/ControlNet/blob/main/README.md) which allows one to input a sketch and prompt, based on which the model will output an SD-generated image. This allows us to specifically draw out the position and shape of foods, which helps to constrain the outputs. See below for an example:


Press enter or click to view image in full size


Looking forward, we seek to test some further ideas for improved prompt quality, such as using models like[BLIP](https://huggingface.co/docs/transformers/model_doc/blip) , which generate a prompt given an image. By feeding very high-quality images to such a model, we might be able to gain insight into how to phrase the prompts better.


**Lessons learned**


While the images we are currently generating are still not quite where we want them to be, the work with stable diffusion has taught us some useful lessons:


- When coming up with prompts, no one size fits all. Depending on the food to be generated, the prompt needs to be customized in order to get the best results.
- Working with negative prompts makes life considerably easier! Especially when seeking to eliminate elements that keep on popping up.
- Things move incredibly fast and each day new models are being published and resources made available. Staying up to date on Reddit, Discord and Huggingface has proven incredibly helpful.


For inspiration, here are a few of the most recent images we generated:


## **Voice Controlled Food Tracking**


**Background**


Developing a food-tracking habit is consistently shown to be the most important factor in weight loss. WW is constantly looking for ways to make food tracking easier and exploring the use of AI tools to create better food tracking experiences. In this capstone project, our team built a website where people can talk about the food they eat, and the website can extract the food entities from the audio, matching with the current WW database, and returning the points.


The current way to track food on the Weight Watcher app is to manually search for the food that you eat for every meal. However, this process could become time-consuming and inefficient. For instance, Sweetgreen is one popular option among current customers. However, when it comes to a customized bowl, it could easily contain more than 10 different ingredients. This kind of complex real-life scenario deters users from tracking food consistently and accurately when they need it most. Taking images of food is a standard industrial solution and also might be your first thought. However, images could encounter problems as well. A human or model may be able to identify a casserole from an image, but this does not provide enough information to determine how we should track its nutritional value, which can vary widely depending on the ingredients used.


On the other hand, talking is a low-friction activity. People love to talk about food and can describe a whole meal in a few seconds. We believe that voice is the lowest friction method to track food. We also believe that the latest advances in open source and multimodal AI models have put “voice-to-track” within reach. In the rest of the blog, we will introduce it to you in more detail.


**App Structure**


Press enter or click to view image in full size


Let’s take a closer look at the app structure and tech stack. At a very high level, the app is mainly coded in Python. We utilized FastAPI, used state-of-the-art models from open source communities, which are mainly OpenAI and Hugging Face, and deployed with docker. For the front end, we used Streamlit, which is a free and open-source framework to rapidly build and share beautiful data science web apps.


To transcribe a user’s audio describing the food to text, we used the Whisper model from OpenAI. To extract food names from the transcribed text, we tried both food-based BERT and GPT API. Next, we encoded the found food names in embeddings with SBert and then compared their similarity with encoded food names in the WW database using FAISS. Finally, we return the most related food names in the database to the front end.


**App Demo**


The figure below is a screenshot of the user interface of our app. First, users would be asked to record their voice by clicking on the microphone icon. Once the recording is successful, users would click on the “Detect” button to initiate the audio transcription, food extraction and matching process. The final matchings along with their respective portions, units, and points would be returned to the user and the user would provide feedback on the accuracy of the results by either selecting “Yes!!” or “No, this is not what I ate!” to answer the question “Is this what you ate?”. If the latter is selected, the user could choose to manually type in what they ate in the text box and new food extractions as well as final matchings would be generated based on the input in the textbox. Given that the matching is correct, the user could then click on the drop down menu on the rightmost column to adjust the amount of food intake as well as the unit. In addition, we also allow users to choose OpenAI’s GPT 3.5 Turbo as an alternative for Food-Based BERT to perform food entity extraction. Users could switch to this option by selecting the “OpenAI GPT 3.5-turbo” button and inputting their OpenAI API key in the leftmost column. Overall, our clear and interactive user interface allows users to track their daily food intake in an efficient way and provides highly customized results.


Press enter or click to view image in full size


App User Interface


**User Expression Pattern Research**


Having our model at hand, we also conducted research on the user side to gain a deeper understanding of how people would use our app. We collected 112 recordings of people describing what they ate on the Columbia University morningside campus and performed analysis on these data collected in an attempt to find patterns in user expressions. The insights we gained from these real-life user data are as follows:


- Insight 1: Users almost always use ‘a’ to specify a single item but they tend to use ‘some’ when they had more than one item rather than specifying the specific amount
- Insight 2: When people describe what they ate, the name of the restaurant/ store is often neglected
- Insight 3: “and” usually connects two food entities whereas “with” might connect two food entities (e.g. toast with tea) or might be used when the user wanted to provide a more detailed description of the ingredients inside a food (e.g. coffee with oat milk/ bagel with cream cheese)
- Insight 4: Nouns (mostly foods) occur most frequently in people’s descriptions, with an overall frequency of 0.24. Better prompts might reduce the percentage of redundant information such as adjectives and adverbs.


**Results**


The accuracy of food entity extraction was compared between OpenAI GPT method and Food-Based BERT method. Out of all 112 collected transcribed texts, the Food-Based BERT method correctly extracts all food entities from 56.8% of the sentences, while the GPT method’s performance is 81.1%. Across all sentences, the Food-Based BERT method correctly detects 76.6% of entities, compared to 93.4% using the GPT method.


Press enter or click to view image in full size


*Comparison of NER accuracy*


**Next Steps**


Potential future improvements that could be made to our app include running Whisper model on GPU to increase speed, finding a better name entity recognition model in terms of accuracy and speed, and adding aesthetic elements to our user interface. The next step would be to integrate our model with the WW app to provide users with this time-saving alternative for food tracking.


**Interested in joining the WW team?** Check out the[careers page](https://www.weightwatchers.com/us/corporate-careers) to view technology job listings as well as open positions for other teams.
