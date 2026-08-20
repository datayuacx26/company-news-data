---
schema_version: "1.0.0"
document_id: "3359e16ce22269312a62fb0b42cf348b3681afe2376a41e2fb700ddaeb66e0f5"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/build-you-a-google-groups"
published_at: "2022-01-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:e2f956c079718fb4026cabc75ef137d9ccdc2d18b3243b40c7b96c56c2c9ae0f"
---

# Build you a Google Groups

**NOTE:** This post is a companion to the video below. The video is in the form of an exploratory discussion. This post discusses the resulting artifact.


Google Groups is a mailing list discussion service that Google launched back in 2001. Over time it has expanded in scope to also facilitate general group management within Google Workspace (formerly G Suite). There are a number of interesting permissions features which make this a fun service to model. They are:


- Permissions can require various different levels of permissions on a per-group basis.
- Some groups are open to posting from anybody, even anonymous users.
- Groups can hold roles in other groups.
- Custom roles that can be granted many of the permissions
- Banned users


As a permissions systems provider, we have developed names for the common solutions to these features. They are, respectively:


- User defined roles
- Public resources
- Recursive membership
- User defined roles with roles defined in data
- Exclusions


In this post I will talk about how those features are exposed in Google Groups, how one would model them in Authzed, and how to wire it up with your application.


## Per-Group Permissions


The Google Groups UI is rife with sliders that let you pick which role is required to perform an action. Here is an example slider which lets you configure who can view past conversations in a group:


On the far left, and also required, is the group’s owners. The following snippet shows how to always grant` owners` the` view_conversations` permission.


zed


1


2


3


4


5


6


```text
definition    user    {  }


definition    group    {
relation    owner  :  user
permission   view_conversations  =   owner
}


```


zed


1


2


3


4


5


6


```text
definition    user    {  }


definition    group    {
relation    owner  :  user
permission   view_conversations  =   owner
}


```


We can bind a user as the owner of a group by writing the following relationship:


python


1


2


```text
authzed.write(resource= "group:test-group"  , relation= "owner"  , subject= "user:the-owner"  )


```


python


1


2


```text


```


Now we’ve handled the static case, where the role *always* has the permission. Now let’s work through the dynamic cases.


Based on the detents in the slider, we can infer that similar to our group` owner` relation, there must also be a` manager` and` member` relation, and that we must therefore be able to grant` view_conversations` to all members of the organization, meaning we need a relationship to the organization.


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


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    organization  :  organization


relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   view_conversations  =   owner  +   ???
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


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    organization  :  organization


relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   view_conversations  =   owner  +   ???
}


```


If we directly put` manager` , and` direct_member` in the permission computation for` view_conversations` that would make them static rules, and users would always be given that permission, invaliding the use of the slider.


To make the behavior of` view_conversations` dynamic, and user-defined, we need another layer of indirection. We will add a relation called` viewers` and use that to bind *back to the same group* for various positions in the slider.


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
definition    user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   member  =   owner  +    (  manager  +   direct_member )


relation    viewers  :  group  # manager     |   group#member  |   organization#member
permission   view_conversations  =   owner  +   viewers
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
definition    user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   member  =   owner  +    (  manager  +   direct_member )


permission   view_conversations  =   owner  +   viewers
}


```


The following slider images and their corresponding relationships show how one would respond to a slider change:


python


1


2


```text
authzed.write(resource= "group:test-group"  , relation= "viewers"  , subject= "group:test-group"  , subject_relation= "manager"  )


```


python


1


2


```text


```


python


1


2


```text
authzed.write(resource= "group:test-group"  , relation= "viewers"  , subject= "group:test-group"  , subject_relation= "member"  )


```


python


1


2


```text


```


python


1


2


3


4


```text
authzed.write([
(resource= "group:test-group"  , relation= "viewers"  , subject= "group:test-group"  , subject_relation= "member"  ),
(resource= "group:test-group"  , relation= "viewers"  , subject= "organization:big-company"  , subject_relation= "member"  ),
])


```


python


1


2


3


4


```text
authzed.write([
])


```


These snippets presume a bit of hierarchy. First, in our schema we define that group` member` also includes group` manager` . This means that when the slider is in the "Group members" position, we do not need to separately write a relationship for "Group managers". For the "Entire organization" detent, we **do not** presume that organization` member` includes all group members and managers, so we write separate relationships for both of those paths *in a single batch transaction* .


You can basically copy and paste this pattern for most of the other sliders, which have the same detents and capabilities. There are a few sliders, such as those that can post, that include a detent for "Anyone on the web". For this, we’ll need to head into the wild, wild\[card\] west.


## Public Resources


For those sliders that include an option for "Anyone on the web" to post, we’ll need a different kind of dynamic permission. One that doesn’t just match *explicitly stated* subjects, but one that can match *any subject of a given type* .


To handle this, we recently launched a feature called "[wildcard permissions](https://docs.authzed.com/reference/schema-lang#wildcards) ". Wildcard permissions let you write a relationship that will cause any subject of the given type to match. In our specific example, we want to allow the user to say that *any user* can post to the group. To start, we’ll need to replicate the work we’ve done for` view_conversations` and adapt it to` post` .


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
definition    user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   member  =   owner  +    (  manager  +   direct_member )


relation    posters  :  group  # manager     |   group#member  |   organization#member
permission   post  =   owner  +   posters
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
definition    user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   member  =   owner  +    (  manager  +   direct_member )


permission   post  =   owner  +   posters
}


```


Now, to extend this for any user, we extend the types allowed for the` posters` relation to include support for a wildcard over the` user` object type:


zed


1


```text
relation    posters  :  group  # manager     |   group#member  |   organization#member  |    user  :*


```


zed


1


```text


```


Now to extend our model to handle the detent for "Any user on the web" we can write a relationship like the following, where we replace a specific object id with the literal` *` :


python


1


```text
authzed.write(resource= "group:test-group"  , relation= "posters"  , subject= "user:*"  )


```


python


1


```text


```


This will now match any user object used as a subject, e.g.` user:the-owner` ,` user:stacey` or` user:a-bad-guy` . There is still a remaining deficiency. What do we do about truly anonymous users, who haven’t even been given a user id?


For that, we can add an additional object type to use as the subject for anonymous users. Then, in our code, when we want to perform a permissions check for an anonymous user, we just synthesize any object of this new type.


Here is the updated schema for anonymous:


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


```text
definition    user    {  }


definition    anonymous_user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   member  =   owner  +    (  manager  +   direct_member )


relation    posters  :  group  # manager     |   group#member  |   organization#member  |    user  :*    |    anonymous_user  :*
permission   post  =   owner  +   posters
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


```text
definition    user    {  }


definition    anonymous_user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user
relation    manager  :  user
relation    direct_member  :  user


permission   member  =   owner  +    (  manager  +   direct_member )


permission   post  =   owner  +   posters
}


```


Now when we want to open up posting to the public we write these **two** relationships.


python


1


2


3


4


```text
authzed.write([
(resource= "group:test-group"  , relation= "posters"  , subject= "user:*"  ),
(resource= "group:test-group"  , relation= "posters"  , subject= "anonymous_user:*"  ),
])


```


python


1


2


3


4


```text
authzed.write([
(resource= "group:test-group"  , relation= "posters"  , subject= "user:*"  ),
])


```


And finally, a check call for an anonymous user would look like the following:


python


1


2


```text
if   authzed.check(resource= "group:test-group"  , permission= "post"  , subject= "anonymous_user:this-can-be-anything"  ):
post()


```


python


1


2


```text
post()


```


As we’ve built up this example, I’ve been glossing over the fact that groups can also be members of other groups! Time to get a little recursive.


## Groups in Groups (in Groups (in Groups))


Luckily, it’s a relatively easy change to allow groups to be members of other groups! Here is our updated` view_conversations` example schema:


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
definition    user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user     |   group#member
relation    manager  :  user     |   group#member
relation    direct_member  :  user     |   group#member


permission   member  =   owner  +    (  manager  +   direct_member )


permission   view_conversations  =   owner  +   viewers
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
definition    user    {  }


definition    organization    {
relation    member  :  user
}


definition    group    {
relation    owner  :  user     |   group#member
relation    manager  :  user     |   group#member
relation    direct_member  :  user     |   group#member


permission   member  =   owner  +    (  manager  +   direct_member )


permission   view_conversations  =   owner  +   viewers
}


```


Notice all we had to do was to add` group#member` to the allowable types for each of our direct group relations. The following screenshot shows adding a group to another group, and the corresponding relationship write.


python


1


```text
authzed.write(resource= "group:test-group"  , relation= "direct_member"  , subject= "group:security"  , subject_relation= "member"  )


```


python


1


```text


```


This ability is very powerful! In fact, it’s one of the core differentiators between a Zanzibar-based permissions system and a more traditional direct role grant model.


Now that we’ve handled all of the built-in roles, let’s delve into custom roles.


## Custom Roles


Google Groups has a somewhat hidden feature to be able to define custom roles, and give that role permissions to do various things within a group. For a simple example, we will add support for custom roles, and allow that role to be granted the` view_conversations` permission.


First, we need to start by adding a` custom_role` object type, and allow users to be set as members of that role.


zed


1


2


3


```text
definition    custom_role    {
relation    member  :  user     |   group#member
}


```


zed


1


2


3


```text
definition    custom_role    {
relation    member  :  user     |   group#member
}


```


Similarly to our groups in groups, custom roles can also have groups as members. Next, we’ll add that custom role type to our group object type, allowing relationships to be written granting that role the` view_conversations` permission.


**NOTE:** In the real Google Groups product, custom roles are primarily used for group meta-permissions, e.g. who can change group membership or perform content moderation.


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
definition    group    {
relation    owner  :  user     |   group#member
relation    manager  :  user     |   group#member
relation    direct_member  :  user     |   group#member


permission   member  =   owner  +    (  manager  +   direct_member )


relation    viewers  :  group  # manager     |   group#member  |   organization#member  |   custom_role#member
permission   view_conversations  =   owner  +   viewers
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
definition    group    {
relation    owner  :  user     |   group#member
relation    manager  :  user     |   group#member
relation    direct_member  :  user     |   group#member


permission   member  =   owner  +    (  manager  +   direct_member )


permission   view_conversations  =   owner  +   viewers
}


```


python


1


```text
authzed.write(resource= "group:test-group"  , relation= "viewers"  , subject= "custom_role:super-secret-people"  , subject_relation= "member"  )


```


python


1


```text


```


Now that we’ve got our basic schema fairly well developed these new features are proving relatively easy to add! Now let’s take a look at banning users through exclusions.


## Banned Users


Google Groups allows you to ban users from interacting with the group.


So far all of our permissions have been computed using the[union](https://docs.authzed.com/reference/schema-lang#-union) (` +` ) operation. This is the most commonly used operator, but the schema language also supports[intersection](https://docs.authzed.com/reference/schema-lang#-intersection) (` &` ) and[exclusion](https://docs.authzed.com/reference/schema-lang#--exclusion) (` -` ). We’ll use exclusion to set up a new type of user: a banned user.


By now you might be able to guess how this will be added to the schema:


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
definition    group    {
relation    owner  :  user     |   group#member
relation    manager  :  user     |   group#member
relation    direct_member  :  user     |   group#member
relation    banned  :  user     |   group#member


permission   member  =   owner  +    (  manager  +   direct_member  -   banned )


permission   view_conversations  =   owner  +    (  viewers  -   banned )
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
definition    group    {
relation    owner  :  user     |   group#member
relation    manager  :  user     |   group#member
relation    direct_member  :  user     |   group#member
relation    banned  :  user     |   group#member


permission   member  =   owner  +    (  manager  +   direct_member  -   banned )


permission   view_conversations  =   owner  +    (  viewers  -   banned )
}


```


**NOTE:** We’ve used parentheses to enforce an order of operations that will still allow banned users who are also group owners to view conversations. This is in keeping in spirit with the actual Google Groups which uses meta-permissions to prevent owners from being banned in the first place.


Now we can ban a user by writing another simple relationship!


python


1


```text
authzed.write(resource= "group:test-group"  , relation= "banned"  , subject= "user:villain"  )


```


python


1


```text


```


That’s it! After writing that relationship, the user will be magically unable to view any conversations, regardless of whether they otherwise would have been able to.


## Putting It All Together


If you found this interesting, you can watch Jimmy and I model permissions (and the meta-permissions) for nearly every feature in Google Groups in the video above. I have also embedded a working playground with the entire example from the video here for posterity.


If you’re experiencing similar permissions problems in your application, I encourage you to[join us on Discord](https://authzed.com/discord) where you can collaborate with our team or other users who are using Authzed and SpiceDB to solve permissions in their app. If you think this type of content is valuable for others, you can use the social links below to share it with others!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Per-Group Permissions
- Public Resources
- Groups in Groups (in Groups (in Groups))
- Custom Roles
- Banned Users
- Putting It All Together
- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
