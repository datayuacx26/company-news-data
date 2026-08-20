---
schema_version: "1.0.0"
document_id: "c2396129e232f0d3c5c6e7e1d8c292a5ecd62ad176c4f111426243822ade49fc"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/visualizing-supabase-data-using-metabase"
published_at: "2022-06-29T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:02d4cea90bee8208614da2899d3dd86af3bb6afc016ef40a1606402c8164524c"
---

# Visualizing Supabase Data using Metabase

Data helps organizations make better decisions. With a programming language like Python to analyze your data and transform data into visual representations, you can effortlessly tell the story of your business. One way to create customized visuals from your data would be to use data visualization libraries in Python like[Matplotlib](https://matplotlib.org/) ,[Seaborn](https://seaborn.pydata.org/) ,[Ggplot2](https://ggplot2.tidyverse.org/index.html) ,[Plotly](https://plotly.com/) , or[Pandas](https://pandas.pydata.org/) . When you want to accomplish this task with little or no code (not even SQL), you might consider using tools like[Metabase](https://www.metabase.com/) .


With Metabase, a powerful visualization tool, you can quickly turn your data into easy-to-understand visuals like graphs, pie charts, flow diagrams, and much more. Then, using Metabase’s intuitive interface, you can cut through the data noise and focus on what’s essential for your business.


In the previous blog of this series, we explained[how to use Python to load data into Supabase](https://supabase.com/blog/loading-data-supabase-python) . In this blog, we will create different kinds of charts out of the data stored in Supabase using Metabase.


## Prerequisites#


Before we dive in, let’s look at some prerequisites that you will need:


- **Supabase project with data**


Based on our[previous article](https://supabase.com/blog/loading-data-supabase-python) , we assume we already have a Supabase project setup and have data loaded into it.


- **Metabase Docker Container**


To take advantage of the open-source version of Metabase, you can use the Metabase docker container[here](https://hub.docker.com/r/metabase/metabase) .


## Visualizing data in Supabase with Metabase#


### Launching Metabase#


To launch Metabase, simply go to[http://localhost:3000/setup/](http://localhost:3000/setup/) which is the default port that the Metabase server will be listening to.


After Metabase is launched, select your preferred language and add your contact information. In the *Add your data* markdown, you will need to choose PostgreSQL.


You will be prompted to add the necessary connection information to your Supabase project. Go to your[Supabase project](https://supabase.com/dashboard/) and hit *Settings > Database to get the database info* .


Enter the necessary information on Metabase and hit next. Finally, select your data preference, after which you will land on the Metabase homepage.


### View Database and Tables#


We can now see the "Supabase DB" Supabase project under "Our data".


To view the tables, go to *SupabaseDB > public*


### View Table Data Insights#


Go back to the home page and select public schema under "Try these x-rays based on your data"


Here is the output of the product table.


As you can see, we can get some handy information from this like:


- How many products are present with a given range of inventory count.
- How many products are present for a given range of price.
- The ratio between the number of employees to the number of products.
- How many products each vendor has created.


If you have column-specific views, you can select the *zoom-in* option under *More x-rays* .


For example, let's select the total employees field.


With information like this, you will be able to answer some key questions like


- What are some common statistics for company employees like average, minimum, maximum, and standard deviation?
- What is the distribution of the employees across different geo locations?
- What is the distribution of the employees across different vendors?


## Using custom SQL queries#


We can also use custom queries to set up our dashboards. To do this, go to *New > SQL query.*


Next, under the database, select "SupabaseDB".


We will be using the following SQL query:


`
_10


select "Vendor".vendor_name, product_name, "Vendor".total_employees


_10


from


_10


"Product"


_10


left join "Vendor" on "Product".vendor_id = "Vendor".vendor_id


_10


where "Vendor".total_employees;


`


This query should fetch us the vendor name and the product where the number of employees for a given vendor is less than 110.


To run the SQL query, hit the play button.


This will be shown below in the output window. To visualize the data, hit the visualization button.


Next, select the type of visualizer you want. Let us choose *Bar* .


Choose the appropriate x-axis and y-axis fields, and you will be able to view the data in bar format.


## Conclusion#


Data visualization empowers organizations to turn unused data into actionable insights, leading to faster and better decision-making. Why wait?


With our[Free Plan Supabase account](https://supabase.com/dashboard/) , you can start a new project today and use Metabase to visualize your app data.


If you have any questions please reach out via[Twitter](https://twitter.com/supabase) or join our[Discord](https://discord.supabase.com/) .


## More Python and Supabase resources#


- [supabase-py](https://github.com/supabase-community/supabase-py)
- [Slack Consolidate: a slackbot built with Python and Supabase](https://supabase.com/blog/slack-consolidate-slackbot-to-consolidate-messages)
- [Supabase-py (Database) on Replit](https://replit.com/@Supabase/Supabase-py-Database?v=1)
