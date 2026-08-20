---
schema_version: "1.0.0"
document_id: "ba32a1b5050e75a8301892247a5a33c0c222b448385d840e79b28be38699f255"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/taming-text-in-mysql-how-a-simple-emoji-uncovered-a-complex-collation-problem"
published_at: "2025-11-03T08:20:38+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:22e26a498d9e10f33907494e093789d82d1a6e022870b68574a7a7293c9e7820"
---

# Taming Text in MySQL: How a Simple Emoji Uncovered a Complex Collation Problem

###


Introduction


At Wix Emails we are working towards building a world-class email marketing platform along with an intuitive, fast, and reliable automated emails system. We care deeply about the quality of our products and work hard towards building a system that the users can count on.


Thus, when we saw a strange but occasional error some time ago it was not a question of whether users noticed it and whether we should wait until enough people complained. We needed to fix it immediately.


The error happened when when users were


filtering


their email campaigns:


```text
[HY000][1366] Incorrect string value: '\\xF0\\x9F\\x98\\x82' for column '...' at row 1
```


Initially, we were stumped. How come the string is incorrect for a column that contains, well, strings? Was someone trying to abuse the system? Were we modifying data in strange ways before it reached our MySQL database?


After the initial confusion wore off, we realized


\\\\xF0\\\\x9F\\\\x98\\\\x82


stands for “😂”. If someone was trying to abuse the system, they were having a jolly good time, it seemed. However, after some digging we realized it was a classic, innocent case of collation mismatch after all.


What are collations and what’s a collation mismatch, I hear you ask. Well, that’s what we’re about to find out. Despite being a professional developer for some years, I have not run into them myself until this error forced me to. Let’s stage the scene and explore them together.


###


Taming the Text


Once upon a time most text on computers was in ASCII. It was a simple, honest age, where people used only


[128 characters](https://en.wikipedia.org/wiki/ASCII) for communication. Each character mapped directly to a numeric value. Every character was known, and every byte told a simple truth.


It was also an age of frustration. Since the dawn of time, people were not content with plain things. From this malcontent


[more than 7000 languages](https://www.ethnologue.com/insights/how-many-languages/) sprang forth, resulting in thousands of letters and symbols. Eventually, people rebelled against the simple life.


Led by the noble Unicode Consortium they threw down the ASCII shackles, trading simplicity and honesty for the richness and complexity of


[Unicode](https://en.wikipedia.org/wiki/Unicode) . With it, one could finally represent any character from any of the languages. More importantly, one could finally use emojis.


However, as with the


[Lernaean Hydra](https://en.wikipedia.org/wiki/Lernaean_Hydra) , so in computer science: solving one problem caused twice as many to spring forth. The victory over character limits was complete, but now the text, stored safely in databases, began asking riddles. People wanted to sort and


rummage through


the stored treasures, yet what seemed simple before (


apple


before


banana


, and


citrus


after that) became a mind-bending puzzle.


What is


APPLE


to


apple


? Are they twins or distant relatives? If a user seeks one, should they find both? Which one should be presented first? What about a foreign


äpple


- is it still the same


apple


, or something else entirely? Would the neighbours next door agree? Does it matter?


To bring order to this chaos, collations were created.


###


The Rules of the Game: What Are Collations?


[Collation](https://en.wikipedia.org/wiki/Collation) is a fundamental concept for relational databases - virtually all major relational databases rely on them. It can be thought of as a set of rules that tells a database how to compare and sort sets of characters. For example, in the Swedish alphabet letter


å


[comes after](https://en.wikipedia.org/wiki/Swedish_alphabet) letter


z


.


This means that


ånga


(


*steam* ) would come after


zebra


. Collations also determine whether letter casing is taken into account. Using some collations we’d see


apple, APPLE, zebra, ZEBRA


, while with others the order would change to be


APPLE, ZEBRA, apple, zebra


. Plenty of collations have far more intricate rules, but the gist remains the same: given two characters, databases use collations to compare the characters and determine which of them comes first.


For some, this might seem a pedantic and irrelevant issue. Isn’t it better left for dusty academic journals while we march into the bright future free of such worries? Let us do some experiments on MySQL and see whether it is indeed something we should be concerned about in our day-to-day lives.


###


The Collation Laboratory


####


1 - Setting the Stage: MySQL Setup


To prepare for the experiment, let’s use a plain MySQL database. We’ll pull it from docker so that we don’t need to worry about setting it up. First, let’s create


docker-compose.yaml


file:


```text
services:
mysql:
image: mysql:8.0
container_name: mysql-local
environment:
MYSQL_ROOT_PASSWORD: root
MYSQL_DATABASE: collations
ports:
- "3306:3306"
```


Once we have it, let’s get it up and connect to it:


```text
> docker compose pull
> docker compose up -d mysql
# Feel free to use your favorite database IDE.
# However, if you prefer the loving embrace of the terminal:
> docker compose exec mysql bash
bash 5.1# > mysql -u root -proot collations
```


Now that we’re connected to our database, let’s create a very simple table for our experiments:


```text
CREATE TABLE texts(text TEXT NOT NULL);
```


Now we’re ready to collate!


####


2 - Inspecting the Toolbox: Character Sets & Collations


First, let’s see what our database has to offer:


```text
SHOW CHARACTER SET;
```


This should return 40+ rows of various


[character sets](https://developer.mozilla.org/en-US/docs/Glossary/Character_set) that are available for use. Some are more exotic, whereas the others are more wide-spread, such as:


-


ascii


- ASCII character set with 128 characters. One byte per character. Hard to beat the simplicity of it. Unless you do not happen to live in a 128 character-only universe.


-


[utf8mb3](https://dev.mysql.com/doc/refman/8.4/en/charset-unicode-utf8mb3.html) -


[UTF-8](https://en.wikipedia.org/wiki/UTF-8) character set that supports


[BMP](https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane) characters only, without support for


[supplementary characters](https://unicode.org/glossary/#supplementary_character) . Takes up to 3 bytes to store a character. Deprecated.


-


[utf8mb4](https://dev.mysql.com/doc/refman/8.4/en/charset-unicode-utf8mb4.html) - superset of


utf8mb3


. Supports


[BMP](https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane) characters and supplementary characters. Takes up to 4 bytes to store a character. Supersedes


utf8mb3


.


What about collations themselves? Let’s check:


```text
SHOW COLLATION;
```


This will likely return hundreds of collations. Some will be marked as default for the specified charset. For example,


utf8mb4_0900_ai_ci


is default for


utf8mb4


. A keen observer will notice that collation names start with the name of the character set, followed by one or a few suffixes.


The meanings of the more common ones are


[as follows](https://dev.mysql.com/doc/refman/8.4/en/charset-collation-names.html) :


-


_ai


- accent-insensitive


-


_as


- accent-sensitive


-


_ci


- case-insensitive


-


_cs


- case-sensitive


-


_bin


- binary


Taking these naming conventions we see that


utf8mb4_0900_ai_ci


represents a collation that deals with


utf8mb4


character set, and is accent and case insensitive.


But how do we check what collation is our


texts


table in? It’s as easy as issuing the following command:


```text
-- Assumes we're already connected to 'collations' database
SELECT TABLE_COLLATION FROM information_schema.TABLES
WHERE TABLE_NAME = 'texts'
AND TABLE_SCHEMA = DATABASE();
```


We can see that our collation is


utf8mb4_0900_ai_ci


, the default for


utf8mb4


. Note that the collation of the table is the default collation for the table. This means it will be applied to any


*new* columns in the table. However, table and column collations do not necessarily have to match.


####


3 - Collations: A Sort-Off


Rules for sorting texts are one of the primary use cases for collations. Let’s not waste our time and see it in practice. To do that, let’s add some data to our table first:


```text
INSERT INTO texts (text) VALUES ('apple'), ('Apple'), ('APPLE'), ('zebra'), ('ZEBRA');
```


Since we’re running with


utf8mb4_0900_ai_ci


, we’d expect the order to be more or less the same: apples should go with apples, zebras should go with zebras. Let’s confirm it:


```text
SELECT * FROM texts ORDER BY text;
```


This


*should* return:


```text
apple
Apple
APPLE
zebra
ZEBRA
```


Note the use of


*should* : since we are using a case insensitive ordering, there’s no guarantee that


apple


will come before


APPLE


. We’ll never get


apple


after


zebra


, but the order between apples is not deterministic. Can we do better?


Turns out we can. We can sort in case sensitive order by utilizing case sensitive collations. In this case, applying


utf8mb4_0900_as_cs


collation we will ensure that we will always get a consistent order that respects casing. We can switch to another collation in a couple of ways:


-


Change the collation of the column:


```text
ALTER TABLE texts MODIFY COLUMN text TEXT COLLATE utf8mb4_0900_as_cs;
```


If we wish to apply a new collation to the existing column, this is the way to do it.


Note, however, that it is paramount not to confuse


*column’s* collation with


*table’s* collation. We can change the collation of the table as follows:


```text
ALTER TABLE texts COLLATE utf8mb4_0900_as_cs;
```


However, this


*will not* affect the collation of existing columns - only the new columns that are added after the table's collation has been updated.


-


We can also apply the collation for the specified operation:


```text
SELECT * FROM texts ORDER BY text COLLATE utf8mb4_0900_as_cs;
```


Naturally, this command is less dangerous than the one on column. Changing collations on columns involves rebuilding the table which requires locks and additional disk space. It might result in data loss, break unique constraints (more on that later). Applying the collation to the command avoids these disasters.


Another interesting collation option is a binary one. The command:


```text
SELECT * FROM texts ORDER BY text COLLATE utf8mb4_bin;
```


Will get us:


```text
APPLE
Apple
ZEBRA
apple
zebra
```


This is because


APPLE


,


Apple


, and


ZEBRA


binary values come before


apple


and


zebra


values. Binary collations are relevant in situations where performance and exact matches are of paramount importance: there’s no need to consult complex Unicode rules, we can simply compare binary values, making database operations faster. However, for human readers the order might seem unusual.


####


4 - Collations: The Filter Face-Off


Usually we need to find the relevant data before doing anything with it. This presents a problem of how to go about


filtering it


. Since collations describe rules for string matching, they have a profound impact on how the


filter


functionality behaves and performs.


Taking what we already know about collations, we can make some educated guesses. We can infer that accent and case (in)sensitivity of the collation will have a big impact on the


filter


results. Let’s see if that is the case.


First, let’s add another apple and make sure we’re on


utf8mb4_0900_ai_ci


.


```text
INSERT INTO texts (text) VALUES ('äpple');
ALTER TABLE texts MODIFY COLUMN text TEXT COLLATE utf8mb4_0900_ai_ci;
```


Our filter should be both case and accent insensitive. In other words,


filtering


for


apple


should return all the apples we have. Let’s see:


```text
SELECT * FROM texts WHERE text = 'apple';
```


As expected, we get:


```text
apple
Apple
APPLE
äpple
```


Would switching to a more sensitive collation change anything?


```text
SELECT * FROM texts WHERE text = 'apple' COLLATE utf8mb4_cs_0900_as_cs;
```


Now we get a single result:


apple


. It seems our educated guesses have been correct.


You might wonder what would happen if we used


utf8mb4_bin


collation. Well, since the collation would be comparing


*binary* values (bytes), we’d only get the


*exact* match we’re


filtering


for, case, accent, and all.


This might be somewhat confusing as some Unicode characters look identical to human eyes. However, they still have different binary representations. Thus, binary collation is useful where we want to make sure that we get exactly what we ask for.


###


Navigating the Pitfalls


Sorting and


filtering


are the two obvious use cases for collations. By this point it should be clear that making a wrong choice might impact the results you get. However, there are more database operations that heavily depend on collations. If we accidentally mix the collations in these cases, the results can range from confusing to catastrophic.


Let’s try them out on our toy database so that we can avoid them in production.


###


**Deletion: More Than You Asked For**


Let’s first turn to a case that has the potential to make your day much worse really quickly: deletion. This one’s probably one of the most insidious gotchas that we might run into. What’s worse, we may not even realize something is wrong until it’s way too late.


Recall that we have a table in


utf8mb4_0900_ai_ci


collation with following rows:


```text
apple
Apple
APPLE
äpple
zebra
ZEBRA
```


Imagine that we had a UI where a user was able to delete a row by value. Let’s assume they enter


apple


, hit delete, and on the backend we execute the following statement:


```text
DELETE FROM texts WHERE text = 'apple';
```


Sure enough, the entry is deleted. Pretty standard, everyday stuff. But what’s left in our table?


```text
SELECT * FROM texts ORDER BY text;
```


…the query returns two lonely rows:


```text
zebra
ZEBRA
```


What happened? We asked to delete


apple


, but along with it the database deleted all other apples! How can this be?


As we already know,


utf8mb4_0900_ai_ci


, ignores both accent and case. Thus, when the database searches for rows to delete, it finds not only


apple


, but


Apple


,


APPLE


, and


äpple


as well. What seemed to be a rather innocent, targeted operation turned out to be a much more destructive command.


In many cases where collations are mixed up the result is confusing (or absent). However, ultimately it’s more annoying than anything. Delete, on the other hand, is dangerous precisely because oftentimes we’d not even notice that it’s deleting far more data than we asked for. Thus, we must take special care to make sure collation rules match our expectations.


###


**Uniqueness: They Are the Same Zebra**


After disaster with the delete we decide that it is warranted to have a more thorough data design. We vow to do better, and start by putting a unique constraint on our texts:


```text
TRUNCATE TABLE texts;
ALTER TABLE texts ADD CONSTRAINT unique_text UNIQUE (text(255));
```


With a clean table we proceed to inserting new entries:


```text
INSERT INTO texts (text) VALUES ('zebra');
```


We want to add another one, but this time louder:


```text
INSERT INTO texts (text) VALUES ('ZEBRA');
```


To our surprise, this fails with the following error:


```text
[23000][1062] Duplicate entry 'ZEBRA' for key 'texts.unique_text'
```


What gives? Won’t the database let us do anything right?


While initially confusing, recall that we’re using case


*insensitive* collation. It means the database will not differentiate between


zebra


and


ZEBRA


- it will assume they’re the same. Therefore, when instructed to insert


ZEBRA


into a


text


column with a unique constraint, MySQL sees that the value already exists, and fails the command.


A similar issue would present itself if we tried to apply a unique constraint on a column with case insensitive collation that had the two aforementioned values. It would refuse to put the unique constraint as it’d see duplicate values. The solution is to either clean up the data, or use case


*sensitive* collation.


###


**Indexes: Now You See Me, Now You Don’t**


Indexing is closely related to filtering and sorting. It’s unsurprising, then, that indexes depend on collations. What would happen if we tried to use a filter for the value in the column using a different collation than the column is in?


This is an interesting situation to check. Let’s build an index and add a few entries to our table:


```text
CREATE INDEX idx_texts ON texts (text(255));
INSERT INTO texts (text) VALUES ('apple'), ('APPLE');
```


Then, run the following query:


```text
EXPLAIN ANALYZE SELECT * FROM texts WHERE text = 'apple';
```


We’ll see something like this:


```text
-> Filter: (texts.`text` = <cache>(('apple' collate utf8mb4_0900_ai_ci)))  (cost=0.7 rows=2) (actual time=0.0162..0.0196 rows=2 loops=1)
-> Index lookup on texts using idx_texts (text=('apple' collate utf8mb4_0900_ai_ci))  (cost=0.7 rows=2) (actual time=0.014..0.0173 rows=2 loops=1)
```


Note that the index is used correctly (as indicated by the second output line). Now let’s try a stricter collation:


```text
EXPLAIN ANALYZE SELECT * FROM texts WHERE text = 'apple' COLLATE utf8mb4_0900_as_cs;
```


This time, we’re out of lucky - MySQL does a table scan instead:


```text
-> Filter: (texts.`text` = <cache>(('apple' collate utf8mb4_0900_as_cs)))  (cost=0.65 rows=1) (actual time=0.0339..0.0414 rows=1 loops=1)
-> Table scan on texts  (cost=0.65 rows=4) (actual time=0.0292..0.0357 rows=4 loops=1)
```


Suddenly, our index is not used anymore. The reason is that it was built with a different collation than what we’re using when


filtering


.


MySQL can’t reuse the index we already have, thus it has to resort to a table scan. For a few rows this will not matter. Imagine, however, if we had billions of rows - the


filter operation


would become extremely costly and slow.


###


**The Trouble with Joins**


We’ve seen how collations affect many aspects of the databases - from filtering to indexing. The list, however, is not exhaustive. For now, let’s put the cherry on top by briefly discussing joins between tables.


As we’ve seen, we can alter collations on the columns. It’s not a big stretch to figure out that columns in separate tables might have different collations. You might wonder what would happen if we tried to


JOIN


columns with different collations. In such a situation a collation with the strictest rules will be chosen. Thus, you may have


apple


in one table,


Apple


in another, and expect the


JOIN


to find them equal. However, if one of the columns in the


JOIN


clause has a case sensitive or binary collation, the join will find no matches.


This might be exactly what you’re expecting - or it might be confusing if one of the columns has case insensitive collation and we forget that the other does not.


###


Production Story: Mismatched Collations


Up until now, our discussion has been quite theoretical. Let’s come back to the issue we’ve faced in production at Wix Emails - the issue that sparked this blog post.


Wix Emails product has been in operation for over ten years. Naturally, we find that some choices we have made at the beginning of our journey have become somewhat outdated. One such choice was to use


utf8mb3_bin


collation for one of our main table’s columns.


This column contains text that users provide themselves, meaning that our users have relative freedom of what text ends up there. At the time, the choice of collation made sense, since


utf8mb3_bin


supports almost everything we see in practice, and it offers precise and quick comparisons. However, during the years we’ve started running into several problems.


One of the first issues was that our users started using emojis. This was not such a prevalent phenomena ten years ago, and we failed to consider it. However, nowadays many emails we send on behalf of our users have them. What happens when you have a table that does not support emojis, yet we try to shove it into it anyway? I’m glad you asked - let’s try it.


First, let’s downgrade our table’s collation to


utf8mb3_bin


:


```text
ALTER TABLE texts COLLATE utf8mb3_bin;
```


And now let’s add an apple that’s positively hilarious:


```text
INSERT INTO texts (text) VALUES ('apple 😂');
```


…which will fail:


```text
[HY000][1366] Incorrect string value: '\\xF0\\x9F\\x98\\x82' for column 'text' at row 1
```


This is the exact issue we have run into multiple times over the years. What can be done?


Since our table is rather large and changing collation on a column would involve table rebuild (which takes a long time and can be risky), we came up with a way to circumvent the limitations of


utf8mb3_bin


: we encoded emojis as a plain string, which could then be stored in the column. This solved an issue of saving and retrieving emojis.


What we failed to consider was that users might wish to search for the text, and the search term might include emojis as well. This resulted in occasional errors (search terms with emoji aren’t that popular yet).


While initially confusing, once we figured out collations were involved the reason for errors became clear: we tried


filtering


for characters that were not supported by the column’s collation. The fix, of course, was to apply the same encoding to the search terms. In the future, however, we’ll be able to avoid this workaround by using


utf8mb4_bin


from the get go.


###


Conclusion


Our experiments with MySQL and production stories highlight that collations are one of the cornerstones of databases. They permeate many of the key operations that are fundamental to databases: sorting,


filtering


, indexing, insertion, and deletion.


We’ve seen that understanding our use case is paramount. To pick the right collation, we must ask ourselves:


-


Do we want to support Unicode, including all the supplementary characters?


-


Perhaps our data is only ever going to contain ASCII characters, allowing us to pick a more restrictive, but simpler and faster collation?


-


Is user input going to end up in the queries? How do we expect it to work?


-


Do some of our commands explicitly specify collation to use which could hurt performance?


-


Do we know our


filter


patterns and do they match the indexes we have?


-


Do we foresee use cases in the future that may require us to be more flexible?


Answering these questions well will make the database queries faster, safer and more reliable.


Furthermore, understanding how collations work will let us write more efficient, simpler queries. Instead of firing


SELECT


*FROM texts WHERE* LOWER*(text) =


*LOWER* ('apple')


that does a table scan, we will be able to lean on case insensitive collation and simply run


SELECT * FROM texts WHERE text = 'apple'


, hitting the index and making the query blazingly fast.


Finally, understanding collations will inform us on one more crucial aspect of the databases, one more piece of a puzzle. While it may not be immediately relevant to our day-to-day tasks, it will allow us to appreciate what a beautiful, complex technology modern databases are. This in itself is a worthy pursuit.


This post was written by


**Simonas Viliunas**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) or
