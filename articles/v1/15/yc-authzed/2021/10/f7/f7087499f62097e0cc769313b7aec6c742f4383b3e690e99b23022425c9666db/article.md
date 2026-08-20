---
schema_version: "1.0.0"
document_id: "f7087499f62097e0cc769313b7aec6c742f4383b3e690e99b23022425c9666db"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/go-ecosystem"
published_at: "2021-10-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:ccbe40ae534d0b9890f1217e28a115ff76c2a7bf5bf8321caed80d024735f727"
---

# We picked Go for the Ecosystem

At Authzed, we're building a platform to manage the permissions built into applications. While services like Okta and PingFederate tackle the question of *who you are* , Authzed tackles *what you can do* . The foundation of solving this problem is[SpiceDB](https://github.com/authzed/spicedb) , our open source graph database that builds upon the design expertise used to solve this problem by companies like Google (via[Zanzibar](https://authzed.com/blog/what-is-zanzibar/) ) and Facebook (via Social Graph). After building an initial prototype of SpiceDB in Python, it became clear that we needed to address some architectural concerns and we took the opportunity to consider rebuilding with a different language. Despite the common sentiment on Python's performance characteristics, our prototype was surprisingly fast, as we heavily used functional programming patterns to compose and perform computations lazily and efficiently.


Our requirements were to mostly maintain the clarity of implementation, but also have more control over concurrency, performance, and memory efficiency. SpiceDB requires reasonably low latency: roughly ~20ms p99 for our permissions checking API requests. This is not quite the threshold where we'd have to use a non-garbage collected language, but regardless we did take a moment to consider Rust anyway. Our team is mostly comprised of ex-[CoreOS](https://cloud.redhat.com/learn/topics/coreos) engineers, so we were extremely familiar with Go. I'll try to avoid rehashing all of the commonly discussed advantages of Go, since those have been[well](https://www.cockroachlabs.com/blog/why-go-was-the-right-choice-for-cockroachdb/)[covered](https://getstream.io/blog/switched-python-go/)[for](https://stackoverflow.blog/2020/11/02/go-golang-learn-fast-programming-languages/)[years](https://www.slideshare.net/jpetazzo/docker-and-go-why-did-we-decide-to-write-docker-in-go/) .


While gophers love to gush over the fast compiler toolchain and standard tools, you'd be leaving something on the table by ignoring the third-party tools outside of what the Go team has built. Linting tools like[golangci-lint](https://golangci-lint.run/) and release management tools like[goreleaser](https://goreleaser.com/) , once setup, are a phenomenal boon to contributor workflow. This attention to a good experience spans not only developer tools, but also the libraries that build experiences for our own users. For example, the excellent[spf13/cobra](https://github.com/spf13/cobra) makes it trivial to create intuitive command-line interfaces with tab-completion and a unified way to pass flags as environment variables. For our power-users that are looking for good experiences with deployment and operational tooling, the ease at which we could expose our Go software to the industry's best[observability](https://www.ibm.com/cloud/learn/observability) tooling, was second to none. With almost no additional development overhead, we've been able to instrument our programs with metrics via[Prometheus](https://prometheus.io/) , distributed tracing via[OpenTelemetry](https://opentelemetry.io/) , structured logging via[zerolog](https://github.com/rs/zerolog) , and profiling via[pprof](https://github.com/google/pprof) .


You might be seeing a theme here: the largest impact on our decision was not the language properties, but actually the Go *ecosystem* . There are many databases implemented in Go, so we have the option to avoid reimplementing *many* algorithms and libraries that we needed to build SpiceDB. Notably, we're huge fans of[dgraph's Ristretto](https://github.com/dgraph-io/ristretto) and[InfluxDB's tdigest](https://github.com/influxdata/tdigest) . Because of the ubiquity of Go idioms and standard workflows throughout the Go open source community, once adopted, it was quite easy for us to contribute back changes to the code we were sharing, benefiting everyone. As SpiceDB requires client libraries for various languages, we are quite sensitive to the maturity of the gRPC ecosystem in our primary language. In this respect, Go shines: all gRPC and Protobuf libraries are natively implemented in Go and are extremely mature -- better than any other language I've used. The availability of idiomatic, native Go libraries for software that is typically only made available via C is a breath of fresh air coming from our previous language choice.


But, in the end, we cannot forget that it's always about people and not code. The Go community has always been extremely welcoming and caring to their fellow gophers. As we've worked with Go over the years, we've met some of the most phenomenally smart, thoughtful, and kind people. These are the people that we'd be honored to have represent and extend our company culture as we continue to grow. It's been almost a year since we made our decision to invest in Go as our primary development language. We're so appreciative of all the value we've gained from doing so and plan to ensure that our work will always reciprocate value back to the wonderful people that have made all of this possible.


If this post resonates with you and you have a passion for open source, Go, databases, distributed systems, we invite you to explore the community and code for[SpiceDB](https://github.com/authzed/spicedb) and, if you're looking for a workplace with those same values, you can take a look at openings on our[Jobs page](https://www.workatastartup.com/companies/authzed) .


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Additional Reading


## Related


[Engineering The Culture Panel: Inside AuthZed's Engineering Interview Process Part 3 of our interview series: Learn how AuthZed evaluates collaboration, ownership, and cultural fit in engineering candidates. Discover what we're looking for beyond technical skills and how to prepare for behavioral questions that matter. May 19, 2026 · 4 min](https://authzed.com/blog/the-culture-panel-authzed-engineering-interview-process)[Engineering The Culture Panel: Inside AuthZed's Engineering Interview Process Part 3 of our interview series: Learn how AuthZed evaluates collaboration, ownership, and cultural fit in engineering candidates. Discover what we're looking for beyond technical skills and how to prepare for behavioral questions that matter. Irit Goihman · May 19, 2026 · 4 min](https://authzed.com/blog/the-culture-panel-authzed-engineering-interview-process)


[Engineering The Technical Skills Panel: Inside AuthZed's Engineering Interview Process Part 2 of our interview series: Discover how AuthZed evaluates your ability to design, debug, and reason about real systems. Learn what to expect from our technical skills panel and how to showcase your architectural thinking. Apr 3, 2026 · 4 min](https://authzed.com/blog/the-technical-skills-panel-authzed-engineering-interview-process)[Engineering The Technical Skills Panel: Inside AuthZed's Engineering Interview Process Part 2 of our interview series: Discover how AuthZed evaluates your ability to design, debug, and reason about real systems. Learn what to expect from our technical skills panel and how to showcase your architectural thinking. Irit Goihman · Apr 3, 2026 · 4 min](https://authzed.com/blog/the-technical-skills-panel-authzed-engineering-interview-process)


[Engineering The Background Panel: Inside AuthZed's Engineering Interview Process Learn what to expect from AuthZed's background panel - a collaborative deep dive into your technical experience, decision-making, and engineering ownership. Get practical tips on how to prepare and what signals we're looking for. Apr 2, 2026 · 4 min](https://authzed.com/blog/the-background-panel-authzed-engineering-interview-process)[Engineering The Background Panel: Inside AuthZed's Engineering Interview Process Learn what to expect from AuthZed's background panel - a collaborative deep dive into your technical experience, decision-making, and engineering ownership. Get practical tips on how to prepare and what signals we're looking for. Irit Goihman · Apr 2, 2026 · 4 min](https://authzed.com/blog/the-background-panel-authzed-engineering-interview-process)
