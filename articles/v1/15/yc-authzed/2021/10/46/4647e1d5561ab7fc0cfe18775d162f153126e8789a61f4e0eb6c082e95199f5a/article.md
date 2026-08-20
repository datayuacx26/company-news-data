---
schema_version: "1.0.0"
document_id: "4647e1d5561ab7fc0cfe18775d162f153126e8789a61f4e0eb6c082e95199f5a"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/user-defined-roles"
published_at: "2021-10-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:96785ad219887bfe49289a4bf192cd1ab6bd160244aaa7fca2383bdddc52df46"
---

# User Defined Roles

We’ve all interacted with applications and products that have a role editor that’s spiritually similar to the one above. As an administrator of the app, you get to define the names of the roles, and the capabilities that they imply. This allows you to set up the roles to match the roles at your company and federate out permissions to match. Often these applications will come with a default set of roles and permissions that match a pre-canned workflow, but allow you to change the workflow to fit your organization.


[JIRA](https://www.atlassian.com/software/jira) , for example, comes with a built-in role called` admin` , and lets you define roles for the various people who will be contributing to this particular productivity app. The example above was inspired by a far simpler version of an issue tracking tool which only has issues and comments on those issues.


In today’s post we will work through how to model this application in Authzed’s schema language, how to integrate it with your application, and how to direct that integration’s reads and writes to both an instance of our open-source implementation[SpiceDB](https://github.com/authzed/spicedb) or to our hosted service[Authzed](https://authzed.com/) .


Let’s get started by modeling the schema in the[Authzed Playground](https://play.authzed.com/) !


## Modeling the Schema


### Modeling the User


The first thing we usually do when modeling any application in Authzed is create an object definition for users themselves:


zed


1


```text
definition    user    {  }


```


zed


1


```text
definition    user    {  }


```


This allows us to refer to users as[subjects](https://docs.authzed.com/concepts/terminology#subject) in test[relationships](https://docs.authzed.com/concepts/terminology#relationship) with the following syntax:


zed


1


2


```text
user  : claudia
user  : really   -  long -  user -  id -  maybe -  a -  uuid


```


zed


1


2


```text
user  : claudia
user  : really   -  long -  user -  id -  maybe -  a -  uuid


```


### Modeling Capabilities


Before we can start assigning capabilities to roles, we must first enumerate them! In Authzed we think about capabilities in terms of[permissions](https://docs.authzed.com/concepts/terminology#permission) . As a reminder, permissions are the external API of your permissions system, and they are the integration point with your application at enforcement points. For example, before we allow someone to create an issue within a project we probably want to check that the user has permission.


In pseudocode:


python


1


2


3


```text
if   authzed.check(resource= "project:oursoftware"  , permission= "create_issue"  , subject= "user:claudia"  ):
# let "claudia" create the issue on the "oursoftware" project
create_issue()


```


python


1


2


3


```text
# let "claudia" create the issue on the "oursoftware" project
create_issue()


```


For our example, the schema would be the following:


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


12


13


14


15


```text
definition    user    {  }


definition    project    {
permission   create_issue  =   todo
}


definition    issue    {
permission   assign  =   todo
permission   resolve  =   todo
permission   create_comment  =   todo
}


definition    comment    {
permission   delete  =   todo
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


12


13


14


15


```text
definition    user    {  }


definition    project    {
permission   create_issue  =   todo
}


definition    issue    {
permission   assign  =   todo
permission   resolve  =   todo
permission   create_comment  =   todo
}


definition    comment    {
permission   delete  =   todo
}


```


For now I have used the placeholder` todo` for the actual computation of those permissions. You can see from our original example that these permissions closely follow the capabilities defined at the top of our control panel.


### Modeling Roles


Since this is an article about user defined roles, we probably want to also model out the roles themselves. In order to keep permissions flexible in Authzed, we separate out permissions from relations. Relations define how objects can relate to other objects. Permissions are how we interpret relations to make access control decisions. Our` role` object will relate to users that have the role:` member` and to a` project` for which the role is defined.


zed


1


2


3


4


```text
definition    role    {
relation    project  :  project
relation    member  :  user
}


```


zed


1


2


3


4


```text
definition    role    {
relation    project  :  project
relation    member  :  user
}


```


That’s it! Now that we have roles, we can start filling in the relations and definitions on our` project` ,` issue` , and` comment` objects.


### Adding Relations to Bind Roles to Capabilities


As mentioned earlier, we separate out permissions and relations in Authzed in order to keep the permissions computation flexible. The first thing we will do is add one relation for each grantable capability, and set the allowable type to users who are the members of a role:


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
definition    project    {
relation    issue_creator  :  role  # member
relation    issue_assigner  :  role  # member
relation    any_issue_resolver  :  role  # member
relation    assigned_issue_resolver  :  role  # member
relation    comment_creator  :  role  # member
relation    comment_deleter  :  role  # member


permission   create_issue  =   todo
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
definition    project    {
relation    issue_creator  :  role  # member
relation    issue_assigner  :  role  # member
relation    any_issue_resolver  :  role  # member
relation    assigned_issue_resolver  :  role  # member
relation    comment_creator  :  role  # member
relation    comment_deleter  :  role  # member


permission   create_issue  =   todo
}


```


This will allow us to grant a capability to a role with the following pseudocode:


python


1


2


```text
# Grant the "issue_creator" capability to those who have been granted the "project_manager" role
authzed.write(resource= "project:oursoftware"  , relation= "issue_creator"  , subject= "role:project_manager#member"  )


```


python


1


2


```text


```


Now that we have the proper relations in place, we can compute all of the permissions we left blank earlier!


### Computing Permissions from Relations


Permissions such as` create_issue` which are both granted and evaluated at the organization level, are very easy to compute:


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
definition    project    {
relation    issue_creator  :  role  # member
relation    issue_assigner  :  role  # member
relation    any_issue_resolver  :  role  # member
relation    assigned_issue_resolver  :  role  # member
relation    comment_creator  :  role  # member
relation    comment_deleter  :  role  # member


permission   create_issue  =   issue_creator
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
definition    project    {
relation    issue_creator  :  role  # member
relation    issue_assigner  :  role  # member
relation    any_issue_resolver  :  role  # member
relation    assigned_issue_resolver  :  role  # member
relation    comment_creator  :  role  # member
relation    comment_deleter  :  role  # member


permission   create_issue  =   issue_creator
}


```


This permission, in short, says: "those who are members of any role which has an` issue_creator` relationship on this project, shall have the` create_issue` permission."


The rest of our permissions are evaluated on` issue` and` comment` , but the relationships are on the parent project object. How can we calculate these permissions?


Authzed schema provides an[arrow](https://docs.authzed.com/reference/schema-lang#--arrow)` ->` operator which allows you to traverse through relationships matching a relation on an object, and then evaluate a relation on *that* object. For example,` project->issue_assigner` , says "follow any relationships with the` project` relation, and compute the` issue_assigner` relation on those objects. From here it is easy to see how to compute indirect permissions. We also need to add a relationship between the issue and project:


zed


1


2


3


4


5


6


7


```text
definition    issue    {
relation    project  :  project


permission   assign  =   project -  >issue_assigner
permission   resolve  =   ???  +   project -  >any_issue_resolver
permission   create_comment  =   project -  >comment_creator
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


```text
definition    issue    {
relation    project  :  project


permission   assign  =   project -  >issue_assigner
permission   resolve  =   ???  +   project -  >any_issue_resolver
permission   create_comment  =   project -  >comment_creator
}


```


We have a problem though: one of the capabilities that we can grant is` assigned_issue_resolver` . We don’t have any way of knowing if a particular user is assigned to the issue. We can fix this with another relation, and now compute the` resolve` using that relation:


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
definition    issue    {
relation    project  :  project
relation    assigned  :  user


permission   assign  =   project -  >issue_assigner
permission   resolve  =    (  project -  >assigned_issue_resolver  &   assigned )    +   project -  >any_issue_resolver
permission   create_comment  =   project -  >comment_creator
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
definition    issue    {
relation    project  :  project
relation    assigned  :  user


permission   assign  =   project -  >issue_assigner
permission   create_comment  =   project -  >comment_creator
}


```


We use the[intersection](https://docs.authzed.com/reference/schema-lang#-intersection) operation (` &` ) to make sure that the user is *both* the assigned user, and is a member of a role which grants them the` assigned_issue_resolver` capability.


The last permission to fill in is the` delete` permission on comments. Similar to how` issue` objects are related to` project` objects,` comment` objects are related to` issue` objects. We can fill in the permission as follows:


zed


1


2


3


4


```text
definition    comment    {
relation    issue  :  issue
permission   delete  =   issue -  >???
}


```


zed


1


2


3


4


```text
definition    comment    {
relation    issue  :  issue
permission   delete  =   issue -  >???
}


```


Oh no! We found another problem. Until support for[nested arrows](https://github.com/authzed/spicedb/issues/15) is finished, the` ->` operator only lets us traverse one level and recompute a relation, but the capability for` comment_deleter` is nested two levels away on the project!


We can solve this problem and keep the schema normalized by creating a synthetic permission on the` issue` which will do the next step of the resolution. The complete example is as follows:


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


12


13


14


15


16


```text
definition    issue    {
relation    project  :  project
relation    assigned  :  user


permission   assign  =   project -  >issue_assigner
permission   create_comment  =   project -  >comment_creator


// synthetic relation
permission   project_comment_deleter  =   project -  >comment_deleter
}


definition    comment    {
relation    issue  :  issue
permission   delete  =   issue -  >project_comment_deleter
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


12


13


14


15


16


```text
definition    issue    {
relation    project  :  project
relation    assigned  :  user


permission   assign  =   project -  >issue_assigner
permission   create_comment  =   project -  >comment_creator


// synthetic relation
permission   project_comment_deleter  =   project -  >comment_deleter
}


definition    comment    {
relation    issue  :  issue
permission   delete  =   issue -  >project_comment_deleter
}


```


The final capability that we haven’t modeled is the` role_manager` capability, which will be used to determine who is allowed to make changes to roles.


### Adding Role Meta-Permissions


We need to add some permissions to the role itself in order to allow roles to be created, bound to capabilities, and bound to users. However, we don’t want the built-in roles to be modifiable. We can do this all in our schema as well!


First we will add a` role_manager` capability to our` project` object:


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


12


```text
definition    project    {
relation    issue_creator  :  role  # member
relation    issue_assigner  :  role  # member
relation    any_issue_resolver  :  role  # member
relation    assigned_issue_resolver  :  role  # member
relation    comment_creator  :  role  # member
relation    comment_deleter  :  role  # member
relation    role_manager  :  role  # member


permission   create_issue  =   issue_creator
permission   create_role  =   role_manager
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


12


```text
definition    project    {
relation    issue_creator  :  role  # member
relation    issue_assigner  :  role  # member
relation    any_issue_resolver  :  role  # member
relation    assigned_issue_resolver  :  role  # member
relation    comment_creator  :  role  # member
relation    comment_deleter  :  role  # member
relation    role_manager  :  role  # member


permission   create_issue  =   issue_creator
permission   create_role  =   role_manager
}


```


Then we will add a` built_in_role` relation on our` role` object, and use that to disable certain permissions (` delete` ,` add_permissionon` , and` remove_permission` ) on the roles which would allow them to be modified or removed:


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
definition    role    {
relation    project  :  project
relation    member  :  user
relation    built_in_role  :  project


permission   delete  =   project -  >role_manager  -   built_in_role -  >role_manager
permission   add_user  =   project -  >role_manager
permission   add_permission  =   project -  >role_manager  -   built_in_role -  >role_manager
permission   remove_permission  =   project -  >role_manager  -   built_in_role -  >role_manager
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
definition    role    {
relation    project  :  project
relation    member  :  user
relation    built_in_role  :  project


permission   add_user  =   project -  >role_manager
}


```


By adding a built_in_role relationship with the project and using the[exclusion](https://authzed.com/blog/check-it-out-2) operator, when any of the role modifying permissions are evaluated, the result will be an empty set of users since built in roles will have relationships for both project->role_manager and built_in_role->role_manager.


### Putting It All Together


The following is the complete schema, which we can now start adding relationships to:


## Integrating With an Application


We need to add a few things to an application before we can fully utilize the schema that we’ve developed. We need to:


1. Bootstrap the permissions when we create a new project
2. Write a relationship from an issue to a project whenever we create a new issue
3. Write a relationship from a comment to an issue whenever we make a comment
4. Write or delete a relationship whenever a role is modified to change the permission
5. Protect all of the above with the proper permissions checks.


The following **pseudocode** blocks give a rough idea of how to do this for some of the operations in our application:


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


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


50


51


52


53


54


55


56


```text
def    create_project  ( creator_id, project_id  ):
authzed.write([
# Create the roles in the project
(resource:  "role:{project_id}-admin"  , relation:  "project"  , subject:  "project:{project_id}"  ),
(resource:  "role:{project_id}-developer"  , relation:  "project"  , subject:  "project:{project_id}"  ),
(resource:  "role:{project_id}-user,"   relation:  "project"  , subject:  "project:{project_id}"  ),
(resource:  "role:{project_id}-admin"  , relation:  "built-in-role"  , subject:  "project:{project_id}"  ),
(resource:  "role:{project_id}-developer"  , relation:  "built-in-role"  , subject:  "project:{project_id}"  ),
(resource:  "role:{project_id}-user"  , relation:  "built-in-role"  , subject:  "project:{project_id}"  ),


# Grant the permissions for the built-in roles
(resource:  "project:{project_id}"  , relation:  "issue_creator"  , subject:  "role:{project_id}-admin"  ),
(resource:  "project:{project_id}"  , relation:  "issue_assigner"  , subject:  "role:{project_id}-admin"  ),
(resource:  "project:{project_id}"  , relation:  "any_issue_resolver"  , subject:  "role:{project_id}-admin"  ),
(resource:  "project:{project_id}"  , relation:  "assigned_issue_resolver"  , subject:  "role:{project_id}-admin"  ),
(resource:  "project:{project_id}"  , relation:  "comment_creator"  , subject:  "role:{project_id}-admin"  ),
(resource:  "project:{project_id}"  , relation:  "comment_deleter"  , subject:  "role:{project_id}-admin"  ),
(resource:  "project:{project_id}"  , relation:  "role_manager"  , subject:  "role:{project_id}-admin"  ),


(resource:  "project:{project_id}"  , relation:  "issue_creator"  , subject:  "role:{project_id}-developer"  ),
(resource:  "project:{project_id}"  , relation:  "assigned_issue_resolver"  , subject:  "role:{project_id}-developer"  ),
(resource:  "project:{project_id}"  , relation:  "comment_creator"  , subject:  "role:{project_id}-developer"  ),


(resource:  "project:{project_id}"  , relation:  "issue_creator"  , subject:  "role:{project_id}-user"  ),
(resource:  "project:{project_id}"  , relation:  "comment_creator"  , subject:  "role:{project_id}-user"  ),


# Bind the project creator as an admin
(resource:  "role:{project_id}-admin"  , relation:  "member"  , subject:  "user:{creator_id}"  ),
])


def    create_issue  ( creator_id, project_id  ):
if   authzed.check(resource:  "project:{project_id}"  , permission:  "create_issue"  , subject:  "user:{creator_id}"  ):
issue_id = create_issue_in_database()


# Bind the issue to the project in authzed
authzed.write(resource:  "issue:{issue_id}"  , relation:  "project"  , subject:  "project:{project_id}"  )


def    assign_issue  ( assigner_id, assigned_to_id, issue_id  ):
if   authzed.check(resource:  "issue:{issue_id}"  , permission:  "assign"  , subject:  "user:{assigner_id}"  ):
# Assign the issue to the assigned_to
authzed.write(resource:  "issue:{issue_id}"  , relation:  "assigned"  , subject:  "user:{assigned_to_id}"  )


def    create_role  ( creator_id, project_id, role_name  ):
if   authzed.check(resource:  "project:{project_id}"  , permission:  "create_role"  , subject:  "user:{creator_id}"  ):
# Bind the role to the project in authzed
authzed.write(resource:  "role:{project_id}-{role_name}"  , relation:  "project"  , subject:  "project:{project_id}"  )


def    add_capability_to_role  ( creator_id, project_id, role_name, capability_name  ):
if   authzed.check(resource:  "role:{project_id}-{role_name}"  , permission:  "add_permission"  , subject:  "user:{creator_id}"  ):
# Bind the role to the capability in authzed
authzed.write(resource:  "project:{project_id}"  , relation: capability_name, subject:  "role:{project_id}-{role_name}"  )


def    add_member_to_role  ( creator_id, project_id, role_name, user_to_grant  ):
if   authzed.check(resource:  "role:{project_id}-{role_name}"  , permission:  "add_user"  , subject:  "user:{creator_id}"  ):
# Bind the role to the capability in authzed
authzed.write(resource:  "role:{project_id}-{role_name}"  , relation:  "member"  , subject:  "user:{user_to_grant}"  )


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


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


50


51


52


53


54


55


56


```text
def    create_project  ( creator_id, project_id  ):
authzed.write([
# Create the roles in the project


# Grant the permissions for the built-in roles


# Bind the project creator as an admin
])


def    create_issue  ( creator_id, project_id  ):
issue_id = create_issue_in_database()


# Bind the issue to the project in authzed


def    assign_issue  ( assigner_id, assigned_to_id, issue_id  ):
# Assign the issue to the assigned_to


def    create_role  ( creator_id, project_id, role_name  ):
# Bind the role to the project in authzed


# Bind the role to the capability in authzed


# Bind the role to the capability in authzed


```


And so on.


## Wrapping Up


I hope this has been a useful introduction to the theory and practice behind adding user defined roles to an application using Authzed. This powerful paradigm is often used to great effect in business-to-business and productivity apps, but you can even find similar permissions panels in consumer and social products. If you have any questions, get stuck adding user-defined roles to your own application, or just want to say hi, come[join us on Discord](https://authzed.com/discord) .


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Modeling the Schema
- Modeling the User
- Modeling Capabilities
- Modeling Roles
- Adding Relations to Bind Roles to Capabilities
- Computing Permissions from Relations
- Adding Role Meta-Permissions
- Putting It All Together
- Integrating With an Application
- Wrapping Up
- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
