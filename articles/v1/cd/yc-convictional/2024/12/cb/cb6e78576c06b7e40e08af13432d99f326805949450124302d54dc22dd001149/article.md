---
schema_version: "1.0.0"
document_id: "cb6e78576c06b7e40e08af13432d99f326805949450124302d54dc22dd001149"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/engineering-time-reclaimed-automating-r-d-documentation-with-ai/"
published_at: "2024-12-20T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:2ab3fa3886d97ebe501e43e1d0ae5f4fb5eb7ee613b18d21c96029efef2da5c3"
---

# Engineering Time Reclaimed: Automating R&D Documentation with AI

#


Documentation is the hidden cost of engineering innovation. While teams prioritize shipping new capabilities, documenting technical decisions and research often gets fragmented across systems. At Convictional, we found this challenge became particularly acute when preparing for research and development (R&D) tax credits.


R&D tax credits offer substantial returns - often hundreds of thousands in credits - for investing in technical innovation. However, qualifying requires engineering teams to pause development and reconstruct months of technical decisions. The process demands coordination between engineering and finance teams, with engineers spending 3+ hours per project preparing for tax credit interviews.


This essay outlines how we strategically transformed this multi-week effort into an automated process using LLMs. Building the solution took less than a week for a single engineer and created an extendable system for future years - a compelling return compared to manual documentation. The project demonstrates how engineering teams can leverage AI to solve operational challenges without compromising bandwidth.


The patterns we discovered now power practical business applications across our platform, helping our customers make better decisions. Through this case study, we'll share technical implementation details and strategic decisions that apply broadly to documentation and knowledge extraction challenges that engineering teams face.


#### The problem


From a Finance perspective, preparing for tax credits involve 3 main steps:


- Gather context about R&D projects worked on at the company
- Collate details of specific projects, who worked on them, and who owned them
- Working with tax consultants for interviews and final submission


The initial discovery and documentation phases are particularly challenging. Engineering must comb through thousands of records across various systems (Github, Jira, etc.) to identify and document R&D work. Without consistent year-round documentation practices, this retrospective process pulls engineering time away from core development work.


This challenge scales with the organization. As an engineering team grows, technical initiatives tend to become more complex, and the manual documentation process will eventually consume an unsustainable amount of engineering bandwidth.


#### The solution


##### Strategic approach


When Finance approached us about the R&D tax credits, we saw an opportunity to automate the documentation process using AI. The requirements were clear: create a comprehensive list of R&D projects with detailed descriptions, focus on high-level initiatives, identify ownership and contributors, and ensure auditable documentation.


Our solution needed to balance several key constraints. Results had to be verifiable against source material while capturing all significant R&D initiatives. Also, given the tax compliance nature of the project, we needed to maintain clear traceability for audit purposes while meeting tight filing deadlines.


Traditional automation approaches (like keyword filtering and clustering) won't capture the nuanced technical discussions that define true R&D work. Instead, we chose LLMs for their ability to: (i) Process existing knowledge in our Github repositories, (ii) Understand technical context and subtleties, (iii) Summarize thousands of discussions into coherent projects, and (iv) Extract historical data without engineering disruption.


##### Technical architecture overview


We designed a multi-stage pipeline to break this complex task into manageable components, allowing us to iterate independently on each stage while maintaining accuracy.


The process followed five steps:


1. **Source data collection and preprocessing**


- Filtered Github data for relevant time periods and repositories
- Preprocessing to handle token limitations and content chunking
- This resulted in about 900 Github chunks to process


2. **Initial R&D project extraction**


- Emphasis on high recall over precision at this stage
- Maintained source content traceability through ID references
- About 1500 fine-grained "projects" were extracted


3. **Grouping of initial projects and summarization**


- Iterative grouping and summarization process
- Group and reduce fine-grained "projects" into about 380 coarse-grained entities


4. **High-level theme identification**


- Two-step approach to handle LLM attention limitations
- Reduce coarse-grained projects into 25 high-level R&D initiatives


5. **People and ownership attribution**


- Use source content citations to augment high level projects with ownership and contributor information


##### Technical challenges and solutions summary


Throughout implementation, several challenges were encountered, and are summarized in the table below:


Challenge Challenge description Solution


LLM task overload The LLM is tasked with doing too much in a single prompt Break the problem down into manageable steps - don't ask an LLM to do something you wouldn't ask a human to do


Token limitations Large context windows still weren't enough for our data volume Developed multi-stage processing pipeline with intelligent chunking


Attention limitations LLM performance degraded with too many projects to analyze at once Implemented staged summarization with verification steps


Output consistency Initial results varied between runs Used temperature = 0 and structured output validation


Data lineage Maintaining source Github data traceability through multiple processing stages Implemented ID reference system


Group projects into high level themes When tasked with grouping and summarizing in one shot, the LLM didn't do well Break the problem down into 2 steps: (i) Prompt the LLM to group similar projects together, and (ii) Take those lists of projects and separately ask the LLM to summarize each of them into a single high level project


##### Results and impact


**Quantitative outcomes**


Our pipeline successfully transformed 900 Github chunks into 25 high-level R&D projects, through intermediate steps of 1,500 fine-grained and 380 coarse-grained projects.


**Quality improvements**


The pipeline maintained complete traceability to source documentation throughout the process, capturing both nuanced technical details and high-level strategic context. Documentation remained consistent with comprehensive descriptions, source links, and contribution attribution. The LLM effectively identified major R&D themes, which team members validated via Github source links.


**Organizational benefits**


By extracting insights directly from existing Github documentation we minimized disruption to engineering work. This allowed the Finance team to receive the structured documentation they needed while engineering maintained focus on product development.


**Unexpected advantages**


The process revealed interesting and unplanned, but significant, R&D patterns in our development work that weren't initially apparent. This resulted in increased tax credits since more projects were sourced.


**Team feedback**


The project received positive sentiment across the organization. Source links enabled teams to validate project categorizations that weren't explicitly planned as R&D initiatives, while clear connections between summaries and Github sources built confidence in the process.


**Streamlined interview preparation**


Engineers accessed specific Github links connected to their projects, enabling confident technical discussions with minimal preparation overhead. This saved approximately three hours of prep work per engineer for each project.


**Areas for future enhancement**


The system shows potential for productization, with learnings applicable to broader documentation challenges and how we interact with LLMs.


##### Key takeaways


Takeaway Description


Rethinking documentation processes - Strategic automation can solve traditional documentation challenges
- LLMs enable both engineering productivity and comprehensive documentation
- Leverage existing artifacts rather than creating new documentation overhead


Breaking down complex problems - Engineering principle: solve complex problems through simpler components
- Maintain quality through staged processing of large volume of unstructured content
- Focus on iterative refinement at each stage


LLM implementation strategy - Handle token limits using chunking approaches
- Manage attention head constraints with multi-step summarization
- Treat LLMs as powerful but bounded tools requiring careful engineering


Balancing automation and human oversight - Maintain human validation touchpoints throughout the process
- Connect summaries to source documentation for verification
- Build trust through transparency and traceability


Future implications - Methods adaptable to various documentation challenges
- Applicable to technical documentation and knowledge base creation
- Demonstrates AI's potential for solving operational challenges while maintaining core focus


##### Looking ahead


This R&D documentation project represents just one example of how we approach innovation at Convictional. Whether we're building our core infrastructure platform or developing internal tools, we maintain the same rigorous engineering standards and thoughtful approach to problem solving.


If you're interested in learning more about how we're building the infrastructure that powers decisions at the world's most ambitious companies, reach out to us atdecide@convictional.com .


#### Appendix


##### Implementation deep dive


For the technically curious, this section details the recipe we employed to distill high level R&D projects from our Github source data.


##### Strategy and technology selection


In designing our pipeline, we balanced technical sophistication with practical implementation needs. After evaluating various LLM options, we selected Claude Sonnet (2024-06-20, at the time of this project) for its expanded input context window and performance in our previous projects. To ensure reproducible results - critical for any process that might need to withstand audit scrutiny - we configured the model with a temperature of 0, maximizing response consistency.


##### Step 1: Collect source content data


We identified Github as our primary data source, given that it houses virtually all of our engineering work and technical context. Leveraging our analytics data warehouse, we could systematically access our comprehensive Github history.


Our data collection focused on Github issues, pull requests, and their associated comments, along with relevant metadata. We structured the source data with the granularity of individual issues and pull requests, filtering for content created within the target tax year and limiting scope to relevant repositories.


A key technical consideration emerged around content processing. While Claude offers a substantial input context window, we discovered that approaching this limit often triggered output token constraints - a classic example of LLM response scaling with input size. To address this, we implemented a chunking strategy with maximum token limits per content chunk.


This preprocessing resulted in about 900 Github content chunks.


##### Step 2: Initial extraction of R&D projects


For our first processing phase, we prioritized comprehensive information capture over precision. Rather than immediately tackling content grouping or managing input token limits, we focused on extracting potential R&D projects from each individual piece of Github content. This approach optimized for high recall, knowing we would refine and summarize these projects to higher levels in later stages.


The Python pseudo code looks something like:


```text
class Project:
name: str
description: str
source_content_ids: list[int]


extracted_projects: list[Project] = []


for content in all_content:
projects: list[Project] = get_projects_from_content_using_llm(content)
extracted_projects.extend(projects)


```


We leveraged the[instructor](https://python.useinstructor.com/) library to structure LLM responses into Project objects, each containing a name, description, and source content IDs for Github issue and pull request linkage.


A critical learning emerged during this stage: a precise definition of qualified R&D projects proved essential for accurate extraction. Without explicit criteria, the LLM's interpretation of "R&D projects" showed surprising liberty, highlighting the importance of clear specification in prompt engineering.


This initial extraction yielded approximately 1500 "projects" - a number that immediately signaled the need for higher-level summarization. For context, this would suggest more than five unique R&D projects per working day, an unlikely scenario for our company's size. This granularity, while comprehensive, set the stage for our subsequent summarization steps.


##### Step 3: Summarizing initial projects


Having extracted our initial projects, we faced a technical challenge of how to effectively summarize 1500 fine-grained projects into meaningful, higher-level groupings. While the ideal solution might seem straightforward - simply asking the LLM to group everything into themes - we needed to account for both input token limitations and LLM attention head constraints.


The solution involved an iterative grouping strategy. Here is a snippet of Python pseudo code:


```text
if randomize_projects == true:
initial_extracted_projects: list[Project] = shuffle_order_of_projects(initial_extracted_projects)


# max_project_bunch_size is typically beteen 40 and 60
project_bunches: list[list[Project]] = get_project_bunches(initial_extracted_projects, max_project_bunch_size)


grouped_projects: list[Project] = []
for project_bunch in project_bunches:
current_list_of_projects: list[Project] = []


for glean in num_gleans_per_project_bunch: # num gleans = 3
projects: list[Project] = group_similar_projects_using_llm(project_bunch, current_list_of_projects)
current_list_of_projects.extend(projects)


grouped_projects.extend(current_list_of_projects)


```


We implemented a multi-pass approach, first bundling projects into manageable groups (typically 40-60 projects per bunch). For each bundle, we employed 3 "gleaning" passes, allowing the LLM to identify and group similar projects while maintaining a list of existing groupings to prevent duplication. Notably, the LLM demonstrated remarkable accuracy in maintaining source content ID relationships throughout this process.


Our initial implementation, running 3 iterations without randomization (randomization was added after this fact), showed promising convergence: 1500 → 800 → 550 → 450 projects.


However, we identified a limitation: as iterations progressed, project bunching became increasingly static, creating a natural plateau in the grouping of projects. To overcome this, we introduced randomization before bunching, running 4 additional iterations: 450 → 420 → 400 → 390 → 380 projects.


This final count of 380 projects was significantly more manageable than our initial 1500, and provided a foundation for our next phase of high-level theme identification. The reduction represented not just a numerical improvement, but a meaningful consolidation of related technical work.


##### Step 4: High-level grouping of R&D projects


While our previous stage successfully reduced our project count to 380, we discovered that even fitting these within a single input context window didn't solve all our problems. Our initial attempt - directly asking the LLM to synthesize these into dozens of high-level projects - produced ineffective results, with minimal new insights after the first glean and persistent duplication issues despite explicitly instructing the LLM otherwise.\\


A breakthrough came through a colleague's recommendation, suggesting a 2-phase approach:\\


Phase 1: Theme clustering


First, we tasked the LLM with clustering, asking it to group projects by (arbitrary) theme IDs without generating any descriptive content. The output took the form of a simple mapping between "theme IDs" and a list of project IDs:


```text
{
1: [1, 2, 3],
2: [4, 5],
3: [6, 7, 8],
...
}


```


We set an arbitrary maximum of 25 themes, striking a balance between comprehensiveness and practicality. Through experimentation, we made several key prompting decisions:


- Relaxed the requirement for all projects to be themed, avoiding forced groupings of unrelated work
- Allowed multi-theme project assignment, recognizing that initiatives like "implement integration X" often impact multiple high-level projects
- Maintained flexibility in the LLM's grouping decisions to leverage its natural understanding of project relationships


Phase 2: Theme development


With our clustering complete, we then processed each theme to generate comprehensive project descriptions. The Python pseudo code looks like:


```text
high_level_projects: list[Project] = []


for theme_id, project_ids in dict_of_themes_and_projects: # this is the dictionary above
relevant_projects: list[Project] = filter_for_relevant_projects(list_of_projects, project_ids)
high_level_project: Project = get_high_level_project_from_llm(relevant_projects)
high_level_projects.append(high)level_project)


```


Thus, given a list of relevant projects from the step just above, the LLM was tasked with combining those projects' names and descriptions into a single high-level qualified R&D project. Of course, the LLM was again given our definition of a qualified R&D project and is further instructed to use information from all of the relevant coarse-grained projects when coming up with the content for the high-level project.


In this phase, the LLM combined projects within each theme, maintaining our strict definition of qualified R&D projects throughout. The result is a culmination of distilling about 1500 fine-grained "projects" into 25 cohesive, high-level qualified R&D initiatives.


##### Step 5: People attribution


Following Finance team feedback on our initial delivery, we had an additional request: mapping people to projects to facilitate follow-up interviews. Rather than retrofitting our existing pipeline, we further developed an attribution model to handle this requirement ex post facto.


While our first instinct was to process all Github content for each high-level project in a single LLM query, we quickly encountered input token limit constraints. This led us to develop a 2-step solution.


Step 1: Individual contribution extraction:


```text
class PeopleInvolved:
list_of_names: list[str]
reason_for_involvement: str


people_all_projects: list[list[PeopleInvolved]]
for project in high_level_projects:
content_data: list[str] = get_content_data(project.source_content_ids)


people_per_project: list[PeopleInvolved]
for content in content_data:
people: PeopleInvolved = get_people_involved_using_llm(content_data, project)
people_per_project.append(people)


people_all_projects.append(people_per_project)


```


‍


In this initial step, we processed each high-level project's Github content individually, extracting not just participant names but also their specific contributions and involvement reasoning. This granular approach ensured we captured the full context of each person's contribution.


Step 2: Contribution summarization


The second phase synthesized these individual contributions per piece of Github content into project-level insights. For each project, we aggregated all PeopleInvolved data to identify the project owner, reasoning for selecting the project owner, and a complete list of other project contributors.


This structured approach to attribution provided Finance with the detailed people mapping they needed while maintaining the accuracy and completeness established in our earlier stages.


Interested in trying Convictional? Email us atdecide@convictional.com .
