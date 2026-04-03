# What is Database Sharding?

From [Video](https://www.youtube.com/watch?v=be6PLMKKSto)

Traditionally, data has been stored in RDBMS or Relational Database Management System. As data increases traditional database systems rans into bottlenecks on CPU, memory or disk usage.

Sometimes, the structure of data is such that the table holding data can be broken up and spread across multiple servers. The process of breaking up large tables into horizontal data partitions, each of which contains a subset of the whole table and putting each partition in a separate database server is called Sharding. Each partition called A Shard.

## Sharding techniques
Most times partition data will depend on the structure of data itself. A few common sharding techniques are: 
- GeoBase Sharding, data is partitioned based on the user location
- Rang Base Sharding
- Hash Base Sharding
### Manual vs. Automatic Sharding
Some databases supports automatic sharding but most of monolithic databases don't support automatic sharding. In such a case sharding can be done in application layer but this will significantly increasing development complexity. 

## Advantages
1. Allows system to scale out as the size of data increases, allows the application to deal with larger amount of data then can be done using traditional RDBMS.
2. Smaller set of data in each shard also means that the indexes on that data are smaller which results in faster query performance.
3. If an unplanned outage takes down a shard the majority of the system stays accessible while that shard is restored. Down time doesn't take out the whole system.
4. Smaller amount of data on each shard means that the nodes can run on commodity hardware and do not require expensive high-end hardware to deliver acceptable performance.
## Disadvantages
1. Not all data is amenable to sharding, foreign key relationships can only be maintained within single shard. 
2. Manual sharding can be complex and lead to hotspots. 
3. Since each shards run on separate databases servers some type of cross shard queries such as table joins are either very expensive or just not possible. 
4. Once sharding is setup it is very hard if not impossible in some systems to undo sharding or to change the shard key
5. Increase operation cost