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