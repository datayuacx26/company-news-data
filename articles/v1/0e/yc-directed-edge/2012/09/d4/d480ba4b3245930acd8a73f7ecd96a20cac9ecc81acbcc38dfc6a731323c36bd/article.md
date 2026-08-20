---
schema_version: "1.0.0"
document_id: "d480ba4b3245930acd8a73f7ecd96a20cac9ecc81acbcc38dfc6a731323c36bd"
company_key: "yc-directed-edge"
company: "Directed Edge"
source_id: "yc-directed-edge-rss-7c8e8fe81473"
canonical_url: "https://blog.directededge.com/2012/09/18/shopify-app-updates-new-ruby-bindings-beta-new-web-services-features-coming-down-the-pipe/"
published_at: "2012-09-19T03:12:11+00:00"
first_seen_at: "2026-07-27T01:55:50.875550+00:00"
fetched_at: "2026-08-20T02:30:11.821086+00:00"
content_hash: "sha256:baad8429c34806e7dfd768a6a23a1c9c662a96f79699124abdaf3d44ba92e0c7"
---

# Shopify app updates, new Ruby bindings beta, new web services features coming down the pipe

#### Shopify app updates


So, it’s been a gazillion years since we posted updates here, but there have been a number of things shaking out of the woodwork.


First, we just did the biggest update to[our Shopify app](http://apps.shopify.com/directed-edge-product-recommender) since we first launched it a couple years back. Â It features a new[Bootstrap](http://twitter.github.com/bootstrap/) -ified configuration interface and a whole bunch of new recommendations types.


#### New Ruby Bindings in Beta


Whilst working on our Shopify app, which itself is a Rails app, we got frustrated with the current state of our Ruby bindings.


You see, our Ruby bindings were written back in the dark days before we actually used Ruby at all inside of the company. Â Truth be told, the original version was written by yours truly a couple hours after I started learning Ruby.


These days, along with Java and C++ (which our lower-level, performance critical bits are written in), we write quite a bit of Ruby as as such, our tastes have become more refined.


Here are a couple of quick examples:


**Old:**


```text
require 'rubygems'
require 'directed_edge'


DE_USER = ENV['DIRECTEDEDGE_TEST_DB']
DE_PASS = ENV['DIRECTEDEDGE_TEST_PASS']


# Import some data


DE_FILE = 'temp.xml'


exporter = DirectedEdge::Exporter.new(DE_FILE)


(1..10).each do |i|
item = DirectedEdge::Item.new(exporter.database, "item_#{i}")
item.add_tag('item')
item.link_to("item_#{rand(10) + 1}")
exporter.export(item)
end


exporter.finish


database = DirectedEdge::Database.new(DE_USER, DE_PASS)
database.import(DE_FILE)


# Get recommendations


item = DirectedEdge::Item.new(database, 'item_1')
item.related([ 'item' ]).each { |i| puts i }


# Update item


item.add_tag('foo')
item.save
```


**New:**


```text
require 'rubygems'
require 'directededge'


DE_USER = ENV['DIRECTEDEDGE_TEST_DB']
DE_PASS = ENV['DIRECTEDEDGE_TEST_PASS']


database = DirectedEdge::Database.new(DE_USER, DE_PASS)


DirectedEdge::UpdateJob.run(database, :replace) do |update|
(1..10).each do |i|
update.item("item_#{i}") do |item|
item.tags.add('item')
item.links.add("item_#{rand(10) + 1}")
end
end
end


# Get recommendations


item = DirectedEdge::Item.new(database, 'item_1')
item.related(:tags => :item).each { |i| puts i }


# Update item


item.tags.add('foo')
item.save
```


The changes are most obvious in the importer above, but there are a lot of little subtle improvements in the API’s semantics.


If you’re up for beta testing the new bindings,drop us a line . Â The documentation is sorely lacking at present (ahem, meaning non-existent), but that’s the main thing that we know of that’s missing at the moment: Â the new bindings are already in use by our Shopify app and the[code is up on Github](https://github.com/directededge/directed-edge-bindings/tree/v1/Ruby) .


#### Web services updates on the way


In a similar vein, we’ve got some stuff that we added to the web services for the Shopify app that still have some rough corners that we want to smooth out before pushing them out to the unwashed masses, but we’ll soon have explicit API support for tracking product view histories and showing recommendations based on them. Â It’s been possible to do such in a hackey way in the past, but the new stuff adds some special math-fu for time-clustered recommendations and storing high-traffic queues of updates. Â More on that soon.
