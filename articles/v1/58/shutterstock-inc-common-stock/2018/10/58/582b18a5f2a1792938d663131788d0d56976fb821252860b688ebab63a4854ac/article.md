---
schema_version: "1.0.0"
document_id: "582b18a5f2a1792938d663131788d0d56976fb821252860b688ebab63a4854ac"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2018/10/10/tech-blog-relaunch"
published_at: "2018-10-10T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:ec037d8595bb75cabfaeeec4dc7018594fb2b583079536ca918b5c636c92da86"
---

# Tech Blog Relaunch

Shutterstock turned[15 this year](https://shutterstock.com/blog/15-year-anniversary-posters) and anniversaries like this give us an opportunity to reflect back on (and appreciate) how far we’ve come and where we’re going.


From our origins as a bootstrapped startup to Shutterstock today, we’ve achieved milestones that I had never imagined possible at the outset, for example, a content library of over 200 million images (as well as more than 11 million video and thousands of music clips) and paying out in excess of $600 million dollars to our contributors around the world.


The engine behind this journey is technology and with the relaunch of our blog, we wanted to provide some insight into our tech stack, our engineers, and our thinking around engineering practice and organizational design.


## Quick History in Code


For the first 100,000 images or so, the following code snippet powered search for the site. This implementation was purposefully naïve (based solely on keywords), yet it was perfectly adequate for discovering content given the limited number of images we were licensing at the time.


```text
my    $keywords    =    join    "  ,   "    map    {    $dbh  ->  quote  (  $_  )    }    keys    %  terms  ;
my    $model_clause    =    '';
my    $keyword_clause    =    '';


if    (  $model_released  )    {
$model_clause    =    "  AND (model_released = 1 OR model_released = 2)  ";
}


if    (  $keywords  )    {
$keyword_clause    =    "  k.keyword IN (  $keywords  ) AND  ";
}


my    $sql    =    "  SELECT k.id FROM keywords k, photos p WHERE   $keyword_clause   k.id=p.id   \
AND p.approved=1 AND p.cat1 != 19 AND p.cat2 != 19 AND p.cat3 != 19   \
AND p.cat1 != 20   $model_clause  ";


my    $search_sth    =    $dbh  ->  prepare  (  $sql  );
$search_sth  ->  execute  ;


while    (  my    (  $id  )    =    $search_sth  ->  fetchrow_array  )    {
$matches  {  $id  }  ++  ;
}


for    my    $id    (  sort    match_sort    keys    %  matches  )    {
push    @photos  ,    {    id    =>    $id    };
}


return    \  @photos  ;


```


This is what it looked like on the original Shutterstock website:


Fast forward to today—we have one of the largest, curated image data sets in the world and that gives us a nearly unparalleled opportunity to be at the forefront of computer vision innovation and research. In order to surface relevant content to our customers from our vast trove of images, our search code has become increasingly complex and powerful, now at over 2.2 million lines and leveraging, among many things, the power of convolutional neural networks and other sophisticated deep learning techniques.


## Growth & Evolution


### Technology & Systems


Like any technology company that’s existed for as long as we have, the code that served us well at the beginning begins to serve us less well over time; nothing surprising here—we have the benefit of hindsight, technology marches on, machines get faster, languages get more expressive, markets change—we adapt and evolve. For us, our challenge was to evolve and ensure that the core business remained unscathed while we rebuilt and/or replaced nearly every part of the tech stack to support our growth and scale.


Underneath the hood and for the last few years, we’ve been transforming the Shutterstock tech stack from our original Perl-based monolith to a cloud-based, containerized, microservices architecture using a blend of languages including Node, React, Java, Python, etc. This massive undertaking sets the future foundation while unlocking the ability for teams to build at scale. Large parts of the original tech stack carried us through 15 years of growth, but we needed something that could carry the torch even further.


And as with most modern, large-scale software systems, this transformation relied on open source software up and down the stack. In recognition of the value of OSS, we are strong supporters of the OSS community (for example, we host the[NYC Node.js Meetup](https://www.meetup.com/nodejs/) at our headquarters in the Empire State Building) and encourage a culture of engineers[contributing to](https://github.com/fabricjs/fabric.js) and releasing[open source projects](https://github.com/shutterstock) .


### People & Teams


In the early days of the company, engineering teams, like our systems, were monolithic and undifferentiated, largely because of the early stage of business/technical maturity—we valued the ability of generalists to solve problems of all sorts, from network engineering all the way up to front-end development. Greater technical maturity has meant that the “easy” problems had been tackled, and we were faced with more challenging problems that require specialized teams and individuals to solve.


To that end, we reconfigured the engineering org this way, implementing agile practices and operating on a synchronized 2-week sprint cadence, pushing releases through our automated CI/CD pipeline dozens of times a day. We move quickly to test hypotheses, push new features, and make data-driven product decisions that improve customer experiences and conversion. This means we sometimes break things, but we accept the cost as part of our relentless pursuit of continuous improvement.


As part of our commitment to building and cultivating a best-in-class engineering organization, we’ve doubled-down to support long-term career growth at Shutterstock by launching a robust, transparent career track framework earlier this year across the org. This has brought much needed clarity to the expectations we have at every level of individual contributor and people leadership tracks while aligning with individual specializations and career objectives. We’ll write more about what we’ve done on this in a future post.


## Looking Forward


I’m proud of what we’ve accomplished together over the last 15 years but am most excited about the challenges we have yet to solve and the progress we’ll make in the next 15. I hope you’ll follow us on that journey.
