---
author: Anton Stroganov (@aeon)
---

[@caseyrosenthal](http://twitter.com/caseyrosenthal) from Basho (RIAK):

- High availability... is harder than it sounds.
	- The internet is generally highly available and eventually consistent...
	- SQL databases are strongly consistent but not necessarily highly available
- Two mindsets
	- SQL
		- get a problem, model it, normalize, denormalize, set up indexes... 
		- hopefully once it's done, it will represent the information well to the client
		- much harder to scale in a fault-tolerant way  
	- K/V system
		- start with the idea of how you want to present the data
		- the model is basically the materialization of the presentation
		- much easier to scale due to simplicity
- Expect bad things to happen to your servers, because they do
- create inverted indexes to lookup records by other fields.
	- very simple... just array of primary keys that have the same value for secondary key.
	- but where do we store it?
	- option one, document-based inverted index
		- store it next to the object we are indexing... but then we have copies of the same index on all boxes that has records it references.
		- very efficient for writes, but for reads, have to check all machines in case any of them have relevant records.
	- option two, term-based inverted index
		- store it in a single place on the cluster
		- less efficient for writes, but reads are faster
	- both are useful, pick the one that will fit the use model better.
- what happens when database gets split...
	- resolving conflicts... is difficult.
		- timestamps in a distributed system are unreliable, clocks drift...
	- one, push the conflict resolution to application, just store sibling data and present both
	- two, present both versions, let human decide
	- Riak is implementing CRDTs - research into the kinds of data structures that can be automatically converged into a single version without conflicts...
		- HA GeoHashes
		- HA ACiD transactions

Further info:
- zombies.samples.basho.com
- Look for a talk called "Distributed Systems Archaeology" - there is lots of existing academic research that has never really been applied in practice...