---
schema_version: "1.0.0"
document_id: "41e4f5ffebe81e74e9148805bf476864b082173ab60e1a61eed627cfdac1b833"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-rs-graphql-tools-in-rust"
published_at: "2021-11-01T14:36:31+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:9513b3de9ccdaa82bf0507a9fbff83efb26a27ff65a7b361bc63740b7280ef3a"
---

# apollo-rs: spec-compliant GraphQL tools in Rust

At Apollo, we write tools that empower developers to operate graphs of all sizes safely and efficiently. To help us do this, we’ve started adopting Rust. Today, we’re pleased to announce[apollo-rs](https://github.com/apollographql/apollo-rs) : an open-source collection of spec-compliant tools for working with GraphQL in Rust.


` apollo-rs` comes with two components:` apollo-parser` and` apollo-encoder` .` apollo-parser` converts schema text into a native Rust representation. And` apollo-encoder` converts Rust back into schemas. Both components are entirely compliant with the latest GraphQL specification (October 2021) and enable a new generation of GraphQL tools to be written in Rust.


Before we dive into details, here is a quick peek at what using` apollo-parser` is like today:


```text
use apollo_parser::Parser;
use apollo_parser::ast::{Definition};


// Let's create a GraphQL document with just an object
// type definition.
let gql = "
type Pet {
name: String
favSnack: [String!]
}";


// Pass the GraphQL document to our parser, and make sure
// there are no errors.
let parser = Parser::new(gql);
let ast = parser.parse();
assert!(ast.errors().is_empty());


// We now have a parsed schema, and we can walk all of its
// definitions represented direclty through Rust types!
let doc = ast.document();
for def in doc.definitions() {
if let Definition::ObjectTypeDefinition(object) = def {
assert_eq!(object.name().unwrap().text(), "Pet");


for field in object.fields_definition().unwrap().field_definitions() {
// As this loops, it will print the two field
// names it encounters:
// field name: name
// field name: favSnack
println!("field name: {}", field.name().unwrap().text());
}
}
}
```


In this post, we’ll first cover why we’re building` apollo-rs` . Then, we’ll go through the architecture of` apollo-parser` . Finally, we’ll share the future of the` apollo-rs` collection.


## Building reliable GraphQL tooling in Rust


Teams that rely on the Apollo Graph Platform are building graphs with thousands of types and fields. Over the past year, Apollo engineers have explored writing tooling and infrastructure in Rust to help developers run graph workloads of all sizes. This March, we revealed[Rover](https://www.apollographql.com/docs/rover/) — a new version of our command-line interface (CLI) written in Rust. While we loved our experience writing Rust, we learned we needed a reliable set of Rust “building blocks” that could enable us to compile multiple graphs together.


We evaluated existing GraphQL parsers written in Rust, but they didn’t meet our requirements. While building tooling for graph developers, we often find ourselves building implementations that operate on the graph in unique ways — for example, doing schema composition and query planning for Apollo Federation.


As we spin up more and more teams building in Rust, we want them to have a set of uniform interfaces which facilitate interoperability. We’re designing` apollo-rs` to be that library.


In fact,` apollo-rs` is created with a few design principles in mind:


1. **Prioritizing developer experience.** Elegant and ergonomic APIs is the theme for Rust as a language, and we want to make sure that all component APIs we provide are aligned with these principles.
2. **Stability and reliability.** Spec-compliant, and idempotent APIs which, when complete, can be used safely in enterprise-grade codebases.
3. **Diagnostics.** The tools are to be written in a way that will allow us to produce detailed diagnostics. It does not` panic` or return early if there is a lexical or a syntactic error. Instead, the parser is meant to gather as much context and information as possible and return errors *alongside* the output that is valid.
4. **Extensibility.** The parser is written to work with different use cases in our budding Rust GraphQL ecosystem, be it building schema-diagnostics for Rover, or writing out query planning and composition algorithms in Rust. These all have quite different requirements when it comes to AST manipulation. We wanted to make sure we account for them early on.


While our needs may be different than the typical needs of GraphQL consumers, we still think offering a general purpose set of libraries that can support those more particular use-cases is important.


## ` apollo-parser` Architecture


` apollo-parser` is the parser crate of` apollo-rs` . Its job is to take GraphQL queries or schemas as input and produce an Abstract Syntax Tree (AST). Users of` apollo-parser` can then programmatically traverse the AST to get information about their input.


There are three main components to` apollo-parser` : the lexer, the parser and the analyser. We’ve already written the lexer and the parser, and we are in the process of designing and building the analyser! Here is how the parsing pipeline currently works:


An overview of apollo-parser showing the three components: lexer, parser and analyser.


1. The lexer takes input GraphQL and produces tokens based on the input. It provides the guarantee that all tokens are **lexically correct** . The tokens are then passed to the parser.
2. The parser establishes relationships between tokens and arranges them first into an untyped syntax tree. After an untyped syntax tree is complete, it gets converted to a typed syntax tree, with Rust types attached to all syntax tokens in a tree. The parser provides a guarantee that there are no missing tokens in a tree and all tokens are in correct order. That is to say, the tree is **syntactically correct!**
3. In the near future we will be also writing an analyser. It will take the typed syntax tree produced by the parser and provide a semantic model, or a more straightforward way to access and manipulate the syntax tree. The job of the analyser will be to ensure the input is **entirely correct** , that is all types used have been previously defined, all GraphQL operation names used are unique, and other[validation rules](https://spec.graphql.org/October2021/#sec-Validation) .


` apollo-parser` is a hand-written recursive-descent parser. This is a type of parser that starts from the top of a file and recursively walks its way down generating AST nodes along the way. This style of parser is common in industrial-strength compilers; for example, Clang and Rustc use this style of parsing. In particular, recursive-descent parsers make it easier to output helpful diagnostics. They perform well and they’re easier to maintain.


We didn’t start this project off as parser experts; rather, we’re standing on the shoulders of giants. In particular, we want to thank[Aleksey Kladov](https://github.com/matklad) , of the[Rust-Analyzer project](http://github.com/rust-analyzer/rust-analyzer) . Gems such as[“Simple but powerful Pratt Parsing”](https://matklad.github.io/2020/04/13/simple-but-powerful-pratt-parsing.html) have been a guiding beacon when designing our parser.


Let’s talk about the individual parts that make up` apollo-parser` next: the lexer, the parser and the analyser.


### **Lexer**


The lexer’s main responsibility is to create tokens from the input stream it receives. The input stream for us is schema definition language (SDL). When encountering a` (` or a` 47` , it creates` LPAREN` and` INT` tokens. It is also responsible for checking if the input is lexically correct and producing errors if it’s not. We designed the lexer to be **error resilient** . This means the *lexer never fails!* When encountering an error during lexing, instead of exiting early and returning only the error, we return all valid tokens *alongside* the error that occurs.This sets up` apollo-parser` with room to produce useful diagnostics for users.


### **Parser**


Our lexer returns a vector of tokens, and a vector of errors. If no errors were returned, we can be sure that our input program is lexically valid. That is to say: we’re sure there are no invalid tokens in our input. That’s one less thing to worry about!


The next step in our parsing pipeline is the **parser.** The parser’s job is to take the tokens produced by the lexer and create nodes with information and relationships that in the end make up a syntax tree. Much like with the lexer, the parser is **error resilient.** Syntactic errors, such as a missing` Name` in a` ScalarDefinition` , are added to parser’s error vector while the parser carries on parsing.


A diagram of how tokens created by the lexer are arranged into a syntax tree by the parser.


As mentioned in the beginning our parsing is done in two steps: first an untyped syntax tree, then a typed syntax tree. Let’s take a look at some of the details!


#### **Parser’s Untyped Syntax Tree**


We first create an untyped syntax tree when we manually parse incoming tokens. This tree is stored with the help of` <a href="https://docs.rs/rowan">rowan</a>` crate, a really quite excellent library written by the rust-analyzer team.` rowan` creates a[Red/Green tree](https://blog.yaakov.online/red-green-trees/) , which is an efficient way of representing ASTs that can be updated over time. This is a common technique used in many modern compilers such as Rust-Analyzer and the Swift compiler.


The untyped tree stores information about the nodes, such as the token’s data and its relationship to other tokens in the tree, but not Rust type data; that comes later. We build the tree as we walk down the list of tokens. This is, for example, how we build the tree for a` ScalarTypeDefinition` :


```text
# schema.graphql


scalar UUID @specifiedBy(url:"cats.com/cool-kitten-schema")
```


The parser for the scalar is built something like this:


```text
// grammar/scalar.rs


/// See: https://spec.graphql.org/October2021/#ScalarTypeDefinition
///
/// ScalarTypeDefinition =
///   Description? 'scalar' Name Directives?
pub(crate) fn scalar_type_definition(parser: &mut Parser) {


// We already know this is a Scalar Type, so we
// start a SCALAR_TYPE_DEFINITION node.
//
// This is not yet an actual Rust type, but a simple
// enum that later gets converted to a Rust type.
let _guard = parser.start_node(SyntaxKind::SCALAR_TYPE_DEFINITION);


// Descriptions are optional, so we just check whether
// or not the lexer provided us with a token that
// represents a description and add it to the node we
// started above.
if let Some(TokenKind::StringValue) = parser.peek() {
description::description(parser);
}


// Add the "scalar" keyword to the node.
if let Some("scalar") = parser.peek_data().as_deref() {
parser.bump(SyntaxKind::scalar_KW);
}


// A Scalar Type must have a Name. If it doesn't have a
// Name token, we add an error to our parser's error
// vector and don't add anything to the node.
match parser.peek() {
Some(TokenKind::Name) => name::name(parser),
_ => parser.err("expected a Name"),
}


// Finally, we check if a directive was provided and add
// it to the current node.
if let Some(TokenKind![@]) = parser.peek() {
directive::directives(parser);
}


// This is the end of the ScalarTypeDefinition parsing.
// The SCALAR_TYPE_DEFINITION node automatically
// gets closed and added to the current untyped
// syntax tree.
}
```


#### **Parser’s Typed Syntax Tree**


Once the incoming token stream is done parsing, we create a typed syntax tree, which is the basis of the parser’s API.


The accessor methods to the typed tree are generated using[ungrammar](https://docs.rs/ungrammar) crate, another great Rust-Analyzer Team Original(TM). Ungrammar is a domain-specific language (DSL) that allows us to specify the shape of our syntax tree. If you’re interested to learn more about this crate’s design, you can read about it[in this post](https://rust-analyzer.github.io/blog/2020/10/24/introducing-ungrammar.html) .


So, how do we actually specify what our syntax tree should look like? Here is a small example of how we do it for` ScalarTypeDefinition` :


```text
// graphql.ungram


ScalarTypeDefinition =
Description? 'scalar' Name Directives?
```


Given this definition, we can then generate a` struct` with applicable accessor methods for` Description` , scalar token,` Name` and` Directives` :


```text
// generated nodes.rs file


#[derive(Debug, Clone, PartialEq, Eq, Hash)]
pub struct ScalarTypeDefinition {
pub(crate) syntax: SyntaxNode,
}


impl ScalarTypeDefinition {
pub fn description(&self) -> Option<Description> {
support::child(&self.syntax)
}
pub fn scalar_token(&self) -> Option<SyntaxToken> {
support::token(&self.syntax, S![scalar])
}
pub fn name(&self) -> Option<Name> {
support::child(&self.syntax)
}
pub fn directives(&self) -> Option<Directives> {
support::child(&self.syntax)
}
}
```


Here is what it looks like to walk a` ScalarDefinition` and get its` Name` from the syntax tree using the above` name()` accessor method:


```text
use apollo_parser::Parser;
use apollo_parser::ast::{Definition, ObjectTypeDefinition};


// Let's create a GraphQL document with just a
// scalar type definition.
let gql = r#"scalar UUID @specifiedBy(url: "cats.com/cool-kitten-schema")"#;


// Parse the input data.
let parser = Parser::new(gql);
let ast = parser.parse();


// Make sure the are no errors.
assert!(ast.errors.is_empty());


// Check that the Scalar's name is indeed UUID.
let doc = ast.document();
for def in doc.definitions() {
if let Definition::ScalarTypeDefinition(scalar) = def {
assert_eq!("UUID", scalar.name().unwrap().text().to_string());
}
}
```


And that’s what we have today! So, what comes after this?


## What’s next: A semantic analyser


You might have noticed that the Nodes API returns` Option<Name>` , rather than just` Name` . That’s because we’re not yet sure if our Nodes are all correct, and still need to run our nodes through semantic analysis before we can return just` Name` . This is the next thing we will be working on.


Not only do we want lexically and syntactically correct programs, but we also want semantically valid programs. Let’s say our schema consists only of the following:


```text
# schema.graphql


type Person implements NamedEntity {
name: String
age: Int
}
```


We can see that` NamedEntity` is nowhere to be seen, which makes this schema semantically incorrect. This should be reflected in the error messaging:


```text
error[E0405]: cannot find interface `NamedEntity` in this scope
--> schema.graphql:24:35
|
1 | type Person implements NamedEntity {
|                        ^^^^^^^^^^^ not found in this scope
```


Aside from correctly identifying issues in the code, we intend to write an API on top of the model that semantic analysis will produce. This will allow for more efficient schema querying and provide easily accessible information about things like queries, types and their subtypes, mutations, and directives. We’d like this API to answer questions like “where is a given directive being used”, “what are the dependencies of a given type”, and “what is the type that this field returns”.


### Delightful diagnostics


As you might’ve noticed from the previous sections, we’ve put a lot of work in making sure that we don’t fail the parser if it encounters an error. Whether something’s invalid, missing, or perhaps just in the wrong place, we want to share this information back with the users. Errors, warnings, hints, and suggestions provided by a compiler are what we collectively refer to as “diagnostics”.


This is something we’re still working on, but we’ve got the foundation in place! To give you an example of what we mean; take this schema, for example:


```text
scalar UUID @specifiedBy(url:"cats.com/cool-kitten-schema")
```


Say we tried copying it over by hand, and accidentally forgot to give it a name. We might end up with this:


```text
scalar @specifiedBy(url:"cats.com/cool-kitten-schema")
```


Not only do we want the parser to let us know we have an error, but we’d also like the parser to tell us what went wrong, where it went wrong, and possibly even tell us how we can fix it. And with` apollo-parser` , we have all the information needed to create those kinds of errors. If we look at what the parser returns today, we can see this in action:


```text
PS C:\Users\shest\Code\apollographql\apollo-rs>


running 1 test
- DOCUMENT@0..63
- SCALAR_TYPE_DEFINITION@0..63
- scalar_KW@0..6 "scalar"
- WHITESPACE@6..7 " "
- DIRECTIVES@7..63
- DIRECTIVE@7..63
- AT@7..8 "@"
- NAME@8..19
- IDENT@8..19 "specifiedBy"
- ARGUMENTS@19..63
- L_PAREN@19..20 "("
- ARGUMENT@20..62
- NAME@20..23
- IDENT@20..23 "url"
- COLON@23..24 ":"
- WHITESPACE@24..25 " "
- STRING_VALUE@25..62
- STRING@25..62 "\"cats.com/cool-kitten-schema\""
- R_PAREN@62..63 ")"
- ERROR@7:8 "expected a Name" @
```


Because our parser is resilient to errors, we keep going even after realizing we’re missing a name. And because our parser is lossless (i.e. we keep *all* user input), all of the original input is included. Put together, this is enough information to create helpful diagnostics.


## Come join us! 🦀


In this post we introduced` apollo-rs` , a collection of GraphQL tools in Rust, and shared an early look at one of its components,` apollo-parser` . We’ve described the reasons for building it, covered the various stages of the parser, and shown how its properties allow us to generate helpful diagnostics.


You can find the project on[GitHub](https://github.com/apollographql/apollo-rs) . I’m very excited to finally be able to share this project with the world, and hope you found it interesting to dive into it with me!


If building Rust tooling for the graph ecosystem sounds exciting, I hope you’ll consider joining us![We have several Rust engineering positions open now](https://www.apollographql.com/careers/#positions) , with even more to come.


Written by


Irina Shestak


[Read more by Irina Shestak](https://www.apollographql.com/blog/author/irina)
