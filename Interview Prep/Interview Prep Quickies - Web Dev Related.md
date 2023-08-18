#interviewprep #revision 

## HTTP
The <span style="color: cyan; font-weight: bold; font-style: italic;">Hyper Text Transfer Protocol</span>. An application layer protocol that facilitates moving HTML documents over a network. HTTP follows a classical [client-server model](https://en.wikipedia.org/wiki/Client%E2%80%93server_model), with a client opening a connection to make a request, then waiting until it receives a response. HTTP is a [stateless protocol](https://en.wikipedia.org/wiki/Stateless_protocol), meaning that the server does not keep any data (state) between two requests. The client or a proxy of the client (usually the web browser) sends a request to the server and awwaits a response. The headers/format ofthe requests and responses, and the HTTP verbs for encoding different kind of requests are provided by the HTTP protocol.

HTTP is:
- **Simple**: Designed to be human readable
- **Extensible**: HTTP headers make it easy to extend the protocol. The client and server just have to agree on the headers for a new request type to introduce new functionality
- **Stateless but not sessionless**: It is stateless, as there is no link between two sequential requests carried out over the same connection. This introduces problems where data needs to be kept consistent over many requests like with a shopping cart on a store page. HTTP cookies provide the functionality to track a session, keeping a shared state for multiple requests

HTTP flow of operations:
1. Open  a TCP connection between client and server
2. Send a request over the connection
3. Read the response
4. Close the TCP connection or reuse it for further requests

## CRUD
A standard for restful APIs outlining what they should be able to do. <span style="color: cyan; font-weight: bold; font-style: italic;">Create Read Update Delete.</span>
These functions for a CRUD api map neatly onto the HTTP verbs POST, GET, PATCH, DELETE.
## Rest API
This is a standard for writing APIs. Apis are server hosted code that can be interacted with in a pre-determined, documented way, usually to get access to some resource. Rest stands for representational state transfer. The rest api is hosted on a server and has endpoints for each of the HTTP verbs that correspond to CRUD operations.

## Caches
A mechanism for taking data that is accessed alot and storing it somewhere easily and quickly accessible. Certain databases like REDIS are faster than trad DB technologies like relational DBs, but don't scale well. It is common to cache data that many users need all the time in a redis db, whie using a solution like postgres for the main data store. 

## Local caching
This refers to the local cache in a users browser. When your browser makes a request to a page, depending on the response headers, it may take that page and cache it, allowing a faster load the next time the browser wants to access the same resource as it is not waiting for the HTTP request to resolve first. 

## Cookies
Typically used for authentication but can be used for all sorts of things. When the browser makes a request to the server, you can include a cookies header that has a bunch of information attached to it automatically, for example a session token, so that all your requests will have the users session information attached to verify the user is logged in. 

## Query strings
This is a format for attaching data to the URL for an endpoint. They are denoted with a question mark followed by the data to be handled on the back end. For example a search endpoint could have the search string appended to the URL like this: `mysite.com/search?f=javascript`. In this case the `f=javascript` part is the query string, that can be seperated from the rest of the URL on the backend and used in some operation (searching for a resource in this case).