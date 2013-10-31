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


##Structure
* A __collection__ is made up of _documents_
* A __document__ is made up of _fields_
* A __Cursor__ is returned when you query for data,
	* _Cursors_ can be used to count, skip ahead, etc. before they execute


##Relational vs Document-Oriented DBs
* A relational DB defines its _columns_ at the _table_ level
* A document-oriented DB defines its _fields_ at the _document_ level
	* each document can have its own structure


##Documents
<pre><code>
{
	_id: ObjectId("507f1f77bcf86cd799439011"),
	name: "Dr. Bernie Peyton",
	occupation: ["Bear Biologist", "Paperfolder"] 
}
</code></pre>
* ___id__ - every document has a unique __Object Id__
* A 12-byte BSON object that acts as a _primary key_
	* a 4-byte value representing the seconds since the Unix epoch,
	* a 3-byte machine identifier,
	* a 2-byte process id, and
	* a 3-byte counter, starting with a random value.

<img src="http://www.worldwideinterweb.com/images/blogphotos/Funny/Greatest%20Job%20Titles%20Ever/best%20jobs%20titles%20ever.png" />



##Modeling Data
* __No Joins__
* Denormalized data
	* multiple tables --> single collection
	* a single seek operation reduces query time



##Data Storage
* Padding factor starts at 0,
	* as MongoDB learns DB usage, it changes padding
* If document exceeds space in memory location,
	* the whole document is deleted and rewritten in a new location
* __Power of 2__
	* you can tell MongoDB to use power of 2 sizes to pad document
	* 256, 512, etc...



* Referencing vs Embedding


##Replica Sets


##Sharding


##Javascript Shell & Drivers


##Command Line Tools
* mondodump and mongostore
	* backup and store DBs easily in the native BSON format
* mongoexport and mongoimport
	* json, csv, tsv


##Indexing
* b-tree
* 64 indexes (max) on a collection
* only 1 index used per query


##Data Aggregation
* Pre-aggregated data
* Aggregation Framework
	* Data processing pipeline
	* Good for performing ad-hoc queries
	* Limitations
		* Final result limited by maximum BSON document size
		(16 MB) - problem should be alleviated in 2.6
		* Pipeline operator memory limited to 10% of RAM
* MongoDB Map-Reduce
	* Inteded for complex data analysis
	* Implemented w/ Javascript
	* Limitations
		* Hard to debug
		* Slower than aggregation framework - used primarily for batch jobs
* External Map-Reduce (e.g. Hadoop)


## Aggregation Framework
Starting with all documents in a collection, stream data from one aggregator
to the next.

__Operators__

* $match
* $group
* $sort
* $limit
* $project
* $unwind
* $geoNear


##Next Release Features
* Text search out of beta (indexing on string and array of string fields)
* Aggregation changes - db.collection.aggregate() returns a cursor instead
  of an array of documents.
* LDAP Authentication
* Geospatial improvements

