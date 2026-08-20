---
schema_version: "1.0.0"
document_id: "df7a9442da8bbbc74ab0d59f236fde80724cec2b84969d5b42980d2adf48d60f"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/google-cloud-iam-modeling"
published_at: "2023-01-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:abc1303b6e9362184e2159ccb3189ffffb8466b11596e03babf59b6258be973f"
---

# Modeling Google Cloud IAM in SpiceDB

## Introduction


We often get asked about how you would model Infrastructure as a Service (IaaS) permissions in our[SpiceDB Schema Language](https://authzed.com/docs/reference/schema-lang) . Since we know that[Google Cloud IAM uses Zanzibar internally](https://zanzibar.tech/29zTn0kO4U:1o:1-) , it should be possible to use[relationship based access control](https://authzed.com/blog/check-it-out/) to get the desired effect. It might seem like the most interesting challenge would be modeling the hierarchical organization of Google Cloud into cloud services, and the projects into which they’re deployed. It turns out it’s actually the[complete separation of permissions and roles](https://cloud.google.com/iam/docs/roles-overview) .


Often when we think about role-based access control within the context of a service, the service owner has some idea of what roles are relevant to their service. For example: in Google Docs, there are the owner, editor, commented, and viewer roles, in order of most-to-least privileges. Having explored[user-defined roles](https://authzed.com/blog/user-defined-roles/) in a previous post, I wanted to take this opportunity to extend that idea into a more concrete example that separates out permissions completely, and allows for things like gaining access to a whole class of object, not just a particular instance.


I have recently been playing with[Google’s Cloud Spanner](https://cloud.google.com/spanner) service as part of turning-up customer[SpiceDB Dedicated](https://authzed.com/pricing/) clusters in Google Cloud Platform, so I’ll use that as an example here. The approach should be fully generalizable to modeling other GCP services as well.


## Cloud Spanner Permissions Model


In order to model Cloud Spanner’s permissions model, we first need to understand exactly what the model is. If we look at the full list of` spanner.*` permissions in Google Cloud IAM, we can see that there are five explicit types of entities referenced:


1. [Instances](https://cloud.google.com/spanner/docs/instances)
2. [Databases](https://cloud.google.com/spanner/docs/databases)
3. [Sessions](https://cloud.google.com/spanner/docs/sessions)
4. [Database Roles](https://cloud.google.com/spanner/docs/configure-fgac)
5. Database Operations, such as backup and restore operations


From a hierarchy perspective, the objects relate to one another as follows (arrows point from a sub-resource to the parent):


We can see from the[full list of permissions for Cloud Spanner](https://cloud.google.com/spanner/docs/iam) that each level of the Spanner hierarchy has specific permissions that cover one or more of the basic CRUDL operations, as well as some specific verbs for each object type. There are also a few additional rules we must incorporate into our Cloud Spanner model:


- GCP IAM roles are granted to a user within the context of a database, instance, or project.
- Databases inherit all permissions from the instances they belong to.
- Instances inherit permissions from the projects they belong to.


Without further ado, let’s get to modeling!


## Modeling


We are going to model in several phases. The first phase will be setting out the skeleton for the objects and permissions.


### Permissions Skeleton


First we want to define the object types and how they are going to relate to one another in the hierarchy.


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
definition    user    {  }


definition    project    {  }


definition    spanner_instance    {
relation    project  :  project
}


definition    spanner_database    {
relation    instance  :  spanner_instance
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
definition    user    {  }


definition    project    {  }


definition    spanner_instance    {
relation    project  :  project
}


definition    spanner_database    {
relation    instance  :  spanner_instance
}


```


Next we’ll stub out support for the various operations that we can do:


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


```text
definition    spanner_instance    {
relation    project  :  project


permission   get  =    nil
permission   getiampolicy  =    nil
permission   list  =    nil
}


definition    spanner_database    {
relation    instance  :  spanner_instance


// Database
permission   beginorrollbackreadwritetransaction  =    nil
permission   beginpartitioneddmltransaction  =    nil
permission   beginreadonlytransaction  =    nil
permission   create  =    nil
permission   drop  =    nil
permission   get  =    nil
permission   get_ddl  =    nil
permission   getiampolicy  =    nil
permission   list  =    nil
permission   partitionquery  =    nil
permission   partitionread  =    nil
permission   read  =    nil
permission   select  =    nil
permission   setiampolicy  =    nil
permission   update  =    nil
permission   updateddl  =    nil
permission   userolebasedaccess  =    nil
permission   write  =    nil


// Sessions
permission   create_session  =    nil
permission   delete_session  =    nil
permission   get_session  =    nil
permission   list_sessions  =    nil


// Database Operations
permission   cancel_operation  =    nil
permission   delete_operation  =    nil
permission   get_operation  =    nil
permission   list_operations  =    nil


// Database Roles
permission   list_roles  =    nil
permission   use_role  =    nil
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


```text
definition    spanner_instance    {
relation    project  :  project


permission   get  =    nil
permission   getiampolicy  =    nil
permission   list  =    nil
}


definition    spanner_database    {
relation    instance  :  spanner_instance


// Database
permission   beginorrollbackreadwritetransaction  =    nil
permission   beginpartitioneddmltransaction  =    nil
permission   beginreadonlytransaction  =    nil
permission   create  =    nil
permission   drop  =    nil
permission   get  =    nil
permission   get_ddl  =    nil
permission   getiampolicy  =    nil
permission   list  =    nil
permission   partitionquery  =    nil
permission   partitionread  =    nil
permission   read  =    nil
permission   select  =    nil
permission   setiampolicy  =    nil
permission   update  =    nil
permission   updateddl  =    nil
permission   userolebasedaccess  =    nil
permission   write  =    nil


// Sessions
permission   create_session  =    nil
permission   delete_session  =    nil
permission   get_session  =    nil
permission   list_sessions  =    nil


// Database Operations
permission   cancel_operation  =    nil
permission   delete_operation  =    nil
permission   get_operation  =    nil
permission   list_operations  =    nil


// Database Roles
permission   list_roles  =    nil
permission   use_role  =    nil
}


```


Note: You’ll notice that we listed session, operations, and roles under the database. We could have modeled them explicitly but as they are somewhat more inextricably linked to the database, and ephemeral in the case of operations and sessions, we can handle their permissions on their parent object.


Next, we need to define how we’re going to do custom roles.


### Roles and Role Bindings


When modeling our roles and role bindings, we need to be able to answer a few questions:


What are all of the permissions that can be bound to a role? Who is the role going to be bound to? At what layer in the hierarchy are we going to bind it?


First let’s start with the role itself. We’re going to enumerate all of the possible permissions that can be held by the role. We’re also going to set the subject type for adding a permission to a role to` user:*` . This will allow us to express the concept: “any user who holds this role will have this permission”.


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


```text
​​ definition    role    {
relation    spanner_databaseoperations_cancel  :  user  :*
relation    spanner_databaseoperations_delete  :  user  :*
relation    spanner_databaseoperations_get  :  user  :*
relation    spanner_databaseoperations_list  :  user  :*
relation    spanner_databaseroles_list  :  user  :*
relation    spanner_databaseroles_use  :  user  :*
relation    spanner_databases_beginorrollbackreadwritetransaction  :  user  :*
relation    spanner_databases_beginpartitioneddmltransaction  :  user  :*
relation    spanner_databases_beginreadonlytransaction  :  user  :*
relation    spanner_databases_create  :  user  :*
relation    spanner_databases_drop  :  user  :*
relation    spanner_databases_get  :  user  :*
relation    spanner_databases_getddl  :  user  :*
relation    spanner_databases_getiampolicy  :  user  :*
relation    spanner_databases_list  :  user  :*
relation    spanner_databases_partitionquery  :  user  :*
relation    spanner_databases_partitionread  :  user  :*
relation    spanner_databases_read  :  user  :*
relation    spanner_databases_select  :  user  :*
relation    spanner_databases_setiampolicy  :  user  :*
relation    spanner_databases_update  :  user  :*
relation    spanner_databases_updateddl  :  user  :*
relation    spanner_databases_userolebasedaccess  :  user  :*
relation    spanner_databases_write  :  user  :*
relation    spanner_instances_get  :  user  :*
relation    spanner_instances_getiampolicy  :  user  :*
relation    spanner_instances_list  :  user  :*
relation    spanner_sessions_create  :  user  :*
relation    spanner_sessions_delete  :  user  :*
relation    spanner_sessions_get  :  user  :*
relation    spanner_sessions_list  :  user  :*
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


```text
​​ definition    role    {
relation    spanner_databaseoperations_cancel  :  user  :*
relation    spanner_databaseoperations_delete  :  user  :*
relation    spanner_databaseoperations_get  :  user  :*
relation    spanner_databaseoperations_list  :  user  :*
relation    spanner_databaseroles_list  :  user  :*
relation    spanner_databaseroles_use  :  user  :*
relation    spanner_databases_beginorrollbackreadwritetransaction  :  user  :*
relation    spanner_databases_beginpartitioneddmltransaction  :  user  :*
relation    spanner_databases_beginreadonlytransaction  :  user  :*
relation    spanner_databases_create  :  user  :*
relation    spanner_databases_drop  :  user  :*
relation    spanner_databases_get  :  user  :*
relation    spanner_databases_getddl  :  user  :*
relation    spanner_databases_getiampolicy  :  user  :*
relation    spanner_databases_list  :  user  :*
relation    spanner_databases_partitionquery  :  user  :*
relation    spanner_databases_partitionread  :  user  :*
relation    spanner_databases_read  :  user  :*
relation    spanner_databases_select  :  user  :*
relation    spanner_databases_setiampolicy  :  user  :*
relation    spanner_databases_update  :  user  :*
relation    spanner_databases_updateddl  :  user  :*
relation    spanner_databases_userolebasedaccess  :  user  :*
relation    spanner_databases_write  :  user  :*
relation    spanner_instances_get  :  user  :*
relation    spanner_instances_getiampolicy  :  user  :*
relation    spanner_instances_list  :  user  :*
relation    spanner_sessions_create  :  user  :*
relation    spanner_sessions_delete  :  user  :*
relation    spanner_sessions_get  :  user  :*
relation    spanner_sessions_list  :  user  :*
}


```


Forgive the formatting, I generated this out of the list of permissions in the Cloud IAM console using find and replace. This definition answers the question: “What are all of the permissions that can be bound to a role?” At runtime we will bind a permission to a role by writing a relationship like the following:


` role:database_reader#spanner_databases_read@user:*`


Next we’ll add an object to bind a role to one or more specific users:


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


```text
definition    role_binding    {
relation    user  :  user
relation    role  :  role


permission   spanner_databaseoperations_cancel  =   user  &   role -  >spanner_databaseoperations_cancel
permission   spanner_databaseoperations_delete  =   user  &   role -  >spanner_databaseoperations_delete
permission   spanner_databaseoperations_get  =   user  &   role -  >spanner_databaseoperations_get
permission   spanner_databaseoperations_list  =   user  &   role -  >spanner_databaseoperations_list
permission   spanner_databaseroles_list  =   user  &   role -  >spanner_databaseroles_list
permission   spanner_databaseroles_use  =   user  &   role -  >spanner_databaseroles_use
permission   spanner_databases_beginorrollbackreadwritetransaction  =   user  &   role -  >spanner_databases_beginorrollbackreadwritetransaction
permission   spanner_databases_beginpartitioneddmltransaction  =   user  &   role -  >spanner_databases_beginpartitioneddmltransaction
permission   spanner_databases_beginreadonlytransaction  =   user  &   role -  >spanner_databases_beginreadonlytransaction
permission   spanner_databases_create  =   user  &   role -  >spanner_databases_create
permission   spanner_databases_drop  =   user  &   role -  >spanner_databases_drop
permission   spanner_databases_get  =   user  &   role -  >spanner_databases_get
permission   spanner_databases_getddl  =   user  &   role -  >spanner_databases_getddl
permission   spanner_databases_getiampolicy  =   user  &   role -  >spanner_databases_getiampolicy
permission   spanner_databases_list  =   user  &   role -  >spanner_databases_list
permission   spanner_databases_partitionquery  =   user  &   role -  >spanner_databases_partitionquery
permission   spanner_databases_partitionread  =   user  &   role -  >spanner_databases_partitionread
permission   spanner_databases_read  =   user  &   role -  >spanner_databases_read
permission   spanner_databases_select  =   user  &   role -  >spanner_databases_select
permission   spanner_databases_setiampolicy  =   user  &   role -  >spanner_databases_setiampolicy
permission   spanner_databases_update  =   user  &   role -  >spanner_databases_update
permission   spanner_databases_updateddl  =   user  &   role -  >spanner_databases_updateddl
permission   spanner_databases_userolebasedaccess  =   user  &   role -  >spanner_databases_userolebasedaccess
permission   spanner_databases_write  =   user  &   role -  >spanner_databases_write
permission   spanner_instances_get  =   user  &   role -  >spanner_instances_get
permission   spanner_instances_getiampolicy  =   user  &   role -  >spanner_instances_getiampolicy
permission   spanner_instances_list  =   user  &   role -  >spanner_instances_list
permission   spanner_sessions_create  =   user  &   role -  >spanner_sessions_create
permission   spanner_sessions_delete  =   user  &   role -  >spanner_sessions_delete
permission   spanner_sessions_get  =   user  &   role -  >spanner_sessions_get
permission   spanner_sessions_list  =   user  &   role -  >spanner_sessions_list
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


```text
definition    role_binding    {
relation    user  :  user
relation    role  :  role


permission   spanner_databases_get  =   user  &   role -  >spanner_databases_get
permission   spanner_instances_get  =   user  &   role -  >spanner_instances_get
permission   spanner_sessions_get  =   user  &   role -  >spanner_sessions_get
permission   spanner_sessions_list  =   user  &   role -  >spanner_sessions_list
}


```


Now we can bind the role to a user by writing a pair of relationships, atomically, in a transaction:


` role_binding:jake_is_reader#user@user:jake role_binding:jake_is_reader#role@role:database_reader`


Notice the use of the[intersection operator &](https://authzed.com/docs/reference/schema-lang#-intersection) . This operator is what lets us check **both** conditions, that the role has the permission, and that the user has the role.


We’ve answered our first two questions, now we need to bind it to our hierarchy to answer our final question:


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


17


```text
definition    project    {
relation    granted  :  role_binding
}


definition    spanner_instance    {
relation    project  :  project
relation    granted  :  role_binding


…
}


definition    spanner_database    {
relation    instance  :  spanner_instance
relation    granted  :  role_binding


…
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


17


```text
definition    project    {
relation    granted  :  role_binding
}


definition    spanner_instance    {
relation    project  :  project
relation    granted  :  role_binding


…
}


definition    spanner_database    {
relation    instance  :  spanner_instance
relation    granted  :  role_binding


…
}


```


Let's put it all together!


### Using Roles to Make Decisions


Now that we have our user-defined roles with explicit permissions, our objects, and our permissions defined, the last step is to use those bindings to make our permissions decisions. We’re going to start explicitly referencing role binding permissions (which are acting as synthetic relations) in our specific instance and database permissions computations.


Warning: this is going to be VERY verbose, as we need to thread through the permissions in the hierarchy manually. We hope to address this in the future by[allowing for nested arrow -> syntax](https://github.com/authzed/spicedb/issues/15) .


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


57


58


59


60


61


62


63


64


65


66


67


68


69


70


71


72


73


74


75


76


77


78


79


80


81


82


83


84


85


86


87


88


89


90


91


92


93


94


95


96


97


98


99


100


101


102


103


104


105


106


107


108


109


110


111


112


113


114


115


116


117


118


119


120


121


122


123


124


125


126


127


128


129


130


```text
definition    project    {
relation    granted  :  role_binding


// Synthetic Instance Relations
permission   granted_spanner_instances_get  =   granted -  >spanner_instances_get
permission   granted_spanner_instances_getiampolicy  =   granted -  >spanner_instances_getiampolicy
permission   granted_spanner_instances_list  =   granted -  >spanner_instances_list


// Synthetic Database Relations
permission   granted_spanner_databases_beginorrollbackreadwritetransaction  =   granted -  >spanner_databases_beginorrollbackreadwritetransaction
permission   granted_spanner_databases_beginpartitioneddmltransaction  =   granted -  >spanner_databases_beginpartitioneddmltransaction
permission   granted_spanner_databases_beginreadonlytransaction  =   granted -  >spanner_databases_beginreadonlytransaction
permission   granted_spanner_databases_create  =   granted -  >spanner_databases_create
permission   granted_spanner_databases_drop  =   granted -  >spanner_databases_drop
permission   granted_spanner_databases_get  =   granted -  >spanner_databases_get
permission   granted_spanner_databases_getddl  =   granted -  >spanner_databases_getddl
permission   granted_spanner_databases_getiampolicy  =   granted -  >spanner_databases_getiampolicy
permission   granted_spanner_databases_list  =   granted -  >spanner_databases_list
permission   granted_spanner_databases_partitionquery  =   granted -  >spanner_databases_partitionquery
permission   granted_spanner_databases_partitionread  =   granted -  >spanner_databases_partitionread
permission   granted_spanner_databases_read  =   granted -  >spanner_databases_read
permission   granted_spanner_databases_select  =   granted -  >spanner_databases_select
permission   granted_spanner_databases_setiampolicy  =   granted -  >spanner_databases_setiampolicy
permission   granted_spanner_databases_update  =   granted -  >spanner_databases_update
permission   granted_spanner_databases_updateddl  =   granted -  >spanner_databases_updateddl
permission   granted_spanner_databases_userolebasedaccess  =   granted -  >spanner_databases_userolebasedaccess
permission   granted_spanner_databases_write  =   granted -  >spanner_databases_write


// Synthetic Sessions Relations
permission   granted_spanner_sessions_create  =   granted -  >spanner_sessions_create
permission   granted_spanner_sessions_delete  =   granted -  >spanner_sessions_delete
permission   granted_spanner_sessions_get  =   granted -  >spanner_sessions_get
permission   granted_spanner_sessions_list  =   granted -  >spanner_sessions_list


// Synthetic Database Operations Relations
permission   granted_spanner_databaseoperations_cancel  =   granted -  >spanner_databaseoperations_cancel
permission   granted_spanner_databaseoperations_delete  =   granted -  >spanner_databaseoperations_delete
permission   granted_spanner_databaseoperations_get  =   granted -  >spanner_databaseoperations_get
permission   granted_spanner_databaseoperations_list  =   granted -  >spanner_databaseoperations_list


// Synthetic Database Roles Relations
permission   granted_spanner_databaseroles_list  =   granted -  >spanner_databaseroles_list
permission   granted_spanner_databaseroles_use  =   granted -  >spanner_databaseroles_use
}


definition    spanner_instance    {
relation    project  :  project
relation    granted  :  role_binding


permission   get  =   granted -  >spanner_instances_get  +   project -  >granted_spanner_instances_get
permission   getiampolicy  =   granted -  >spanner_instances_getiampolicy  +   project -  >granted_spanner_instances_getiampolicy
permission   list  =   granted -  >spanner_instances_list  +   project -  >granted_spanner_instances_list


// Synthetic Database Relations
permission   granted_spanner_databases_beginorrollbackreadwritetransaction  =   granted -  >spanner_databases_beginorrollbackreadwritetransaction  +   project -  >granted_spanner_databases_beginorrollbackreadwritetransaction
permission   granted_spanner_databases_beginpartitioneddmltransaction  =   granted -  >spanner_databases_beginpartitioneddmltransaction  +   project -  >granted_spanner_databases_beginpartitioneddmltransaction
permission   granted_spanner_databases_beginreadonlytransaction  =   granted -  >spanner_databases_beginreadonlytransaction  +   project -  >granted_spanner_databases_beginreadonlytransaction
permission   granted_spanner_databases_create  =   granted -  >spanner_databases_create  +   project -  >granted_spanner_databases_create
permission   granted_spanner_databases_drop  =   granted -  >spanner_databases_drop  +   project -  >granted_spanner_databases_drop
permission   granted_spanner_databases_get  =   granted -  >spanner_databases_get  +   project -  >granted_spanner_databases_get
permission   granted_spanner_databases_getddl  =   granted -  >spanner_databases_getddl  +   project -  >granted_spanner_databases_getddl
permission   granted_spanner_databases_getiampolicy  =   granted -  >spanner_databases_getiampolicy  +   project -  >granted_spanner_databases_getiampolicy
permission   granted_spanner_databases_list  =   granted -  >spanner_databases_list  +   project -  >granted_spanner_databases_list
permission   granted_spanner_databases_partitionquery  =   granted -  >spanner_databases_partitionquery  +   project -  >granted_spanner_databases_partitionquery
permission   granted_spanner_databases_partitionread  =   granted -  >spanner_databases_partitionread  +   project -  >granted_spanner_databases_partitionread
permission   granted_spanner_databases_read  =   granted -  >spanner_databases_read  +   project -  >granted_spanner_databases_read
permission   granted_spanner_databases_select  =   granted -  >spanner_databases_select  +   project -  >granted_spanner_databases_select
permission   granted_spanner_databases_setiampolicy  =   granted -  >spanner_databases_setiampolicy  +   project -  >granted_spanner_databases_setiampolicy
permission   granted_spanner_databases_update  =   granted -  >spanner_databases_update  +   project -  >granted_spanner_databases_update
permission   granted_spanner_databases_updateddl  =   granted -  >spanner_databases_updateddl  +   project -  >granted_spanner_databases_updateddl
permission   granted_spanner_databases_userolebasedaccess  =   granted -  >spanner_databases_userolebasedaccess  +   project -  >granted_spanner_databases_userolebasedaccess
permission   granted_spanner_databases_write  =   granted -  >spanner_databases_write  +   project -  >granted_spanner_databases_write


// Synthetic Sessions Relations
permission   granted_spanner_sessions_create  =   granted -  >spanner_sessions_create  +   project -  >granted_spanner_sessions_create
permission   granted_spanner_sessions_delete  =   granted -  >spanner_sessions_delete  +   project -  >granted_spanner_sessions_delete
permission   granted_spanner_sessions_get  =   granted -  >spanner_sessions_get  +   project -  >granted_spanner_sessions_get
permission   granted_spanner_sessions_list  =   granted -  >spanner_sessions_list  +   project -  >granted_spanner_sessions_list


// Synthetic Database Operations Relations
permission   granted_spanner_databaseoperations_cancel  =   granted -  >spanner_databaseoperations_cancel  +   project -  >granted_spanner_databaseoperations_cancel
permission   granted_spanner_databaseoperations_delete  =   granted -  >spanner_databaseoperations_delete  +   project -  >granted_spanner_databaseoperations_delete
permission   granted_spanner_databaseoperations_get  =   granted -  >spanner_databaseoperations_get  +   project -  >granted_spanner_databaseoperations_get
permission   granted_spanner_databaseoperations_list  =   granted -  >spanner_databaseoperations_list  +   project -  >granted_spanner_databaseoperations_list


// Synthetic Database Roles Relations
permission   granted_spanner_databaseroles_list  =   granted -  >spanner_databaseroles_list  +   project -  >granted_spanner_databaseroles_list
permission   granted_spanner_databaseroles_use  =   granted -  >spanner_databaseroles_use  +   project -  >granted_spanner_databaseroles_use
}


definition    spanner_database    {
relation    instance  :  spanner_instance
relation    granted  :  role_binding


// Database
permission   beginorrollbackreadwritetransaction  =   granted -  >spanner_databases_beginorrollbackreadwritetransaction  +   instance -  >granted_spanner_databases_beginorrollbackreadwritetransaction
permission   beginpartitioneddmltransaction  =   granted -  >spanner_databases_beginpartitioneddmltransaction  +   instance -  >granted_spanner_databases_beginpartitioneddmltransaction
permission   beginreadonlytransaction  =   granted -  >spanner_databases_beginreadonlytransaction  +   instance -  >granted_spanner_databases_beginreadonlytransaction
permission   create  =   granted -  >spanner_databases_create  +   instance -  >granted_spanner_databases_create
permission   drop  =   granted -  >spanner_databases_drop  +   instance -  >granted_spanner_databases_drop
permission   get  =   granted -  >spanner_databases_get  +   instance -  >granted_spanner_databases_get
permission   get_ddl  =   granted -  >spanner_databases_getddl  +   instance -  >granted_spanner_databases_getddl
permission   getiampolicy  =   granted -  >spanner_databases_getiampolicy  +   instance -  >granted_spanner_databases_getiampolicy
permission   list  =   granted -  >spanner_databases_list  +   instance -  >granted_spanner_databases_list
permission   partitionquery  =   granted -  >spanner_databases_partitionquery  +   instance -  >granted_spanner_databases_partitionquery
permission   partitionread  =   granted -  >spanner_databases_partitionread  +   instance -  >granted_spanner_databases_partitionread
permission   read  =   granted -  >spanner_databases_read  +   instance -  >granted_spanner_databases_read
permission   select  =   granted -  >spanner_databases_select  +   instance -  >granted_spanner_databases_select
permission   setiampolicy  =   granted -  >spanner_databases_setiampolicy  +   instance -  >granted_spanner_databases_setiampolicy
permission   update  =   granted -  >spanner_databases_update  +   instance -  >granted_spanner_databases_update
permission   updateddl  =   granted -  >spanner_databases_updateddl  +   instance -  >granted_spanner_databases_updateddl
permission   userolebasedaccess  =   granted -  >spanner_databases_userolebasedaccess  +   instance -  >granted_spanner_databases_userolebasedaccess
permission   write  =   granted -  >spanner_databases_write  +   instance -  >granted_spanner_databases_write


// Sessions
permission   create_session  =   granted -  >spanner_sessions_create  +   instance -  >granted_spanner_sessions_create
permission   delete_session  =   granted -  >spanner_sessions_delete  +   instance -  >granted_spanner_sessions_delete
permission   get_session  =   granted -  >spanner_sessions_get  +   instance -  >granted_spanner_sessions_get
permission   list_sessions  =   granted -  >spanner_sessions_list  +   instance -  >granted_spanner_sessions_list


// Database Operations
permission   cancel_operation  =   granted -  >spanner_databaseoperations_cancel  +   instance -  >granted_spanner_databaseoperations_cancel
permission   delete_operation  =   granted -  >spanner_databaseoperations_delete  +   instance -  >granted_spanner_databaseoperations_delete
permission   get_operation  =   granted -  >spanner_databaseoperations_get  +   instance -  >granted_spanner_databaseoperations_get
permission   list_operations  =   granted -  >spanner_databaseoperations_list  +   instance -  >granted_spanner_databaseoperations_list


// Database Roles
permission   list_roles  =   granted -  >spanner_databaseroles_list  +   instance -  >granted_spanner_databaseroles_list
permission   use_role  =   granted -  >spanner_databaseroles_use  +   instance -  >granted_spanner_databaseroles_use
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


57


58


59


60


61


62


63


64


65


66


67


68


69


70


71


72


73


74


75


76


77


78


79


80


81


82


83


84


85


86


87


88


89


90


91


92


93


94


95


96


97


98


99


100


101


102


103


104


105


106


107


108


109


110


111


112


113


114


115


116


117


118


119


120


121


122


123


124


125


126


127


128


129


130


```text
definition    project    {
relation    granted  :  role_binding


// Synthetic Instance Relations


// Synthetic Database Relations


// Synthetic Sessions Relations
permission   granted_spanner_sessions_get  =   granted -  >spanner_sessions_get


// Synthetic Database Operations Relations


// Synthetic Database Roles Relations
}


definition    spanner_instance    {
relation    project  :  project
relation    granted  :  role_binding


// Synthetic Database Relations


// Synthetic Sessions Relations


// Synthetic Database Operations Relations


// Synthetic Database Roles Relations
}


definition    spanner_database    {
relation    instance  :  spanner_instance
relation    granted  :  role_binding


// Database


// Sessions


// Database Operations


// Database Roles
}


```


## Putting It All Together


I’ve included a[playground share link with the complete example](https://play.authzed.com/s/pNIDdxdKKOMG/schema) that you can hopefully use to adapt and expand for your own use. You can also look forward to a future post where I will cover how to use our brand new[caveated relationships](https://authzed.com/blog/caveats/) feature to model the[Google Cloud IAM conditions](https://cloud.google.com/iam/docs/conditions-overview) feature for *even more* fine-grained access! If this kind of modeling is relevant to your organization’s use case, but you’re still struggling a bit, I highly recommend joining our[Discord](https://authzed.com/discord) or[scheduling a call](https://authzed.com/contact/?utm_source=blog) !


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Introduction
- Cloud Spanner Permissions Model
- Modeling
- Permissions Skeleton
- Roles and Role Bindings
- Using Roles to Make Decisions
- Putting It All Together
- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
