---
schema_version: "1.0.0"
document_id: "37bc210b417eac622776eb17e3279ee5025e4fb6219e20194a81b6964c3b75d8"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/cleartax-javascript-data-grid-3ee082afb7c0"
published_at: "2020-10-29T12:32:57+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:e729c9b0f23f2d6746d5a677828e78b80f65a58c39cb508ad1bf7a3d3f0f5bf9"
---

# This in-house solution helped ClearTax save 10,000 man hours!

# This in-house solution helped ClearTax save 10,000 man hours!


[Lofty Khanna](https://medium.com/@loftykhanna?source=post_page---byline--3ee082afb7c0---------------------------------------)


4 min read


·


Oct 17, 2020


--


Press enter or click to view image in full size


Computation of taxes involves a lot of tables for data entry, data visualization, and report downloads. Our ClearTax website is full of such tables. **CTGrid** (ClearTax Javascript Data Grid) is an official table component of ClearTax and is developed inhouse. It is a react component that provides a consistent experience across the entire website.


CTGrid is a wrapper on top of[Handsontable](https://handsontable.com/) . When we mention Handsontable (HOT), two questions are very commonly asked -


**What is Handsontable (HOT)?** Handsontable is a data grid with the features and the look & feel of a spreadsheet. Handsontable is written in JavaScript and works with the most popular frameworks such as Angular, Vue, and React.


**Why CTgrid when you can directly use HOT?**


- Grid or data tables are the main supporting pillars of our business. We, at ClearTax, keep on exploring new grid libraries (until we can develop it on our own 😜 ) and these libraries keep on getting updated. At present, we are using HOT. In the near future, if we want to use another library, we will just have to write a transformer for backward compatibility. No changes would be required at the application level
- At Cleartax, we believe in making simpler fin-tech products. CTGrid has helped us a lot in maintaining consistency across the organization through its features, renderer, UI, etc.
- We were not very satisfied with the API of Handsontable. One can find it confusing (example: column header name needs to be passed as separate config in the same order as column order) and limited (example: configuration for a column is based on the column index, while it is nicer to have configuration based on column identifier). We were able to tune the API of CT Grid to be simpler and more in line with the way we use grids in our product
- CTGrid supports all features by HOT and also certain additional features. In other words, it is not a wrapper but a superset of HOT.


## **Features of CTGrid -**


**Renderer :** Renderers define what to render inside a particular cell or column. CTGrid supports the default renderer provided by HOT along with the following renderers -


- Button Renderer
- Link Renderer
- Tag Renderer
- Money Renderer: It formats the numeric value.
- Custom Render: **Render your own HTML in cells.**
- Conditional Renderer: Renderers in HOT are at the column level. CTGrid allows you to have different types of renderers in the same column conditionally.


**Filters** : In HOT when you add filters, they are added to all columns but in CTGrid you can control for which specific column you want to add filters.


Press enter or click to view image in full size


**Validators :** CTGrid provides inbuilt validators for email, phone number and GSTIN. If someone enters a wrong value it highlights it in red.


Press enter or click to view image in full size


**Select One Radio Button :** We can select one particular record from the grid. It’s a small configuration you need to pass for this. We can also pass a callback function which is executed when a record is selected.


Select Radio Button Config Press enter or click to view image in full size


**Select multiple records :** We can select multiple records or all records from the grid. The callback is executed when we select/deselect all records or multiple records.


Press enter or click to view image in full size


**Theme :** There is no concept of theming in HOT, on other hand CTGrid Support two themes — **Light** and **Excel** . This is done to maintain UI consistency and reduce the efforts of other developers.


Press enter or click to view image in full size


**Displaying Errors :** Sometimes we want to show an error for a particular row or cell but row and cell number keep on changing, so the only thing we rely on for showing errors is data. If a Data object has ‘errorDetails property’, CTGrid would read it and show an error for row or cell. Row-level errors should be resolved before showing cell level error. Below is the contact for error Details:


Press enter or click to view image in full size


## Upcoming features in CTGrid -


**Custom Filters** : HOT’s default filters do not completely solve our purpose/requirement and HOT custom filters are quite confusing to implement. We are building our own configuration based to ease custom filters implementation in CTGrid.


**Inbuilt Pagination** : HOT doesn't provide any pagination, although CTGrid performance doesn't lag for 45000 data rows, still at certain instances it is required to paginate the data.


**More Renderers** : We will be adding more renderers to ease the development and latest UI design implementation.


**More Themes** : To give a more rich user experience, we will be adding more themes.


**CTGrid has helped us save more than 10,000 man-hours and with continued improvements helps us provide a rich user experience across the ClearTax website.**
