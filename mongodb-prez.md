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
* NoSQL (Non-relational)
* ad hoc queries
* horizontally scalable
* schemaless database
* __Performance is a primary goal!__


##Terminology
<img src="http://image.slidesharecdn.com/10-17rdbmstomongodb-131016140544-phpapp01/95/slide-4-638.jpg?" height=574 width=765 alt="terminology" />


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
<img src="http://www.uproxx.com/wp-content/uploads/2012/09/worldwar11.jpeg" alt="donkrasin" height="300" width="400"/>
<pre><code>{
	_id: ObjectId("505bd76785ebb509fc183733"),
	name: "Don Krasin",
	occupation: ["World War 11 Veteran"],
	birthyear: 5023
}</code></pre>


##Documents
<img src="http://www.worldwideinterweb.com/images/blogphotos/Funny/Greatest%20Job%20Titles%20Ever/best%20jobs%20titles%20ever.png" alt="berniepeyton" height="300" width="400"/>
<pre><code>{
	_id: ObjectId("507f1f77bcf86cd799439011"),
	name: "Dr. Bernie Peyton",
	occupation: ["Bear Biologist", "Paperfolder"] 
}</code></pre>


##Object ID
* ___id__ - every document has a unique __Object Id__
* A 12-byte BSON object that acts as a _primary key_
	* a 4-byte value representing the seconds since the Unix epoch,
	* a 3-byte machine identifier,
	* a 2-byte process id, and
	* a 3-byte counter, starting with a random value.

<pre><code>_id: ObjectId("507f1f77bcf86cd799439011")</code></pre>



##Modeling Data
* __No Joins__
* Denormalized data
	* multiple tables --> single collection
	* a single seek operation reduces query time
* Embedding
	* embedding a document in another document allows for a single-query to retrieve all information


##Data Storage
* Padding factor starts at 0,
	* as MongoDB learns DB usage, it changes padding
* If document exceeds space in memory location,
	* the whole document is deleted and rewritten in a new location
* __Power of 2__
	* you can tell MongoDB to use power of 2 sizes to pad document
	* 256, 512, etc...
	
	<pre><code>db.runCommand( {collMod: "collection1", usePowerOf2Sizes : true })</code></pre>


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


## Data Aggregation (cont.)
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

* $match-    _Filter documents_
* $group-    _Group documents by an identifier_
* $sort-     _Sorts documents according to a sort key_
* $limit-    _Restrict the number of documents returned_
* $skip-     _Skips over a number of documents, passes remainder_
* $project-  _Reshape documents by renaming, adding, or removing fields_
* $unwind-   _Peels elements off an array, turns them into stream of documents_
* $geoNear-  _Returns documents in order of nearest to farthest from a point_


## Aggregation Framework Example
<pre><code>
db.pokemon.aggregate(
	{ $match: {$gte: {$spatk: 100}}},
	{ $project: {name: -1, type: 1, spatk: 1}},
	{ $group: {_id: "$type", powerLevel: { $sum: "$spatk"}}}
)
</code></pre>


<img src="http://imgur.com/QgH040j.png" width="800" height="500"></img>

<img src="http://images.wikia.com/cardfight/images/7/7b/It-s-over-9000-its-over-9000-29849302-496-370.jpg" width="400" height="300"></img>


##Next Release Features
* Text search out of beta (indexing on string and array of string fields)
* Aggregation changes - db.collection.aggregate() returns a cursor instead
  of an array of documents.
* LDAP Authentication
* Geospatial improvements

