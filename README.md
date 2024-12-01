# Replication
## What is replication?
* Replication means keeping copy of data on multiple machines/nodes that are connected via network.
## Why replication?
* To keep data geograpically close to users.
    * Reduce latency.
* To allow the system to continue working even if any machine/node goes down/failes.
    * Fault tolerent.
* To scale out the number of machine that can serve read queries.
    * Increase read throughput.
## What are the popular algorithms for replicating changes?
* Single leader replication.
* Multi leader replication.
* Leaderless replication.
## How do we ensure all the data ends up on all the replicas?
![image](https://github.com/user-attachments/assets/4dcc1aa0-0104-4acc-9043-40c6a277071c)
* Most common solution is **_Master-Slave / Leader-Follower / Active-Passive_** replication.
    * One of the replica is designated as leader.
        * When client wants to write to the database, they must send there request to leader.
        * Leader saves the new data in its local and notifies the followers as part of _replication log or change stream_.
    * Other replicas are designated as followers.
        * Followeres consumes the replication log or change stream and updates there local copy.
    * When client wants to read data client can read from leader or follower.
## How data are synced to replicas, Sync vs Async replication?
![image](https://github.com/user-attachments/assets/15098199-c6c3-4c3d-8942-b5e0e1afe588)
* Above figure illustrates the flow..
  * Client sends write request to leader.
  * Leader writes the data to local store and then sends the replication log or change data stream to followers.
  * The replication to _follower 1_ is sync.
    * Leader waits unitl follower 1 confirms that it received and written the change data stream.
    * And then notifies to client write operation is success.
  * The replication to _follower 2_ is async.
    * Leader sends the message but doesn't wait for the response from the follower 2.
## What if sync follower is not responding or failed?
* If the synchronous follower becomes unavailable or slow, one of the asynchronous followers is made synchronous.
* This guarantees that you have an up-to-date copy of the data on at least two nodes:
* The leader and one synchronous follower.
* This configuration is sometimes also called **_semi-synchronous_**
## How to add new follower?
* Take a consistent snapshot of leader dataset up to some _log sequence number_.
* Apply the snapshot to new follower node.
* Follower connects to leader and requests all the changes after log sequence number.
* After processing backlog of data change stream follower is ready to server.
## How to handle failed replicas?
## What is eventual consistency?
## Problems with replication lags?
## Read-your-writes guarentee?
## Monotonic read guarentee?


