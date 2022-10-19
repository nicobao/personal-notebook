# Technology comparison when designing a public API

## TL;DR

I'd use either:
- (REST + OpenAPI AND WebSocket + AsyncAPI) OR
- protobuf + gRPC + buf

The reason is:
- the tooling is great for an api-first approach to designing APIs. One trusted statically typed source of truth which is the OpenAPI/AsyncAPI documents or the .proto files - and then everything compiles from it: documentation and client/server code in any languages you like
- both solutions support bidirectional communication

## Intro

Which technology can you use to publish a somewhat-standard public API?

So far, I explored the following:
- REST (OpenAPI)
- Websocket (AsyncAPI)
- JSON-RPC (OpenRPC)
- GraphQL (GraphQL Schema)
- gRPC (buf)

There are plenty of others that were dismissed for the moment because either I haven't had the time to look into it, I don't know about them, they are too niche / unrelated to my current work (e.g: WebRTC) or they are too immature (e.g: Cap'n proto).

Before diving into each technologies let's reflect on underlying principles.

## Approaches to API Development

There are basically two approaches to API development.

In every approaches, you need some document/schema that describe your API.
It's important for API discovery, documentation and code generation.

### code-first

The #1 source of truth is some backend implementing an API. Then the document describing the API is generated from it, and consumed by external tools to generate documentation or client code.

This approach is easier to start with and more "natural" from a developer perspective.
There are three issues with this approach:
- librairies to generate API schema generation from code are not always available depending on the framework and language you use. For example, it is NOT available with golang and gin.
- even when a mature library exists (e.g: Java Spring), they rarely support all the OpenAPI features in a fine-grain way and it's often cumbersome to use (ex: add comments, examples?)
- because the source of truth is the backend, the API tends to become too backend-centric and not necessarily adapted for the frontend usage. Because developers have to worry about the implementation and the same time, they often tend to ignore thinking about improving the API itself. Backend developers tend to do it on their own instead of API-design being a team work involving all stakeholders.

### api-first

Write the API spec first (types, endpoints, examples, comments).

Generate everything from it:
- backend stub
- clients
- documentation

This approach is by far the best but it comes with one big limitation:
- sometimes the tooling just isn't there

## API Technologies (and their schema)

### REST (OpenAPI)

[REST](https://restfulapi.net/) is a set of lightweight rules over HTTP(S).
Like HTTP, it only supports Request/Response, not publish/subscribe pattern.
It is by far the most used technology for internal and external API on the web.
Because REST relies on raw HTTP, it is very easy to optimize both in the frontend and backend, and it is very easy to use existing tools for authentication/authorization, rate limiting, telemetry...etc.

[OpenAPI](https://www.openapis.org/) is the reference specification used to formalize REST APIs.
It comes in the form of a JSON Schema specification. 

You describe a JSON Schema file that matches with this schema. This JSON Schema describes your API:
- the types you use across the API
- the endpoints available and their specificities: urls, query params, path params, response body and status code...
- examples and comments
- and more

OpenAPI comes with a whole bunch of _very mature_ tools, most notably:
- [Swagger Editor](https://swagger.io/tools/swagger-editor/) for defining the API visually
- [Swagger UI](https://swagger.io/tools/swagger-ui/) or [ReDoc](https://github.com/Redocly/redoc) for visualizing and interacting with the API
- [openapi-generator (community fork of swagger-codegen and most active of the two)](https://github.com/OpenAPITools/openapi-generator) to generate client and server stubs using the API schema in pretty much every language and framework out there. This means it is _very easy to do api-first with REST_.
- plenty of librairies to generate OpenAPI doc from the code itself
- and more

One thing to be noted is that with `openapi-generator` you can generate `statically typed` client and server stubs from the schema.

Meaning for example if you have this API endpoint described in your JSON Schema:

```md
- Description of the CID type.
GET proposal/:address:
   - Description of the expected JSON response in a type.
   - Query params (name - type):
      - latest - boolean
      - cids - List[CID]
```

and the query returns a JSON which body is:
```json
{
	"proposal": {
		"status": "in-progress",
		"content": "{ some proposal }"
	}
}
```

then the output of `openapi-generator` in a statically typed language will be a function:

`GetProposalByAddressResponseDTO getProposalByAddress(String address, Boolean onlyLatest, List[CID] cids);`
With the type `CID` and `GetProposalByAddressResponseDTO` generated for you in your language!

This is very important to remember, because one of the #1 advantage people say GraphQL has over REST is static typing and schema. But with OpenAPI and code/documentation generation, you already have it.

Drawbacks of REST:
- it doesn't have publish/subscribe, you need to use Websocket for that
- it is not easy to avoid underfetching or overfetching
- with JSON as body, REST is relatively slow as opposed to something like gRPC. That being said, protobuf can be used with REST in HTTP body directly to optimize for speed, and JSON is usually fast _enough_
- that's pretty much it because OpenAPI and documentation / code generation solve the other issues (API discovery, lack of schema and static typing)

### Websocket (AsyncAPI)

Websocket is basically a fancy TCP socket over which you can send whatever you want.
Unlike raw TCP, websocket is supported by web browsers and in particular it reuses the same security models.

Quote from the [official RFC](https://www.rfc-editor.org/rfc/rfc6455):
```quote
The goal of this technology is to provide a mechanism for browser-based
   applications that need two-way communication with servers that does
   not rely on opening multiple HTTP connections (e.g., using
   XMLHttpRequest or <iframe>s and long polling).
```

WebSocket is fundamentally complementary to REST. If you need some sort of publish/subscribe mechanism (that's not streaming - otherwise go for WebRTC), then you need WebSocket.

[AsyncAPI](https://www.asyncapi.com/) is the OpenAPI equivalent of WebSocket. It is not limited to WebSocket though, but to every event-driven ("publish-subscribe") sort of transports. It's also a JSON Schema. It was written to be [as compatible with OpenAPI as possible](https://www.asyncapi.com/docs/reference/specification/v2.5.0). So much that there are converters from one spec to the other.

This standard is less mature than OpenAPI (hint: everything is), but it's going the right direction and it's being adopted by major actors, such as Slack, Salesforce or [Kraken](https://docs.kraken.com/websockets/).

AsyncAPI is transport-agnostic, unlike OpenAPI that is directly bound to REST and HTTP.

Like OpenAPI, AsyncAPI comes with bunch of tools:
- [AsyncAPI Studio](https://studio.asyncapi.com/) to edit your API visually, like Swagger Editor. As of October 2022, it is in Beta but it already looks good.
- [Generator](https://www.asyncapi.com/tools/generator)  which is the equivalent of `openapi-generator`. AsyncAPI Generator isn't even closely as comprehensive as `openapi-generator` in terms of the amount of languages and framework it supports. In particular, Go Backend isn't supported. Typescript support is good. 
- [Modelina](https://www.asyncapi.com/tools/modelina). It's a type generator based on the API schema. Fortunately, AsyncAPI Modelina covers a much wider variety of languages, but only generates `types`, and not the whole transport layer. It is not ideal, but it is often enough.

The API-first workflow is [supported](https://www.asyncapi.com/blog/websocket-part3). [Documentation and tutorials are good quality](https://www.asyncapi.com/blog/websocket-part1).

### JSON-RPC (OpenRPC)

[JSON-RPC](https://www.jsonrpc.org/) is very easy to use and to understand, and yet powerful:
- you send a HTTP POST request to some fixed URL, like `https:/api.example.com/jsonrpc`
- in the body of the HTTP request, you have to send a JSON that follows the JSON-RPC specification (some JSON Schema). The response will be a JSON formatted following the JSON-RPC spec.

Example request:
```json
{"jsonrpc": "2.0", "method": "subtract", "params": {"minuend": 42, "subtrahend": 23}, "id": 3}
```
Example response:
```json
{"jsonrpc": "2.0", "result": 19, "id": 3}
```

It is quite a niche technology. It is mainly used by:
- Microsoft and its amazing [LSP](https://microsoft.github.io/language-server-protocol/)
- The cryptocurrency space (Bitcoin, Ethereum and pretty much all the other chains)

I like JSON-RPC. I wish it was the #1 web standard instead of REST. It's much easier not to have to worry about choosing HTTP Method, or between query param / path param / body.

Besides, REST is bound to HTTP - but JSON-RPC is transport-agnostic. JSON-RPC only defines the application-level protocol (the JSON Schema to be sent over some transport layer). JSON-RPC can also be used over WebSocket or raw TCP / UDP. JSON-RPC is still bount to JSON and its relatively poor performance though, it doesn't have an Intermediary Description Language (IDL) like other RPC out there, which would enable this decoupling. But it's not JSON-RPC goal. JSON-RPC is designed to be simple.

[This article](https://dev.to/radixdlt/json-rpc-vs-rest-for-distributed-platform-apis-3n0m) summarizes well why JSON-RPC is a superior approach to REST.

Unfortunately, the standardization and tooling story around JSON-RPC are lacking behind.

[OpenRPC](https://open-rpc.org/) is the equivalent of [OpenAPI] and comes with a bunch of tools:
- [Generator](https://github.com/open-rpc/generator): it can mainly generate Typescript backend & frontend. It can only generate Go frontend and Rust backend.
- no visual editor
- no documentation visualizer

Sadly, that's a deal breaker for me. It means all the downsides of REST that are solved by OpenAPI... aren't solved for JSON-RPC.

That being said - maybe OpenAPI can be reused for JSON-RPC as long as HTTP is used as the transport layer. It seems [people already thought of that](https://dev.to/vearutop/json-rpc-2-0-with-swagger-ui-2h3g), so there may be way to reuse the nice concepts of JSON-RPC with the tooling of REST.

### GraphQL (and its GraphQL Schema)

TL;DR: GraphQL is an interesting option for large-scale applications that have to deal with a large amount of microservices (hint: 99% of us are not in this situation). And even for such organizations - I doubt its usefulness. Some of the problems REST has that GraphQL solves are already solved by OpenAPI - but without the problems that GraphQL brings to the table (and they are numerous). In general I understand the initial appeal, but after diving in I am convinced that GraphQL is a wrong good idea that can be very costly in the long run.

Much of the analysis comes from [this analysis](https://www.youtube.com/watch?v=S1wQ0WvJK64) which I pretty much agree with (for the things I know personally, but the rest makes sense too). His seems to be shared by many people in the community.

#### What is GraphQL?

A Query Language that lets you define a statically typed Schema for your API.
In the Schema, you define how your data are "resolved". Resolving means actually doing the queries to get the data.
In the frontend, you just say which data you want, and you don't care about how it is fetched.

#### Advantages

- you can choose exactly which data you need from the frontend, which avoids underfetching or overfetching. This is not easy with any of the other options.
- statically typed with the schema (api discovery, code generation, documentation). This is solved by OpenAPI in REST.

##### Disadvantages

- huge complexity especially on the backend
- plenty of abstract concepts to grasp: query arguments, input arguments, fragments, input types, union types, directives...
- "new solution creates new problems"
- performance issues: you can very easily overflow a database with a crazy amount of requests without expecting it. Quote from the video above: "If you have an array of friends, and each friend object has an array of mutual friends, if you don't take into account in your GraphQL Server, you might end up making an exponential amount of requests in your server, because GraphQL fetching is naive. The solution in the GraphQL ecosystem is adding a caching layer between the GraphQL Server and the Database." But caching is very complex. Understand where the performance problems come from is not easy to spot.
- caching in the frontend is not easy, like it would be for an HTTP Request. You have to use another layer of library to do it.
- error responses are not standardized and not explicit
- subscribtions (publish/subscribe WebSocket for GraphQL) is very complex
- GraphQL relay is solving problems for large-scale applications, which most of us aren't dealing with

### gRPC (buf)

In gRPC, you define your API in a statically typed Intermediary Definition Language (IDL) named Protobuf. You define types and functions which are Remote Procedure Calls (RPC). It's a little bit like the Schema for your API.

gRPC uses HTTP/2, data is sent in binary format and hence gRPC is the fastest API technology I evaluated (except WebSocket eventually). HTTP/2 allows multiplexing and hence processing in parallel in any order requests received in a single underlying TCP connection.

gRPC allows for Unary, Client, Server and Bidirectional streaming.

`buf` is the tool for:
- generating code (pretty comprehensive here, and golang is supported)
- doing versioning and in particular spotting breaking changes
- storing the api in a special public repository
- [generate documentation](https://docs.buf.build/bsr/documentation)
- and more

There is no visual editor to create the .proto files, but it is unecessary as it is simply a programming language.

[grpcurl](https://github.com/fullstorydev/grpcurl) is the functional equivalent of cURL for gRPC.

gRPC has [built-in](https://github.com/grpc/grpc-go/blob/master/Documentation/grpc-auth-support.md) security and authentication/authorization mechanisms.

Weaknesses:
- Client-side and Bi-directional streaming are currently not supported in the browser. [Only Unary and Server-side streamings are](https://github.com/grpc/grpc-web#make-a-unary-rpc-call)
- in general running gRPC in a browser [requires a proxy such as Envoy to translate HTTP/1.1 request from the browser to HTTP/2 request in the backend ](https://github.com/grpc/grpc-web/blob/master/net/grpc/gateway/examples/echo/tutorial.md#configure-the-envoy-proxy)
- the messages themselves are not human reable (it's binary)
- not suitable for real-time communication - but it's not the purpose here anyway
