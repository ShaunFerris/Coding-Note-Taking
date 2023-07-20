#projects 

Now that homehub is hosted and ready for use, testing and further development, this doc will serve as a record of the things that I learn while maintaining and optimizing the site. Each entry will represent one day in which I did work on the site, and those notes will chain together, but will also each be accessible from here. The main focus of these entries will be finishing out all of the features of the sites inital scope and solving any problems that arise in production.

## 08/06/2023
Current issues to track down:
- [x] Next auth session callbacks failing due to failed GET endpoint request. Likely due to accessing the database in the callback to fetch userId. Look at the callback in the taxonomy git repos source code and see if you can pull the sessionUser Id from the token instead.
- [x] OAuth disallowed user agent issue. Only happend once and have not been able to reproduce. May be tied to the session callback issue but I am not sure. May require more devices/google accounts to test for this

- [[Project notes - Homehub - 09.06.2023]]
- [[Project notes - Homehub - 12.06.2023]]
- [[Project notes - Homehub - 15.06.2023]]
- [[Project notes - Homehub - 16.06.2023]]
- [[Project notes - Homehub - 18.06.2023]]
- [[Project notes - Homehub - 19.06.2023]]
- [[Project notes - Homehub - 20.06.2023]]
- [[Project Notes - Homehub - 8.07.2023]]
- [[Project Notes - Homehub - 20.07.2023 - tsconfig Issue]]

At this point the app is pretty stable and working as intended as far as my inital vision for it was concerned. Now I am planning some more impressive new features and overhauls to really make sure that it is an impressive project for my resume.

- [[Project Notes - Homehub - Version 1.1 planning]]