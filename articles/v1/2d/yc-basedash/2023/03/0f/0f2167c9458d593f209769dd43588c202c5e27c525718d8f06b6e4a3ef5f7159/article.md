---
schema_version: "1.0.0"
document_id: "0f2167c9458d593f209769dd43588c202c5e27c525718d8f06b6e4a3ef5f7159"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/implementing-re-ordering-at-the-database-level-our-experience/"
published_at: "2023-03-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:2121a1872fb68b3bdb164bf4688c9980473262e91d28ae84691a13d74724a2da"
---

# Implementing Re-Ordering at the Database Level: Our Experience

We recently implemented a long-requested feature in Basedash: the ability to re-order pages in the sidebar. The biggest challenge was figuring out how to save the order in our database and how we were going to update that order value when a user re-orders the page in the sidebar.


### Re-ordering requirements


We had two requirements for re-ordering pages in the sidebar:


- Reorder pages within a section
- Allow a page to be dragged to a new section


In the sidebar, pages are grouped by Team and then can be further classified in sections. This image shows 3 *sections* ( **Apps** , **Docks** , and **Users** ) within the **Everyone** *team* .


We tried out two different implementations for how we manage the order of pages.


### Initial implementation


Initially, we opted to go with the most obvious approach which was to add an` order` column to our` Page` table that would contain the index of the page within a section.


```text
model Page {
id Int
sectionId Int
order Int
}
```


Here’s a look at how the page order value would look like for sidebar items:


This implementation proved to be difficult to work with because when you re-order a page, you cannot simply change the` order` value for a single page in the database, but rather you must update the order of multiple pages so that there are no gaps between numbers.


You can imagine that if I were to move a page with an` order` of` 0` to an` order` of` 2` , I would need to write a database query that looks like the following:


```text
UPDATE   "Page"   SET   order   =   order   -   1    WHERE   order   >   0   AND   order   <=   2   AND   "sectionId"   =   XXX;


UPDATE   "PAGE"   SET   order   =   2   WHERE   id   =   XXX;
```


The SQL is slightly different if having to move a page from` order` 2 to` order` 0.


```text
UPDATE   "Page"   SET   order   =   order   +   1    WHERE   order   >=   0   AND   order   <   2   AND   "sectionId"   =   XXX;


UPDATE   "PAGE"   SET   order   =   0   WHERE   id   =   XXX;
```


The above is only if you consider a page moving to a different order within the same section. Things get more complicated when you considered that pages could be moved to a different section because then you would need to update all the pages in two different sections.


These update statements are also not performant—every reorder causes multiple records to be updated, sometimes requiring *every* page in a section to be updated. Even worse, we have an index on the` order` column, which means that updates are even slower.


We had also added a database level constraints to make sure there can only be a unique combination of` sectionId` and` order` to make sure there cannot be the same order for a page in a given section, but ran into many edge cases where we were trying to set the same order for a given section. We also found out that` null` values in postgres are treated as unique, which means that if one row has a` sectionId` of` null` and and` order` of` 1` and another row has the same value for` sectionId` being` null` and` order` being` 1` , the unique constraint between` sectionId` and` order` will pass because one` null` value compared to another` null` is seen as different.


So as you can see, this approach was complicated and it proved hard to get to work properly.


### Final implementation


The implementation we decided to ultimately use implements floating point numbers for the order in the database.


```text
model Page {
id Int
sectionId Int
order Float
}
```


Using floating point values for the` order` in the database allows us to move a page between two other pages and determine the new` order` value by averaging the` order` values for the surrounding pages. This means that we only need to update a single record: the one being moved.


As an example, if we were to insert a page between one that has an order of` 1` and` 2` , then the inserted page would have an order of` 1.5` (` 1` +` 2` /` 2` )


One thing to be aware with this approach is that because floating point numbers have limited precision, we will eventually hit a floating point rounding situation, which will give us an incorrect value for the order. As an example, if we had a page with` order` of` 1000` and another page with order of` 1001` and repeatedly kept moving pages to the position right after the page with` order` of` 1000` , we would run into issues on the 38th iteration due to floating point rounding.


Number of iterations before floating point rounding occurs when starting with an order of 1000 and 1001 (source:[https://begriffs.com/posts/2018-03-20-user-defined-order.html](https://begriffs.com/posts/2018-03-20-user-defined-order.html) ).


38 iterations is still a lot and it seems unlikely in Basedash’s case that someone will repeatedly try to move a pages in such a way that this threshold will be reached, so we aren’t too concerned with the approach. If we wanted to resolve this, we could occasionally “re-balance” the order values, aligning them along a single order of magnitude.


To walk through the flow from front-end to back-end, it looks like this:


1. User moves a page with order` 4` to the position between pages` 1` and` 2`
2. The client-side state is updated for the page being moved to have a new value for` order` of` 1.5`
3. An API call is made to the server that indicates that the page should have an` order` of` 1.5`
4. A database query is made to update the page with the new` order` of` 1.5`


This is a nice and straightforward approach that doesn’t involve having to update the order of many pages when moving a single page (like the initial implementation required).


### Possible improvements to ordering at the database level


According to[this article](https://begriffs.com/posts/2018-03-20-user-defined-order.html) , an even more robust approach than the floating point method described above is to store “true fractions” in your database. Read the article if you’d like to find out how to do it. It requires installing an Postgres extension to get a new column type, but it seems like you can go have many more orders of magnitudes more list re-order operations before you run into any sort of rounding issues.


### How the drag and drop re-ordering was implemented on the client


We used the[dnd-kit library](https://dndkit.com/) to handle the drag and drop re-ordering. Native drag & drop APIs are a nightmare to work with, so using a library like dnd-kit is almost necessary in my opinion. I’m not going to get into the nitty gritty of how we put together the parts of dnd-kit to form the drag & drop behaviour since the code is fairly complex. The code complexity is because there is a lot going on with drag & drop re-ordering, such as:


- Highlighting the section for which the page is being dragged in
- Re-ordering items “on the fly” as you drag
- Showing a preview of where the element will be “dropped” using a[<DragOverlay>](https://docs.dndkit.com/api-documentation/draggable/drag-overlay)
- Update both the local state of draggable items as well as the` page` entity in the global store with an updated` order`
- Figure out what new` order` value should be used and all the possible edge cases (e.g. what if moving a page to the end/beginning of a list?)


---


I have a much greater appreciation for the effort it takes to incorporate ordering capabilities into an app now. If anyone knows of a simpler way that this could have been implemented, I’d be curious to hear. You can reach out to me on Twitter[@RobertCooper_RC](https://twitter.com/RobertCooper_RC) .


### Summary


Learn about the challenges faced by Basedash in implementing re-ordering at the database level, and the solutions they tried out. Discover how they ultimately used floating point numbers to move a page between two other pages, and the possible improvements to ordering at the database level.


### Title Tag


Implementing Re-Ordering at the Database Level: Our Experience


### Meta Description


Discover Basedash’s experience in implementing re-ordering at the database level, the challenges they faced, and the solutions they tried out. Learn how they ultimately used floating point numbers to move a page between two other pages, and the possible improvements to ordering at the database level.
