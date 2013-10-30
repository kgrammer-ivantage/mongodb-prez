<img src="http://cloudtimes.org/wp-content/uploads/2011/05/mongo-db-logo.png" alt="MongoDB" />


##A Little History
* 10gen, a New York based start-up began working on a software PAAS in 2007
* Included an application server and a database.
* Goal was for developers to be able to concentrate on the application code,
so the software needed to __scale gracefully across multiple machines__
* Developers didn't like giving up so much control over their technology stack
* 10gen dropped the application server and concentrated solely on the database
* Result: __MongoDB__


##Design Features
* __NoSQL__
* __ad hoc queries__
* __horizontally scalable__
* __schemaless database__


##The Language of MongoDB
* table --> collection
* row --> document
* index --> index
* join --> embedded document
* foreign key --> reference
* partition --> shard


##Documents
* bson


##Documents
* _id object id


##Modeling Data
* one-to-one and one-to-many
* power of 2 (underlying memory usage)
* Referencing vs Embedding


##Replica Sets


##Sharding


##Javascript Shell & Drivers


##Command Line Tools
* mondodump and mongostore
* mongoexport and mongoimport


##Indexing
* b-tree
* 64 indexes (max) on a collection
* only 1 index used per query


##Data Aggregation
* Map-Reduce


##Next Release Features
