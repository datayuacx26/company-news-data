---
schema_version: "1.0.0"
document_id: "4c0bc8cafe55ce25c149fe7e081e73bb242e22043326bd9994f31054b210c016"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-connect-python-to-cockroachdb-complete-guide"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:7d4687813648eb63babd201b40c4d81e187dbb6105b24874d05609684dd082d8"
---

# How to Connect Python to CockroachDB: Complete Guide

[See more blogs](https://www.trymito.io/blog)


# How to Connect Python to CockroachDB: Complete Guide


The Mito Team


10/6/2025


10 min read


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


CockroachDB is a distributed SQL database designed for cloud-native applications. It combines the familiarity of SQL with the scalability of NoSQL, offering horizontal scaling, strong consistency, and automatic failover. This comprehensive guide covers everything you need to know about connecting Python to CockroachDB.


## Why Use CockroachDB with Python?


CockroachDB with Python offers powerful capabilities:


- Distributed SQL database with PostgreSQL compatibility
- Automatic horizontal scaling across multiple nodes
- Strong consistency with ACID guarantees
- Built-in replication and automatic failover
- Geo-distributed capabilities for global applications
- Survives infrastructure failures automatically
- Compatible with PostgreSQL drivers and tools
- No manual sharding required


## Prerequisites


Before connecting Python to CockroachDB, ensure you have:


- Python 3.6 or higher installed
- CockroachDB cluster running (local or cloud)
- psycopg2 driver (CockroachDB is PostgreSQL-compatible)
- Connection details (host, port, database, user, password/certificates)
- Basic understanding of SQL and distributed systems


## Installing Required Libraries


Since CockroachDB is PostgreSQL-compatible, we use psycopg2:


bash


```text
pip install psycopg2-binary
```


For production, also install:


bash


```text
pip install sqlalchemy cockroachdb-python
```


## Basic Connection to CockroachDB


### Local Connection


python


```text
import psycopg2


# Connect to local CockroachDB
connection = psycopg2.connect(
host='localhost',
port=26257,
database='defaultdb',
user='root',
sslmode='disable'  # Only for local dev
)


print("Connected to CockroachDB successfully!")


# Get version
cursor = connection.cursor()
cursor.execute("SELECT version()")
version = cursor.fetchone()[0]
print(f"CockroachDB version: {version}")


cursor.close()
connection.close()
```


### Secure Connection with Certificates


python


```text
import psycopg2


# Connect with SSL certificates (production)
connection = psycopg2.connect(
host='your-cluster.cockroachlabs.cloud',
port=26257,
database='your_database',
user='your_username',
password='your_password',
sslmode='verify-full',
sslrootcert='/path/to/ca.crt'
)


print("Connected to CockroachDB Cloud!")


connection.close()
```


## Connection with Error Handling


python


```text
import psycopg2
from psycopg2 import OperationalError, Error


def create_connection():
try:
connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


print("Successfully connected to CockroachDB")


# Set default search path
cursor = connection.cursor()
cursor.execute("SET search_path = public")
cursor.close()


return connection


except OperationalError as e:
print(f"Operational error: {e}")
return None
except Error as e:
print(f"Error connecting: {e}")
return None


# Usage
conn = create_connection()
if conn:
conn.close()
```


## Creating Tables


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Create employees table
create_table_sql = """
CREATE TABLE IF NOT EXISTS employees (
id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
first_name STRING NOT NULL,
last_name STRING NOT NULL,
email STRING UNIQUE,
department STRING,
salary DECIMAL(10, 2),
hire_date DATE,
created_at TIMESTAMP DEFAULT now()
)
"""


cursor.execute(create_table_sql)
connection.commit()


print("Table created successfully!")


cursor.close()
connection.close()
```


## Inserting Data


### Insert Single Record


python


```text
import psycopg2
from datetime import date


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Insert single employee
insert_sql = """
INSERT INTO employees (first_name, last_name, email, department, salary, hire_date)
VALUES (%s, %s, %s, %s, %s, %s)
RETURNING id
"""


employee_data = ('John', 'Doe', 'john.doe@company.com', 'Engineering', 75000.00, date.today())


cursor.execute(insert_sql, employee_data)
employee_id = cursor.fetchone()[0]
connection.commit()


print(f"Employee inserted with ID: {employee_id}")


cursor.close()
connection.close()
```


### Bulk Insert with Retry Logic


CockroachDB may require retries for transaction contention:


python


```text
import psycopg2
from psycopg2 import Error
import time


def insert_employees_with_retry(connection, employees_data, max_retries=3):
for attempt in range(max_retries):
try:
cursor = connection.cursor()


insert_sql = """
INSERT INTO employees (first_name, last_name, email, department, salary)
VALUES (%s, %s, %s, %s, %s)
"""


cursor.executemany(insert_sql, employees_data)
connection.commit()


print(f"Successfully inserted {cursor.rowcount} employees")
cursor.close()
return True


except Error as e:
if "40001" in str(e):  # Retry error code
print(f"Transaction retry needed (attempt {attempt + 1})")
connection.rollback()
time.sleep(0.1 * (2 ** attempt))  # Exponential backoff
else:
print(f"Error: {e}")
connection.rollback()
return False


print("Max retries exceeded")
return False


# Usage
connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


employees_data = [
('Jane', 'Smith', 'jane@company.com', 'Marketing', 68000.00),
('Bob', 'Johnson', 'bob@company.com', 'Sales', 72000.00),
('Alice', 'Williams', 'alice@company.com', 'Engineering', 80000.00)
]


insert_employees_with_retry(connection, employees_data)


connection.close()
```


## Querying Data


### Basic Queries


python


```text
import psycopg2
from psycopg2.extras import RealDictCursor


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


# Use RealDictCursor for dictionary access
cursor = connection.cursor(cursor_factory=RealDictCursor)


# Select all employees
select_sql = "SELECT * FROM employees ORDER BY last_name"
cursor.execute(select_sql)


employees = cursor.fetchall()


print(f"Total employees: {len(employees)}\n")


for emp in employees:
print(f"Name: {emp['first_name']} {emp['last_name']}, Dept: {emp['department']}")


cursor.close()
connection.close()
```


### Parameterized Queries


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Query with parameters
select_sql = "SELECT * FROM employees WHERE department = %s AND salary > %s"
cursor.execute(select_sql, ('Engineering', 70000))


employees = cursor.fetchall()


print("Senior Engineers:")
for emp in employees:
print(f"{emp[1]} {emp[2]} - ${emp[5]}")


cursor.close()
connection.close()
```


## Updating Records with Retry Logic


python


```text
import psycopg2
from psycopg2 import Error
import time


def update_with_retry(connection, update_func, max_retries=3):
for attempt in range(max_retries):
try:
cursor = connection.cursor()
update_func(cursor)
connection.commit()
cursor.close()
return True


except Error as e:
if "40001" in str(e):
print(f"Retry needed (attempt {attempt + 1})")
connection.rollback()
time.sleep(0.1 * (2 ** attempt))
else:
print(f"Error: {e}")
connection.rollback()
return False


return False


# Usage
connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


def update_salary(cursor):
cursor.execute(
"UPDATE employees SET salary = %s WHERE id = %s",
(85000.00, 'some-uuid-here')
)


success = update_with_retry(connection, update_salary)
print(f"Update {'succeeded' if success else 'failed'}")


connection.close()
```


## Distributed Transactions


python


```text
import psycopg2
from psycopg2 import Error


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


try:
# Begin transaction
cursor.execute("BEGIN")


# Transfer budget between departments
cursor.execute("UPDATE departments SET budget = budget - 10000 WHERE name = 'Engineering'")
cursor.execute("UPDATE departments SET budget = budget + 10000 WHERE name = 'Marketing'")


# Commit transaction
cursor.execute("COMMIT")
print("Transaction completed successfully")


except Error as e:
cursor.execute("ROLLBACK")
print(f"Transaction failed: {e}")


finally:
cursor.close()
connection.close()
```


## Working with Geo-Partitioned Data


CockroachDB excels at geo-distribution:


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Create geo-partitioned table
create_table_sql = """
CREATE TABLE IF NOT EXISTS user_data (
id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
user_id INT,
region STRING NOT NULL,
data JSONB,
created_at TIMESTAMP DEFAULT now()
) PARTITION BY LIST (region)
"""


cursor.execute(create_table_sql)


# Create partitions for different regions
cursor.execute("""
ALTER TABLE user_data PARTITION BY LIST (region) (
PARTITION us_east VALUES IN ('us-east'),
PARTITION us_west VALUES IN ('us-west'),
PARTITION europe VALUES IN ('europe'),
PARTITION asia VALUES IN ('asia')
)
""")


connection.commit()


# Insert geo-partitioned data
cursor.execute("""
INSERT INTO user_data (user_id, region, data)
VALUES (%s, %s, %s)
""", (1001, 'us-east', '{"preferences": {"theme": "dark"}}'))


connection.commit()


print("Geo-partitioned table created and data inserted!")


cursor.close()
connection.close()
```


## Using JSONB Columns


python


```text
import psycopg2
import json


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Create table with JSONB
cursor.execute("""
CREATE TABLE IF NOT EXISTS employee_metadata (
employee_id UUID PRIMARY KEY,
metadata JSONB
)
""")


# Insert JSONB data
metadata = {
"skills": ["Python", "SQL", "Docker"],
"certifications": ["AWS", "Azure"],
"projects": [{"name": "Project A", "role": "Lead"}]
}


cursor.execute("""
INSERT INTO employee_metadata (employee_id, metadata)
VALUES (gen_random_uuid(), %s)
""", (json.dumps(metadata),))


connection.commit()


# Query JSONB data
cursor.execute("""
SELECT metadata->>'skills' as skills,
metadata->'certifications' as certs
FROM employee_metadata
""")


for row in cursor.fetchall():
print(f"Skills: {row[0]}")
print(f"Certifications: {row[1]}")


cursor.close()
connection.close()
```


## Using SQLAlchemy with CockroachDB


SQLAlchemy provides a higher-level ORM interface:


python


```text
from sqlalchemy import create_engine, Column, Integer, String, Float, Date
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from datetime import date


# Create engine
engine = create_engine(
'cockroachdb://root@localhost:26257/company?sslmode=disable',
echo=True
)


Base = declarative_base()


# Define model
class Employee(Base):
__tablename__ = 'employees_orm'


id = Column(Integer, primary_key=True)
first_name = Column(String(50), nullable=False)
last_name = Column(String(50), nullable=False)
email = Column(String(100), unique=True)
department = Column(String(50))
salary = Column(Float)
hire_date = Column(Date)


# Create tables
Base.metadata.create_all(engine)


# Create session
Session = sessionmaker(bind=engine)
session = Session()


# Insert data
new_employee = Employee(
first_name='John',
last_name='Doe',
email='john@company.com',
department='Engineering',
salary=75000.00,
hire_date=date.today()
)


session.add(new_employee)
session.commit()


print(f"Employee created with ID: {new_employee.id}")


# Query data
engineers = session.query(Employee).filter_by(department='Engineering').all()


print(f"\nEngineers: {len(engineers)}")
for emp in engineers:
print(f"{emp.first_name} {emp.last_name} - ${emp.salary}")


# Update data
employee = session.query(Employee).filter_by(email='john@company.com').first()
if employee:
employee.salary = 80000.00
session.commit()
print(f"Updated {employee.first_name}'s salary")


# Delete data
session.delete(employee)
session.commit()


session.close()
```


## Change Data Capture (CDC)


CockroachDB supports changefeeds for streaming changes:


python


```text
import psycopg2
import time


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Create changefeed (Enterprise feature)
cursor.execute("""
CREATE CHANGEFEED FOR TABLE employees
INTO 'kafka://localhost:9092'
WITH updated, resolved
""")


print("Changefeed created for employees table")


cursor.close()
connection.close()
```


## Backup and Restore


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Create backup
backup_location = 'nodelocal://1/backups'


cursor.execute(f"""
BACKUP TABLE employees
TO '{backup_location}'
""")


print("Backup created successfully")


# Restore backup
cursor.execute(f"""
RESTORE TABLE employees
FROM '{backup_location}'
""")


print("Backup restored successfully")


cursor.close()
connection.close()
```


## Performance Optimization


### Creating Indexes


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Create single column index
cursor.execute("""
CREATE INDEX IF NOT EXISTS idx_department
ON employees (department)
""")


# Create composite index
cursor.execute("""
CREATE INDEX IF NOT EXISTS idx_dept_salary
ON employees (department, salary DESC)
""")


# Create covering index
cursor.execute("""
CREATE INDEX IF NOT EXISTS idx_email_covering
ON employees (email) STORING (first_name, last_name)
""")


connection.commit()


print("Indexes created successfully")


cursor.close()
connection.close()
```


### Query Optimization


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Use EXPLAIN to analyze queries
cursor.execute("""
EXPLAIN ANALYZE
SELECT * FROM employees
WHERE department = 'Engineering' AND salary > 70000
""")


plan = cursor.fetchall()
print("Query Plan:")
for row in plan:
print(row[0])


# Collect statistics for better query planning
cursor.execute("CREATE STATISTICS emp_stats FROM employees")
connection.commit()


print("\nStatistics created for query optimization")


cursor.close()
connection.close()
```


## Complete Application Example


python


```text
import psycopg2
from psycopg2.extras import RealDictCursor
from psycopg2 import Error
import time
from contextlib import closing


class CockroachDBManager:
def __init__(self, host, port, database, user, sslmode='disable'):
self.config = {
'host': host,
'port': port,
'database': database,
'user': user,
'sslmode': sslmode
}


def get_connection(self):
return psycopg2.connect(**self.config)


def execute_with_retry(self, operation, max_retries=3):
"""Execute operation with automatic retry for transaction conflicts"""
for attempt in range(max_retries):
try:
with closing(self.get_connection()) as conn:
with closing(conn.cursor(cursor_factory=RealDictCursor)) as cursor:
result = operation(cursor)
conn.commit()
return result
except Error as e:
if "40001" in str(e) and attempt < max_retries - 1:
time.sleep(0.1 * (2 ** attempt))
continue
else:
raise
return None


def create_employee(self, first_name, last_name, email, department, salary):
def operation(cursor):
cursor.execute("""
VALUES (%s, %s, %s, %s, %s, CURRENT_DATE)
RETURNING id
""", (first_name, last_name, email, department, salary))
return cursor.fetchone()['id']


return self.execute_with_retry(operation)


def get_employee(self, employee_id):
def operation(cursor):
cursor.execute("SELECT * FROM employees WHERE id = %s", (employee_id,))
return cursor.fetchone()


return self.execute_with_retry(operation)


def get_employees_by_department(self, department):
def operation(cursor):
cursor.execute(
"SELECT * FROM employees WHERE department = %s ORDER BY last_name",
(department,)
)
return cursor.fetchall()


return self.execute_with_retry(operation)


def update_salary(self, employee_id, new_salary):
def operation(cursor):
cursor.execute(
"UPDATE employees SET salary = %s WHERE id = %s",
(new_salary, employee_id)
)
return cursor.rowcount


return self.execute_with_retry(operation)


def transfer_budget(self, from_dept, to_dept, amount):
"""Atomic budget transfer between departments"""
def operation(cursor):
cursor.execute("""
UPDATE departments
SET budget = budget - %s
WHERE name = %s AND budget >= %s
""", (amount, from_dept, amount))


if cursor.rowcount == 0:
raise Exception("Insufficient budget")


cursor.execute("""
UPDATE departments
SET budget = budget + %s
WHERE name = %s
""", (amount, to_dept))


return True


return self.execute_with_retry(operation)


def get_department_statistics(self):
def operation(cursor):
cursor.execute("""
SELECT
department,
COUNT(*) as employee_count,
AVG(salary) as avg_salary,
MIN(salary) as min_salary,
MAX(salary) as max_salary
FROM employees
WHERE department IS NOT NULL
GROUP BY department
ORDER BY employee_count DESC
""")
return cursor.fetchall()


return self.execute_with_retry(operation)


def search_employees(self, search_term):
def operation(cursor):
cursor.execute("""
SELECT * FROM employees
WHERE first_name ILIKE %s
OR last_name ILIKE %s
OR email ILIKE %s
""", (f'%{search_term}%', f'%{search_term}%', f'%{search_term}%'))
return cursor.fetchall()


return self.execute_with_retry(operation)


# Usage
db = CockroachDBManager(
host='localhost',
port=26257,
database='company',
user='root'
)


# Create employee
try:
emp_id = db.create_employee('Sarah', 'Connor', 'sarah@company.com', 'Security', 78000)
print(f"Created employee with ID: {emp_id}")
except Exception as e:
print(f"Error creating employee: {e}")


# Get employee
employee = db.get_employee(emp_id)
if employee:
print(f"Employee: {employee['first_name']} {employee['last_name']}")


# Get employees by department
engineers = db.get_employees_by_department('Engineering')
print(f"\nFound {len(engineers)} engineers")


# Update salary with automatic retry
rows_updated = db.update_salary(emp_id, 82000)
print(f"Updated {rows_updated} employee(s)")


# Get statistics
stats = db.get_department_statistics()
print("\nDepartment Statistics:")
for stat in stats:
print(f"{stat['department']}: {stat['employee_count']} employees, "
f"Avg Salary: ${stat['avg_salary']:.2f}")


# Search employees
results = db.search_employees('Sarah')
print(f"\nSearch results: {len(results)} found")
```


## Multi-Region Configuration


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Add regions to cluster
cursor.execute("""
ALTER DATABASE company CONFIGURE ZONE USING
num_replicas = 3,
constraints = '[+region=us-east, +region=us-west, +region=europe]'
""")


# Set table locality
cursor.execute("""
ALTER TABLE employees SET LOCALITY REGIONAL BY ROW
""")


connection.commit()


print("Multi-region configuration applied")


cursor.close()
connection.close()
```


## Monitoring and Observability


python


```text
import psycopg2


connection = psycopg2.connect(
host='localhost',
port=26257,
database='company',
user='root',
sslmode='disable'
)


cursor = connection.cursor()


# Check cluster health
cursor.execute("SHOW CLUSTER SETTING cluster.organization")
print(f"Cluster organization: {cursor.fetchone()[0]}")


# View active queries
cursor.execute("""
SELECT query_id, node_id, user_name, start, query
FROM crdb_internal.cluster_queries
WHERE query NOT LIKE '%crdb_internal%'
""")


print("\nActive Queries:")
for row in cursor.fetchall():
print(f"Query ID: {row[0]}, User: {row[2]}")
print(f"Query: {row[4][:80]}...")


# Check node status
cursor.execute("SELECT node_id, address, is_live FROM crdb_internal.gossip_nodes")


print("\nNode Status:")
for row in cursor.fetchall():
status = "Live" if row[2] else "Dead"
print(f"Node {row[0]} ({row[1]}): {status}")


cursor.close()
connection.close()
```


## Best Practices


Follow these best practices when using CockroachDB with Python:


1. **Implement retry logic** for transaction conflicts (error 40001)
2. **Use UUID primary keys** for better distribution
3. **Design for distribution** with appropriate partitioning
4. **Use batch operations** when inserting multiple rows
5. **Create appropriate indexes** for query optimization
6. **Monitor transaction contention** and optimize hot spots
7. **Use connection pooling** for better performance
8. **Set appropriate transaction isolation** levels
9. **Leverage JSONB** for flexible schemas
10. **Use REGIONAL tables** for geo-distributed applications
11. **Implement proper error handling** with exponential backoff
12. **Avoid sequential keys** that create hotspots
13. **Use EXPLAIN ANALYZE** to optimize queries
14. **Monitor cluster metrics** regularly
15. **Plan for multi-region** deployment from the start


## Conclusion


Connecting Python to CockroachDB provides a powerful foundation for building globally distributed, resilient applications. CockroachDB's PostgreSQL compatibility makes it easy to get started, while its distributed architecture provides automatic scaling and high availability. By following the examples and best practices in this guide, you'll be able to build robust applications that leverage CockroachDB's unique capabilities including automatic failover, geo-partitioning, and strong consistency guarantees. Whether you're building cloud-native applications, multi-region services, or systems requiring high availability, mastering Python-CockroachDB connectivity is essential for modern distributed application development.


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
