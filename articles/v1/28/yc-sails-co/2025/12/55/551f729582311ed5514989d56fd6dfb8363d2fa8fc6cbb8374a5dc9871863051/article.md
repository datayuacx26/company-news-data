---
schema_version: "1.0.0"
document_id: "551f729582311ed5514989d56fd6dfb8363d2fa8fc6cbb8374a5dc9871863051"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/virtual-request-pattern/"
published_at: "2025-12-22T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:727922e3d80cb5c33ee8b04159f332e22988965d36af02da30a760d4d0d8d869"
---

# The Virtual Request Pattern in Sails.js

## Introduction


Modern web applications increasingly require both traditional HTTP endpoints and real-time WebSocket connections. This dual requirement typically forces developers into maintaining parallel codebases: one set of handlers for HTTP requests and another for WebSocket messages. The result is duplicated validation logic, authorization checks, and business rules across both transport layers.


Sails.js addresses this architectural challenge through the virtual request pattern, a mechanism that treats WebSocket messages as synthetic HTTP requests. This abstraction enables a single codebase to handle both protocols without duplication or conditional logic.


For a broader overview of Sails’ WebSocket architecture, see[Understanding Sails WebSockets: A Modern, Unified Approach to Real-Time in Node.js](https://blog.sailscasts.com/understanding-sails-websockets) . This article examines the specific implementation details of the virtual request pattern and how it achieves protocol unification.


## Protocol-Agnostic Actions


The virtual request pattern enables a single action to handle both HTTP and WebSocket requests without modification. Consider this basic Sails action:


```text
// api/controllers/user/get-users.js
module  .  exports   =   {
friendlyName:   'List users'    ,
description:   'Return a list of users'  ,
exits: {
success: {}
},
fn  :   async   function   () {
const   users   =   await   User.  find  ();
return   { users };
}
};
```


This action handles both HTTP requests and WebSocket messages without requiring protocol-specific code. The same validation, authorization, and business logic apply uniformly across both transports.


HTTP client usage:


```text
const   users   =   await   fetch  (  '/api/users'  )
```


WebSocket client usage:


```text
io.socket.  get  (  '/api/users'  ,   function  (  data  ,   jwr  ) {
console.  log  (data.users);
});
```


The action, route configuration, validation logic, and authorization policies remain identical across both protocols. This unification is the core benefit of the virtual request pattern.


## Implementation Details


The virtual request pattern is implemented in the[sails-hook-sockets](https://github.com/balderdashy/sails-hook-sockets) hook, specifically within the[receive-incoming-sails-io-msg.js](https://github.com/balderdashy/sails-hook-sockets/blob/bd8d1c051e883cccf08f23be9f2c27b24d00ab65/lib/receive-incoming-sails-io-msg.js#L1) module. This module intercepts WebSocket messages and transforms them into request/response pairs that flow through the standard Sails routing layer.


When a client sends` io.socket.get('/api/users')` , the following sequence occurs:


### Step 1: Message Reception


The socket server receives a[Socket.IO](https://socket.io/) message that looks like this:


```text
{
method  :   'get'  ,
url  :   '/api/users'  ,
data  : {   /* optional request body */   },
headers  : {   /* optional headers */   }
}
```


### Step 2: Request Context Creation


The incoming message is used to construct a request object that mirrors the structure of a standard HTTP request:


```text
const   requestContext   =   {
method:   'get'  ,
url:   '/api/users'  ,
path:   '/api/users'  ,
body: {},
headers: {
cookie: socket.handshake.headers.cookie,
origin: socket.handshake.headers.origin,
// ... other headers
},
isSocket:   true  ,
socket: socket,    // Access to the raw socket
ip:   '192.168.1.1'  ,
// ... all the properties you expect from req
};
```


The[isSocket](https://github.com/balderdashy/sails-hook-sockets/blob/bd8d1c051e883cccf08f23be9f2c27b24d00ab65/lib/receive-incoming-sails-io-msg.js#L96) property is set to` true` , enabling protocol-specific handling when necessary. In practice, the abstraction typically eliminates the need for such conditionals.


### Step 3: Response Context Creation


Sails also creates a response object with a special[_clientCallback](https://github.com/balderdashy/sails-hook-sockets/blob/bd8d1c051e883cccf08f23be9f2c27b24d00ab65/lib/receive-incoming-sails-io-msg.js#L187) function:


```text
const   responseContext   =   {
_clientCallback  :   function  (  clientRes  ) {
// Build JWR (JSON WebSocket Response)
const   jwr   =   {
body: clientRes.body,
statusCode: clientRes.statusCode,
headers: clientRes.headers
};


// Send it back over the socket
socketIOClientCallback  (jwr);
}
};
```


### Step 4: Router Integration


The virtual request and response objects are passed to the standard Sails router:


```text
app.router.  route  (requestContext, responseContext);
```


The router processes the virtual request identically to an HTTP request. Routes defined in` config/routes.js` , policies, and validation logic all execute without modification.


## JSON WebSocket Response


When an action returns a response through a virtual request, Sails serializes it into a JSON WebSocket Response (JWR). This format preserves HTTP semantics while remaining compatible with Socket.IO’s message-based protocol:


```text
{
body  : {   users  : [  ...  ] },
statusCode  :   200  ,
headers  : {
'content-type'  :   'application/json'
}
}
```


The JWR structure allows clients to inspect status codes, headers, and response bodies using the same patterns as HTTP.


Client-side handling:


```text
io.socket.  get  (  '/api/users'  ,   function  (  data  ,   jwr  ) {
if   (jwr.statusCode   !==   200  ) {
console.  error  (  'Something went wrong!'  );
return  ;
}


console.  log  (  'Success:'  , data.users);
});
```


## Authentication and Authorization


The virtual request pattern ensures that authentication and authorization logic applies uniformly across both protocols. Consider a protected route with a policy:


```text
// config/routes.js
'GET /api/dashboard'  : {
action:   'dashboard/view'  ,
policy:   'is-logged-in'
}
```


```text
// api/policies/is-logged-in.js
module  .  exports   =   async   function   (  req  ,   res  ,   proceed  ) {
if   (  !  req.session.userId) {
return   res.  unauthorized  ();
}


return   proceed  ();
};
```


This policy executes for both HTTP and WebSocket requests without modification. The session is loaded from the socket’s cookie during the WebSocket handshake, ensuring consistent authentication state.


HTTP request:


```text
fetch  (  '/api/dashboard'  , {
credentials:   'include'    // Sends cookies
});
```


WebSocket request:


```text
io.socket.  get  (  '/api/dashboard'  ,   function  (  data  ,   jwr  ) {
// Session automatically loaded from handshake cookie
});
```


Unauthenticated requests receive a 401 response regardless of protocol.


## Performance Considerations


The virtual request pattern introduces overhead compared to raw Socket.IO handlers. Creating synthetic request/response objects and routing through the full Sails stack adds processing time to each WebSocket message.


However, this overhead is typically negligible for most applications. The tradeoff favors developer productivity and code maintainability over marginal performance differences. The cost of maintaining duplicate codebases and the bugs that result from divergent implementations generally outweigh the minor performance impact.


For applications requiring maximum throughput on specific socket events, Sails provides direct access to the underlying Socket.IO server:


```text
// config/bootstrap.js
module  .  exports  .  bootstrap   =   async   function  () {
sails.io.  on  (  'connection'  ,   function  (  socket  ) {
// Direct Socket.io handler for high-performance needs
socket.  on  (  'ping'  ,   function  () {
socket.  emit  (  'pong'  );
});
});
};
```


## Progressive Disclosure of Complexity


The virtual request pattern follows a design philosophy of[progressive disclosure](https://en.wikipedia.org/wiki/Progressive_disclosure) . Initial implementations require no understanding of the underlying transport mechanisms. A simple action handles both protocols without explicit configuration or conditional logic.


As application requirements evolve, the abstraction provides escape hatches:


- Check` req.isSocket` to differentiate between protocols when necessary
- Access` req.socket` for direct Socket.IO operations
- Override default behaviors at the transport layer


This approach prioritizes convention over configuration while maintaining flexibility for advanced use cases.


## Comparison to Alternative Approaches


Most Node.js frameworks require separate handler implementations for HTTP and WebSocket protocols.


### Express


```text
// HTTP handler
app.  get  (  '/api/users'  ,   async   (  req  ,   res  )   =>   {
const   users   =   await   getUsers  ();
res.  json  ({ users });
});


// Socket handler (duplicate logic!)
io.  on  (  'connection'  , (  socket  )   =>   {
socket.  on  (  'users:get'  ,   async   (  callback  )   =>   {
const   users   =   await   getUsers  ();    // Same logic!
callback  ({ users });
});
});
```


### Next.js


Requires a custom server setup that bypasses Next.js’s standard entry point, with manual Socket.IO integration and separate handler logic for HTTP and WebSocket events.


### Sails


```text
// Single handler for both transports
module  .  exports   =   {
fn  :   async   function  () {
const   users   =   await   User.  find  ();
return   { users };
}
};
```


The unified approach eliminates an entire category of duplication and synchronization issues.


## Limitations and Constraints


The virtual request abstraction includes several inherent limitations:


### 1. Origin Headers Are Immutable


The` origin` header is established during the initial WebSocket handshake and cannot be modified on a per-request basis. This constraint prevents origin spoofing:


```text
// This won't work:
io.socket.  request  ({
method:   'get'  ,
url:   '/api/users'  ,
headers: {
origin:   'https://evil.com'    // Ignored
}
});
```


The origin remains constant for the duration of the connection, providing security at the cost of flexibility.


### 2. No Response Streaming


HTTP responses support streaming, but virtual requests buffer the entire response before transmission:


```text
// This streams over HTTP but buffers over WebSockets
res.  send  ({ huge:   'data'   });
```


This limitation is acceptable for typical JSON APIs but may require alternative approaches for large payloads.


### 3. Request Size Limits


Socket.IO enforces a` maxHttpBufferSize` configuration (default 1MB). Requests exceeding this limit are rejected:


```text
// config/sockets.js
module  .  exports  .sockets   =   {
maxHttpBufferSize:   5   *   1024   *   1024    // 5MB
};
```


## Architectural Benefits


The virtual request pattern provides more than syntactic convenience. It establishes a mental model where protocol concerns become implementation details rather than architectural constraints. Developers build APIs without distinguishing between HTTP and WebSocket transport layers.


This unification is similar to approaches taken by Laravel Echo and Rails Action Cable, where real-time features integrate naturally into the framework’s existing patterns rather than requiring separate architectural considerations.


## Conclusion


The virtual request pattern demonstrates how careful abstraction can eliminate entire categories of complexity. By treating WebSocket messages as synthetic HTTP requests, Sails enables developers to write protocol-agnostic code that maintains consistency across both transports.


This mechanism forms the foundation of Sails’ WebSocket architecture. Related topics including session management, broadcasting patterns, and horizontal scaling are covered in[Understanding Sails WebSockets: A Modern, Unified Approach to Real-Time in Node.js](https://blog.sailscasts.com/understanding-sails-websockets) .


For community discussion and support, join the[Sails Discord](https://sailsjs.com/support) .
