---
schema_version: "1.0.0"
document_id: "70c73a52dc29afbd0673f43fc44d29c1727d2711346bbb4d20febc062cdef4d2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/eusable-graphql-schema-directives"
published_at: "2018-03-15T21:16:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:fc3196cb6f3bf6227de99d3cda1dacfacdbe10f7ef5cd324de745faeb8a367fe"
---

# Reusable GraphQL schema directives

No matter what programming language or software stack you choose for your GraphQL server, the[Schema Definition Language](https://github.com/facebook/graphql/pull/90) (SDL for short) is the[lingua franca](https://en.wikipedia.org/wiki/Lingua_franca) shared by everyone building GraphQL APIs:


```text
type Query {
posts: [Post]
author(id: Int!): Author
}


type Author {
id: Int!
firstName: String
lastName: String
posts: [Post]
}


# ... other types
```


Describing the structure and types of your data is the first step to doing anything with GraphQL, and the essence of[SDL-first development](https://www.apollographql.com/docs/graphql-tools/#recommendations) . The formal specification of this shared language can be found[here](http://facebook.github.io/graphql/draft/#sec-Schema) , though you shouldn’t feel intimidated by that formalism. The goal of SDL is to allow both humans and machines to understand GraphQL data at a glance. If you’re just getting started, pay attention to[the examples](http://graphql.org/learn/schema/#type-system) , and you’ll pick it up in no time.


As the data you expose through your GraphQL API become more complicated, your schema will naturally grow larger as well, and you may begin to notice repetition, or get tired of implementing the same sort of[resolvers](https://www.apollographql.com/docs/graphql-tools/resolvers.html) over and over again. That’s why SDL provides a special syntax for annotating your types, fields, and arguments with[directives](http://facebook.github.io/graphql/draft/#sec-Type-System.Directives) that your schema-processing tools can use to modify the structure and/or runtime behavior of your schema:


As you can see in this made-up example, the` @formattableDate` directive is *declared* once, with argument names, acceptable argument types, default argument values, and permitted schema locations, and then *used* zero or more times throughout the schema.


The possible applications of directive syntax are numerous: enforcing access permissions, formatting date strings, auto-generating resolver functions for a particular backend API, marking strings for internationalization, synthesizing globally unique object identifiers, specifying caching behavior, skipping or including or deprecating fields, and just about anything else you can imagine.


Today we’re thrilled to tell you about a new set of tools for implementing custom schema directives, so that you can dramatically simplify your own GraphQL schemas, or even (if you’re so ambitious) enable other GraphQL developers to write more expressive schemas of their own.


Read below to learn more and see some examples, or check out[the full documentation](https://www.apollographql.com/docs/graphql-tools/schema-directives.html) for even more examples and details about how best to use this new feature. Also,[let us know on Twitter](https://twitter.com/apollographql) if you’re interested in building directives that other developers can use in their apps!


## Using schema directives


Most of this post is concerned with *implementing* schema directives, and that can take a fair bit of thought to get right. However, the API for *using* a schema directive is extremely simple.


Just import the implementation of the directive, then pass it (along with your` typeDefs` ) to the` makeExecutableSchema` function via the` schemaDirectives` option, which is an object that maps directive names to directive implementations:


```text
import { makeExecutableSchema } from "graphql-tools";
import { UniqueIdDirective } from "anyone-could-implement-this-package";


const typeDefs = `
type Query {
people: [Person]
}
# This hypothetical type uses both the "Person" type name and the value
# of the personID field to synthesize a globally unique ID field:
type Person @uid(from: "personID") {
personID: Int
name: String
}`;


const schema = makeExecutableSchema({
typeDefs,
schemaDirectives: {
uid: UniqueIdDirective
}
});
```


That’s it. The implementation of` UniqueIdDirective` can take care of everything else, including declaring` directive @uid(from: String) on OBJECT` if necessary. If you understand what a certain directive is supposed to do to your schema, and you verify that it works as advertised, you don’t have to think about how it works.


Of course, understanding how your tools work can enable you to use them more effectively, so we encourage you to keep reading! Everything written below addresses some aspect of how a directive like` @formattableDate` or` @uid` could be implemented. Even if the majority of your experiences with directives involve using rather than implementing them, this information will equip you to debug directives that don’t seem to be working, and might even inspire you to implement your own.


## Implementing schema directives


Since the GraphQL specification does not discuss any specific implementation strategy for directives, it’s up to each GraphQL server framework to expose an API for implementing new directives. If you’re using Apollo Server, then you are also likely to be using the[graphql-tools](https://github.com/apollographql/graphql-tools) npm package. Today we’re excited to introduce a convenient yet powerful tool for implementing custom directive syntax:[the SchemaDirectiveVisitor class](https://www.apollographql.com/docs/graphql-tools/schema-directives.html) .


To implement a schema directive using` SchemaDirectiveVisitor` , simply create a subclass of` SchemaDirectiveVisitor` that overrides one or more of the following visitor methods (type annotations below for clarity):


```text
import { SchemaDirectiveVisitor } from "graphql-tools";


class SomeDirective extends SchemaDirectiveVisitor {
visitSchema(schema: GraphQLSchema) {}
visitObject(object: GraphQLObjectType) {}
visitFieldDefinition(field: GraphQLField<any, any>) {}
visitArgumentDefinition(argument: GraphQLArgument) {}
visitInterface(iface: GraphQLInterfaceType) {}
visitInputObject(object: GraphQLInputObjectType) {}
visitInputFieldDefinition(field: GraphQLInputField) {}
visitScalar(scalar: GraphQLScalarType) {}
visitUnion(union: GraphQLUnionType) {}
visitEnum(type: GraphQLEnumType) {}
visitEnumValue(value: GraphQLEnumValue) {}
}
```


By overriding a method like` visitObject` , a subclass of` SchemaDirectiveVisitor` expresses interest in certain schema types such as` GraphQLObjectType` (the first parameter type of` visitObject` ). These method names correspond to all possible[locations](https://github.com/graphql/graphql-js/blob/a62eea88d5844a3bd9725c0f3c30950a78727f3e/src/language/directiveLocation.js#L22-L33) where a directive may be used in a schema. For example, the` INPUT_FIELD_DEFINITION` location is handled by` visitInputFieldDefinition` .


Here’s one possible implementation of the[@deprecated directive required by the GraphQL specification](http://facebook.github.io/graphql/draft/#sec--deprecated) , which needs to implement` visitObject` ,` visitFieldDefinition` , and` visitEnumValue` to handle the` OBJECT` ,` FIELD_DEFINITION` , and` ENUM_VALUE` locations:


```text
import { SchemaDirectiveVisitor } from "graphql-tools";


class DeprecatedDirective extends SchemaDirectiveVisitor {
visitObject(object) {
this._deprecate(object);
}


visitFieldDefinition(field) {
this._deprecate(field);
}


visitEnumValue(value) {
this._deprecate(value);
}


_deprecate(thing) {
// Add some metadata to the object that the GraphQL server
// can use later to display deprecation warnings.
thing.isDeprecated = true;
thing.deprecationReason = this.args.reason;
}
}
```


In order to apply this implementation to a schema that uses` @deprecated` directives, simply pass the` DeprecatedDirective` class to the` makeExecutableSchema` function via the` schemaDirectives` option:


```text
import { makeExecutableSchema } from "graphql-tools";


const schema = makeExecutableSchema({
typeDefs,
schemaDirectives: {
deprecated: DeprecatedDirective
}
});
```


Alternatively, if you want to modify an existing GraphQL.js schema object, you can call` SchemaDirectiveVisitor.visitSchemaDirectives` directly:


```text
SchemaDirectiveVisitor.visitSchemaDirectives(schema, {
deprecated: DeprecatedDirective
});
```


Note that a subclass of` SchemaDirectiveVisitor` will be instantiated once for each occurrence of the` @deprecated` directive. That’s why you provide the` DeprecatedDirective` class rather than an instance of that class.


If for some reason you have a schema that uses another name for the` @deprecated` directive, but you want to use the same implementation, you can! Just use a different key in the` schemaDirectives` object that you pass to` makeExecutableSchema` . In other words,` SchemaDirectiveVisitor` implementations are effectively anonymous, so it’s up to whoever uses them to assign names to them.


Visitor methods can return a new object to replace the existing one, or` null` to remove it from the schema, though it’s more common for visitor methods to return nothing and instead modify the given object in place. Since` makeExecutableSchema` is used for creating *new*` GraphQLSchema` objects, there’s no need to make defensive copy of the schema object before modifying it.


### Example: fetching data from a REST API


Suppose you’ve defined an object type that corresponds to a[REST](https://en.wikipedia.org/wiki/Representational_state_transfer) resource, and you want to avoid implementing resolver functions for every field:


```text
directive @rest(url: String) on FIELD_DEFINITION


type Query {
people: [Person] @rest(url: "/api/v1/people")
}
```


Here’s how you might implement this` @rest` directive:


```text
class RestDirective extends SchemaDirectiveVisitor {
public visitFieldDefinition(field) {
const { url } = this.args;
field.resolve = () => fetch(url);
}
}


const schema = makeExecutableSchema({
typeDefs,
schemaDirectives: {
rest: RestDirective
}
});
```


There are many more issues to consider when implementing a real GraphQL wrapper over a REST endpoint (such as how to do caching or pagination), but this example demonstrates the basic structure.


Modifying the` field.resolve` function is a common implementation technique for directives that need to intercept query results at runtime. In this example, we decided not to worry whether the field had an existing resolver; if it did, then we clobbered it with the new one. In the next example, you’ll see how to wrap an existing resolver function.


### Example: formatting date strings


Suppose a field resolver returns a` Date` object but you want to return a formatted string to the client, and you want to let the GraphQL client decide what format to use by passing in an argument:


```text
directive @formattableDate(
defaultFormat: String = "mmmm d, yyyy"
) on FIELD_DEFINITION


scalar Date


type Query {
today: Date @formattableDate
}
```


Here’s how you could implement the` @formattableDate` directive to add an optional` format` argument to the` Query.today` field, wrap its resolver function, and update its result type from` Date` to` GraphQLString` :


```text
import formatDate from "dateformat";
import {
defaultFieldResolver,
GraphQLString,
} from "graphql";


class FormattableDateDirective extends SchemaDirectiveVisitor {
public visitFieldDefinition(field) {
const { resolve = defaultFieldResolver } = field;
const { defaultFormat } = this.args;


// Add an additional `format` argument to the field:
field.args.push({
name: 'format',
type: GraphQLString
});


field.resolve = async function (
source,
{ format, ...otherArgs },
context,
info,
) {
// Call the original resolver function to get the Date
// object to be formatted:
const date = await resolve.call(
this, source, otherArgs, context, info);


// If a format argument was not provided, default to the
// optional defaultFormat argument taken by the directive:
return formatDate(date, format || defaultFormat);
};


// Update the result type of the field, since now it returns
// a String instead of a Date:
field.type = GraphQLString;
}
}
```


Now the client can specify a desired` format` argument when requesting the` Query.today` field, or omit the argument to use the` defaultFormat` string specified in the schema:


```text
import { graphql } from "graphql";
import { makeExecutableSchema } from "graphql-tools";


const schema = makeExecutableSchema({
typeDefs,
schemaDirectives: {
formattableDate: FormattableDateDirective
}
});


graphql(schema, `query { today }`).then(result => {
// Logs with the default "mmmm d, yyyy" format:
console.log(result.data.today);
});


graphql(schema, `query {
today(format: "d mmm yyyy")
}`).then(result => {
// Logs with the requested "d mmm yyyy" format:
console.log(result.data.today);
});
```


### Example: enforcing authorization permissions


Imagine a hypothetical` @auth` directive that takes an argument` requires` of type` Role` , which defaults to` ADMIN` . This` @auth` directive can appear on an` OBJECT` like` User` to set default access permissions for all` User` fields, as well as appearing on individual fields, to enforce field-specific` @auth` restrictions:


```text
directive @auth(
requires: Role = ADMIN,
) on OBJECT | FIELD_DEFINITION


enum Role {
ADMIN
REVIEWER
USER
UNKNOWN
}


type User @auth(requires: USER) {
name: String
banned: Boolean @auth(requires: ADMIN)
canPost: Boolean @auth(requires: REVIEWER)
}
```


What makes this example tricky is that the` OBJECT` version of the directive needs to wrap all fields of the object, even though some of those fields may be individually wrapped by` @auth` directives at the` FIELD_DEFINITION` level, and we would prefer not to rewrap resolvers if we can help it:


```text
class AuthDirective extends SchemaDirectiveVisitor {
visitObject(type) {
this.ensureFieldsWrapped(type);
type._requiredAuthRole = this.args.requires;
}


// Visitor methods for nested types like fields and arguments
// also receive a details object that provides information about
// the parent and grandparent types.
visitFieldDefinition(field, details) {
this.ensureFieldsWrapped(details.objectType);
field._requiredAuthRole = this.args.requires;
}


ensureFieldsWrapped(objectType) {
// Mark the GraphQLObjectType object to avoid re-wrapping:
if (objectType._authFieldsWrapped) return;
objectType._authFieldsWrapped = true;


const fields = objectType.getFields();


Object.keys(fields).forEach(fieldName => {
const field = fields[fieldName];
const { resolve = defaultFieldResolver } = field;


field.resolve = async function (...args) {
// Get the required Role from the field first, falling back
// to the objectType if no Role is required by the field:
const requiredRole =
field._requiredAuthRole ||
objectType._requiredAuthRole;


if (! requiredRole) {
return resolve.apply(this, args);
}


const context = args[2];
const user = await getUser(context.headers.authToken);
if (! user.hasRole(requiredRole)) {
throw new Error("not authorized");
}


return resolve.apply(this, args);
};
});
}
}
```


One drawback of this approach is that it does not guarantee fields added later will be wrapped, and the whole` getUser(context.headers.authToken)` is a made-up API that would need to be fleshed out. In other words, we’ve glossed over some of the details that would be required for a production-ready implementation of this directive, though we hope the basic structure shown here inspires you to find clever solutions to the remaining problems.


### Declaring schema directives


In the examples above, for simplicity, we often included a declaration of the directive in the` typeDefs` string. However, if you’re implementing a reusable` SchemaDirectiveVisitor` for public consumption, you will probably not be the person writing the SDL syntax, so you may not have control over which directives the schema author decides to declare, and how.


That’s why a well-implemented, reusable` SchemaDirectiveVisitor` should consider overriding the` getDirectiveDeclaration` method. For example, the` AuthDirective` class we saw above could enforce the following declaration


```text
directive @auth(
requires: Role = ADMIN,
) on OBJECT | FIELD_DEFINITION
```


by returning a new` GraphQLDirective` object from` getDirectiveDeclaration` (in addition to implementing the visitor methods we saw above):


```text
import {
DirectiveLocation,
GraphQLDirective,
} from "graphql";


import { SchemaDirectiveVisitor } from "graphql-tools";


class AuthDirective extends SchemaDirectiveVisitor {
static getDirectiveDeclaration(directiveName, schema) {
return new GraphQLDirective({
name: directiveName,
locations: [
DirectiveLocation.OBJECT,
DirectiveLocation.FIELD_DEFINITION,
],
args: {
requires: {
// Having the schema available here is important for
// obtaining references to existing type objects, such
// as the Role enum.
type: schema.getType('Role'),
defaultValue: 'ADMIN',
}
}
});
}
}
```


By the way, this` new GraphQLDirective(...)` syntax is what you would have to write if there was no such thing as a Schema Definition Language. Notice how much shorter and more expressive the SDL version is!


Because` AuthDirective` implements` getDirectiveDeclaration` , it’s no longer necessary for the schema author to include the` directive @auth ...` declaration explicitly in the schema. The returned` GraphQLDirective` object will be used to enforce the argument types and default values, as well as enabling tools like[GraphiQL](https://github.com/graphql/graphiql) to discover the directive using schema introspection. Additionally, if the` AuthDirective` class fails to implement` visitObject` or` visitFieldDefinition` , a helpful error will be thrown.


## Automatic schema healing 👩‍⚕️🔮✨


Implementing schema directives is already hard enough without having to worry about all the little details of keeping the schema internally consistent. Thankfully,` graphql-tools` handles many of these details for you!


For example, every` GraphQLSchema` object maintains a map from type names to` GraphQLNamedType` objects (e.g., instances of` GraphQLObjectType` ,` GraphQLUnionType` ,` GraphQLScalarType` , etc.), each of which has its own` name` property. When the schema is first constructed, the following property holds for all names:


```text
schema.getTypeMap()[name].name === name
```


However, after you’ve modified the schema, it’s entirely possible that the` name` property in one of these objects changed, but the corresponding key in` schema.getTypeMap()` was left unchanged. This small inconsistency could easily go unnoticed and might cause subtle bugs later on.


As another example, if you replace a type in the schema by returning a new object from a visitor method, then you might expect you need to update all references to that type elsewhere in the schema:


```text
import assert from "assert";
import { GraphQLObjectType } from "graphql";


const typeDefs = `
type Query {
people: [Person]
}
type Person @replace {
name: String
}`;


const schema = makeExecutableSchema({
typeDefs,
schemaDirectives: {
replace: class extends SchemaDirectiveVisitor {
visitObject(object) {
// Return a new GraphQLObjectType object to replace any
// type decorated with the @replace directive:
return new GraphQLObjectType({
name: object.name,
...
});
}
}
}
});


// There should be only one Person type in the schema!
assert.strictEqual(
schema.getType("Query").getFields().people.type.ofType,
schema.getType("Person")
);
```


Fortunately,` SchemaDirectiveVisitor.visitSchemaDirectives` can **auto-correct** inconsistencies like these, keeping the schema internally consistent in some important ways. In fact, the assertion you see above will *succeed* even though the code makes no effort to update the type of the` Query.people` field to refer to the new` Person` type object.


How does this magic work? In fact, it’s not magic at all:


- First, we assume that each` GraphQLNamedType` object “knows” its own name better than anyone else, which allows us to fix any mismatched property names in the` schema.getTypeMap()` object to match the` name` properties of the corresponding objects.
- Second, we ensure that any two` GraphQLNamedType` objects anywhere in the schema with the same` name` property are` ===` to each other. The crucial assumption here is that the type objects in` schema.getTypeMap()` are authoritative, and any field types, argument types, and/or` union` member types elsewhere in the schema must match the objects found in` schema.getTypeMap()` , if they have the same` name` s.


In case these assumptions are not accurate in some strange situation you’ve created for yourself, you are more than welcome to enforce whatever invariants you prefer, by whatever means necessary. The healing process only makes changes to fix inconsistencies, so it won’t intervene if you find a way to remove the inconsistencies yourself. However, we’ve found that relying on these assumptions (and trusting the healing process) can greatly simplify the implementation of schema directives.


Of course, it’s probably best if you don’t needlessly replace or rename existing type objects, but we’ve got you covered if you do.


## Potential frequently asked questions


### Where can I find more documentation?


A more permanent version of this blog post, with additional examples of schema directives, can be found[here](https://www.apollographql.com/docs/graphql-tools/schema-directives.html) .


### What about query directives?


As its name suggests, the` SchemaDirectiveVisitor` abstraction is specifically designed to enable transforming GraphQL *schemas* based on directives that appear in your SDL text.


While directive syntax can also appear in GraphQL queries sent from the client, implementing query directives would require runtime transformation of query documents. We have deliberately restricted this implementation to transformations that take place when you call the` makeExecutableSchema` function—that is, at schema construction time.


We believe confining this logic to your schema is more sustainable than burdening your clients with it, though you can probably imagine a similar sort of abstraction for implementing query directives. If that possibility becomes a desire that becomes a need for you, let us know, and we may consider supporting query directives in a future version of these tools.


### What about directiveResolvers?


Before` SchemaDirectiveVisitor` was implemented, the` makeExecutableSchema` function took a` directiveResolvers` option that could be used for implementing certain kinds of directives on fields with resolver functions.


The new abstraction is more general, since it can visit any kind of schema syntax, and do much more than just wrap resolver functions. However, the old` directiveResolvers` API has been[left in place](https://www.apollographql.com/docs/graphql-tools/directive-resolvers.html) for backwards compatibility, though it has been reimplemented in terms of` SchemaDirectiveVisitor` .


Existing code that uses` directiveResolvers` should probably consider migrating to` SchemaDirectiveVisitor` if feasible, though we have no immediate plans to deprecate` directiveResolvers` .


## An ecosystem of directives


We’re really excited about these tools, since they have the potential to make GraphQL schema development a lot more convenient. The techniques for implementing custom directives that we’ve presented here are flexible enough to support a whole ecosystem of packages that schema developers will be able to use to streamline their development process. We hope you get inspired to develop and publish your own custom directives!


We decided to ship this feature as soon as we felt the core functionality was complete, but of course we won’t know its full potential without help from the GraphQL community. So please give schema directives a try, let us know about any improvements you think we could make, and teach us about the best practices that you invent along the way.


Written by


Ben Newman


[Read more by Ben Newman](https://www.apollographql.com/blog/author/benjamn)
