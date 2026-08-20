---
schema_version: "1.0.0"
document_id: "84d2fd20408d1d9670d9d4da779f6a03da1378eeecb2e570db4f536b3aa57a01"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/role-explosion-rbac-nested-access-lists/"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-17T14:10:22.271050+00:00"
fetched_at: "2026-08-17T14:10:24.787467+00:00"
content_hash: "sha256:2319fbca74d14cb7775febbf7f02c746e515f4e2aecd6731fdea6e90c2c253dc"
---

# How to Prevent RBAC Role Explosion with Nested Access Lists

**Read this article to learn:**


- What role explosion is in RBAC, and how it happens.
- How access lists work and how they can prevent role explosion.
- How nested access lists work with Teleport.


In[RBAC (Role-based Access Control)](https://goteleport.com/learn/what-is-rbac/) , a role is a defined object with explicit permissions attached to it. Because roles are designed to be fixed, changing what a particular role can do (for example, in a one-off situation where other permissions are needed for the role) requires editing the role itself.


However, repeatedly editing roles makes them less flexible and re-usable, and ultimately complicates access strategies as organizations scale. Instead of repeatedly editing roles, nesting access lists can apply multiple permissions to roles, while allowing the roles to remain fixed.


In this post, discover how to use nested access lists to prevent role explosion, and to create hierarchical RBAC with inherited permissions across groups, teams, and resources.


## Role explosion: How it happens


A known limitation of RBAC is **role explosion** , where an organization’s role count begins to outpace the number of actual users. In addition to making privilege management more difficult, role explosion can slow processing downstream; every user login must be checked against whatever roles apply to that user.


Role explosion happens for several reasons. Different development teams often need their own mix of tools, environments, and permission levels, and those needs can change as projects evolve. This means that a role created for members of one team may not work for another team. Administrators under pressure to move quickly end up creating near-duplicate roles or granting broader permissions to a team than they actually need; it’s a quick fix to keep operations moving.


When this behavior is multiplied across many teams, the number of roles can grow more quickly than they can be tracked. Eventually, administrators no longer have a good overview of which roles have which permissions and why.


Further exceptions can arise when permissions are assigned by team or role group rather than to individual users. For example, support engineers or application teams may share a broad role group, but only a subset of them may need access to specific customer environments, APIs, or project resources. These situations are usually handled by creating a new role rather than modifying an existing one, because editing a shared role risks changing access for everyone who holds it.


## How access lists work


[Access lists](https://goteleport.com/docs/reference/access-controls/access-lists/) provide a way to keep the number of roles at an organization small, even as teams and environments grow. Nesting those access lists lets you assign permissions through inheritance across an entire org chart without manually adding individual users to multiple lists.


An access list maps a user to a set of roles. When a new user is provisioned, they automatically belong to an access list (or lists). This greatly simplifies role-based access verification, and takes much less time than analyzing role logic one role at a time.


There are two ways to create access lists:


1. An admin can create an access list manually for a specific, narrow case, like a small marketing group that needs access to three internal tools and nothing else.
2. An access list can be generated automatically by an identity provider, like Okta, with one access list for each group. The members of that list will match the users in that Okta group.


Because the list itself is the entity that changes, rather than the role, each role can continue to be generic and reusable. Therefore, new exceptions don’t require the creation of new roles; thus preventing role explosion from taking over.


## Nested access lists and how they work


But access lists can go much further. In Teleport, access lists can contain other access lists “nested” as members or owners. Users in a[nested access list](https://goteleport.com/docs/identity-governance/access-lists/nested-access-lists/#how-it-works) inherit access from the parent list (as long as they meet both lists' membership requirements).


For example, an "all employees" list can be nested as a member of a separate wiki-access list. Instead of a department-by-department role for wiki access, one role tied to the wiki access list now covers everyone, since every employee is already a member of the nested "all employees" list.


Inheritance of permissions with nested access lists happens in two ways:


1. When a list is nested as a member of another list, its members inherit the parent's member rights (the roles and traits that come with membership).
2. When a list is nested as an owner, its members inherit the parent's owner rights and can manage the parent list itself.


A list nested inside of another list can itself have its own nested lists as members, and so on. This lets users apply permissions that can cascade down through multiple layers at once.


## Applying access lists team-by-team


Consider a large enterprise with dozens of internal development teams, each controlling access to its own APIs, code, and services. Bart and Lisa are both engineers at this organization, but on different teams. However, Bart now needs to reach a service that Lisa’s team owns.


Without access lists, Bart would need to ask Lisa directly, and then Lisa (or whoever on her team holds admin rights) would need to manually add Bart to a role or an exception list. If this scenario repeats frequently across dozens of teams, every cross-team request becomes a person-to-person favor that is only tracked by the person who remembers doing it. At growing organizations, these exceptions can accumulate quickly.


Now consider an enterprise running 200 Kubernetes clusters, each with 15 to 20 namespaces, with each namespace holding 15 to 20 applications. Handling access manually, team by team, isn’t viable at that scale.


Using access lists, each team can own one access list for its API, tied to one role. Adding Bart to Lisa's team's list grants him access without the need for a manual role edit. Nesting that list under a broader group, for example a company-wide "backend engineering" list, means anyone who joins backend engineering inherits access to Lisa's team’s services automatically. The same nesting principle enables groups of teams to pick up access to each other's services, without admins having to manually provision each connection.


## Learn more about nested access lists


Nested access lists let a small set of lists carry permissions across an entire organization, without the need to create new roles or edit existing roles.


Watch our video[Hierarchical Access Control Made Easy](https://youtu.be/rPB3gup0ckI?si=T3jSjYWfOA5ZqrP8) to walk through the concepts discussed in this blog, including how permissions are passed down through parent and child lists.
