---
schema_version: "1.0.0"
document_id: "89674946dacafecb5543084284377c0e3ba5a32b079e87183f2256935b8f4e40"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/pgrouting-postgres-graph-database"
published_at: "2025-02-25T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:58d3bf1691dcabc49673483bca0fb0142660e718803e5da3da06f88d1e85409c"
---

# Postgres as a Graph Database: (Ab)using pgRouting

[pgRouting](https://github.com/pgRouting/pgrouting) is a Postgres extension. It's often used for finding the “shortest path” between two locations, however it's a hidden gem in Postgres and can be used for basic graph functionality.


pgRouting is typically combined with PostGIS for working with geospatial data, but it can also be[useful beyond that](https://news.ycombinator.com/item?id=26350454) as a lightweight alternative to Graph extensions like[Apache AGE](https://age.apache.org/) , or specialized graph databases like[Neo4j](https://neo4j.com/) .


Let's explore some useful applications of pgRouting and graphs.


## What is pgRouting?#


pgRouting is an extension of PostGIS that provides geospatial routing functionality. You can use it to calculate the shortest path, perform network analysis, and solve complex routing problems on a graph-based structure. Most commonly, this is used in Geographic Information Systems (GIS) for tasks like determining the fastest route between two locations.


## Working with Graphs#


The power of pgRouting lies in its ability to work with any data structured as a graph. A graph is essentially a network of interconnected points, where:


- **Nodes** represent entities.
- **Edges** represent relationships or paths between those nodes.


In maps /[GIS](https://en.wikipedia.org/wiki/Geographic_information_system) , nodes and edges represent intersections and roads respectively. However, this structure can also be applied to abstract systems like a social networks, where users are nodes and friendships are edges.


## Non-GIS Use Cases for pgRouting#


Let's explore how pgRouting can be applied to a few non-[GIS](https://en.wikipedia.org/wiki/Geographic_information_system) problems.


### Task scheduling#


In any project, tasks have dependencies. For example, task B can only start after task A is completed. This creates a[directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (DAG), where:


- nodes represent tasks
- edges represent dependencies


One of the most challenging aspects of managing projects is determining the “critical path” — the project's overall duration, determined by the longest sequence of dependencies.


Using pgRouting, you can model your task's dependencies, using graph algorithms to find the critical path. Suppose we have a table tasks with task dependencies modeled as a graph:


`
_35


-- Create the tasks table with dependencies


_35


create table tasks (


_35


id serial primary key,


_35


name text not null


_35


);


_35


_35


-- insert tasks into the table


_35


insert into tasks (name)


_35


values


_35


('Start Project'),


_35


('Task A'),


_35


('Task B'),


_35


('Task C'),


_35


('Task D'),


_35


('End Project');


_35


_35


-- create the dependencies table


_35


create table dependencies (


_35


id serial primary key,


_35


source integer not null, -- task id where the dependency starts


_35


target integer not null, -- task id where the dependency ends


_35


duration integer not null, -- duration of the task in days


_35


constraint fk_source foreign key (source) references tasks (id),


_35


constraint fk_target foreign key (target) references tasks (id)


_35


);


_35


_35


-- insert dependencies with durations (directed edges)


_35


insert into dependencies (source, target, duration)


_35


values


_35


(1, 2, 3), -- start project -> task a (3 days)


_35


(2, 3, 4), -- task a -> task b (4 days)


_35


(3, 4, 5), -- task b -> task c (5 days)


_35


(4, 5, 2), -- task c -> task d (2 days)


_35


(5, 6, 6);


_35


-- task d -> end project (6 days)


`


You can then use the[pgr_dijkstra()](https://docs.pgrouting.org/latest/en/pgr_dijkstra.html) function to find the shortest (or longest) path through the tasks, allowing you to map out the project schedule effectively:


`
_10


create schema if not exists extensions;


_10


create extension pgrouting schema extensions cascade;


_10


_10


-- find the longest path using pgr_dijkstra()


_10


-- (as it calculates shortest path, use negative weights)


_10


select * FROM extensions.pgr_dijkstra(


_10


'select id, source, target, duration as cost from dependencies',


_10


1, -- Start Project (Task ID 1)


_10


6 -- End Project (Task ID 6)


_10


);


`


Which returns a table showing that this project will take 20 days from start to finish:


**seq** **path_seq** **node** **edge** **cost** **agg_cost**


1 1 1 1 3 0


2 2 2 2 4 3


3 3 3 3 5 7


4 4 4 4 2 12


5 5 5 5 6 14


6 6 6 -1 0 20


Tangent: the Dijkstra algorithm


The` pgr_dijkstra()` function implements[Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra's_algorithm) , which is used to find the shortest path between nodes in a graph. This algorithm guarantees the shortest path from a source node to a target node (or all other nodes), based on the cost of edges connecting the nodes.


Fun fact: Dijkstra's algorithm was published in 1959 by Dutch computer scientist Edsger Dijkstra. It's a “greedy” algorithm, meaning it always picks the closest, cheapest node to explore next.


### Reverse proxy routing based on resource allocation#


Distributed systems usually involve allocating resources efficiently across a network of nodes. Each node might represent a physical location or a computing process, and the edges represent the available pathways to move resources between them. For example, in a cloud infrastructure, pgRouting could help determine how to allocate compute tasks across a set of distributed servers by finding the shortest or least-congested path to route data.


Suppose you have a network of servers represented by nodes and their data connections as edges in a table servers.


`
_38


-- create the servers table representing the nodes


_38


create table servers (


_38


id serial primary key,


_38


name text,


_38


x double precision, -- x coordinate for spatial data (latitude)


_38


y double precision -- y coordinate for spatial data (longitude)


_38


);


_38


_38


-- insert some sample servers


_38


insert into servers (name, x, y)


_38


values


_38


('server a', 0, 0),


_38


('server b', 2, 1),


_38


('server c', 4, 3),


_38


('server d', 3, 5);


_38


_38


-- create the server_connections table representing the edges


_38


create table server_latency (


_38


id serial primary key,


_38


source integer,


_38


target integer,


_38


cost double precision, -- cost could represent latency or bandwidth


_38


x1 double precision, -- x coordinate of source


_38


y1 double precision, -- y coordinate of source


_38


x2 double precision, -- x coordinate of target


_38


y2 double precision, -- y coordinate of target,


_38


constraint fk_source foreign key (source) references servers (id),


_38


constraint fk_target foreign key (target) references servers (id)


_38


);


_38


_38


-- insert connections between servers


_38


insert into server_latency (source, target, cost, x1, y1, x2, y2)


_38


values


_38


(1, 2, 1.5, 0, 0, 2, 1), -- server a -> server b with a cost of 1.5 (could be latency)


_38


(2, 3, 2.0, 2, 1, 4, 3), -- server b -> server c with a cost of 2.0


_38


(2, 4, 1.8, 2, 1, 3, 5), -- server b -> server d with a cost of 1.8


_38


(4, 3, 1.0, 3, 5, 4, 3);


_38


-- server d -> server c with a cost of 1.0


`


You can then use[pgr_astar](https://docs.pgrouting.org/latest/en/pgr_aStar.html) () to find the most efficient path for data or compute tasks to travel through this network, optimizing for speed or load:


`
_10


-- Query to find the most efficient path (using pgr_astar)


_10


select *


_10


from


_10


extensions.pgr_astar(


_10


'select id, source, target, cost, x1, y1, x2, y2 from server_latency',


_10


1,


_10


3 -- Start from Server A (id=1) to Server C (id=3)


_10


);


`


Tangent: the A* algorithm


The` pgr_astar()` function is an implementation of the[A* (A-star) algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) . It's used to find the most efficient (shortest) path between two points in a graph. A* is commonly used in navigation and routing because it is more efficient than Dijkstra's algorithm in many scenarios, especially when you have spatial data with coordinates (e.g., X, Y positions).


Fun fact: A* was originally designed in the 1960s for artificial intelligence applications and pathfinding in games. Today, it's one of the most widely used algorithms in video game development to help characters navigate complex environments efficiently.


### Recommendation engines like YouTube#


In recommendation engines or search algorithms that use knowledge graphs, pgRouting can be used to build relationships between entities and events. Take YouTube's recommendation algorithm, we can structure this data as a graph where:


- **Nodes** represent entities like users, videos, or categories.
- **Edges** represent relationships or interactions between those entities, such as a user liking a video or videos being part of the same category.


Let's create a list of “nodes”:


`
_24


create table categories (


_24


id serial primary key,


_24


name text


_24


);


_24


_24


insert into categories (name)


_24


values


_24


('Graph Theory'),


_24


('AI & Machine Learning'),


_24


('Python Programming');


_24


_24


create table videos (


_24


id serial primary key,


_24


title text,


_24


category_id int references categories (id)


_24


);


_24


_24


insert into videos (title, category_id)


_24


values


_24


('Intro to Graph Theory', 1),


_24


('Advanced Graph Algorithms', 1),


_24


('Graph Neural Networks', 2),


_24


('Beginner Python Tutorial', 3),


_24


('Advanced Python Techniques', 3);


`


And some “edges”:


`
_27


create table video_relationships (


_27


source_video_id int references videos (id),


_27


target_video_id int references videos (id),


_27


relationship_type text, -- 'same_category', 'watched_by_same_users', etc.


_27


weight int default 1 -- strength of the relationship


_27


);


_27


_27


insert into video_relationships (source_video_id, target_video_id, relationship_type, weight)


_27


values


_27


(1, 2, 'same_category', 5), -- "Intro to Graph Theory" and "Advanced Graph Algorithms" are in the same category


_27


(2, 3, 'watched_by_same_users', 3), -- "Advanced Graph Algorithms" and "Graph Neural Networks" are often watched together


_27


(4, 5, 'same_category', 5); -- "Beginner Python Tutorial


_27


create table interactions (


_27


user_id int references auth.users (id),


_27


video_id int references videos (id),


_27


interaction_type text, -- 'liked', 'viewed', etc.


_27


weight int default 1 -- strength of the interaction


_27


);


_27


_27


insert into interactions (user_id, video_id, interaction_type, weight)


_27


values


_27


('user_01', 1, 'viewed', 5), -- "User 01" watched "Intro to Graph Theory" to the end (weight = 5)


_27


('user_01', 2, 'liked', 5), -- "User 01" liked "Advanced Graph Algorithms"


_27


('user_02', 3, 'viewed', 2), -- "User 02" watched "Graph Neural Networks" and bounced halfway through (weight = 2)


_27


('user_03', 4, 'liked', 5), -- "User 03" liked "Beginner Python Tutorial"


_27


('user_03', 5, 'viewed', 2);


_27


-- "User 03" watched "Advanced Python Techniques" and bounced halfway through (weight = 2)


`


Now we can use the` pgr_dijkstra()` function to find the shortest or most relevant path between a user and new videos. For example, let's find videos that are most relevant to` user_01` considering their past interactions:


Tangent: ranking recommendations


Since it's “just postgres” it's simple enough to rank the results using an` order by` clause. For example, if we stored the` pgr_dijkstra()` results above in a table called “recommendations”, we use this a query like this to sort the paths by the highest ranking:


`
_10


select videos.title, sum(weight) as recommendation_score


_10


from


_10


recommendations


_10


join videos on recommendations.target = videos.id


_10


group by videos.title


_10


order by recommendation_score desc;


`


## Get Started#


pgRouting is a powerful extension for Postgres that can be used to solve a wide range of graph-based problems. Check out the[pgRouting docs](https://docs.pgrouting.org/latest/en/index.html) for more information on how to use it. You can also use it on Supabase:


- Docs:[pgrouting: Geospatial Routing](https://supabase.com/docs/guides/database/extensions/pgrouting)
- Launch a new Postgres database:[database.new](https://database.new/)
