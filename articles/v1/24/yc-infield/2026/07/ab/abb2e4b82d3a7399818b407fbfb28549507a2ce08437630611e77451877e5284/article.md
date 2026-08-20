---
schema_version: "1.0.0"
document_id: "abb2e4b82d3a7399818b407fbfb28549507a2ce08437630611e77451877e5284"
company_key: "yc-infield"
company: "Infield"
source_id: "yc-infield-news-import-3dde9d4b3c10"
canonical_url: "https://www.infield.ai/post/migrating-from-rest-client-to-faraday"
published_at: null
first_seen_at: "2026-07-25T09:33:54.576450+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:c906eb654b7d2b20a5719dc704ccdab30578597ef4544c18c899f887223e132a"
---

# Migrating from rest-client to faraday

One of the components of dependency management that can sometimes be less than straightforward is removing dependencies that are no longer maintained, or abandoned. At Infield we try to assess the “abandonedness” of a dependency based on a few different criteria, like release cadence and Github chatter asking whether this package is being maintained. Recently, while I was removing and replacing the` rest-client` gem I ran into a feature that isn’t present in its most common replacement.


First,[rest-client](https://github.com/rest-client/rest-client) 2.1.0 was released in 2019, and has been in maintenance mode, receiving only minor changes in the last 6 years. It's a prime candidate for being replaced with something that receives active support, like[faraday](https://github.com/lostisland/faraday) . Faraday has a helpful[guide](https://lostisland.github.io/faraday/#/getting-started/rest-client-migration) in their docs to help with this exact migration, which is very handy, but it of course can't cover everything. One convenient pattern from` rest-client` that didn't have a direct Faraday replacement for in their guide was the ability to use` \[\]` to construct sub-resources to arbitrary depth:


` site = RestClient::Resource.new('http://example.com')`


` # => #<RestClient::Resource @url="http://example.com">`


` posts = site\['posts'\]`


` # => #<RestClient::Resource @url="http://example.com/posts">`


` first_post = posts\['1'\]`


` # => #<RestClient::Resource @url="http://example.com/posts/1">`


` comments = first_post\['comments'\]`


` # => #<RestClient::Resource @url="http://example.com/posts/1/comments">`


‍


This opens up a pattern where you can store your root url into a constant or method that can be built on top of because` \[\]` returns another` RestClient::Resource` object, with the new path tacked on:


‍


` RESOURCE = RestClient::Resource.new('http://example.com')`


` def create_record_resource`


` RESOURCE\['record/create.json'\]`


` end`


‍


Those resources are then used in equally simple method calls:


` def create_record(phone_number)`


` create_record_resource.post(default_params.merge(id: @record.id, phone_number:))`


` end`


‍


This usage of` \[\]` isn't built-in to Faraday, so changes have to be made. I started by converting the methods that called` RESOURCE\['some/path'\]` into methods that just returned the path. Then, I could easily move those methods into the calls to` post` and` put` like so:


` def create_record_path = 'record/create.json'`


` def create_record(phone_number)`


` RESOURCE.post(create_record_path, default_params.merge(id: @record.id, phone_number:))`


` end`


‍


Take reducing duplication one step further you can use metaprogramming to write` RESOURCE` and merge` default_params` in one place:


` %i\[ post put \].each do |verb|`


` define_method(verb) do |url, **body|`


` RESOURCE.send(verb, url, default_params.merge(body))`


` end`


` end`


‍


This bit of ruby magic lets us preserve the simplicity of rest-client's` \[\]` , with the security of an actively maintained gem like` faraday` :


` def create_record(phone_number)`


` post create_record_resource, id: @record.id, phone_number:`


` end`


‍


‍
