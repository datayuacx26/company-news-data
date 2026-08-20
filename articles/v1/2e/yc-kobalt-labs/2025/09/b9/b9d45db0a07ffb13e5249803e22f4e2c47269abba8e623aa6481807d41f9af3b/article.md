---
schema_version: "1.0.0"
document_id: "b9d45db0a07ffb13e5249803e22f4e2c47269abba8e623aa6481807d41f9af3b"
company_key: "yc-kobalt-labs"
company: "Kobalt Labs"
source_id: "yc-kobalt-labs-rss-c4f45a5ecda6"
canonical_url: "https://blog.kobaltlabs.com/sqlalchemy-checkconstraints-operator-precedence-84ce740df71a"
published_at: "2025-09-05T14:35:47+00:00"
first_seen_at: "2026-07-20T23:21:08.518495+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:6baa28f15ac0c88c29b2aa74b8a14a97417a33124898f4f904bae331d605cb70"
---

# sqlalchemy checkconstraints operator precedence

Sqlalchemy


Alembic


# sqlalchemy checkconstraints 🤝 operator precedence


[Ashi Agrawal](https://medium.com/@ashi.agrawal?source=post_page---byline--84ce740df71a---------------------------------------)


4 min read


·


Jun 28, 2025


--


## the gist


At Kobalt, our backend is built on Python + Postgres. We use SQLAlchemy as our ORM, and Alembic as our corresponding database migration tool. Although Alembic has been around still 2011, it still has many limitations— there’s a[whole section](https://alembic.sqlalchemy.org/en/latest/autogenerate.html#what-does-autogenerate-detect-and-what-does-it-not-detect) in the documentation describing changes Alembic can’t automatically generate correct SQL for.


Here’s one of those surprising edges: how I started with a simple` CheckConstraint` and ended up tangled in Postgres operator precedence and` alembic` 's tricky corners. I'll convince you that` True OR False AND False` ... is actually` True` , not` False` (if you're playing by Postgres rules).


Note that a working knowledge of SQLAlchemy, Alembic, and check constraints will be useful in understanding the details.


## the goal


Add a check constraint to my` Files` table to validate that certain fields were null only when the file status was set to deleted. I wanted to make sure the effort of validating this data integrity was a backend/database concern rather than spread across its clients.


## the process


Here’s a simplified version of my files table:


```text
class Files(Base):   id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)   status = Column(ENUM(FileStatus), nullable=False)   filename = Column(VARCHAR(256), nullable=True)   storage_key = Column(VARCHAR(256), nullable=True)
```


Here’s what I envisioned my` CheckConstraint` would look like:


```text
__table_args__ = (   CheckConstraint(     f"status = '{FileStatus.DELETED.value.upper()}' OR (filename IS NOT NULL AND storage_key IS NOT NULL)",     name="file_fields_not_null_unless_deleted",   ),  )
```


Simple enough, eh? I used Alembic’s[autogenerate](https://alembic.sqlalchemy.org/en/latest/autogenerate.html) to create the constraint.` autogenerate` doesn't natively support` CheckConstraint` , but I had previously added custom support to the` autogenerate` utility to do so. To check whether a constraint has changed, I naively use the expression` str(old.sqltext).lower() != new\["sqltext"\].lower()` ). All looked good; I ran` alembic upgrade head` and my constraint worked as intended.


Before shipping my PR, though, I wanted to validate that I had a clean[alembic check](https://alembic.sqlalchemy.org/en/latest/api/commands.html#alembic.command.check) to ensure running` alembic revision --autogenerate` wouldn't produce any migrations. Unfortunately,` alembic check` gave me something like below:


```text
FAILED: New upgrade operations detected:   [     ('remove_constraint',     CheckConstraint(<sqlalchemy.sql.elements.TextClause object at 0x11e467c10>,     name='file_fields_not_null_unless_deleted',     table=Table('files', MetaData(), Column('x', Integer(), table=<files>), schema=None))     ),     ('add_constraint',     CheckConstraint(<sqlalchemy.sql.elements.TextClause object at 0x11cfe6410>,     name='file_fields_not_null_unless_deleted',     table=Table('files', MetaData(), Column('x', Integer(), table=<files>), schema=None))    )  ]
```


The key difference here is that` autogenerate` detects a different` TextClause` object: the constraint that` SQLAlchemy` generated was not the same as what it saw in the database.


## Get Ashi Agrawal’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The first thing to go was my f-string, due to my use of the` FileStatus` enum. SQLAlchemy's[CheckConstraint](https://docs.sqlalchemy.org/en/20/core/constraints.html#sqlalchemy.schema.CheckConstraint) takes in the parameter` sqltext` -` A string containing the constraint definition, which will be used verbatim, or a SQL expression construct` . Since a verbatim string wouldn't interpolate my enum value, I had to use a SQL expression construct if I wanted to use the enum.


Unfortunately, the columns I need in the expression aren’t initialized in time for processing` __table_args__` . I took a detour into trying to overload the` __table_cls__` class method (which runs after all of these are initialized) on the` Files` model so that I could manually update the` Table` object. After fiddling with that and failing, I decided to bite the bullet and just use a raw string.


Since I knew I could create a constraint that worked, I was only focused on changing my` CheckConstraint` to match what was in the database. I used the following query to figure out what the database was storing:


```text
SELECT conname, pg_get_constraintdef(oid) AS constraint_def  FROM pg_constraint  WHERE conrelid = 'files'::regclass   AND contype = 'c'   AND conname = 'file_fields_not_null_unless_deleted';
```


Here’s the result:


```text
CHECK (((status = 'DELETED'::filestatus) OR ((filename IS NOT NULL) AND (storage_key IS NOT NULL))))
```


Postgres really loves its parentheses, I guess.


The second thing to go was all of the parentheses in my` CheckConstraint` : I couldn't figure out which ones Postgres was adding so figured I'd start with no parentheses and count which ones it added.


```text
__table_args__ = (   CheckConstraint(     "status = 'DELETED'::filestatus or filename IS NOT NULL and storage_key IS NOT NULL",    name="file_fields_not_null_unless_deleted",   ),  )
```


To my surprise…. this gave me a clean` alembic check` against the existing constraint! When I read this` CheckConstraint` , I assumed that the OR would be evaluated before the AND (classic left to right execution).


Nope. Unbeknownst to me (someone in the habit of using parentheses — and yes, em dashes), the AND operator in SQL has higher precedence than the OR operator, so Postgres naturally translates that into what we want, namely either the status is set to DELETED or both filename and storage_key are populated.


Interesting, but wouldn’t it at least be better to be explicit and add parentheses around the second clause?


```text
__table_args__ = (   CheckConstraint(     "status = 'DELETED'::filestatus or (filename IS NOT NULL and storage_key IS NOT NULL)",     name="file_fields_not_null_unless_deleted",   ),  )
```


Not according to` alembic check` : this fails with the same` TextClause` inconsistency as before. From here, I took my foot off the gas and ceded control of the parentheses to Postgres since it didn't seem to like any that I added.


In the case of` SQLAlchemy` +` CheckConstraint` , simple is better; let the database handle formatting the constraint itself. Also maybe we should have a Postgres version of PEMDAS.


*Originally published at*[https://www.createwithashi.com](https://www.createwithashi.com/2025-06-27-sqlalchemy/) *on June 28, 2025.*
