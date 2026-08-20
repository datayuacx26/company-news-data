---
schema_version: "1.0.0"
document_id: "0daba0decd47c5137312308d20e7e269153f81d57e08def4dd20a31a8f2b851c"
company_key: "upland-software-inc-common-stock"
company: "Upland Software Inc."
source_id: "upland-software-inc-common-stock-rss-53a5709ba49e"
canonical_url: "https://olresourcecenter.uplandsoftware.com/blog/dynamic-sheet-names/"
published_at: "2024-07-29T11:52:21+00:00"
first_seen_at: "2026-07-26T13:25:40.115128+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:4d6ba7c56349f1dcf045f61b09bd4f3dc522b6b9f64cc22f6deca76fff198663"
---

# Dynamic sheet names

Starting with version 2024.1, the OL Connect DataMapper allows you to specify a dynamic sheet name for Excel workbooks. You can therefore re-use the same data mapping configuration even when the name of the target Excel sheet changes over time.


## Adapting to the sheet name


By default, the DataMapper uses the first sheet name it finds in the Excel workbook, so you usually don’t have to do anything to select the data. On some occasions, your data might be on a different sheet and the DataMapper allows you to select the proper one via a dropdown list:


There are some cases, however, when the data can either be in one sheet or another. For instance, let’s imagine we have a workbook with two sheets that contain almost identical data: the list of the 10 largest countries in the world. One data sheet is in English under the name *Countries* , while the other is in French under the name *Pays* :


Now in the DataMapper, it’s just a matter of selecting the proper sheet name in the dropdown list, and things will adapt seamlessly. But what if you want to automate the selection of the sheet name? Wouldn’t it be useful to pass a runtime parameter to the DataMapper so that it dynamically picks the proper sheet name?


With OL Connect 2024.1, the DataMapper allows you to specify the sheet name through an expression that can be based on runtime parameters. So for instance, we can create a runtime parameter named` sheet` and assign a debug value of` countries` to it.


Then, in the Input Data settings’ *sheet* option, you can click on the *Dynamic sheet name* button to open up the Dynamic sheet builder dialog, where double-clicking on the` automation.parameters.sheet` property automatically builds the sheet name using the value stored in our runtime parameter.


So when the *sheet* runtime parameter is set to *countries* , the **Countries** sheet is automatically displayed in the Tabular viewer:


Now if we change to parameter to read *pays* , the display immediately switches to the **Pays** sheet in the workbook:


Note that the sheet names are not case-sensitive, so *countries* and *Countries* both point to the same sheet.


## Using the sheet index


Sometimes you can’t know the sheet name because the author of the workbook may decide to rename it on a whim. Or the sheet name is written in Klingon and your command of that language has waned since the end of the *Next Generation* series.


Whatever the reason, you can access the sheet via its index. Just specify a number (starting with 1) in the dynamic sheet name, or set your runtime parameter to that number:


Note that the dynamic sheet name feature always tries to match the sheet name first, and if it doesn’t find it, then it attempts to use the index specified. That means that if, for some unfathomable reason, the first sheet in your Excel workbook is named “2” and the second one is named “1”, then specifying a dynamic sheet name of` 2` will open the first sheet. Yes, that is confusing, but then again, naming your first sheet “2” is also confusing, isn’t it?


## Extracting data by column index


This section isn’t about a new feature in 2024.1 but rather a reminder of a functionality that was added a while back in the DataMapper. You may have noticed in the first screenshots above that our two sheets have different column headings: *Name* and *Area* in English, and *Nom* and *Superficie* in French and yet the next screenshots show the headings as *COLUMN1* and *COLUMN2* . That’s because I wanted to make sure that changing sheets dynamically would not force me to change the field names in the extraction steps.


To that end, I modified the Data settings as follows:


First, the DataMapper is instructed to skip the first line (i.e. the headers) and then it is told that the first row does not include field names. When that happens, the DataMapper automatically assigns indexed column names (column1, column2, etc.) to each column, allowing us to always use those names in our extraction steps, regardless of the sheet we are currently processing.


But we could also have done things in a different way, without having to change any of the data settings. We could have extracted each cell using the` data.extractByIndex()` method, which allows us to target specific cells by their index value rather than by their name. This is a lesser-known feature of the DataMapper that can be quite the life saver when dealing with multiple data sheets in a Workbook.


## Resources


[DynamicSheetNames](https://olresourcecenter.uplandsoftware.com/wp-content/uploads/2024/05/DynamicSheetNames.OL-datamapper)[Download](https://olresourcecenter.uplandsoftware.com/wp-content/uploads/2024/05/DynamicSheetNames.OL-datamapper)


### Share this:


- [Share on Facebook (Opens in new window) Facebook](https://olresourcecenter.uplandsoftware.com/blog/dynamic-sheet-names/?share=facebook)
- [Share on X (Opens in new window) X](https://olresourcecenter.uplandsoftware.com/blog/dynamic-sheet-names/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://olresourcecenter.uplandsoftware.com/blog/dynamic-sheet-names/?share=linkedin)
-


### *Next best reads for you!*


### Leave a Reply[Cancel reply](https://olresourcecenter.uplandsoftware.com/blog/dynamic-sheet-names/#respond)
