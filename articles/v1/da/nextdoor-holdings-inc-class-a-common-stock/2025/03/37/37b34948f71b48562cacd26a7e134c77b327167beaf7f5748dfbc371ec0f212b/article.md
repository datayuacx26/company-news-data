---
schema_version: "1.0.0"
document_id: "37b34948f71b48562cacd26a7e134c77b327167beaf7f5748dfbc371ec0f212b"
company_key: "nextdoor-holdings-inc-class-a-common-stock"
company: "Nextdoor Holdings Inc."
source_id: "nextdoor-holdings-inc-class-a-common-stock-rss-10247875dc01"
canonical_url: "https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-4-c9d3d3edcd34"
published_at: "2025-03-19T15:08:57+00:00"
first_seen_at: "2026-07-20T23:17:36.074163+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:6e6b045be52cdd7a2439af92642af63195ea94d1de6abe31faca3b34cf6a30a4"
---

# Scaling Nextdoor’s Datastores: Part 4

# Scaling Nextdoor’s Datastores: Part 4


[Ronak Shah](https://medium.com/@ronakts?source=post_page---byline--c9d3d3edcd34---------------------------------------)


6 min read


·


Mar 19, 2025


--


In this part of the Scaling Nextdoor’s Datastores blog series, we will see how the Core-Services team at Nextdoor keeps its cache consistent with database updates and avoids stale writes to the cache.


In this post, we’ll focus specifically on inconsistencies caused by racing writes and our solution. We’ll discuss other causes and our full solution for consistent caching in the next installment of our blog:[Part 5: A time-bounded eventually-consistent cache](https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-5-5221da60f374) .


## Inconsistent Cache


Caches can become inconsistent with the database for several reasons, such as:


- **Racing Writes / Concurrent Updates:** Multiple writes occurring simultaneously can result in a stale cache.
- **Missed Writes / Failing to Update the Cache:** Failure to update or set cache correctly after a database write.
- **Delayed Cache Updates or Deletes:** Slow propagation of updates or invalidation can leave the cache out of sync.
- **Application-Level Bugs:** Bugs in application side caching logic.


Maintaining cache consistency with the database is crucial for data accuracy, especially in distributed systems with concurrent web requests. Consistent caching ensures reliable read-after-write behavior, improving performance and user experience. Without it, applications may face unpredictable behavior and user frustration. A well-designed caching system boosts performance, ensures consistency, and delivers up-to-date data even under high concurrency.


## Racing Writes


Let’s look at the scenario where two writers, A and B, update the same user in the database but write to the cache in a different order, causing the cache to become inconsistent with the database.


> Author’s Note: In examples moving forward we’ll use “User_1” to mean id=1 in the “users” table.


Look-Aside Cache with two writers.


Sequence of operations:


1. *Writer A* updates the name of *user_1* to *Foo* in the database.
2. *Writer B* updates the name of the same *user_1* to *Bar* in the database.
3. *Writer B* updates the cache for *user_1* with *name = Bar.*
4. *Writer A* updates the cache for user_1 with *name = Foo* .


As shown, the cache and database end up out of sync because the cache updates by Writer A and B occurred in a different order than the database updates. This results in an inconsistent *name* value for *user_1* between the database ( *Bar* ) and the cache ( *Foo* ). Essentially, this means stale writes were allowed in the cache.


From the above example, it’s clear that the cache allows stale copies to overwrite newer updates, leading to inconsistencies. Since execution order in distributed systems is beyond our control, our objective is to equip the cache with mechanisms to detect and reject stale updates to prevent such discrepancies.


## Our Approach


To ensure the cache can distinguish between new changes and stale ones, we need a mechanism to identify and reject stale updates while correctly applying newer ones. This can be achieved by introducing a version identifier for database table rows and using this version identifier to reject stale cache updates.


### Adding row versions to the database


While there are different approaches to generating row version information for database rows, the key part is that the client must not participate in the process. That is, the next version value must be generated on the database side in such a way that it is unique and monotonic.


> Author’s Note: Simply using a timestamp is not sufficient because it is not unique and is not always monotonic, as one might hope it would be.


Our solution was to introduce a new column called db_version to our tables. To handle the incremental versioning we implemented a Postgres database trigger to handle this. This trigger increments the version for each update and initializes it to 1 for the first insert.


## Get Ronak Shah’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Postgres Database Trigger:


```text
CREATE OR REPLACE FUNCTION uvf_update_db_version()                  RETURNS TRIGGER AS $$                  BEGIN                      IF TG_OP = 'INSERT' THEN                          NEW.db_version = 1;                      ELSEIF TG_OP = 'UPDATE' THEN                          NEW.db_version = OLD.db_version + 1;                      END IF;                      RETURN NEW;                  END;                  $$ LANGUAGE plpgsql;  CREATE TRIGGER uv_update_db_version BEFORE INSERT OR UPDATE ON <table_name> FOR EACH ROW EXECUTE FUNCTION uvf_update_db_version();
```


> Author’s Note: If you plan to use a trigger or UDF, ensure that your database provides the same consistency level as your update. That is, you want the row update and version update to be an atomic operation from a consistency perspective.


When our application updates a row in the database, it also needs to update the cache with the new value. Since Django’s ORM does not return the updated row, we perform a select query after the update operation to retrieve the incremented version from the database. Both the update and select queries are executed within a transaction block.


### Adding Version to the Cache


Since we added version information to database tables, we also need to include versioning in the cache to detect and reject stale writes.


To achieve this, we incorporate the version as a metadata header in the cache value. This header contains the version information and additional metadata. For more details, refer to Part 3: Appropriately serializing data for caching.


The version is included as a metadata header to maintain separation between serialization methods and cache functionality. This means the cache doesn’t need to know how to interpret the payload to get the version information from the serialized object.


### Atomic Cache Operations


To correctly update the cache, we need to compare the supplied and stored version and perform the update as an atomic operation. Doing this on the client side is not feasible because we operate in a distributed and concurrent environment, where multiple cache updates to the same data can occur simultaneously. If the version check happens on the client side, there is a risk of stale writes, as the version may have changed after the check but before cache update.


A similar issue can exist on the cache side if the compare and update are not an atomic operation. We were able to leverage Redis’s single threaded nature to our advantage because it naturally protects against concurrent access. This allowed us to write custom Lua functions, executed by Redis, to perform conditional updates and deletes without worrying about concurrency issues.


## Custom Lua functions


To effectively detect and reject stale writes to the cache, we implemented two Lua functions to perform conditional updates and deletes. The function stubs are:


```text
--- Sets a key only if the provided version is greater than the stored version or if the key does not exist.  -- If the supplied version is equal to or lower than the stored version, the operation is rejected.  -- @param key (string) Name of the key.  -- @param version (string) Corresponds to the database version value.  -- @param metadata (string) Additional header information to store.  -- @param value (string) Data to associate with the key.  -- @param ttl (number) Time-to-live for the key in seconds.  -- @return (string) "OLD" if the supplied version is lower than the stored version.  --                  "OK" if the key was set or if versions match.  function set_if_version(key, version, metadata, value, ttl)     ...  end   --- Deletes a key only if the stored version is less than the supplied version.  -- @param key (string) Name of the key.  -- @param version (string) Corresponds to the database version value.  -- @return (number) 0 if the key was not deleted or did not exist.  --                 1 if the key was deleted.  --                -1 if the key was not deleted because the stored version is greater than the supplied version.  function del_if_version(key, version)     ...  end
```


## Racing Write Example


Let’s bring everything together and see how we handle the racing write problem and prevent stale writes to cache.


Sequence of operations:


1. Writer A updates the name of User_1 to Foo in the database.
2. Writer A receives db_version=12 from database
3. Writer B updates the name of the same User_1 to Bar in the database
4. Writer B receives db_version=13 from database
5. Writer B updates the cache for User_1 with version=13 using set_if_version
6. Writer B receives OK from set_if_version call since it updated the cache
7. Writer A updates the cache for User_1 with version=12 using set_if_version
8. Writer A receives OLD from set_if_version call since it supplied old version and the cache update was rejected


With this solution, we can effectively detect and prevent stale writes to the cache.


## Conclusion


To address cache inconsistencies due to racing writers, a look-aside cache must have a way to reject stale writes. This will mean each database row version will need a unique and monotonic version and the cache must perform conditional updates in a serializable manner.


This approach is a building block to solve more advanced consistency edge cases such as missed writes and racing cache fills. To learn more about these issues and the solution we implemented, stay tuned for[Part 5: A Time-Bounded Eventually-Consistent Cache](https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-5-5221da60f374) of the Scaling Nextdoor’s Datastores blog series.


Authors:


[Ronak Shah](https://medium.com/u/3d01db79be59?source=post_page---user_mention--c9d3d3edcd34---------------------------------------)


,[Slava Markeyev](https://medium.com/u/d8db977cf129?source=post_page---user_mention--c9d3d3edcd34---------------------------------------)


, and[Tushar Singla](https://medium.com/u/e734698562ba?source=post_page---user_mention--c9d3d3edcd34---------------------------------------)
