# How to choose a Database in software system design?
Choices are SQL and NoSQL Databases.
From [video](https://www.youtube.com/watch?v=_Ss42Vb1SU4)

## Definitions & Comparison
### SQL (STRUCTRED QUERY LANGUAGE)
* Table based
* Vertically scalable (Increase RAM, CPU, etc.)
#### Strengths
1. Relational Databases, allowing easy querying on relationships between data among multiple tables. Table Relations are really important for effectively organizing and structuring a lot of different data.
2. Data is well structured and the room for potential error is therefore reduced. This is because SQL Schemas require the data model and format of the data to be known before storing anything.
3. SQL databases are ACID compliant, transactions[^1] are Atomic, Consistent, Isolated and Durable.
	- Atomic: All or nothing transactions
	- Consistent: Data is valid before and after
	- Isolated: Multiple transactions at the same time
	- Durable: Committed data is never lost
#### Weaknesses
1. Structured data might mean fewer error but also means that columns and tables have to be created ahead of time. So, SQL databases take more time to setup compared to NoSQL databases. They are also not effective for storing and query unstructured data, when format is unknown.
2. Difficult to scale horizontally because of their relational nature. For read heavy system it is straightforward to provision multiple ready-only replicas but for write heavy systems usually the options is to vertically scales the database up, which is generally more expensive than provisioning additional servers.

### NoSQL
* Document, Key/Value, Graph or wide column stores
* Horizontally scalable (Adding more instances) 
#### Strengths 
1. More flexible and simpler to set up because they do not support table relationships and data usually stored in documents or key/value pairs, they are better choice for storing unstructured data.
2. Because they are better at handling unstructured data, they can also Shard this data across different data stores allowing for distributed databases. This makes horizontally scaling much easier and large amount of data can be stored without having purchase a single expensive server.
#### Weaknesses
1. They are typically designed for distributed use cases and write heavy systems can be supported by having multiple write shards for the same data partition. This is called peer-to-peer replication. However the tradeoff is the lost of strong consistency. After a write to a shard in a distributed NoSQL cluster there is a small delay before that update can be propagated to other replicas. During this time reading from a replica can result in accessing stale data (Eventual Consistency)

[^1]: SQL Transactions are groups of statements that executed atomically.