---
schema_version: "1.0.0"
document_id: "af17827cc807452949df509fe1baad306206cd92da0e072a797f14d6c78b3d76"
company_key: "yc-helixdb"
company: "HelixDB"
source_id: "yc-helixdb-news-import-f26087e13605"
canonical_url: "https://www.helix-db.com/blog/building-a-graphrag-system-for-professor-recommendations-with-helixdb"
published_at: "2025-08-17T15:49:00.751+00:00"
first_seen_at: "2026-07-24T11:14:24.417435+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:2937c7c9738183cc8601a9c609a6cd802b2a27b3d72e0a4fab9fb888ae509d08"
---

# Building a GraphRAG System for Professor Recommendations with HelixDB

## Introduction


In this comprehensive guide, we'll build a Graph-Vector RAG (Retrieval-Augmented Generation) system using HelixDB to help students find professors based on their research interests. This system combines the power of graph databases with vector embeddings to provide intelligent, context-aware recommendations.


Our system will be able to answer queries like:


-


"What professors does High-Energy Physics research?"


-


"What professors are working in X University?"


-


"What professors are working in the Computer Science department?"


-


"What professors are working in X University and are in the Computer Science department?"


-


"I like doing research in Large Language Models, can you recommend some professors at X University?"


## Understanding the Dataset


We'll work with a comprehensive professor dataset containing:


-


**Name** and **Title**


-


**Department(s)** and **University**


-


**Personal page URL**


-


**Short biography**


-


**Key Research Areas** with descriptions


-


**Lab affiliations** and research focus


Here's an example professor record:


```text
{
"name": "James",
"title": "Assistant Professor",
"page": "<a target="_blank" href="https://james.com">https://james.com</a>",
"department": ["Computer Science"],
"university": ["Uni X"],
"bio": "James is an Assistant Professor whose work sits at the intersection of basketball analytics, computer vision, and large-scale machine learning. His research focuses on turning raw player-tracking video, wearable-sensor streams, and play-by-play logs into actionable insights for teams, coaches, and broadcasters. Signature projects include ShotNet— a deep learning model that predicts shot success probability in real time— and DunkGPT, a language model fine-tuned on millions of play descriptions to generate advanced scouting reports.",
"key_research_areas": [
{
"area": "Computer Vision for Basketball",
"description": "Designing CNN and Transformer architectures that track player pose, ball trajectory, and court zones to quantify defensive pressure and shooting mechanics."
},
{
"area": "Predictive Modelling & Simulation",
"description": "Building Monte-Carlo and sequence models that forecast possession outcomes and season performance using play-by-play and spatial data."
},
{
"area": "Sports Analytics with Large Language Models",
"description": "Leveraging LLMs to explain model outputs, auto-generate commentary, and mine historical game archives for strategic patterns."
},
{
"area": "Wearable Sensor Data Mining",
"description": "Applying time-series and graph learning techniques to inertial-measurement signals for fatigue monitoring and injury prevention."
},
{
"area": "Fairness & Ethics in Sports AI",
"description": "Studying algorithmic bias and ensuring equitable analytics across different leagues, genders, and play styles."
}
],
"labs": [
{
"name": "Basketball Data Science Lab",
"research_focus": "An interdisciplinary group combining data science, biomechanics, and sport psychology to create next-generation analytics tools for basketball."
}
]
}
```


## Why Vector-Graph RAG?


Traditional search systems struggle with semantic understanding and relationship traversal. By combining graph databases with vector embeddings, we get:


1.


**Graph Capabilities** : Fast traversal of relationships (professor → department → university)


2.


**Vector Search** : Semantic similarity matching for research areas


3.


**Hybrid Queries** : Filter by exact matches (university) while finding similar research interests


At scale with 1000+ universities and millions of relationships, this hybrid approach provides both precision and semantic understanding.


## System Architecture


### Graph Schema Design


Our graph consists of the following components:


**Nodes:**


-


**Professor** : Core entity with` name` ,` title` ,` page` ,` bio`


-


**ResearchArea** : Research domains with` area` and` description`


-


**Department** : Academic departments with` name`


-


**University** : Institutions with` name`


-


**Lab** : Research labs with` name` and` research_focus`


-


**ResearchAreaAndDescriptionEmbedding** : Vector node for semantic search


**Edges:**


-


Professor → ResearchArea


-


Professor → Department


-


Professor → University


-


Professor → Lab


-


Professor → ResearchAreaAndDescriptionEmbedding


## Setting Up HelixDB


### Step 1: Initialize HelixDB Project


```text
mkdir professor_rag_system
cd professor_rag_system
helix init
```


### Step 2: Define the Graph Schema


Create` schema.hx` :


```text
// NODES //
N::Professor {
name: String,
title: String,
page: String,
bio: String,
}
N::ResearchArea {
research_area: String,
description: String,
}
N::Department {
name: String,
}
N::University {
name: String,
}
N::Lab {
name: String,
research_focus: String,
}
// EDGES //
E::HasLab {
From: Professor,
To: Lab,
}
E::HasResearchArea {
From: Professor,
To: ResearchArea,
}
E::HasUniversity {
From: Professor,
To: University,
Properties: {
since: Date DEFAULT NOW,
}
}
E::HasDepartment {
From: Professor,
To: Department,
Properties: {
since: Date DEFAULT NOW,
}
}
// VECTOR NODES //
V::ResearchAreaAndDescriptionEmbedding {
areas_and_descriptions: String,
}
E::HasResearchAreaAndDescriptionEmbedding {
From: Professor,
To: ResearchAreaAndDescriptionEmbedding,
Properties: {
areas_and_descriptions: String,
}
}
```


### Step 3: Create HelixQL Queries


Create` query.hx` :


```text
// Node Creation Queries
QUERY create_professor(name: String, title: String, page: String, bio: String ) =>
professor <- AddN<Professor>({ name: name, title: title, page: page, bio: bio })
RETURN professor
QUERY create_department(name: String) =>
department <- AddN<Department>({ name: name })
RETURN department
QUERY create_university(name: String) =>
university <- AddN<University>({ name: name })
RETURN university
QUERY create_lab(name: String, research_focus: String) =>
lab <- AddN<Lab>({ name: name, research_focus: research_focus })
RETURN lab
QUERY create_research_area(name: String) =>
research_area <- AddN<ResearchArea>({ research_area: name })
RETURN research_area
// Relationship Linking Queries
QUERY link_professor_to_department(professor_id: ID, department_id: ID) =>
professor <- N<Professor>(professor_id)
department <- N<Department>(department_id)
edge <- AddE<HasDepartment>::From(professor)::To(department)
RETURN edge
QUERY link_professor_to_university(professor_id: ID, university_id: ID) =>
professor <- N<Professor>(professor_id)
university <- N<University>(university_id)
edge <- AddE<HasUniversity>::From(professor)::To(university)
RETURN edge
QUERY link_professor_to_lab(professor_id: ID, lab_id: ID) =>
professor <- N<Professor>(professor_id)
lab <- N<Lab>(lab_id)
edge <- AddE<HasLab>::From(professor)::To(lab)
RETURN edge
QUERY link_professor_to_research_area(professor_id: ID, research_area_id: ID) =>
professor <- N<Professor>(professor_id)
research_area <- N<ResearchArea>(research_area_id)
edge <- AddE<HasResearchArea>::From(professor)::To(research_area)
RETURN edge
// Embedding Creation and Search
QUERY create_research_area_embedding(professor_id: ID, areas_and_descriptions: String, vector: [F64]) =>
professor <- N<Professor>(professor_id)
research_area <- AddV<ResearchAreaAndDescriptionEmbedding>(vector, { areas_and_descriptions: areas_and_descriptions })
edge <- AddE<HasResearchAreaAndDescriptionEmbedding>::From(professor)::To(research_area)
RETURN research_area
QUERY search_similar_professors_by_research_area_and_description(query_vector: [F64], k: I64) =>
vecs <- SearchV<ResearchAreaAndDescriptionEmbedding>(query_vector, k)
professors <- vecs::In<HasResearchAreaAndDescriptionEmbedding>
RETURN professors
QUERY get_professor_research_areas_with_descriptions(professor_id: ID) =>
research_areas <- N<Professor>(professor_id)::Out<HasResearchAreaAndDescriptionEmbedding>
RETURN research_areas::{areas_and_descriptions}
// Search Queries
QUERY get_professor_by_research_area_name(research_area_name: String) =>
professors <- N<Professor>::Out<HasResearchArea>::WHERE(_::{research_area}::EQ(research_area_name))
RETURN professors
QUERY get_professors_by_university_name(university_name: String) =>
professors <- N<Professor>::Out<HasUniversity>::WHERE(_::{name}::EQ(university_name))
RETURN professors
QUERY get_professors_by_department_name(department_name: String) =>
professors <- N<Professor>::Out<HasDepartment>::WHERE(_::{name}::EQ(department_name))
RETURN professors
QUERY get_professors_by_university_and_department_name(university_name: String, department_name: String) =>
professors <- N<Professor>::WHERE(AND(
EXISTS(_::Out<HasUniversity>::WHERE(_::{name}::EQ(university_name))),
EXISTS(_::Out<HasDepartment>::WHERE(_::{name}::EQ(department_name)))
))
RETURN professors
```


## Python Implementation


### Step 1: Environment Setup


```text
python -m venv venv


source venv/bin/activate


pip install helix-py sentence-transformers
```


```text
uv venv
uv add helix-py sentence-transformers
```


### Step 2: Initialize Connection and Model


```text
import helix
from sentence_transformers import SentenceTransformer
# Initialize embedding model (Qwen 0.6B for this example)
model = SentenceTransformer("Qwen/Qwen3-Embedding-0.6B")
# Connect to HelixDB
db = helix.Client(local=True, port=6969, verbose=True)
```


### Step 3: Create Base Nodes


```text
# Define research areas with descriptions
research_areas = {
"Computer Vision for Basketball": "Designing CNN and Transformer architectures that track player pose, ball trajectory, and court zones to quantify defensive pressure and shooting mechanics.",
"Predictive Modelling & Simulation": "Building Monte-Carlo and sequence models that forecast possession outcomes and season performance using play-by-play and spatial data.",
"Sports Analytics with Large Language Models": "Leveraging LLMs to explain model outputs, auto-generate commentary, and mine historical game archives for strategic patterns.",
"Wearable Sensor Data Mining": "Applying time-series and graph learning techniques to inertial-measurement signals for fatigue monitoring and injury prevention.",
"Fairness & Ethics in Sports AI": "Studying algorithmic bias and ensuring equitable analytics across different leagues, genders, and play styles."
}
# Create research area nodes and store IDs
research_area_ids = {}
for research_area in research_areas:
research_area_node = db.query("create_research_area", {"area": research_area})
research_area_ids[research_area] = research_area_node[0]['research_area']['id']
# Create department nodes
departments = ["Computer Science", "Mathematics", "Physics", "Chemistry", "Biology"]
department_ids = {}
for department in departments:
department_node = db.query("create_department", {"name": department})
department_ids[department] = department_node[0]['department']['id']
# Create university nodes
universities = ["Uni X", "Uni Y", "Uni Z"]
university_ids = {}
for university in universities:
university_node = db.query("create_university", {"name": university})
university_ids[university] = university_node[0]['university']['id']
# Create lab nodes
labs = {
"Basketball Data Science Lab": "An interdisciplinary group combining data science, biomechanics, and sport psychology to create next-generation analytics tools for basketball."
}
lab_ids = {}
for lab in labs:
lab_node = db.query("create_lab", {"name": lab, "research_focus": labs[lab]})
lab_ids[lab] = lab_node[0]['lab']['id']
```


### Step 4: Ingest Professor Data


```text
for professor in professors:
# Create Professor Node
professor_node = db.query("create_professor", {
"name": professor["name"],
"title": professor["title"],
"page": professor["page"],
"bio": professor["bio"]
})


professor_id = professor_node[0]['professor']['id']


# Link Professor to Research Areas
for research_area in professor["key_research_areas"]:
if research_area['area'] in research_areas:
research_area_id = research_area_ids[research_area['area']]
db.query("link_professor_to_research_area", {
"professor_id": professor_id,
"research_area_id": research_area_id
})


# Link Professor to Departments
for department in professor["department"]:
if department in department_ids:
department_id = department_ids[department]
db.query("link_professor_to_department", {
"professor_id": professor_id,
"department_id": department_id
})


# Link Professor to Universities
for university in professor["university"]:
if university in university_ids:
university_id = university_ids[university]
db.query("link_professor_to_university", {
"professor_id": professor_id,
"university_id": university_id
})


# Link Professor to Labs
for lab in professor["labs"]:
if lab['name'] in lab_ids:
lab_id = lab_ids[lab['name']]
db.query("link_professor_to_lab", {
"professor_id": professor_id,
"lab_id": lab_id
})


# Create and store research area embeddings
research_area_and_description = "\n".join([
research_area['area'] + ": " + research_area['description']
for research_area in professor['key_research_areas']
])


research_area_and_description_embedding = model.encode(research_area_and_description).astype(float).tolist()


db.query("create_research_area_embedding", {
"professor_id": professor_id,
"areas_and_descriptions": research_area_and_description,
"vector": research_area_and_description_embedding
})
```


## Querying the System


### Vector Similarity Search


Find professors with similar research interests:


```text
query = "Find me a professor who does computer vision for basketball"
embedded_query_vector = model.encode(query).astype(float).tolist()


results = db.query("search_similar_professors_by_research_area_and_description", {
"query_vector": embedded_query_vector,
"k": 5
})
print(results)
```


**Example Output:**


```text
[{'professors': [{
'page': '<a target="_blank" href="https://www.example.com">https://www.example.com</a>',
'label': 'Professor',
'bio': 'James is an Assistant Professor whose work sits at the intersection of basketball analytics...',
'name': 'James',
'id': '...',
'title': 'Assistant Professor'
}]}]
```


### Graph-Based Queries


#### 1. Find professors by research area


```text
professors_by_research_area = db.query("get_professor_by_research_area_name", {
"research_area_name": "Computer Vision for Basketball"
})
```


#### 2. Find professors by university


```text
professors_by_university = db.query("get_professors_by_university_name", {
"university_name": "Uni X"
})
```


#### 3. Find professors by department


```text
professors_by_department = db.query("get_professors_by_department_name", {
"department_name": "Computer Science"
})
```


#### 4. Complex filtering: University AND Department


```text
professors_filtered = db.query("get_professors_by_university_and_department_name", {
"university_name": "Uni X",
"department_name": "Computer Science"
})
```


### Retrieving Research Details


Get the full research areas and descriptions for a specific professor:


```text
prof_research_areas = db.query("get_professor_research_areas_with_descriptions", {
"professor_id": results[0]['professors'][0]['id']
})
print(prof_research_areas)
```


## Integration with LLMs


You can create custom tools for LLMs to interact with your HelixDB system. Here's an example using Google's Gemini:


```text
from google import genai
from google.genai import types
import dotenv
import os
dotenv.load_dotenv()
def search_similar_professors_by_research_area_and_description(query: str) -> dict:
"""Takes the user's query and embeds it then uses the embedded query to search for similar professors


Args:
query (str): The user's query


Returns:
A list of professors who are similar to the user's query
"""
embedded_query_vector = model.encode(query).astype(float).tolist()
"query_vector": embedded_query_vector,
"k": 5
})
return results
# Configure Gemini with the custom tool
client = genai.Client()
config = types.GenerateContentConfig(
tools=[search_similar_professors_by_research_area_and_description]
)
# Use the tool in conversation
response = client.models.generate_content(
model="gemini-2.5-flash",
contents="Find me a professor who does computer vision for basketball",
config=config,
)
print(response.text)
```


## Best Practices


1.


**Data Ingestion** : Process data in batches for large datasets


2.


**Embedding Models** : Choose models based on your domain (academic text vs general)


3.


**Index Management** : Create appropriate indexes for frequently queried fields


4.


**Query Optimization** : Use graph traversal for structured queries, vectors for semantic search


5.


**Caching** : Cache frequently accessed professor profiles and embeddings


## Conclusion


This Graph-Vector RAG system demonstrates the power of combining structured graph relationships with semantic vector search. By leveraging HelixDB's capabilities, we've created a system that can handle both exact matches (filtering by university/department) and semantic similarity (finding professors with related research interests).


The system is easily extensible - you can add new node types (publications, grants, collaborations), create more sophisticated embeddings (combining bio + research + publications), or integrate with various LLM providers for natural language interactions.


This approach scales well from small departmental databases to large-scale academic networks with millions of relationships, providing fast, accurate, and semantically-aware professor recommendations.
