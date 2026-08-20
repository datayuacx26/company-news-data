---
schema_version: "1.0.0"
document_id: "a9bbf540975eeaa8eec647da1818c9c8edb18e886bd35915033c0cefeb7058f2"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/using-grpc-with-golang/"
published_at: "2024-12-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:aa974e0839faf10059274856af1bea4c4a2ec399747461765f863f7ba468ce12"
---

# gRPC Golang Example: Using gRPC with Golang

In this tutorial, you will learn how to work with the gRPC Golang library for microservice communication by creating a simple note-taking application. You will generate a gRPC client that is highly efficient and has a service implementation that handles a diverse range of request and response types. APIs and service-to-service communication are what make modern microservice architecture possible. REST is generally the preferred implementation pattern, but if you only use REST, you could miss out on the significant performance gains that gRPC can offer. gRPC can provide[better speed and efficiency](https://blog.dreamfactory.com/grpc-vs-rest-how-does-grpc-compare-with-traditional-rest-apis) than REST APIs.


## What Is gRPC?


[gRPC](https://grpc.io/) is a modern open-source remote procedure framework for remote procedure calls (RPC, as in gRPC) that facilitates communication between distributed applications. RPC is an *action-based* paradigm, similar to remotely calling a function from another microservice. This makes gRPC a type of[inter-process communication (IPC)](https://geeksforgeeks.org/inter-process-communication-ipc) protocol built around[Protobufs](https://developers.google.com/protocol-buffers) to handle messaging between the client and the server. gRPC is perfect for intensive and efficient communication because it supports server and client-side streaming. In contrast, REST is a *resource-based* protocol, which means the client tells the server what resource needs to be created, read, updated, or deleted based on the body of the request. Whereas RESTful services chiefly send requests for resource information, gRPC clients receive information based on actions. This difference means you can’t just translate your REST API into gRPC, as the core way in which each system responds - and the response message types they generate - are functionally different. Instead, you need to adapt your design to the other protocol:


## Why Is gRPC Faster than REST?


gRPC code utilizes the[HTTP/2 protocol](https://pingdom.com/blog/will-http-2-make-your-site-faster#:~:text=Published%20in%20early%202015%2C%20HTTP,without%20changing%20your%20web%20applications.) , which offers multiple ways to improve performance:


- Header compression and reuse to reduce the message size
- Multiplexing to send multiple requests and receive multiple responses simultaneously over a single TCP connection
- Persistent TCP connections for multiple sequential requests and responses on a single TCP connection
- Binary format support, such as protocol buffers


Protocol buffers are a key element in gRPC. They provide more efficient binary data representation than other text-based formats such as JSON or XML. Because of their binary format, message sizes are significantly smaller. The combination of smaller messages and faster communication offers stronger performance than REST-based APIs. Now that you have a basic understanding of gRPC, you’re going to implement a simple client-server application in Go.


## Protocol Buffers


Protocol Buffers (protobufs) is a platform-neutral language for structuring data with built-in fast serialization and support for schema migration. They are used to encode message semantics in a parsable form that the client and server can share. The latest version of the protobuf syntax is proto3. Protocol Buffers are used to define the structure of the data that will be exchanged between the client and server. They are compiled into language-specific code using the Protocol Buffer Compiler (protoc). This code can then be used to serialize and deserialize the data. Protocol Buffers have several benefits, including:


- Efficient serialization and deserialization
- Platform independence
- Schema migration support
- Language independence


By using Protocol Buffers, you ensure that your data is compact, efficient, and easy to work with across different programming languages and platforms. This makes them an ideal choice for defining the data structures in your gRPC web services.


## Defining a gRPC Service


A gRPC service is defined using a .proto file. The photo file exposes information via the protocol compiler. This file contains the definition of the service, including the gRPC methods (and general RPC methods) that can be called remotely, their parameters, and return types. To define a gRPC service, you need to specify the following:


- The service name
- The methods that can be called remotely
- The parameters and return types for each method request


For example, the following .proto file defines a simple gRPC service: syntax = “proto3”;


```text
package   example  ;
service Greeter { rpc SayHello (HelloRequest) returns (HelloResponse) {} }
message HelloRequest {   string   name   =   1  ; }
```


This service has one method, **SayHello** , which takes a **HelloRequest** message as input and returns a **HelloResponse** message. The **.proto** file serves as a contract between the client and server, ensuring that both sides understand the structure and types of the data being exchanged.


## Implementing gRPC


To see gRPC in action, you’re first going to build a small note-taking application. For this gRPC Golang example, you’ll use the client to send notes to the server that can be archived and retrieved based on keywords. Additionally, you will set up a new gRPC server in Go to handle incoming TCP connections and register endpoints. The tutorial uses Go 1.17 and the[gRPC-Go](https://github.com/grpc/grpc-go) library 1.45.0. You can find all the code in[this GitHub repo](https://github.com/xNok/go-grpc-demo) .


### Creating the Go Project


Start building your application by initializing a new Go project. Be sure to replace the name in the command line with the name of your repository:


```text
go   mod   init   github.com/xNok/go-grpc-demo
```


Install the gRPC-Go library:


```text
go   mod   edit   -require=google.golang.org/grpc@v1.45.0
go   mod   download
```


Install the protocol buffer compiler (` protoc` ) using the instructions that match your operating system, according to the[installation docs](https://grpc.io/docs/protoc-installation/) . For Linux, this tutorial uses the following command:


```text
sudo   apt   install   protobuf-compiler
```


Next, install Golang code generators for the protocol buffer:


```text
go   install   google.golang.org/protobuf/cmd/protoc-gen-go@v1.26
go   install   google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1
```


The binaries for these two tools need to be in your path. For Linux, use:


```text
export   PATH  =  "  $PATH  :$(  go   env GOPATH)/bin"
```


### Generating Code with the Protocol Buffer Compiler


Once you have defined your gRPC service using a .proto file, you need to generate the code for the client and server using the Protocol Buffer Compiler (protoc). The protoc compiler takes the .proto file as input and generates language-specific code for the client and server. This code includes the service gRPC interface, the client and server stubs, and the serialization and deserialization code. To generate the code, you need to run the following command:


```text
protoc   --go_out=.   example.proto
```


This will generate the server and client code in the current directory. The generated code includes the service interface and the necessary methods defined to serialize and deserialize the data, making it easier to implement the gRPC service and client.


### Implementing the Application Logic


It is considered good practice to separate the business logic from the server code. To do this, you’ll create a notes folder for your application’s business logic. Protocol buffers provide more than a fast and efficient data format; they come with a rich ecosystem of tools to make development easier. A declarative .proto file describes your data structures (messages) and application interfaces (services). This file is simple to write and gives a high-level definition of what your application will be doing. It’s also language-agnostic and can generate the code required to manipulate these data structures. Begin by creating a **notes.proto** file in the notes folder. A . **proto** file starts by indicating which version of the protocol buffer you’re using, the name of the package to prevent collision, and a series of options to help the code generator. In your case, you only need to specify the name of the Go package where the generated files belong:


```text
syntax = "proto3";
package notes;
option go_package = "github.com/xNok/go-grpc-demo;notes";
```


In` option go_package` ,` github.com/xNok/go-grpc-demo` is the name of the module matching the name of the GitHub repo, and` notes` defines the package for the generated code.


### Defining the Data Structure of the Service


Next, define the data structure:


```text
// The notes service definition.
service Notes {
// Saving a note
rpc Save (Note) returns (NoteSaveReply) {}
// Retrieving a note
rpc Load (NoteSearch) returns (Note) {}
}
// The request message containing the note title
message Note {
string   title   =   1  ;
bytes body    =   2  ;
}
// The response message confirming if the note is saved
message NoteSaveReply {
bool   saved   =   1  ;
}
// The request message containing the note title
message NoteSearch {
string   keyword   =   1  ;
}
```


You’ve created a service called` Notes` that can perform two actions:` Save` a note and` Load` a note. You also specified that you want to use` rpc` for these functions as the communication protocol. The` Save` function takes a` Note` (composed of a` title` and a` body` ) as an input and returns` NoteSaveReply` , which contains a Boolean to confirm if the note was saved. The` Load` function takes the` NoteSearch` message as input that provides your desired keyword and returns a` Note` .


The syntax differs from Go` struct` and` interface` but should be easy to learn. Note that the types aren’t the same as in Go. Check the[language guide](https://developers.google.com/protocol-buffers/docs/proto3) to start on the right foundation.


Finally, note the attributes. Attributes are always assigned an ID; for instance, in the` Note` message, the` title` has ID` 1` and the` body` has ID` 2` .


The data-structure definition is now complete. Next, you’ll generate the related code using protoc, the compiler for protocol buffer definitions files you installed earlier:


```text
protoc   --go_out=.   --go_opt=paths=source_relative
--go-grpc_out  =  .   --go-grpc_opt  =  paths  =  source_relative
notes/notes.proto
```


The command generates two new files: notes.pb.go, containing the code to handle the data serialization/deserialization for the protocol buffer; and notes_grpc.pb.go to handle the gRPC communication protocol. These files represent several hundreds of lines of code you don’t have to write.


You now have the structure of your application but still need some business logic. Create a file called note.go in the notes folder. You’ll code the logic here:


```text
package   notes
import   (
"  errors  "
"  io/ioutil  "
"  log  "
"  os  "
"  path/filepath  "
"  strings  "
)
// Save a Note to the disk with the title as filename
func   SaveToDisk  (  n   *  Note  ,   folder   string  )   error   {
filename   :=   filepath.  Join  (folder, n.Title)   //title should be sanitized
return   os.  WriteFile  (filename, n.Body,   0600  )
}
// Scan files in a folder to find first occurrence of a keyword
func   LoadFromDisk  (  keyword   string  ,   folder   string  ) (  *  Note  ,   error  ) {
filename, err   :=   searchKeywordInFilename  (folder, keyword)
if   err   !=   nil   {
return   nil  , err
}
body, err   :=   os.  ReadFile  (filepath.  Join  (folder, filename))
if   err   !=   nil   {
return   nil  , err
}
return   &  Note  {Title: filename, Body: body},   nil
}
// Scan a directory and if a file name contains a substring, return the first one
func   searchKeywordInFilename  (  folder   string  ,   keyword   string  ) (  string  ,   error  ) {
items, _   :=   ioutil.  ReadDir  (folder)
for   _, item   :=   range   items {
// Read the whole file at once
// this is the most inefficient search engine in the world
// good enough for an example
b, err   :=   ioutil.  ReadFile  (filepath.  Join  (folder, item.  Name  ()))
if   err   !=   nil   {
// This is not normal but we can safely ignore it
log.  Printf  (  "Could not read   %v  "  , item.  Name  ())
}
s   :=   string  (b)
if   strings.  Contains  (s, keyword) {
return   item.  Name  (),   nil
}
}
return   ""  , errors.  New  (  "no file contains this keyword"  )
}
```


You’ll notice in the above code that you reuse the` Note` generated for you by` protoc` in the` notes.pb.go` file.


### Implementing the gRPC Server


Create a folder called` notes_server` and inside this folder create a` main.go` file. Start by implementing the minimal server configuration:


```text
package   main


import   (
"  context  "
"  flag  "
"  fmt  "
"  github.com/xNok/go-grpc-demo/notes  "
"  google.golang.org/grpc  "
"  log  "
"  net  "
)


var   (
port   =   flag.  Int  (  "port"  ,   50051  ,   "The server port"  )
)


func   main  () {
// parse arguments from the command line
// this lets us define the port for the server
flag.  Parse  ()
lis, err   :=   net.  Listen  (  "tcp"  , fmt.  Sprintf  (  ":  %d  "  ,   *  port))
// Check for errors
if   err   !=   nil   {
log.  Fatalf  (  "failed to listen:   %v  "  , err)
}
// Instantiate the server
s   :=   grpc.  NewServer  ()
// Register server method (actions the server will do)
// TODO
log.  Printf  (  "server listening at   %v  "  , lis.  Addr  ())
if   err   :=   s.  Serve  (lis); err   !=   nil   {
log.  Fatalf  (  "failed to serve:   %v  "  , err)
}
}
```


With this code, you have a working server but you haven’t registered any action that this server can perform. All the structure for handling gRPC communication is generated in` notes/notes_grpc.pb.go` . The only thing left is to implement the` notes.NotesServer` interface defined in that generated file. Since you already have your business logic in the` notes` package, implement the` notes.NotesServer` interface by creating the required methods (` Save` and` Load` ):


```text
// Implement the notes service (notes.NotesServer interface)
type   notesServer   struct   {
notes  .  UnimplementedNotesServer
}


// Implement the notes.NotesServer interface
func   (  s   *  notesServer  )   Save  (  ctx   context  .  Context  ,   n   *  notes  .  Note  ) (  *  notes  .  NoteSaveReply  ,   error  ) {
log.  Printf  (  "Received a note to save:   %v  "  , n.Title)
err   :=   notes.  SaveToDisk  (n,   "testdata"  )
if   err   !=   nil   {
return   &  notes  .  NoteSaveReply  {Saved:   false  }, err
}
return   &  notes  .  NoteSaveReply  {Saved:   true  },   nil
}


// Implement the notes.NotesServer interface
func   (  s   *  notesServer  )   Load  (  ctx   context  .  Context  ,   search   *  notes  .  NoteSearch  ) (  *  notes  .  Note  ,   error  ) {
log.  Printf  (  "Received a note to load:   %v  "  , search.Keyword)
n, err   :=   notes.  LoadFromDisk  (search.Keyword,   "testdata"  )
if   err   !=   nil   {
return   &  notes  .  Note  {}, err
}
return   n,   nil
}
```


Finally, register the` notesServer` you created. You only need to add a single line of code where the` TODO` comment is:


```text
// Register server method (actions the server will do)
notes.  RegisterNotesServer  (s,   &  notesServer  {})
```


Start the server:


```text
go   run   ./notes_server/main.go
```


## Implementing the gRPC Client


Create a folder called` notes_client` . Inside this folder, create a` main.go` file. Once again, you’ll implement the minimal client configuration, then add the gRPC elements. You want to create a command line that can either save a note or load a note. Here is the basic structure for this:


```text
package   main


import   (
"  context  "
"  flag  "
"  fmt  "
"  github.com/xNok/go-grpc-demo/notes  "
"  google.golang.org/grpc  "
"  google.golang.org/grpc/credentials/insecure  "
"  log  "
"  os  "
"  time  "
)


var   (
addr   =   flag.  String  (  "addr"  ,   "localhost:50051"  ,   "the address to connect to"  )
)


func   main  () {
flag.  Parse  ()
// Set up a connection to the server.
// TODO
// Define the context
ctx, cancel   :=   context.  WithTimeout  (context.  Background  (), time.Second)
defer   cancel  ()
// define expected flag for save
saveCmd   :=   flag.  NewFlagSet  (  "save"  , flag.ExitOnError)
saveTitle   :=   saveCmd.  String  (  "title"  ,   ""  ,   "Give a title to your note"  )
saveBody   :=   saveCmd.  String  (  "content"  ,   ""  ,   "Type what you like to remember"  )
//define expected flags for load
loadCmd   :=   flag.  NewFlagSet  (  "load"  , flag.ExitOnError)
loadKeyword   :=   loadCmd.  String  (  "keyword"  ,   ""  ,   "A keyword you'd like to find in your notes"  )
if   len  (os.Args)   <   2   {
fmt.  Println  (  "expected 'save' or 'load' subcommands"  )
os.  Exit  (  1  )
}
switch   os.Args[  1  ] {
case   "save"  :
saveCmd.  Parse  (os.Args[  2  :])
// Call the server
// TODO
case   "load"  :
loadCmd.  Parse  (os.Args[  2  :])
// Call the server
// TODO
default  :
fmt.  Println  (  "Expected 'save' or 'load' subcommands"  )
os.  Exit  (  1  )
}
}
```


Now, add the element required to communicate with the server. The first` TODO` is to establish the connection. Use the code generated by` protoc` and the client constructor` NewNotesClient` :


```text
// Set up a connection to the server.
conn, err   :=   grpc.  Dial  (  *  addr, grpc.  WithTransportCredentials  (insecure.  NewCredentials  ()))
if   err   !=   nil   {
log.  Fatalf  (  "did not connect:   %v  "  , err)
}
defer   conn.  Close  ()


c   :=   notes.  NewNotesClient  (conn)
```


For the next two` TODO` s, use the client you instantiated to remotely call the` Save` and` Load` functions from the server. Here is the code for the save command:


```text
case   "save"  :
saveCmd.  Parse  (os.Args[  2  :])
_, err   :=   c.  Save  (ctx,   &  notes  .  Note  {
Title:   *  saveTitle,
Body:  []  byte  (  *  saveBody),
})


if   err   !=   nil   {
log.  Fatalf  (  "The note could not be saved:   %v  "  , err)
}


fmt.  Printf  (  "Your note was saved:   %v  n"  ,   *  saveTitle)
```


Here is the code for the load command:


```text
case   "load"  :
loadCmd.  Parse  (os.Args[  2  :])
note, err   :=   c.  Load  (ctx,   &  notes  .  NoteSearch  {
Keyword:   *  loadKeyword,
})


if   err   !=   nil   {
log.  Fatalf  (  "The note could not be loaded:   %v  "  , err)
}
fmt.  Printf  (  "  %v  n"  , note)
```


Your client is now ready to use. Create a` testdata` folder to hold your notes (the name of the output folder is hard coded in the code). Next, try to take some notes:


```text
go   run   notes_client/main.go   save   -title   test   -content   "Lorem ipsum dolor sit amet, consectetur "
```


Can you retrieve the note you just saved?


```text
go   run   notes_client/main.go   load   -keyword   Lorem
```


## Testing and Error Handling


Testing and error handling are crucial aspects of gRPC development. Here are some best practices to follow:


- Use unit tests to test the service logic
- Use integration tests to test the client and server interaction
- Use error handling mechanisms, such as error codes and error messages, to handle errors
- Use logging mechanisms to log errors and other important events


For example, the following code shows how to handle errors in a gRPC service:


```text
func   (  s   *  GreeterServer  )   SayHello  (  ctx   context  .  Context  ,   req   *  HelloRequest  )
(  *  HelloResponse  ,   error  ) {   if   req.Name   ==   ""   {   return   nil  ,
status.  Errorf  (codes.InvalidArgument,   "name is required"  ) }   // ... }
```


This code checks if the name field is empty and returns an error if it is. Proper error handling ensures that your gRPC service can gracefully handle unexpected situations and provide meaningful feedback to the client.


## Going Further with gRPC


So far, you’ve only been using simple unary gRPC communication, in which a single request is sent to the server, and you get a single response back. gRPC also supports three types of streaming protocols: server streaming, client streaming, and bidirectional streaming. Each works well for a large amount of asynchronous data. You’re going to implement a` SaveLargeNote` function that utilizes streaming. First, update your service definition in` notes.proto` to include:


```text
// Save a note via Streaming
rpc SaveLargeNote (stream Note) returns (NoteSaveReply) {}
```


Notice that you use the` stream` keyword to indicate the direction of the stream. The` stream` keyword on the function argument is on the caller side for client-server streaming. Generate the protocol buffer code using` protoc` . The command is the same as before:


```text
protoc   --go_out=.   --go_opt=paths=source_relative
--go-grpc_out  =  .   --go-grpc_opt  =  paths  =  source_relative
notes/notes.proto
```


Implement the new` SaveLargeNote` function in the server code:


```text
func   (  s   *  notesServer  )   SaveLargeNote  (  stream   notes  .  Notes_SaveLargeNoteServer  )   error   {
var   finalBody []  byte
var   finalTitle   string
for   {
// Get a packet
note, err   :=   stream.  Recv  ()
if   err   ==   io.EOF {
log.  Printf  (  "Received a note to save:   %v  "  , finalTitle)
err   :=   notes.  SaveToDisk  (  &  notes  .  Note  {
Title: finalTitle,
Body:  finalBody,
},   "testdata"  )
if   err   !=   nil   {
stream.  SendAndClose  (  &  notes  .  NoteSaveReply  {Saved:   false  })
return   err
}
stream.  SendAndClose  (  &  notes  .  NoteSaveReply  {Saved:   true  })
return   nil
}
if   err   !=   nil   {
return   err
}
log.  Printf  (  "Received a chunk of the note to save:   %v  "  , note.Body)
// Concat packet to create final note
finalBody   =   append  (finalBody, note.Body  ...  )
finalTitle   =   note.Title
}
}
```


Note these elements in the above code:


- ` stream.Recv()` lets you consume a packet (the order of packets in a stream is preserved)
- ` io.EOF` error ends a stream
- ` stream.SendAndClose` is used to close the stream and send the response to the client


Now you need to update the client. You’ll add a new option` -l` that will instruct the client to use the new` SaveLargeNote` :


```text
saveLargeBody   :=   saveCmd.  Bool  (  "l"  ,   false  ,   "flag to upload a note broken as a stream"  )
```


Create the stream by splitting the` Body` into chunks. The following code needs to be placed inside the` save` case. When using the save comment, if` saveLargeBody` is provided, then use the gRPC stream.


```text
if   *  saveLargeBody {
stream, err   :=   c.  SaveLargeNote  (ctx)
if   err   !=   nil   {
log.  Fatalf  (  "Fail to create stream:   %v  "  , err)
}
chunks   :=   split  ([]  byte  (  *  saveBody),   10  )
for   _, chunk   :=   range   chunks {
note   :=   &  notes  .  Note  {
Title:   *  saveTitle,
Body:  chunk,
}
if   err   :=   stream.  Send  (note); err   !=   nil   {
log.  Fatalf  (  "  %v  .Send(  %v  ) =   %v  "  , stream, note, err)
}
}
_, err   =   stream.  CloseAndRecv  ()
if   err   !=   nil   {
log.  Fatalf  (  "  %v  .CloseAndRecv() got error   %v  , want   %v  "  , stream, err,   nil  )
}
}   else   {
```


Note these elements in the above code:


- ` stream.Send` sends a packet to the stream
- ` stream.CloseAndRecv` closes the stream and waits for the server response


Here is the split function used to create a chunk of the` Body` :


```text
// create bytes chunks for streaming
func   split  (  buf   []  byte  ,   lim   int  ) [][]  byte   {
var   chunk []  byte
chunks   :=   make  ([][]  byte  ,   0  ,   len  (buf)  /  lim  +  1  )
for   len  (buf)   >=   lim {
chunk, buf   =   buf[:lim], buf[lim:]
chunks   =   append  (chunks, chunk)
}
if   len  (buf)   >   0   {
chunks   =   append  (chunks, buf[:  len  (buf)])
}
return   chunks
}
```


You can learn how to implement the remaining two communication options with the[gRPC basic tutorial](https://grpc.io/docs/languages/go/basics/) .


## Conclusion


gRPC is a faster, smarter option for service-to-service communication and client-server mobile applications. As you saw in this tutorial, gRPC and Golang are good combinations for microservice communication. You need to change your perspective from creating resource-based servers to client API paradigms and move towards creating action-based APIs. If you’re creating gRPC applications and need to test app performance,[Speedscale](https://speedscale.com/) can help. The traffic replay framework conducts API testing in Kubernetes, offering automatically generated tests based on real-time data that’s stored in Speedscale’s data warehouse for analysis. Speedscale supports gRPC, REST, and GraphQL, among other protocols and environments. To see how Speedscale can help you,[sign up for a free trial](https://app.speedscale.com/signup/) . To check your work on this tutorial, you can find the code[on GitHub](https://github.com/xNok/go-grpc-demo) .
