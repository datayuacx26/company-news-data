---
schema_version: "1.0.0"
document_id: "4cff6c57507c2aed871e439793c7d36d7299789ae2205a5ec5bcf9445d5b3333"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/leveraging-llms-to-integrate-autonomy-into-robots"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:1d328eb013371a03857173367770998841c7075b2cbd65e1779cfe56d0ebc046"
---

# Polymath Robotics Blog | Leveraging LLMs to integrate autonomy into Robots

Polymath is a company that makes off-highway vehicles autonomous. Our goal is to find the best tools to help our clients get their vehicles to autonomously do the tasks they want their vehicles to do.


At Polymath, we try to find bottlenecks in our engineering process and find creative ways to solve them. In general we are skeptical of AI solutions. We have heard of solutions that claim to "build a robot." But building robots is not as easy as some of the AI hype claims. The real question for us is, "what areas can AI be helpful to robotics?"


Recently we found a way that AI, specifically RAG based LLMs, could be helpful. A bottleneck our engineers faced was creating DBC files. You can think of DBC files as the Rosetta Stone between the robot and our software. We call our software the Hardware Abstraction Layer (HAL).


Polymath's HAL allows developers to write code that is hardware-independent. This makes it easier to port software across different robot platforms.


We would spend days upon days going through the robot's user manual PDFs building these DBC files. It was a painful, tedious process. Then we had an idea: "could we use AI to reduce the amount of work we had to do to create a DBC file?"


Before we get into how we used AI, first a little primer about DBC files.


#### **The DBC File: the Rosetta Stone of CAN Communications**


A DBC File is used to decode CAN messages that may be specific to a vehicle/robot's setup. Once you have the DBC file, you then know the CAN codes the vehicle needs to turn right, left, go forward and stop, and in some rare cases how to pump up the stereo. Once you have the DBC File, Polymath HAL can communicate with the drive-by-wire vehicle/robot and start sending and receiving signals. The vehicle and Polymath can now talk to each other!


But there is a problem.


As mentioned before, the problem is that creating the DBC File is extraordinarily time consuming-- just like creating the original Rosetta Stone was time consuming. Drive-by-wire vehicles can have hundreds of CAN codes. On top of this each company documents these codes in different ways in large pdfs. Therefore, it can take tons of engineering hours creating and testing the DBC File. How can we make this process faster?


#### **Could we use RAG LLMs to create DBC files? ****


RAGs or Retrieval Augmented Generation is essentially a way to make LLMs more accurate. In school, most tests are "closed book" tests. But you can sometimes get an "open book" test. The nice thing with open book tests is that you can always look up facts and you can always cross check what you think is the answer with what the book says is the correct answer. Your accuracy will probably improve on an open book test compared to a closed book test. Likewise, RAGs are kind of like LLMs that have the "open book" of extra data or information that you give it before you ask the LLM for the answer. For more info on RAGs check out this[post from Nvidia](https://blogs.nvidia.com/blog/what-is-retrieval-augmented-generation/) .


Our engineering team thought of this idea: could we use a RAG based LLM to create new DBC files from 1. a set of PDFs given to us by our integration partner and 2. a set of our own internal reference DBC files?


Using the analogy above, Polymath's internal reference DBC files would be the "open book" for the RAG LLM to use for its "open book exam," The exam for the RAG based LLM would be to create a new DBC file for our HAL and the robot to communicate with each other.


At Polymath we have many DBC files from different robots for different operations. This is because we have worked with hundreds of different robots. Therefore, we have DBC files for commissioning (initial setup process of the robot), DBC files for feedback (what happens after the command is sent, i.e. success or error), DBC files for heartbeat, DBC files for configuration (2 wheels vs 4 wheels), DBC files for controls (moving, joint extending), DBC files for faults. We have around two hundred unique DBC files (some files work with multiple robots as one integration partner may have one unified system for multiple robots).


Two hundred unique DBC files may not seem like a lot when we see numbers like millions and billions get thrown around in talks about AI. However, two hundred is a lot. With two hundred, we have enough unique DBC files for Skynet to take over all American construction sites. That is, whenever Skynet decides to do its thing. From our tests, our two-hundred DBC files is more than enough for a RAG.


#### Learning from smaller projects


Often in engineering when you take on a small project, you get some of your rookie mistakes out of the way and gain experience that helps you in bigger projects. Before taking on DBC files with RAGs, we first created our own internal RAG based Slackbot.


We store all our docs in Notion. We thought it would be fun to be able to just type into slack, "What is the office door code again?" And then get the answer back. This beats searching and looking it up in our 3500 pages of docs. In that process we learned a few things:


1. The context window (the amount of text a model can process at a time to generate a response) is a limitation on local models. We needed to tell our RAG details but we couldn't fit it in one prompt.


2. Ollama (an excellent framework on building LLMs locally) was not as straightforward as it looks with bigger models run on multiple GPUs.


‍


Therefore, we turned to the big AI companies APIs for our DBC file with RAGs project. They have huge context windows and they are user friendly.


#### High level Architectural Plan


The architectural plan that we arrived at to create DBC files was to use multiple third-party LLM providers to act as "experts." These "experts" would then cross verify the work of each LLM.


For each type of DBC file that we need to create, i.e. commissioning, feedback, controls, we would do the following:


1. We would send a set of Reference DBCs and the documentation PDFs to Google Gemini with a prompt to create a DBC file with the given information only. We started with Gemini as it has the largest context window(at the time of writing).


2. We would then ask our next expert, OpenAI GPT, "did Gemini create any errors with this DBC file it created from these reference DBC files and documentation PDFs? If so, fix them."


3. Then we would move on and ask Anthropic's Claude the same thing. See Figure 1.


Figure 1


‍


4. Once we had the output DBC file of our "multiple AI experts" we would then move to syntax validation. We run it through the python library "CAN BUS tools" as a first pass on validation. "CAN BUS tools" would validate the structure and the syntax of the newly LLM generated DBC file. You could think of this as like checking the grammar of an English Writing Assignment. Is the grammar all correct? "CAN BUS tools" is not checking if the DBC file actually works.


3. If the LLM generated DBC file failed structure and syntax validation, then we would take this back to the LLM with the errors and tell the LLM to retry and fix the errors. We would start with Gemini again. Then we would go back to "CAN BUS tools." If "CAN BUS tools" said the Gemini corrections were syntactically valid, then we would be done with the DBC file creation! If Gemini failed, then we would ask OpenAI to fix it. We would then go back to "CAN BUS tools" to check if it was valid. If that failed, we would go to the next expert to correct it, Antropic. We would continue this process until we got a structurally and syntactically valid DBC file from our multiple experts. See Figure 2.


‍


Figure 2


‍


4. Once we got a valid file, we bring the human in the loop and start testing the DBC file on the robot. If our integration partner had simulation software, then we would test the LLM generated DBC file against the simulation software. If they didn't have simulation software, or we are not targeting using our own CI/CD, then we moved to the next step of testing, which is putting the robot on cinder blocks in the field. We then start sending it signals and validate the DBC file by hand, making corrections when necessary.


#### From Plan to Implementation


When we sent PDFs to our multiple experts, we would have an issue with parsing. For example, we would take our PDFs and run them through a PDF parser to create a plain text corpus (think giant text file). The problem with this was that often key commands were in screenshots in the PDF. Therefore, our plain text corpus would say "Commands in Figure 1". But there is no Figure 1 in plain text. This key information would be missed.


What we found to be a better approach was to convert the pages of the reference PDFs into individual images. We would then tell our AI expert additional information like, "These images are sequential images. The first page screenshot needs to act as context for the second page screenshot." This avoided all the parsing issues that we were having. Of course, if the screenshots inside the PDF were bad, then it would lead to a PDF image with a low res screenshot inside of it. We would then have to include that info in the screenshot in the prompt. As you can see, we needed large context windows that Gemini, OpenAI GPT, and Anthropic offer. We are sending a ton of info.


One interesting thing that we found was that we only needed to give a limited number of Reference DBC files to the LLMs. For example, if an integration partner has multiple robots that have the same controls, we can send one Controls Reference DBC to the LLM. Whereas to create the Heartbeat DBC, we would send 10 Reference Heartbeat DBCs. We did not need to provide all our old DBCs to create a new, accurate DBC file for the current robot.


What about the cost? How expensive is this?


Big company LLM APIs can be expensive to use. But for us, we are not creating millions of DBC files. Therefore, the cost was relatively small compared to the engineering time that we saved. We reduced the engineering time to create a DBC file from five to six days to half a day. For a given robot, we need to create about eight different DBC files. Across multiple LLMs, from experimentation to final output, our total cost was less than $20 for a given robot setup. For the creation of eight DBC files, Figure 3 shows our cost from the Anthropic portion.


‍


‍


Figure 3: Anthropic cost for eight DBC files and the input and output token totals


‍


#### Worrying about Errors and Hallucinations


There are two major concerns with the results that you get from a DBC file created via an LLM.


The first question is how do you make sure that everything relevant in the documentation PDFs is included in the LLM generated DBC file?


The second question is how do you make sure that the LLM didn't end up adding some extra commands in the DBC file that it made up?


Regarding making sure everything relevant is in the LLM generated DBC file: when we go to our integration partners, we ask them for a list of the things that they want Polymath to automate in their drive-by-wire robot. They only ask for a subset of the tasks that the robot can do. For example, our integration partner may only want us to automate navigation and safety. They would not want us to automate the other tasks.


As an example, let's say you have a dump truck. Our integration partner may ask us to automate only making sure the dump truck safely navigates to the correct location.


Once Polymath takes the dump truck to the right spot, the dump truck's remote operator can then take control via a joystick. The remote operator can then dump the junk in the trunk!


This way you can have a single remote operator with a joystick work on the dumping and loading task for five different dump trucks, while polymath handles the safety and the navigation of those same five trucks.


When we have the LLM created DBC file, we check via human-in-the-loop to see if all the safety and navigation commands needed are in the LLM generated DBC file. If other extra commands are added in the LLM generated DBC file such as the dumping action, we don't worry about it because this command will never be given to the robot from the Polymath HAL Layer, hence this becomes non-executing code.


Regarding hallucinated commands: the main thing to remember is that Polymath needs to make sure it gets all the commands for safety, navigation, and any other command needed by our integration partner correct. Therefore, if the LLM hallucinates and adds a command for the dump truck to turn on "rap song hydraulics mode" that command will not matter. This is because the Polymath HAL Layer would never send that hallucinated "rap song hydraulics mode" command to the robot. We would of course remove it when we find it in testing. Furthermore, the commands that are safety relevant are reviewed by engineers, and tested in our CI/CD before they get on-vehicle. This gives us a few extra layers of protection in real world deployments, and we’d never recommend deploying LLM generated code straight to a robot.


#### Conclusions


With our multi-expert RAG LLM approach in place, we cut down the time needed to create a DBC file from 5 to 6 days to down to half a day! Creating DBC files, the Rosetta Stone between a robot and Polymath's HAL layer got a heck of a lot faster!


In the end, we created a nice GUI where anyone, even non-engineers at Polymath can drop a bunch of reference PDFs in, and our system will generate a working DBC file that is ready for human in the loop testing.


AI tools are neat once you find the right use case. Creativity lies in finding the right use case. We found some other use cases internally for AI as well. We named our multi-expert RAG process the Polymath DBC Validator. We also gave the family of our internal AI tools a cool name, Polymath Intelligence. Our DBC Validator is part of the Polymath Intelligence suite. We look forward to finding more areas in our processes where we can harvest some of the newer AI tools!


‍
