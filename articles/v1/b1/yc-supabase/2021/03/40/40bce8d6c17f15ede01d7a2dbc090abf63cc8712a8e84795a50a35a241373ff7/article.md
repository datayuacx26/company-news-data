---
schema_version: "1.0.0"
document_id: "40bce8d6c17f15ede01d7a2dbc090abf63cc8712a8e84795a50a35a241373ff7"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/using-supabase-replit"
published_at: "2021-03-11T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:6cdb78703305163d6b3d1f7fb498c23aea758bbe7259f4c73a6eebf77df68793"
---

# Using Supabase in Replit

[Replit.com](http://replit.com/) is an awesome new browser based IDE where you can code alone or collaboratively with friends using their awesome multiplayer features! It's particularly useful for education, and sharing code examples with others.


They support a ton of different languages and execution environments and even recently introduced a simple key value store you can use to persist data.


As a Replit user, if you want to access larger amounts of data direct from your repl, or if you fancy accessing some super-powerful query tools, at some point you may want to start interacting with a relational database. Supabase is a good fit here; just like Replit, you don't need to worry about servers, and hosting, you can just click a few buttons and get a fully featured relational database which you can start communicating with directly from javacript, using supabase-js.


Here's how to start a Supabase + Node.js repl:


Sign up for[replit.com](http://replit.com/) and hit new repl in the top left


Select node.js, give it a name, and click Create repl


Import supabase's createClient method and hit run to install the required libs:


`
_10


const { createClient } = require('@supabase/supabase-js')


`


Setup a new Supabase project and grab the URL and anon key from Settings > API. Create the client in javascript using:


`
_10


const supabase = createClient(


_10


'https://ajsstlnzcmdmzbtcgbbd.supabase.co',


_10


'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...'


_10


)


`


Now that supabase is connected you'll want to add some data to your db, you can grab any SQL dataset on the web, or make your own, but the fasted way to test is to open the SQL tab in the Supabase dashboard and click the Countries sample database and click Run.


From within your repl you can now query your countries table like:


`
_24


// .then() syntax


_24


supabase.


_24


.from('countries')


_24


.select('*')


_24


.limit(5)


_24


.then(console.log)


_24


.catch(console.error)


_24


_24


// or...


_24


// async/await syntax


_24


const main = async() => {


_24


const { data, error } = supabase


_24


.from('countries')


_24


.select('*')


_24


.limit(5)


_24


_24


if (error) {


_24


console.log(error)


_24


return


_24


}


_24


_24


console.log(data)


_24


}


_24


main()


`


Once this is working, if you want to learn more about the[query interface](https://supabase.com/docs/reference/javascript/filter) you might want to try some of these challenges:


`
_10


// 1. List all the countries in Antarctica


_10


// 2. Fetch the iso3 code of the country with ID 3


_10


// 3. List the countries with 'Island' in the name


_10


// 4. Count the number of countries that start with 'Z' or 'Q'


_10


// 5. Fetch all the Countries where continents is null


`


There are full solutions provided in the video version of this blog, but some examples you may find useful are:


`
_22


// or


_22


const { data, error } = await supabase


_22


.from('cities')


_22


.select('name, country_id')


_22


.or('id.eq.20,id.eq.30')


_22


_22


// is


_22


const { data, error } = await supabase.from('cities').select('name, country_id').is('name', null)


_22


_22


// in


_22


const { data, error } = await supabase


_22


.from('cities')


_22


.select('name, country_id')


_22


.in('name', \['Rio de Janeiro', 'San Francisco'\])


_22


_22


// neq (not equal to)


_22


const { data, error } = await supabase


_22


.from('cities')


_22


.select('name, country_id')


_22


.neq('name', 'The shire')


_22


_22


// full docs here: /docs/reference/javascript/filter


`


We look forward to showing off some more Supabase + Replit examples.


You can find my example repl here:[https://repl.it/@awalias/supabase-test#index.js](https://repl.it/@awalias/supabase-test#index.js)


Supabase has a Free Plan, head over to[https://supabase.com/dashboard](https://supabase.com/dashboard) to get started.
