# Tables and Table Clusters

## Introduction to Schema Objects

A database [schema](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#i996727) is a logical container for data structures, called schema objects. Examples of schema objects are tables and indexes. Schema objects are created and manipulated with [SQL](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#i432766).

### rowids

Every row stored in the database has an address. Oracle Database uses a `ROWID` data type to store the address ([rowid](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDEFIHG)) of every row in the database. Rowids fall into the following categories:

-Oracle Database uses rowids internally for the construction of indexes. A [B-tree index](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#BGBFAGFF), which is the most common type, contains an ordered list of keys divided into ranges. Each key is associated with a rowid that points to the associated row's address for fast access. End users and application developers can also use rowids for several important functions:

- Rowids are the fastest means of accessing particular rows.

- Rowids provide the ability to see how a table is organized.

- Rowids are unique identifiers for rows in a given table.

ROWID                   GRADE      LOSAL      HISAL  

------------------ ---------- ---------- ----------  

AAAVw1AAHAAAACTAAA          1        700       1200  
AAAVw1AAHAAAACTAAB          2       1201       1400  
AAAVw1AAHAAAACTAAC          3       1401       2000  
AAAVw1AAHAAAACTAAD          4       2001       3000  
AAAVw1AAHAAAACTAAE          5       3001       9999

The database stores rows in data blocks. Each row of a table containing data for less than 256 columns is contained in one or more row pieces.

### Object Tables

An Oracle object type is a user-defined type with a name, attributes, and methods. Object types make it possible to model real-world entities such as customers and purchase orders as objects in the database.

## Name

OLTP: OLTP（OnLine Transaction Processsing 联机事务处理）是与功能、业务强相关的事务查询系统，要保证高并发场景下低时延的查询和处理效率，因此对CPU的性能要求较高。而直接存储与功能和业务直接相关的地方，就叫数据库，大多时候也叫业务服务器（简称：业务服），一般是服务端开发的同事来负责。

# 3 Indexes and Index-Organized Tables

## Overview of Indexes

An index is an optional structure, associated with a table or [table cluster](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDJGGGF), that can sometimes speed data access. By creating an [index](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#i432409) on one or more columns of a table, you gain the ability in some cases to retrieve a small set of randomly distributed rows from the table. Indexes are one of many means of reducing disk I/O.

If a heap-organized table has no indexes, then the database must perform a [full table scan](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDCGIFF) to find a value.



#### Composite Indexes

A composite index, also called a concatenated index, is an index on multiple columns in a table. Columns in a composite index should appear in the order that makes the most sense for the queries that will retrieve data and need not be adjacent in the table.

#### Types of Indexes

- B-tree indexes
  
  These indexes are the standard index type. They are excellent for primary key and highly-selective indexes. Used as concatenated indexes, B-tree indexes can retrieve data sorted by the indexed columns. B-tree indexes have the following subtypes:
  
  - Index-organized tables
    
    An index-organized table differs from a heap-organized because the data is itself the index. See ["Overview of Index-Organized Tables"](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#CBBJEBIH).
  
  - Reverse key indexes
    
    In this type of index, the bytes of the index key are reversed, for example, 103 is stored as 301. The reversal of bytes spreads out inserts into the index over many blocks. See ["Reverse Key Indexes"](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#CBBFDEAJ).
  
  - Descending indexes
    
    This type of index stores data on a particular column or columns in descending order. See ["Ascending and Descending Indexes"](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#CBBFFFFG).
  
  - B-tree cluster indexes
    
    This type of index is used to index a table cluster key. Instead of pointing to a row, the key points to the block that contains rows related to the cluster key. See ["Overview of Indexed Clusters"](https://docs.oracle.com/cd/E11882_01/server.112/e40540/tablecls.htm#CFABHBAG).

- Bitmap and bitmap join indexes
  
  In a bitmap index, an index entry uses a bitmap to point to multiple rows. In contrast, a B-tree index entry points to a single row. A bitmap join index is a bitmap index for the join of two or more tables. See ["Bitmap Indexes"](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#CBBFJFDD).

- Function-based indexes
  
  This type of index includes columns that are either transformed by a function, such as the `UPPER` function, or included in an expression. B-tree or bitmap indexes can be function-based. See ["Function-Based Indexes"](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#CBBGIIFB).

- Application domain indexes
  
  This type of index is created by a user for data in an application-specific domain. The physical index need not use a traditional index structure and can be stored either in the Oracle database as tables or externally as a file. See ["Application Domain Indexes"](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#CBBFEBGI).



### B-Tree Indexes

- **balanced**

- **branch blocks for searching and leaf blocks that store values.**
  
  





A B-tree index has two types of blocks: **branch blocks for searching and leaf blocks that store values.** The upper-level branch blocks of a B-tree index contain index data that points to lower-level index blocks. In [Figure 3-1](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#i5765), the root branch block has an entry `0-40`, which points to the leftmost block in the next branch level. This branch block contains entries such as `0-10` and `11-19`. Each of these entries points to a leaf block that contains key values that fall in the range.

A B-tree index is balanced because all leaf blocks automatically stay at the same depth. Thus, retrieval of any record from anywhere in the index takes approximately the same amount of time. The height of the index is the number of blocks required to go from the root block to a leaf block. The branch level is the height minus 1. In [Figure 3-1](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#i5765), the index has a height of 3 and a branch level of 2.

Branch blocks store the minimum key prefix needed to make a branching decision between two keys. This technique enables the database to fit as much data as possible on each branch block. The branch blocks contain a pointer to the child block containing the key. The number of keys and pointers is limited by the block size.

**The leaf blocks contain every indexed data value and a corresponding rowid used to locate the actual row**. Each entry is sorted by (key, rowid). Within a leaf block, a key and rowid is linked to its left and right sibling entries. The leaf blocks themselves are also doubly linked. In [Figure 3-1](https://docs.oracle.com/cd/E11882_01/server.112/e40540/indexiot.htm#i5765) the leftmost leaf block (`0-10`) is linked to the second leaf block (`11-19`).



##### Full Index Scan

In a full index scan, the database reads the entire index in order. A full index scan is available if a [predicate](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDGAHDF) (`WHERE` clause) in the SQL statement references a column in the index, and in some circumstances when no predicate is specified. A full scan can eliminate sorting because the data is ordered by index key.

##### Fast Full Index Scan

A fast full index scan is a full index scan in which the database accesses the data in the index itself without accessing the table, and the database reads the index blocks in no particular order.

Fast full index scans are an alternative to a [full table scan](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDCGIFF) when both of the following conditions are met:

- The index must contain all columns needed for the query.

- A row containing all nulls must not appear in the query result set. For this result to be guaranteed, at least one column in the index must have either:
  
  - A `NOT NULL` constraint
  
  - A predicate applied to it that prevents nulls from being considered in the query result set



##### Index Range Scan

An index range scan is an ordered scan of an index that has the following characteristics:

- One or more leading columns of an index are specified in conditions. A condition specifies a combination of one or more expressions and logical (Boolean) [operators](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDJCEGF) and returns a value of `TRUE`, `FALSE`, or `UNKNOWN`.

- 0, 1, or more values are possible for an index key.

The database commonly uses an index range **scan to access selective data**. The [selectivity](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDFBHAH) is the percentage of rows in the table that the query selects, with 0 meaning no rows and 1 meaning all rows. Selectivity is tied to a query [predicate](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#CHDGAHDF), such as `WHERE last_name LIKE 'A%'`, or a combination of predicates. A predicate becomes more selective as the value approaches 0 and less selective (or more unselective) as the value approaches 1.

定位+横向查找；





##### Index Unique Scan

In contrast to an index range scan, an index unique scan must have either 0 or 1 rowid associated with an index key. The database performs a unique scan when a predicate references all of the columns in a `UNIQUE` index key using an equality operator. An index unique scan stops processing as soon as it finds the first record because no second record is possible.



##### Index Skip Scan

An index skip scan uses logical subindexes of a composite index. The database "skips" through a single index as if it were searching separate indexes. Skip scanning is beneficial if there are few distinct values in the leading column of a composite index and many distinct values in the nonleading key of the index.

Oracle中的索引跳跃式扫描仅仅适用于那些目标索引前导列的distinct值数量较少、后续非前导列的可选择性又非常好的情形，因为索引跳跃式扫描的执行效率一定会随着目标索引前导列的distinct值数量的递增而递减。



##### Index Clustering Factor(集群因子）

The [index clustering factor](https://docs.oracle.com/cd/E11882_01/server.112/e40540/glossary.htm#BGBJGGJI) measures row order in relation to an indexed value such as employee last name. The more order that exists in row storage for this value, the lower the clustering factor.

 ----row存储的越有序，clustering factor的值越低。

  简单的说，Index Clustering Factor是通过一个索引扫描一张表，需要访问的表的数据块的数量，即对I/O的影响，也代表索引键存储位置是否有序。

    (1)、如果越有序，即相邻的键值存储在相同的block，那么这时候Clustering Factor的值就越低；

    (2)、如果不是很有序，即键值是随机的存储在block上，这样在读取键值时，可能就需要一次又一次的去访问相同的block，从而增加了I/O。

    Clustering Factor的计算方式如下：

     (1)、扫描一个索引(large index range scan)；

     (2)、比较某行的rowid和前一行的rowid，如果这两个rowid不属于同一个数据块，那么cluster factor增加1；

     (3)、整个索引扫描完毕后，就得到了该索引的clustering factor。

            如果clustering factor接近于表存储的块数，说明这张表是按照索引字段的顺序存储的。

            如果clustering factor接近于行的数量，那说明这张表不是按索引字段顺序存储的。

            **在计算索引访问成本的时候，这个值十分有用。** Clustering Factor乘以选择性参数(selectivity)就是访问索引的开销。

            如果这个统计数据不能真实反映出索引的真实情况，那么可能会造成优化器错误的选择执行计划。另外如果某张表上的大多数访问是按照某个索引做索引扫描，那么将该表的数据按照索引字段的顺序重新组织，可以提高该表的访问性能。

#### Reverse Key Indexes

可以高效地打散正常的索引键值在索引叶块中的分布位置。

避免热点块的存在数据库的热点块，从简单了讲，就是极短的时间内对 少量数据块进行了过于频繁的访问。

### Bitmap Indexes

In a bitmap index, the database stores a bitmap for each index key. In a conventional B-tree index, one index entry points to a single row. In a bitmap index, each index key stores pointers to multiple rows.

上面讲了，位图索引适合只有几个固定值的列，如性别、婚姻状况、行政区等等，而身份证号这种类型不适合用位图索引。　　此外，位图索引适合静态数据，而不适合索引频繁更新的列。

### Function-Based Indexes

You can create indexes on functions and expressions that involve one or more columns in the table being indexed. A function-based index computes the value of a function or expression involving one or more columns and stores it in the index. A function-based index can be either a B-tree or a bitmap index.
