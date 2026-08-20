---
schema_version: "1.0.0"
document_id: "9b5e9235acc3bb8795bd864dbe37cfbbaa69d29e6c60a07dc36118944918c253"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-cli-v1-and-admin-api-beta"
published_at: "2022-08-15T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:1262bd6b12768db97163b5e7318031fb19e18189981668e92be63ff28bac9094"
---

# Supabase CLI v1 and Management API Beta

Today we are moving the Supabase CLI v1 out of beta. The Supabase CLI is capable of managing database migrations and generating TypeScript types. Follow these[install instructions](https://supabase.com/docs/guides/cli) to get started.


In addition, we are releasing a Management API (in beta). The Management API is a REST API that allows you to manage organizations, projects, Edge Functions, and more. You can read the[API docs](https://supabase.com/docs/reference/api) or interact with the Management API from the Supabase CLI v1.


(Note: The Management API was previously called the Admin API.)


*Watch the CLI team announcing the new features.*


## Manage organizations, projects, Edge Functions, and more#


If you've used[Supabase Edge Functions](https://supabase.com/blog/supabase-edge-functions#quickstart) , you've used the Supabase CLI. We're extending it to let you manage organizations and projects.


`
_10


supabase login


_10


supabase orgs list


_10


supabase projects create my-project --org-id cool-green-pqdr0qc --db-password ******** --region us-east-1


`


Once your new Supabase project is created, use the CLI to link it locally to begin development.


`
_10


supabase link --project-ref <project-id>


`


View the[Supabase CLI docs](https://supabase.com/docs/reference/cli) for the full list of available commands.


## Schema migrations#


You asked for more database migration support, and we listened. We've made improvements to manually-written migrations and auto generated schema diffs.


### Schema diff-ing#


Previously we supported schema diff-ing using PgAdmin. We found that the tool did not account for default privileges when generating role grants. This leads to verbose statements when diff-ing complex schema changes.


To improve the usability of` db diff` command, we are introducing another tool,[migra](https://github.com/djrobstep/migra) , via the` --use-migra` experimental flag. We found that` migra` runs faster and produces more concise DDL statements. While the generated scripts are[not perfect](https://supabase.com/blog/supabase-cli#migrations) , we hope this tool helps you iterate quicker on your migration scripts. Over time (and with your feedback) we hope to improve schema-diffing to cover all edge-cases.


`
_10


$ supabase db diff --use-migra --file file_name


_10


# Creates a DDL script: supabase/migrations/<datetime_string>_file_name.sql


`


Running the above command diffs the` public` schema of your local development database against a fresh shadow database. You may specify other schema by passing in the` --schema` flag multiple times or as a comma separate list. More details in our[migration guide](https://supabase.com/docs/guides/cli/cicd-workflow#creating-a-new-migration) .


In the long-term we hope to consolidate on a single diff-ing tool which is perfect, but diff-ing is hard so we would need your help and feedback to improve tooling.


### Manual migrations#


You can test manual migrations locally without data loss using local branching. Run the following commands to clone your local database to a new branch.


`
_10


supabase db branch new my_branch


_10


supabase db branch switch my_branch


`


Now you can run any DDL statements from Studio UI's SQL Editor. To undo the changes, simply switch back and delete the new branch.


`
_10


supabase db branch switch main


_10


supabase db branch delete my_branch


`


## CI / CD#


Automating migrations and tests on your CI / CD pipeline gives developers more confidence that each PR contains a working migration script. CLI v1 focuses on both local and[GitHub Actions](https://github.com/supabase/setup-cli) support for the following workflows.


Test all migrations on a fresh local database:


`
_10


supabase init


_10


supabase start


`


Release schema changes to staging and production:


`
_10


$ supabase link --project-ref $PROJECT_ID


_10


$ supabase db push


`


We created an[example project](https://github.com/supabase/supabase-action-example) showing how to set up GitHub Actions to test and migrate with Supabase CLI v1.


## Type generation#


You can now generate TypeScript types using the CLI:


`
_10


# in a project set up with the CLI:


_10


supabase gen types typescript --local


_10


supabase gen types typescript --db-url $SUPABASE_DB_URL


`


Of course, the types aren't very useful on its own, you need some way to consume it. For that, stay tuned for tomorrow! 😉


## Management API#


The CLI is the first consumer of our new Management API. Over the next few weeks, we'll be adding the endpoints needed to programmatically manage your Supabase projects and organizations. This is ideal for CI/CD workflows and spinning up test environments.


Here's an example of deploying a new project from the command line (generate your access token from the[Supabase Dashboard](https://supabase.com/dashboard/account/tokens) ).


`
_10


curl 'https://api.supabase.com/v1/projects' \\


_10


-H 'Authorization: Bearer <\[your-access-token\](https://supabase.com/dashboard/account/tokens)>' \\


_10


-H "Content-Type: application/json" \\


_10


-d '{"name": "my-project", "organization_id": "cool-green-pqdr0qc", "region": "us-east-1", "plan": "free", "db_pass": "********"}'


`


The response JSON will match the example below:


`
_10


{


_10


"id": "abcdefghijklmnopqrst",


_10


"organization_id": "cool-green-pqdr0qc",


_10


"name": "hello",


_10


"region": "us-east-1",


_10


"created_at": "2022-08-12T17:37:11.88819Z"


_10


}


`


Check out the[API docs](https://supabase.com/docs/reference/api) to browse all the functionality added so far.


The new API also opens the door to a whole new suite of integrations, including Zapier, Terraform, Pulumi etc. We're looking forward to seeing how the dev community interacts with these new public endpoints. Try it out and let us know which functionality you'd like to see next.


## More Launch Week 5#


- [Launch Week Page](https://supabase.com/launch-week)
- [Launch Week 5 Hackathon](https://supabase.com/blog/launch-week-5-hackathon)
- [Supabase Series B](https://supabase.com/blog/supabase-series-b)
- [Open Source at Supabase - Founders Fireside Chat](https://www.youtube.com/watch?v=4t_63HT3rZY)
- [Day 2 - supabase-js v2 Release Candidate](https://supabase.com/blog/supabase-js-v2)
- [Youtube Video - supabase-js v2 Release Candidate](https://www.youtube.com/watch?v=iqZlPtl_b-I)
- [Day 3 - Supabase is SOC2 compliant](https://supabase.com/blog/supabase-soc2)
- [Youtube video - Security Day](https://www.youtube.com/watch?v=6bGQotxisoY)
