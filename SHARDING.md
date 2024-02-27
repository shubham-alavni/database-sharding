# All About Database Sharding

## Introduction

As your application grows, the number of active users increases, more features are added, and more data is generated every day. If your database is unable to handle the load or is bottlenecking, Database Sharding could be the solution to your problem. This article aims to provide a clear understanding of Database Sharding, its architecture, advantages, disadvantages, and real-life scenarios and use cases.

## Understanding Sharding

The term "Sharding" comes from the word "Shard" which means a small part of a whole. Database Sharding is a technique of breaking down a large database into smaller databases, also known as Horizontal scaling. It is a database architecture design pattern where one table's rows are separated into multiple tables known as "shards". Each shard has the same schema and columns, but different rows. The data held in each shard is unique and non-overlapping. Sharding overcomes the limitations of a single database by splitting the data into smaller chunks and storing them across multiple database servers.

## Vertical Partitioning vs Horizontal Partitioning

### Vertical Partitioning

Vertical partitioning is a technique of dividing a table based on columns. It is useful when the table has a large number of columns and some columns are not frequently used. Vertical partitioning improves query performance by reducing I/O and allowing for more efficient indexing of relevant columns. However, it may require joins to retrieve data from multiple partitions.

**Example:** A table with 100 columns, where 20 columns are frequently accessed and 80 columns are rarely accessed. In this case, we can split the table into two tables, one with 20 columns and another with 80 columns.

### Horizontal Partitioning

Horizontal partitioning is a technique of dividing a table based on rows. It is useful when dealing with tables with a large number of rows, and data can be divided based on some criteria. Horizontal partitioning improves query performance by reducing the number of rows to be scanned for specific queries and allows for easier data management. Unlike vertical partitioning, joins between partitions are typically not required because they contain disjoint sets of rows.

**Example:** A table with 1 billion rows, where 300 million rows are accessed frequently and 700 million rows are rarely accessed. In this case, we can split the table into two tables, one with 300 million rows and another with 700 million rows.

![Vertical vs Horizontal Partitioning](https://assets.digitalocean.com/articles/understanding_sharding/DB_image_1_cropped.png)

# Sharding Architectures

After deciding to shard your database, the subsequent step is determining the method to implement it. The process of running queries or distributing incoming data to sharded tables or databases is critical. It's essential to ensure data goes to the appropriate shard to prevent data loss or slow queries.

In the following section, we will discuss several prevalent sharding architectures. Each architecture employs a unique process to distribute data across shards.

## 1. Key-Based Sharding

Key-based sharding, also known as hash-based sharding, uses a hash function to distribute data across shards. A specific data value, such as a user ID, IP address, ZIP code, or Region, is used as input to the hash function. The output is a Shard ID, which determines where the data will be stored.

The data value used in the hash function is called the Shard key. The Shard key should be a static column, similar to a primary key, to ensure consistent data distribution and efficient update operations.

However, key-based sharding can complicate the process of adding or removing database servers. As servers change, the data must be remapped and migrated. This can be an expensive and time-consuming process, potentially causing system downtime.

Despite these challenges, Key-based sharding is a popular choice for many applications. It evenly distributes data across shards and algorithmically distributes data, minimizing the risk of database hotspots, where a single shard contains significantly more data than others. Key-based sharding helps maintain balanced workloads across shards.

![Key-Based Sharding](https://assets.digitalocean.com/articles/understanding_sharding/DB_image_2_cropped.png)

## 2. Range-Based Sharding

Range-based sharding is a technique that divides data into shards based on a specific range of values. For example, in a Products database, products could be sharded based on their price ranges. Products with prices between $0 and $100 could be stored in one shard, while products with prices between $100 and $200 could be stored in another shard.

The primary advantage of range-based sharding is its simplicity. Each shard holds a unique data set but shares the same schema with the original database. The application determines the data's range and writes it to the appropriate shard. This process is straightforward and easy to understand.

However, range-based sharding can lead to uneven data distribution, or "database hotspots". Some shards may receive more reads and writes than others if their data is accessed more frequently. This can lead to performance issues, slow queries, and imbalanced workloads. For example, a shard containing products with prices between $0 and $100 may receive more traffic than a shard containing products with prices between $100 and $200.

![Range-Based Sharding](https://assets.digitalocean.com/articles/understanding_sharding/DB_image_3_cropped.png)

## 3. Directory-Based Sharding

Directory-based sharding is a database distribution method that uses a lookup table to determine where data should be stored. A lookup table holds a fixed collection of information indicating where data is located. Directory-based sharding assigns each key to a specific shard. This technique is flexible and allows for easy addition of new shards.

For example, a lookup table has columns for Delivery Zone and Shard ID. The Delivery Zone column is defined as a Shard key. Data from a specific delivery zone is written to the shard ID listed in the lookup table. This is similar to range-based sharding, but instead of determining the shard based on a range of values, the shard is determined by a lookup table.

Directory-based sharding offers flexible data distribution, efficient query routing, and dynamic scalability. It uses a central directory or lookup table to manage and update the mapping of data to shard locations, allowing for efficient load balancing and adaptation to changing data patterns. This central directory also optimizes query performance by directing queries to the specific shard containing the relevant data. Furthermore, the system can dynamically scale by adding or removing shards as needed, without requiring changes to the application logic, as the central directory handles the mapping and distribution of data, making it easier to adapt to changing requirements and workloads.

However, Directory-based sharding can slow down the process of each query or write operation due to the access of the lookup table for each query or write operation. This can lead to performance issues and slow queries. Directory-based sharding can lead to a single point of failure. If the lookup table fails, the entire database can become inaccessible. This can be mitigated by using a distributed lookup table, but this adds complexity to the system.

![Directory-Based Sharding](https://assets.digitalocean.com/articles/understanding_sharding/DB_image_4_cropped.png)

# Database Sharding: Benefits, Drawbacks, and Use Cases

## Benefits of Sharding

Database Sharding offers several advantages:

- **Scalability:** Sharding allows you to add more servers to your database, spreading the load and enabling more traffic and faster processing. This contrasts with the traditional method of scaling up, which involves adding more resources to a single server.

- **Increased Operation Capacity:** By distributing your database into multiple shards, you can increase both read and write operation capacity, as long as the tasks are performed within one shard at a time.

- **Expanded Storage Capacity:** Sharding also increases the storage capacity of your database, potentially achieving nearly infinite storage capacity.

- **High Availability:** Sharding provides high availability. If one shard goes down, the other shards can still be accessed, preventing a total system shutdown.

## Drawbacks of Sharding

Despite its benefits, sharding also comes with challenges and drawbacks:

- **Complexity:** Sharding involves managing a complex database structure. If not done correctly, it can lead to a high risk of data loss or corrupted database tables.

- **Latency:** Every sharded database needs a unique service or machine to direct queries to the right shard, adding latency to each operation.

- **Increased Maintenance:** A sharded database requires upkeep of both the shards and additional service nodes. If replication is used, data updates must be reflected across all nodes.

- **Cost:** Sharding requires more machines and computing power than a single database server. Each extra shard increases costs, and an improperly optimized distributed database system can be notably expensive.

## Should I Shard My Database?

Sharding is a powerful tool for scaling databases, but it's not a one-size-fits-all solution. Consider the following factors before deciding to shard your database:

- **Database Size:** Sharding is typically used for large databases that have outgrown the capacity of a single server.

- **Traffic Patterns:** If your database experiences uneven traffic patterns, sharding may be beneficial.

- **Growth Projections:** If your database is expected to scale significantly in the future, sharding may be a good option.

- **Complexity:** Sharding adds complexity to your database architecture and requires careful planning and maintenance.

- **Cost:** Sharding can be expensive, as it requires additional hardware resources and infrastructure to support multiple servers.

## Real-Life Scenarios and Use Cases

Sharding is commonly used in the following scenarios:

- **E-commerce Platforms:** These platforms deal with large volumes of product data, customer data, and order data. Sharding helps distribute the load across multiple servers and improve performance. Eg: Amazon, Alibaba, etc.

- **Social Media Platforms:** With billions of users and large amounts of user-generated content, sharding helps these platforms manage data effectively.
- Eg: Meta (Facebook), Twitter, Instagram, etc.

- **Gaming Platforms:** Online multiplayer games often need to manage real-time data for millions of players. Sharding can help distribute the load and improve performance.

# Conclusion

Sharding can be a powerful strategy for those looking to horizontally scale their database. However, it's not without its challenges. It introduces significant complexity and increases the potential areas of failure in your application. While sharding might be essential for some, the effort and resources required to establish and manage a sharded architecture might outweigh the advantages for others.

This article aimed to provide a comprehensive understanding of the benefits and drawbacks of sharding. Armed with this knowledge, you can make a more informed decision on whether a sharded database architecture is the right fit for your application's needs.