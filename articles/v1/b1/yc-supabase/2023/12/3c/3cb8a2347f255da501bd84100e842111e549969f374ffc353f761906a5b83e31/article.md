---
schema_version: "1.0.0"
document_id: "3cb8a2347f255da501bd84100e842111e549969f374ffc353f761906a5b83e31"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgres-language-server-implementing-parser"
published_at: "2023-12-08T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:b9a9dc15e398730b75e0883b61d549c71f1959fd966ede69a2826cc9b0d99827"
---

# Postgres Language Server: implementing the Parser

During our previous Launch Week we[announced](https://news.ycombinator.com/item?id=37020610) the development of a[Postgres Language Server](https://github.com/supabase/postgres_lsp) . The response was more enthusiastic than we imagined and we received some excellent ideas.


This is an update on the Parser, which is the fundamental primitive of a Language Server. We’ve been through several iterations of the parser and we want to detail our explorations.


Want to learn some more acronyms?


There will be a few acronyms in this post. We don’t want this post to be “inside baseball” so here are a few concepts that you need to know:


- ***LSP / Language Server Protocol:*** Something that helps code editors understand code better. It provides functionality like linting and error detection.
- **Postgres Language Server:** a server that will help with Postgres programming - writing SQL, type inference, etc.
- **Parser** : A parser converts written code into a form that tools can work with. For example, it identifies variables in code. and then the tools can easily access those variables. We’ll describe this more below.


## Background: Why build a Language Server?#


Postgres is gaining popularity, and yet the tooling around it still needs a lot of work. Writing SQL in editors like VSCode is a pain. One of the unique features of Supabase is the ability to access your Postgres database from a browser or mobile app through our[Data APIs](https://supabase.com/docs/guides/api) . This means that developers are writing more[PL/pgSQL](https://www.postgresql.org/docs/current/plpgsql.html) .


While code editors have great support for most programming languages, SQL support is underwhelming. We want to make Postgres as simple as Python.


## Language Server's Core: The Role of the Parser#


On the highest level, a language server is a thing which


1. accepts input source code from the client
2. turns it into a semantic model of the code
3. and provides language-specific smarts back to the client.


The parser is the core of every language server. It takes the raw string, turns it into a stream of tokens, and builds a syntax tree. This syntax tree can then be used to extract structural information.


Usually, the parser builds a concrete[syntax tree](https://en.wikipedia.org/wiki/Parse_tree) (CST) before turning it into an[abstract syntax tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) (AST). A CST preserves all syntax elements present in the source code with the goal of being able to re-create the exact original source, while an AST contains only the meaning of the source. For example, take a simple expression` 2 * (7 + 3)` :


`
_15


CST AST


_15


----- -----


_15


expr *


_15


/ | \\ / \\


_15


term * term 2 +


_15


| | / \\


_15


factor factor 7 3


_15


| / | \\


_15


2 ( expr )


_15


/ | \\


_15


term + term


_15


| |


_15


factor factor


_15


| |


_15


7 3


`


## Implementing a Parser for Postgres#


Implementing a parser for Postgres is challenging because of the ever-evolving and complex syntax of Postgres. Even` select` statements are very complex to parse. Then there are common table expressions, sub-queries and the like. This is one of the reasons why existing Postgres tooling is scarce, badly maintained, and often does not work well.


We decided to not create a custom parser. Instead, we leverage[libpg_query](https://github.com/pganalyze/libpg_query) to parse SQL code reliably. The pganalyze team has a published a great blog post on[why this approach is preferable](https://pganalyze.com/blog/parse-postgresql-queries-in-ruby) .


However, libpg *query is designed to parse _executable* SQL — not to provide language intelligence. Using it for a language server means adapting it to our specific use case. Let’s explore how we adapted it:


### Tokenization#


Before any syntax tree can be built, the input string needs to be converted into a stream of tokens. libpg_query exposes a` scan` API that returns all non-whitespace tokens of the source, even for invalid SQL. For example, a simple statement` select '1';` returns


`
_18


ScanToken {


_18


start: 0,


_18


end: 6,


_18


token: Select,


_18


keyword_kind: ReservedKeyword,


_18


},


_18


ScanToken {


_18


start: 7,


_18


end: 10,


_18


token: Sconst,


_18


keyword_kind: NoKeyword,


_18


},


_18


ScanToken {


_18


start: 10,


_18


end: 11,


_18


token: Ascii59,


_18


keyword_kind: NoKeyword,


_18


}


`


Every` ScanToken` token contains its variant, a range, and a keyword kind. To simplify the implementation of the parser, we extract the text for each token using the range for now. We arrive at the following` Token` struct.


`
_10


pub struct Token {


_10


/// The kind of the token


_10


pub kind: SyntaxKind,


_10


/// Text from the input


_10


pub text: String,


_10


/// Range within the input


_10


pub span: TextRange,


_10


/// Variants from \`ScanToken.keyword_kind\` + \`Whitespace\`


_10


pub token_type: TokenType,


_10


}


`


To have a complete token stream, we merge the results of` scan` with a list of all whitespace tokens in the source, where the latter are extracted by a simple regular expression. For a simple statement` select '1';` , the following tokens are returned by the lexer.


`
_26


\[


_26


Token {


_26


kind: Select,


_26


text: "select",


_26


span: 0..6,


_26


token_type: ReservedKeyword,


_26


},


_26


Token {


_26


kind: Whitespace,


_26


text: " ",


_26


span: 6..7,


_26


token_type: Whitespace,


_26


},


_26


Token {


_26


kind: Sconst,


_26


text: "'1'",


_26


span: 7..10,


_26


token_type: NoKeyword,


_26


},


_26


Token {


_26


kind: Ascii59,


_26


text: ";",


_26


span: 10..11,


_26


token_type: NoKeyword,


_26


},


_26


\]


`


### Conversion to a Syntax Tree#


To transform the stream of tokens into a syntax tree, we face the first challenges with` libpg_query` . In a language server, it's important to handle incomplete or improperly formatted input gracefully. When an error occurs you don’t want the parser to “stop”. You want it to check for *all* errors in your code:


`
_10


create table posts (


_10


id serial primary key # <- ERR: missing comma, Parser should continue


_10


content text


_10


);


_10


_10


create table comments (


_10


id serial primary key,


_10


post_id int references posts # <- ERR: missing comma, second error returned


_10


comment text


_10


);


`


Unfortunately, the` parse` api from` libpg_query` only parses the entire input — if any SQL statement contains a syntax error, an error is returned for the entire input.


To overcome this, we implemented a resilient` source` parser. This parser breaks the input into individual SQL statements and parses them one by one, allowing us to handle syntax errors within each statement independently. It is implemented as a top-down[LL parser](https://en.wikipedia.org/wiki/LL_parser) . Specifically, the parser iterates the token stream left-to-right, and checks if the cursor currently is at the start of a new statement. Once a statement is entered, it walks the tokens until` ;` or another statement start is encountered while skipping sub-statements.


Luckily, Postgres statements always start with distinct keywords. An update statement is identifiable with` update` , a delete statement with` delete from` . There are a few statements that need more than the first few tokens to be distinguishable, but we only care about whether there is a statement, and not what statement there is exactly, so ambiguity is very much acceptable.


For the implementation, we only need to provide the distinct keywords each statement starts with and compare it to the current tokens using a lookahead mechanism.


### Reverse-Engineering the CST#


The second limitation we encountered:` libpg_query` only exposes an API for the AST, not for the CST. To provide language intelligence on the source code, both are required. Since we do not want to implement our own parser, we need to work with what we have to build the CST: the AST and a stream of tokens. The goal is to reverse-engineer the AST into the CST. This involves re-using the AST nodes as CST nodes, and figuring out what token belongs beneath what node. The exemplary statement` select '1';` should be be parsed into


`
_10


SelectStmt@0..11


_10


Select@0..6 "select"


_10


Whitespace@6..7 " "


_10


ResTarget@7..10


_10


AConst@7..10


_10


Sconst@7..10 "'1'"


_10


Ascii59@10..11 ";"


`


To do that, we need to know the range of every node that is within the AST.


We made *numerous* iterations over the past few months to figure out how to accomplish this with minimal manual implementations. Before diving into details, lets take a closer look at the` parse` API of` libpg_query` . For the exemplary statement above, it returns (simplified for readability):


`
_23


SelectStmt {


_23


target_list: \[


_23


ResTarget {


_23


val: Some(


_23


Node {


_23


node: Some(


_23


AConst {


_23


location: 7,


_23


val: Some(


_23


Sval(


_23


String {


_23


sval: "1",


_23


},


_23


),


_23


),


_23


},


_23


),


_23


},


_23


),


_23


location: 7,


_23


},


_23


\],


_23


}


`


There are a few limitations:


1. Some nodes do have a` location` property that indicates where the node starts in the source, but not all.
2. There is no information on the range of a node within the source.
3. Some locations are not what you would expect. For example the location of the expression node is the location of the operator, not the start of the left expression.


To summarise: *we need a range for each node, and we only have a start position, but not always, and sometimes it is wrong.*


Our first very iterations were naive. We explored what information we could extract from` scan` and` parse` , and if anything can help in reverse-engineering the CST.


It turns out, the most reliable way of determining the range of a node is by knowing all **properties** of that node, and its position in the tree.


A property is any text for which a Token can potentially be found in the source code. For example, a` SelectStmt` node has the` select` keyword as a property, and if there is a` from_clause` , a` from` keyword. For the exemplary statement above, the properties are


`
_10


SelectStmt: \[Select\],


_10


RangeVar: \[\],


_10


AConst: \['1'\]


`


Note that we do not have an extra` String` node, and instead add its properties to the parent` AConst` node. This reason is that a` String` node does not bring any value to the CST, since we already know that` ‘1'` is a string from the kind of the respective` Token` . The same is true for all nodes that just contain type information such as` Integer` ,` Boolean` and` Float` .


The position of any node is the same in AST and CST, and thereby can be reflected from it.


### Implementation#


Before the actual parsing begins, the AST returned by` parse` is converted into an uniform tree structure where each node holds the kind of the node, a list of properties, the depth and, if available, the location.


For example, for` select '1';` :


`
_39


edges: (0, 1), (1, 2),


_39


nodes: {


_39


0: Node {


_39


kind: SelectStmt,


_39


depth: 1,


_39


properties: \[


_39


TokenProperty {


_39


value: None,


_39


kind: Some(


_39


Select,


_39


),


_39


},


_39


\],


_39


location: None,


_39


},


_39


1: Node {


_39


kind: ResTarget,


_39


depth: 2,


_39


properties: \[\],


_39


location: Some(


_39


7,


_39


),


_39


},


_39


2: Node {


_39


kind: AConst,


_39


depth: 3,


_39


properties: \[


_39


TokenProperty {


_39


value: Some(


_39


"1",


_39


),


_39


kind: None,


_39


},


_39


\],


_39


location: Some(


_39


7,


_39


),


_39


},


_39


},


`


To implement such a conversion we need a way to


- get an` Option<usize>` with the location for every node type, if any
- compile a list of all properties that a node contains
- walk down the AST until the leaves are reached


Due to the strict type system of Rust, a manual implementation would be a significant and repetitive effort. With languages such as JavaScript, getting the location of a node would be as simple as` node?.location` . In Rust, a large match statement covering all possible nodes is required to do the same. Luckily,` libpg_query` exports a protobuf definition containing all AST nodes and their fields. For example, an` AExpr` node is defined as


`
_10


message A_Expr


_10


{


_10


A_Expr_Kind kind = 1 \[json_name="kind"\];


_10


repeated Node name = 2 \[json_name="name"\];


_10


Node lexpr = 3 \[json_name="lexpr"\];


_10


Node rexpr = 4 \[json_name="rexpr"\];


_10


int32 location = 5 \[json_name="location"\];


_10


}


`


We introspect that definition to generate code at build time using[procedural macros](https://doc.rust-lang.org/reference/procedural-macros.html) .


Leveraging the powerful repetition feature of the[quote](https://github.com/dtolnay/quote) crate, the` match` statement of a` get_location` function can be implemented with just a few lines of code.


`
_20


pub fn get_location_mod(_item: proc_macro2::TokenStream) -> proc_macro2::TokenStream {


_20


// Parse the proto file using a custom parser


_20


let parser = ProtoParser::new("libpg_query/protobuf/pg_query.proto");


_20


let proto_file = parser.parse();


_20


_20


// get all node identifiers for the left side of the match arm


_20


let node_identifiers = node_identifiers(&proto_file.nodes);


_20


// get a TokenStream for each node that returns the location


_20


// if it is part of the node properties


_20


let location_idents = location_idents(&proto_file.nodes);


_20


_20


quote! {


_20


/// Returns the location of a node


_20


pub fn get_location(node: &NodeEnum) -> Option<usize> {


_20


match node {


_20


#(NodeEnum::#node_identifiers(n) => #location_idents),*


_20


}


_20


}


_20


}


_20


}


`


The` location_idents` function iterates all nodes, searches for a` location` property in the protobuf definition for each node, and returns a` TokenStream` with either` Some(n.location)` or` None` for each.


`
_18


fn location_idents(nodes: &\[Node\]) -> Vec<TokenStream> {


_18


nodes


_18


.iter()


_18


.map(|node| {


_18


if node


_18


.fields


_18


.iter()


_18


// has location property?


_18


.find(|n| n.name == "location" && n.field_type == FieldType::Int32)


_18


.is_some()


_18


{


_18


quote! { Some(n.location) }


_18


} else {


_18


quote! { None }


_18


}


_18


})


_18


.collect()


_18


}


`


Similarly, we can generate code to recursively walk down the` FieldType == Node` and` FieldType == Vec<Node>` properties of the AST nodes. No manual work required.


Even the function that returns all properties for a node can be generated, at least partly. All AST fields of` FieldType == String` can always be added to the list of properties. In the example above, the` sval: “1”` of the` String` node makes up the properties of its parent, the` AConst` node. What is remaining are mostly just the keywords that need to be defined for every node. A` SelectStmt` node has the` select` keyword as a property, and if there is a` from_clause` , a` from` keyword.


`
_11


fn custom_handlers(node: &Node) -> TokenStream {


_11


match node.name.as_str() {


_11


"SelectStmt" => quote! {


_11


tokens.push(TokenProperty::from(Token::Select));


_11


if n.from_clause.len() > 0 {


_11


tokens.push(TokenProperty::from(Token::From));


_11


}


_11


...


_11


},


_11


...


_11


}


`


### Parsing a Statement#


After the tree has been generated, the parser goes through the tokens and finds the node in whose properties the current token can be found. But not every node is a possible successor. Lets look how the parser builds the CST for the statement` select '1' from contact` at a high level.


We start with the full tree, and all tokens:


`
_10


Remaining Tokens: \["select", "'1'", "from", "contact"\]


_10


_10


Parse Tree:


_10


_10


SelectStmt (0, \[Select, From\])


_10


/ \\


_10


1 (ResTarget, \[\]) 2 (RangeVar, \['contact'\])


_10


|


_10


3 (AConst, \['1'\])


`


Starting with the root node, the parser first searches the current node for the token. In this case, with success.` Select` is removed from` SelectStmt` .


In the next iteration, we search for` '1'` . Since its not in the current node, a breadth-first search is used to find the property within children nodes. We arrive at` AConst` , open all nodes we encountered along the way, and advance.


`
_17


Remaining Tokens: \["from", "contact"\]


_17


_17


Parse Tree:


_17


_17


SelectStmt (0, \[From\])


_17


/ \\


_17


1 (ResTarget, \[\]) 2 (RangeVar, \['contact'\])


_17


|


_17


3 (AConst, \[\])


_17


_17


CST:


_17


SelectStmt


_17


Select@0..6 "select"


_17


Whitespace@6..7 " "


_17


ResTarget@7..10


_17


AConst@7..10


_17


Sconst@7..10 "'1'"


`


Since we arrived at a leaf node with no properties left, the next token can not be part of this node or any child. It can be closed immediately after advancing the token. We remove it from the tree and set the current node to its parent. The same now applies to` ResTarget` , so we arrive back at` SelectStmt` :


`
_15


Remaining Tokens: \["from", "contact"\]


_15


_15


Parse Tree:


_15


_15


SelectStmt (0, \[From\])


_15


\\


_15


2 (RangeVar, \['contact'\])


_15


_15


CST:


_15


SelectStmt


_15


Select@0..6 "select"


_15


Whitespace@6..7 " "


_15


ResTarget@7..10


_15


AConst@7..10


_15


Sconst@7..10 "'1'"


`


The` from` token can once again be found within the current node and we just advance the parser. Since` SelectStmt` is not a leaf node, we stay where we are.


`
_17


Remaining Tokens: \["contact"\]


_17


_17


Parse Tree:


_17


_17


SelectStmt (0, \[\])


_17


\\


_17


2 (RangeVar, \['contact'\])


_17


_17


CST:


_17


SelectStmt


_17


Select@0..6 "select"


_17


Whitespace@6..7 " "


_17


ResTarget@7..10


_17


AConst@7..10


_17


Sconst@7..10 "'1'"


_17


Whitespace@10..11 " "


_17


From@11..15 "from"


`


From here, it repeats itself:` contact` is found within` RangeVar` using a breadth-first search. It becomes a leaf node that is closed after applying the token. Since no tokens are left, we finish parsing by closing` SelectStmt` , resulting in:


`
_11


SelectStmt@0..23


_11


Select@0..6 "select"


_11


Whitespace@6..7 " "


_11


ResTarget@7..10


_11


AConst@7..10


_11


Sconst@7..10 "'1'"


_11


Whitespace@10..11 " "


_11


From@11..15 "from"


_11


Whitespace@15..16 " "


_11


RangeVar@16..23


_11


Ident@16..23 "contact"


`


Keep in mind, this illustration shows you only an overview of the process.If you are interested in the details, take a look at the source code[here](https://github.com/supabase/postgres_lsp/blob/2bb50e7b0733bd4cf630aba453d5132d03dfc113/crates/parser/src/parse/libpg_query_node.rs) .


You may have noticed that neither` location` nor` depth` were mentioned. Both are only used to improve performance and safeguard. Among other things, branches with nodes behind the current position of the parser are skipped. Further, the parser panics when a node is opened and either its position or its depth does not match the current state of the parser. This means that the returned CST is guaranteed to be correct.


### Limitations#


If the SQL is invalid and` parse` returns an error, the returned CST is just a flat list of tokens. Consequently, the statement parser is not resilient. This is not great, but we have intentionally implemented it so that custom and resilient implementations can be added statement by statement later.


Ultimately, we want the` libpg_query` -based parser to just serve as a fallback. For now however, our goal is to provide a usable language server as fast as possible. And even if some intelligence features will only work on valid statements, we believe it is still better than what we have today: no intelligence at all.


## Next Steps#


There are some minor improvements remaining for the parser. But the largest part are the manual implementations missing in` get_node_properties` . Its a time-consuming effort, and ***we would love to get help from the community*** here. Check out[this issue](https://github.com/supabase/postgres_lsp/issues/51) if you like to support.


After that, we will move on to the semantic data model and the language server itself. Other parser features such as support for[PL/pgSQL](https://www.postgresql.org/docs/current/plpgsql.html) function body parsing will be added later. We want to get this project into a usable state as fast as possible.
