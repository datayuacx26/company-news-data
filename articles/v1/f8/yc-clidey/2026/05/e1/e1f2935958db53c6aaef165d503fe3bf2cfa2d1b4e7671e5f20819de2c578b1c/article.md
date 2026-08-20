---
schema_version: "1.0.0"
document_id: "e1f2935958db53c6aaef165d503fe3bf2cfa2d1b4e7671e5f20819de2c578b1c"
company_key: "yc-clidey"
company: "Clidey"
source_id: "yc-clidey-rss-03eb479b7595"
canonical_url: "https://blog.clidey.com/whodb-cli-now-live/"
published_at: "2026-05-13T07:00:00+00:00"
first_seen_at: "2026-07-24T22:35:58.719229+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:6373ebc5e79460864b10c4459b2687f3b8fd62f7939e6df99d53112010cbdbb5"
---

# WhoDB CLI now live

Why We Built a Database Client for the Terminal (and Made It an MCP Server)


I live in the terminal. If you’re reading this, you probably do too.


For years, my workflow for checking a database record looked like this: I’m writing code in Neovim or VS Code. I need to verify a schema change or check if a user was actually created. I` Alt-Tab` away from my editor. I wait for a heavy Java-based GUI to load. I click through a tree of servers. I find the table. I run a query. I look at the results. Then I` Alt-Tab` back to my code.


By the time I’m back, I’ve lost my flow. The context switch cost is real.


The alternative was using native CLI tools like` psql` or` mysql` . They are powerful. I respect them. But let’s be honest: they are hostile to exploration. If you` SELECT *` on a wide table, your terminal wraps the text into an unreadable wall of ASCII characters. You can’t scroll horizontally easily. You can’t click a foreign key to jump to another table. You have to write SQL for everything, even just to peek at the last five rows.


We built WhoDB to solve the "bloated GUI" problem with a lightweight web interface. But we realized we hadn't solved the "context switch" problem for terminal die-hards.


So we built the WhoDB CLI.


## The Philosophy: The Database Should Be Where You Are


We built a full-featured database client that happens to live in your terminal. There are plenty of REPLs, and we didn't want to build another one.


The core principle was simple: mouse-free exploration.


When you are debugging, you want speed. You want to hit` j` and` k` to scroll through rows. You want to hit` /` to search. You want to filter data without writing a` WHERE` clause from scratch every time. You want the visual structure of a GUI, grids, borders, colors, without the overhead of a window manager.


We chose Go for the backend because it gives us a single, static binary. No JVM. No Python environment to manage. It starts instantly. For the frontend, we used Bubble Tea. If you haven't used it, it's a framework that treats the terminal like a proper rendering surface. It lets us build responsive, interactive UIs that run over SSH just as well as they run locally.


## The Solution: A TUI That Actually Feels Good


The WhoDB CLI is an interactive Terminal User Interface (TUI).


When you run` whodb-cli` , you aren't greeted with a blank prompt. You get a dashboard. You can navigate your connections. Once you’re in a database, you see a table browser.


Here is what makes it different from standard command-line clients:


It handles wide tables. This was a non-negotiable requirement. If a table has 50 columns,` psql` breaks. WhoDB CLI handles horizontal scrolling natively. You can freeze columns. You can toggle visibility. You see the data as a grid, not a mess.


It speaks Vim. Navigation uses` h` ,` j` ,` k` ,` l` . You don't need to reach for the arrow keys. It feels like an extension of your editor.


It supports (almost) everything. We didn't just build this for Postgres. The CLI supports PostgreSQL, MySQL/MariaDB, SQLite3, MongoDB, Redis, ClickHouse, and ElasticSearch. You use the same interface and the same keybindings regardless of the underlying engine. You don't need to remember that MySQL uses` SHOW TABLES` while Postgres uses` \\dt` . The CLI abstracts that away.


## The Killer Feature: Model Context Protocol (MCP)


This is where things get interesting. We built a tool for AI.


We are seeing a shift in how developers work. We aren't just typing code anymore; we are conversing with LLMs like Claude or using AI-integrated editors like Cursor. But those AIs are usually blind to your local data. They can write SQL, but they can't see your schema unless you copy-paste it into the chat window.


We decided to make the WhoDB CLI a Model Context Protocol (MCP) server.


MCP is an open standard that lets AI assistants connect to external tools. By running` whodb-cli mcp serve` , you turn your local database client into a toolset that Claude or Cursor can use.


Here is what that looks like in practice:


You are in Claude Desktop. You have the WhoDB MCP server running. You ask: *"Why is the user registration query failing?"*


Without you pasting anything, Claude can call` whodb_schemas` to see your database structure, call` whodb_query` to inspect the` users` table constraints, and analyze the error to tell you that you're missing a non-nullable field that was added in a recent migration.


The AI reads your schema.


We built in security modes for this. By default, it runs in "Confirm-writes" mode. If the AI tries to run a` DELETE` or` UPDATE` , the CLI pauses and asks you for explicit permission. We also have a` --read-only` flag if you want to be completely safe, and a` --safe-mode` for demos.


## How It Works Under the Hood


We wanted installation to be trivial. If it takes five minutes to install, you won't use it.


You can grab it via npm:


```text
npm install -g @clidey/whodb-cli
```


Or via Homebrew:


```text
brew install whodb-cli
```


Or just use the install script which detects your OS and grabs the correct Go binary:


```text
curl -fsSL https://raw.githubusercontent.com/clidey/whodb/main/cli/install/install.sh | bash
```


Once installed, you can connect interactively or via flags. For a local Postgres instance:


```text
whodb-cli connect \
--type postgres \
--host localhost \
--user postgres \
--database mydb
```


But we know developers manage dozens of environments. So we added environment profiles. You can define your connections in your` .zshrc` or` .bashrc` :


```text
export WHODB_POSTGRES='[{"alias":"prod","host":"db.prod.com","user":"admin","database":"app"}]'
```


Now, the CLI (and the MCP server) automatically knows about "prod". You don't have to type credentials every time.


## Features for the Power User


We packed a lot into the terminal interface because we use it every day ourselves.


The Scratchpad. Sometimes you do need to write raw SQL. We included a multi-line editor with schema-aware autocomplete. It knows your table names. It knows your columns. It highlights syntax. It feels like a mini-IDE.


Visual Filtering. You can build` WHERE` clauses visually. If you want to find all users where` status` is "active" and` created_at` is this year, you can toggle those filters in the UI. You don't have to remember the specific date format string for the database you are currently in.


Data Export. You can pipe results directly to CSV or Excel.


```text
whodb-cli export --connection prod --table users --format csv --output users.csv
```


This is useful for quick data dumps or sharing reports with non-technical team members.


Mock Data Generation. Need to fill a table with garbage data to test a UI component? The CLI can generate mock data for you based on column types. It’s a work in progress, but it’s already saving us time on frontend work.


## Why This Matters


We believe tools should be fast, single-purpose, and composable.


Traditional database GUIs try to do too much. They include diagram modelers, administrative dashboards, and heavy reporting tools. That’s fine for a DBA who spends 8 hours a day optimizing indexes. But for the application developer, it’s bloat.


We built the WhoDB CLI to be the tool you keep open in a split pane. It uses less than 50MB of RAM. It starts instantly. It respects your terminal color scheme. It lets you stay in the flow.


And because it’s open source, you can see exactly how it works. We built it in the open because we want feedback. We want to know what keybindings you hate, what databases we missed, and how you are using the MCP server with your AI workflows.


## Try It Out


The CLI is live now. It’s Apache 2.0 licensed. It’s free.


If you are tired of waiting for Java apps to load, or if you are tired of squinting at broken ASCII tables in` psql` , give it a shot.


Check out the code, file an issue, or drop us a star on GitHub: https://github.com/clidey/whodb


We’re just getting started with the terminal. Let us know what you think.
