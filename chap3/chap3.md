# Storage and retrieval

In this chapter, the author will examine two families of storage engines: log-structured storage engines, and page-oriented 
storage engines such as B-trees.

## Data structures that power your database

-  In order to efficiently find the value for a particular key in the database, we need a different data structure: an *index*.
-  This is an important trade-off in storage systems: well-chosen indexes speed up read queries, but every index slows down writes.

## Hash indexes

-  An append-only design is good for the following reasons:
   -  Appending and segment merging are sequential write operations, which are generally much faster than random writes, especially on magnetic spinning-disk hard drives;
   -  Concurrency and crash recovery are much simpler if segment files are appendonly or immutable;
   -  Merging old segments avoids the problem of data files getting fragmented over time.
   
-  The hash table index also have the following limitations:
   -  The hash table must fit in memory, so if you have a very large number of keys, you’re out of luck.
   -  Range queries are not efficient. For example, you cannot easily scan over all keys between **kitty00000** and **kitty99999**—you’d have to look up each key individually in the hash maps.
   
## SSTables and LSM-Trees

SSTables(*Sorted String Table*) is a simple modification of hash table indexes: we require that the sequence of *key-value* pairs is sorted by key, and each key only appears once within each merged segment file

-  SSTables have several big advantages over log segments with hash indexes:
   -  Merging segments is simple and efficient, even if the files are bigger than the available memory.
   -  In order to find a particular key in the file, you no longer need to keep an index of all the keys in memory.
   -  Since read requests need to scan over several key-value pairs in the requested range anyway, it is possible to group those records into a block and compress it before writing it to disk.
   
## B-tree

Like SSTables, B-trees keep key-value pairs sorted by key, which allows efficient keyvalue lookups and range queries. But that’s where the similarity ends: The log-structured indexes we saw earlier break the database down into *variable-size
segments*, whereas B-trees break the database down into *fixed-size blocks* or *pages*.

## Column-oriented storage

-  In most OLTP databases, storage is laid out in a row-oriented fashion: all the values from one row of a table are stored next to each other. On contrast, for most analytics, it stores all the values from each *column* together instead.
   -  column compression
   -  bitmap encoding
   -  sort order in column storage
   -  aggregation: data cubes and materialized views
