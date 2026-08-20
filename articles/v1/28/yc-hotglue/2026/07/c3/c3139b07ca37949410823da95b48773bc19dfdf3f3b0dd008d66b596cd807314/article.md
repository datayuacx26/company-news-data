---
schema_version: "1.0.0"
document_id: "c3139b07ca37949410823da95b48773bc19dfdf3f3b0dd008d66b596cd807314"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/optimizing-microsoft-sql-insert-speed-in-python"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:69ac47877fb8fd30f9446a4a95de66325c71adf47a166f664faf2b79342980d5"
---

# Optimizing Microsoft SQL insert speed in Python: pymssql vs. pyodbc vs. bcp

At hotglue, we power thousands of data syncs every day — improving the performance of these syncs translates into huge cost savings and better experiences for our users. Our customers count on us to deliver connectors that are reliable, fast, and ensure high data quality.


In this post, we’ll walk through how our team optimized our[Microsoft SQL target](https://github.com/hotgluexyz/target-mssql) and significantly improved throughput. We’ll compare three approaches —` pymssql` ,` pyodbc` , and` bcp` — and share the results of our testing.


Let’s dive in!


# Connectivity issues with pymssql


Our original Microsoft SQL Server target was built using SQLAlchemy with[pymssql](https://github.com/pymssql/pymssql) .


An AI startup was trialing our product, and their use case centered around reading large amounts of data from CRMs like HubSpot and Salesforce and writing it to their Azure SQL server.


The first issue we encountered was with connecting to Azure SQL Server. It turns out this was a common issue with` pymssql` , as Azure SQL Server requires encryption and the` pymssql` wheel does not have SSL bindings by default (there’s a nice[StackOverflow thread about this](https://stackoverflow.com/questions/39129928/why-can-i-connect-to-azure-ms-sql-with-tsql-but-not-pymssql) ).


# Switching to pyodbc


Rather than fiddle with installing` pymssql` from source, we switched to using[pyodbc](https://github.com/mkleehammer/pyodbc) with the official Microsoft SQL Server driver. This was easy enough as we were already using SQLAlchemy – just changing our connection URL and installing the ODBC driver was enough:


` connection_url = sqlalchemy.engine.url.URL.create(


drivername="mssql+pyodbc", username=config\['user'\], password=urllib.parse.quote_plus(config\["password"\]), host=config\["host"\], port=config\["port"\], database=config\["database"\], query={ "driver": "ODBC Driver 17 for SQL Server", # Use Microsoft's ODBC driver "Encrypt": "yes", # Ensures SSL encryption for Azure SQL "TrustServerCertificate": "yes", # Prevents bypassing certificate validation "MARS_Connection": "Yes", "ConnectRetryCount": "3", "ConnectRetryInterval": "15" } )`


This solved the initial issue and we were able to connect to Azure SQL Server! However, we immediately ran into performance issues – writing even relatively small amounts of records (we tested with batches of 10,000) was slow. Worse, we seemed to have intermittent timeout issues while trying to write data, with error messages like:


` \[08S01\] \[Microsoft\]\[ODBC Driver 17 for SQL Server\]TCP Provider: Error code 0x68 (104) (SQLExecDirectW)


`


# Trying to boost pyodbc performance


Doing some research we found that other folks had hit similar performance constraints with` pyodbc` – the recommendation was to try using the` fast_executemany=True` flag. Again, this was straightforward thanks to SQLAlchemy:


` engine


=


sqlalchemy


.


create_engine


(


self


.


sqlalchemy_url


,


fast_executemany


=


True


,


echo


=


False


,


isolation_level


=


None


)


`


However, this broke our core logic as our target initially creates local temporary tables (denoted in MS SQL using a` #` ). The problem is that` fast_executemany` breaks with local temp tables – that rabbit hole took us to a[GitHub issue thread on the pyodbc repo discussing possible solutions](https://github.com/mkleehammer/pyodbc/issues/785) .


According to that thread, we could have avoided this error by adding the flag` UseFMTONLY` or switching to global temp tables. However, we were still not convinced this` fast_executemany` flag would solve our core performance issue, so we wrote a simple workaround to create “fake” temp tables prefixed with` TEMP_`


So we dropped the original temp table creation logic:


` DROP


TABLE


IF


EXISTS


#{from_table_name};


SELECT


TOP


0


*


into


#{from_table_name}


FROM


{from_table_name}


;


`


and replaced it with creating this “fake” temp table.


` DROP


TABLE


IF


EXISTS


TMP_{from_table_name


.


split


(


'.'


)


\[


-


1


\]


}


;


CREATE


TABLE


TMP_{from_table_name


.


split


(


"."


)


\[


-


1


\]


}


(


{


", "


.


join


(


column_definitions


)


}


)


;


`


As you can see, in doing this we switched the approach for creating the table and manually built the` column_definitions` . We did this by querying the` sys.columns` and` sys.types` tables in MS SQL:


` # Query to get column definitions, including identity property


get_columns_query


=


f


"""


SELECT


c.name AS COLUMN_NAME,


t.name AS DATA_TYPE,


c.max_length AS COLUMN_LENGTH,


c.precision AS PRECISION_VALUE,


c.scale AS SCALE_VALUE,


d.definition AS COLUMN_DEFAULT,


COLUMNPROPERTY(c.object_id, c.name, 'IsIdentity') AS IS_IDENTITY


FROM sys.columns c


JOIN sys.types t ON c.user_type_id = t.user_type_id


LEFT JOIN sys.default_constraints d ON c.default_object_id = d.object_id


WHERE c.object_id = OBJECT_ID('{from_table_name}')


"""


columns


=


self


.


connection


.


execute


(


get_columns_query


)


.


fetchall


(


)


self


.


logger


.


debug


(


f


"Fetched columns: {columns}"


)


# Construct the CREATE TABLE statement


column_definitions


=


\[


\]


for


col


in


columns


:


col_name


=


col


\[


0


\]


col_type


=


col


\[


1


\]


col_length


=


col


\[


2


\]


precision_value


=


col


\[


3


\]


scale_value


=


col


\[


4


\]


col_default


=


f


"DEFAULT {col\[5\]}"


if


col


\[


5


\]


else


""


is_identity


=


col


\[


6


\]


identity_str


=


"IDENTITY(1,1)"


if


is_identity


else


""


# Apply length only if it's a varchar/nvarchar type


if


col_type


.


lower


(


)


in


\[


"varchar"


,


"nvarchar"


\]


:


col_length_str


=


"(MAX)"


if


col_length


=


=


-


1


else


f


"({col_length})"


else


:


col_length_str


=


""


column_definitions


.


append


(


f


"\[{col_name}\] {col_type}{col_length_str} {identity_str} {col_default}"


)


`


Finally, we were able to test our change with a batch of 10,000 records:


` 2025-01-15 19:01:55,224 Starting 'Load records in database!! Table TMP_contacts; Records count 10000'...


2025-01-15 19:06:35,295 'Load records in database!! Table TMP_contacts; Records count 10000' took 4:40 (mm:ss) to run.`


Yikes… 4 minutes 40 sec just to insert 10,000 records was not even close to enough of a performance gain. Worse still, the timeout issues persisted.


# Using bcp for the inserts


After that realization, we went back to the drawing board and did some research on what the most performant ways to write to Microsoft SQL Server are. This is when we found Microsoft’s[bulk copy program utility](https://learn.microsoft.com/en-us/sql/tools/bcp-utility?view=sql-server-ver17&tabs=windows) (aka` bcp` ) – if you’re like me, that name made you chuckle :)


The concept of` bcp` is pretty simple, it’s a CLI that enables you to insert CSV files into Microsoft SQL Server super quickly. Plus, it’s designed specifically for loading large amounts of data.


We were a little dubious that` bcp` could be that much faster, so we did a local test. It did not disappoint – we were able to write 10k rows in just 8 seconds. We had found our solution!


` bcp {db} in test-data.csv -S "host" -U "user" -P "pass" -c -t"\\\\t" -e "error_log.txt"


`


Updating the target was a little trickier.


We decided to continue using` pyodbc` for everything except the inserts. We would use it to create the tables, adding missing columns, ensure schemas, and create the “fake” temp tables. Note that we kept our “fake” temp table solution intact because` bcp` (similar to` fast_executemany` ) behaved strangely with local temporary tables.


To generate the CSV files for` bcp` to insert we decided to use` pandas` – the code was pretty straightforward. We converted our array of` insert_records` (a list of dictionaries) into a dataframe. Notice that we had to remove new lines and tabs from the input data to avoid parsing errors with` bcp` . Finally, we serialize the dataframe to a tab delimited CSV with no header.


` # Convert the array of records to a dataframe


df


=


pd


.


DataFrame


(


insert_records


)


# Remove new lines and tabs to avoid parsing errors with bcp


df


=


df


.


replace


(


r"\[\\\\n\\\\r\\\\t\]"


,


" "


,


regex


=


True


)


# Serialize the chunk to CSV with no headers and using tab separators


df


.


to_csv


(


f"


{


table_name


}


.csv"


,


index


=


False


,


header


=


False


,


sep


=


"\\\\t"


)


`


Now, for the fun part! We dropped the old` INSERT` code and replaced it with a call to` bcp` , passing the fully qualified table name, database credentials, and the path to the CSV file.


` #


The


JOB_ROOT


env


var


here indicates


if


we're running


in


hotglue or just locally


bcp


=


"/opt/mssql-tools/bin/bcp"


if


os


.


environ


.


get


(


"JOB_ROOT"


)


else


"bcp"


#


Fully


qualified table name to insert to


db


=


f


'"\[{database}\].\[{db_schema}\].\[{table_name}\]"'


#


Build


the bcp command to insert the data


bcp_cmd


=


f


'{bcp} {db} in {table_name}.csv -S "{host}" -U "{user}" -P "{password}" -c -t"\\\\t" -e "error_log.txt"'


#


Run


the command using subprocess


result


=


subprocess


.


run


(


bcp_cmd


,


shell


=


True


,


capture_output


=


True


,


text


=


True


)


#


Log


the output


self


.


logger


.


info


(


result


.


stdout


)


if


result


.


stderr


:


#


If


there are parsing on insert errors we will capture them here


self


.


logger


.


error


(


result


.


stderr


)


`


The rest of the code stayed the same (and used` pyodbc` ) – once the insert completes, we simply merge this temp table into the main table + drop the temp table.


With all our changes done, we ran a test in production and saw a huge impact: runtime was down to 1.6 sec to upload the data to temp tables! Our logs below:


` 2025-01-15 21:04:05,473 /opt/mssql-tools/bin/bcp "\[db\].\[salesforce\].\[temp_contact\]" in temp_contact.csv -S sample.database.windows.net -U admin -c -t"\\\\t" -e "error_log.txt"


Starting copy... 1000 rows sent to SQL Server. Total sent: 1000 1000 rows sent to SQL Server. Total sent: 2000 1000 rows sent to SQL Server. Total sent: 3000 1000 rows sent to SQL Server. Total sent: 4000 1000 rows sent to SQL Server. Total sent: 5000 1000 rows sent to SQL Server. Total sent: 6000 1000 rows sent to SQL Server. Total sent: 7000 1000 rows sent to SQL Server. Total sent: 8000 1000 rows sent to SQL Server. Total sent: 9000 1000 rows sent to SQL Server. Total sent: 10000 10000 rows copied. Network packet size (bytes): 4096 Clock Time (ms.) Total : 1644 Average : (6082.7 rows per sec.) 2025-01-15 21:04:07,502 Merging data from temp table to salesforce.contact 2025-01-15 21:04:08,084 Dropping temp table as batch is done salesforce.contact`


# Conclusion


There were a few small clean up things to do (e.g. adding support for databases on custom ports), but these changes transformed our Microsoft SQL target to a far more performant and reliable connector. Our customer was able to sync millions of CRM records into their Azure SQL Server instance with ease.


If you’re interested in checking out the code in more detail, check out the GitHub repo:[https://github.com/hotgluexyz/target-mssql/](https://github.com/hotgluexyz/target-mssql/)


Thanks for reading, and stay tuned for more deep dives into how we are building high-performance connectors!


TABLE OF CONTENTS


- Connectivity issues with pymssql
- Switching to pyodbc
- Trying to boost pyodbc performance
- Using bcp for the inserts
- Conclusion


RECOMMENDED BLOGS


[How to build a public Shopify app Learn how to build a public Shopify app. This guide walks through authentication, app setup, and how to use hotglue to simplify the entire process.](https://hotglue.com/blog/how-to-build-a-public-shopify-app)


[QuickBooks New API Fees Explained + How to Monitor & Minimize Usage Starting July 28, 2025, QuickBooks is launching its new QuickBooks App Partner Program, which introduces paid tiers and API fees. Without careful monitoring, this new pricing model can get expensive fast.](https://hotglue.com/blog/how-to-lower-your-coreplus-quickbooks-api-usage)


[hotglue melt: May 2025 Universal external id support, connector builder updates, subtenant symlinks, and more!](https://hotglue.com/blog/hotglue-melt-may-2025)
