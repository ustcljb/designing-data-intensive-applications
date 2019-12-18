# Batch processing

In the first two parts of this book we talked a lot about *requests* and *queries*, and the corresponding *responses* 
or *results*. These are *online* systems, where we pay more attention to the *response time*.

- Basically we have three types of systems:
   - Servece(online systems)
   - Batch processing systems (The primary performance is usually *throughput*)
   - Stream processing systems (somewhere between)
- Batch processing is an important building block in our quest to build reliable, scalable, and maintainable applications. (MapReduce to Google).
- Philosophy of Unix system:
   - Make each program do one thing well.
   - Expect the output of every program to become the input to another, as yet unknown, program.
   - Design and build software, even operating systems, to be tried early, ideally within weeks.
   - Use tools in preference to unskilled help to lighten a programming task.
- The biggest limitation of Unix tools is that they run only on a single machine—and that’s where tools like Hadoop come in.

## MapReduce and distributed filesystems
- MapReduce is a bit like Unix tools, but distributed across potentially thousands of machines.
- MapReduce is a programming framework with which you can write code to process large datasets in a distributed filesystem like HDFS.
- MapReduce dataflow
![]()
### Reduce-side joins and grouping
- When a MapReduce job is given a set of files as input, it reads the entire content of all of those files; a database would call this operation a *full table scan*.
- Sort-merge joins
![]()
- Handling skew
   - The pattern of “bringing all records with the same key to the same place” breaks down if there is a very large amount of data related to a single key.
   - The *skewed* join method in Pig first runs a sampling job to determine which keys are hot.
   - Hive’s skewed join optimization takes an alternative approach. It requires hot keys to be specified explicitly in the table metadata, and it stores records related to those keys in separate files from the rest.
   
### Map-side joins
- If you can make certain assumptions about your input data, it is possible to make joins faster by using a so-called map-side join.
- This approach uses a cut-down MapReduce job in which there are no reducers and no sorting.
- Broadcast hash joins.
   - The simplest way of performing a map-side join applies in the case where a large dataset is joined with a small dataset.
- Partitioned hash joins
   - If the inputs to the map-side join are partitioned in the same way, then the hash join approach can be applied to each partition independently.
   - Partitioned hash joins are known as *bucketed map joins* in Hive.
- Map-side merge joins
   - Another variant of a map-side join applies if the input datasets are not only partitioned in the same way, but also sorted based on the same key.

## Comparing Hadoop to distributed databases
- When the MapReduce paper was published, it was—in some sense—not at all new. The same idea had been implemented in MPP.
- The biggest difference is that MPP databases focus on parallel execution of analytic SQL queries on a cluster of machines, while the combination of MapReduce and a distributed filesystem provides something much more like a general-purpose operating system that can run arbitrary programs.
- Hadoop opened up the possibility of indiscriminately dumping data into HDFS, and only later figuring out how to process it further.
- On the other hand, not all kinds of processing can be sensibly expressed as SQL queries.
- If you have HDFS and MapReduce, you can build a SQL query execution engine on top of it, and indeed this is what the Hive project did.
- The Hadoop ecosystem includes both random-access OLTP databases such as HBase and MPP-style analytic databases such as
Impala.

## Beyond MapReduce
### Materialization of intermediate state
- The process of writing out the intermediate state to files is called materialization.
- Unix pipes do not fully materialize the intermediate state, but instead stream the output to the input incrementally,
using only a small in-memory buffer.
- In order to fix these problems with MapReduce, several new execution engines for distributed batch computations were developed, the most well known of which are Spark, Tez and Flink.
- Since they explicitly model the flow of data through several processing stages, these systems are known as *dataflow engines*.
- Fault tolerance:
   - These dataflow engines don't write intermediate state to HDFS, so they take a different approach to tolerating faults.
   - Spark uses the resilient distributed dataset (RDD) abstraction for tracking the ancestry of data, while Flink checkpoints operator state, allowing it to resume running an operator that ran into a fault during its execution.
