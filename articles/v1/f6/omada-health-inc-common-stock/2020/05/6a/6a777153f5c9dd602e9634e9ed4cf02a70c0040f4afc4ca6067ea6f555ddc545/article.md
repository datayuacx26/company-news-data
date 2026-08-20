---
schema_version: "1.0.0"
document_id: "6a777153f5c9dd602e9634e9ed4cf02a70c0040f4afc4ca6067ea6f555ddc545"
company_key: "omada-health-inc-common-stock"
company: "Omada Health Inc."
source_id: "omada-health-inc-common-stock-rss-9e1c3b1ccfca"
canonical_url: "https://medium.com/omada-health-tech/graphql-tips-the-backend-553375e1b669"
published_at: "2020-05-19T16:31:01+00:00"
first_seen_at: "2026-07-20T23:18:22.449098+00:00"
fetched_at: "2026-07-28T22:26:47.413551+00:00"
content_hash: "sha256:785de180838ee68d3a3b5f1715b666b42df17fc3416582637c9858523c916477"
---

# GraphQL Tips: The Backend

Ruby


Ruby On Rails


GraphQL


API


# GraphQL Tips: The Backend


[Omada Health Engineering](https://medium.com/@OmadaEng?source=post_page---byline--553375e1b669---------------------------------------)


7 min read


·


May 19, 2020


--


*Written by*[Will Hastings](https://www.linkedin.com/in/willhastings4) *, Staff Engineer*


Photo by[Joshua Fuller](https://unsplash.com/@joshuafuller?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/s/photos/ruby?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


Welcome back to the saga of Omada Engineering’s adventures with GraphQL. In previous posts, I discussed[our processes for learning and adopting](https://medium.com/omada-health-tech/exploring-graphql-at-omada-f64133a50462) GraphQL and[the benefits it brought us](https://medium.com/omada-health-tech/graphql-reaping-the-benefits-423daeb96be8) . This time, I’ll share two technical tips we discovered while adding GraphQL support to our Ruby on Rails API server using the[graphql-ruby gem](https://graphql-ruby.org/) . First, I’ll cover the code organization structure we chose and the benefits it gives us in terms of discoverability and maintainability. Second, I’ll demonstrate the solution we implemented for adding GraphQL-specific details to our server request logs.


## Code Organization


By default, graphql-ruby generates the directory app/graphql/types for you to place the Ruby classes that define the types for your GraphQL schema. It also gives you the app/graphql/mutations directory for the Ruby classes that define your GraphQL mutations. We realized that adding all types and mutations to these directories without any further organization would quickly become a maintenance burden. So we chose to organize type and mutation classes into Ruby modules (and the corresponding directories on the filesystem) based on the category of data the types apply to.


For types, this often means grouping together classes for the following:


- A GraphQL type for a core part of our business domain (e.g. a community Discussion)
- A corresponding[GraphQL input type](https://graphql.org/learn/schema/#input-types)
- A collection type for representing paginated lists
- Any associated[GraphQL enums](https://graphql.org/learn/schema/#enumeration-types)


For example, the classes associated with our Discussion type are grouped in one directory like so:


- app/graphql/types/
- discussion/
- discussion_input_type.rb
- discussion_topic_type.rb
- discussion_topics_status_enum.rb
- discussion_type.rb
- discussions_collection_type.rb
- discussions_sort_key_enum.rb


When defining one of these classes, we place it in a Ruby module to match the directory. For example, DiscussionType:


```text
module Types    module Discussion      class DiscussionType < Types::Base::BaseObject          # ...      end    end  end
```


For mutations, we group actions related to a core data type in one directory/Ruby module. For example, we have the following mutation classes for our BloodPressureReading type:


- app/graphql/mutations/
- blood_pressure/
- blood_pressure_reading_toggle_filter.rb
- blood_pressure_reading_update.rb


Using these patterns, we keep the code for our GraphQL mutations and types organized and related classes close together.


## Breaking Up the Query Type Implementation


In GraphQL, the root of your schema’s types is the[Query type](https://graphql.org/learn/schema/#the-query-and-mutation-types) , which provides fields for returning data for all of your types. The graphql-ruby gem generates a GraphQL type class at app/graphql/types/query_type.rb for defining your Query type. While it makes sense for your GraphQL schema to have a root type, defining all the fields on the Query type in one Ruby class would definitely lead to maintenance difficulties, as you would need to define methods for loading all your data types in one place. To keep us from ending up with a giant Query class, we took advantage of a graphql-ruby feature called[Resolvers](https://graphql-ruby.org/fields/resolvers.html) .


A resolver class defines the signature and data lookup code for one field of a GraphQL type, including the field’s return type and those of any arguments it takes. For each field on our Query type, we’ve created a resolver class to implement it.


For example, our resolver for our Query’s accountProfile(account_id: Int!) field looks similar to the following:


```text
module Resolvers    class AccountProfileResolver < GraphQL::Schema::Resolver      type Types::AccountProfile::AccountProfileType      argument :account_id, Integer, required: true      def resolve(account_id:)          # Load account...          # Check if current user has permission to access account data          # Return account      end    end  end
```


And so our QueryType class becomes simply a list of fields:


```text
module Types    class QueryType < Types::Base::BaseObject       field :account_profile, resolver: Resolvers::AccountProfileResolver      # Other fields...    end  end
```


While the graphql-ruby docs do recommend alternative approaches to code organization other than resolvers, we felt resolvers solved the problem best, as they allow the field signature definition and data lookup code to live in the same place, while keeping the QueryType class as minimal as possible.


## Logging Query Details


In order to have access to better aggregate data on the queries and mutations our GraphQL API receives, we needed to work out a strategy to get more than the raw query string into the structured JSON we record in our general server request logs. We wanted a JSON representation of the query or mutation received so we could aggregate log entries to, for example, get a count of all requests that queried a certain field in our GraphQL schema. The challenge here was how to get that data out of graphql-ruby so that we could record it with the rest of the request data.


We found that we could use graphql-ruby’s[Ahead-of-Time AST Analysis API](https://graphql-ruby.org/queries/ast_analysis.html) . The library parses the incoming GraphQL query into an[abstract syntax tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) , and this API will run your code for each piece of a query, such as every field it accesses. To use this API, you create a class that extends GraphQL::Analysis::AST::Analyzer and implements one of the callback methods that class supports. In our case, we implemented on_leave_field so that we could record the name of each field an incoming query or mutation accessed. You also implement a result method to return the result of your analysis, which we’ll see how to access later.


Our class to collect a representation of each field accessed is as follows:


```text
class LoggingGraphqlQueryAnalyzer < GraphQL::Analysis::AST::Analyzer    def initialize(query_or_multiplex)      super      @nodes_map = {}      @root_entries = []      @operation_node = nil    end    def on_leave_field(node, parent_node, _visitor)      @nodes_map[node] ||= { name: node.name }      case parent_node      when GraphQL::Language::Nodes::Field        @nodes_map[parent_node] ||= { name: parent_node.name }        parent_entry = @nodes_map[parent_node]        parent_entry[:fields] ||= []        parent_entry[:fields] << @nodes_map[node]      when GraphQL::Language::Nodes::OperationDefinition        @root_entries << @nodes_map[node]        @operation_node = parent_node      end    end    def result      {        type: @operation_node&.operation_type,        operation_name: @operation_node&.name,        query: { fields: @root_entries }      }    end  end
```


Let’s consider how it would operate on the following query:


```text
query DiscussionQuery {    discussion(discussionId: 1) {      title      message      author {        firstName      }    }  }
```


The on_leave_field method receives a node object and a parent_node object. A node represents a piece of a GraphQL query, such as a field or a field argument. It has a name method which returns a string name, such as the name of a field. In our implementation of on_leave_field, we create a hash to store the name of the field the current node represents, and then store that hash in a larger hash called @nodes_map using the node object as the key. This allows us to look up the field-specific hash the next time we encounter that node.


Then we check if the current node’s parent node is a field node or an operation node. An operation node represents the root of the query. In our example, its name is DiscussionQuery. Fields whose parent is the operation node are the top level fields our query is accessing, such as discussion in this example.


If the current node’s parent is another field, which we can tell if the parent’s class is GraphQL::Language::Nodes::Field, we then know it is not a top-level field. So we add the hash representing the current node to a fields array on the hash representing the parent node, which we initialize and add to our @nodes_map if it doesn’t already exist. If the hash for the parent node already exists, we add to it, so that a parent field that contains multiple sub-fields will end up with all of them in its fields array.


If the current node’s parent is the operation node, which we can tell if the parent’s class is GraphQL::Language::Nodes::OperationDefinition, we then know it is a top-level field. So we add its hash to an array called @root_entries, which in our current example would only contain one element with the name discussion. We also store the parent node in the @operation_node instance variable so that we can access information about it in our implementation of result.


In our result method, we put together a final hash containing the data we want added to our request log. It includes the operation type (query or mutation), operation name (DiscussionQuery in our example), and the tree of hashes representing the fields we captured in on_leave_field. This final hash can then be serialized to JSON as part of the larger JSON structure we log for the request.


For this example, the JSON output would be the following:


```text
{    "type": "query",    "operation_name": "DiscussionQuery",    "query": {      "fields": [        {          "name": "discussion",          "fields": [            {              "name": "title"            },            {              "name": "message"            },            {              "name": "author",              "fields": [                {                  "name": "firstName"                }              ]            }          ]        }      ]    }  }
```


To run our analyzer class on each incoming request, we configure it in our schema class:


```text
class GraphqlSchema < GraphQL::Schema    use(GraphQL::Analysis::AST)    query_analyzer(LoggingGraphqlQueryAnalyzer)  end
```


Finally, we need to access the results of the analyzer in our Rails controller that handles incoming GraphQL requests. To do this, we leverage graphql-ruby’s[Tracing API](https://graphql-ruby.org/queries/tracing.html) , which allows you to provide an object that can listen for different events the library fires as it’s processing a GraphQL query. It will call your object’s trace method with a key for the event and associated data, plus a block that executes the next segment of query processing.


We created a simple class that implements trace and for the analyze_query event saves the result from our query analyzer class so that we can access it from our controller:


```text
class GraphqlRequestTracer    attr_accessor :log_data    def trace(key, _data)      result = yield      if key == 'analyze_query'        self.log_data = result.first      end      result    end  end
```


In our GraphqlController, we pass an instance of the tracer along with the query context and then append the data from the analyzer to our log data in append_info_to_payload, which our logging system calls to get log data for the current request. It looks something like this:


```text
class GraphqlController < ApplicationController    def execute      # ...      context = {        # ...        tracers: [request_tracer],      }      @result = GraphqlSchema.execute(query, variables: variables, context: context, operation_name: operation_name)      render json: @result      # ...    end    protected    def append_info_to_payload(payload)      super      return payload unless @result      payload[:graphql] = {        status: @result.has_key?('errors') ? 'error' : 'success',        errors: @result['errors'],      }.merge(request_tracer.log_data || {})      payload    end    # ...    private    def request_tracer      @request_tracer ||= GraphqlRequestTracer.new    end    # ...  end
```


This adds the GraphQL-specific data to our log entry JSON under the graphql key. It was tricky to figure out that the way to get the analyzer data to the controller was through the tracing API, but now that it’s in place we could extend it for other query analysis concerns in the future.


## Conclusion


So far we’re happy with the approaches we chose for organizing our GraphQL class file structure and separating the implementation of our QueryType into encapsulated classes. On the logging side, our addition of GraphQL-specific details in JSON format makes it easier for us to get aggregate data from our logging system on how often the fields and types in our GraphQL schema are accessed. If you’re using GraphQL with Rails and graphql-ruby like we are, I hope these tips give you a leg up in your GraphQL journey.
