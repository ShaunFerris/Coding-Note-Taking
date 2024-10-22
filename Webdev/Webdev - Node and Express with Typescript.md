#webdevSpecific 

Adding on from the FSO course content, here is a quick note on how to scaffold a simple express project that wants to use typescript instead of raw js. We will also use a little async/await syntax in our code rather than the promised chaining used in th FSO content examples.

## Project init and packages to install
Make a new folder, move into it and initialize a new node app:
```bash
mkdir project-name
cd project-name
npm init -y
```
Now install packages related to typescript as dev-dependancies:
```bash
npm install typescript ts-node @types/node @tsconfig/node20 --save-dev
```
The ts-node package will allow us to run the project with Just In Time compiling (JIT) so we don't have to recompile inbetween tests.  The tsconfig/node20 is a base config file for typescript that is a good starting point to extend from.

Next, run `tsc --init` to generate a `tsconfig.json` file. Open it, delete the default content and paste in the below set up which  extends the installed node20 base config:
```json
{
  "extends": "@tsconfig/node20/tsconfig.json",
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```
Next, create a `src` directory if you want to structure the project that way. Install and configure es-lint (if you want) with `npm init @eslint?config@latest` and follow the prompts, this will generate a config file for it. 

## Getting started with express
Start by getting express, also get cors and add morgan for logging:
```bash
npm install express cors morgan
```
These will all be added as production dependacies, but we should also install the relevant types as dev deps:
```bash
npm install @types/express @types/cors @types/morgan --save-dev
```
Now we can create an entry point at ./src/server.ts:
```ts
// ./src/server.ts
import express, {Request, Response} from "express";
import morgan from "morgan";
import cors from "cors";

export const createServer = () => {
	const app = express();
	app
		.disable("x-powered-by")
		.use(morgan("dev"))
		.use(express-urlencoded({ extended: true }))
		.use(express.json())
		.use(cors());

	app.get("/health", (req: Request, res: Response) => {
		res.json({ ok: true });
	});

	return app;
}
```
The only route we have added so far is a health check route, which returns true to show that the service is running.

Next, create an `index.ts` file in the `src` directory, import the create function from the server file and have it listen on a port:
```ts
// ./src/index.ts
import { createServer } from "./server";

const server = createServer();

port = process.env.PORT || 3000
server.listen(port, () => {
	console.log(`api running on port ${port}`)
})
```
At this point install nodemon as a dev dep with `npm install nodemon --save-dev` and then create a `nodemon.json` file at root with this contents:
```json
{
  "watch": ["src"],
  "ext": "js,ts,json",
  "ignore": ["src/**/*.test.ts"],
  "exec": "ts-node ./src/index.ts"
}
```
And don't forget to add a "dev": "nodemon" script to the `package.json`.

## Using await for async functions in express
Just look at [this](https://ioannisioannou.me/using-async-await-in-express-js/) article, it's pretty simple and this article does a good job of explaining it. I might add to this later once I have actually used it myself on a project.

A quick example from the FSO example phonebook app would be that this route handler:
```js
app.put("/api/persons/:id", (request, response, next) => {
  const { id, number } = request.body;
  Person.findByIdAndUpdate(id, { number: number })
    .then((updatedPerson) => {
      if (updatedPerson) {
        return response.json(updatedPerson.toJSON());
      } else {
        response.status(404).end();
      }
    })
    .catch((error) => next(error));
});
```
Could be re-written with async syntax as this:
```js
app.put("/api/persons/:id", async (request, response, next) => {
  const { id, number } = request.body;
  try {
	  const updatePerson = await Person.findByIdAndUpdate(id, { number: number })
	  if (updatePerson) {
		  return response.json(updatePersons.toJson());
	  } else {
		  response.status(404).end();
	  }
  } catch (err) {
	  next(err)
	}
});
```
Or you could implement an asyncHandler function to handle the try/catch block and implement it like this:
```js
const asyncHandler = (func) => (req, res, next) => {
  Promise.resolve(func(req, res, next))
    .catch(next)
}

app.put("/api/persons/:id", asyncHandler(async (request, response) => {
  const { id, number } = request.body;
  const updatePerson = await Person.findByIdAndUpdate(id, { number: number })
  if (updatePerson) {
	  return response.json(updatePersons.toJson());
  } else {
	  response.status(404).end();
  }
}));
```