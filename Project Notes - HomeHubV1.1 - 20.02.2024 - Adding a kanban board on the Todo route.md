#projects 

Returning to do some work on homeHub for the first time in a few months. Have decided to build a new feature into the Todo route where you can choose to use the old system, or you can open a kanban board for for a given larger task and use that to track the progress of the associated sub-tasks. 

## The kanban component
This started with building the actual component, the idea for which came from [this](https://www.youtube.com/watch?v=O5lZqqy7VQE) video. I followed the outline of the code in this video but re-wrote it using typescript and re-did the styling. As of today (feb 20th) the front-end is mostly done, but I still need to:
- make it responsive and useable on mobile
- connect it to the backend and implement database models for it's data
- implement a page that comes off the todo route and allows creation/re-entrance to saved kanban boards for projects and leads to dynamically routed kanban board pages

## The new todo portal page
Am going to implement a new root page for the todo route, which asks you to select if you want to see general todos, or would like to use the kanban boards. If you select kanban boards it goes to another new page where you can create a new blank board or re-enter a previously saved one.