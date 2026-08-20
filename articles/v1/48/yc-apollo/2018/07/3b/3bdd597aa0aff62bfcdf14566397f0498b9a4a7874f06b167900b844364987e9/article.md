---
schema_version: "1.0.0"
document_id: "3bdd597aa0aff62bfcdf14566397f0498b9a4a7874f06b167900b844364987e9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/with-apollo-server-2-0"
published_at: "2018-07-03T17:47:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:e59f5a572729aa94461f9e34b741f078ceab944dc926396475d108d539e1378d"
---

# File Uploads with Apollo Server 2.0

**Update: May 2022** For our recommendation on how to handle file uploads, read[Apollo Server File Uploads Best Practices](https://www.apollographql.com/blog/apollo-server-file-upload-best-practices-1e7f24cdc050) .


Our current recommendation is to **not** use the multipart request approach described in this package. In fact, this feature was removed from Apollo Server in version 3, although you can still manually integrate with the` graphql-upload` package if you take care to[protect yourself from Cross-Site Request Forgery (CSRF) attacks](https://www.apollographql.com/docs/apollo-server/security/cors/#preventing-cross-site-request-forgery-csrf) .


In web applications, one of the most common requirements is file uploads. For a while now, the question being asked around the GraphQL community is **“How does one perform file uploads in GraphQL itself?”** The community turned to the GraphQL specification and reference implementation but found no answers because the specification has no provisions for file uploads.


There are several ways you could upload files when using a GraphQL API, for example sending the file to Cloudinary or S3 directly and passing the returned URL through a GraphQL mutation. However, coordinating these multiple requests might be hard to manage. One of the simplest ways of achieving file uploads in a single request is to base64-encode a file and send as a string variable in a mutation. Another option is to send file(s) in the same request as your mutation, which I’ll cover in this post!


[Jayden Seric](https://medium.com/u/5bd1b4d20a6d?source=post_page-----5db2f3f60675----------------------) , a JavaScript engineer and GraphQL community member came up with a[specification for GraphQL multipart form requests(file uploads)](https://github.com/jaydenseric/graphql-multipart-request-spec) and also provided a reference implementation.


At Apollo, we are committed to making sure you have the best developer experience throughout your GraphQL journey.


We decided to incorporate the much requested file uploads feature into **Apollo Server 2.0** , thanks to inspiration from the wonderful[apollo-upload-server](https://github.com/jaydenseric/apollo-upload-server) package by Jayden. With GraphQL, we are achieving a single paradigm of data management. Just how Apollo Client merges remote and local data. Apollo Server enables you to combine multiple previously orthogonal data sources into a single paradigm. This post being about the specific case of file uploads and JSON data.


## How it works


The upload functionality follows the GraphQL multipart form requests specification. Two parts are needed to make the upload work correctly. The server and the client:


**The Client:** On the client, file objects are mapped into a mutation and sent to the server in a multipart request.


**The Server:** The multipart request is received. The server processes it and provides an upload argument to a resolver. In the resolver function, the upload promise resolves an object.


Stay with me! The resolver section will reveal what the object contains and how you can use it.


## Server Configuration


The default option for enabling file uploads in Apollo Server 2.0 involves creating a schema and using the` Upload` type like so:


```text
const { ApolloServer, gql } = require('apollo-server');


const typeDefs = gql`
type File {
filename: String!
mimetype: String!
encoding: String!
}
type Query {
uploads: [File]
}
type Mutation {
singleUpload(file: Upload!): File!
}
`;
```


The first question you’re most likely to ask from the schema observation above is “Where does` Upload` scalar type come from?” Don’t fret. It is added automatically by Apollo Server.


Apollo Server 2.0 automatically adds the` Upload` scalar to the schema, when you are not setting the schema manually.


### Resolver implementation


Earlier on, I mentioned that the server returns an upload promise that resolves an object. The object contains the following:


1. *stream:* The upload stream manages[streaming the file(s)](https://nodejs.org/api/stream.html) to a filesystem or any storage location of your choice. e.g. S3, Azure, Cloudinary, e.t.c.
2. *filename:* The name of the uploaded file(s).
3. *mimetype:* The MIME type of the file(s) such as` text/plain` ,` application/octet-stream` , etc.
4. *encoding:* The file encoding such as UTF-8.


```text
const resolvers = {
Query: {
files: () => {
// Return the record of files uploaded from your DB or API or filesystem.
}
},
Mutation: {
async singleUpload(parent, { file }) {
const { stream, filename, mimetype, encoding } = await file;


// 1. Validate file metadata.


// 2. Stream file contents into cloud storage:
// https://nodejs.org/api/stream.html


// 3. Record the file upload in your DB.
// const id = await recordFile( … )


return { filename, mimetype, encoding };
}
},
};
```


In the code above, the file can be validated after the promise resolves. If the file size or type is right (depending on the validation technique), it can be streamed into cloud storage like[Cloudinary](https://cloudinary.com/documentation/node_image_upload) and the returned link can be stored in a database. Otherwise an[Apollo error can be thrown](https://dev-blog.apollodata.com/full-stack-error-handling-with-graphql-apollo-5c12da407210) within the resolver.


## Client Setup


If the user is expected to upload files from an interactive client UI, then you need to install the` apollo-upload-client` package from npm. And deal with single and multiple files from the client.


*Single file upload example from the client:*


```text
import gql from 'graphql-tag'
import { Mutation } from 'react-apollo'


export const UPLOAD_FILE = gql`
mutation uploadFile($file: Upload!) {
uploadFile(file: $file) {
filename
}
}
`;


const uploadOneFile = () => {
return (
<Mutation mutation={UPLOAD_FILE}>
{uploadFile => (
<input
type="file"
required
onChange={({ target: { validity, files: [file] } }) =>
validity.valid && uploadFile({ variables: { file } });
}
/>
)}
</Mutation>
);
};
```


Use` <a href="https://developer.mozilla.org/en/docs/Web/API/FileList" target="_blank" rel="noreferrer noopener">FileList</a>` ,` <a href="https://developer.mozilla.org/en/docs/Web/API/File" target="_blank" rel="noreferrer noopener">File</a>` ,` <a href="https://developer.mozilla.org/en/docs/Web/API/Blob" target="_blank" rel="noreferrer noopener">Blob</a>` or` <a href="https://github.com/jaydenseric/apollo-upload-client#react-native" target="_blank" rel="noreferrer noopener">ReactNativeFile</a>` instances anywhere within query or mutation input variables to send a[GraphQL multipart request](https://github.com/jaydenseric/graphql-multipart-request-spec) . In the example above, we operated a single file upload, thus using` <a href="https://developer.mozilla.org/en/docs/Web/API/File" target="_blank" rel="noreferrer noopener">File</a>`` .`


The user selects a file, the client validates the file and immediately sends a multipart request in the same request as the` uploadFile` mutation to the server.


Check out the[file uploads documentation for more information on multiple and blob file uploads from the client.](https://www.apollographql.com/docs/guides/file-uploads.html)


## Try it out


A[full-stack working example created by Jayden is available on GitHub](https://github.com/jaydenseric/apollo-upload-examples) . Clone and try it out. Feel free to provide feedback.


To keep learning, check out our[new best practices guides about topics like schema design, versioning, access control, and more.](https://www.apollographql.com/docs/)


---


Finally, I hope you’ll also[join us at the 3rd annual GraphQL Summit on Nov 7–8 in San Francisco](https://summit.graphql.com/) . With over 800 attendees expected, it’s the largest GraphQL developer event in the world. Super early bird tickets are selling fast, so[register today](https://www.eventbrite.com/e/graphql-summit-2018-tickets-46601841362) to reserve your spot!


Written by


Prosper Otemuyiwa


[Read more by Prosper Otemuyiwa](https://www.apollographql.com/blog/author/prosper)
