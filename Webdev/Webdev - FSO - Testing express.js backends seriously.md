#webdevSpecific 

In the last block of FSO course content, we looked at writing basic tests, but only for utility functions that we wrote in preparation for actual tests. 

Moving on to test the actual backend, we should consider that the backend is currently so simple that it doesn't make sense to test it. It does not contain any complex logic or transformations of data. The only thing we could really unit test at the moment is the `toJSON` method used for formatting the notes. 

In some situations, it can be beneficial to implement some of the backend tests by mocking the database instead of using the real db. One libraray that could be used for this is [mongodb-memory-server](https://github.com/nodkz/mongodb-memory-server).

Since our apps backend is still relatively simple, we will decide to test the entire app through it's REST API, so that the database is also included. The kind of testing where multiple components of the system are being tested in a group is called [integration testing](https://en.wikipedia.org/wiki/Integration_testing). Unit testing would require isolation from the database and thus would involve mocking.

## Test environment
