---
schema_version: "1.0.0"
document_id: "efe917e94bf26ae94eb5b696a83272b97f218a24a2cddf42aee085e10bd269d9"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/java-sdk/"
published_at: "2025-02-24T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:786d5944227af420d4a8d76dee60fcef26664671336985e37d0b60724d73e14b"
---

# Announcing the Dagger Java SDK: A New Way to Automate with Java

At Dagger, our goal is to make automation easier for developers by allowing them to define powerful, reusable workflows in their preferred programming languages. Today, we’re excited to introduce the Experimental Dagger Java SDK - a new way for Java developers to leverage Dagger to build and run automation workflows!


#### Why Java Developers Will Love the Dagger SDK


If you work with Java, you know that managing automation, orchestrating services, and streamlining workflows can be challenging, especially when you need to cobble together shell scripts, DSLs, and YAML to get work done. The Dagger Java SDK brings the flexibility of Dagger’s API to Java, enabling you to:


- Define and execute workflows in Java – No need to switch languages to manage automation.
- Run workflows locally or in cloud environments – Develop and test with full reproducibility.
- Easily compose and reuse logic – Keep your automation modular and maintainable.
- Leverage Dagger’s caching and portability – Speed up execution and run seamlessly across environments.
- Interoperate with other Dagger SDKs – Combine Java with Go, Python, and TypeScript seamlessly.


#### A Sneak Peek at the Java SDK in Action


The Dagger Java SDK makes it easy to define and execute workflows using familiar Java syntax. Here’s a quick example of a simple Java module:


```text
@Object
public class MyModule extends AbstractModule {
@Function
public Service httpService() {
return dag.container()
.from("nginx:1.25-alpine")
.withExposedPort(80)
.withNewFile("/usr/share/nginx/html/index.html", "Hello, from Dagger!")
.asService();
}
@Function
public String get() throws ExecutionException, DaggerQueryException, InterruptedException {
return dag.container()
.from("alpine")
.withServiceBinding("web", httpService())
.withExec(List.of("wget", "-O-", "http://web:80"))
.stdout();
}
}
```


With just a few lines of Java, you can define a fully portable execution workflow that runs anywhere! Check out the demo below:


#### Interoperability: Mix and Match Java with Other SDKs


One of the most exciting features of the Dagger Java SDK is its cross-language compatibility. Java modules can work alongside Dagger modules written in Go, Python, or TypeScript. That means if your platform team uses Java but your application teams work with Go or TypeScript, you can now unify your automation workflows under one API. Yes, that means all of the modules in the[Daggerverse](https://daggerverse.dev/) work with the Java SDK out of the box!


#### How to Get Started with the Java SDK


Since this is an experimental release, we’re looking for early adopters to test the SDK, provide feedback, and help shape its future. Here’s how you can get started:


- [Try it out](https://docs.dagger.io/quickstart) – Provide feedback in the[#java](https://discord.gg/c27ugATDVS) channel in Discord or[file an issue](https://github.com/dagger) in GitHub
- Join the discussion – Connect with other developers in the[#java](https://discord.gg/c27ugATDVS) channel on Dagger Discord.


#### What’s Next?


This is just the beginning of the Java SDK! We’re already working on performance, better caching, documentation, and expanded ecosystem support. We’d love your input on features, use cases, and Java-related improvements.


Join the conversation on Discord, and let’s build the future of automation with Java together!


Happy coding! 💙
