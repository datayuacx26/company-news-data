---
schema_version: "1.0.0"
document_id: "e2c9b4c4c9adabf0e986990f53ee58d7f35659277df9d03baf1726d7acf98aa7"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/monitor-aws-redshift-top-s3-scan-queries-bf502ab9f73c"
published_at: "2022-09-08T18:55:08+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:30.015715+00:00"
content_hash: "sha256:58b82bea065cb17f400f803aad40ee1fa4c18eac046ba19712d6fea563d80032"
---

# Monitor AWS Redshift top S3 scan queries

# Monitor AWS Redshift top S3 scan queries


[Gordon Yuan](https://medium.com/@gyuan_91840?source=post_page---byline--bf502ab9f73c---------------------------------------)


8 min read


·


Sep 8, 2022


--


Press enter or click to view image in full size


Image courtesy[stockup](https://stockup.sitebuilderreport.com/) .


> Gordon is a Data *Engineer* at Cars.com and he has been a Senior Database Administrator with over 15 years of experience developing, deploying, evaluating, and enhancing Database Systems.


CARS’s business is growing (measured by our customer-base) and with that our tech teams have grown as well. With the addition of new products and instrumentalizing our platform strategy, the need to keep our data-production servers in healthy state 24X7 is evident. We now have more prod-jobs, data and machine learning processes than ever before. The total queries in production and lower environments have increased tremendously. In this technical-blog (catering to AWS users and data engineers), we articulate the need to monitor S3 scan queries and how to bring it to action.


**Why do we need to monitor S3 scan queries?**


Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud. With Amazon Redshift, we can use SQL to analyze structured and semi-structured data across data warehouses, operational databases, and data lakes, using AWS-designed hardware and machine learning to deliver the best price performance at any scale. One of the challenges is to protect the data warehouse workload from poorly written queries that can consume significant resources.


Spectrum is a key component for Redshift. With Redshift Spectrum, we can Run SQL queries directly against the data in our Amazon S3 data lake, out to exabytes. Cars.com data team uses Spectrum a lot since more than 90% data stores in S3 data lake. However, With Redshift Spectrum, we are billed at $5 per terabyte of data scanned. According to the Redshift cost explorer breakdown by usage type in our Production data account in the past few months, around 40% cost was from S3 data scanned.


Therefore, monitoring S3 top scanned queries becomes critical for cars.com data team. Below are 4 steps we took to get the monitoring ready.


1. **Redshift Spectrum Data Scan simple query**


To find how much data is being transferred by Redshift spectrum queries, we will have to look at system table ‘SVL_S3QUERY_SUMMARY’ and a specific field called ‘s3_scanned_bytes’ (number of bytes scanned from Amazon S3). The cost of a Redshift Spectrum query is reflected in the amount of data scanned from Amazon S3.


s3_scanned_bytes — The number of bytes scanned from Amazon S3 and sent to the Redshift Spectrum layer, based on compressed data.


For example: To determine number of bytes transferred by 1 particular spectrum query:


```text
select s3_scanned_bytes from svl_s3query_summary where query=  query_id  ;
```


To determine sum of bytes of all queries from Redshift spectrum:


```text
select sum(s3_scanned_bytes) from svl_s3query_summary ;
```


To determine sum of bytes of all queries in last 24 hours from Redshift spectrum:


```text
select sum(s3_scanned_bytes)/1024/1024/1024 total_scan from svl_s3query_summary  where starttime >= Getdate () — ‘1 day’ :: interval;
```


**2. Redshift Spectrum Data Scan advanced query**


SVL_S3QUERY_SUMMARY contains only a summary of Amazon Redshift Spectrum queries (S3 queries) that have been run on the system. It doesn’t include more detailed information such as who ran the query (user name) and what the query text looks like. To retrieve all that information, we need to join SVL_S3QUERY_SUMMARY with svl_user_info to retrieve user name and also join with stl_query & svl_statementtext to get query text.


Initially, I tried below query:


```text
select top 10 s.query as query_id, starttime as time, s3_scanned_bytes as scan_size, t.text as sql  from SVL_S3QUERY_SUMMARY s  left join STL_QUERYTEXT t on s.query=t.query  where s.starttime>= GETDATE () - '1 day'::INTERVAL  and t.sequence=0  order by s.s3_scanned_bytes desc;
```


Unfortunately, the result from the above query doesn’t show ‘good’ SQL text. The SQL text only shows: fetch 10000 in “SQL_CUR” because most of our top queries are from Tableau. For whatever reason, the fetch cursor statement is a part of Tableau’s mechanism to run queries on Redshift. To solve this problem, we need to use the LISTAGG function with system table svl_statementtext. For each group in a query, the LISTAGG aggregate function orders the rows for that group according to the ORDER BY expression, then concatenates the values into a single string.


Below script generate an admin view which contains user name, s3 scanned size, and SQL text:


```text
create or replace view admin.top_10_s3_scan_vw  as SELECT  p.usename                                            AS user_name,  s.userid                                             AS user_id,  s.s3_scanned_bytes/1024/1024/1024                    AS   3_scanned_gb,  s.starttime                                          AS start_time,  s.endtime                                             AS end_time,  s.query                                       AS query_id,  LISTAGG(distinct st.text, ',' ) within GROUP (ORDER BY st.SEQUENCE) AS SQL  FROM      svl_s3query_summary s  left join svl_user_info p  ON p.usesysid = s.userid  left join stl_query t  ON        s.query=t.query  left join svl_statementtext st  ON        t.xid=st.xid  WHERE     s.starttime>= getdate () - '1 day'::interval  AND       st.TYPE <> 'DDL'  AND        s.userid > 100  AND        s.userid != 286 -- This is a super user sometimes causes larger query which scans small data  GROUP BY  s.query,  p.usename,  s.userid,  s.starttime,  s.endtime,  s.s3_scanned_bytes  ORDER BY  s3_scanned_gb DESC limit 10;
```


In the above query, we need to reconstruct the SQL stored in the text column of the system table SVL_STATEMENTTEXT, run a SELECT statement to create SQL from 1 or more parts in the text column. Keep in mind, the text column data type of SVL_STATEMENTTEXT is character(200), in 200-character increments. We need to use DISTINCT to return a list of unique text, aggregate text, ordered by SEQUENCE in system table SVL_STATEMENTTEXT.


The return type for LISTAGG is VARCHAR(MAX). If the result set is larger than the maximum VARCHAR size (64K — 1, or 65535), then LISTAGG returns the error “Result size exceeds LISTAGG limit”. LISTAGG with distinct often helps solve the issue of errors. We also found out that sometimes the larger query is scanning pretty less data from admin and super users. Therefore, we had to add s.userid > 100 (admin users) and s.userid != 286 (a super user) to filter admin and super users in the WHERE clause.


Additionally, we can create another view in the admin schema to find out a daily S3 scan/usage group by user. With this view, we can quickly find out who scanned most/least S3 data in the past 24 hours.


```text
CREATE OR replace VIEW admin.daily_user_s3_scan_size_view AS  SELECT p.usename,  q.userid,  Min(starttime)                 AS time_started,  Count(DISTINCT s.query)        AS total_query_run,  SUM(s.s3_scanned_bytes)/1024/1024/1024        AS scan_gb,  SUM(q.query_cpu_time)          AS cpu_time,  SUM(q.query_execution_time)    AS exec_time,  SUM(q.query_cpu_usage_percent) AS cpu_pct,  SUM(q.cpu_skew)                AS cpu_skew,  SUM(q.io_skew)                 AS io_skew  FROM   svl_user_info p  join svl_query_metrics_summary q  ON p.usesysid = q.userid  join svl_s3query s  ON q.query = s.query  WHERE  s.starttime >= Getdate () - '1 day' :: interval  GROUP  BY p.usename,  q.userid  ORDER  BY scan_gb DESC;
```


**3. Create a Lambda function to publish top queries to an SNS topic**


AWS Lambda is an event-driven, serverless computing platform as a part of AWS. It is a computing service that runs code in response to events and automatically manages the computing resources required by that code.


## Get Gordon Yuan’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Below Lambda function is invoked on a scheduled interval through an Amazon EventBridge rule, connects to the Redshift cluster, runs the SQL (querying the view created in previous section) to pull top 10 S3 scanned queries, and publishes top queries to an SNS topic as a JSON string. Admin team receives an email daily for top 10 queries with user name, s3 scanned size, full SQL text, etc. We can then forward the email to specific DB users or teams for query review & potential improvement. By optimizing queries, we successfully cut down the top 10 S3 total scan size by a good percentage.


```text
#!/usr/bin/env python  import sys  import os  import base64  import pg8000  import datetime  import boto3  import json  import collections  # add the lib directory to the path  sys.path.append(os.path.join(os.path.dirname(__file__), "lib"))  print('Loading function')  ssl = True  debug = False  pg8000.paramstyle = "qmark"  def get_env_var(name):    return os.environ[name] if name in os.environ else None  # cluster connection settings and SNS from environment:  database = get_env_var('database')  sns_arn = get_env_var('sns_arn')  secret_name = "arn:aws:secretsmanager:us-east-1:xxxxxxxxxxxx:secret:prod/redshift-data_admins-2cAEih"  region_name = "us-east-1"  session = boto3.session.Session()  client = session.client(  service_name='secretsmanager',  region_name=region_name  )  get_secret_value_response = client.get_secret_value(SecretId=secret_name)  secret = json.loads(get_secret_value_response['SecretString'])  user = secret.get('username')  password = secret.get('password')  host = secret.get('host')  port = secret.get('port')  clusterid = host.split(".")[0]  try:    sns = boto3.resource('sns')    platform_endpoint = sns.PlatformEndpoint('{sns_arn}'.format(sns_arn = sns_arn))  except:    print('SNS access failed: Exception %s' % sys.exc_info()[1])  def run_command(cursor, statement):    if debug:      print("Running Statement: %s" % statement)    return cursor.execute(statement)  def publish_to_sns(message):    try:      if debug:    print('Publishing messages to SNS topic')    # Publish a message:    response = platform_endpoint.publish(    Subject='Redshift Prod - Top 10 Query daily Monitoring Notifications',    Message=message,    MessageStructure='string',    MessageAttributes={      'RedshiftQueryMonitoringRuleNotifications': {        'StringValue': 'Redshift Prod - Top 10 Query daily Monitoring Notifications',        'DataType': 'String'      }    }    )    return  response  except:    print(' Failed to publish messages to SNS topic: exception %s' % sys.exc_info()[1])    return 'Failed'  def query_redshift(conn):    try:      sql_query = """select * from admin.top_10_s3_scan_vw;""      print(sql_query)            # Connect to Redshift and run the query      cursor = conn.cursor()      run_command(cursor,sql_query)      result = cursor.fetchall()      objects_list = []      for row in result:  user_name,user_id,s3_scanned_gb,start_time,end_time,query_id,sql=row        d = collections.OrderedDict()        d['user_name'] = user_name        d['user_id'] = user_id        d['s3_scanned_gb'] = s3_scanned_gb        d['start_time'] = start_time.isoformat()        d['end_time'] = end_time.isoformat()        d['query_id'] = query_id        d['sql'] = sql        objects_list.append(d)      #Publish to SNS if any rows fetched      if len(objects_list) == 0:        print('No rows to publish to SNS')      else:        json_formatted_str = json.dumps(objects_list, indent=2)        print(json_formatted_str)        response= publish_to_sns(json_formatted_str)      cursor.close()      conn.commit()      print('Completed Successfully ')    except:      print('Query Failed: exception %s' % sys.exc_info()[1])      return 'Failed'  def lambda_handler(event, context):    try:      if debug:        print('Connect to Redshift: %s' % host)      conn = pg8000.connect(database=database, user=user, password=password, host=host, port=port, ssl_context=ssl)    except:      print('Redshift Connection Failed: exception %s' % sys.exc_info()[1])      return 'Failed'    if debug:      print('Successfully Connected Redshift Cluster')    query_redshift(conn)    conn.close()    return 'Finished'
```


**4. Implement an aborting rule based on Spectrum scan size**


After a few months by closely monitoring Redshift top S3 scanned queries, we learned the threshold we can configure to abort Spectrum scan queries through Query monitoring rules to better manage Redshift S3 scan cost.


One of query monitoring rules has been configured as below for all Workload queues in our mission critical Redshift production cluster:


Predicates: Spectrum scan (MB) > 2000000


Actions: abort


To monitor whose queries were aborted through QMR in past few days, below query can be used:


```text
select b.usename, a.*  from stl_wlm_rule_action a inner join pg_user b on a.userid=b.usesysid  where action = ‘abort’ order by recordtime desc;
```


Another big question may be raised — how do we improve Spectrum S3 scan queries? There are lots of options to optimize your queries. For example, avoid using select * and include only the columns you specifically need; use a WHERE clause to restrict the dataset as much as possible; use partitions to limit the data that is scanned; use materialized views, etc.


One example is from one big Tableau Query. The query was aborted because Spectrum scan size was more than 2 TB after we implemented the QMR. The query tried to scan one year data and the WHERE clause contains filter “cast(filedate as date) between last_day(add_months(current_date, -13)) + 1 and current_date-1”.


I forwarded the email alert to and discussed with the developer who owns the query. The developer then spoke to the related business team and agreed that we can instead display the data at a Month level. I worked with the developer and changed the WHERE clause to restrict the dataset from one year to one month by changing last_day(add_months(current_date, -13)) to last_day(add_months(current_date, -1)) in subqueries. We tested the new process, a month refresh only scanned 170 GB!


Follow[this](https://tech.cars.com/) space for technical content on Elixir and Python Programming Languages. We also have good resources on our Machine Learning & Data Engineering Practices. Also, check out open positions at[CARS](https://www.cars.com/careers/) .


**References and System tables/views used::**


Amazon Redshift pricing:[https://aws.amazon.com/redshift/pricing/#Redshift_Spectrum_Pricing](https://aws.amazon.com/redshift/pricing/#Redshift_Spectrum_Pricing)


Monitoring Metrics in Amazon Redshift Spectrum:[https://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-metrics.html](https://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-metrics.html)


Improving Amazon Redshift Spectrum query performance:[https://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-external-performance.html](https://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-external-performance.html)


SVL_S3QUERY_SUMMARY:[https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_S3QUERY_SUMMARY.html](https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_S3QUERY_SUMMARY.html)


SVL_USER_INFO:[https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_USER_INFO.html](https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_USER_INFO.html)


STL_QUERY:[https://docs.aws.amazon.com/redshift/latest/dg/r_STL_QUERY.html](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_QUERY.html)


SVL_STATEMENTTEXT:[https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_STATEMENTTEXT.html](https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_STATEMENTTEXT.html)
