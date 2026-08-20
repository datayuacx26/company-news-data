---
schema_version: "1.0.0"
document_id: "bae59afa348e6b36be90c83a5451d692faaeb839452ecc7ae329ab5a8950556c"
company_key: "yc-directed-edge"
company: "Directed Edge"
source_id: "yc-directed-edge-rss-7c8e8fe81473"
canonical_url: "https://blog.directededge.com/2011/03/15/the-easiest-way-to-add-recommendations-to-your-rails-app-announcing-acts_as_edgy/"
published_at: "2011-03-15T18:17:44+00:00"
first_seen_at: "2026-07-27T01:55:50.875550+00:00"
fetched_at: "2026-08-20T02:30:11.821086+00:00"
content_hash: "sha256:3911cdd3e7924bd6964ef3e511429ac9efa7173bd41298373f25bf7900c8732d"
---

# The easiest way to add recommendations to your Rails app, announcing acts_as_edgy

So, there are two things I want to talk about. The first is how our new beta hotness, acts_as_edgy, just made it super easy to add Directed Edge recommendations to Rails 2 apps. How easy? One line easy.


### Recommendations the Easy Way


Let’s suppose for example that you’ve got a model called User and another called Product. And let’s also suppose that you support “likes” in your app, so you’ve got a model called Like too that just maps from a user ID to a product ID. Now, you want to show people personalized recommendations when they log in and related products when they’re viewing product pages. Nothing too exotic there.


So, here’s what you add to your app in that case:


```text
class User < ActiveRecord::Base
acts_as_edgy(:like, Like, Product)
...
end


```


The acts_as_edgy line is the whole shebang. That's it. After that's in there you've got rake tasks that let you do rake edgy:export and it'll push your data over to our servers. You can also have it automatically update your data with our web service on saves to your model. Did I mention it's easy?


Okay, okay. So, yeah, I skipped the part about where you actually do something with the recommendations. But that's not rocket surgery either. Now that you've got that in your model you can do:


```text
User.first.edgy_recommended # returns a list of recommended products based on likes
Product.first.edgy_related  # returns a list of related products based on likes


```


You can do much more complicated stuff too. Our Black Magic figures out the route between models that you provide a list of. One of the apps that we've been testing with is Spree. Let's say that we want to do product recommendations based on sales. In Spree that means that we have to map a User to a Product, but there are a bunch of intermediaries: Orders belong to a User, LineItems belong to an Order, LineItems also belong to a Variant and Variants belong to a Product. Whew. What's that look like in acts_as_edgy nomenclature?


```text
class User < ActiveRecord::Base
acts_as_edgy(:purchase, Order, LineItem, Variant, Product)
# ...
end


```


You just list the models that it has to pass through on its way to its target and it figures out (or, well, tries to, this assumes you've used nice consistent column names, which you of course did, didn't you?) how to get there.


And then, once again, we can just query for related and recommended stuff like it's nobody's business.


You also have access to the regular stuff that our API supports. So if you had both likes and purchases in the same application, that's where that handle sitting up there right at the front of the acts_as_edgy line comes in handy. You can chose how you want to weight those, e.g.:


```text
Product.first.edgy_related(:like_weight => 0.1, :purchase_weight => 0.9)


```


And there you've got recommendations based on some mix and match weights that seem appropriate to you.


I wanted to get the cool stuff out first, but there are naturally a couple of set up steps that have to happen beforehand:


**THIS IS THE PART YOU ACTUALLY HAVE TO READ**


1. **[Sign up for a Directed Edge account](http://www.directededge.com/signup.html)**
2. Install the[directed-edge](http://rubygems.org/gems/directed-edge) and[will_paginate](http://rubygems.org/gems/will_paginate) gems
3. Install the[plugin](https://github.com/directededge/acts_as_edgy)
4. Run rake edgy:configure to enter your login info


There's more on the nuts and bolts of that over on the[Github page](https://github.com/directededge/acts_as_edgy) , and just let us know if you get stuck. But here's a real world example. Wait. I've just realized I lied to you. There are *three* things I want to tell you about.


### We have a Spree plugin.


Yes, yes, we do. Hell, we even have a full *[tutorial](http://developer.directededge.com/article/Getting_started_with_Spree)* on working with Directed Edge and Spree! It only works with Spree 0.11 at present since we haven't ported this baby to Rails 3 yet. Mostly we needed something real to test this thing on, and Spree seemed like a nice way to test with data models that weren't tied to our own assumptions.


You can get at[the full plugin on Github](https://github.com/directededge/acts_as_edgy_spree) .


And here are the guts -- the example I mentioned above:


1. An[initializer](https://github.com/directededge/acts_as_edgy_spree/blob/master/config/initializers/edgy_spree.rb) to add the line above to the User model
2. A[helper](https://github.com/directededge/acts_as_edgy_spree/blob/master/app/helpers/edgy_helper.rb) to figure out if we're doing personalized recommendations, related products or basket recommendations
3. A[partial template](https://github.com/directededge/acts_as_edgy_spree/blob/master/app/views/shared/_edgy_related_table.html.erb) to handle showing the results


That's pretty lean for all of the glue and display code for adding recommendations (and instant updating) to a full-blown e-commerce thingereedoo.


### The nerdy bits.


There are a few neat technical things that are happening behind the scenes to make all of this stuff easy from a user's perspective.


**SQL Generator**


One of them is a fancy custom SQL generator that builds optimal queries for all of this stuff. Entire Rails models get exported with one query. The generated SQL can get hella ugly, but it offloads most of the dirty work to the database rather than having to do it all in Ruby code.


The above Spree example (the User to Product mapping) generates this SQL monstrosity:


```text
select users.id as from_id, 'user' as from_type, variants.product_id as to_id,
case when variants.product_id is not null then 'product' end as to_type,
case when variants.product_id is not null then 'purchase' end as link_type from users
left outer join orders on users.id = orders.user_id
left outer join line_items on orders.id = line_items.order_id
left outer join variants on line_items.variant_id = variants.id
where users.id is not null order by from_id;


```


We build up a thing we call a "connection" which is a number of "bridges". A bridge is just a route between two models, including the foreign keys and so a connection is a full path from one model to another one via a chain of foreign keys. In the simple case this is detected based on the model's name and built in ActiveRecord methods for reporting foreign keys, but you can also specify a bridge manually for foreign keys that are created that don't match the typical nomenclature. That isn't documented at the moment, but shout if that's something that you need.


**Triggers**


Another neat thing is the automatic installation of model triggers. So when it's building that connection mentioned before, our system knows which models trigger updates which need to be sent over to Directed Edge to keep your data in sync.


So if acts_as_edgy is set up to automagically send updates (a config parameter that can be called in the config blog that gets written when you call rake edgy:configure) then as soon as a model changes anywhere along that path, we get the goods. And this triggers a different code path that leads to our SQL generator just pulling out the stuff that needs to be updated in a single query.


**Future: Asynchronous Web-service Calls**


And since those updates are hitting a remote web service, it's ideal if they're not blocking execution. We make liberal use of a class we call Future (conceptually borrowed from[QFuture](http://doc.qt.nokia.com/4.7-snapshot/qfuture.html) from Qt) which executes a block of code in a background thread and only forces synchronization when its data is accessed. Here's what it looks like:


```text
class Future
def initialize(postprocessor = nil, &finalize)
if ENV['EDGY_SYNCHRONOUS']
@data = postprocessor ? postprocessor.call(finalize.call) : finalize.call
return
end
@postprocessor = postprocessor
@future = Thread.new do
begin
finalize.call
rescue => ex
warn "Exception in background thread: #{ex}"
end
end
end


def method_missing(method, *args, &block)
data.send(method, *args, &block)
end


def to_s
data.to_s
end


private


def data
@data ||= @postprocessor ? @postprocessor.call(@future.value) : @future.value
end


```


Since we implement method_missing on the Future class to first block and then forward the call on, you can use it just like a normal value, e.g.:


```text
foo = Future.new { sleep 1; 42 }
puts foo # prints 42


```


Altogether, while weighing in at a slim 382 lines of code the plugin is fairly light, but it's pretty dense code with quite a bit of interesting stuff going on.


It's still quite beta, so we expect there will be a handful of kinks to iron out and there are features we'd still like to add, but hopefully we'll be doing that iteratively as you wonderful folks start building things with it and telling us what you need.
