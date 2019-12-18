# Replication

*Replication* means keeping a copy of the same data on multiple machines that are connected via a network. There are several
reasons why you might want to replicate data:
-  To keep data geographically close to your users (and thus reduce latency)
-  To allow the system to continue working even if some of its parts have failed (and thus increase availability)
-  To scale out the number of machines that can serve read queries (and thus increase read throughput)

All of the difficulty in replication lies in handling *changes* to replicated data, and there are three popular algorithms for replicating changes between nodes: *single-leader*, *multi-leader*, and *leaderless* replication.

## Leaders and followers
![leaders and followers](https://github.com/ustcljb/designing-data-intensive-applications/blob/master/chap5/leader_follower.png)

## Synchronous versus asynchronous replication
![synchronous versus asynchronous replication](https://github.com/ustcljb/designing-data-intensive-applications/blob/master/chap5/sync_async.png)

## Setting up new follower
Simply copying data files from one node to another is typically not sufficient: clients are constantly writing to the database and the data is always in flux. Common steps include:
-  Take a consistent snapshot of the leader’s database at some point in time.
-  Copy the snapshot to the new follower node.
-  The follower connects to the leader and requests all the data changes that have happened since the snapshot was taken. This requires that the snapshot is associated with an exact position in the leader’s replication log.
-  When the follower has processed the backlog of data changes since the snapshot, we say it has *caught up*.

## Handling node outrages
-  Follower failure: Catch-up recovery
-  Leader failure: Failover (promote one of the followers to be the new leader). An automatic failover process usually consists of the following steps:
   -  Determining that the leader has failed
   -  Choosing a new leader
   -  Reconfiguring the system to use the new leader
## Implementation of replication logs
-  Statement-based replication
-  Write-ahead log (WAL) shipping
-  Logical (row-based) log replication
-  Trigger-based replication

## Multi-leader replication
