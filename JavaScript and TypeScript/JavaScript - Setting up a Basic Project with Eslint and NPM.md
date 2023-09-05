#javascript 

This note will be concerned with my own personal workflow, so keep that in mind if you are not me and are reading this note.

Eslint is a utility that does both style checking and syntax/error detection for JS and TS. I use prettier for style checking in my JS and TS projects, so I won't be covering that aspect of it, and this note will only cover the very basics of getting eslint into a project. 

## Steps
1. Go to your project directory and initialize it as an npm project with `npm init -y`. This will add a standard boilerplate `package.json` file for managing any node modules you add
2. Install esLint as a development only dependancy with `npm install esLint --save-dev`
3. Add a config file to get esLint working and tell it what rules to follow. To do this run `npm init @eslint/config` and then follow the prompts to tell it how to set up the config for the needs of your project
4. Finally, don't forget to gitignore the `node_modules` directory.