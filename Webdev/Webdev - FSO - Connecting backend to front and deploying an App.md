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
So far, the frontend code for the phonebook/notes apps used as examples has been running in developer mode. This is configured to give human readable error messages, imediately re-render on code change etc. 

For deployment, we don't want to use dev mode, as we want to bundle up the code into the  smallest, most efficient package possible, and it does not need to be human readable, or include dev dependancies. **This is why we create a production build**.

A prod build for a vite app can be created by running `npm run build`. This command will create a `dist` directory at the project root which will contain the HTML entry point and a minified version of the projects js and css.

## Serving static files from the backend
One option for deploying the front-end of an app, is to copy the production build of it to the root of the backend repository and configure the backend to serve the front-ends main page (`dist/index.html`) as it's main page.

In unix you can use the `cp` bash built in for this:
```bash
cp -r dist ../backend
```
Note the `-r` flag to recursively copy all the content.

Once you have copied the `dist` directory, we need to use a middleware to allow express to serve static content, the static middleware is built-in to express.
```
app.use(express.static('dist))
```
With the inclusion of the above line in our express code, whenever the server receives and HTTP GET, it will first check if the `dist` directory contains the file corresponding to the requests address, and return the file if it does. So now, GET requests to http://serversaddress.com/index.html, or http://serversaddress.com will show the React front end, while get requests to http://serversaddress.com/api/notes will be handled by the express route handlers.

In this situation, with both front and back ends at the same address, we can declare the `baseURL` in the frontend code as a relative address:
```js
import axios from 'axios'
const baseUrl = '/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...
```
Note that any change to frontend code, including this one, will necessitate re-building the frontend and copying the dist over again.

Our app now works exactly like the Single Page Application (SPA) discussed in part 0 of the FSO course.

When a user visits http://localhost:3001 the server returns the `index.html` file, which fetches the CSS sheet and the JS file from the `dist/assets` directory. The react code, minified into the JS file, fetches the data from the express app at http://localhost:3001/api/notes and renders them to the screen. You can see this in the network tab of the browser dev tools.

This setup is how you would prepare the notes/phonebook app for deployment. It can be diagrammed out like this:
![[Pasted image 20241008125110.png]]

## Deploying the full app to Render
After ensuring that the production version of the application works locally, commit the production build of the frontend to the backend repository, and push the code to GitHub again.

Make sure the directory _dist_ is not ignored by git on the backend.

Using Render a push to GitHub _might_ be enough. If the automatic deployment does not work, select the "manual deploy" from the Render dashboard.

Our application saves the notes to a variable. If the application crashes or is restarted, all of the data will disappear.

The application needs a database. Before we introduce one, let's go through a few things.

The setup now looks like as follows:
![[Pasted image 20241008125324.png]]

## Streamlining the frontend deployment
To create a new production build of the frontend without extra manual work, let's add some npm-scripts to the _package.json_ of the backend repository.

Because Render automatically deploys when changes are pushed to the git repos main branch, our scripts can look like this:
```json
{
  "scripts": {
    //...
    "build:ui": "rm -rf dist && cd ../frontend && npm run build && cp -r dist ../backend",
    "deploy:full": "npm run build:ui && git add . && git commit -m uibuild && git push"
  }
}
```

## Proxy
When we changed the baseURL in the frontend code to a relative path, we broke the development mode of the React app. 

Because in development mode the frontend is at the address _localhost:5173_, the requests to the backend go to the wrong address _localhost:5173/api/notes_. The backend is at _localhost:3001_.

If the project was created with Vite, this problem is easy to solve. It is enough to add the following declaration to the _vite.config.js_ file of the frontend repository.
```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],

  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:3001',
        changeOrigin: true,
      },
    }
  },
})
```
After a restart, the React development environment will work as a [proxy](https://vitejs.dev/config/server-options.html#server-proxy). If the React code does an HTTP request to a server address at _[http://localhost:5173](http://localhost:5173)_ not managed by the React application itself (i.e. when requests are not about fetching the CSS or JavaScript of the application), the request will be redirected to the server at _[http://localhost:3001](http://localhost:3001)_.

Note that with the vite-configuration shown above, only requests that are made to paths starting with _/api_-are redirected to the server.

Now the frontend is also fine, working with the server both in development and production mode.

A negative aspect of our approach is how complicated it is to deploy the frontend. Deploying a new version requires generating a new production build of the frontend and copying it to the backend repository. This makes creating an automated [deployment pipeline](https://martinfowler.com/bliki/DeploymentPipeline.html) more difficult. Deployment pipeline means an automated and controlled way to move the code from the computer of the developer through different tests and quality checks to the production environment. Building a deployment pipeline is the topic of [part 11](https://fullstackopen.com/en/part11) of this course. There are multiple ways to achieve this, for example, placing both backend and frontend code in the same repository but we will not go into those now.

In some situations, it may be sensible to deploy the frontend code as its own application.