#webdevSpecific 

tRPC is the typescript Remote Procedure Call library. <span style="color:cyan; font-weight: bold;">What is RPC? It is just functions. It's a way of calling some function on some computer (server) from another. Instead of a calling some URL exposed in a REST API and getting a response, you call a function and get a response.</span>
```typescript
// HTTP/REST
const res = await fetch('/api/users/1');
const user = await res.json();
// RPC
const user = await api.users.getById({ id: 1 });
```
tRPC is a  layer of abstraction that lets you not think about the implementation details of the HTTP requests that are being sent back and forth between the server and client. You can just call functions and have tRPC take care of everything else, and no that it will all be typesafe. It also has a lot of pre-built tools that are just useful to have and allow you to not reinvent the wheel.

tRPC is also integrated with tanstack query to allow end to end typesafety on all your data queries subscriptions and mutations.

## Setting tRPC up in the Next.js app router
Getting tRPC set up requires a fair amount of boilerplate which can be pretty annoying, but you really only need to do it once for a given project. These are the steps to get it setup in the app router:
