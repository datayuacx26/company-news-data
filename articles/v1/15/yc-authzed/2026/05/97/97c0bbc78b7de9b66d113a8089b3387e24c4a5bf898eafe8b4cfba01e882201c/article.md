---
schema_version: "1.0.0"
document_id: "97c0bbc78b7de9b66d113a8089b3387e24c4a5bf898eafe8b4cfba01e882201c"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/github-permission-system-authzed-cloud"
published_at: "2026-05-26T15:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:8237456c76a6f8cfd3f9e1d4dbf4ad5ab4327625aeb8df4075c9af1b5fc9e68d"
---

# Build and Deploy a GitHub-Style Permission System in AuthZed Cloud

Take a second to reflect on the complexity of Access Permissions on GitHub.


Imagine you're a GitHub org owner. You can delete any repository in the org, edit billing, and merge any pull request without the need to explicitly configure these on each individual repo. And that's because GitHub's permission model is built on layers: tiered roles like *Reader* , *Writer* , and *Admin* intersect with account structures to govern everything from issue tracking to repository settings. Permissions flow naturally from organizational ownership and team hierarchies down to the individual resources.


Modeling such a complex system sounds like an impossibly hard problem to solve, and then there's the engineering challenge of scaling it to the size of GitHub.


That's where SpiceDB + AuthZed Cloud comes in. SpiceDB is an open-source database system for security-critical app permissions, and uses Relationship-Based Access Control (ReBAC) for permission checks. You can deploy SpiceDB on AuthZed Cloud to provision, manage, and scale your authorization on demand.


In this post I'll show how you can model a complex GitHub-style permission system and deploy it to AuthZed Cloud. You can use this as inspiration for similar authorization challenges you might encounter.


---


## Start with what you're protecting


Let's start with the foundationals.


GitHub's model has four main objects: users, organizations, teams, and repositories. The thing most users care about is:


- pushing code
- merging PRs
- opening issues


All these happen on **repositories** . So let's define our first objects - a` user` and a` repository` and work outward from there.


zed


1


2


3


4


5


```text
definition    user    {
}


definition    repository    {
}


```


zed


1


2


3


4


5


```text
definition    user    {
}


definition    repository    {
}


```


## Roles are nouns; permissions are verbs


A` relation` defines how two objects (or an object and subject) can relate to one another. For example, a` reader` on a document, or a` member` of a group. A permission defines a *computed* set of subjects that have a permission of some kind on the object. For example, is a user within the set of users that can` edit` a document.


Going back to our example, GitHub gives repositories five access levels:


1. Reader
2. Triager
3. Writer
4. Maintainer
5. Admin


Here's how these would map to a[SpiceDB schema](https://authzed.com/docs/spicedb/concepts/schema) with relations and permissions:


zed


1


2


3


4


5


6


7


8


9


10


```text
definition    repository    {
relation    organization  :  organization


relation    reader  :  user     |   team#member
relation    triager  :  user     |   team#member
relation    writer  :  user     |   team#member
relation    maintainer  :  user     |   team#member
relation    admin  :  user     |   team#member
...
}


```


zed


1


2


3


4


5


6


7


8


9


10


```text
definition    repository    {
relation    organization  :  organization


relation    reader  :  user     |   team#member
relation    triager  :  user     |   team#member
relation    writer  :  user     |   team#member
relation    maintainer  :  user     |   team#member
relation    admin  :  user     |   team#member
...
}


```


The` team#member` syntax is worth understanding. Rather than granting a permission to a team as a whole, we're granting it to the *members* of that team at query time, and not write time. When someone joins or leaves the team, their repository access updates automatically.


Now, GitHub doesn't check whether you're a "reader" when you click Clone. It checks whether you have *clone permission* . The permissions layer is where roles combine:


zed


1


2


3


4


5


6


7


8


9


10


```text
permission   clone  =   reader  +   triager  +   push
permission   push  =   writer  +   maintainer  +   admin  +   organization -  >owner


permission   read  =   reader  +   triager  +   writer  +   maintainer  +   admin  +   organization -  >owner
permission   delete  =   admin  +   organization -  >owner


permission   create_issue  =   read
permission   close_issue  =   triager  +   writer  +   maintainer  +   admin  +   organization -  >owner


permission   merge_pull_request  =   maintainer  +   organization -  >owner


```


zed


1


2


3


4


5


6


7


8


9


10


```text
permission   clone  =   reader  +   triager  +   push


permission   delete  =   admin  +   organization -  >owner


permission   create_issue  =   read


permission   merge_pull_request  =   maintainer  +   organization -  >owner


```


Notice that` clone` is defined in terms of` push` , not the other way around. Anyone who can push can obviously clone, and if you later add a new role to` push` , they automatically get` clone` too. Change it once; the rest follows.


The other thing worth understanding:` organization->owner` appearing on repository permissions is the arrow operator. SpiceDB schema supports[Operations](https://authzed.com/docs/spicedb/concepts/schema#operations) such as **union** , **intersection** , **exclusion** and **arrows** .


The '->' operation indicates 'find the organization this repository belongs to, then check who has the` owner` relation on that organization'. Org owners inherit repository permissions without ever being explicitly granted them on each repo. That's exactly how GitHub behaves, and thanks to the flexibility of the SpiceDB scheme you can get there in one line.


## Granularity is a bet on the future


Let's take a look at GitHub's settings page. On first glance this page has dozens of options which may sound daunting if you're modelling each one as a separate permission.


But you can actually achieve it with just two permissions:


zed


1


2


```text
permission   manage_setting  =   maintainer  +   admin  +   organization -  >owner
permission   manage_sensitive_setting  =   admin  +   organization -  >owner


```


zed


1


2


```text
permission   manage_sensitive_setting  =   admin  +   organization -  >owner


```


The logic here is: we don't need more permissions than users with different access needs. If everyone who can configure webhooks can also configure branch protection, one permission covers both. Add granularity when you actually have a use case for splitting it.


The exception is when you *know* the split is coming in your product roadmap. If you're building a SaaS where billing will eventually be a distinct surface, you can model` manage_billing` now. Retrofitting fine-grained permissions after shipping means touching both schema and application code.[Here's a nice read](https://authzed.com/blog/fine-grained-access-control) on if your fine-grain permissions are too fine?


## Org ownership bleeds into repos


In GitHub, an` organization` is an object which a` user` is a part of (and hence has a relation to). Let's define an organization object along with the relevant relations and permissions:


zed


1


2


3


4


5


6


7


8


9


10


11


```text
definition    organization    {
relation    own  :  user
relation    member  :  user
relation    billing_manager  :  user
relation    team_maintainer  :  user


permission   owner  =   own
permission   create_repository  =   owner  +   member
permission   manage_billing  =   owner  +   billing_manager
permission   change_team_name  =   team_maintainer  +   owner
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


```text
definition    organization    {
relation    own  :  user
relation    member  :  user
relation    billing_manager  :  user
relation    team_maintainer  :  user


permission   owner  =   own
permission   create_repository  =   owner  +   member
permission   manage_billing  =   owner  +   billing_manager
permission   change_team_name  =   team_maintainer  +   owner
}


```


` billing_manager` is a first-class role as regular members can't touch billing.` create_repository` is on the *organization* , not individual repos, because a repository doesn't exist yet when you're creating it. And` change_team_name` is an org-level permission, even though teams are nested objects. That last one will make more sense in a second.


## Teams are the interesting part


The next object in our schema is a` team` . Teams add a third layer between users and repositories. The definition is:


zed


1


2


3


4


5


6


7


8


```text
definition    team    {
relation    parent  :  organization     |   team
relation    maintainer  :  user
relation    direct_member  :  user


permission   member  =   maintainer  +   direct_member
permission   change_team_name  =   maintainer  +   parent -  >change_team_name
}


```


zed


1


2


3


4


5


6


7


8


```text
definition    team    {
relation    parent  :  organization     |   team
relation    maintainer  :  user
relation    direct_member  :  user


permission   member  =   maintainer  +   direct_member
permission   change_team_name  =   maintainer  +   parent -  >change_team_name
}


```


The` parent->change_team_name` traversal is the payoff.


A` team_maintainer` at the org level can rename any team, and this permission flows down through the hierarchy automatically. You represent a cross-object permission policy in one line without writing any application code. Teams can also be nested (` parent: organization | team` ). Sub-teams are a thing in GitHub, and the schema handles them without any special cases.


Now that our schema is complete, let's see how we can write relationships to reflect the state of our system.


---


## Write relationships on every state change


If the schema defines the structure of your system, relationships are the data. Whenever the underlying state of your system changes your app should CRUD a relationship.


A few examples:


bash


1


2


3


4


5


6


7


8


9


10


11


12


```text
# User joins the org
organization:authzed#member@user:alice


# User gets write access on a specific repo
repository:spicedb#writer@user:bob


# A support team gets maintainer access on a repo
# (note: team#member, not team — SpiceDB resolves the membership at check time)
repository:spicedb#maintainer@team:support#member


# A repo belongs to an org
repository:spicedb#organization@organization:authzed


```


bash


1


2


3


4


5


6


7


8


9


10


11


12


```text
# User joins the org
organization:authzed#member@user:alice


# User gets write access on a specific repo
repository:spicedb#writer@user:bob


# A support team gets maintainer access on a repo
repository:spicedb#maintainer@team:support#member


# A repo belongs to an org
repository:spicedb#organization@organization:authzed


```


That last write is easy to forget, but it's what makes` organization->owner` on repository permissions actually work. Without it, the repository has no org to traverse into. Checks on org-level permissions will silently fail if there's a floating repository and a floating organization that is not connected.


The general rule: write a relationship tuple whenever a user gets a new role, joins a new team, when a team gets access to a repo, or when a repo is created under an org. Delete the tuple when access is revoked. The permission checks will handle the rest.


Here's the Python code for writing a relationship when` user:bob` gets` write` permissions on` repository:spicedb`


python


1


2


3


4


5


6


7


8


9


10


11


12


13


```text
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_CREATE,
relationship=Relationship(
resource=ObjectReference(object_type= "repository"  , object_id= "spicedb"  ),
relation= "writer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "bob"  ,
)
),
),
),


```


python


1


2


3


4


5


6


7


8


9


10


11


12


13


```text
RelationshipUpdate(
operation=RelationshipUpdate.Operation.OPERATION_CREATE,
relationship=Relationship(
resource=ObjectReference(object_type= "repository"  , object_id= "spicedb"  ),
relation= "writer"  ,
subject=SubjectReference(
object  =ObjectReference(
object_type= "user"  ,
object_id= "bob"  ,
)
),
),
),


```


---


## Taking it live on AuthZed Cloud


It's time to deploy our Permissions System, and we can use AuthZed Cloud to do so.


Sign into[AuthZed Cloud](https://app.authzed.cloud/) , create a Permissions System, and pick Development to start. Once it's provisioned, create a **Service Account** (call it something like` github-model` ) and generate a token for it.


Create a **Role** with the permissions your application needs:


1


2


3


4


5


```text
ReadSchema
WriteSchema
WriteRelationships
ReadRelationships
CheckPermission


```


1


2


3


4


5


```text
ReadSchema
WriteSchema
WriteRelationships
ReadRelationships
CheckPermission


```


Bind the role to the service account via a **Policy** , and you're ready.


Using the` zed` CLI to test it out:


shell


1


2


3


4


5


6


7


8


9


```text
#   Write the schema
zed schema write schema.zed


#   Write some  test   relationships
zed relationship create repository:spicedb writer user:alice
zed relationship create repository:spicedb organization organization:authzed


#   Check a permission
zed permission check repository:spicedb push user:alice


```


shell


1


2


3


4


5


6


7


8


9


```text
#   Write the schema
zed schema write schema.zed


#   Write some  test   relationships
zed relationship create repository:spicedb writer user:alice
zed relationship create repository:spicedb organization organization:authzed


#   Check a permission
zed permission check repository:spicedb push user:alice


```


That last command returns` true` or` false` . That's what your application calls before allowing a git push, a PR merge, or a settings change.


In production, you can swap the` zed` CLI calls for a SpiceDB client in your language of choice. Authzed ships[clients for Go, Python, Java, and Node](https://github.com/authzed/awesome-spicedb#official-libraries) . You can also choose the scale and regions in which you want your Permissions System to run in.


Here's the entire schema discussed above in the[AuthZed Playground](https://play.authzed.com/s/qDtUwr_WjEkl) .


---


Modeling authorization systems such as this one forces real decisions: how org-level roles flow into repos, when a "team" is the right primitive instead of an individual user, where to draw the line on granularity. With SpiceDB, it's easy to answer those questions in schema, not scattered across custom conditional code embedded in your application.


Check out the[AuthZed Cloud Starter program](https://authzed.com/cloud/starter-program) to receive $700 in AuthZed Cloud credits and see the value of scalable authorization infrastructure.


On this page


- Start with what you're protecting
- Roles are nouns; permissions are verbs
- Granularity is a bet on the future
- Org ownership bleeds into repos
- Teams are the interesting part
- Write relationships on every state change
- Taking it live on AuthZed Cloud


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)


[Engineering Interact with SpiceDB Seamlessly from within PostgreSQL using the SpiceDB Foreign Data Wrapper Introducing the SpiceDB Foreign Data Wrapper (FDW) for PostgreSQL — a new experimental way to bring real-time authorization context from SpiceDB into Postgres queries, without duplicating data or embedding authorization logic where it doesn't belong. Feb 12, 2026 · 5 min](https://authzed.com/blog/spicedb-postgres-fdw)[Engineering Interact with SpiceDB Seamlessly from within PostgreSQL using the SpiceDB Foreign Data Wrapper Introducing the SpiceDB Foreign Data Wrapper (FDW) for PostgreSQL — a new experimental way to bring real-time authorization context from SpiceDB into Postgres queries, without duplicating data or embedding authorization logic where it doesn't belong. Joey Schorr · Feb 12, 2026 · 5 min](https://authzed.com/blog/spicedb-postgres-fdw)
