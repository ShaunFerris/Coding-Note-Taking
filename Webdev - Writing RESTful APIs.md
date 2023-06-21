#webdevSpecific #backend #apis

The initial basis of these notes is a transcription of this video: ![](https://www.youtube.com/watch?v=-MTSQjw5DrM&list=PLlrrMbxpJkWztSP91KThXQBCsIg7maJWH&index=12&t=153s)
The content of this note is also closely linked to the content here: [[Webdev - HTTP Verbs]]

## APIs
An Application Programming Interface, or API, is a way for two computers to communicate. Using an API is just like using a website in a browser, but instead of clicking buttons or filling out forms, you write code to dictate the transmission of information. 

For example, you could visit the NASA website to look at pictures of asteroids, or we could use their REST API to request the raw JSON data that is shown on screen.

## REST
REST is a set of rules for that most APIs are written to follow, as they are the defacto standard for API development. It measns "Representational State Transfer", and was coined in a 2000 PhD thesis written by Roy Fielding.

A restful API organises data 'entities' or 'resources' into a bunch of unique URIs that differentiate different types of data resources on a server. A client can get data about a resource by making a request to that endpoint over HTTP. 

## Request and responses
The response to that request has a very specific format. Most Importantly, the start line has the HTTP verb that represents your requests intention, followed by the URI of the resource you are making a request to. 

Below the start line, the request has headers which contain metadata about the request, including the accept header for specifying the format you want in the reponse (eg json) and the authorization header to provide credentials that prove you are allowed to make the request. 

Next comes the body, which contains a custom payload of data. The server will receive the request message, then execute some code, usually to read data from a database that can then be formatted as a response and sent back. 

The top line of the response contains a status code. Codes at the 200 level, eg 200 or 201, indicate the request was OK, 400s indicate something was wrong with the request, and 500s indicate that the server failed. Then like in requests, below the top line are headers and then the body which contains the response payload, usually as JSON or some other serialized format.

## State
An importat feature of this architecture that we have been describing is that it is stateless, meaning that neither party, the server or the client, need to store any information about each other, and every request response cycle is independant. This leads to web applications that are predictable and reliable. 