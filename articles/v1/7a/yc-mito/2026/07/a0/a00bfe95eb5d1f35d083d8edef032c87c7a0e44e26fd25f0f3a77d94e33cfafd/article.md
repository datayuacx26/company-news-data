---
schema_version: "1.0.0"
document_id: "a00bfe95eb5d1f35d083d8edef032c87c7a0e44e26fd25f0f3a77d94e33cfafd"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-connect-python-to-aws-dynamodb-complete-guide"
published_at: null
first_seen_at: "2026-07-24T04:41:15.374247+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:f77ad19fb7e9ddfc4ba542bb18fa970d8d23d638cfb79c18db2928be77a55afc"
---

# How to Connect Python to AWS DynamoDB: Complete Guide

[See more blogs](https://www.trymito.io/blog)


# How to Connect Python to AWS DynamoDB: Complete Guide


The Mito Team


10/6/2025


7 min read


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. When combined with Python, DynamoDB enables developers to build highly scalable, serverless applications with minimal operational overhead. This comprehensive guide covers everything you need to know about connecting Python to DynamoDB.


## Why Use DynamoDB with Python?


DynamoDB with Python offers powerful capabilities:


- Fully managed with zero server administration
- Consistent single-digit millisecond latency at any scale
- Built-in security, backup, and restore
- Automatic scaling based on workload
- Global tables for multi-region replication
- On-demand and provisioned capacity modes
- Perfect for serverless architectures
- Integrates seamlessly with AWS Lambda


## Prerequisites


Before connecting Python to DynamoDB, ensure you have:


- Python 3.6 or higher installed
- AWS account with DynamoDB access
- AWS CLI configured with credentials
- Basic understanding of NoSQL and key-value stores
- boto3 library (AWS SDK for Python)


## Installing boto3


Install the AWS SDK for Python:


bash


```text
pip install boto3
```


## Setting Up AWS Credentials


Configure AWS credentials using AWS CLI:


bash


```text
aws configure
```


Or set environment variables:


bash


```text
export AWS_ACCESS_KEY_ID='your_access_key'
export AWS_SECRET_ACCESS_KEY='your_secret_key'
export AWS_DEFAULT_REGION='us-east-1'
```


## Basic Connection to DynamoDB


Let's start with a simple connection:


python


```text
import boto3


# Create DynamoDB resource
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')


# Create DynamoDB client (lower-level API)
client = boto3.client('dynamodb', region_name='us-east-1')


print("Connected to DynamoDB successfully!")


# List all tables
tables = list(dynamodb.tables.all())
print(f"Existing tables: {[table.name for table in tables]}")
```


## Connection with Error Handling


python


```text
import boto3
from botocore.exceptions import ClientError, NoCredentialsError


def create_dynamodb_connection():
try:
dynamodb = boto3.resource(
'dynamodb',
region_name='us-east-1',
endpoint_url=None  # Use for local DynamoDB
)


# Test connection by listing tables
tables = list(dynamodb.tables.all())
print(f"Successfully connected to DynamoDB")
print(f"Available tables: {len(tables)}")


return dynamodb


except NoCredentialsError:
print("AWS credentials not found")
return None
except ClientError as e:
print(f"AWS error: {e}")
return None
except Exception as e:
print(f"Error: {e}")
return None


# Usage
dynamodb = create_dynamodb_connection()
```


## Creating a Table


python


```text
import boto3
from botocore.exceptions import ClientError


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')


# Create table
try:
table = dynamodb.create_table(
TableName='Employees',
KeySchema=[
{
'AttributeName': 'employee_id',
'KeyType': 'HASH'  # Partition key
},
{
'AttributeName': 'email',
'KeyType': 'RANGE'  # Sort key
}
],
AttributeDefinitions=[
{
'AttributeName': 'employee_id',
'AttributeType': 'S'  # String
},
{
'AttributeName': 'email',
'AttributeType': 'S'
}
],
BillingMode='PAY_PER_REQUEST'  # On-demand billing
)


# Wait for table to be created
table.wait_until_exists()


print(f"Table {table.table_name} created successfully!")
print(f"Table status: {table.table_status}")


except ClientError as e:
if e.response['Error']['Code'] == 'ResourceInUseException':
print("Table already exists")
else:
print(f"Error creating table: {e}")
```


## Creating Table with Provisioned Capacity


python


```text
import boto3


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')


table = dynamodb.create_table(
TableName='Products',
KeySchema=[
{'AttributeName': 'product_id', 'KeyType': 'HASH'}
],
AttributeDefinitions=[
{'AttributeName': 'product_id', 'AttributeType': 'S'},
{'AttributeName': 'category', 'AttributeType': 'S'}
],
GlobalSecondaryIndexes=[
{
'IndexName': 'CategoryIndex',
'KeySchema': [
{'AttributeName': 'category', 'KeyType': 'HASH'}
],
'Projection': {'ProjectionType': 'ALL'},
'ProvisionedThroughput': {
'ReadCapacityUnits': 5,
'WriteCapacityUnits': 5
}
}
],
ProvisionedThroughput={
'ReadCapacityUnits': 10,
'WriteCapacityUnits': 10
}
)


table.wait_until_exists()
print("Table with GSI created!")
```


## Inserting Items


### Insert Single Item


python


```text
import boto3
from datetime import datetime
from decimal import Decimal


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Insert single item
response = table.put_item(
Item={
'employee_id': 'EMP001',
'email': 'john.doe@company.com',
'first_name': 'John',
'last_name': 'Doe',
'department': 'Engineering',
'salary': Decimal('75000.00'),  # Use Decimal for numbers
'hire_date': str(datetime.now().date()),
'skills': ['Python', 'AWS', 'DynamoDB']
}
)


print(f"Item inserted successfully!")
print(f"Response: {response['ResponseMetadata']['HTTPStatusCode']}")
```


Note: DynamoDB requires Decimal type for numbers, not float.


### Batch Write Items


python


```text
import boto3
from decimal import Decimal


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Batch write items
with table.batch_writer() as batch:
batch.put_item(
Item={
'employee_id': 'EMP002',
'email': 'jane@company.com',
'first_name': 'Jane',
'last_name': 'Smith',
'department': 'Marketing',
'salary': Decimal('68000')
}
)
batch.put_item(
Item={
'employee_id': 'EMP003',
'email': 'bob@company.com',
'first_name': 'Bob',
'last_name': 'Johnson',
'department': 'Sales',
'salary': Decimal('72000')
}
)
batch.put_item(
Item={
'employee_id': 'EMP004',
'email': 'alice@company.com',
'first_name': 'Alice',
'last_name': 'Williams',
'department': 'Engineering',
'salary': Decimal('80000')
}
)


print("Batch write completed!")
```


## Retrieving Items


### Get Single Item


python


```text
import boto3


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Get item by key
response = table.get_item(
Key={
'employee_id': 'EMP001',
'email': 'john.doe@company.com'
}
)


if 'Item' in response:
item = response['Item']
print(f"Employee: {item['first_name']} {item['last_name']}")
print(f"Department: {item['department']}")
print(f"Salary: ${item['salary']}")
else:
print("Item not found")
```


### Batch Get Items


python


```text
import boto3


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')


# Batch get items
response = dynamodb.batch_get_item(
RequestItems={
'Employees': {
'Keys': [
{'employee_id': 'EMP001', 'email': 'john.doe@company.com'},
{'employee_id': 'EMP002', 'email': 'jane@company.com'},
{'employee_id': 'EMP003', 'email': 'bob@company.com'}
]
}
}
)


items = response['Responses']['Employees']
print(f"Retrieved {len(items)} items")


for item in items:
print(f"{item['first_name']} {item['last_name']} - {item['department']}")
```


## Query Operation


Query is efficient for retrieving items with same partition key:


python


```text
import boto3
from boto3.dynamodb.conditions import Key


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Query by partition key
response = table.query(
KeyConditionExpression=Key('employee_id').eq('EMP001')
)


items = response['Items']
print(f"Found {len(items)} items")


for item in items:
print(f"{item['first_name']} {item['last_name']}")


# Query with sort key condition
response = table.query(
KeyConditionExpression=Key('employee_id').eq('EMP001') &
Key('email').begins_with('john')
)
```


## Scan Operation


Scan reads all items in table (expensive operation):


python


```text
import boto3
from boto3.dynamodb.conditions import Attr


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Scan all items
response = table.scan()


items = response['Items']
print(f"Total items: {len(items)}")


for item in items:
print(f"{item['first_name']} {item['last_name']}")


# Scan with filter
response = table.scan(
FilterExpression=Attr('department').eq('Engineering')
)


engineers = response['Items']
print(f"\nEngineers: {len(engineers)}")
```


## Updating Items


python


```text
import boto3
from decimal import Decimal


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Update item
response = table.update_item(
Key={
'employee_id': 'EMP001',
'email': 'john.doe@company.com'
},
UpdateExpression='SET salary = :new_salary, department = :new_dept',
ExpressionAttributeValues={
':new_salary': Decimal('85000'),
':new_dept': 'Senior Engineering'
},
ReturnValues='UPDATED_NEW'
)


print(f"Updated attributes: {response['Attributes']}")


# Atomic counter increment
response = table.update_item(
Key={
'employee_id': 'EMP001',
'email': 'john.doe@company.com'
},
UpdateExpression='SET salary = salary + :increment',
ExpressionAttributeValues={
':increment': Decimal('5000')
},
ReturnValues='UPDATED_NEW'
)


print(f"New salary: {response['Attributes']['salary']}")


# Add to list
response = table.update_item(
Key={
'employee_id': 'EMP001',
'email': 'john.doe@company.com'
},
UpdateExpression='SET skills = list_append(skills, :new_skill)',
ExpressionAttributeValues={
':new_skill': ['Docker']
},
ReturnValues='UPDATED_NEW'
)
```


## Conditional Updates


python


```text
import boto3
from boto3.dynamodb.conditions import Attr
from botocore.exceptions import ClientError
from decimal import Decimal


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Update only if condition is met
try:
response = table.update_item(
Key={
'employee_id': 'EMP001',
'email': 'john.doe@company.com'
},
UpdateExpression='SET salary = :new_salary',
ConditionExpression=Attr('salary').lt(Decimal('80000')),
ExpressionAttributeValues={
':new_salary': Decimal('85000')
},
ReturnValues='UPDATED_NEW'
)
print("Update successful!")
except ClientError as e:
if e.response['Error']['Code'] == 'ConditionalCheckFailedException':
print("Condition not met - update skipped")
else:
print(f"Error: {e}")
```


## Deleting Items


python


```text
import boto3


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Delete single item
response = table.delete_item(
Key={
'employee_id': 'EMP005',
'email': 'old@company.com'
}
)


print(f"Item deleted: {response['ResponseMetadata']['HTTPStatusCode']}")


# Conditional delete
from boto3.dynamodb.conditions import Attr


response = table.delete_item(
Key={
'employee_id': 'EMP006',
'email': 'temp@company.com'
},
ConditionExpression=Attr('department').eq('Temporary')
)
```


## Working with Global Secondary Indexes


python


```text
import boto3
from boto3.dynamodb.conditions import Key


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Products')


# Query using GSI
response = table.query(
IndexName='CategoryIndex',
KeyConditionExpression=Key('category').eq('Electronics')
)


items = response['Items']
print(f"Electronics products: {len(items)}")


for item in items:
print(f"{item['product_id']}: {item.get('name', 'N/A')}")
```


## Pagination


python


```text
import boto3


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Employees')


# Paginate through scan results
response = table.scan(Limit=10)


items = response['Items']
print(f"Page 1: {len(items)} items")


# Continue pagination
while 'LastEvaluatedKey' in response:
response = table.scan(
Limit=10,
ExclusiveStartKey=response['LastEvaluatedKey']
)


items.extend(response['Items'])
print(f"Retrieved {len(response['Items'])} more items")


print(f"Total items retrieved: {len(items)}")
```


## Transactions


DynamoDB supports ACID transactions:


python


```text
import boto3
from decimal import Decimal


dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
client = boto3.client('dynamodb', region_name='us-east-1')


# Transactional write
try:
response = client.transact_write_items(
TransactItems=[
{
'Update': {
'TableName': 'Employees',
'Key': {
'employee_id': {'S': 'EMP001'},
'email': {'S': 'john@company.com'}
},
'UpdateExpression': 'SET salary = salary - :amount',
'ExpressionAttributeValues': {
':amount': {'N': '5000'}
}
}
},
{
'Update': {
'TableName': 'Employees',
'Key': {
'employee_id': {'S': 'EMP002'},
'email': {'S': 'jane@company.com'}
},
'UpdateExpression': 'SET salary = salary + :amount',
'ExpressionAttributeValues': {
':amount': {'N': '5000'}
}
}
}
]
)
print("Transaction successful!")
except Exception as e:
print(f"Transaction failed: {e}")
```


## Complete Application Example


python


```text
import boto3
from boto3.dynamodb.conditions import Key, Attr
from botocore.exceptions import ClientError
from decimal import Decimal
from datetime import datetime


class EmployeeDatabase:
def __init__(self, region='us-east-1'):
self.dynamodb = boto3.resource('dynamodb', region_name=region)
self.table_name = 'Employees'
self.table = self.dynamodb.Table(self.table_name)


def create_table(self):
try:
table = self.dynamodb.create_table(
TableName=self.table_name,
KeySchema=[
{'AttributeName': 'employee_id', 'KeyType': 'HASH'},
{'AttributeName': 'email', 'KeyType': 'RANGE'}
],
AttributeDefinitions=[
{'AttributeName': 'employee_id', 'AttributeType': 'S'},
{'AttributeName': 'email', 'AttributeType': 'S'},
{'AttributeName': 'department', 'AttributeType': 'S'}
],
GlobalSecondaryIndexes=[
{
'IndexName': 'DepartmentIndex',
'KeySchema': [
{'AttributeName': 'department', 'KeyType': 'HASH'}
],
'Projection': {'ProjectionType': 'ALL'},
'ProvisionedThroughput': {
'ReadCapacityUnits': 5,
'WriteCapacityUnits': 5
}
}
],
BillingMode='PAY_PER_REQUEST'
)
table.wait_until_exists()
print(f"Table {self.table_name} created")
except ClientError as e:
if e.response['Error']['Code'] == 'ResourceInUseException':
print("Table already exists")


def add_employee(self, employee_id, email, first_name, last_name,
department, salary):
try:
self.table.put_item(
Item={
'employee_id': employee_id,
'email': email,
'first_name': first_name,
'last_name': last_name,
'department': department,
'salary': Decimal(str(salary)),
'created_at': str(datetime.now())
}
)
return True
except ClientError as e:
print(f"Error adding employee: {e}")
return False


def get_employee(self, employee_id, email):
try:
response = self.table.get_item(
Key={'employee_id': employee_id, 'email': email}
)
return response.get('Item')
except ClientError as e:
print(f"Error getting employee: {e}")
return None


def get_employees_by_department(self, department):
try:
response = self.table.query(
IndexName='DepartmentIndex',
KeyConditionExpression=Key('department').eq(department)
)
return response['Items']
except ClientError as e:
print(f"Error querying department: {e}")
return []


def update_salary(self, employee_id, email, new_salary):
try:
response = self.table.update_item(
Key={'employee_id': employee_id, 'email': email},
UpdateExpression='SET salary = :salary',
ExpressionAttributeValues={':salary': Decimal(str(new_salary))},
ReturnValues='UPDATED_NEW'
)
return response['Attributes']
except ClientError as e:
print(f"Error updating salary: {e}")
return None


def delete_employee(self, employee_id, email):
try:
self.table.delete_item(
Key={'employee_id': employee_id, 'email': email}
)
return True
except ClientError as e:
print(f"Error deleting employee: {e}")
return False


# Usage
db = EmployeeDatabase()


# Add employee
db.add_employee('EMP001', 'sarah@company.com', 'Sarah', 'Connor',
'Security', 78000)


# Get employee
employee = db.get_employee('EMP001', 'sarah@company.com')
print(f"Employee: {employee['first_name']} {employee['last_name']}")


# Get by department
engineers = db.get_employees_by_department('Engineering')
print(f"Engineers: {len(engineers)}")


# Update salary
db.update_salary('EMP001', 'sarah@company.com', 82000)
```


## Best Practices


Follow these best practices when using DynamoDB with Python:


1. **Design tables** based on access patterns
2. **Use Decimal** type for numeric values
3. **Implement exponential backoff** for retries
4. **Use batch operations** for multiple items
5. **Choose appropriate partition keys** to avoid hot partitions
6. **Use GSIs** for alternative query patterns
7. **Enable point-in-time recovery** for critical tables
8. **Monitor capacity metrics** in CloudWatch
9. **Use on-demand billing** for unpredictable workloads
10. **Implement proper error handling** for all operations
11. **Use conditional writes** to prevent race conditions
12. **Avoid scans** on large tables - use Query instead
13. **Use projection expressions** to fetch only needed attributes
14. **Enable encryption at rest** for sensitive data
15. **Cache frequently accessed data** using DAX


## Conclusion


Connecting Python to AWS DynamoDB provides a powerful foundation for building serverless, highly scalable applications. The boto3 library offers comprehensive features and seamless integration with DynamoDB's capabilities. By following the examples and best practices in this guide, you'll be able to build robust applications that leverage DynamoDB's speed, scalability, and fully managed nature. Whether you're building web applications, mobile backends, or IoT platforms, mastering Python-DynamoDB connectivity is essential for modern cloud-native development on AWS.


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
