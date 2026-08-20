---
schema_version: "1.0.0"
document_id: "eccd27bb032638177ff85cedf85d06ef2153bfd46233106f497cd914480d068e"
company_key: "yc-oneschema"
company: "OneSchema"
source_id: "yc-oneschema-news-import-43da02420b1d"
canonical_url: "https://www.oneschema.co/blog/import-csv-mysql"
published_at: null
first_seen_at: "2026-07-22T07:23:01.413777+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:de49fdc875fbaf3837ce49ec4155d816820a157a91ff53fd3f31565f5ee09313"
---

# How to Import CSV into MySQL

MySQL is easily one of the most popular open-source relational database management systems around. It’s renowned for its reliability, performance, and ease of use.


This article explores five methods to import CSV files into MySQL, from simple one-time imports to more complex programmatic data ingestion.


‍


## **Method #1: LOAD DATA INFILE**


The LOAD DATA INFILE command is an efficient way to import a CSV file directly into a MySQL table. This command reads rows from a text file, then inserts them into the database table. It is particularly useful for large CSV files, as it is optimized for performance.


### **Use case**


Best for large-scale, server-side CSV imports requiring high performance.


### **Example usage:**


```text
sql
LOAD DATA INFILE '/path/to/your_file.csv'
INTO TABLE your_table
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;


```


Note: This requires the CSV file to be accessible on the server where MySQL is running.


‍


## **Method #2: phpMyAdmin**


phpMyAdmin, a widely-used web-based interface for MySQL, provides easy-to-use CSV import functionality. It allows users to import CSV files through a GUI, great for those not comfortable with command-line operations.


### **Use case**


Ideal for users who prefer a graphical interface and need to perform occasional, simple CSV imports.


### **Example usage:**


1. Log into phpMyAdmin and select your database.
2. Choose the target table and click on the "Import" tab.
3. Browse and select your CSV file.
4. Choose the appropriate options for format, character set, and other settings.
5. Click on "Go" to start the import.


‍


## **Method #3: MySQL Workbench**


MySQL Workbench is the official MySQL GUI tool that provides comprehensive tools for database management. Its import wizard allows you to import CSV files, providing options to configure the import process, such as field delimiters and encoding.


### **Use case**


Suitable for users who require a more advanced GUI with detailed configuration options.


### **Example usage**


1. Open MySQL Workbench and connect to your database.
2. Navigate to the "Server" menu and select "Data Import".
3. Choose "Import from Self-Contained File" and select your CSV file.
4. Select the target schema and table.
5. Configure the import settings and start the import.


‍


## **Method #4: Using a Programming Language (e.g., Python)**


For more flexibility and control, using a programming language like Python, along with a MySQL connector, can be an effective way to import CSV data. This method allows for preprocessing, error handling, and integration into larger applications or workflows.


### **Use case**


Ideal for custom imports requiring data manipulation or integration into automated systems.


### **Example usage**


1. First, install a MySQL connector for Python:


```text
bash
pip install mysql-connector-python


```


1. Then, use a script like this:


```text
python
import csv
import mysql.connector


# MySQL connection parameters
cnx = mysql.connector.connect(user='username', password='password', host='localhost', database='your_db')
cursor = cnx.cursor()


# CSV file path
csv_file_path = 'path_to_your_csv.csv'


with open(csv_file_path, mode='r') as csvfile:
reader = csv.reader(csvfile)
next(reader)  # Skip the header row
for row in reader:
cursor.execute("INSERT INTO your_table (column1, column2) VALUES (%s, %s)", row)


cnx.commit()
cursor.close()
cnx.close()


```


‍


{{blog-content-cta}}


‍


## **Method #5: Using Airbyte for Data Integration**


Airbyte, an open-source data integration platform, offers a robust and scalable solution for importing CSV files into MySQL. It stands out for its ability to handle both batch and real-time data synchronization, making it a versatile tool for various data integration needs.


### **Use Case**


Ideal for organizations seeking a reliable and scalable data integration platform. Airbyte is particularly useful for continuous, automated synchronization of CSV data into MySQL.


### **Example Usage**


1. Set Up Airbyte:some text


1. Install Airbyte on your local system or a cloud server.
2. Access the Airbyte dashboard through your web browser.


2. Configure the Source:some text


1. In the Airbyte dashboard, add a new source and select the 'File' option for CSV files.
2. Provide the necessary information to access your CSV files, such as file location and format.


3. Configure the Destination:some text


1. Add a new destination in Airbyte and choose MySQL.
2. Provide MySQL connection details, including host, port, username, password, and database name.


4. Set Up the Connection:some text


1. Create a connection between your CSV source and MySQL destination.
2. Configure the data replication frequency and any additional settings, like data normalization or transformation if needed.


5. Start the Data Sync:some text


1. Once the configuration is complete, start the data synchronization process.
2. Airbyte will automatically handle the CSV import into MySQL, respecting the scheduled frequency and any transformations specified.


‍


## **Conclusion**


From graphical user interfaces like phpMyAdmin and MySQL Workbench to command-line tools and custom scripts, the methods outlined above should cater to a wide range of technical proficiencies and requirements.


If you’re looking for a comprehensive CSV import solution, consider OneSchema. OneSchema provides a powerful CSV parsing and importing tool that seamlessly integrates with your front-end framework of choice.
