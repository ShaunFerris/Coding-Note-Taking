#webdevSpecific 

Through all of the FSO course content covered so far, it has only touched on front-end development. Back end content will begin in part three ([[Webdev - FSO - Node.js and Express]]), but before getting into that we need to look at how the front-end of an application consumes data that it gets from the back end.

To start this section we will be using a tool meant for use in development and testing, called JSON server as a stand in for a proper back end server. This tool is available over npm, and can serve a json file over HTTP, using the local host ports. 

You can either install it globally with:
```bash
npm install -g json-server
```
and then serve your desired JSON with:
```bash
json-server --port 3001 --watch db.json
```
In the above example the json file we want to serve is called db.json, and should be in the directory from which you run the command. The watch flag automatically updates the served file if there are changes, and the port flag changes from the default port of 3000 to a specified one. 

You can also run it from the directory of the json file you want to serve with npx to avoid installing globally:
```bash
npx json-server --port 3001 --watch db.json
```

For the purposes of this example, the contents of the `db.json` file should be this:
```json
{
  "notes": [
    {
      "id": "1",
      "content": "HTML is easy",
      "important": true
    },
    {
      "id": "2",
      "content": "Browser can execute only JavaScript",
      "important": false
    },
    {
      "id": "3",
      "content": "GET and POST are the most important methods of HTTP protocol",
      "important": true
    }
  ]
}
```
and once served by the server, you can see the contents by navigating to `localhost:3001/notes` in the browser. Note the /notes in the url comes from the name of the json object. Also note that you can install a json view plugin for your browser to get a prettier view of the data.

Going forward in this exercise, the idea will be to save notes added in our example notes app to the server, which in this case means adding them to the json file being served. 

json-server stores all the data in the _db.json_ file, which resides on the server. In the real world, data would be stored in some kind of database. However, json-server is a handy tool that enables the use of server-side functionality in the development phase without the need to program any of it.

## The browser as a runtime environment
