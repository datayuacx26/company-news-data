---
schema_version: "1.0.0"
document_id: "8267bfa64dfdf1d6915da317d362020d1181d82a049ffbc7f8243903564e52ec"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/grpc-workers/"
published_at: "2026-08-03T13:00:00+00:00"
first_seen_at: "2026-08-03T13:28:47.669131+00:00"
fetched_at: "2026-08-03T14:10:52.670502+00:00"
content_hash: "sha256:d7b8309c865069ec9c0c2a4ce0a73377239e84bcefac6dac7487b1b7a5d1762f"
---

# Cloudflare Workers and Containers now support inbound TCP connections and gRPC

AI is changing how people interact with computers, and voice is becoming an increasingly important part of that shift. Real-time assistants, AI-powered dictation, and other voice interfaces need low-latency communication between clients, models, and supporting services. Many developers use[gRPC](https://grpc.io/) , a Remote Procedure Call (RPC) framework built on HTTP/2 and TCP, for this infrastructure.


Ever since Workers[launched in 2017](https://blog.cloudflare.com/introducing-cloudflare-workers/) , we’ve been expanding their capabilities, including adding the ability to[open outbound TCP connections](https://blog.cloudflare.com/workers-tcp-socket-api-connect-databases/) and a[JavaScript-native RPC system](https://blog.cloudflare.com/workers-javascript-modules/) built on[Cap’n Proto](https://capnproto.org/) . And so as part of Agents Week, we’re extending Workers in the other direction, supporting inbound TCP connections and adding new ways to run gRPC applications on Cloudflare.


Today, we’re announcing:


- **connect(socket)** — a new[handler](https://developers.cloudflare.com/workers/runtime-apis/handlers/) in the Workers runtime that lets your Worker directly accept an inbound TCP socket provided by[Spectrum](https://developers.cloudflare.com/spectrum/) (Cloudflare’s ingress proxy for non-HTTP traffic)
- **Full-duplex, bi-directional gRPC from Cloudflare Containers** — forward the socket from your Worker to your gRPC server running in a container
- **Workers can serve unary and server-streaming gRPC APIs and call gRPC servers** — you write your code using[gRPC-web](https://grpc.io/docs/platforms/web/) , and Cloudflare automatically converts incoming and outgoing requests to gRPC


We’re introducing this in private beta — you can sign up[here](https://forms.gle/Q2SoJLUKjBFGxBgW6) .


Let’s dig into each of these below.


## connect(socket) from your Worker to Durable Objects and Containers


The Workers runtime now provides a[connect() handler](https://developers.cloudflare.com/workers/runtime-apis/handlers/) that accepts a socket that you can read from and write to:


```text
export   default   {
async   connect  (  socket  )  :   Promise  <  void  > {
const   writer   =   socket.writable.  getWriter  ();
await   writer.  write  (  new   TextEncoder  ().  encode  (  "Hello, world!  \n  "  ));
await   writer.  close  ();
},
}   satisfies   ExportedHandler  ;
```


You can pass this socket from one Worker to another Worker, or from a Worker to a Durable Object. This lets your Worker control where an incoming TCP connection is routed:


```text
import   { DurableObject }   from   "cloudflare:workers"  ;


export   class   SocketDurableObject   extends   DurableObject  <  Env  > {
async   connect  (  socket  :   Socket  )  :   Promise  <  void  > {
// Echo bytes from inside the Durable Object
await   socket.readable.  pipeTo  (socket.writable);
}
}


export   default   {
async   connect  (  socket  ,   env  )  :   Promise  <  void  > {
const   stub   =   env.  SOCKET_DO  .  getByName  (  "my-server"  );
const   durableObjectSocket   =   stub.  connect  (  "host:port"  );


await   Promise  .  all  ([
socket.readable.  pipeTo  (durableObjectSocket.writable),
durableObjectSocket.readable.  pipeTo  (socket.writable),
]);
},
}   satisfies   ExportedHandler  <  Env  >;
```


You can pass a socket from a Durable Object to[its Container](https://developers.cloudflare.com/durable-objects/api/container/) :


```text
import   { DurableObject }   from   "cloudflare:workers"  ;


export   class   SocketContainer   extends   DurableObject  <  Env  > {
constructor  (  ctx  :   DurableObjectState  ,   env  :   Env  ) {
super  (ctx, env);
this  .ctx.container  !  .  start  ();
}


async   connect  (  socket  :   Socket  )  :   Promise  <  void  > {
const   containerSocket   =   this  .ctx.container  !
.  getTcpPort  (  8080  )
.  connect  (  "10.0.0.1:8080"  );


await   containerSocket.opened;


await   Promise  .  all  ([
socket.readable.  pipeTo  (containerSocket.writable),
containerSocket.readable.  pipeTo  (socket.writable),
]);
}
}
```


And then handle the socket in the container:


```text
# server.py
import   socketserver


class   Handler  (  socketserver  .  BaseRequestHandler  ):
def   handle  (self):
while   data   :=   self  .request.recv(  64   *   1024  ):
self  .request.sendall(  b  "Echo: "   +   data)


class   Server  (  socketserver  .  ThreadingTCPServer  ):
allow_reuse_address   =   True
daemon_threads   =   True


with   Server((  "0.0.0.0"  ,   8080  ), Handler)   as   server:
server.serve_forever()
```


This gives you full control over the entire path from client to your server running in a container on Cloudflare, opening the door to full-duplex communication between client and server running any program, in any language, for any TCP-based protocol.


To expose the raw TCP socket to the client, we’re introducing a new type of[Spectrum](https://developers.cloudflare.com/spectrum/) application, where you specify a Worker that you want incoming TCP connections to be routed to.[Spectrum](https://developers.cloudflare.com/spectrum/) is Cloudflare’s ingress proxy for non-HTTP traffic, and allows Cloudflare to sit in front of any TCP or UDP application.


## Bidirectional gRPC from Cloudflare Containers


[gRPC](https://grpc.io/) is a well-established and popular Remote Procedure Call (RPC) framework that was initially released by Google almost 10 years ago, and is now used across mobile apps, distributed systems, and most recently — voice AI applications.


Real-time voice AI applications demand low-latency, and both client and server to be able to send messages to each other over a single, persistent connection. WebSockets and Durable Objects are excellent fits for this, and the Cloudflare Agents SDK provides[@cloudflare/voice](https://developers.cloudflare.com/agents/communication-channels/voice/) to make this easy. But there is a ton of software out there that uses gRPC for real-time client-server communication.


Using the APIs described above, you can now deploy gRPC servers to Cloudflare, written in any language, with full support for bidirectional streaming between client and server. This lets you take advantage of Cloudflare’s network of[330+ locations](https://www.cloudflare.com/network/) and handle requests much closer to clients than is possible elsewhere. We’re excited about the doors this opens up for low-latency voice and colocated inference.


For example, here’s a minimal gRPC server that echoes messages it receives back to the client:


```text
package   main


import   (
"  io  "
"  log  "
"  net  "


pb   "  example/proto  "
"  google.golang.org/grpc  "
)


type   server   struct   {
pb  .  UnimplementedByteStreamServer
}


func   (  server  )   Chat  (  stream   pb  .  ByteStream_ChatServer  )   error   {
if   err   :=   stream.  Send  (  &  pb  .  ByteChunk  {
Payload: []  byte  (  "connected  \n  "  ),
}); err   !=   nil   {
return   err
}


for   {
message, err   :=   stream.  Recv  ()


if   err   ==   io.EOF {
return   stream.  Send  (  &  pb  .  ByteChunk  {
Payload: []  byte  (  "goodbye  \n  "  ),
})
}
if   err   !=   nil   {
return   err
}


if   err   :=   stream.  Send  (  &  pb  .  ByteChunk  {
Payload:   append  ([]  byte  (  "echo: "  ), message.Payload  ...  ),
}); err   !=   nil   {
return   err
}
}
}


func   main  () {
listener, err   :=   net.  Listen  (  "tcp"  ,   ":50051"  )
if   err   !=   nil   {
log.  Fatal  (err)
}


grpcServer   :=   grpc.  NewServer  ()
pb.  RegisterByteStreamServer  (grpcServer,   &  server  {})


log.  Println  (  "gRPC server listening on :50051"  )
log.  Fatal  (grpcServer.  Serve  (listener))
}
```


With this, there’s pretty much no gRPC-based application that you can’t deploy to Cloudflare, no matter what language it’s in or dependencies it relies on. But what if you need to do something simpler, and just serve a basic gRPC server or connect from a Worker to a gRPC server running somewhere else?


## Workers as gRPC servers and clients with gRPC to gRPC-web conversion — no container needed


[gRPC-web](https://grpc.io/docs/platforms/web/basics/) is a browser-compatible version of gRPC. Web browsers don’t expose the lower-level HTTP/2 features that gRPC requires, and there is no raw TCP Socket API built into web browsers — this is why the WebSocket API exists, and why Workers have supported WebSockets[since 2021](https://blog.cloudflare.com/introducing-websockets-in-workers/) .


HTTP/2 splits each request and response into small binary messages called[frames](https://httpwg.org/specs/rfc7540.html#FramingLayer) . This is core to how a single HTTP/2 or HTTP/3 connection is able to multiplex — many requests can be interleaved over one connection. Each frame has a stream ID, allowing the receiver to reassemble it into the correct request or response. gRPC depends on this stream-level control for efficient streaming, cancellation, flow control, and[trailers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Trailer) .


Web platform APIs like[fetch()](https://developers.cloudflare.com/workers/runtime-apis/fetch/) don’t provide this control. So how can we make it simple and easy to use gRPC from Cloudflare Workers — without clients needing to make any changes? We translate incoming gRPC to gRPC-web, and translate outgoing gRPC-web to gRPC.


We’ve actually used gRPC-web within Cloudflare’s reverse proxy since 2020, when we wrote about the[Road to gRPC](https://blog.cloudflare.com/road-to-grpc/#converting-to-http11) on the Cloudflare blog. We convert requests to HTTP/1.1 so that messages can be inspected and gRPC apps can benefit from[Cloudflare’s security features](https://www.cloudflare.com/solutions/security/) , like WAF rules and Bot Management.


Now, in private beta and then rolling out to everyone, we’re extending this so that given a Protocol Buffer (protobuf) definition file like this:


```text
syntax   =   "proto3"  ;


package hello;


service   Greeter   {
rpc   SayHello   (  HelloRequest  ) returns (  HelloReply  );
}


message   HelloRequest   {
string name   =   1  ;
}


message   HelloReply   {
string message   =   1  ;
}
```


You can write a unary gRPC server in a Worker in just a few lines of code, using the[@connectrpc/connect](https://connectrpc.com/) open-source package:


```text
import   { createConnectRouter }   from   "@connectrpc/connect"  ;
import   {
universalServerRequestFromFetch,
universalServerResponseToFetch,
}   from   "@connectrpc/connect/protocol"  ;
import   { Greeter }   from   "./gen/hello_pb"  ;


const   router   =   createConnectRouter  ();


router.  service  (Greeter, {
sayHello  : ({   name   })   =>   ({ message:   `Hello, ${  name  }!`   }),
});


const   handlers   =   new   Map  (
router.handlers.  map  ((  handler  )   =>   [handler.requestPath, handler]),
);


export   default   {
async   fetch  (  request  :   Request  )  :   Promise  <  Response  > {
const   handler   =   handlers.  get  (  new   URL  (request.url).pathname);
return   universalServerResponseToFetch  (
await   handler  (  universalServerRequestFromFetch  (request, {})),
);
},
}   satisfies   ExportedHandler  ;
```


You can make outbound requests to external gRPC servers this way too, by using the client built into[@connectrpc/connect](http://connectrpc/connect) :


```text
import   { createClient }   from   "@connectrpc/connect"  ;
import   { createGrpcWebTransport }   from   "@connectrpc/connect-web"  ;
import   { Greeter }   from   "./gen/hello_pb"  ;


const   client   =   createClient  (
Greeter,
createGrpcWebTransport  ({
baseUrl:   "https://grpc.example.com"  ,
fetch  : (  input  ,   init  )   =>
fetch  (input, {   ...  init, redirect:   "manual"   }),
}),
);


export   default   {
async   fetch  ()  :   Promise  <  Response  > {
const   reply   =   await   client.  sayHello  ({ name:   "Workers"   });
return   Response.  json  (reply);
},
}   satisfies   ExportedHandler  ;
```


Your code uses gRPC-web, but when it speaks to the outside world, it is automatically translated into gRPC. This means that clients and servers that you already depend on don’t need to change. For example, you can:


- **Provide gRPC backends to mobile apps that speak gRPC** — Many mobile apps already use gRPC to reduce network payloads, serialize data more efficiently, and generate strongly-typed client libraries. You can now build the backend server for mobile apps on Workers, while still using established gRPC native libraries like[grpc-swift-2](https://www.swift.org/blog/grpc-swift-2/) and[grpc-kotlin](https://grpc.io/docs/languages/kotlin/quickstart/) .
- **Put a Worker in front of an existing gRPC backend** — So many developers already put Workers in front of existing REST APIs to move performance critical work closer to the user, or to incrementally move state into[Durable Objects](https://developers.cloudflare.com/durable-objects/) . Now you can do this with existing gRPC backends as well, or build new APIs and services that fetch data from your existing gRPC backend.


## What’s next for Socket Workers and gRPC on Cloudflare


We’re introducing everything from this post in private beta — you can sign up[here](https://forms.gle/Q2SoJLUKjBFGxBgW6) .


At Cloudflare, we use[Cap’n Proto](https://capnproto.org/) and[Cap’n Web](https://blog.cloudflare.com/capnweb-javascript-rpc-library/) and the[JavaScript-native RPC system that is built into Cloudflare Workers](https://blog.cloudflare.com/javascript-native-rpc/) instead of gRPC. And when we ship things, we always aim to be using them ourselves. So in this case, we want to first work closely with a smaller set of developers using gRPC, and make sure we’ve nailed it before turning this on for everyone.


More broadly, we’re excited to continue to push the bounds of what types of traffic the Workers platform can serve, going beyond TCP and into UDP-based protocols. Keep[telling us what you want to build](https://x.com/CloudflareDev) on Workers, and we’ll keep pushing the bounds of what is possible.
