#### nth-salary-DB-Query

```
SELECT name, salary 
FROM #Employee e1
WHERE N-1 = (SELECT COUNT(DISTINCT salary) FROM #Employee e2
WHERE e2.salary > e1.salary)
```

#### Why use an index? Index has so many advantages, why not create an index for each column in the table? How does the index improve the query speed? Let me talk about the precautions of using the index? Mysql index mainly uses two data structures What is a covering index?
##### Why use an index?

1. By creating a unique index, you can guarantee the uniqueness of each row of data in the database table.
2. It can greatly speed up the retrieval of data (the amount of data retrieved is greatly reduced), which is also the main reason for creating indexes.
3. Helps the server avoid sorting and temporary tables
4. Change random IO to sequential IO
5. Can speed up the connection between the table, especially in terms of data reference integrity.

##### Indexes have so many advantages, why not create an index for each column in the table?

1. When data in the table is added, deleted, and modified, the index must also be dynamically maintained, which reduces the speed of data maintenance.
2. Indexes need to occupy physical space. In addition to data tables occupying data space, each index also takes up a certain amount of physical space. If a clustered index is to be established, the required space will be larger.
3.Creating and maintaining indexes takes time, and this time increases as the amount of data increases.

##### How does indexing improve query speed?

Turn unordered data into relatively ordered data (like looking up a directory)

##### Talk about the considerations of using the index

1. Avoid imposing functions on fields in the where clause, which can cause index misses.
2. When using InnoDB, use a business-independent primary key as the primary key, that is, use a logical primary key instead of a business primary key.
3. Set the column to be indexed to NOT NULL, otherwise it will cause the engine to give up using the index and perform a full table scan
4. Delete long-term unused indexes. The existence of unused indexes will cause unnecessary performance loss. MySQL 5.7 can query the sys library's schema_unused_indexes view to query which indexes have never been used
5. Index can be used to improve performance when query using limit offset is slow

##### What two data structures are used by Mysql indexes?

* Hash index: For a hash index, the underlying data structure is a hash table. Therefore, when the vast majority of queries are for a single record query, you can choose a hash index for the fastest query performance. For most other scenarios, it is recommended Select the BTree index.
* BTree index: Mysql's BTree index uses B + Tree in B-tree. But the implementation of the two main storage engines (MyISAM and InnoDB) is different.

For more on the index, you can check out my article: [Mind Map-Index] Getting a Database Index Is So Simple

##### What is a covering index?

If an index contains (or covers) the values ​​of all the fields to be queried, we call it a "covering index." We know that in the InnoDB storage engine, if it is not the primary key index, the leaf node stores the primary key + column value. In the end, it is necessary to "back to the table", that is, to look up again by the primary key, which will be slower. Covering an index means that the column to be queried corresponds to the index, and no table operation is performed!

#### explain what is pooling design thinking. What is a database connection pool? Why do you need a database connection pool?
Chi dialect design should not be a new term. Our common such as java thread pool, jdbc connection pool, redis connection pool, etc. are representative implementations of this type of design. This design will initially preset resources, and the problem solved is to offset the consumption of each resource acquisition, such as the overhead of creating a thread and the overhead of acquiring a remote connection. It's like you go to the cafeteria to cook, and the aunt who cooks the meal will put a few servings of rice there first, and you can just take the lunch box and add the vegetables, and you do n’t need to serve and cook again temporarily. In addition to initializing resources, the pooling design also includes the following characteristics: the initial value of the pool, the active value of the pool, the maximum value of the pool, etc. These features can be directly mapped to the member attributes of the java thread pool and database connection pool. ——This article is a good introduction to the design idea of pooling. Copy it directly to avoid rebuilding the wheel.

A database connection is essentially a socket connection. The database server also needs to maintain some caches and user permissions, so it takes up some memory. We can think of the database connection pool as a cache of database connections that can be reused when future database requests are needed. Opening and maintaining database connections for each user, especially requests for dynamic database-driven web applications, is expensive and wastes resources. **In the connection pool, once a connection is created, it is placed in the pool and used again, so there is no need to establish a new connection. If all connections are used, a new connection is established and added to the pool. Connection pooling also reduces the amount of time users have to wait to establish a connection to a database.
