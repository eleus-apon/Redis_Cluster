# Redis_Cluster
![1200px-Redis_Logo svg](https://user-images.githubusercontent.com/35254833/90369794-2d682800-e08e-11ea-8b25-893c78c409ac.png)

# REDIS?
First of all Redis stands for “Remote Dictionary Server” which can run on locally or in a cloud platform. Redis is a key value store. Which means it can store data as key value pairs.
Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams.

- In-memory store: Redis keeps the data in the cache and it does not write to the disk. This makes reading/writing data very fast. (However, Redis has an option to write data to the disk)

## Special Features
- No SQL database — there are no structures like rows,columns, tables etc and it doesn’t uses commands like UPDATE, SELECT, INSERT, -DELETE etc. Instead of of this Redis uses data structures to store data. Such as Strings, Lists, Sort Sets, Hashes and Sets.
- The interaction with data is by using commands.
- Redis is an in-memory database — Redis keeps data in the in the cache/memory and it does not write to the disk making interaction with data more quicker.

## Redis as a Database
![1_yu83a4zDVMC42v5YJN6GTw](https://user-images.githubusercontent.com/35254833/90369641-f5f97b80-e08d-11ea-8072-d64c9a3e565a.png)
Getting data from a disk can be time-consuming. In order to increase the performance, we can put the requests that need to be serviced very fast into the Redis memory and service from there while keeping the rest of the data in the main database.

## Master-Slave Replication of Redis
![1_m-qEuChkmvJmsQ3oRf1vug](https://user-images.githubusercontent.com/35254833/90369774-23dec000-e08e-11ea-9683-e37635e7faca.jpeg)
Redis can replicate data to any number of slaves. That is, it lets the slaves have the exact copy of their master. This greatly helps in performance optimization.

# Setting up Redis on Ubuntu
## Installing & Configuring Redis
Updating local apt package cache and install Redis by typing:
```
sudo apt update
sudo apt install redis-server
```
This will download and install Redis and its dependencies. Following this, there is one important configuration change to make in the Redis configuration file, which was generated automatically during the installation.
To do so, open the below file.
```
sudo nano /etc/redis/redis.conf
```
Inside the file, find the `supervised` directive. This directive allows you to declare an init system to manage Redis as a service, providing you with more control over its operation. The `supervised` directive is set to `no` by default. Since you are running Ubuntu, which uses the systemd init system, change this to `systemd`:
![Capture](https://user-images.githubusercontent.com/35254833/90371630-09f2ac80-e091-11ea-930e-9549ceeb142c.PNG)

Then restart the Redis service.
```
sudo service redis-server restart
```
or to start the server: `sudo service redis-server start`

To check the redis server version: `redis-server -v` 
![Capture_rv](https://user-images.githubusercontent.com/35254833/90972937-2a15e600-e53f-11ea-8b0c-12792a92fb88.PNG)

and to confrim if the server is running or not: `redis-cli ping`. It should reply with `PONG`
![Capture_rornot](https://user-images.githubusercontent.com/35254833/90972946-39952f00-e53f-11ea-8c38-13ba82121ca3.PNG)
This output confirms that the server connection is still alive. 

Next, check that if we are able to set keys by running:
![Capture_set_test](https://user-images.githubusercontent.com/35254833/90973288-8595a300-e542-11ea-8c78-cd2b2d852c1d.PNG)

Now It is confirmed that we can fetch the value, exit the Redis prompt to get back to the shell. `exit`.

# What is Redis-Cluster?
![1_YlZIesKl-3rvAr6KLEoZiQ](https://user-images.githubusercontent.com/35254833/90973839-9399f280-e547-11ea-815a-0e7bbca2f8e4.png)

A Redis cluster is simply a data sharding strategy. It automatically partitions data across multiple Redis nodes. It is an advanced version of Redis that achieves distributed storage and prevents a single point of failure.
In Summary, a Redis cluster is,
- Horizontally scalable: We can continue to add more nodes as the capacity requirement increases.
- Auto data sharding: can automatically partition and split data among the nodes.
- Fault tolerant: even though we lose a node, we can still continue to operate without losing any data.
- Decentralized cluster management: no single node acts as an orchestrator of the entire cluster, every node participates in the cluster configuration (via gossip protocol)

## Sharding
Sharding is a database architecture pattern related to horizontal partitioning — the practice of separating one table’s rows into multiple different tables, known as partitions. Each partition has the same schema and columns, but also entirely different rows. Likewise, the data held in each is unique and independent of the data held in other partitions.

Redis Cluster does not use consistent hashing, but a different form of sharding where every key is conceptually part of what we call an hash slot.
There are 16384 hash slots in Redis Cluster, and to compute what is the hash slot of a given key, we simply take the CRC16 of the key modulo 16384.
Every node in a Redis Cluster is responsible for a subset of the hash slots, so for example you may have a cluster with 3 nodes, where:
- Node A contains hash slots from 0 to 5500.
- Node B contains hash slots from 5501 to 11000.
- Node C contains hash slots from 11001 to 16383.


# Redis Cluster Goals
Redis Cluster is a distributed implementation of Redis with the following goals, in order of importance in the design:
- High performance and linear scalability up to 1000 nodes. There are no proxies, asynchronous replication is used, and no merge operations are performed on values.

- An acceptable degree of write safety: the system tries (in a best-effort way) to retain all the writes originating from clients connected with the majority of the master nodes. Usually, there are small windows where acknowledged writes can be lost. Windows to lose acknowledged writes are larger when clients are in a minority partition.

- Availability: Redis Cluster is able to survive partitions where the majority of the master nodes are reachable and there is at least one reachable slave for every master node that is no longer reachable. Moreover using replicas migration, masters no longer replicated by any slave will receive one from a master which is covered by multiple slaves.

Redis cluster uses a form of sharding where every key is a part of what it called as a hash slot. Hash slots are defined by Redis so the data can be mapped to different nodes in the Redis cluster. The number of slots (16384 ) can be divided and distributed to different nodes.

For example, in a 3 node cluster, one node can hold the slots 0 to 5640, next 5461 to 10922 and the third 10923 to 16383. Input key or a part of it is hashed (run against a hash function) to determine a slot number and hence the node to add the key to.

# Redis cluster topology
The minimal cluster that works as expected requires:
- Minimum 3 Redis master nodes
- Minimum 3 Redis slaves, 1 slave per master (to allow minimal fail-over mechanism)

# Distributed Storage of Redis Cluster
Every key that you save into a Redis cluster is associated with a hash slot. There are 0–16383 slots in a Redis cluster. Thus, a Redis cluster can have a maximum of 16384 master nodes (however the suggested max size of nodes is ~ 1000 nodes). Each master node in a cluster handles a subset of the 16384 hash slots.

The distributed algorithm that Redis Cluster uses to map keys to hash slots is,
```
HASH_SLOT = CRC16(key) mod HASH_SLOTS_NUMBER
```
### CRC stands for Cyclic Redundancy Check.
For example, let’s assume the key space is divided into 10 slots (0–9). Each node will hold a subset of the hash slots.
![1_g_uPH1TnC40Nqiqx4X0ifQ](https://user-images.githubusercontent.com/35254833/90974643-d9a68480-e54e-11ea-8d13-05a495b6857e.png)

A given key “name” is at slot:
```
slot = CRC16(“name”) % 16384
```

# Failure Handling in Redis Cluster
Redis has introduced the Master-Slave concept to increase data availability by preventing the single point of failure. Every master node in a Redis cluster has at least one slave node. When the Master node fails or becomes unreachable, the cluster will automatically choose its slave node/one of the slave nodes and make that one the new Master. Therefore, failure in one node will not stop the entire system from working.

# How to detect failures?
Every node has a unique ID in the cluster. This ID is used to identify each and every node across the entire cluster using the gossip protocol.
So, a node keeps the following information within itself;
- node ID, IP, and port
- a set of flags
- what is the Master of the node if it is flagged as “slave”
- last time a node was pinged
- last time the pong was received

N.B. When you ping a Redis node, if it is working properly, it will reply with a pong.

![1_1kS33aRPwETLyYeFCw7rIg](https://user-images.githubusercontent.com/35254833/90974751-b8926380-e54f-11ea-95e2-cec2f7021139.png)
Nodes in a cluster always keep gossiping with each other, enabling them to auto-discover other nodes.
e.g. If A knows B, and B knows C, eventually B will send gossip messages to A about C. Then A will register C as part of the network and will try to connect with C.
There are two flags that are used for failure detection namely PFAIL and FAIL.
- PFAIL (Possible FAILure): a non-acknowledged failure type.
- FAIL: This tells that a node is failing, and it was confirmed by the majority of the masters within a fixed amount of time.
![1_M8LA8LamTirqF4DIna0sSA](https://user-images.githubusercontent.com/35254833/90974792-30608e00-e550-11ea-903a-2ba3daef3dba.png)
The node-to-node communication follows the binary protocol (Cluster Bus Protocol) which is optimized for bandwidth and speed, but the node-client communication follows the ASCII protocol.

# Setting Up Reddis Cluster
https://blog.usejournal.com/first-step-to-redis-cluster-7712e1c31847


































Source:
https://blog.usejournal.com/first-step-to-redis-cluster-7712e1c31847
https://medium.com/@iamvishalkhare/create-a-redis-cluster-faa89c5a6bb4
https://medium.com/@inthujan/introduction-to-redis-redis-cluster-6c7760c8ebbc
https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-18-04


