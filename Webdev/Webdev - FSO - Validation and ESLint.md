#webdevSpecific 

When working on POST routes that receive input data from the client, we often want to validate that data in some way before commiting it to the database. The notes app for example, should not accept any new notes that have a missing or emtpy `content` prop. The validity of a new note can be checked against this rule in the relevant route handler like this:
```js
app.post('/api/notes', (request, response) => {
  const body = request.body
  if (body.content === undefined) {
    return response.status(400).json({ error: 'content missing' })
  }
  // ...
})
```
If the `content` prop is missing, we just respond with a 400, bad request.

Alternatively, we could validate the data prior to db storage by simply using the [validation](https://mongoosejs.com/docs/validation.html) functionality available in Mongoose. We can define specific validation rules for each field of a valid notes object in the schma for that object like this:
```js
const noteSchema = new mongoose.Schema({
  content: {
    type: String,
    minLength: 5,
    required: true
  },
  important: Boolean
})
```
The `content` field is now reuqired to be at least five characters long and it is not allowed to be empty (required). The `minLength` and `required` validators are built-in to the Mongoose library, but we can also use Mongoose's [custom validator](https://mongoosejs.com/docs/validation.html#custom-validators) functionality to crate new validators for our needs.

If we try to store an object in the db that breaks one of the validation constraints in the schema definition, the operation will throw an exception. Let's change our handler for the PUT route so that it passses any potential exceptions to an error handler middleware:
```js
app.post('/api/notes', (request, response, next) => {
  const body = request.body

  const note = new Note({
    content: body.content,
    important: body.important || false,
  })

  note.save()
    .then(savedNote => {
      response.json(savedNote)
    })
    .catch(error => next(error))
})
```
And now expand the error handler to deal with validation errors:
```js
const errorHandler = (error, request, response, next) => {
  console.error(error.message)

  if (error.name === 'CastError') {
    return response.status(400).send({ error: 'malformatted id' })
  } else if (error.name === 'ValidationError') {
    return response.status(400).json({ error: error.message })
  }

  next(error)
}
```
When validation for an object fails, we return the following default message from Mongoose:
![[Pasted image 20241018135852.png]]The back end will now have a problem though: validations are not done when editing a note. The Mongoose [documentation](https://mongoosejs.com/docs/validation.html#update-validators) addressest this issue by explaining that validations are not run be default when the `findOneAndUpdate` and related methods are called. **The fix for this is easy though, we can just include the "run validators" option in the options object for the method call:**
```js
app.put('/api/notes/:id', (request, response, next) => {
  const { content, important } = request.body

  Note.findByIdAndUpdate(
    request.params.id, 
    { content, important },
    { new: true, runValidators: true, context: 'query' }
  ) 
    .then(updatedNote => {
      response.json(updatedNote)
    })
    .catch(error => next(error))
})
```

## Deploying the database to prod
Going to mostly skip this section, as it is really only concerned with adding an env variable to a fly.io or render app, which is baby town easy and I already did without thinking as soon as I added mongo to the phonebook app.

## Linting your code
The FSO course content takes a little aside here to go over the concept of Linting in general. It starts with this quote pulled from wikipedia that is probably worth reading:

> _Generically, lint or a linter is any tool that detects and flags errors in programming languages, including stylistic errors. The term lint-like behavior is sometimes applied to the process of flagging suspicious language usage. Lint-like tools generally perform static analysis of source code._

In compiled statically typed languages like Java, IDE's built for that environment like net beans can point out errors in the code, even ones that are more insidious than compile errors.

Additional tools for static analysis like checkstlye can be used for expanding the capabilities of the IDE for checking things like indent and line length.

In the JS ecosystem, the current leading tool for this kind of **Static Analysis** is **ESLint**. You can add ESLint to an exisiting project through npm:
```bash
npm install eslint @eslint/js --save-dev
```
**Remember to alter this if you are using typescript. In that case you should use this install:**
```bash
npm install --save-dev eslint @eslint/js @types/eslint__js typescript typescript-eslint
```
After install, we can sed a default ESLint config with this command:
```bas
npx eslint --init
```
This will start an interactive dialog to choose the details of your config. The config will be saved in an `eslint.config.mjs` file, that will look something like this:
```json
// ...
export default [
  {
    files: ["**/*.js"],
    languageOptions: {
      sourceType: "commonjs",
      globals: {
        ...globals.node,
      },
      ecmaVersion: "latest",
    },
  },
]
```
From here I mostly just copied the content direct from FSO:

So far, our ESLint configuration file defines the _files_ option with _["*_/_.js"]_, which tells ESLint to look at all JavaScript files in our project folder. The _languageOptions_ property specifies options related to language features that ESLint should expect, in which we defined the _sourceType_ option as "commonjs". This indicates that the JavaScript code in our project uses the CommonJS module system, allowing ESLint to parse the code accordingly.

The _globals_ property specifies global variables that are predefined. The spread operator applied here tells ESLint to include all global variables defined in the _globals.node_ settings such as the _process_. In the case of browser code we would define here _globals.browser_ to allow browser specific global variables like _window_, and _document_.

Finally, the _ecmaVersion_ property is set to "latest". This sets the ECMAScript version to the latest available version, meaning ESLint will understand and properly lint the latest JavaScript syntax and features.

We want to make use of [ESLint's recommended](https://eslint.org/docs/latest/use/configure/configuration-files#using-predefined-configurations) settings along with our own. The _@eslint/js_ package we installed earlier provides us with predefined configurations for ESLint. We'll import it and enable it in the configuration file:
```js
// ...
import js from '@eslint/js'
// ...

export default [
  js.configs.recommended,  {
    // ...
  }
]
```

We've added the _js.configs.recommended_ to the top of the configuration array, this ensures that ESLint's recommended settings are applied first before our own custom options.

Let's continue building the configuration file. Install a [plugin](https://eslint.style/packages/js) that defines a set of code style-related rules:```bash
npm install --save-dev @stylistic/eslint-plugin-js
```json

Import and enable the plugin, and add these four code style rules:

```js
// ...
import stylisticJs from '@stylistic/eslint-plugin-js'

export default [
  {
    // ...
    plugins: {
      '@stylistic/js': stylisticJs
    },
    rules: {
      '@stylistic/js/indent': [
        'error',
        2
      ],
      '@stylistic/js/linebreak-style': [
        'error',
        'unix'
      ],
      '@stylistic/js/quotes': [
        'error',
        'single'
      ],
      '@stylistic/js/semi': [
        'error',
        'never'
      ],
    },
  },
]
```

The [plugins](https://eslint.org/docs/latest/use/configure/plugins) property provides a way to extend ESLint's functionality by adding custom rules, configurations, and other capabilities that are not available in the core ESLint library. We've installed and enabled the _@stylistic/eslint-plugin-js_, which adds JavaScript stylistic rules for ESLint. In addition, rules for indentation, line breaks, quotes, and semicolons have been added. These four rules are all defined in the [Eslint styles plugin](https://eslint.style/packages/js).

Inspecting and validating a file like _index.js_ can be done with the following command:

```bash
npx eslint index.js
```

It is recommended to create a separate _npm script_ for linting:

```json
{
  // ...
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    // ...
    "lint": "eslint ."  },
  // ...
}
```

Now the _npm run lint_ command will check every file in the project.

Files in the _dist_ directory also get checked when the command is run. We do not want this to happen, and we can accomplish this by adding an object with the [ignores](https://eslint.org/docs/latest/use/configure/ignore) property that specifies an array of directories and files we want to ignore.

```js
// ...
export default [
  // ...
  { 
    ignores: ["dist/**"],
  },
  //...
]
```

This causes the entire _dist_ directory to not be checked by ESlint.

Lint has quite a lot to say about our code:

![terminal output of ESlint errors](https://fullstackopen.com/static/cdf7d27db507f48c4ab9f7bd59f8071f/5a190/53ea.png)

Let's not fix these issues just yet.

A better alternative to executing the linter from the command line is to configure an _eslint-plugin_ to the editor, that runs the linter continuously. By using the plugin you will see errors in your code immediately. You can find more information about the Visual Studio ESLint plugin [here](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).

The VS Code ESlint plugin will underline style violations with a red line:

![Screenshot of vscode ESlint plugin showing errors](https://fullstackopen.com/static/64cf2fbae36000083aa1e48292aed8f2/5a190/54a.png)

This makes errors easy to spot and fix right away.

ESlint has a vast array of [rules](https://eslint.org/docs/rules/) that are easy to take into use by editing the _eslint.config.mjs_ file.

Let's add the [eqeqeq](https://eslint.org/docs/rules/eqeqeq) rule that warns us if equality is checked with anything but the triple equals operator. The rule is added under the rules field in the configuration file.

```js
export default [
  // ...
  rules: {
    // ...
   'eqeqeq': 'error',
  },
]
```

While we're at it, let's make a few other changes to the rules.

Let's prevent unnecessary [trailing spaces](https://eslint.org/docs/rules/no-trailing-spaces) at the ends of lines, require that [there is always a space before and after curly braces](https://eslint.org/docs/rules/object-curly-spacing), and also demand a consistent use of whitespaces in the function parameters of arrow functions.

```js
export default [
  // ...
  rules: {
    // ...
    'eqeqeq': 'error',
    'no-trailing-spaces': 'error',
    'object-curly-spacing': [
      'error', 'always'
    ],
    'arrow-spacing': [
      'error', { 'before': true, 'after': true },
    ],
  },
]
```

Our default configuration takes a bunch of predefined rules into use from:

```js
// ...

export default [
  js.configs.recommended,
  // ...
]
```

This includes a rule that warns about _console.log_ commands. Disabling a rule can be accomplished by defining its "value" as 0 or "off" in the configuration file. Let's do this for the _no-console_ rule in the meantime.

```js
[
  {
    // ...
    rules: {
      // ...
      'eqeqeq': 'error',
      'no-trailing-spaces': 'error',
      'object-curly-spacing': [
        'error', 'always'
      ],
      'arrow-spacing': [
        'error', { 'before': true, 'after': true },
      ],
      'no-console': 'off',
    },
  },
]
```

Disabling the no-console rule will allow us to use console.log statements without ESLint flagging them as issues. This can be particularly useful during development when you need to debug your code. Here's the complete configuration file with all the changes we have made so far:

```js
import globals from "globals";
import stylisticJs from '@stylistic/eslint-plugin-js'
import js from '@eslint/js'

export default [
  js.configs.recommended,
  {
    files: ["**/*.js"],
    languageOptions: {
      sourceType: "commonjs",
      globals: {
        ...globals.node,
      },
      ecmaVersion: "latest",
    },
    plugins: {
      '@stylistic/js': stylisticJs
    },
    rules: {
      '@stylistic/js/indent': [
        'error',
        2
      ],
      '@stylistic/js/linebreak-style': [
        'error',
        'unix'
      ],
      '@stylistic/js/quotes': [
        'error',
        'single'
      ],
      '@stylistic/js/semi': [
        'error',
        'never'
      ],
      'eqeqeq': 'error',
      'no-trailing-spaces': 'error',
      'object-curly-spacing': [
        'error', 'always'
      ],
      'arrow-spacing': [
        'error', { 'before': true, 'after': true },
      ],
      'no-console': 'off',
    },
  },
  { 
    ignores: ["dist/**", "build/**"],
  },
]
```

**NB** when you make changes to the _eslint.config.mjs_ file, it is recommended to run the linter from the command line. This will verify that the configuration file is correctly formatted:

![terminal output from npm run lint](https://fullstackopen.com/static/07f46b5e48a00a479880e6625fcf342f/5a190/lint2.png)

If there is something wrong in your configuration file, the lint plugin can behave quite erratically.

## Next
This is the end of part three of FSO. Part four covers testing node applications and implementing auth, and begins here: [[Webdev - FSO - Structuring a backend project]]