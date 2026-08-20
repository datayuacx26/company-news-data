---
schema_version: "1.0.0"
document_id: "38d60ad81f5b7f0bae01c7523c257ed2e476d3910549d6342081269065e210d0"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-connect-python-to-microsoft-sql-server-complete-guide"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:a2cdc92a9bafd8fcf7de4f730725937954fe215a76d952de8a693c621ef5d294"
---

# How to Connect Python to Microsoft SQL Server: Complete Guide

[See more blogs](https://www.trymito.io/blog)


# How to Connect Python to Microsoft SQL Server: Complete Guide


The Mito Team


10/6/2025


6 min read


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


Microsoft SQL Server is a powerful relational database management system widely used in enterprise environments. Connecting Python to SQL Server allows developers to build robust data-driven applications combining Python's flexibility with SQL Server's enterprise features. This comprehensive guide covers everything you need to know about Python-SQL Server connectivity.


## Why Use SQL Server with Python?


SQL Server combined with Python offers powerful capabilities:


- Seamless integration with Microsoft ecosystem
- Excellent Windows environment support
- Advanced security and encryption features
- High availability and disaster recovery
- Powerful T-SQL language for complex operations
- Integration Services (SSIS) for ETL
- Analysis Services for business intelligence
- Strong support for JSON and XML data


## Prerequisites


Before connecting Python to SQL Server, ensure you have:


- Python 3.6 or higher installed
- SQL Server installed (Express, Standard, or Enterprise)
- ODBC Driver for SQL Server installed
- Database credentials (server, database, username, password)
- Basic understanding of Python and T-SQL


## Installing pyodbc


The most popular library for connecting Python to SQL Server is pyodbc:


bash


```text
pip install pyodbc
```


Alternatively, you can use pymssql:


bash


```text
pip install pymssql
```


We'll primarily use pyodbc as it's more feature-rich and widely adopted.


## Installing SQL Server ODBC Driver


Download and install the Microsoft ODBC Driver for SQL Server from Microsoft's website. Check installed drivers:


python


```text
import pyodbc


# List available ODBC drivers
drivers = pyodbc.drivers()
print("Available ODBC drivers:")
for driver in drivers:
print(f"  - {driver}")
```


## Basic Connection to SQL Server


Let's start with a simple connection:


python


```text
import pyodbc


# Connection parameters
server = 'localhost'
database = 'TestDB'
username = 'sa'
password = 'YourPassword123'


# Create connection string
connection_string = f'DRIVER={{ODBC Driver 17 for SQL Server}};SERVER={server};DATABASE={database};UID={username};PWD={password}'


# Establish connection
connection = pyodbc.connect(connection_string)


print("Connected to SQL Server successfully!")


# Close connection
connection.close()
```


## Windows Authentication


For Windows Authentication (trusted connection):


python


```text
import pyodbc


server = 'localhost'
database = 'TestDB'


# Connection string for Windows Authentication
connection_string = f'DRIVER={{ODBC Driver 17 for SQL Server}};SERVER={server};DATABASE={database};Trusted_Connection=yes'


connection = pyodbc.connect(connection_string)


print("Connected using Windows Authentication!")


connection.close()
```


## Connection with Error Handling


python


```text
import pyodbc


def create_sql_connection():
try:
connection_string = (
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


connection = pyodbc.connect(connection_string)


print("Successfully connected to SQL Server")


# Get SQL Server version
cursor = connection.cursor()
cursor.execute("SELECT @@VERSION")
version = cursor.fetchone()[0]
print(f"SQL Server version: {version[:50]}...")
cursor.close()


return connection


except pyodbc.Error as e:
print(f"Database error: {e}")
return None
except Exception as e:
print(f"Error: {e}")
return None


# Usage
conn = create_sql_connection()
if conn:
conn.close()
```


## Creating Tables


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Create employees table
create_table_sql = """
CREATE TABLE employees (
id INT IDENTITY(1,1) PRIMARY KEY,
first_name NVARCHAR(50) NOT NULL,
last_name NVARCHAR(50) NOT NULL,
email NVARCHAR(100) UNIQUE,
department NVARCHAR(50),
salary DECIMAL(10, 2),
hire_date DATE DEFAULT GETDATE()
)
"""


try:
cursor.execute(create_table_sql)
connection.commit()
print("Table created successfully!")
except pyodbc.Error as e:
print(f"Error creating table: {e}")


cursor.close()
connection.close()
```


## Inserting Data


### Insert Single Record


python


```text
import pyodbc
from datetime import date


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Insert single employee
insert_sql = """
INSERT INTO employees (first_name, last_name, email, department, salary, hire_date)
VALUES (?, ?, ?, ?, ?, ?)
"""


employee_data = ('John', 'Doe', 'john.doe@company.com', 'Engineering', 75000.00, date.today())


cursor.execute(insert_sql, employee_data)
connection.commit()


# Get the inserted ID
cursor.execute("SELECT @@IDENTITY")
new_id = cursor.fetchone()[0]
print(f"Employee inserted with ID: {int(new_id)}")


cursor.close()
connection.close()
```


SQL Server uses` ?` as placeholders for parameterized queries.


### Insert Multiple Records


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


insert_sql = """
INSERT INTO employees (first_name, last_name, email, department, salary)
VALUES (?, ?, ?, ?, ?)
"""


employees_data = [
('Jane', 'Smith', 'jane@company.com', 'Marketing', 68000.00),
('Bob', 'Johnson', 'bob@company.com', 'Sales', 72000.00),
('Alice', 'Williams', 'alice@company.com', 'Engineering', 80000.00),
('Charlie', 'Brown', 'charlie@company.com', 'HR', 62000.00)
]


cursor.executemany(insert_sql, employees_data)
connection.commit()


print(f"{cursor.rowcount} employees inserted successfully!")


cursor.close()
connection.close()
```


## Querying Data


### Fetch All Records


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Select all employees
select_sql = "SELECT id, first_name, last_name, department, salary FROM employees"
cursor.execute(select_sql)


rows = cursor.fetchall()


print(f"Total employees: {len(rows)}\n")


for row in rows:
print(f"ID: {row.id}, Name: {row.first_name} {row.last_name}, Dept: {row.department}, Salary: ${row.salary}")


cursor.close()
connection.close()
```


### Fetch with Conditions


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Find employees in Engineering department
select_sql = "SELECT * FROM employees WHERE department = ?"
cursor.execute(select_sql, 'Engineering')


engineers = cursor.fetchall()


print("Engineering Department:")
for emp in engineers:
print(f"{emp.first_name} {emp.last_name} - ${emp.salary}")


cursor.close()
connection.close()
```


### Using Row Dictionary


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Query with dictionary-like access
select_sql = "SELECT id, first_name, last_name, salary FROM employees WHERE salary > ?"
cursor.execute(select_sql, 70000)


# Get column names
columns = [column[0] for column in cursor.description]


# Create dictionaries
employees = []
for row in cursor.fetchall():
employee = dict(zip(columns, row))
employees.append(employee)


for emp in employees:
print(f"{emp['first_name']} {emp['last_name']} - ${emp['salary']}")


cursor.close()
connection.close()
```


## Updating Records


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Update employee salary
update_sql = "UPDATE employees SET salary = ? WHERE id = ?"
cursor.execute(update_sql, 85000.00, 1)
connection.commit()


print(f"{cursor.rowcount} record(s) updated")


# Update multiple employees
update_sql = "UPDATE employees SET salary = salary * 1.10 WHERE department = ?"
cursor.execute(update_sql, 'Engineering')
connection.commit()


print(f"{cursor.rowcount} Engineering employees received raises")


cursor.close()
connection.close()
```


## Deleting Records


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Delete employee by ID
delete_sql = "DELETE FROM employees WHERE id = ?"
cursor.execute(delete_sql, 10)
connection.commit()


print(f"{cursor.rowcount} record(s) deleted")


cursor.close()
connection.close()
```


## Working with Stored Procedures


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Create stored procedure
proc_sql = """
CREATE PROCEDURE GetEmployeesByDepartment
@DeptName NVARCHAR(50)
AS
BEGIN
SELECT * FROM employees
WHERE department = @DeptName
ORDER BY last_name
END
"""


try:
cursor.execute(proc_sql)
connection.commit()
print("Stored procedure created!")
except pyodbc.Error as e:
print(f"Error: {e}")


# Call stored procedure
cursor.execute("EXEC GetEmployeesByDepartment ?", 'Engineering')


employees = cursor.fetchall()


print(f"Found {len(employees)} employees in Engineering")
for emp in employees:
print(f"{emp.first_name} {emp.last_name}")


cursor.close()
connection.close()
```


## Working with Output Parameters


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Create procedure with output parameter
proc_sql = """
CREATE PROCEDURE GetEmployeeCount
@DeptName NVARCHAR(50),
@EmpCount INT OUTPUT
AS
BEGIN
SELECT @EmpCount = COUNT(*)
FROM employees
WHERE department = @DeptName
END
"""


cursor.execute(proc_sql)
connection.commit()


# Call procedure with output parameter
dept = 'Engineering'
cursor.execute(f"DECLARE @count INT; EXEC GetEmployeeCount ?, @count OUTPUT; SELECT @count", dept)


count = cursor.fetchone()[0]
print(f"Employee count in {dept}: {count}")


cursor.close()
connection.close()
```


## Working with Transactions


python


```text
import pyodbc


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


try:
# Start transaction
cursor.execute("BEGIN TRANSACTION")


cursor.execute("UPDATE employees SET salary = salary - 5000 WHERE id = 1")
cursor.execute("UPDATE employees SET salary = salary + 5000 WHERE id = 2")


# Commit transaction
cursor.execute("COMMIT TRANSACTION")
connection.commit()
print("Transaction completed successfully")


except pyodbc.Error as e:
# Rollback on error
cursor.execute("ROLLBACK TRANSACTION")
connection.commit()
print(f"Transaction failed: {e}")


finally:
cursor.close()
connection.close()
```


## Using Context Managers


python


```text
import pyodbc
from contextlib import closing


def get_connection():
return pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


with closing(get_connection()) as conn:
with closing(conn.cursor()) as cursor:
cursor.execute("SELECT COUNT(*) FROM employees")
count = cursor.fetchone()[0]
print(f"Total employees: {count}")
```


## Working with JSON Data


SQL Server has excellent JSON support:


python


```text
import pyodbc
import json


connection = pyodbc.connect(
'DRIVER={ODBC Driver 17 for SQL Server};'
'SERVER=localhost;'
'DATABASE=TestDB;'
'UID=sa;'
'PWD=YourPassword123'
)


cursor = connection.cursor()


# Create table with JSON column
cursor.execute("""
CREATE TABLE user_settings (
user_id INT PRIMARY KEY,
settings NVARCHAR(MAX)
)
""")


# Insert JSON data
settings = {
"theme": "dark",
"language": "en",
"notifications": True
}


cursor.execute(
"INSERT INTO user_settings (user_id, settings) VALUES (?, ?)",
1, json.dumps(settings)
)
connection.commit()


# Query and parse JSON
cursor.execute("SELECT settings FROM user_settings WHERE user_id = 1")
result = cursor.fetchone()[0]
user_settings = json.loads(result)


print(f"User theme: {user_settings['theme']}")


# Query JSON using SQL Server JSON functions
cursor.execute("""
SELECT JSON_VALUE(settings, '$.theme') as theme
FROM user_settings
WHERE user_id = 1
""")


theme = cursor.fetchone()[0]
print(f"Theme from JSON_VALUE: {theme}")


cursor.close()
connection.close()
```


## Connection Pooling


python


```text
import pyodbc


class ConnectionPool:
def __init__(self, connection_string, pool_size=5):
self.connection_string = connection_string
self.pool = []
self.pool_size = pool_size


for _ in range(pool_size):
self.pool.append(pyodbc.connect(connection_string))


def get_connection(self):
if self.pool:
return self.
```


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
