#webdevSpecific 

The next step in learning full stack development is to connect the front-end react app to the back-end express app. 

Thinking about the example notes app discussed in the course content, we could replace the JSON-server used as a place holder in the react app with our express backend by changing the `baseURL` used for fetch requests with the baseURL for the express app, then starting both the front and back end servers.
```jsx
import axios from 'axios'

const baseUrl = 'http://localhost:3001/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...

export default { getAll, create, update }
```
However after doing this, the GET request from the front-end to get all the notes into memory, will fail with a CORS policy error. Let's dive into why that is.

## Same origin policy and CORS
The error comes from the browsers "same origin policy". A URLs origin is defined by a combination of it's protocol (also called scheme), hostname and port.
```
http://example.com:80/index.html
  
protocol: http
host: example.com
port: 80
```
When you visit a website, the browsers issues a request to the server hosting the site, and the server responds with an HTML document. The HTML file may contain one or more references to external resources hosted either on the same or different servers. When the browser sees references to a URL in the HTML, it sends a request there. If the request is issued to a URL that does not share the same origin as the HTML from the initial request, the browser will have to check the `Access-Control-Allow-origin` response header. If it contains `*` on the URL of the source HTML, the browser will process the reponse, otherwise it will throw an error.

**The same-origin policy is a security measure in browsers to prevent session hijacking and other attack vectors**.

In order to enable legitimate cross-origin requests (requests to URLs that don't share the same origin) W3C came up with a mechanism called **CORS**(Cross-Origin Resource Sharing). According to [Wikipedia](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing):

> _Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a web page to be requested from another domain outside the domain from which the first resource was served. A web page may freely embed cross-origin images, stylesheets, scripts, iframes, and videos. Certain "cross-domain" requests, notably Ajax requests, are forbidden by default by the same-origin security policy._

The Javascript code of an app running in the browser is not allowed to communicate across origins by default. Because the vite front-end and express back-end have different origins (port 5173 and port 3001 respectively), they don't have the same origin.

We can allow requests from other origins in our express app by using Nodes cors middleware. Install if from NPM:
```bash
npm install cors
```

Then use it like a regular middleware:
```js
const cors = require('cors')

app.use(cors())
```
NOTE: When enabling cors, think about how you want to configure it. For example we may want, in this situation, to configure it to only allow requests from our front-end.

You can read more about CORS from [Mozilla's page](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

With both the front and back ends running locally and the app working in the browser, we can diagram the architecture like this:
![[Pasted image 20241007123808.png]]

## Hosting the app on the internet
There are a lot of options for hosting an app like our example. We will mostly be looking at Platform as a Service providers, that are dev friendly and take care of installing the execution env (node) and provide various other helpful services.

Heroku was great for this till they took away their free-tier, and other providers have since followed suit. The FSO content reccommends either Fly.io or Render. However, Fly.io also no longer have a completely free tier, so I will be using Render here. As of October 2024, Render still has a free tier, who knows how long it will last though.

For both render, fly.io and basically any other PaaS provider, you will need to change the express app listening port to dynamically use the PORT assigned by the hosting platform over the dev port, like this:
```js
const PORT = process.env.PORT || 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

### Deploying on Render
First sign in with a github account. You can use other methods to make a render account but the notes here will assume that you used github.

Select the options to create a new web service from your Render dashboard, then connect it to the relevant github repo.

Fill out the options for server location etc, and note that for a mono-repo, you can specify the root directory. By default Render will use the root of the git repo, but in the case of my FSO exercises repo I wanted to use a sub repo so I specified it here.

After filling this all out and selecting that you want it to run on a free instance, the app will run and render will give you the URL. Any updates pushed to the main branch of the repo will automatically cause a rebuild and redeploy.

## Frontend production build
