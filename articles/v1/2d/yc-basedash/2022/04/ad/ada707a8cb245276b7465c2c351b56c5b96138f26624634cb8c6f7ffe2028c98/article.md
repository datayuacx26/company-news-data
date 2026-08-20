---
schema_version: "1.0.0"
document_id: "ada707a8cb245276b7465c2c351b56c5b96138f26624634cb8c6f7ffe2028c98"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-virtualization-increased-our-table-performance-by-500/"
published_at: "2022-04-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:ef3cd5057e4f10d5969602136b845fe43921bf0fe7b673118b650771c8a45ff7"
---

# How virtualization increased our table performance by 500%

At Basedash, we work with tables a lot. The core of our app revolves around rendering large amounts of data from your SQL database in an editable table, complete with unique rendering by data type, and and supporting real-time collaborative editing (multiplayer).


As we continued adding new functionality to our table component, we started noticing some serious performance issues with loading tables. I was tasked with addressing these issues and getting the app to feel performant again.


After all is said and done, we managed to **improve table load time by 4-5 times** ! Here’s how we did it.


### The problem


When first loading a table of data, the page would show a loading spinner and would “lock” up for a while. Depending on the number of columns we needed to load for a particular table, this could take well over *10 seconds* to load. Some of our users that had in the realm of 100’s columns would simply not be able to load their data.


The root cause of the problem was that we were trying to render the entire table at once, even if most of the data for the table was off the screen/viewport. Also, the React code for rendering a single table cell was quite inefficient, so when we needed to render thousands of table cells on initial load, all those inefficiencies compounded.


### The solution


The very first thing that we did ended up **halving** the load time: **Reducing the amount of rows we rendering** from 50 down to 25. It was a quick win, but a little unfortunate since the user saw fewer rows per page. This proved to only be a temporary change.


Next, we started addressing the problem of table cells being expensive to render.


One thing we noticed was that we were doing an array` .find` to look up a value in our global store. This meant that we were iterating over a possibly massive array of items to get a value, and we were doing that for *every* table cell. We refactored this code to instead **use a lookup** within an object and that resulted in an approximately **45% improvement** in performance!


[https://github.com/Basedash/Basedash/pull/716/files](https://github.com/Basedash/Basedash/pull/716/files)


We continued to try to optimize the table cell performance, but didn’t have any more big performance wins. For example, we noticed that we were rendering foreign key popover components (each foreign key cell renders a popover component) even if they were not open. We optimized things so that we would **only render the popover when it is open** . That did not result in a noticeable performance improvement , but the react devtools profiler did confirm that we **shaved off some 50-100 ms** of render time. Notice how the` ForeignKeyPopover` no longer shows up as being rendered in the devtools screenshot after the change was implemented.


This is a foreign key popover in Basedash. It’s a popover that allows you to select what value you would like to use for a foreign key cell.


This is the react devtools analysis of our table cell rendering time **before** our improvements.


This is the react devtools analysis of our table cell rendering time **after** our improvements.


Pull request to prevents rendering foreign key popovers if they are not open.


[https://github.com/Basedash/Basedash/pull/728](https://github.com/Basedash/Basedash/pull/728)


After exhausting our options in optimizing our table cell, we knew that we needed to go ahead and implement something drastic that would rethink how we render out our table. We knew we had to implement **virtualization** .


### Virtualization


Virtualization is a technique used to only render out HTML elements to the DOM if they are within the user’s viewport. In other words, you wouldn’t render out HTML for elements that appear outside of the visible bounds of the user’s viewport. This allows for better performance since you are not unnecessarily rendering elements that aren’t being viewed by the user. If the user scrolls, you would render HTML “on-demand”.


We also briefly talked about perhaps using HTML canvas for our table, but that seemed like a very drastic change that would require a lot of work. It seems like working with a canvas-based table would require us to move more logic and code away from “react-land” in order to manage the UI and state of the table and it seemed like that would be difficult to manage.


In order to get virtualization working with our table, we needed to change the HTML structure of our table. We were using native HTML` table` elements (e.g.` <table>` ,` <tbody>` ,` <tr>` , etc.), but had to move away from using those in order to be able to render table cells “on-demand”. We were a bit bummed to have to do that since our table HTML elements were no longer “semantic” and we couldn’t benefit from the browser’s ability to properly size elements within a table. See the end of this post where I talk more about some issues with column sizing that was caused by moving away from native` table` elements.


We started out by trying to use the[react-window](https://github.com/bvaughn/react-window) library. We were able to get the table working, but the code to have sticky columns and headers was a mess, and we couldn’t get column resizing to work very well. We then tried out[react-virtual](https://github.com/TanStack/react-virtual) which we liked much better since it gave us total control over the underlying HTML elements, and so it was easier to implement sticky columns/headers and resizable columns.


Once we got the first implementation of table virtualization live in front of our users, we were able to **increase the page size from 25 to 100** without any noticeable change in initial load time. In fact, we tested out loading 1000 rows and it also didn’t result in a very noticeable change in load times. However, it could result in a noticeable increase in the amount of data that we send via an HTTP request, and most of the time users don’t need to view more than 100 records.


Our initial implementation of table virtualization helped reduce initial table load time **dramatically (in the scale of 4 times improvement for most tables)** , however we were encountering **significant jankiness** while scrolling a table. I was a bit demoralized since I thought this meant we needed to optimize the performance of the` TableCell` even further, which would be difficult since we couldn’t find any more low-hanging fruit. Thankfully, we found out that the problem with the scroll jankiness wasn’t primarily the time it took to render a` TableCell` , but rather that we were constantly re-rendering existing table cells upon scrolling.


The highlights show how often each table cell is re-rendering as the user scrolls.


The reason that the re-rendering was happening was because we were passing a` style` prop down to each cell which was an object whose value changed on scroll. The` style` is used to determine the placement of the table cell in the virtualized table. This re-rendering issue was resolved by no longer passing down a` style` prop, and instead, wrapping the table cell component with a` div` where we pass the` style` to the wrapper element.


After optimizing the virtualization logic, you can see that we are now only rendering cells that are newly showing up in the viewport and not re-rendering already visible cells.


Pull request that resolves the scroll jankiness issue.


Code diff that shows how we fixed the scroll jankiness issue.


[https://github.com/Basedash/Basedash/pull/837/files](https://github.com/Basedash/Basedash/pull/837/files)


### Existing issues we have with our table


Our table isn’t perfect. With the introduction of virtualization, some things are more difficult to do. Here’s a list of some existing problems we still need to figure out:


**Column resizing**


When resizing a column, we don’t re-render the table as you drag so it’s a bit difficult to gauge what the ideal size the column should be while dragging the column resizing handle. We tried re-rendering as you drag, but ran into an issue with how this could work with sticky columns. so ended up with a solution where we only re-render after the user releases the resizing handler.


Notice how the user can’t know if the column will be the right size while they are dragging the resizing handle. It is only after the user releases the resizing handle will they know if the modified column size will properly fit the contents of the column.


**Auto-resizing of columns**


Without rendering the entire table all at once, it’s difficult to figure out what size the columns should be set to upon initial load. We used to have the ability to automatically set the column widths that would allow for the best viewing of all your data based on the table contents, but that is now more difficult to do now that we are no longer using` <table>` HTML elements, and all the table content is virtualized.


**Cell focus**


We have an issue where if a user focuses on a cell, then scrolls the cell out of the viewport, then the cell will lose focus. That is because when the user scrolls the cell off the screen, the HTML for that cell will be removed from the DOM, and so the focus also gets lost.


In the future, we want to move away from relying on the browser focus management for our table, which will allow us to better manage focus in our table.


**Keyboard navigation**


If trying to navigate the table using the keyboard, it works fairly well except for some edge cases. For example, if you have focus on the very first cell in a table and want to move focus to the cell in the last column (which is currently out of the viewport), then we need to be able to quickly render out the HTML for the end of the table and then move focus to the cell in the last column. We have code that tries to do this and it works *sometimes* , but not always. The code is quite hacky so that could explain why it is unreliable.


## Results


Over all, after implementing both virtualization and improvements to our table cell, we were able to speed up table load times by 4-5x in most cases, and over 10x in extreme cases. All while increasing the default page size from 50 rows to 100.


If you’re interested in playing around with our newly virtualized table UI, you can sign up for Basedash and join our demo workspace. We’re building a tool that lets you view and edit your database with the ease of a spreadsheet. On top of that, you can build views of your data and share them with your teammates to give them limited read/write access to certain tables.


If you’re interested, you can sign up here:[https://charts.basedash.com/signup](https://charts.basedash.com/signup?email=)
