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
