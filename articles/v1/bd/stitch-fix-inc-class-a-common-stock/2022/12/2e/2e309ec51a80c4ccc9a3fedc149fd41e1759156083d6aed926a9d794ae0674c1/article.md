---
schema_version: "1.0.0"
document_id: "2e309ec51a80c4ccc9a3fedc149fd41e1759156083d6aed926a9d794ae0674c1"
company_key: "stitch-fix-inc-class-a-common-stock"
company: "Stitch Fix Inc."
source_id: "stitch-fix-inc-class-a-common-stock-rss-75294c3181e6"
canonical_url: "https://multithreaded.stitchfix.com/blog/2022/12/14/plain-old-python-functions/"
published_at: "2022-12-14T09:00:00+00:00"
first_seen_at: "2026-07-20T23:19:28.351732+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:fa613a56730c01609411b1dacb625cdfb5ff89dd01d03fbc5290ce2b862f76bc"
---

# Leveraging the Plain Old Python Function

The role of the *full-stack-data-scientist* is not what it once was.


With the advent of more powerful tooling, new industry standards in MLOps, and greater investment in platforms, the day-to-day of a data scientist has changed significantly at Stitch Fix. The difference, however, is subtle. The structure of their job remains the same – *[engineers still do not write ETLs](https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/)* and *[data scientists function as generalists](https://multithreaded.stitchfix.com/blog/2019/03/11/FullStackDS-Generalists/)* , but they now have to think on a higher level. Their job is constantly getting more and more complex—the business needs are in flux and the infrastructure they use is more powerful than it ever was. The old strategy of cobbling together complex systems will only end in stressed-out data scientists with too much infrastructure on their plate.


To avoid this cycle of complexity, Stitch Fix invests in a platform team to innovate new ways of supporting a data scientist’s engineering needs. Rather than constructing custom model-deployment mechanisms, building microservices from the ground up, and managing highly interdependent chains of data transformations, data scientists at Stitch Fix can leverage powerful infrastructure by **constructing plain old Python functions to represent their needs** .


In this blog post we’re going to take a different approach than usual. Rather than digging into a specific piece of technology, we’ll present our philosophy of functions for data science APIs and back it up with some motivating examples. We’ll explain the power of functions as a DSL, share some successes we’ve had using functional interfaces to build our MLOps stack, and connect our approach with external, open-source frameworks that the industry is beginning to adopt. Our goal is to convince you that a function-first approach will enable data practitioners to do more while doing less. The functional approach allows them to plug into the business in a scalable manner while avoiding the complexity of managing infrastructure and architectural decisions.


# On Functions and Functionality


Since way before many of us were born, the software industry has been battling between *functional* and *procedural* programming paradigms. Do you offer imperative commands over the operations of your system? Or do you declare the intent and let some system in the background handle the operations for you? While we won’t opine on this age-old debate, we do bring this up for a reason. At Stitch Fix, data scientists get the best of both worlds:


- They can declare infrastructure and plug into the business by writing python functions to fit into platform-provided frameworks
- They can specify custom model and business logic by implementing said functions


At the core, we believe data scientists should *only* have to think about business logic, and all else (infrastructure, etc…) should be given to them on a silver platter. The delineation of responsibilities here is critical in providing high-power tooling to allow data scientists to plug into the business, and Python functions happen to be the perfect way to codify it. The name, type annotations, decorators, and parameters of a function all provide plenty of information to declare the structure of the systems they need. The guts of a function allow a data scientist to iterate on and cleanly represent their business logic.


```text
@  do_something_fancy_with_function
def    my_function  (  param_1  :    pd  .  Series  ,    param_2  :    int  )    ->    pd  .  Series  :
"""Does a thing with some params"""
return    fancy_business_logic  (  param_1  ,    param_2  )


```


**Function Name**` my_function`


**Type Annotations**` pd.Series, int`


**Decorators**` @do_something_fancy_with_function`


**Parameters**` param_1, param_2`


**Docstring**` """Does a thing with some params"""`


**Logic** **` return fancy_business_logic(param_1, param_2)`**


How do we actually link this metadata to give data scientists MLOps capabilities? Let’s go through a few examples…


# Functional Capabilities


## Productionizing Models


Years ago, releasing a model at Stitch Fix took considerable effort. To execute a model in a production context (batch/online), data scientists had to:


1. Save the model blob to a blob store
2. Build a service or a batch job that exposed their model, with Python requirements that exactly matched training
3. Download the model into the appropriate production context, updating as needed
4. Maintain and manage the service/batch job


This regularly repeated chain of micromanaged infrastructure was not nimble enough to support the velocity at which data scientists at Stitch Fix work. So, we built new tooling that enables data scientists to publish their model to production by:


1. Saving a function using our API
2. Configuring deployment through a centralized model-management service


To write a model, one need simply implement a Python function (we’re not picky – a model can be *any* python function) and save it to the platform-provided API. This provides a host of capabilities to choose from with the push of a button: a data scientist can run their model over a large dataset on Spark, deploy in a platform-managed online service context to respond to http requests, track/manage/add metrics, and set up CI/CD. To gather the requisite model information for the platform to carry out these capabilities, we take advantage of everything the data scientist’s function provides us:


**Function Name** Delineates this query on the model from others


**Annotations** Specifies types for model inputs


**Parameters** Allows us to generate openAPI docs, connect to features


**Logic** Enables actual model evaluation!


We use a little more than this to add additional metadata—see the[full writeup](https://multithreaded.stitchfix.com/blog/2022/07/14/deployment-for-free/) for more details. That said, the core principle is simple. By reading the code and looking at the function a data scientist writes, we should be able to understand its role in the business and provide the infrastructure it requires.


## Writing Services


At Stitch Fix, our data scientists write and manage services, beyond those that can be represented by writing models (they’re on call, even!). Constructing/managing a service generally requires a lot of architectural decisions. What framework do you use? How do you specify/validate the shape of the input? How do you handle errors? And so on…


At Stitch Fix, the platform team has made it very easy to manage decisions. All a data scientist has to do is write a simple set of functions to specify their service’s endpoints. Built on top of FastAPI + Uvicorn, Stitch Fix’s service toolkit gives a data scientist the power of a full-scale web application.


While we have not yet blogged about this, the API is simple enough for Data Scientists to go from zero to a production service in an hour. What does it look like?


```text
@  get  (  '/v0/api/recommendations'  )
def    get_recommendations  (  customer_id  :    int  )    ->    List  [  Recommendation  ]:
"""Provides recommendations for a customer"""
return    some_business_logic  (...)


```


**Annotations** Specifies types for generating documentation/validating.


**Decorators** Specifies method type, endpoint, tags for organization. Enables distributed tracing. Registers function for use with FastAPI route.


**Parameters** Allows validating of inputs, generates API signature.


**Docstring** Generates openAPI documentation.


**Guts** Does the business logic.


Again, we don’t always use every part of the function (the name, in this case, is supplanted by the endpoint path), but from reading just the snippet of code above, it should be clear (a) where this fits in the business and (b) how this manifests in the infrastructure.


## Transforming Data


The vast majority of code data scientists write transforms data from one shape to another. Data transformations are a natural way to iteratively present value to the business, but representing them in code can be challenging. Scientists often develop their own methods of organizing and executing these transformations, which range from monolithic scripts to custom-built sophisticated pipelining mechanisms. While these get the job done, as teams switch up, goals change, and initiatives are cast aside or rushed into production, the details of these bespoke methodologies become long-forgotten. To solve this problem and allow our data teams to scale with the complexity of the business, we built and open-sourced a framework called[hamilton](https://github.com/stitchfix/hamilton) .


Initially designed for feature engineering, *hamilton* enables a data scientist to express a complex data pipeline as a series of functions; each function encodes the upstream dependencies, the data type, and the referenceable artifact.


```text
@  config  .  when  (  region  =  "US"  )
def    my_artifact  (  dep_1  :    pd  .  Series  ,    dep_2  :    int  )    ->    pd  .  Series  :
"""does something fancy with dep_1 and dep_2"""
return    _some_fancy_logic  (  dep_1  ,    dep_2  )


```


**Function Name** Name of the artifact generated by the fn


**Annotations** Types of the artifact/dependencies


**Decorators** Configuration, higher-level operations


**Parameters** Name of the upstream dependencies


**Docstring** Enables automatic generation of documentation


**Guts** Code to generate, transform the data


Representing generated data as functions enables us to flexibly execute pipelines in whatever framework we want while simultaneously allowing a data scientist to write highly expressive, intuitive pipelines. Better yet, these require little knowledge of framework or infrastructure, only the libraries data scientists love and the Python code they are accustomed to writing. All they have to do is write the function – the rest comes for free.


We’ve written about hamilton before (we’re very proud): read prior posts[introducing the framework](https://multithreaded.stitchfix.com/blog/2021/10/14/functions-dags-hamilton/) ,[talking about scale](https://multithreaded.stitchfix.com/blog/2022/02/22/scaling-hamilton/) , and sharing[how we enable data validation](https://multithreaded.stitchfix.com/blog/2022/07/26/hamilton-data-quality/) .


# Open Source and Beyond


One of Stitch Fix’s greatest assets is our innovative set of data scientists who optimize the customer experience every step of the way. Without the proper tooling, however, they would get bogged down in details of infrastructure and architecture. They need control over the stack they run to operate independently, but cannot afford to be slowed down with its minutiae. Thus, it falls to the platform team to deliver tooling that makes data scientists efficient and productive. At Stitch Fix we iterated on many designs, and found that, as is often the case, the simplest solution was the best. To express business logic, data scientists write plain old Python functions. The associated metadata, structure, and contents of these functions yield enough information for an insightful platform team to plug the data science innovations back into the business.


Python functions are comfortable to write, easy to debug, and straightforward to read and maintain. Add in a little config, and we’ve got a fully-fledged, highly-expressive API!


We believe this API approach has implications far past the frontiers of our own data science efforts. Aside from our own open-source tools, we’ve begun to notice it creep up elsewhere:


- Dagster’s[software-defined-assets](https://dagster.io/blog/software-defined-assets) _ _are one of many Python APIs that specify transformations on data through the orchestration framework
- [Pytest fixtures](https://docs.pytest.org/en/6.2.x/fixture.html) actually use the same concept as hamilton to inject data into Python tests
- [metaflow](https://metaflow.org/) allows definitions of functions specifying steps to modify workflow state
- …and so on


These (and more) amazing frameworks confirm our priors that APIs of this flavor are the future. This API has been essential in enabling data scientists to focus on what matters most. Serving the customer better, optimizing operations, and providing lasting, high-caliber tools that empower the next generation to reach even greater heights.
