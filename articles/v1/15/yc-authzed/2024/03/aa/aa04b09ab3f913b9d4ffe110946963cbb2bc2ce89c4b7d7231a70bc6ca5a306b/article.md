---
schema_version: "1.0.0"
document_id: "aa04b09ab3f913b9d4ffe110946963cbb2bc2ce89c4b7d7231a70bc6ca5a306b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/fine-grained-access-control"
published_at: "2024-03-05T05:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:13.235024+00:00"
content_hash: "sha256:80333e1568baa3fd7a0d8ad3c83a6bf0975c4043135667bb70f7a39128eb9ba7"
---

# Fine-Grained Access Control: Can You Go Too Fine?

SpiceDB, our open-source, Google Zanzibar-inspired permissions database, gives developers a lot of flexibility when building an authorization scheme—you can build common patterns like Role-Based Access Control (RBAC) or Attribute-Based Access Control (ABAC), and more complex ones like User-Defined Roles or Fine Grained Authorization (FGA).


Underpinning all this flexibility is SpiceDB’s underlying graph database which lets you store relationships between objects in accordance with a purpose built schema—your permissions data. Once you have your permissions data stored, you can issue authorization queries against SpiceDB to get answers to questions like “does user 123 have access to document 456?”


But for FGA it’s not always apparent how fine your permissions data should be. A user of ours was building a Fine Grained Access Control system and asked me:


> > > “Should I be creating relationships for every cell in a spreadsheet?”


More generally, “which things do I need to write relationships for?” It’s an interesting question that doesn’t always have a clear answer. Our guidance to users is usually to model the smallest resource upon which they need to check permissions. Sometimes this is really obvious: if you are building a document editor, it’s probably the document; if you are creating a ride-sharing app, it’s probably the trip; and if you are building a video-sharing app, it’s probably the video in its entirety, not the individual frames of the video. In all of these cases, there is nothing to be gained from breaking the resource down further in your modeling.


Take for example a document, like those in Microsoft Word or Google Docs. What would happen if we decided to model permissions on each individual word? For one, every time a new word was written, we would have to call[WriteRelationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#WriteRelationships) to link that word to the document. This blog post alone has 1,343 words in itself, and I can write a few new words every second. This would generate an incredible amount of data and write workload. Is it worth it? What additional capability do we get? Is there a case where the permissions will change from word to word? Unlikely in this scenario.


Let’s get back to our spreadsheet example though. Sometimes individual cells *can* have different effective permissions. Some cells can be protected, some cells can be in a read-only or computed column. Maybe this spreadsheet supports granting access to individual rows or columns of the sheet. So should we model the individual cells? In this post, I hope to give you some tools that you can use to evaluate this question for your own tricky modeling scenarios.


## Balancing flexibility with maintainability in Fine-Grained Authorization


Our usual guidance to model the smallest thing possible is borne out of the desire to give the greatest amount of permissions model flexibility while preserving consistency at the enforcement call site. Take for example the following contrived schema and pseudocode:


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
definition    user    {  }


definition    folder    {
relation    member  :  user
}


definition    document    {
relation    folder  :  folder
permission   view  =   folder -  >member
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
definition    user    {  }


definition    folder    {
relation    member  :  user
}


definition    document    {
relation    folder  :  folder
permission   view  =   folder -  >member
}


```


python


1


```text
check(resource=document. id  , permission=“view”, subject=user. id  )


```


python


1


```text
check(resource=document. id  , permission=“view”, subject=user. id  )


```


Right now the` document` object has its` view` permission entirely derived from the parent` folder` ’s` member` s. You could easily rewrite the check code as:


python


1


```text
check(resource=document.folder. id  , permission=“member”, subject=user. id  )


```


python


1


```text


```


Then you wouldn’t have to write relationships between the` document` and` folder` objects, and you could save both time and money! But what happens when you need to change the way the view permission is computed:


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


definition    folder    {
relation    member  :  user
}


definition    document    {
relation    folder  :  folder
relation    viewer  :  user
permission   view  =   folder -  >member  +   viewer
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


definition    folder    {
relation    member  :  user
}


definition    document    {
relation    folder  :  folder
relation    viewer  :  user
permission   view  =   folder -  >member  +   viewer
}


```


If your check call used the` folder`` member` option, you now have to change your code to check directly on` document` objects, **and** you also have to backfill all of the relationships between` document` and` folder` .


This is why we usually tell users to choose the most granular option. But it comes at a cost.


### Granularity has a cost


Let’s go back to our spreadsheet cell example. Modeled as a schema, it might look like the following:


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


```text
definition    user    {  }


definition    sheet    {
relation    owner  :  user
}


definition    sheet_row    {
relation    sheet  :  sheet
relation    row_viewer  :  user
permission   view_row  =   sheet -  >owner  +   row_viewer
}


definition    sheet_col    {
relation    sheet  :  sheet
relation    col_viewer  :  user
permission   view_column  =   sheet -  >owner  +   col_viewer
}


definition    cell    {
relation    srow  :  sheet_row
relation    scol  :  sheet_col
permission   view  =   srow -  >view_row  &   scol -  >view_column
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


```text
definition    user    {  }


definition    sheet    {
relation    owner  :  user
}


definition    sheet_row    {
relation    sheet  :  sheet
relation    row_viewer  :  user
permission   view_row  =   sheet -  >owner  +   row_viewer
}


definition    sheet_col    {
relation    sheet  :  sheet
relation    col_viewer  :  user
permission   view_column  =   sheet -  >owner  +   col_viewer
}


definition    cell    {
relation    srow  :  sheet_row
relation    scol  :  sheet_col
permission   view  =   srow -  >view_row  &   scol -  >view_column
}


```


python


1


```text
check(resource=cell. id  , permission=“view”, subject=user. id  )


```


python


1


```text
check(resource=cell. id  , permission=“view”, subject=user. id  )


```


This model restricts someone from viewing a cell to only those cells for which they have permissions to both view the row and the column.


In this model, each` cell` would require two relationships, one for the` row` and one for the` column` , and each` row` and` column` would require a relationship each to bind them to the` sheet` .


A modestly sized spreadsheet of 15 columns and 2,000 rows would require 62,015 relationships! And that is only for spreadsheets with known columns and rows, many spreadsheets start off with a much larger (or simulated infinite) size! Even if you didn’t write the relationships until the cell was used, a “fill down” operation would generate 4,000 new relationships! Finally, opening up the spreadsheet and only showing the user the cells they have permissions to see would require the full 62,015 checks.


All of these numbers aren’t really *that* large, but consider how many cells there probably are in all of the spreadsheets on Google Sheets. It quickly becomes apparent that Google likely doesn’t model permissions on their spreadsheets this way internally.


### Striking a balance


In our spreadsheet schema, the` view` permission was based on the permissions inherited through the row and column. We didn’t really have a hard requirement to directly federate access to individual cells, we only chose to model it that way to maximize flexibility and call site stability.


When we find ourselves in the position of paying too high a price for too little value, we can look to compromise! Maybe there is a way to get the feature we want while preserving *relatively* good flexibility with *relatively* stable call sites.


If our requirement is to federate access based on rows or columns, we can model that directly and slightly denormalize our call sites for a vast reduction in cost. Let’s take a look at another schema:


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
definition    user    {  }


definition    sheet    {
relation    owner  :  user
}


definition    sheet_row    {
relation    sheet  :  sheet
relation    row_viewer  :  user
permission   view_row  =   sheet -  >owner  +   row_viewer
}


definition    sheet_col    {
relation    sheet  :  sheet
relation    col_viewer  :  user
permission   view_column  =   sheet -  >owner  +   col_viewer
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
definition    user    {  }


definition    sheet    {
relation    owner  :  user
}


definition    sheet_row    {
relation    sheet  :  sheet
relation    row_viewer  :  user
permission   view_row  =   sheet -  >owner  +   row_viewer
}


definition    sheet_col    {
relation    sheet  :  sheet
relation    col_viewer  :  user
permission   view_column  =   sheet -  >owner  +   col_viewer
}


```


By eliminating the` cell` definition, we’ve necessitated a change to our call sites as well. To preserve the original intention of restricting access to those who can both view the row and view the column, we would update our call sites accordingly:


python


1


```text
check(resource=cell.column. id  , permission=“view_column”, subject=user. id  )  and   check(resource=cell.row. id  , permission=“view_row”, subject=user. id  )


```


python


1


```text


```


Now our theoretical spreadsheet with 15 columns and 2000 rows only requires 2,015 relationships, and creating new cells doesn’t require any additional writes. However, because we need 2 checks for every cell, opening the sheet now requires double the checks!


The final optimization is to either move the checks to the rendering logic for a column and row or to cache the results of the checks for columns and rows because the check results on a row and column don’t change per cell.


Now opening our spreadsheet only makes 2,015 checks and only grows when new rows or columns are added.


## Takeaways


When modeling permissions for your app, it isn’t always obvious what resources and relationships are necessary. We recommend the following steps as a good starting point and process for refinement:


1. Start by modeling the most granular resource you need to check permissions. This provides the greatest amount of permissions model flexibility while preserving consistency at the enforcement call site.
2. If the required number of relationships or check calls for the most granular resource explodes, look to make a tradeoff. Are you actually utilizing that resource in your model? Does it align with an actual hard requirement?
3. Reduce the number of required relationships by checking permissions on an aggregate resource. In the example, changing the app to make check calls on row and column eliminated the need for relationships between cell and` sheet_row` and` sheet_col` .
4. Reduce the number of required check calls by caching the check results on the aggregate resources. In the example, cells in the same row and column depend on the same check results. Furthermore, checks on the rows/columns could be delayed by making those calls only when the row or column became visible.


While these steps are a good starting point, every app can have unique requirements. If you have questions or encounter any modeling challenges, join us in our[Discord server](https://authzed.com/discord) . Authzed engineers and a community of users are there to discuss and help.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Balancing flexibility with maintainability in Fine-Grained Authorization
- Granularity has a cost
- Striking a balance
- Takeaways
- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)


[Engineering Modeling Google Cloud IAM in SpiceDB We often get asked about how you would model Infrastructure as a Service (IaaS) permissions in our SpiceDB Schema Language. Since we know that Google Cloud IAM uses Zanzibar internally, it should be possible to use relationship based access control to get the desired effect. Jan 19, 2023 · 11 min](https://authzed.com/blog/google-cloud-iam-modeling)[Engineering Modeling Google Cloud IAM in SpiceDB We often get asked about how you would model Infrastructure as a Service (IaaS) permissions in our SpiceDB Schema Language. Since we know that Google Cloud IAM uses Zanzibar internally, it should be possible to use relationship based access control to get the desired effect. Jake Moshenko · Jan 19, 2023 · 11 min](https://authzed.com/blog/google-cloud-iam-modeling)
