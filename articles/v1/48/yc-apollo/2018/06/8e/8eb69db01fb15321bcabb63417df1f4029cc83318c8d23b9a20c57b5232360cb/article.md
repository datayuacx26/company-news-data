---
schema_version: "1.0.0"
document_id: "8eb69db01fb15321bcabb63417df1f4029cc83318c8d23b9a20c57b5232360cb"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-validation-using-directives"
published_at: "2018-06-06T04:17:39+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:97853437adedc29d3c49096d3b7c148c820d0a738a72572dfa9c22fb0b88b61f"
---

# GraphQL validation using directives

*This is a guest post by*[James Mortemore](https://github.com/confuser/) *, who is a software engineer at*[YLD](https://www.yld.io/) *. He has an extensive background in API development & enterprise integration architecture.*


Out of the box, GraphQL supports validating your inputs based on type information. For example, you can ensure that an argument is a particular scalar type, such as String or Int. You can also enforce the shape of object arguments using input object types. While this prevents a large class of possible errors, this doesn’t cater to more complex validation needs like enforcing the length of a string, or the size of a number.


### A simple example: User sign up form


To see how schema validation could be used, it’s easiest to start with an example. The SDL[(Schema Definition Language)](https://github.com/facebook/graphql/pull/90) will be as follows:


```text
type SignUpInput {
email: String!
password: String!
firstName: String!
lastName: String!
}


type Mutation {
signUp(input: SignUpInput!)
}
```


The above enforces all fields are strings, and ensures they are not null. However, we’ve got some additional constraints that come from the database that we need to be wary of. In our server, the backend database is using a` VARCHAR(255)` field for all Strings. This means providing a field greater than 255 characters will cause an error!


Today, people most commonly write this kind of validation logic in their resolver functions or models. However, this means we can’t easily see our validation logic, and we have to write some repetitive code to verify the same kinds of conditions over and over.


### A new option: Validation with schema directives


It’s possible to add this logic directly to your SDL using a new module:[graphql-constraint-directive](https://www.npmjs.com/package/graphql-constraint-directive) . This module exposes a` @constraint` directive to decorate an SDL with validation rules.


Thanks to[graphql-tools](https://www.npmjs.com/package/graphql-tools) and[apollo-server](https://www.apollographql.com/docs/apollo-server/) , it’s easy to integrate them. Here’s how you do it:


```text
npm install graphql-constraint-directive
```


```text
const { makeExecutableSchema } = require('graphql-tools')
const ConstraintDirective = require('graphql-constraint-directive')


const typeDefs = `SDL HERE`
const schema = makeExecutableSchema({
typeDefs, schemaDirectives: { constraint: ConstraintDirective }
})
```


After just a few lines of configuration, it’s now possible to decorate any part of your SDL with` @constraint` to attach validation rules. A full example can be found on the[graphql-constraint-directive readme](https://github.com/confuser/graphql-constraint-directive/blob/master/README.md) .


Here’s how your SDL will look like with some constraint directives added in:


```text
type SignUpInput {
email: String! @constraint(format: "email", maxLength: 255)
password: String! @constraint(maxLength: 255)
firstName: String! @constraint(pattern: "^[0-9a-zA-Z]*$", maxLength: 255)
lastName: String! @constraint(pattern: "^[0-9a-zA-Z]*$", maxLength: 255)
}


type Mutation {
signUp(input: SignUpInput!)
}
```


Before we move on, it’s good to mention that this isn’t a silver bullet solution. More complex validation rules, such as those which rely on state (e.g. does this entity exist?) will need to remain elsewhere in your code.


### Looking at the example in detail


Let’s break down the example above, field by field…


```text
email: String! @constraint(format: "email", maxLength: 255)
```


` @constraint` has a number of available arguments (check the[readme](https://github.com/confuser/graphql-constraint-directive/blob/master/README.md) for a full list) depending on the field type. In this case, as it’s a string, the option of defining a` format` can be used. This particular constraint is validating that the value is an email address and it’s 255 characters at most.


Another way to write this could be to split it into multiple` @constraint` directives. Note that directives are resolved right to left. For example, maxLength would be resolved before format in the following field definition:` email: String! @constraint(format: "email") @constraint(maxLength: 255)`


```text
lastName: String! @constraint(pattern: "^[0-9a-zA-Z]*$", maxLength: 255)
```


On a different field, it is confirming the last name is a maximum of 255 characters and also matches a regular expression. In this instance, alphanumeric only (sorry double barrelled surnames!).


You could remove` maxLength` and perform this check within the expression. However, regex and` .length` return unexpected results when[emojis are used… 💩](https://blog.jonnew.com/posts/poo-dot-length-equals-two)


### Receiving validation errors on the client


When a consumer sends a request which breaks these rules, the application will respond with an error message.


```text
{
"errors": [{
"message": "Variable \"$input\" got invalid value {\"email\":\"foo💩\"}; Expected type ConstraintString at value.email; Must be in email format"
}]
}
```


Note that isn’t a very user-friendly message. If you wish to provide your own message, you can do that within the[formatError](https://www.apollographql.com/docs/apollo-server/api/apollo-server#formaterror) option to Apollo Server.


## How does the @constraint directive work?


There’s something interesting about the error message above! Specifically, it says` Expected type ConstraintString` , even though the field is defined as` String!` . The constraint directive wraps each field with its own[scalar](https://github.com/confuser/graphql-constraint-directive/blob/e553a0526725edf015ae4dea16ff6f380500c25e/scalars/string.js#L6)[type](https://github.com/confuser/graphql-constraint-directive/blob/e553a0526725edf015ae4dea16ff6f380500c25e/scalars/number.js#L4) .` <a href="https://github.com/apollographql/graphql-tools" target="_blank" rel="noreferrer noopener">graphql-tools</a>` takes care of this implementation detail for the consumer, so the fields in question can still just be the type` String!` when submitting the mutation.` ConstraintString` &` ConstraintNumber` will never appear within your Schema Definition Language (SDL), and the client/consumer needn’t be concerned by it.


When the value of each decorated field is parsed, the validation rules are executed. For example, here’s the code for` minLength` :


```text
if (args.minLength && !isLength(value, { min: args.minLength })) {
throw new GraphQLError(`Must be at least ${args.minLength} characters in length`)
}
```


If all validation rules pass,` ConstraintString` /` ConstraintNumber` executes the original scalar’s` parseValue` and returns it. This allows wrapped scalars to execute their own parseValue function, enabling support for custom scalar types!


Using this approach, it’s trivial to[create your own custom directives](https://www.apollographql.com/docs/apollo-server/schema/directives/#custom-directives) to validate and manipulate field values. You can look at the code of[graphql-constraint-directive](https://github.com/confuser/graphql-constraint-directive) as an example! To publish a custom directive as a module, ensure the custom directive class is exported as the` main` module entry.


I hope this inspires you to develop and publish your own custom directives!


Written by


James Mortemore


[Read more by James Mortemore](https://www.apollographql.com/blog/author/james)
