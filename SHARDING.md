*Title: Database Sharding: What and When to Use it?

# Introduction

- Your Application is growing, increasing the number of active users, more features, and generates more data every day. Now your database is not able to handle the load or bottlenecking. Then Databse Sharding is the solution to your problem.
- But many of us are not clear understanding of What is it? And Speciallly when to use it?
- In this Article, We will cover the concept of Database Sharding, its architecture, advantages, disadvantages, and real-life scenarios and use cases.

# What is Sharding?

- The term "Sharding" is comes from the word "Shard" whaich means a small part of a whole.
- Database Sharding is a technique of breaking down a large database into smaller databases.
- Sharding is also known as Horizontal scaling. It is databse architecture design pattern.
- Sharding is seaparating the one table's rows into multiple tables known as "shards".
- Each shard has same schema and columns, but different rows. Likewise data held in each shard is unique and non-overlapping.
- Sharding overcomes the limitations of a single database by splitting the data into smaller chunks and storing them accross multiple database servers.

# Vertical partitioning vs Horizontal partitioning

## Vertical partitioning

- Verticle partitioning is a technique of dividing a table based on columns.
- It is Useful when the table has a large number of columns and some columns are not frequently used.
- Verticle partitioning improve query performance by reducing I/O and allowing for more efficient indexing of relevant columns.
- Vertical partitioning may require joins to retrieve data from multiple partitions.
- Example: A table with 100 columns, where 20 columns are frequently accessed and 80 columns are rarely accessed. In this case, we can split the table into two tables, one with 20 columns and another with 80 columns.

## Horizontal partitioning

- Horizontal partitioning is a technique of dividing a table based on rows.
- It is useful when the dealing with tables with large number of rows, And data can be divided based on some criteria.
- Horizontal partitioning improve query performance by reducing the number of rows to be scanned for a specific queries and allows for easier data management.
- Horizontal partitioning, joins between partitions are typically not required. Because the contain disjoint sets of rows.
- Example: A table with 1 billion rows, where 300 million rows are accessed frequently and 700 million rows are rarely accessed. In this case, we can split the table into two tables, one with 300 million rows and another with 700 million rows.

# Sharding Architectures

After deciding to shard your database, the subsequent step is determining the method to implement it. The process of running queries or distributing incoming data to sharded tables or databases is critical. It's essential to ensure data goes to the appropriate shard to prevent data loss or slow queries.

In the following section, we will discuss several prevalent sharding architectures. Each architecture employs a unique process to distribute data across shards.

1. Key-Based Sharding:
   Key-based sharding is also known as hash-based sharding. Uses a hash function to distribute data across shards. A specific data value, such as a user ID, or IP address, or ZIP code, or Region, is used as input to the hash function. The output a Shard ID, And Shard ID determines where the data will be stored.


    The data value used in hash function is called as Shard key. Shard key should be a static column, similar to a primary key, to ensure consistent data distribution and efficient update operations.

    However, key-based sharding can complicate the process of adding or removing databse servers. As servers change, the data must be remapped and migrated. This can be a expensive and time-consuming process. Potentially causing system downtime.

    Despite these challenges, Key-based sharding is a popular choice for many applications. Because it evenly distributes data across shards. algorithmically distributes data minimizing the risk of database hotspots. Where a single shard contains significantly more data than others. Key-based sharding helps maintain balanced workloads across shards.

2. Range-Based Sharding:
    Range-based sharding is a technique that divides data into shards based on a specific range of values. For example, in a Products database, products could be sharded based on their price ranges. Products with prices between $0 and $100 could be stored in one shard, while products with prices between $100 and $200 could be stored in another shard.


    The primary advantage of range-based sharding is that its simplicity. Each shard holds a unique data set but shares the same schema with original database. The application determines the data's range and writes it to the appropriate shard. This process is straightforward and easy to understand.

    However, range-based sharding can lead to uneven data distribution, or "databse hotspots". Some shards may receive more reads and writes than others if their data is accessed more frequently. This can lead to performance issues and slow queries and imbalanced workloads. Example: A shard containing products with prices between $0 and $100 may receive more traffic than a shard containing products with prices between $100 and $200.

3. Directory-Based Sharding:
    Directory-based sharding is a technique is database distribution method that uses a lookup table to determine where data should be stored. A lookup table that holds a fixed collection of information indicating where data is located.
    Directory-based sharding assigns each key to a specific shard. This technique is flexible and allows for easy addition of new shards.


    Example: Lookup table has columns Delivery Zone and Shard ID. Delivery Zone column defined as a Shard key. Data from a specific delivery zone is written to the shard ID listed in the lookup table. This is similar to ranged-based sharding, but instaed of determining the shard based on a range of values, the shard is determined by a lookup table.

    Directory-based sharding is a database distribution technique that offers flexible data distribution, efficient query routing, and dynamic scalability. It uses a central directory or lookup table to manage and update the mapping of data to shard locations, allowing for efficient load balancing and adaptation to changing data patterns. This central directory also optimizes query performance by directing queries to the specific shard containing the relevant data. Furthermore, the system can dynamically scale by adding or removing shards as needed, without requiring changes to the application logic, as the central directory handles the mapping and distribution of data, making it easier to adapt to changing requirements and workloads.

    However, Directory-based sharding can slow down the process of each query or write operations due to the access of lookup table for each query or write operation. This can lead to performance issues and slow queries.Directory-based sharding can lead to a single point of failure. If the lookup table fails, the entire database can become inaccessible. This can be mitigated by using a distributed lookup table, but this adds complexity to the system.

# Benefits of Sharding
- Database Sharding is adding more servers to your database to spread the load and allow more traffic and faster processing. This is exactly contrast to the traditional method of scaling up, which is adding more resources to a single server. usually, this is done by adding more RAM or CPU to the server.
- By distributing your database into multiple shards, both read and write operations capacity can be increased. This is true as long as the read and write tasks are done within one shard at a time.
- Similarly, by distributing your database into multiple shards, you can also increase the storage capacity of your database. Nearly infinite storage capacity can be achieved.
- Sharding provides you high availability. Because shards is replicated across multiple servers. If one shard goes down, the other shards can still be accessed.This is in contrast to the traditional method of scaling up, where if the server goes down, the whole database goes down.


# Drawbacks of Sharding
- While sharding can provide many benefits, it also comes with its own set of challenges and drawbacks. It is important to consider these factors when deciding whether to implement sharding in your database architecture.
- Sharding is a complex process of managing a database structure. If not done right, it can cause a high risk of losing data or corrupted database tables.
- Every sharded database needs a unique service or machine to direct queries to the right shard, adding latency to each operation. If data for a query is split across multiple shards, the router must query each, merge results, complicating operations and slowing response times.
- In contrast to a single unsharded database that only requires server maintenance, a sharded database demands upkeep of both the shards and additional service nodes. If replication is used, data updates must be reflected across all nodes. Thus, sharded databases, being more complex, necessitate increased administration.
- Sharding necessitates more machines and computing power than a single database server, allowing for growth beyond a single machine's limits. However, each extra shard increases costs, and an improperly optimized distributed database system can be notably expensive.

# Should I shard my database?
- Sharding is a powerful tool for scaling databases, but it is not a one-size-fits-all solution. Before deciding to shard your database, consider the following factors:
- Database size: If your database is relatively small and can be managed by a single server, sharding may not be necessary. Sharding is typically used for large databases that have outgrown the capacity of a single server.
- Traffic patterns: If your database experiences uneven traffic patterns, with some data being accessed much more frequently than others, sharding may be beneficial. Sharding can help distribute the load across multiple servers and improve performance for frequently accessed data.
- Growth projections: If your database is expected to grow significantly in the future, sharding may be a good option. Sharding allows you to scale your database horizontally by adding more servers, which can accommodate future growth.
- Complexity: Sharding adds complexity to your database architecture, and it requires careful planning and maintenance. If your database can be scaled using other methods, such as vertical scaling or caching, you may want to consider those options before sharding.
- Cost: Sharding can be expensive, as it requires additional hardware and infrastructure to support multiple servers. Consider the cost of sharding compared to the benefits it provides in terms of performance and scalability.

# Real-life Scenarios and use cases
- E-commerce platforms: E-commerce platforms deals with large volumes of product data, customer data, and order data. They use sharding to distribute the load across multiple servers and improve performance for frequently accessed data, such as product listings, customer profiles, and order information.
- Social media platforms: Social media platforms have billions of users and generate large amounts of user-generated content. Sharding helps these platforms manage this data effectively. For example, user data might be sharded based on geographic location.
- Gaming platforms: Online multiplayer games often need to manage real-time data for millions of players. Sharding can help distribute the load and improve performance.

# Conclusion
  Sharding can serve as an effective strategy for those aiming to horizontally scale their database. However, it also introduces significant complexity and increases the potential areas of failure in your application. While sharding might be essential for some, the effort and resources required to establish and manage a sharded architecture might surpass the advantages for others.

  This conceptual article should provide you with a better comprehension of the advantages and disadvantages of sharding. With this knowledge, you can make a more educated decision on whether a sharded database architecture suits your application's needs.