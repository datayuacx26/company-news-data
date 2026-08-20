---
schema_version: "1.0.0"
document_id: "923d9c72e2c80319000b4e2256516b804219994f3d7755beda7cc93556cf4046"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-connect-python-to-cassandra-database-complete-guide-2"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:af5b1bd3c3b7d4463f4937c6c001ac4b6e5e5ca95327cec17da84701baa35570"
---

# How to Connect Python to Cassandra Database: Complete Guide

[See more blogs](https://www.trymito.io/blog)


# How to Connect Python to Cassandra Database: Complete Guide


The Mito Team


10/6/2025


6 min read


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


Apache Cassandra is a highly scalable, distributed NoSQL database designed for handling large amounts of data across multiple servers. When combined with Python, Cassandra provides exceptional performance for big data applications. This comprehensive guide covers everything you need to know about connecting Python to Cassandra.


## Why Use Cassandra with Python?


Cassandra with Python offers powerful capabilities:


- Massive horizontal scalability across multiple nodes
- High availability with no single point of failure
- Linear performance scalability
- Excellent write performance for time-series data
- Tunable consistency levels
- Multi-datacenter replication
- Fault-tolerant architecture
- Perfect for IoT, analytics, and real-time applications


## Prerequisites


Before connecting Python to Cassandra, ensure you have:


- Python 3.6 or higher installed
- Cassandra installed locally or access to a Cassandra cluster
- Basic understanding of CQL (Cassandra Query Language)
- Cassandra connection details (contact points, port, keyspace)


## Installing the Cassandra Driver


The DataStax Python driver is the official client for Cassandra:


bash


```text
pip install cassandra-driver
```


## Basic Connection to Cassandra


Let's start with a simple connection:


python


```text
from cassandra.cluster import Cluster


# Connect to Cassandra
cluster = Cluster(['127.0.0.1'])
session = cluster.connect()


print("Connected to Cassandra successfully!")


# Close connection
session.shutdown()
cluster.shutdown()
```


## Connection with Multiple Contact Points


For production clusters with multiple nodes:


python


```text
from cassandra.cluster import Cluster


# Connect to cluster with multiple contact points
cluster = Cluster(
contact_points=['192.168.1.1', '192.168.1.2', '192.168.1.3'],
port=9042
)


session = cluster.connect()


print("Connected to Cassandra cluster!")


session.shutdown()
cluster.shutdown()
```


## Connection with Authentication


python


```text
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider


# Setup authentication
auth_provider = PlainTextAuthProvider(
username='cassandra',
password='cassandra'
)


# Connect with authentication
cluster = Cluster(
contact_points=['127.0.0.1'],
auth_provider=auth_provider
)


session = cluster.connect()


print("Connected with authentication!")


session.shutdown()
cluster.shutdown()
```


## Connection with Error Handling


python


```text
from cassandra.cluster import Cluster, NoHostAvailable
from cassandra.auth import PlainTextAuthProvider


def create_cassandra_connection():
try:
auth_provider = PlainTextAuthProvider(
username='cassandra',
password='cassandra'
)


cluster = Cluster(
contact_points=['127.0.0.1'],
port=9042,
auth_provider=auth_provider
)


session = cluster.connect()


print("Successfully connected to Cassandra")


# Get cluster metadata
print(f"Cluster name: {cluster.metadata.cluster_name}")
print(f"Cassandra version: {list(cluster.metadata.all_hosts())[0].release_version}")


return cluster, session


except NoHostAvailable as e:
print(f"No Cassandra hosts available: {e}")
return None, None
except Exception as e:
print(f"Error connecting to Cassandra: {e}")
return None, None


# Usage
cluster, session = create_cassandra_connection()
if session:
session.shutdown()
cluster.shutdown()
```


## Creating a Keyspace


Keyspaces in Cassandra are like databases in relational systems:


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect()


# Create keyspace
create_keyspace_query = """
CREATE KEYSPACE IF NOT EXISTS company
WITH replication = {
'class': 'SimpleStrategy',
'replication_factor': 1
}
"""


session.execute(create_keyspace_query)
print("Keyspace created successfully!")


# Use the keyspace
session.set_keyspace('company')


session.shutdown()
cluster.shutdown()
```


## Creating Tables


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Create employees table
create_table_query = """
CREATE TABLE IF NOT EXISTS employees (
employee_id UUID PRIMARY KEY,
first_name TEXT,
last_name TEXT,
email TEXT,
department TEXT,
salary DECIMAL,
hire_date DATE
)
"""


session.execute(create_table_query)
print("Table created successfully!")


# Create index on department
session.execute("CREATE INDEX IF NOT EXISTS ON employees (department)")


session.shutdown()
cluster.shutdown()
```


## Inserting Data


### Insert Single Record


python


```text
from cassandra.cluster import Cluster
import uuid
from datetime import date


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Insert single employee
insert_query = """
INSERT INTO employees (employee_id, first_name, last_name, email, department, salary, hire_date)
VALUES (?, ?, ?, ?, ?, ?, ?)
"""


employee_id = uuid.uuid4()


session.execute(insert_query, (
employee_id,
'John',
'Doe',
'john.doe@company.com',
'Engineering',
75000.00,
date.today()
))


print(f"Employee inserted with ID: {employee_id}")


session.shutdown()
cluster.shutdown()
```


### Batch Insert


python


```text
from cassandra.cluster import Cluster
from cassandra.query import BatchStatement, SimpleStatement
import uuid


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Create batch statement
batch = BatchStatement()


insert_query = SimpleStatement("""
INSERT INTO employees (employee_id, first_name, last_name, email, department, salary)
VALUES (?, ?, ?, ?, ?, ?)
""")


# Add multiple inserts to batch
employees_data = [
(uuid.uuid4(), 'Jane', 'Smith', 'jane@company.com', 'Marketing', 68000),
(uuid.uuid4(), 'Bob', 'Johnson', 'bob@company.com', 'Sales', 72000),
(uuid.uuid4(), 'Alice', 'Williams', 'alice@company.com', 'Engineering', 80000)
]


for emp_data in employees_data:
batch.add(insert_query, emp_data)


# Execute batch
session.execute(batch)


print(f"{len(employees_data)} employees inserted in batch!")


session.shutdown()
cluster.shutdown()
```


## Querying Data


### Simple Queries


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Select all employees
rows = session.execute("SELECT * FROM employees")


print("All Employees:")
for row in rows:
print(f"Name: {row.first_name} {row.last_name}, Dept: {row.department}, Salary: ${row.salary}")


session.shutdown()
cluster.shutdown()
```


### Queries with Conditions


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Query by indexed column
query = "SELECT * FROM employees WHERE department = ?"
prepared = session.prepare(query)


rows = session.execute(prepared, ('Engineering',))


print("Engineering Department:")
for row in rows:
print(f"{row.first_name} {row.last_name} - ${row.salary}")


session.shutdown()
cluster.shutdown()
```


### Using Prepared Statements


Prepared statements improve performance:


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Prepare statement
prepared_query = session.prepare("""
SELECT * FROM employees WHERE department = ?
""")


# Execute prepared statement
rows = session.execute(prepared_query, ('Engineering',))


for row in rows:
print(f"{row.first_name} {row.last_name}")


session.shutdown()
cluster.shutdown()
```


## Updating Records


python


```text
from cassandra.cluster import Cluster
import uuid


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Update employee salary
employee_id = uuid.UUID('some-uuid-here')


update_query = """
UPDATE employees
SET salary = ?
WHERE employee_id = ?
"""


session.execute(update_query, (85000.00, employee_id))


print("Employee salary updated!")


session.shutdown()
cluster.shutdown()
```


## Deleting Records


python


```text
from cassandra.cluster import Cluster
import uuid


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Delete employee
employee_id = uuid.UUID('some-uuid-here')


delete_query = "DELETE FROM employees WHERE employee_id = ?"
session.execute(delete_query, (employee_id,))


print("Employee deleted!")


session.shutdown()
cluster.shutdown()
```


## Working with Collections


Cassandra supports collection types:


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Create table with collections
session.execute("""
CREATE TABLE IF NOT EXISTS employee_skills (
employee_id UUID PRIMARY KEY,
name TEXT,
skills SET<TEXT>,
certifications LIST<TEXT>,
project_hours MAP<TEXT, INT>
)
""")


# Insert with collections
import uuid


emp_id = uuid.uuid4()


session.execute("""
INSERT INTO employee_skills (employee_id, name, skills, certifications, project_hours)
VALUES (?, ?, ?, ?, ?)
""", (
emp_id,
'John Doe',
{'Python', 'Java', 'SQL'},  # SET
['AWS Certified', 'PMP'],  # LIST
{'ProjectA': 120, 'ProjectB': 80}  # MAP
))


# Query collections
rows = session.execute("SELECT * FROM employee_skills WHERE employee_id = ?", (emp_id,))


for row in rows:
print(f"Skills: {row.skills}")
print(f"Certifications: {row.certifications}")
print(f"Project Hours: {row.project_hours}")


# Update collections
session.execute("""
UPDATE employee_skills
SET skills = skills + ?
WHERE employee_id = ?
""", ({'Cassandra'}, emp_id))


print("Skill added to set!")


session.shutdown()
cluster.shutdown()
```


## Time-Series Data Pattern


Cassandra excels at time-series data:


python


```text
from cassandra.cluster import Cluster
from datetime import datetime
import uuid


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Create time-series table
session.execute("""
CREATE TABLE IF NOT EXISTS sensor_data (
sensor_id UUID,
timestamp TIMESTAMP,
temperature FLOAT,
humidity FLOAT,
PRIMARY KEY (sensor_id, timestamp)
) WITH CLUSTERING ORDER BY (timestamp DESC)
""")


# Insert time-series data
sensor_id = uuid.uuid4()


for i in range(10):
session.execute("""
INSERT INTO sensor_data (sensor_id, timestamp, temperature, humidity)
VALUES (?, ?, ?, ?)
""", (sensor_id, datetime.now(), 22.5 + i*0.1, 45.0 + i*0.5))


print("Time-series data inserted!")


# Query latest readings
rows = session.execute("""
SELECT * FROM sensor_data
WHERE sensor_id = ?
LIMIT 5
""", (sensor_id,))


print("Latest 5 readings:")
for row in rows:
print(f"{row.timestamp}: Temp={row.temperature}°C, Humidity={row.humidity}%")


session.shutdown()
cluster.shutdown()
```


## Using TTL (Time To Live)


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Insert with TTL (expires after 3600 seconds)
session.execute("""
INSERT INTO employees (employee_id, first_name, last_name, email)
VALUES (uuid(), 'Temp', 'User', 'temp@company.com')
USING TTL 3600
""")


print("Temporary record inserted with TTL!")


session.shutdown()
cluster.shutdown()
```


## Pagination


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Set fetch size for pagination
query = "SELECT * FROM employees"
statement = session.prepare(query)
statement.fetch_size = 10


# Execute with pagination
result_set = session.execute(statement)


page_num = 1
for row in result_set:
print(f"Page {page_num}: {row.first_name} {row.last_name}")


# Check if we've moved to next page
if result_set.has_more_pages:
print("Fetching next page...")
page_num += 1


session.shutdown()
cluster.shutdown()
```


## Asynchronous Queries


python


```text
from cassandra.cluster import Cluster


cluster = Cluster(['127.0.0.1'])
session = cluster.connect('company')


# Execute async query
future = session.execute_async("SELECT * FROM employees")


# Do other work while query executes
print("Query executing in background...")


# Wait for result
try:
rows = future.result()
print(f"Query complete! Retrieved {len(list(rows))} rows")
except Exception as e:
print(f"Query failed: {e}")


session.shutdown()
cluster.shutdown()
```


## Complete Application Example


python


```text
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider
from cassandra.query import SimpleStatement
import uuid
from datetime import date
from contextlib import closing


class EmployeeDatabase:
def __init__(self, contact_points, keyspace):
self.contact_points = contact_points
self.keyspace = keyspace
self.cluster = None
self.session = None
self.connect()


def connect(self):
self.cluster = Cluster(self.contact_points)
self.session = self.cluster.connect(self.keyspace)
print("Connected to Cassandra")


def create_employee(self, first_name, last_name, email, department, salary):
employee_id = uuid.uuid4()


query = """
VALUES (?, ?, ?, ?, ?, ?, ?)
"""


self.session.execute(query, (
employee_id, first_name, last_name, email, department, salary, date.today()
))


return employee_id


def get_employees_by_department(self, department):
query = "SELECT * FROM employees WHERE department = ?"
prepared = self.session.prepare(query)
return list(self.session.execute(prepared, (department,)))


def update_salary(self, employee_id, new_salary):
query = "UPDATE employees SET salary = ? WHERE employee_id = ?"
self.session.execute(query, (new_salary, employee_id))


def delete_employee(self, employee_id):
query = "DELETE FROM employees WHERE employee_id = ?"
self.session.execute(query, (employee_id,))


def close(self):
if self.session:
self.session.shutdown()
if self.cluster:
self.cluster.shutdown()
print("Connection closed")


# Usage
db = EmployeeDatabase(['127.0.0.1'], 'company')


# Create employee
emp_id = db.create_employee('Sarah', 'Connor', 'sarah@company.com', 'Security', 78000)
print(
```


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
