---
schema_version: "1.0.0"
document_id: "126e77ca0532df5f7f359543ed0cce6335a6254cc15d0829723fa0acdc2ea708"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-connect-python-to-mariadb-database-complete-guide"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:3af476e83b97446983d0bc49883e96b3be6a3e251372267ea85f08114e0c2c6b"
---

# How to Connect Python to MariaDB Database: Complete Guide

[See more blogs](https://www.trymito.io/blog)


# How to Connect Python to MariaDB Database: Complete Guide


The Mito Team


10/6/2025


8 min read


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


MariaDB is a popular open-source relational database management system that serves as a drop-in replacement for MySQL with additional features and performance improvements. When combined with Python, MariaDB provides a powerful platform for building robust, scalable applications. This comprehensive guide covers everything you need to know about connecting Python to MariaDB.


## Why Use MariaDB with Python?


MariaDB with Python offers powerful capabilities:


- Drop-in replacement for MySQL with better performance
- Open-source with no licensing concerns
- Advanced storage engines (Aria, ColumnStore)
- Better query optimization and execution
- Enhanced replication features
- JSON support and dynamic columns
- Active community development
- Excellent compatibility with MySQL tools


## Prerequisites


Before connecting Python to MariaDB, ensure you have:


- Python 3.6 or higher installed
- MariaDB server installed and running
- Database credentials (host, port, username, password, database)
- Basic understanding of Python and SQL


## Installing the MariaDB Connector


The official MariaDB connector for Python provides optimal performance:


bash


```text
pip install mariadb
```


Alternatively, you can use the MySQL connector which is compatible:


bash


```text
pip install mysql-connector-python
```


We'll primarily use the official MariaDB connector in this guide.


## Basic Connection to MariaDB


Let's start with a simple connection:


python


```text
import mariadb
import sys


try:
# Connect to MariaDB
connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
port=3306,
database="your_database"
)


print("Connected to MariaDB successfully!")
print(f"MariaDB version: {connection.server_version}")


# Close connection
connection.close()


except mariadb.Error as e:
print(f"Error connecting to MariaDB: {e}")
sys.exit(1)
```


## Connection with Configuration Dictionary


python


```text
import mariadb


# Connection configuration
config = {
'host': 'localhost',
'port': 3306,
'user': 'your_username',
'password': 'your_password',
'database': 'your_database'
}


try:
connection = mariadb.connect(**config)
print("Connected to MariaDB!")
connection.close()
except mariadb.Error as e:
print(f"Error: {e}")
```


## Connection with Error Handling and Pooling


python


```text
import mariadb
from mariadb import Error


def create_connection():
try:
connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
port=3306,
database="your_database",
autocommit=False,
connect_timeout=10
)


print("Successfully connected to MariaDB")


# Get server info
cursor = connection.cursor()
cursor.execute("SELECT VERSION()")
version = cursor.fetchone()[0]
print(f"MariaDB version: {version}")
cursor.close()


return connection


except Error as e:
print(f"Error connecting to MariaDB: {e}")
return None


# Usage
conn = create_connection()
if conn:
conn.close()
```


## Creating Tables


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Create employees table
create_table_query = """
CREATE TABLE IF NOT EXISTS employees (
id INT AUTO_INCREMENT PRIMARY KEY,
first_name VARCHAR(50) NOT NULL,
last_name VARCHAR(50) NOT NULL,
email VARCHAR(100) UNIQUE,
department VARCHAR(50),
salary DECIMAL(10, 2),
hire_date DATE,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB
"""


cursor.execute(create_table_query)
connection.commit()


print("Table created successfully!")


cursor.close()
connection.close()
```


## Inserting Data


### Insert Single Record


python


```text
import mariadb
from datetime import date


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Insert single employee
insert_query = """
INSERT INTO employees (first_name, last_name, email, department, salary, hire_date)
VALUES (?, ?, ?, ?, ?, ?)
"""


employee_data = ('John', 'Doe', 'john.doe@company.com', 'Engineering', 75000.00, date.today())


cursor.execute(insert_query, employee_data)
connection.commit()


print(f"Employee inserted with ID: {cursor.lastrowid}")


cursor.close()
connection.close()
```


### Bulk Insert Operations


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


insert_query = """
INSERT INTO employees (first_name, last_name, email, department, salary)
VALUES (?, ?, ?, ?, ?)
"""


employees_data = [
('Jane', 'Smith', 'jane@company.com', 'Marketing', 68000.00),
('Bob', 'Johnson', 'bob@company.com', 'Sales', 72000.00),
('Alice', 'Williams', 'alice@company.com', 'Engineering', 80000.00),
('Charlie', 'Brown', 'charlie@company.com', 'HR', 62000.00)
]


cursor.executemany(insert_query, employees_data)
connection.commit()


print(f"{cursor.rowcount} employees inserted successfully!")


cursor.close()
connection.close()
```


## Querying Data


### Fetch All Records


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Select all employees
select_query = "SELECT id, first_name, last_name, department, salary FROM employees"
cursor.execute(select_query)


rows = cursor.fetchall()


print(f"Total employees: {len(rows)}\n")


for row in rows:
print(f"ID: {row[0]}, Name: {row[1]} {row[2]}, Dept: {row[3]}, Salary: ${row[4]}")


cursor.close()
connection.close()
```


### Fetch with Dictionary Cursor


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor(dictionary=True)


# Query with dictionary access
select_query = "SELECT * FROM employees WHERE department = ?"
cursor.execute(select_query, ('Engineering',))


employees = cursor.fetchall()


for emp in employees:
print(f"Name: {emp['first_name']} {emp['last_name']}, Salary: ${emp['salary']}")


cursor.close()
connection.close()
```


### Parameterized Queries


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Safe parameterized query
select_query = "SELECT * FROM employees WHERE salary BETWEEN ? AND ?"
cursor.execute(select_query, (70000, 80000))


employees = cursor.fetchall()


print("Employees with salary $70k-$80k:")
for emp in employees:
print(f"{emp[1]} {emp[2]} - ${emp[5]}")


cursor.close()
connection.close()
```


## Updating Records


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Update employee salary
update_query = "UPDATE employees SET salary = ? WHERE id = ?"
cursor.execute(update_query, (85000.00, 1))
connection.commit()


print(f"{cursor.rowcount} record(s) updated")


# Update multiple employees
update_query = "UPDATE employees SET salary = salary * 1.10 WHERE department = ?"
cursor.execute(update_query, ('Engineering',))
connection.commit()


print(f"{cursor.rowcount} Engineering employees received raises")


cursor.close()
connection.close()
```


## Deleting Records


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Delete employee by ID
delete_query = "DELETE FROM employees WHERE id = ?"
cursor.execute(delete_query, (10,))
connection.commit()


print(f"{cursor.rowcount} record(s) deleted")


cursor.close()
connection.close()
```


## Working with Transactions


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database",
autocommit=False
)


cursor = connection.cursor()


try:
# Start transaction
cursor.execute("UPDATE employees SET salary = salary - 5000 WHERE id = 1")
cursor.execute("UPDATE employees SET salary = salary + 5000 WHERE id = 2")


# Commit transaction
connection.commit()
print("Transaction completed successfully")


except mariadb.Error as e:
# Rollback on error
connection.rollback()
print(f"Transaction failed: {e}")


finally:
cursor.close()
connection.close()
```


## Using Stored Procedures


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Create stored procedure
proc_sql = """
CREATE PROCEDURE IF NOT EXISTS GetEmployeesByDepartment(
IN dept_name VARCHAR(50)
)
BEGIN
SELECT * FROM employees
WHERE department = dept_name
ORDER BY last_name;
END
"""


cursor.execute(proc_sql)
connection.commit()


# Call stored procedure
cursor.callproc('GetEmployeesByDepartment', ['Engineering'])


# Fetch results
for result in cursor.stored_results():
employees = result.fetchall()
print(f"Found {len(employees)} engineers")
for emp in employees:
print(f"{emp[1]} {emp[2]}")


cursor.close()
connection.close()
```


## Working with JSON Columns


MariaDB supports JSON data type:


python


```text
import mariadb
import json


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Create table with JSON column
cursor.execute("""
CREATE TABLE IF NOT EXISTS employee_metadata (
employee_id INT PRIMARY KEY,
preferences JSON,
skills JSON
)
""")


# Insert JSON data
preferences = {"theme": "dark", "notifications": True, "language": "en"}
skills = ["Python", "SQL", "Docker"]


cursor.execute("""
INSERT INTO employee_metadata (employee_id, preferences, skills)
VALUES (?, ?, ?)
""", (1, json.dumps(preferences), json.dumps(skills)))


connection.commit()


# Query JSON data
cursor.execute("SELECT preferences, skills FROM employee_metadata WHERE employee_id = 1")
row = cursor.fetchone()


prefs = json.loads(row[0])
emp_skills = json.loads(row[1])


print(f"Theme: {prefs['theme']}")
print(f"Skills: {', '.join(emp_skills)}")


# Query using JSON functions
cursor.execute("""
SELECT JSON_EXTRACT(preferences, '$.theme') as theme
FROM employee_metadata
WHERE employee_id = 1
""")


theme = cursor.fetchone()[0]
print(f"Theme from JSON_EXTRACT: {theme}")


cursor.close()
connection.close()
```


## Connection Pooling


python


```text
import mariadb


# Create connection pool
pool = mariadb.ConnectionPool(
pool_name="my_pool",
pool_size=5,
pool_reset_connection=True,
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


print(f"Connection pool created with {pool.pool_size} connections")


# Get connection from pool
connection = pool.get_connection()


cursor = connection.cursor()
cursor.execute("SELECT COUNT(*) FROM employees")
count = cursor.fetchone()[0]
print(f"Total employees: {count}")


cursor.close()
connection.close()  # Returns connection to pool
```


## Using Context Managers


python


```text
import mariadb
from contextlib import closing


def get_connection():
return mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


# Using context manager
with closing(get_connection()) as conn:
with closing(conn.cursor(dictionary=True)) as cursor:
cursor.execute("SELECT * FROM employees WHERE salary > ?", (70000,))
employees = cursor.fetchall()


for emp in employees:
print(f"{emp['first_name']} {emp['last_name']} - ${emp['salary']}")
```


## Working with Dynamic Columns


MariaDB's dynamic columns allow flexible schema:


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Create table with dynamic column
cursor.execute("""
CREATE TABLE IF NOT EXISTS employee_extras (
employee_id INT PRIMARY KEY,
extra_data BLOB
)
""")


# Insert dynamic column data
cursor.execute("""
INSERT INTO employee_extras (employee_id, extra_data)
VALUES (?, COLUMN_CREATE('certifications', 'AWS,Azure', 'languages', 'English,Spanish'))
""", (1,))


connection.commit()


# Query dynamic column
cursor.execute("""
SELECT
COLUMN_GET(extra_data, 'certifications' AS CHAR) as certs,
COLUMN_GET(extra_data, 'languages' AS CHAR) as langs
FROM employee_extras
WHERE employee_id = 1
""")


row = cursor.fetchone()
print(f"Certifications: {row[0]}")
print(f"Languages: {row[1]}")


cursor.close()
connection.close()
```


## Batch Processing


python


```text
import mariadb


connection = mariadb.connect(
user="your_username",
password="your_password",
host="localhost",
database="your_database"
)


cursor = connection.cursor()


# Prepare data for batch insert
employees = []
for i in range(1000):
employees.append((
f'FirstName{i}',
f'LastName{i}',
f'email{i}@company.com',
'Engineering',
70000 + (i * 100)
))


# Batch insert
insert_query = """
INSERT INTO employees (first_name, last_name, email, department, salary)
VALUES (?, ?, ?, ?, ?)
"""


cursor.executemany(insert_query, employees)
connection.commit()


print(f"Batch inserted {cursor.rowcount} employees")


cursor.close()
connection.close()
```


## Complete Application Example


python


```text
import mariadb
import sys
from contextlib import closing
from datetime import date


class EmployeeDatabase:
def __init__(self, host, user, password, database):
self.config = {
'host': host,
'user': user,
'password': password,
'database': database,
'autocommit': False
}
self.pool = self._create_pool()


def _create_pool(self):
try:
return mariadb.ConnectionPool(
pool_name="employee_pool",
pool_size=5,
**self.config
)
except mariadb.Error as e:
print(f"Error creating pool: {e}")
sys.exit(1)


def get_connection(self):
return self.pool.get_connection()


def create_employee(self, first_name, last_name, email, department, salary):
with closing(self.get_connection()) as conn:
with closing(conn.cursor()) as cursor:
query = """
VALUES (?, ?, ?, ?, ?, ?)
"""


cursor.execute(query, (first_name, last_name, email, department, salary, date.today()))
conn.commit()


return cursor.lastrowid


def get_employee(self, employee_id):
with closing(self.get_connection()) as conn:
with closing(conn.cursor(dictionary=True)) as cursor:
cursor.execute("SELECT * FROM employees WHERE id = ?", (employee_id,))
return cursor.fetchone()


def get_employees_by_department(self, department):
with closing(self.get_connection()) as conn:
with closing(conn.cursor(dictionary=True)) as cursor:
cursor.execute(
"SELECT * FROM employees WHERE department = ? ORDER BY last_name",
(department,)
)
return cursor.fetchall()


def update_salary(self, employee_id, new_salary):
with closing(self.get_connection()) as conn:
with closing(conn.cursor()) as cursor:
cursor.execute(
"UPDATE employees SET salary = ? WHERE id = ?",
(new_salary, employee_id)
)
conn.commit()
return cursor.rowcount


def delete_employee(self, employee_id):
with closing(self.get_connection()) as conn:
with closing(conn.cursor()) as cursor:
cursor.execute("DELETE FROM employees WHERE id = ?", (employee_id,))
conn.commit()
return cursor.rowcount


def get_department_statistics(self):
with closing(self.get_connection()) as conn:
with closing(conn.cursor(dictionary=True)) as cursor:
cursor.execute("""
SELECT
department,
COUNT(*) as employee_count,
AVG(salary) as avg_salary,
MIN(salary) as min_salary,
MAX(salary) as max_salary
FROM employees
GROUP BY department
ORDER BY employee_count DESC
""")
return cursor.fetchall()


def search_employees(self, search_term):
with closing(self.get_connection()) as conn:
with closing(conn.cursor(dictionary=True)) as cursor:
query = """
SELECT * FROM employees
WHERE first_name LIKE ? OR last_name LIKE ? OR email LIKE ?
"""
search_pattern = f'%{search_term}%'
cursor.execute(query, (search_pattern, search_pattern, search_pattern))
return cursor.fetchall()


# Usage
db = EmployeeDatabase(
host='localhost',
user='your_username',
password='your_password',
database='your_database'
)


# Create employee
emp_id = db.create_employee('Sarah', 'Connor', 'sarah@company.com', 'Security', 78000)
print(f"Created employee with ID: {emp_id}")


# Get employee
employee = db.get_employee(emp_id)
print(f"Employee: {employee['first_name']} {employee['last_name']}")


# Get employees by department
engineers = db.get_employees_by_department('Engineering')
print(f"Found {len(engineers)} engineers")


# Update salary
rows_updated = db.update_salary(emp_id, 82000)
print(f"Updated {rows_updated} employee(s)")


# Get statistics
stats = db.get_department_statistics()
for stat in stats:
print(f"\n{stat['department']}:")
print(f"  Employees: {stat['employee_count']}")
print(f"  Avg Salary: ${stat['avg_salary']:.2f}")


# Search employees
results = db.search_employees('Sarah')
print(f"\nSearch results: {len(results)} found")
```


## Best Practices


Follow these best practices when using MariaDB with Python:


1. **Always use parameterized queries** to prevent SQL injection
2. **Close connections and cursors** to free resources
3. **Use connection pooling** for better performance
4. **Implement proper error handling** with try-except blocks
5. **Use transactions** for related operations
6. **Enable autocommit=False** for better control
7. **Use dictionary cursors** for readable code
8. **Create indexes** on frequently queried columns
9. **Monitor connection pool** usage
10. **Use context managers** for automatic cleanup
11. **Optimize queries** with EXPLAIN
12. **Set appropriate timeouts** for connections
13. **Use UTF-8 encoding** for international characters
14. **Regular backups** of important data
15. **Monitor slow query log** for performance issues


## Conclusion


Connecting Python to MariaDB provides a powerful foundation for building robust, scalable applications. The MariaDB connector offers excellent performance, comprehensive features, and seamless integration. By following the examples and best practices in this guide, you'll be able to build efficient applications that leverage MariaDB's advanced features, improved performance over MySQL, and open-source flexibility. Whether you're building web applications, data analytics platforms, or enterprise systems, mastering Python-MariaDB connectivity is essential for modern application development.


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
