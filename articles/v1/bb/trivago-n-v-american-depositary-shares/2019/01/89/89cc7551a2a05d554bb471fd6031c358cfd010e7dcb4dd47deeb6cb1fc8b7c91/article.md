---
schema_version: "1.0.0"
document_id: "89cc7551a2a05d554bb471fd6031c358cfd010e7dcb4dd47deeb6cb1fc8b7c91"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2019-01-30-functional-approach-complex-types-apache-hive/"
published_at: "2019-01-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:bf7c0d61004b9c59ec6d15d5cbc88da71e238af8ea7aa8205ebc77598f491282"
---

# A New Functional Approach to Complex Types in Apache Hive

When faced with the challenge to store, retrieve and process small or large amounts of data, structured query languages are typically not far away. These languages serve as a nice abstraction between the goal that is to be achieved and how it is actually done. The list of successful applications of this extra layer is long. MySQL users could switch from[MyISAM](https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html) to[InnoDB](https://dev.mysql.com/doc/refman/8.0/en/innodb-introduction.html) or use new algorithms like[Multi-Range-Read](https://dev.mysql.com/doc/refman/5.6/en/mrr-optimization.html) without a change to their application. We, as Hive users, can effortlessly switch our complete processing from MapReduce to, say, Tez or Spark. All this is possible because of SQL serving as an abstraction layer in between. However, in this article, I will outline the effects when SQL - specifically hiveQL - misbehaves and which steps we are taking to recover.


## Complex Data Types in Hive


Despite Hive being a very relationally organized system, it has thorough support for complex data types such as Maps, Arrays, and Structs. A typical example table definition could look like this:


```text
CREATE   TABLE   example  .complex_types (
simple_string string,
simple_int   int  ,
deep_complex   array<  struct  <  deeper:  <  map  <int  ,  <array<  string  >>>>>
)
```


This offers a nice way to keep data organized in many situations. You can put all the data that belongs together into a bigger nested structure. Often you can hear those nested types being referred to as “NoSQL”, directly rejecting a successful coexistence of SQL and complex data types. Here we are, using SQL, processing NoSQL data types.


## Hive approaches to Complex Types


Hive offers great support in just leaving the query language behind and using other languages to crack the nested datatypes. With the[transform](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Transform) keyword, all the data will be sent to a custom program. This program can subsequently read all the data, typically from stdin, apply any logic it wants to and forward the analyzed data back to Hive by writing it to stdout. Furthermore, Hive offers great support for[User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) typically implemented in a JVM (Java Virtual Machine) based language. A UDF can take complex types as arguments, perform the analysis and return a single, eventually complex, value.


Additionally, Hive offers functionality to bring nested data back into a relational view, So called[UDTF’s](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-Built-inTable-GeneratingFunctions(UDTF)&sa=D&ust=1548243581515000) (User defined Table-generating functions) like explode() or inline(). These functions take a complex type field as a parameter and return multiple rows and columns, reflecting the same data in a relational view.


## Where it goes wrong


The only viable solution to analyze complex types and entirely stay within the query language is to use the UDTFs. This leads to wide usage of UDTFs in our analytical workload and hence becomes a priority for this article. A typical query looks like this


```text
SELECT   other, columns, aggregate_function   <  ...  >
FROM   lateral view udtf()   <  ...  >
GROUP BY   other, columns
```


It expands the complex types to columns and rows, then uses` GROUP BY` and aggregate functions to combine the extra rows into single values again. In large queries, the separation between all the places where something important with regards to the analysis is happening might become huge. This leaves the code hard to oversee and makes it easy to introduce bugs. A change in the` FROM` clause of the query might easily add or remove some rows that need to be accounted for in the aggregate function. Columns that are supposed to be just carried along, also need to get wrapped into an aggregate function or need to be stated in the` GROUP BY` clause, essentially cluttering the query.


Furthermore, Hive needs to introduce another MapReduce phase to execute the` GROUP BY` . This causes the data to be shuffled across the network, exactly the opposite of what we wanted when we kept the data together in a complex type.


With the[possibility](https://issues.apache.org/jira/browse/HIVE-13290) to define primary keys for tables in Hive, some potential to optimize these issues opens up. We don’t expect very significant breakthroughs too soon though.


## Our Goals


We wanted to come up with a solution that works for users purely in HiveQL, that allows for the SQL-Statements to be as dense as possible. Ideally, we also wanted to process data locally again and drop the shuffle requirement. It should be flexible enough to cover many use-cases even in situations where nesting levels get uncomfortably deep.


## Our Solution


After we found Hive[Ma](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/DropMacro)[c](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/DropMacro)[ros](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/DropMacro) as a way to define small functions in Hive, we were very excited. With a simple way to refer to them from UDFs, we could use them as[lambdas](https://en.wikipedia.org/wiki/Anonymous_function) and apply them to repeated values in our complex data structures. For now, we came up with a solution to look up Macros from the Hive function registry, where they end up after being defined. We built a few UDFs, that would invoke macros as needed, like` collection_map` to transform one collection type to another,` collection_filter` to remove some elements from collections and a few more. To illustrate how they work, I will take` collection_reduce` , a function capable of reducing a collection into a single value. Using the table definition above a record could look like this


```text
[
{
"deeper"  : {
"2"  : [  "bye"  ,   "bye"  ,   "world"  ],
"4"  : [  "hello"  ,   "world"  ]
}
},
{
"deeper"  : {
"24"  : [  "say"  ,   "hello"  ],
"42"  : [  "say"  ,   "bye"  ,   "bye"  ]
}
}
]
```


And we want to extract the amount of times each word occurs in the deepest nesting level.


We defined a macro to check if a value is in the map already. If it is, we increment the count by one, otherwise we initialize with 1.


```text
CREATE   TEMPORARY MACRO updateWordCount(word string, result_map map  <  string,  int>  )
update_collection(
result_map,
word,
IF  (result_map[word]   IS NOT NULL  , result_map[word]   +   1  ,   1  )
);
```


We create a macro to be called for every map element


```text
CREATE   TEMPORARY MACRO reduce_inner_map(
key_integer   int  ,
value_strings   array<  string  >  ,
result_map map  <  string,  int>
)   IF   (
key_integer   IS NOT NULL  ,
reduce_collection(  "updateWordCount"  , value_strings, result_map),
result_map
);
```


Then we create a macro to be called for every element of the most outer array


```text
CREATE   TEMPORARY MACRO extract_deeper_field(
inputstruct struct  <  deeper:map  <int  ,  array<  string  >>>  ,
result_map map  <  string,  int>
) reduce_collection(  "reduce_inner_map"  ,   inputstruct  .  deeper  , result_map);
```


Finally, we kick off the computation with some example data


```text
SELECT   reduce_collection(
"extract_deeper_field"  ,
array  (
named_struct(  "deeper"  , map(
4  ,   array  (  "hello"  ,   "world"  ),
2  ,   array  (  "bye"  ,   "bye"  ,   "world"  )
)),
named_struct(  "deeper"  , map(
24  ,   array  (  "say"  ,   "hello"  ),
42  ,   array  (  "say"  ,   "bye"  ,   "bye"  ))
)
),
map(  "dummy"  ,   0  ), false);
```


And get our result


```text
{
"world"  :   2  ,
"say"  :   2  ,
"bye"  :   4  ,
"hello"  :   2
}
```


The whole computation happened within the same Hive operator ─ there was no need to shuffle the data around. The entire logic how to handle this specific structure and computation was completely implemented in HiveQL and the code could be neatly arranged to be within 10-12 adjacent lines of code.


## Some obstacles to overcome


Of course, we overcame some obstacles on the way towards these UDFs. When invoking Macros, Hive will[convert](https://github.com/apache/hive/blob/7f0d6e69ec7aae756d1e7cee034df60b744482ce/ql/src/java/org/apache/hadoop/hive/ql/udf/generic/GenericUDFMacro.java#104,105,111) the parameter data type to its internal writable type using Hive Converters. These Hive Converters convert from struct to struct not by field name, but by ordinal position of the field. This sometimes leads to surprising behavior (when the types randomly match up) or crashes (if the types don’t). We had to query the Macro for its input types and convert before invoking the Macro so that the Macro wouldn’t use its internal conversion method.


We also need to make sure that we hang on to the Macro during the init on the Hive launching Java process. If we look the macro up by name from the function registry, we will not be able to find the macro on the worker tasks, but only in the Driver application. The good thing is that we could hang onto the Macro with simple Java serialisation so that we also have it available during execution.


## Availability


The Hive UDFs for processing can be found[here](https://github.com/trivago/hive-lambda-sting) . The code should work with a wide variety of Hive versions. Also, with Hive versions lower than 2.0, this[bug](https://issues.apache.org/jira/browse/HIVE-11432) should not affect this way of Macro processing.


## Outlook


We hope that with this utility we can convince more users to stick to the SQL-based approach. In the future, we plan to implement Map and Reduce operators using[kafka streams](https://kafka.apache.org/documentation/streams/) processor API and then be capable of executing the same logic with streaming in and outputs, allowing for real-time reporting.


In the meantime, we will work on easier unit testability for Hive macros and scripts to address more demands of our users. If these steps require more code to help with that, we will publish it in the repository as soon as it becomes available.
