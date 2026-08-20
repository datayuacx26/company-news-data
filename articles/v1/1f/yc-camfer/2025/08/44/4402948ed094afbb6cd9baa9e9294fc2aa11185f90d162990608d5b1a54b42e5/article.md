---
schema_version: "1.0.0"
document_id: "4402948ed094afbb6cd9baa9e9294fc2aa11185f90d162990608d5b1a54b42e5"
company_key: "yc-camfer"
company: "camfer"
source_id: "yc-camfer-news-import-eadafe2b34d8"
canonical_url: "https://camfer.dev/blog/designing-cad-interface/"
published_at: "2025-08-14T00:00:00+00:00"
first_seen_at: "2026-07-24T23:30:43.881527+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:fdd7a14ce42b330410ffb660b24239b298bf7f8e7fc2e73ce7018c326ae03311"
---

# A CAD interface for LLMs

Hi, I’m Roth. I wanted to shed a little light on our CAD format because we’ve gone down a very different path compared to what other people are working on. Our format allows us to work with a much larger set of CAD features than existing sketch + extrude approaches. It also (somewhat) easily integrates into existing CAD programs. We hope to release more details + code in the future once our language is more cemented.


## Representing CAD features


This is the least interesting part of our format because it’s been done many times before. We encode CAD into text simply by dumping all of the properties of a CAD feature into a JSON object. For example, a sketch is just a 2D plane and the line segments inside. An extrude would just be a sketch + some options for how far to extrude. Using this idea, we expanded the format to the rest of the CAD features we wanted to support.


Seems easy right? No. We wanted to support features such as chamfers and fillets, but quickly found issues encoding geometric entities. A sketch can be explicitly referenced because it appears earlier in the format. It’s tricky when you want to reference an edge even with a simple sketch + extrude CAD model.


## Geometric references


There were two different approaches we considered here. I’ll start with the one we didn’t decide on.


CAD programs store models in their proprietary format, so we don’t know what exactly happens, but at some point, all these references get turned into text somehow. I haven’t found any information on the internet describing this process, but generally these CAD programs store them as the order of operations needed to create each geometric entity. We call these “topological queries” because it makes these references robust to trival changes.


An example of a topological query in a high level language would be “the edge created by the cap end of an extrude of a sketch that corresponds to this line segment”. You can understand this process better by seeing how KittyCAD is reinventing it[here](https://github.com/KittyCAD/modeling-app/discussions/501) .


We decided to not pursue this idea because it didn’t seem feasible for an LLM to generate these sort of queries once they became more complex. Another limiting factor was that their size isn’t constant, and they were usually very large.


For our approach, we decided on continuing the process of dumping all the properties into JSON again. To be clear, we literally just dump properties of the vertex/edge/face/body into the JSON to represent them. The biggest requirements of this is that your format should be able to differentiate between different entities. You never want two entities to have the same properties. We were very deliberate on how we represented each entity, and our goal was to have the smallest size that still differentiated them.


The biggest negative of this approach is that it’s hard for an LLM to generate the correct properties of any geometric entity since each value had to be exact. To get around this, we added tools for our model to find and select various entities, but it comes with some other side effects. With topological queries, you’re able to create CAD without any environment interaction. Since we’re dependent on tools for selection, generation is dependent on a multi-turn sequence between the CAD environment and the model.


## Tell me more


Reach out (roth at camfer dot dev) if you have any questions or if you wanna work here :^)
