<img src="http://cloudtimes.org/wp-content/uploads/2011/05/mongo-db-logo.png" alt="MongoDB" />


##A Little History
* 10gen, a New York based start-up began working on a software PAAS in 2007
	* Included an application server and a database.
* Goal was for developers to be able to concentrate on the application code
	* the software needed to __scale gracefully across multiple machines__
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


##Documents (cont.)
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
* __Denormalized Data__
	* multiple tables --> single collection
	* a single seek operation reduces query time
* __Embedding__
	* embedding a document in another document allows for a single-query to retrieve all information


##Data Storage
* A document's padding factor starts at 0,
	* as MongoDB learns DB usage, it changes padding
* If document exceeds space in memory location,
	* the whole document is deleted and rewritten in a new location
	* this can be costly if done many times a second
* __Power of 2__
	* you can tell MongoDB to use _power of 2_ sizes to pad document
	* 256, 512, etc...
	
	<pre><code>db.runCommand( {collMod: "collection1", usePowerOf2Sizes : true })</code></pre>


##Scaling
<table width=><tr><td>&nbsp;&nbsp;&nbsp;&nbsp;</td><td>
<img src="http://image.slidesharecdn.com/7-18sharding-130718171220-phpapp01/95/slide-7-638.jpg" alt="vertical scaling" width="478" height="359" />
<ul><li>More powerful machines = more $$$ </li><li>Single machine = single point of failure</ul>
</td><td>&nbsp;&nbsp;&nbsp;&nbsp;</td><td>
<img src="http://image.slidesharecdn.com/7-18sharding-130718171220-phpapp01/95/slide-8-638.jpg" alt="horizontal scaling" width="478" height="359" />
<ul><li>Can use lots of cheap machines </li></ul>
</td></tr></table>


##Replica Sets
!["replica sets"](http://docs.mongodb.org/master/_images/replica-set-read-write-operations-primary.png)

* Writes __only__ to Primary node
* Reads can come from Secondary nodes to distribute load
	* Secondary nodes may not be completely up to date


##Replica Sets (cont.)
!["heatbeat"](http://docs.mongodb.org/master/_images/replica-set-primary-with-two-secondaries.png)

* When Primary node fails, a Secondary node is elected to be the new Primary
* __Arbiters__ are nodes used to decide which node should take over as Primary
	* Obviously this node should not live on the same machine as Primary
* When the original Primary comes back, it enters __recovery mode__
	* If this node's priority is highest, it will force an election and become the Primary again


##Sharding
* Separates collection data across multiple servers
<img src="http://docs.mongodb.org/master/_images/sharded-cluster-primary-shard.png" alt="shards" height="327" width="300" />
* __Shard__ - a node of the cluster. Can be a single <code>mongod</code> or a replica set
* __Chunk__ - shards are split up into chunks of 64 MB or less. 
* __Config Server__ - Holds the cluster's metadata. Stores cluster chunk ranges and locations
* __Shard Key__ - (user defined) defines a range of data to distribute the data among the shards
	* __choosing a good shard key is _very_ important__


##Sharding Architecture

!["shard keys"](http://docs.mongodb.org/master/_images/sharded-cluster.png)

* Applications do not access the shards directly
	* __mongos__ - routes the reads/writes from the applications to the shards


##Who Sharded?
* When should we shard?
	* when throughput exceeds I/O
	!["working set"](http://image.slidesharecdn.com/7-18sharding-130718171220-phpapp01/95/slide-6-638.jpg)


##Javascript Shell & Drivers

`mongo` command - fully featured Javascript shell

Drivers available for a multitude of platforms

* NodeJS
* Python
* Ruby
* PHP
* ... and many more



##Command Line Tools
* `mondodump` and `mongostore`
	* backup and store DBs easily in native BSON format
* `mongoexport` and `mongoimport`
	* Import/Export DBs as JSON, CSV, TSV


##Indexing
* Can be defined pretty much anywhere, support secondary indexes
	* _id, Single field, Compound, Multikey (arrays)
	* Geospatial, Text
	* Sub-documents and fields of sub-documents
	* Unique and Sparse
* Used properly improves read speed, writes take a hit
* Uses B-Trees
<img src="http://upload.wikimedia.org/wikipedia/commons/thumb/6/65/B-tree.svg/400px-B-tree.svg.png"></img>
* Limitations
	* 64 indexes (max) on a collection
	* Only 1 index used per query


## Creating and Using Indexes
<pre><code>
db.pokemon.ensureIndex( { pokedexnum: 1}, { background: true, unique:true})
</pre></code>

__Options__

* background
* unique, dropDups (_DANGEROUS!_)
* sparse

<pre><code>
db.pokemon.find( { pokedexnum: { $gt: 151, $lte: 251}})
</pre></code>

* Use `.hint()` to specify which index to use
* `.explain()` is useful for debugging


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
	{ $match: {spatk: {$gte: 100}}},
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

