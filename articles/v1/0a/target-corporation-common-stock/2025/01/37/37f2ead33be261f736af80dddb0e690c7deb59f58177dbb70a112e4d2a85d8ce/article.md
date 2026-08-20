---
schema_version: "1.0.0"
document_id: "37f2ead33be261f736af80dddb0e690c7deb59f58177dbb70a112e4d2a85d8ce"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/Simplify-Your-Cassandra-Database-with-UDTs"
published_at: "2025-01-31T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:99c05cb338fdf38b4ade7f5c3d1ef54ce257783ad70e128ffc6cc76096e3b18b"
---

# Simplify Your Cassandra Database with User Defined Types (UDTs)

Have you ever felt overwhelmed by the sheer number of columns in your Cassandra tables? While the number of columns can vary based on service requirements and performance testing, it's important to design your table schema with a focus on latency, readability, and a high-level view for developers. Grouping related columns into a single structure is a best practice, as having too many columns can create significant issues with read and write operations.


One solution to this problem is to divide the table into multiple parts and read from each table. If splitting the table isn't an option, you can leverage User Defined Types (UDTs) and Collections in Cassandra. UDTs provide a powerful way to organize and structure your data more meaningfully.


UDTs are a powerful feature for efficiently handling complex data structures with better readability and maintainability. Target manages millions of complex, nested data related to orders, items, products, and more, and many teams rely on Cassandra. UDTs ensure efficient performance without compromising on speed, providing a unified way to handle complex data and ease schema design.


Let's explore what a UDT is, along with its advantages and disadvantages, to help you simplify your Cassandra database.


What is a UDT?


UDTs in Cassandra allow you to group primitive data types into a single column. A UDT can also contain other UDTs or collections, enabling you to group attributes from the same object and store them in a single column. Collections can also be stored as columns, supporting primitive data types like INT, TEXT, and BOOLEAN. Unlike many relational databases, Cassandra can store collections of objects as a single column attribute, simplifying query operations when dealing with large sets of columns.


Understanding frozen UDTs


UDTs in Cassandra are often defined as frozen, meaning the UDT value is treated as a single blob. Once created, you cannot update parts


o


f a frozen UDT—you must update the entire UDT.


Both collections and UDTs can be defined as frozen or non-frozen. Frozen UDT data is treated as a whole blob and stored as a single cell in the storage location. This means you cannot perform add, update, or delete operations on specific UDT attributes. Instead, you must reinsert the entire UDT into the database.


In most cases, UDTs are frozen. The frozen keyword can be used with UDTs or collections. Non-frozen UDT attributes are stored in separate cells internally, similar to storing them in distinct columns. Frozen UDTs share common metadata such as TTL and keyspace_name. It is generally advised to define UDTs as frozen, as there is no significant benefit to using non-frozen UDTs.


By understanding and utilizing UDTs and Collections in Cassandra, you can design more efficient and manageable table schemas, simplifying your database management.


How to Create UDTs in Cassandra


Creating UDTs in Cassandra is straightforward. Here's how you can do it:


Step 1: Define the UDT


Use the following CQL query to create a UDT called


address


:


CREATE TYPE address (


apartment text,


flat_number text,


landmark text,


city text,


state text,


pincode text


);


Step 2: Create a Table with UDT


Next, create a table called


guest


that includes the


address


UDT as a column. This column can store a list of addresses for any guest. You can use collections like LIST, SET, or MAP to store the UDTs.


Using LIST:


A LIST contains an ordered collection of values, ensuring elements are added in a specific order (ascending/descending). LIST UDTs can store attributes like dates, numbers, or names in order.


CREATE TABLE guest(


uuid text,


first_name text,


last_name text,


email_id text,


address list<frozen<address>>,


PRIMARY KEY (guest_id)


);


Using SET:


A SET contains elements without duplicates, which helps avoid redundancy.


CREATE TABLE guest(


uuid text,


first_name text,


last_name text,


email_id text,


address set<frozen<address>>,


PRIMARY KEY (guest_id)


);


Using MAP:


A MAP requires a key-value pair. Here,


guest_id


is the key and


address


is the value.


CREATE TABLE guest(


uuid text,


first_name text,


last_name text,


email_id text,


address map<frozen<text, address>>,


PRIMARY KEY (guest_id)


);


Inserting Data with UDT


To insert data into the


guest


table with a list of addresses, use the following query:


INSERT INTO guest (uuid, first_name, last_name, email_id, address) VALUES (


'guest_test',


'JOHN',


'MAT',


'abc@example.com',


\[{ apartment: 'Brigade One', flat_number: '100', landmark: 'near CCD', city: 'Bangalore', state: 'Karnataka', pincode: 560078 },


{ apartment: 'Brigade Two', flat_number: '501', landmark: 'near Pantaloon', city: 'Bangalore', state: 'Karnataka', pincode: 560045 }\]


);


Altering UDTs


You can alter UDTs to add, rename, or drop fields. Here are some examples:


Add a Field:


ALTER TYPE guest.address ADD street text;


Rename a Field:


ALTER TYPE guest.address RENAME pincode TO pin;


Drop a Field:


ALTER TYPE guest.address DROP landmark;


Dropping UDTs:


To drop a UDT, first delete its reference from the table, then drop the UDT:


DROP TYPE IF EXISTS address;


Renaming UDTs:


To rename a UDT, use the following query:


RENAME TYPE guest.address TO billing_address;


Updating UDTs:


Updating a UDT requires updating the entire UDT value. For example, if you need to correct the


pincode


:


UPDATE guest SET address\[0\] = {pincode: 560080} WHERE uuid = 'guest_test';


This will result in an address with only the


pincode


updated and other fields as NULL. To avoid this, ensure all fields are included in the update.


The below figure depicts how this table and UDTs are stored internally with List or Set.


Indexing UDTs


User Defined Types (UDTs) can be indexed using Storage Attached Indexing (SAI) in Cassandra. SAI indexing can be applied to a single UDT or a list of UDTs. This feature provides benefits such as better write times and reduced disk size compared to other indexing mechanisms.


Indexing a List of UDTs


Using the guest and address table example, you can create an index to perform CONTAINS operations in search queries.


Creating the Index:


CREATE INDEX address_idx


ON guest (address)


USING 'sai';


This index allows you to search for guests with a specific pincode and apartment name using the CONTAINS keyword.


Search Query Example:


SELECT * FROM guest


WHERE address CONTAINS {


apartment: 'Brigade One',


flat_number: '100',


landmark: 'near CCD',


city: 'Bangalore',


state: 'Karnataka',


pincode: 560078


};


Indexing Collections (List/SET)


Indexes can also be created on collections like LIST, SET, or MAP. Here’s an example with a table of students and their subjects.


Creating the Table and Index:


CREATE TABLE IF NOT EXISTS Students (


student_id UUID PRIMARY KEY,


name text,


subjects set<text>


);


CREATE INDEX student_idx


ON Students (subjects)


USING 'sai';


Search Query Example:


SELECT * FROM Students WHERE subjects CONTAINS 'English';


Indexing with MAP


Indexing in MAP allows you to create an index on both the key and value. Let's create a table for a school with student_id and student_name as the key-value pair for the MAP.


Creating the Table and Indexes:


CREATE TABLE IF NOT EXISTS School (


school_id UUID PRIMARY KEY,


school_name text,


student_data map<int, text>


);


CREATE INDEX IF NOT EXISTS student_key_idx ON School (KEYS(student_data));


CREATE INDEX IF NOT EXISTS student_values_idx ON School (VALUES(student_data));


Search Queries


:


Query with KEYS:


SELECT * FROM School WHERE student_data CONTAINS KEY 12345;


Query with VALUES:


SELECT * FROM School WHERE student_data CONTAINS 'JOHN_WILLINGTON';


Searchability with UDTs


Direct select operations on a UDT column are not possible. To use filters in UDTs, you need to create indexing on UDTs, as explained above. You can use the CONTAINS keyword to perform a search by listing all the components. Querying the address UDT will require all the components (apartment, flat_number, landmark, city, state, pincode) of this UDT. With collection UDTs, you can perform the CONTAINS operation to search for any value. In some cases, DataStax Enterprise (DSE) Engines can be used to search on UDTs.


When to Use UDTs


Consider the following when designing schemas with UDTs or collection types in Cassandra:


- Avoid Frequent Updates:


UDTs should not be used if fields need to be updated frequently and individually.


- High Read/Write Time:


If read/write time is high due to a large number of cells, try using UDTs or collections to group relevant values.


- Use Cases:


UDTs are beneficial for storing attributes like addresses, multiple contact numbers, and multiple payment details that do not change frequently.


Pros and Cons of UDTs and Frozen UDTs


Pros:


- Improved Read Performance:


Multiple fields can be grouped under the same UDT object.


- Structured Data:


UDTs can store complex and nested data in a more structured manner.


- Schema Organization:


UDTs help in maintaining a well-structured schema, improving high-level data insight.


Cons:


- Immutability:


UDTs are suitable for storing immutable objects and are not advised for frequent updates.


- Not Ideal for MongoDB:


MongoDB supports document-oriented DB design, which can store complex JSON-like structures and collections more effectively.


- Irreversible DROP Command:


The DROP command for UDTs cannot be reversed. If UDTs are referenced in another UDT or table, the DROP operation is not possible.


- Primary Key Limitation:


UDTs cannot be used as a primary key column for any table.


Conclusion


UDTs should be used based on application requirements. Consider all aspects of the data with thorough analysis. While UDTs should not be used everywhere, they can be beneficial in specific use cases like those explored above.


## RELATED POSTS


### Running Cassandra in Kubernetes Across 1,800 Stores


By Daniel Parker and Mike Showalter, August 8, 2018


Stateful Apps in a Stateless World
