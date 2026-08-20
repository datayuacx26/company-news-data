---
schema_version: "1.0.0"
document_id: "daab103655f6236f24064d71f64e56241aa9f1f70357ef9c8e62503ae92847b1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/with-react-hooks-typescript-amazon-s3-tutorial"
published_at: "2020-02-14T18:47:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:f68b476dcd58ffef8b144acd524c67627eb3ed4e51f10fdd65f0b81a6dbbb6f0"
---

# ☝️ GraphQL File Uploads with Apollo Server 2, React Hooks, TypeScript & Amazon S3 [Tutorial]

**Update: May 2022**


This post shows you how to implement file uploads via “multipart requests” in the obsolete Apollo Server 2.


Apollo Server 3 no longer integrates with (a specific outdated version of) the` graphql-upload` package by default, so in order to follow these instructions you’ll need to[explicitly integrate with the graphql-upload package](https://www.apollographql.com/docs/apollo-server/data/file-uploads) .


Additionally, **integrating with` graphql-upload` introduces major “CSRF” security vulnerabilies** unless you specifically prevent them.


Because of this, we no longer recommend implementing uploads via multipart requests to your GraphQL server. Our post on[file upload best practices](https://www.apollographql.com/blog/backend/file-uploads/file-upload-best-practices/) has other suggestions for how to implement uploads.


If you do want to follow this tutorial to implement multipart uploads, we **highly recommend** you do so from Apollo Server 3 (using a manual` graphql-upload` integration) rather than Apollo Server 2, and enable the[CSRF prevention feature](https://www.apollographql.com/docs/apollo-server/security/cors/#preventing-cross-site-request-forgery-csrf) added in Apollo Server 3.7.


As time goes on, it looks like more developers are choosing to build their public-facing APIs with GraphQL instead of REST. We’re going to see a lot of the same problems people were solving with REST, solved with GraphQL, in a much cleaner and enjoyable way.


A common task in a lot of web applications is performing file uploads. If you’re using Apollo Server 2, uploads are enabled by default. (However, using this feature within Apollo Server 2 is actually quite insecure: see[our post on file upload best practices](https://www.apollographql.com/blog/backend/file-uploads/file-upload-best-practices/) for details.)


By adding the` Upload` type to our Apollo Server type definitions, we enable the ability to upload files from the client.


If we build a mutation that utilizes the` Upload` type, what we get back is a` stream` of data that we can pipe to a file stored on our server, or, more interestingly, to an external cloud service like AWS S3. It’s also pretty common for things like profile pictures that we’d want to also make sure we store the URL of the file in our database so that we can use it to show people’s display pictures.


In this practical tutorial, I’ll walk you through how to:


- Set up an Apollo Server with TypeScript for file uploads
- Setup your Apollo Client to upload files
- Pipe an uploaded file to AWS S3
- Get the URL of the uploaded file so that we can save it to our database


**Hold up** ✋: Before we get started, I urge you to check out the[Apollo Server File Upload Best Practices Guide](https://blog.apollographql.com/apollo-server-file-upload-best-practices-1e7f24cdc050) . In that guide, we cover three different types of ways to perform file uploads (Multipart Upload Requests, Signed URL Uploads, and rolling your image server).


In *this tutorial* , we’re going to implement **#1 — Multipart Upload Requests** .


With that said, onwards!


## Apollo Server


Setting up an Apollo Server is a piece of cake. We just need to install the following npm packages.


```text
npm install --save apollo-server graphql
```


If you’re starting a project from scratch, check out “[Getting started with Apollo Server](https://www.apollographql.com/docs/apollo-server/getting-started/) ”. If you’re adding a GraphQL Server to an existing Express.js REST API, check out “[Add a GraphQL Server to a RESTful Express.js API in 2 Minutes](https://dev.to/apollographql/add-a-graphql-server-to-a-restful-express-js-api-in-2-minutes-4gdb) ”.


TypeScript types will come in handy when we build the uploader, so let’s add that to our project as well.


```text
npm install --save-dev typescript @types/node && tsc --init
```


Check out “[How to Setup a TypeScript + Node.js Project](https://khalilstemmler.com/blogs/typescript/node-starter-project/) ” if you’ve never set up a TypeScript app before.


When we’re done with that, the most basic Apollo Server setup we could have should look a little something like this.


```text
import { ApolloServer, gql } from 'apollo-server'


const server = new ApolloServer({
typeDefs: gql`
type Query {
hello: String!
}
`,
resolvers: {
Query: {
hello: () => "Hey!"
}
}
});


server.listen().then(({ url }) => {
console.log(`🚀 Server ready at ${url}`);
});
```


### Upload mutation


We want clients to be able to upload a file to our GraphQL endpoint, so we’ll need to expose a` singleUpload` GraphQL mutation to do just that. Using the` Upload` scalar that comes with Apollo Server, write a` singleUpload` mutation that takes in a non-null` Upload` and returns a non-null` UploadedFileResponse` response.


```text
import { ApolloServer, gql } from 'apollo-server'


const server = new ApolloServer({
typeDefs: gql`
type Query {
hello: String!
}
type UploadedFileResponse {
filename: String!
mimetype: String!
encoding: String!
url: String!
}
type Mutation {
singleUpload(file: Upload!): UploadedFileResponse!
}
`,
resolvers: {
Query: {
hello: () => "Hey!"
},
Mutation: {
singleUpload: async (parent, { file }) => {
const { stream, filename, mimetype, encoding } = await file;


// Do work 💪


return { filename, mimetype, encoding, url: '' }
}
}
}
});


server.listen().then(({ url }) => {
console.log(`🚀 Server ready at ${url}`);
});
```


### The Upload type


The` file` object that we get from the second parameter of the` singleUpload` resolver is a` Promise` that resolves to an` Upload` type with the following attributes:


- ` stream` : The[upload stream](https://nodejs.org/api/stream.html) of the file(s) we’re uploading. We can pipe a Node.js stream to the filesystem or other cloud storage locations.
- ` filename` : The name of the uploaded file(s).
- ` mimetype` : The[MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) of the file(s) such as text/plain, application/octet-stream, etc.
- ` encoding` : The file encoding such as UTF-8.


---


At this point, we have a` singleUpload` mutation ready to accept a file upload and turn it into a stream that we can pipe to some destination. We’re not doing anything with that yet, so let’s change it.


## Uploading to AWS S3


Amazon S3 is a popular object storage service that we can use to store images, videos, and just about any other kind of file that you can think of.


Let’s make another file and create an` AWSS3Uploader` class to hold the responsibility of uploading to S3.


### Creating an AWS S3 Uploader


We’re going to need the AWS SDK, so let’s install that first.


```text
npm install --save aws-sdk
```


Then we’ll create the` AWSS3Uploader` class that accepts an` S3UploadConfig` (a handy-dandy type that we create) in the constructor. To create a new instance of one of these, we need to pass in everything necessary to get an authenticated uploader up and running.


That means we’ll need the:


- ` accessKeyId` – You can get this by using IAM, creating a user, and attaching the` AmazonS3FullAccess` permission to them, then creating an access key for them. Check[this link](https://aws.amazon.com/premiumsupport/knowledge-center/create-access-key/) for more info.
- ` secretAccessKeyId` – Same as above.
- ` destinationBucketName` – With S3, we store data in buckets. You’ll want to create a bucket first, and then use the name of the bucket here.
- (optional)` region`


Here’s what the class looks like so far.


```text
import AWS from "aws-sdk";


type S3UploadConfig = {
accessKeyId: string;
secretAccessKey: string;
destinationBucketName: string;
region?: string;
};


export class AWSS3Uploader {
private s3: AWS.S3;
public config: S3UploadConfig;


constructor(config: S3UploadConfig) {
AWS.config = new AWS.Config();
AWS.config.update({
region: config.region || "ca-central-1",
accessKeyId: config.accessKeyId,
secretAccessKey: config.secretAccessKey
});


this.s3 = new AWS.S3();
this.config = config;
}
}
```


Cool, so when we create a new one of these, we get an instance of` AWSS3Uploader` , initialized with the AWS settings we need to upload file data to an S3 bucket.


### Replacing (or composing) the resolver


Ideally, it would be nice if this` AWSS3Uploader` class could *replace* (or somehow compose) the resolver that we have on our Apollo Server. With TypeScript, we can define the contract of the resolver function using an interface, and then if our` AWSS3Uploader` implements that interface, we can delegate the work.


I like that approach. Using an` IUploader` interface, define the contract for the` singleFileUploadResolver` and create other strict TypeScript types for the parameters and the return value.


```text
export type File = {
filename: string;
mimetype: string;
encoding: string;
stream?: ReadStream;
}


export type UploadedFileResponse = {
filename: string;
mimetype: string;
encoding: string;
url: string;
}


export interface IUploader {
singleFileUploadResolver: (
parent,
{ file } : { file: Promise<File> }
) => Promise<UploadedFileResponse>;
}
```


Then, implement the` IUploader` interface on the` AWSS3Uploader` class.


```text
export class AWSS3Uploader implements IUploader {
private s3: AWS.S3;
public config: S3UploadConfig;


constructor(config: S3UploadConfig) {
AWS.config = new AWS.Config();
AWS.config.update({
region: config.region || "ca-central-1",
accessKeyId: config.accessKeyId,
secretAccessKey: config.secretAccessKey
});


this.s3 = new AWS.S3();
this.config = config;
}


async singleFileUploadResolver (
parent,
{ file } : { file: Promise<File> }
) : Promise<UploadedFileResponse> {
// Todo next!
return null;
}
}
```


Advanced Design tip: What we’ve just done here is planted the seeds to implementing the design principle called[Liskov Substitution](https://khalilstemmler.com/articles/solid-principles/solid-typescript/) in that we should be able to swap one implementation for another. If later on in the future, we’d like to switch to using Cloudinary or Google Cloud for uploads instead, all we have to do is implement the` IUploader` interface on a new object, and we can swap it out safely. Beautiful!


Before we implement the S3 upload code, let’s go back to our Apollo Server and create an instance of our` AWSS3Uploader` .


```text
import { AWSS3Uploader } from './s3'


const s3Uploader = new AWSS3Uploader({
accessKeyId: process.env.AWS_ACCESS_KEY,
secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
destinationBucketName: 'my-really-cool-bucket'
});
```


And then we can replace the anonymous resolver function with our` s3Uploader` .


```text
resolvers: {
...
Mutation: {
singleUpload: s3Uploader.singleFileUploadResolver.bind(s3Uploader)
}
}
```


Because we’re using classes, when control inverts to` s3Uploader` , the value of` this` with respect to` s3Uploader` will be lost. We can save that initial` this` value by using the` bind` method. We’ll need to do this if we’re working with class-based components that call their own methods. There are other ways to do this!


### Implementing the upload logic


Now the fun part. What we want to do is:


- Create the destination file path
- Create an upload stream that goes to S3
- Pipe the file data into the upload stream
- Get the link representing the uploaded file
- (optional) save it to our database


To create the file path, let’s add a method called` createDestinationFilePath` that takes in everything we currently know about the file. I’m going to leave it really simple by just returning the name of the file that we want to upload, but if you wanted to create your own naming pattern, you could do that here.


```text
export class AWSS3Uploader implements IUploader {
private s3: AWS.S3;
public config: S3UploadConfig;


...


private createDestinationFilePath(
fileName: string,
mimetype: string,
encoding: string
): string {
return fileName;
}


async singleFileUploadResolver(
parent,
{ file }: { file: Promise<ApolloServerFileUploads.File> }
): Promise<ApolloServerFileUploads.UploadedFileResponse> {
const { stream, filename, mimetype, encoding } = await file;


// Create the destination file path
const filePath = this.createDestinationFilePath(
filename,
mimetype,
encoding
);


// Create an upload stream that goes to S3
// Pipe the file data into the upload stream
// Get the link representing the uploaded file
// (optional) save it to our database


return { filename, mimetype, encoding, url: '' };
}
}
```


Next, we have to create an upload stream that points to our AWS S3 bucket. Streams are one of the most confusing parts of Node.js, so think of this step as if we’re creating the *fire hose* and pointing it directly at the S3 bucket. We’re not doing anything with the data *yet* ; we’re just defining where it’s going to go.


To do this, we define a new type, an` S3UploadStream` object that holds both the upload stream and a promise that we can invoke to start the upload. That promise is essentially the *valve* to our *fire hose* .


```text
import stream from "stream";


type S3UploadStream = {
writeStream: stream.PassThrough;
promise: Promise<AWS.S3.ManagedUpload.SendData>;
};


export class AWSS3Uploader implements ApolloServerFileUploads.IUploader {
private s3: AWS.S3;
public config: S3UploadConfig;


private createUploadStream(key: string): S3UploadStream {
const pass = new stream.PassThrough();
return {
writeStream: pass,
promise: this.s3
.upload({
Bucket: this.config.destinationBucketName,
Key: key,
Body: pass
})
.promise()
};
}


async singleFileUploadResolver(
parent,
{ file }: { file: Promise<ApolloServerFileUploads.File> }
): Promise<ApolloServerFileUploads.UploadedFileResponse> {
const { stream, filename, mimetype, encoding } = await file;


// Create the destination file path
const filePath = this.createDestinationFilePath(
filename,
mimetype,
encoding
);


// Create an upload stream that goes to S3
const uploadStream = this.createUploadStream(filePath);


// Pipe the file data into the upload stream
// Get the link representing the uploaded file
// (optional) save it to our database


return { filename, mimetype, encoding, url: '' };
}
}
```


Now let’s connect the *read* stream (our data) to the *write/upload* stream.


```text
// Pipe the file data into the upload stream
stream.pipe(uploadStream.writeStream);
```


And let’s open the valve.


```text
const result = await uploadStream.promise;
```


At this point, the` singleFileUploadResolver` method should look like this.


```text
async singleFileUploadResolver(
parent,
{ file }: { file: Promise<ApolloServerFileUploads.File> }
): Promise<ApolloServerFileUploads.UploadedFileResponse> {
const { stream, filename, mimetype, encoding } = await file;


// Create the destination file path
const filePath = this.createDestinationFilePath(
filename,
mimetype,
encoding
);


// Create an upload stream that goes to S3
const uploadStream = this.createUploadStream(filePath);


// Pipe the file data into the upload stream
stream.pipe(uploadStream.writeStream);


// Start the stream
const result = await uploadStream.promise;


// Get the link representing the uploaded file
// (optional) save it to our database


return { filename, mimetype, encoding, url: '' };
}
```


We can get the link that the file was uploaded to by pulling it out of the` result` object. If you wish to save this to a database somewhere, this would be the appropriate place for you to do so. See below.


```text
async singleFileUploadResolver(
parent,
{ file }: { file: Promise<File> }
): Promise<ApolloServerFileUploads.UploadedFileResponse> {
const { stream, filename, mimetype, encoding } = await file;


// Create the destination file path
const filePath = this.createDestinationFilePath(
filename,
mimetype,
encoding
);


// Create an upload stream that goes to S3
const uploadStream = this.createUploadStream(filePath);


// Pipe the file data into the upload stream
stream.pipe(uploadStream.writeStream);


// Start the stream
const result = await uploadStream.promise;


// Get the link representing the uploaded file
const link = result.Location;


// (optional) save it to our database


return { filename, mimetype, encoding, url: result.Location };
}
```


You may need to associate the upload to the particular user who made the request- you can accomplish this using the third argument in the GraphQL resolver- the context argument. For more details on how this works, check out the[Apollo Docs on the Context Argument](https://www.apollographql.com/docs/apollo-server/data/data/#context-argument) .


And that completes our server-side configuration!


Let’s move over to the client-side and walk through a simple setup with Apollo Client.


## Apollo Client


Assuming you already have a React app created (and if you don’t, see how to use[Create React App](https://reactjs.org/docs/create-a-new-react-app.html) to create a new one), you’ll want to set up an instance of Apollo Client.


**Just want the code?** Go ahead and peep it[on Github](https://github.com/stemmlerjs/apollo-cloud-file-uploads/tree/master/src/client) .


Run this command to install the latest version of Apollo Client.


```text
npm install --save @apollo/client
```


Next, we can create an instance of` ApolloClient` , connect it to our Apollo Server using the` HttpLink`[Link component](https://www.apollographql.com/docs/link/links/http/) , and wrap our React app with an` ApolloProvider` .


```text
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import "./index.css"


import {
ApolloClient,
InMemoryCache,
ApolloProvider,
HttpLink
} from "@apollo/client";


const client = new ApolloClient({
cache: new InMemoryCache(),
link: new HttpLink({
uri: 'http://localhost:4000/'
});
});


ReactDOM.render(
<ApolloProvider client={client}>
<App />
</ApolloProvider>,
document.getElementById("root")
);
```


That’s the basic setup.


To uploads working, we need to rely on a community-built package called` apollo-upload-client` which adds capabilities for multipart requests to the ApolloClient instance.


You can read the docs for` apollo-upload-client`[here](https://github.com/jaydenseric/apollo-upload-client) .


Let’s install it.


```text
npm install apollo-upload-client
```


To hook it up, we need to replace the` HttpLink` Link instance with a Link created by using the` apollo-upload-client` ‘s` createUploadLink` factory function.


```text
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import "./index.css"


import {
ApolloClient,
InMemoryCache,
ApolloProvider,
HttpLink
} from "@apollo/client";
import { createUploadLink } from 'apollo-upload-client'


const client = new ApolloClient({
cache: new InMemoryCache(),
//@ts-ignore
link: createUploadLink({
uri: "http://localhost:4000",
}),
});


ReactDOM.render(
<ApolloProvider client={client}>
<App />
</ApolloProvider>,
document.getElementById("root")
);
```


Because the type contracts aren’t nominally equivalent between the official Apollo Client and the object created by` createUploadLink` (at the moment), we need to use` @ts-ignore` to prevent type error.


### Uploading a file from the client to cloud storage


From the client, I’m going to create a straightforward` App` component.


```text
import React from "react";


const App: React.FC = () => {
return (
<UploadFile />
);
};


export default App;
```


In that` App` component, I’ve defined another component called` UploadFile` . Let’s create that now.


```text
const UploadFile = () => {
const [mutate, { loading, error }] = useMutation(SINGLE_UPLOAD);
const onChange = ({
target: {
validity,
files: [file]
}
}: any) => validity.valid && mutate({ variables: { file } });


if (loading) return <div>Loading...</div>;
if (error) return <div>{JSON.stringify(error, null, 2)}</div>;


return (
<React.Fragment>
<input type="file" required onChange={onChange} />
</React.Fragment>
);
};
```


The` UploadFile` component uses the` useMutation` hook that takes in a GraphQL` mutation` that we’re about to write. When the` onChange` callback gets called on the` input` tag, it supplies a` validity` object that we can test against to determine if we should execute the mutation with` mutate` . You can read more about the nuances and features of` apollo-client-uploads` in the[GitHub docs](https://github.com/jaydenseric/apollo-upload-client) .


Lastly, we need to write the` mutation` and import the necessary utilities to do so.


```text
import { useMutation, gql } from "@apollo/client";


const SINGLE_UPLOAD = gql`
mutation($file: Upload!) {
singleUpload(file: $file) {
filename
mimetype
encoding
url
}
}
`;
```


Notice that the` Upload` type we’re referring to is the one that Apollo Server knows about as a scalar type.


That’s it! Try it out, and check your S3 console for your uploaded files.


Here’s the client-side upload component in completion.


```text
import React from "react";
import { useMutation, gql } from "@apollo/client";


const SINGLE_UPLOAD = gql`
mutation($file: Upload!) {
singleUpload(file: $file) {
filename
mimetype
encoding
url
}
}
`;


const UploadFile = () => {
const [mutate, { loading, error }] = useMutation(SINGLE_UPLOAD);
const onChange = ({
target: {
validity,
files: [file]
}
}: any) => validity.valid && mutate({ variables: { file } });


if (loading) return <div>Loading...</div>;
if (error) return <div>{JSON.stringify(error, null, 2)}</div>;


return (
<React.Fragment>
<input type="file" required onChange={onChange} />
</React.Fragment>
);
};


const App: React.FC = () => {
return (
<UploadFile />
);
};


export default App;
```


## Conclusion


We just learned how to use **multipart requests** to perform GraphQL file uploads. If you’re just getting started with GraphQL, or you’re working on a non-critical project, this approach is **excellent** because it’s the simplest way to get something up and running.


If you’re working on something in production that is critical, definitely remember to check out the[best practices guide](https://blog.apollographql.com/apollo-server-file-upload-best-practices-1e7f24cdc050) for the alternative approaches.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
