# Scheme of the Essay

This is what we are gonna use to guide ourselves through the writing process. 

## Introduction 

Purpose of the investigation = understand the functioning of a NoSQL database + Demo. Current data has no structure. Difficult to store in relational ways. Quote Efficient Query Analysis and Performance Evaluation of the Nosql Data Store for BigData. In this case we review CouchDB.

### Definition

CouchDB is open-source, cross-platform, NoSQL document database. Internal architecture for the web to handle massive amounts of data. Makes use of Couch replication protocol to synchronize JSON documents between two peers over HTTP. Example, requests are done to certain URL to obtain info about databases. Allows use of CURL.

Databases of CouchDB store documents. Documents consist of a number of fields, can be of many types, along with metadata. Edited by the client by loading them, changing and saving them back. Provide a RESTful API to do this. The transactions are all or nothing, never partially edited documents. 

Provides all the ACID properties. Updates are serialized, reads are concurrent. Use of MVCC. Documents organized through BTrees. Its updates occur at the end of a transaction, leaving the database in a consistent state.

It's purpose is to work with non-structured data, particularly for the web. Quote at the Forge. Useful for those sort of production environment. Borrows heavily from web architecture. Particular advantages are scalabilty and replication. 

### Comparative Table

Principal differnces: Erlang, JSON, lack of a query language, 

### Uses

Relatively small market share (3853). Used when large data volumes and cloud based storage. Due to them being highly scalable (RDB can't handle more than 1024 columns). Quote Rascovsky. Advances in some environments. 

Quote Hanlon. Faster access to data for the users. When there is disk storage available. Btree index and views stored in them. Used in Teragrid user portal

## Characteristics

### DataModel

### Architecture of Distributed Storage

Cite Padhy. Storage is btree based, and accesed by key or ranges (which use the tree operations), fast. View Engine written in JS, based on MapReduce jobs, serves for indices and extract data. Replicator to a local or remote database and sync. Graphic can be seen. 

#### Fragmentation, Asignation and Replication

CouchDB works in an append only insertion, which works against fragmentation. Appends are in the end of the document. No need for reallocation. It also allows compaction, writes revision to the new database files (delete documents). Then replaces the old one with the compacted one.

Replication can be transient and persistent. Definition of transient. Definition of persistent. Replication works by comparing the target to the souce. Only transfer those who differ and delete the ones who are not anymore.

Replication finishes when all the changes are not anymore, with some checkpoints in-between. Replication is done in pne direction only. So two tasks are needed to do replication in both ways. 

According to the official documentation of couchDB, replication can be done in 3 ways: 

1. Local
2. Selector 
3. Filter functions

#### Processing of Distributed Queries

Distributed Computation is possible thanks to the use of MapReduce programming model. Cite sciabarra. The map produces values that are classified through keys, and reduce aggregates the results. The mapreduce function is embedded in JS. The database does most of the job, while the developer extracts the results. 

The architecture of queries is direct shard connection only. The component that performs the shard key resolution is the query coordinator. One disadvantage is that CouchDB doesn't support queries using non-shard key value. The merge of multiple results from multiple shard is by the query coordinator to all the clients. Sorted order is supported. 

CouchDB Lounge is a package that allows for clustering with constant hashing, which allows the merge from constantly hashed nodes to be easier. This is currently being moved to Erlang. 

## Conclusions

After researching and doing a simple implementation of CouchDB succesfully, we managed to reach the following conclusions.

1. The replication benefits of CouchDB allows for advantages when working with distributed databases. Transient replication allows for backward compatibility with older versions of the platform. And persistent replication allows a document with parameters to be set so the developer knows exactly what the replication process is taking place. Also they offer a variety of ways to do it, which might speed up the process (in some cases selector and in some filter functions). In summary, this allows for a highly scalable database that is also resistent to failures due to the use of checkpoints and the connection between different nodes, with the only disadvantage being the lack of master-master replications. 