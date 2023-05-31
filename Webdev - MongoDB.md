#webdevSpecific #noSQL #databases

Before I get started, the official Mongo docs are [here](https://www.mongodb.com/docs/manual/introduction/).

An Example set of steps to use MongoDB with Mongoose to give persistence to a budget tracking app can be found here: [[Webdev - MongoDB and Mongoose Example Steps]]

## What is MongoDB?
MongoDB is a noSQL (Not Only SQL) document database that stores data in flexible, JSON like documents. Fields can vary from document to document and the data structure can change over time. 

The document model **maps to the objects in your application code**, making data easy to work with.

MongoDB is a **distributed database at its core**, so high availability, horizontal scaling, and geographic distribution are built in and easy to use.

## Document databases
A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, arrays, and arrays of documents.

![A MongoDB document.](https://www.mongodb.com/docs/manual/images/crud-annotated-document.bakedsvg.svg)
The advantages of using documents are:
- Documents correspond to native data types in many programming languages.
- Embedded documents and arrays reduce need for expensive joins.
- Dynamic schema supports fluent polymorphism.

### Collections/Views/On-Demand Materialized Views[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/introduction/#collections-views-on-demand-materialized-views "Permalink to this heading")
MongoDB stores documents in [collections](https://www.mongodb.com/docs/manual/core/databases-and-collections/#std-label-collections). Collections are analogous to tables in relational databases.

In addition to collections, MongoDB supports:
- Read-only [Views](https://www.mongodb.com/docs/manual/core/views/) (Starting in MongoDB 3.4)
- [On-Demand Materialized Views](https://www.mongodb.com/docs/manual/core/materialized-views/) (Starting in MongoDB 4.2).