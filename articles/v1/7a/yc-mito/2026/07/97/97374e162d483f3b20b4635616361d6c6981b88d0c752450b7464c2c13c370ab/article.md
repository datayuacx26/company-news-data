---
schema_version: "1.0.0"
document_id: "97374e162d483f3b20b4635616361d6c6981b88d0c752450b7464c2c13c370ab"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-connect-python-to-neo4j-graph-database-complete-guide"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:731d8255a9c8fe54d3f7bdd6d6221de66ffc7e5ced05a14440a6231e34177357"
---

# How to Connect Python to Neo4j Graph Database: Complete Guide

Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


Neo4j is the world's leading graph database, designed to store and query highly connected data efficiently. When combined with Python, Neo4j enables developers to build powerful applications that leverage relationships between data points. This comprehensive guide covers everything you need to know about connecting Python to Neo4j.


## Why Use Neo4j with Python?


Neo4j with Python offers powerful capabilities:


- Native graph storage optimized for relationships
- Powerful Cypher query language for pattern matching
- Excellent performance for traversing relationships
- ACID compliance for data integrity
- Real-time insights from connected data
- Perfect for social networks, recommendations, fraud detection
- Scalable architecture for enterprise applications
- Rich ecosystem of graph algorithms


## Prerequisites


Before connecting Python to Neo4j, ensure you have:


- Python 3.6 or higher installed
- Neo4j database installed (Community or Enterprise)
- Basic understanding of graph databases and Cypher
- Neo4j connection details (URI, username, password)


## Installing the Neo4j Driver


Install the official Neo4j Python driver:


bash


```text
pip install neo4j
```


## Basic Connection to Neo4j


Let's start with a simple connection:


python


```text
from neo4j import GraphDatabase


# Connection details
uri = "bolt://localhost:7687"
username = "neo4j"
password = "your_password"


# Create driver
driver = GraphDatabase.driver(uri, auth=(username, password))


# Verify connectivity
driver.verify_connectivity()


print("Connected to Neo4j successfully!")


# Close driver
driver.close()
```


## Connection with Error Handling


python


```text
from neo4j import GraphDatabase
from neo4j.exceptions import ServiceUnavailable, AuthError


def create_neo4j_connection():
try:
driver = GraphDatabase.driver(
"bolt://localhost:7687",
auth=("neo4j", "password"),
max_connection_lifetime=3600,
max_connection_pool_size=50,
connection_acquisition_timeout=120
)


# Verify connection
driver.verify_connectivity()


print("Successfully connected to Neo4j")


# Get server info
with driver.session() as session:
result = session.run("CALL dbms.components() YIELD versions")
version = result.single()[0][0]
print(f"Neo4j version: {version}")


return driver


except ServiceUnavailable as e:
print(f"Neo4j service unavailable: {e}")
return None
except AuthError as e:
print(f"Authentication failed: {e}")
return None
except Exception as e:
print(f"Error: {e}")
return None


# Usage
driver = create_neo4j_connection()
if driver:
driver.close()
```


## Creating Nodes


python


```text
from neo4j import GraphDatabase


driver = GraphDatabase.driver(
"bolt://localhost:7687",
auth=("neo4j", "password")
)


# Create a single node
with driver.session() as session:
result = session.run("""
CREATE (e:Employee {
employee_id: $employee_id,
first_name: $first_name,
last_name: $last_name,
email: $email,
department: $department,
salary: $salary
})
RETURN e
""", employee_id="EMP001", first_name="John", last_name="Doe",
email="john@company.com", department="Engineering", salary=75000)


record = result.single()
print(f"Created node: {record['e']}")


driver.close()
```


## Creating Multiple Nodes


python


```text
from neo4j import GraphDatabase


driver = GraphDatabase.driver(
"bolt://localhost:7687",
auth=("neo4j", "password")
)


# Create multiple nodes
employees = [
{"id": "EMP001", "name": "John Doe", "dept": "Engineering"},
{"id": "EMP002", "name": "Jane Smith", "dept": "Marketing"},
{"id": "EMP003", "name": "Bob Johnson", "dept": "Sales"}
]


with driver.session() as session:
for emp in employees:
session.run("""
CREATE (e:Employee {
employee_id: $id,
name: $name,
department: $dept
})
""", id=emp["id"], name=emp["name"], dept=emp["dept"])


print(f"Created {len(employees)} employee nodes")


driver.close()
```


## Creating Relationships


python


```text
from neo4j import GraphDatabase


driver = GraphDatabase.driver(
"bolt://localhost:7687",
auth=("neo4j", "password")
)


with driver.session() as session:
# Create two nodes and relationship
session.run("""
CREATE (e1:Employee {employee_id: 'EMP001', name: 'John Doe'})
CREATE (e2:Employee {employee_id: 'EMP002', name: 'Jane Smith'})
CREATE (e1)-[:REPORTS_TO {since: date('2024-01-01')}]->(e2)
""")


print("Nodes and relationship created!")


# Create relationship between existing nodes
session.run("""
MATCH (e1:Employee {employee_id: 'EMP003'})
MATCH (e2:Employee {employee_id: 'EMP002'})
CREATE (e1)-[:WORKS_WITH {project: 'ProjectX'}]->(e2)
""")


print("Relationship created between existing nodes!")


driver.close()
```


## Querying Nodes


### Find All Nodes


python


```text
from neo4j import GraphDatabase


driver = GraphDatabase.driver(
"bolt://localhost:7687",
auth=("neo4j", "password")
)


with driver.session() as session:
# Find all employee nodes
result = session.run("""
MATCH (e:Employee)
RETURN e.employee_id, e.name, e.department
ORDER BY e.name
```


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
