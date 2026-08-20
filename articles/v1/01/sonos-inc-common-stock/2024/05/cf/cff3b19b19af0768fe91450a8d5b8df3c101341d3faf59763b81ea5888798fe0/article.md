---
schema_version: "1.0.0"
document_id: "cff3b19b19af0768fe91450a8d5b8df3c101341d3faf59763b81ea5888798fe0"
company_key: "sonos-inc-common-stock"
company: "Sonos Inc."
source_id: "sonos-inc-common-stock-rss-b40e25ea1a3a"
canonical_url: "https://tech-blog.sonos.com/posts/sonos-voice-control-adds-support-for-weather/"
published_at: "2024-05-07T22:43:00+00:00"
first_seen_at: "2026-07-20T23:18:24.938468+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:c30bebe0e16b442897b5778856011d48dac199f3dc9e008de94d0d22dd03db48"
---

# Sonos Voice Control adds support for Weather

### Why a Sonos Voice Control (SVC) weather feature?


The weather is the archetype of boring conversation topics. And yet we can’t stop talking, *and* *singing* , about it. We’re blowin’ in the wind, walkin’ on sunshine, singin’ in the rain, or under an umbrella (ella, ella). Stormy Weather, Purple Rain, Blue Skies,…and you can probably think of more. The reason is simple: on top of being an easy source of poetic metaphors, the weather is central to many of our day-to-day decisions. We check the forecast before picking out what clothes to wear, watering the flowers, or organizing a family trip \[3\]. In fact, it’s the most popular type of scientific information: people look it up once a day or more \[1, 2\].


In the 2000s, the primary source of weather information was the TV \[1\], but we have since moved on to smartphone apps \[2, 4\]. Now, smart speakers are making their way into this landscape, with “ *asking for the weather* ” regularly cited among their top use cases. It can represent about 6% of all voice queries, and jumps up to 15% in the early morning \[5\]. So it’s no surprise that, when surveyed, most French and American Sonos Beta users said they would use a Sonos Voice Control Weather feature (henceforth SVC Weather).


Figure 1. Survey results for English speaking (N = 314) and French speaking (N = 638) Sonos Beta users, answering the question “Would you use an SVC Weather feature?”. The majority of respondents answered “Yes”.


Even though getting the weather is one of voice assistants’ top use cases, the interaction design can sometimes be lackluster. Take this example from a current commercially available voice assistant:


User: “ *What’s the weather this afternoon?* ”


Voice assistant: “ *The forecast in Paris this afternoon is 12 degrees and mostly cloudy. It's currently 12 degrees and mostly cloudy.* ”


The user does get the information they were looking for, but the addition of the current weather information, which happens to be the exact same, feels robotic and clunky.


More critical, when asking “ *what’s the weather this week* ” to another voice assistant, we get the answer:


Voice Assistant: “ *In Paris for the next seven days. Saturday, 11 degrees celsius and mostly sunny weather. Sunday, twelve degrees and showers. Monday, 14 degrees and intermittent clouds. Tuesday, 10 degrees and cloudy skies. Wednesday, 14 degrees and lots of clouds. Thursday, 16 degrees and lots of clouds. Friday, 13 degrees and intermittent clouds.* ”


This long list of forecasts is likely too overwhelming for users; they wouldn’t be able to easily extract the information they need to plan out their week. For the same question, with yet another voice assistant, we get:


Voice assistant: “ *Expect rain, cloudy skies and partly cloudy skies this week. Daytime temperatures will fluctuate between \[...\].”*


Now the information is aggregated to a fault, and also rather useless for someone trying to decide on their week’s activities. With SVC Weather, we wanted to avoid such pitfalls, and design an interaction that would be both useful, usable and pleasant.


###
What’s in a forecast, and how to communicate it?


Building SVC Weather means generating *text-to-speech responses* that describe the weather the user asked for. For example:


User: *What’s the weather like today?*


SVC - text-to-speech response: *“It might rain this morning, but the rest of the day will be sunny. Temperatures will stay around 60 degrees”.*


As mentioned, people care about the weather because it informs some of their daily decisions. Therefore, communicating a forecast is an exercise in translating scientific data into something people can use easily and efficiently. To this, SVC Weather adds an extra challenge: it can only rely on voice to convey information (as opposed to phone apps, which can use images). Because of the transient nature of speech, presenting a lot of information can quickly become taxing to listeners, who must stay focused and maintain what they hear in memory. Taking this into account, and drawing inspiration from the scientific literature, we devised a set of guidelines for the design of SVC Weather’s forecasts.


**Organize the information.** First, a forecast is based on a large amount of data: temperatures, cloudiness, probability of precipitation, type of precipitation, chance of thunder, wind speed, wind direction, humidity, etc… But people don’t generally want to spend a long time studying a forecast \[6\], and they are not equally interested in all weather data points \[4\]. Plus some of the information can be redundant (e.g., cloudiness and chance of precipitation). Finally, SVC has limited “real-estate” for presenting information before overwhelming users’ memory capacity. So our first set of guidelines is:


**1.** Prioritize information that is likely to interest users


**2.** Avoid information redundancy


**3.** Be as short as possible


**Translate the information.** Second, weather data is often represented with quantitative values, for example “8 mm of rain per hour” or “36 mph wind”, but non-experts may struggle to make sense of them. Instead, using qualitative terms (e.g., “ *heavy rain* ”) or explaining the impact (e.g., “ *umbrellas are hard to use in this wind* ”) allows for better understanding, and therefore better decision-making. So our fourth guideline is:


**4.** Prefer qualitative descriptors over quantitative ones


**Manage the uncertainty.** Third, although forecast accuracy has reached impressive highs, uncertainty remains. And even when the forecast *is* accurate, people may not perceive it this way. This is most obvious for the “probability of precipitation”, which conveys the likelihood of at least 0.01” of precipitation accumulating within a given area, over a given time. In other words, someone could not see any rain at their position, despite a 100% chance of precipitation for their city. As long as 0.01” of rain occurred somewhere in the covered area, the forecast is accurate \[2\]. This uncertainty must be properly conveyed to avoid frustration, but without sacrificing the reliability of the forecast. There is some debate over whether numerical (e.g., “ *100% chance”* ) or verbal probabilities (e.g., “ *highly likely”* ) are the better option. We chose to use verbal probabilities because they are easier to integrate in a spoken forecast, and because they protect better against credibility loss (in case of a discrepancy between a forecast and what users actually observe \[7\]).


**5.** Prefer modal verbs (e.g., *might* ) or adverbs (e.g., *highly likely* ) to express probabilities


**Make (common) sense.** Finally, we wanted SVC Weather’s responses to follow common sense and sound as natural as possible. In practice, this means that instead of a forecast like “ *It’s currently clear and 19 degrees. Expect clear skies later in the morning. \[...\]”* where the repetition of “ *clear skies* ” betrays the assistant’s lack of understanding of the forecast, leading users to question its intelligence, SVC Weather would say something like “ *Skies will be clear all morning, with temperatures around…”.*


**6.** Sentences should follow common sense and sound natural


These six guidelines give us a framework to generate forecasts that are useful, easy to process, and pleasant to listen to. But how do we ensure that each of SVC Weather’s responses follows our requirements?


###
How did we build SVC Weather?


Figure 2. Schematic representation of the steps between a user’s weather request and the SVC Weather response.


To build SVC Weather following our design principles, we decided to center human editors in the response generating process, rather than fully automate it. In other words, people hand-wrote the forecast text-to-speech responses that SVC Weather provides. Of course, because we wanted SVC Weather to be thorough and accurate, this meant writing thousands of forecasts, corresponding to thousands of possible weather patterns. This required a workflow that would keep our editors’ workload within reasonable bounds. In particular, we needed:


**1.** *An architecture that would allow editors to “own” the text-to-speech responses and continuously tinker with them. Our aim was that editors should be able to directly impact SVC Weather’s behavior, without having to jump through tons of developmental hoops.*


**2.** *A systematic way to summarize raw weather data (e.g., 85°, 0% cloud coverage, 4 mph wind…) into qualitative descriptors (e.g., hot, sunny, no wind…). Fitting weather patterns into a limited number of categories helped reduce the number of text-to-speech responses needed. Plus it’s easier for editors to write a forecast based on these descriptors rather than raw data.*


**3.** *A content management system to store the forecast strings and their corresponding weather pattern.*


Let’s walk through each module of the SVC Weather architecture and how they tackle these three points.


Figure 3. Schematic representation of the backend architecture for the SVC Weather service, going from categorizing the user’s weather request, to rendering the final text-to-speech (TTS) answer.


**Weather Intent Resolver.** Of course the first step for SVC Weather is to properly identify the user’s request. Are they asking for the weather today? Or if it will rain at 3PM? Or if they should wear a coat? Determining what the user is asking for is handled by the weather intent resolver, which:


-


Checks whether the request is supported (e.g., is the user asking for something out of scope, like “ *what's the weather on the Moon?* ”).


-


Checks how the date and time requested relate to the current time, as this could impact the service’s response (e.g., asking for today’s weather at 6 AM vs. at 5 PM should not yield the same response).


-


Checks whether the user is asking for a general weather forecast (e.g., today’s forecast), for a specific weather condition (e.g., rain), or about a specific accessory that relates to the weather (e.g., a coat).


Depending on the answers to each of these points, the user’s request is categorized as one of several intents. For example if at 8 AM they ask “ *what’s the weather today?”* , the Weather Intent Resolver will categorize the request as a “weather_today_am” intent, meaning we should answer with today’s complete forecast (including morning, afternoon and evening weather events).


**Weather Data Provider & Weather Prediction.** Once the user’s request has been identified, SVC Weather can make the correct call to our weather data provider to get the corresponding raw weather data. This data is then transformed into an intermediary format, to be manipulated by the Variables Builder and the Predicates Builder.


**Variables Builder.** Although SVC Weather’s text-to-speech responses are hand-written, some parts of the strings must be filled-in automatically. This is typically the case for the temperature values (we wouldn’t want to hand-write a sentence for each possible temperature). The Variable Builder’s role is to generate these fill-in values, called weather variables, going from weather data (e.g., -10°) to strings (e.g., *minus ten degrees* ).


**Predicates Builder.** This module is at the heart of our weather service. It allows us to abstract away from quantitative weather data (e.g., 85 degrees, 1% cloud coverage) and into qualitative descriptors called predicates (e.g., is_hot, is_sunny). The mappings between the quantitative values and the predicates were defined following standards from expert institutions such as the American Meteorological Society, the National Weather Service, or our weather data provider. See for example Figure 4.


Figure 4. Table taken for the National Weather Service's website, matching the proportion of sky covered by opaque clouds (out of 8) with a qualitative descriptor (e.g., "sunny"), which we used to generate predicate rules. (link: https://www.weather.gov/bgm/forecast_terms)


For a given set of weather data, the predicate builder will output a unique combination of predicates. In our attempt to provide as useful a tool as possible to our users, we allowed for thousands of possible predicate combinations, and each of them was given a unique, finely-tuned text-to-speech response.


**Text-To-Speech Response Selector (TTS Selector).**


At this stage, we know the weather intent requested by the user, the predicates combination representing the weather, and the values of weather variables. Given this information, the TTS Selector’s role is to find the correct forecast sentence stored in our content management system.


This is best explained through an example. Say the user asked “ *what’s the weather now?* ” and the TTS Selector got the following information:


-


Intent: weather_now


-


Predicates:


-


temperature: is_muggy,


-


cloudiness: is_mostly_sunny


-


rain: is_no_chance


-


wind: is_no_wind


-


thunder: is_no_risk


-


Weather variables:


-


Temperature: 85 degrees


First, the intent points the TTS Selector to the correct content base. Indeed, as shown in Figure 5, the text-to-speech responses are stored by intent (e.g., weather_now, weather_today). Inside each base, we find a list of text-to-speech strings paired to predicate combinations. In our example, the correct base is *weather_now* , and the final corresponding text-to-speech response would be: “ *It's 85 degrees and humid right now, with mostly sunny skies* .”, where “ *85 degrees* ” are the only words filled in through an automated process.


Figure 5. Each “base” pictured here corresponds to a supported weather intent.


Figure 6. In the weather_now base, the TTS Selector is looking for the line that matches the predicates combination it was given (i.e., temperature: muggy, cloudiness: mostly_sunny, …) - the match points to a unique, finely-tuned TTS that is specific to this weather pattern.


A great strength of this implementation is that editors can easily add new predicates-to-forecast pairings to a base, or make changes to strings, without impacting the backend architecture of SVC Weather at all. The content management system is fully owned and managed by the editors’ team, and it essentially serves as a database available through an API. Of course, this means that SVC Weather must have access to the latest version of this database - this is achieved with a tool called the Assets Release Manager. This tool first downloads the information in our content management system (i.e., the predicate-to-forecast pairings), also called *assets* . It then versions the assets to be able to identify which version is currently used by the system, and it also checks their consistency. If they are considered stable, these assets are ‘fed’ to the SVC Weather architecture and users will get the latest version of any forecast response.


###
Get started with SVC Weather


For SVC Weather to give you accurate forecasts, it must know your approximate location. So to use it, please ensure that you have the latest version of the Sonos app, and set your location in the settings of Sonos Voice Control. Of course, SVC is still dedicated to being your most private voice assistant. This approximate location is not shared with anyone other than our weather provider, and we do not share any personally identifiable information with them.


###
Conclusion


With SVC Weather, we’ve built a service that provides concise and logical responses, designed based on our current understanding of weather information consumption and human communication principles. In particular, the forecast responses prioritize important information, avoid redundancy, and are easy to understand for users, all while sounding as natural as possible. To achieve this, we’ve built a backend architecture that transforms raw weather data into what we called a “predicate format”, that acts seamlessly with our text-to-speech response management system.


SVC Weather is up and running on all your SVC-enabled devices, and it can answer a range of questions, such as “ *what’s the weather this weekend?”* , “ *will it rain tomorrow?”* or even *“should I wear a coat this evening?”* . Go ahead and give it a try!


### **References**


\[1\][Lazo, J. K., Morss, R. E., & Demuth, J. L. (2009). 300 billion served: Sources, perceptions, uses, and values of weather forecasts. Bulletin of the American Meteorological Society , 90 (6), 785-798.](https://journals.ametsoc.org/view/journals/bams/90/6/2008bams2604_1.xml)


\[2\] Vaughn, C. M. (2022).[From the sky to the smartphone: Communicating weather information in a digital age . Mississippi State University.](https://scholarsjunction.msstate.edu/cgi/viewcontent.cgi?article=6643&context=td)


\[3\] Demuth, J. L., Lazo, J. K., & Morss, R. E. (2011).[Exploring variations in people’s sources, uses, and perceptions of weather forecasts. Weather, Climate, and Society , 3 (3), 177-192.](https://journals.ametsoc.org/view/journals/wcas/3/3/2011wcas1061_1.xml?tab_body=pdf)


\[4\][Phan, M. D., Montz, B. E., Curtis, S., & Rickenbach, T. M. (2018). Weather on the go: An assessment of smartphone mobile weather application use among college students. Bulletin of the American Meteorological Society , 99 (11), 2245-2257.](https://journals.ametsoc.org/view/journals/bams/99/11/bams-d-18-0020.1.xml?tab_body=abstract-display)


\[5\][Bentley, F., Luvogt, C., Silverman, M., Wirasinghe, R., White, B., & Lottridge, D. (2018). Understanding the long-term use of smart speaker assistants. Proceedings of the ACM on Interactive, Mobile, Wearable and Ubiquitous Technologies , 2 (3), 1-24.](https://dl.acm.org/doi/abs/10.1145/3264901)


\[6\][Doksæter Sivle, A., & Kolstø, S. D. (2016). Use of online weather information in everyday decision‐making by laypeople and implications for communication of weather information. Meteorological Applications , 23 (4), 650-662.](https://rmets.onlinelibrary.wiley.com/doi/pdf/10.1002/met.1588)


\[7\][Dhami, M. K., & Mandel, D. R. (2022). Communicating uncertainty using words and numbers. Trends in Cognitive Sciences .](https://www.cell.com/trends/cognitive-sciences/fulltext/S1364-6613(22)00060-2?dgcid=raven_jbs_aip_email)


Share
