---
schema_version: "1.0.0"
document_id: "5eff722d3ba41504c71e816a22dce8e2759829c6ab985ce789ba25e97d51772b"
company_key: "yc-clidey"
company: "Clidey"
source_id: "yc-clidey-rss-03eb479b7595"
canonical_url: "https://blog.clidey.com/whodb-mock-data-now-respects-foreign-keys/"
published_at: "2026-05-15T07:00:00+00:00"
first_seen_at: "2026-07-24T22:35:58.719229+00:00"
fetched_at: "2026-07-28T21:15:16.761066+00:00"
content_hash: "sha256:e242fb73cdd18a484243b56d20036c673a0073c076de4965d283c95fe2b2f31b"
---

# WhoDB Mock Data Now Respects Foreign Keys

## The Last Seed Script You'll Ever Write


You know the feeling. You just pulled the latest changes from the repo. You run` docker compose up` . The containers spin up green. You open your localhost port.


And the app is empty.


It's a ghost town. No users. No products. No orders. Just a pristine, empty UI staring back at you.


So you do what we all do. You open your database tool to insert some test data. You write a quick SQL statement to add a user.` INSERT INTO users (name) VALUES ('test');` .


Error.` Column 'email' cannot be null` .


Fine.` INSERT INTO users (name, email) VALUES ('test', 'test@test.com');` .


Success. Now you need an order.` INSERT INTO orders (user_id, total) VALUES (1, 100);` .


Error.` Foreign key constraint failed` .


Right. You forgot that the` users` table uses UUIDs, not integers. You have to go back, query the user you just made, copy the UUID string, paste it into the insert statement for the order.


Ten minutes later, you have three rows of data. They are named "test", "asdf", and "bob". The dates are all identical. The prices are all integers. It looks nothing like production.


This is the lie of the local environment. We build complex applications designed to handle millions of rows, but we build them against three rows of garbage data.


## The Lie of the Seed Script


We've all tried to fix this. We write seed scripts. We maintain a` seeds.sql` file in the repo.


But seed scripts rot.


Schema changes happen fast. A developer adds a` NOT NULL` column to the` products` table in a migration but forgets to update the seed file. The next person who pulls the code gets a crash on startup.


Or maybe you use a heavy ORM seeder. It works, but it's slow. And you have to write code to generate data. You have to define factories. You have to maintain them. It's code that doesn't ship to production, but you still have to debug it.


I noticed this friction while working on the core engine of WhoDB. We built WhoDB to be the fastest, lightest database manager out there, under 50MB, written in Go, instant startup. We stripped away the JVM bloat of DBeaver. We got the resource usage down to under 100MB of RAM.


But even with a fast tool, the *data* problem remained.


I looked at our "Mock Data" feature. It was basic. It could generate random strings and numbers. But it was dumb. It didn't know that a column named` email` should look like an email. And it definitely didn't respect the relationships between your tables.


If you tried to mock data for an` orders` table, it would generate random IDs for the` user_id` column. The database would reject every single row because those IDs didn't exist in the` users` table.


The tool was fast. The workflow was broken.


## The 'What If' Moment


The spark came from looking at our own schema visualization.


WhoDB already knows your database topology. When you click the "Graph" tab, we draw an interactive map of your tables. We know that` orders.user_id` points to` users.id` . We know the foreign keys. We know the data types.


So I asked: **What if the database tool just did the math for you?**


What if you didn't have to tell the generator "this is an email"? What if it just looked at the column name` customer_email` and figured it out?


And harder yet: What if you didn't have to look up foreign keys? What if the tool looked at the relationship, queried the parent table, grabbed a valid ID, and used that?


If we could do that, you wouldn't need seed scripts. You wouldn't need factories. You would just right-click a table, say "Give me 1,000 rows," and it would just work.


## How the Magic Works


We decided to overhaul the mock data engine. This wasn't just a UI tweak. We had to make the generation logic context-aware and relationally consistent.


We had massive help here from a community contributor, @majiayu000. This is the beauty of building in the open. They saw the gap and helped us close it.


Here is what happens under the hood now when you ask WhoDB to generate data.


### 1. Contextual Column Analysis


First, we analyze the column names. We don't just look at the type (` VARCHAR` ). We look at the intent.


If the column is named` email` ,` user_email` , or` contact_email` , we don't generate "aB3$fG". We generate "alice.smith@example.com".


If the column is` price` ,` cost` , or` balance` , we generate decimal values that look like money.


If it's` created_at` , we generate timestamps that are recent, not from the year 1970 or the year 3000.


This seems small, but it changes how you perceive the data. When you look at a grid of users, and they have real names and real emails, your brain processes the application state differently. You spot UI bugs faster because the data looks like real data.


### 2. The Foreign Key Handshake


This was the hard part.


Previously, generating data for a child table (like` order_items` ) was impossible without manual intervention. You needed valid` order_id` s and valid` product_id` s.


Now, WhoDB respects the constraint.


When you ask for 500 rows in` order_items` , the engine pauses. It sees the foreign key on` order_id` . It executes a lightweight query against your` orders` table to fetch a list of existing, valid IDs.


It does the same for` products` .


Then, as it generates the mock rows, it randomly selects IDs from those valid lists.


The result? You get 500 rows of order items that actually link to real orders and real products. No constraint violations. No errors. Just green success notifications.


You can populate a complex schema in minutes.


1. Generate 100 users.
2. Generate 50 products.
3. Generate 500 orders (WhoDB picks valid users).
4. Generate 2,000 order items (WhoDB picks valid orders and products).


You just seeded a full e-commerce database state without writing a single line of SQL or Python.


## This is Why We're Building


We built this to solve a bigger problem than just saving five minutes of writing` INSERT` statements. We built it because we believe local development tools are stuck in the past.


For a long time, the industry standard has been "heavy." Heavy Java apps that eat 4GB of RAM just to show you a table. Heavy ORMs that require complex setups just to seed data.


Our philosophy with WhoDB is different. We use Go because it compiles down to a small binary. We use React and Wails because they are fast. We want the tool to be invisible.


You shouldn't have to "manage" your database manager. You shouldn't have to fight your tools to get a working environment.


When you spin up a fresh Docker container, you want to get to work. You want to test your feature. You don't want to spend your morning debugging a seed script because someone changed a column name three commits ago.


This update turns the "empty database problem" into a solved problem. It respects your schema, it respects your constraints, and it respects your time.


We are building WhoDB in the open because we want to solve these problems for everyone, regardless of whether you use Postgres, MySQL, SQLite, or MongoDB.


If you want to try it out, you don't need to install anything heavy. It's just a Docker container.


```text
docker run -p 8080:8080 clidey/whodb
```


Or check out the code on GitHub. We're always looking for more people to help us kill the friction of database development.


https://github.com/clidey/whodb
