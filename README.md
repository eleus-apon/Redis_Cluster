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








































Source:
https://blog.usejournal.com/first-step-to-redis-cluster-7712e1c31847
https://medium.com/@iamvishalkhare/create-a-redis-cluster-faa89c5a6bb4
https://medium.com/@inthujan/introduction-to-redis-redis-cluster-6c7760c8ebbc


