---
schema_version: "1.0.0"
document_id: "e4f5d4afa4e2435ac772898897cad2510b82e63069f4c7e487892c0ee5a4910a"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2018-09-03-s3-versioning/"
published_at: "2018-09-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:f18733cdc3f69f69c3e46e99f38dbb6d03bda0285419dfc5e66be497d483a4f3"
---

# Efficient Image Recovery at Scale Using Amazon S3 Versioning

### How trivago Manages Images From a Million Hotels


## Introduction


If you’re using Amazon Web Services, then there is a higher possibility that you’re familiar with Amazon S3.[Amazon S3 ( Simple Storage Service )](https://aws.amazon.com/s3) is a widely used service where we can store (theoretically unlimited amount of) our data with a high availability[99.99%](https://tech.trivago.com/post/1) . That’s why we, the Visual Content team at trivago, use Amazon S3 to store the images which you see on our website and many other tools.


## The challenge


We, the Visual Content team, recently came across a scenario related to recovering deleted images, which made us realize that we should have a reliable backup method for our images. When we were doing the research on finding a better solution, we realized that it would not be cost-efficient to keep copies of our files as regular backups, as we have hundreds of millions of images and it would cost us the same amount as we’re paying for the S3 bucket, where we store the original (live) data.


There are two other services offered by Amazon: S3 IA (S3 Infrequent Access) and \[Amazon Glacier\] ([https://aws.amazon.com/glacier](https://aws.amazon.com/glacier) ). As the name suggests, S3 IA is used when the backed-up files don’t need to be accessed frequently, which will add additional costs when retrieving the files. Amazon Glacier is an archiving method which introduces an additional delay along with the additional cost for the data retrieval \[2\].


Furthermore, we usually do not replace/overwrite files in our S3 bucket, but we upload and (in some cases) delete them. With these requirements, we needed a reactive way of backing up rather than a bulk method, where we will only backup the changed files. Then, we will have a mechanism to rollback in case our files get deleted (by accident or due to any other technical issue). Since most of the third-party solution providers have a bulk-backup approach, we kept on looking for an efficient, yet cost-effective method.


Then we got to know that Amazon S3 itself provides the most convenient solution which matches our requirements perfectly: S3 Versioning.


NOTE: We can afford to spend some time on deciding the filtering criteria for the file restore because even if the files are deleted from S3, they would still be available in the CDN caches for a couple of days.


## Amazon S3 Versioning


Amazon’s approach is not a complex one, yet very effective from the perspective of storage, and ultimately cost. If we enable versioning for an S3 bucket, which is as simple as putting a check mark on the options page of the bucket, Amazon will create a new version of each file only when they’ve changed. Which means, when we replace a file with a new one, it will upload the file with a new version whilst keeping the original file. In case we delete a file, a placeholder called a[Delete Marker](https://docs.aws.amazon.com/AmazonS3/latest/dev/DeleteMarker.html) will be added related to the file while the original file remains untouched. However, from the S3 web console, we have the option to show only the latest versions of the files, which saves us from a lot of confusion.


For ease of understanding, imagine the version history of a single file as a stack, where the latest versions (including the delete markers) are pushed from the top.


However, if we want to delete a file (or a version) permanently, we should delete the file by providing the specific version we want to delete. If you’d like to know more about S3 Versioning, check out the \[official documentation\] ([https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html) ).


## Amazon S3 versioning API


Amazon S3 API provides two useful endpoints for version control:


1. \[list-object-versions\] ([https://docs.aws.amazon.com/cli/latest/reference/s3api/list-object-versions.html](https://docs.aws.amazon.com/cli/latest/reference/s3api/list-object-versions.html) )
2. \[delete-objects\] ([https://docs.aws.amazon.com/cli/latest/reference/s3api/delete-objects.html](https://docs.aws.amazon.com/cli/latest/reference/s3api/delete-objects.html) )


### Listing the object versions


With the first API call, we will get **all** the versions of **all** the files in the given bucket, and there is no method to specify any filters except the *prefix* for the key (file path). As all the responses are paginated, we should use` NextKeyMarker` and` NextVersionIdMarker` from each response and use them for` KeyMarker` and` VersionIdMarker` respectively for the next request, until we receive a response with` IsTruncated` as` False` . Because of this dependency, this retrieval process seems sequential.


In the first iteration of our script, it would have taken more than 60 hours to list all the 100 million files we have in our S3 bucket. We needed to think about ways to reduce this runtime.


### Cleanup and restore operations


Each of the entries in the responses we get from list-object-versions will have two lists:` Versions` and` DeleteMarkers` , which contain object versions and delete markers separately. With these lists, we can,


- Restore the deleted files by deleting the Delete Markers on where the` IsLatest` is` True` .
- Delete older versions of a file by deleting the objects where` IsLatest` is` False` .
- Use the` LastModified` field in the response if we want to restore our bucket to a particular point in time.


## Our Approach


On top of the traditional approach, we made several modifications and enhancements to get better results.


### Making` list-object-versions` requests in parallel


As we explained earlier, the object versions retrieval process *seems* sequential. But if we know how the files and directories are arranged in the bucket, we can perform parallel independent requests for different prefixes which will return the object versions for the files of which the key (file path) starts with the` prefix` . In this way, we can parallelize the` list-object-versions` API calls.


For instance, if we have 8 directories in our S3 bucket (named dir*1… dir_8), we can run 8 threads in parallel performing list-object-versions API calls iteratively, each with dir***x** as the` prefix` .


### Storing the results


An important point to remember is Amazon charges \[per request\] ([https://aws.amazon.com/s3/pricing/#Request_Pricing](https://aws.amazon.com/s3/pricing/#Request_Pricing) ). So, we should try our best to optimize the usage of the API calls. At the time of writing, their pricing is as follows:


` PUT, COPY, POST, or LIST Requests $0.005 per 1,000 requests`


For an example, if we have 100 million objects (with one version) in our bucket, we have to make 100,000 requests to get all the object versions if each response contains 1000 entries. So we would pay 0.005 x (100,000/1000): $ 0.5 to list our entire bucket. If we have many versions of a file, this cost will increase drastically.


So, imagine we have written a nice script to list all the versions, filter the objects and` DeleteMarkers` with our conditions, and delete them. When we are running this script halfway through, it exits due to *some reason* and now we have to run the script again. Even if we can bear the time it takes to run, we should keep in mind it will cost us as well. So, there should be a way to avoid requesting the entire list in such a scenario.


And, on top of that, embedding the filter criteria into our script itself is complex, and then it will slow down the retrieval process. And if the filtering needs feedback from some other party (such as a product owner or a consumer), we can’t perform the deletion on-the-fly.


Because of these constraints, we thought of storing these responses we get from the` list-object-versions` API call in two MySQL database tables:` objects` and` delete_markers` . The reason we chose MySQL is that we wanted to enable this script to be run on even our local machines, where SQLite doesn’t support parallel writes\[3\] and using a cloud database would have added more complexity.


After we get a response, we formulate two queries with multiple rows for the` Versions` and` DeleteMarkers` in that response and we execute them on the two respective tables. In this way, we could reduce the number of insert queries made to the database as well. So, each thread which is responsible for a prefix will do the API call, receive the response, formulate the two queries and then insert into the tables, until it receives the` IsTruncated` as` False` .


Even if the retrieval process stops (for some reason), now we can restart the retrieval by retrieving the starting keys and versions, as we have the last retrieved key and version for each prefix.


### Making` delete-objects` requests in parallel


After refining the versions which need to be restored (i.e. the delete markers to be deleted), we can perform multiple delete API requests in parallel. We can have a configurable number of threads and each thread will read a chunk of records from the database, formulate a delete request and submit it to the API until the number of records read from the database is 0.


For instance, if we have 8 threads and we want to generate deleted requests with 1000 object versions to delete (given the fact that the database now only has versions to be deleted), each thread will read blocks (of 1000s) one after every 8 blocks. It means thread-1 will read rows 0-999, 8,000-8,999, 16,000-16,999 and so on while other threads follow the same pattern.


The reason to use this kind of pattern is that we initially don’t know the number of rows in the database and executing a` count(*)` will also take a few minutes when we have millions of records.


## Discarded approaches


We settled on the method described above after discarding several other approaches. Nevertheless, we’d like to share the learnings of our failed attempts.


### Sequential calls for list-object-versions


Calling the` list-object-versions` endpoint sequentially for the entire bucket is very inefficient. In the first stage, it took us around 40 minutes to retrieve one million objects. After we used multiple threads where each thread handled its own` prefix` , we were able to retrieve one million objects (and write them into the database) in under 5 minutes.


### In-memory filtering


As mentioned earlier, we came across several cases where the script stopped due to some errors and we had to restart it to read the entire versions list from the beginning. Not just to avoid re-iteration, but also to enable offline filtering, we decided to avoid in-memory filtering and use a database. However, we did not use SQLite since it does not support parallel writes. We chose MySQL not only because it supports parallel writes, but also we did not want to complicate our solution by introducing in-memory databases.


### Producer-Consumer pattern


One of the best ways to parallelize this kind of process is to use the producer-consumer pattern, where the producers will do the data retrieval and the consumers will perform the task. First, we implemented producer-consumer for both the retrieval and deletion phases, but it became complex to handle the edge cases.


For instance, the typical producer-consumer pattern runs theoretically infinitely, however in our case we know when the producers are done and there is no simple way to communicate that to the consumers. Passing a simple “done” flag to all consumers and checking on each iteration wouldn’t work, because we have multiple producers and we have to wait for all of them to finish. Because of these kinds of edge cases, there is a high chance for consumer threads to enter deadlocks and/or starvation. Therefore, we avoided using the producer-consumer pattern.


## Final results


We tested our final solution (in Python3 using[Boto3](https://github.com/boto/boto3) ) on one of our largest S3 buckets with around 100 million objects and the results were promising. We chose Python not just because we use Python to write most of our code in Visual Content, but also because of the portability and flexibility we needed for this script.


We were able to retrieve one million objects in under 5 minutes, which means it will only need ~8 hours to list the object versions, which would have taken 60-70 hours if we did it in a completely sequential manner without using prefixes. According to our calculations, we can submit one million delete requests in under 3 minutes, which means it will only take 5 hours to perform the restore on our entire bucket if we deleted the whole 100 million of our files. The summation of the duration is less than one day and it allows us to have enough time to do the filtration and recover within the CDN cache period.


Since we write the entries to a database, we were also concerned about the disk space utilization and it was using 100 MB per 1 million rows, where we would end up around 10 GB to store all the entries in our bucket (with 100 million objects).


## Summary


Backing up a large S3 bucket will cost us more than what we’re paying for the original data. One of the best approaches we came across is to enable versioning for the S3 bucket. In case of a bulk deletion of objects, we can write a script to restore the objects. We implemented our solution with the goal of being able to recover an entire S3 bucket with millions of files.


However, there might be other approaches which could have given better results. Feel free to let us know how we can improve or what we could have done. If you like to work on similar projects with us, maybe you even like to apply for our \[open positions\] ([https://careers.trivago.com/join/open-positions](https://careers.trivago.com/join/open-positions) ).


Cheers!


## References


\[1\] See “General S3 FAQs” → “How reliable is Amazon S3?” Section in the[FAQs](https://aws.amazon.com/s3/faqs/)


\[2\] \[Amazon S3 Storage Classes\] ([https://aws.amazon.com/s3/storage-classes](https://aws.amazon.com/s3/storage-classes) )


\[3\] \[SQLite FAQ\] ([https://www.sqlite.org/faq.html](https://www.sqlite.org/faq.html) ) : Can multiple applications or multiple instances of the same application access a single database file at the same time?
