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

The range-based sharding method is simple and straightforward. Each shard contains a unique set of data but maintains the same schema as the original database. The application decides the range of the data and writes it to the correct shard.

However, range-based sharding can lead to uneven data distribution, or "database hotspots". Some shards may receive more reads and writes than others if their data is accessed more frequently. This can lead to performance issues, slow queries, and imbalanced workloads. For example, a shard containing products with prices between $0 and $100 may receive more traffic than a shard containing products with prices between $100 and $200.

![Range-Based Sharding](https://assets.digitalocean.com/articles/understanding_sharding/DB_image_3_cropped.png)

## 3. Directory-Based Sharding

Directory-based sharding is a database strategy that utilizes a lookup table to dictate data storage locations. It assigns each key to a specific shard, using the lookup table that contains fixed data location information. This method is adaptable and simplifies the process of adding new shards.

For example, a lookup table has columns for Delivery Zone and Shard ID. The Delivery Zone column is defined as a Shard key. Data from a specific delivery zone is written to the shard ID listed in the lookup table.

Directory-based sharding provides flexible data distribution, efficient query routing, and dynamic scalability. It uses a central directory for managing data-to-shard mapping, optimizing query performance, and enabling efficient load balancing. The system can scale dynamically by modifying the number of shards, without affecting the application logic, thus easily adapting to changing needs and workloads.

However, Directory-based sharding can potentially slow down operations due to lookup table access for each query or write. It can also create a single point of failure, making the entire database inaccessible if the lookup table fails. Using a distributed lookup table can mitigate this but adds system complexity.

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

- **E-commerce Platforms:** These platforms deal with large volumes of product data, customer data, and order data. Sharding helps distribute the load across multiple servers and improve performance.

- **Social Media Platforms:** With billions of users and large amounts of user-generated content, sharding helps these platforms manage data effectively.

- **Gaming Platforms:** Online multiplayer games often need to manage real-time data for millions of players. Sharding can help distribute the load and improve performance.

## Lets take a look at scenario

**Scenario:** Envision that you're architecting a user account management system for an application. To address scalability and performance, you've chosen to distribute the user data across multiple database shards. You've selected **directory-based sharding**, utilizing the `country_code` as the key attribute for sharding. The country_code is a three-letter code representing each country. A lookup table is used to store the mapping of each `country_code` to its corresponding `shard_id`.

## Determine the number of shards
Assuming the application is used in 3 countries, we'll use 3 shards.

## Lookup table for mapping country_code to shard_id
- We'll create a lookup table to store the mapping of country_code to shard_id. The table will have two columns: `country_code` and `shard_id`.
- The `country_code` column will store the three-letter code for each country.
- `country_code` example: South Korea (KOR), Thailand (THA), Malaysia (MYS)

| country_code | shard_id |
| ------------ | -------- |
| KOR          | 1        |
| THA          | 2        |
| MYS          | 3        |

## Handling the queries
- We'll demonstrate how to user goes through the process of signing up a new user and how to choose the correct shard based on the country_code of the user.
- We will also show how choose the correct shard to fetch user data from the database while signing in the user.

## Basic Implementation of Database Sharding in Ruby on Rails framework(6.1+)

1. First, let’s set up our Rails application with multiple databases. In `config/database.yml`, we’ll define our shards:


    ```yaml
    production:
      primary:
        database: myapp_primary
        adapter: postgresql
      primary_replica:
        database: myapp_primary_replica
        adapter: postgresql
        replica: true
      primary_kor_shard:
        database: primary_kor_shard
        adapter: postgresql
        migrations_paths: db/kor_migrate_shards
      primary_kor_shard_replica:
        database: primary_kor_shard_replica
        adapter: postgresql
        replica: true
      primary_mys_shard:
        database: primary_mys_shard
        adapter: postgresql
        migrations_paths: db/mys_migrate_shards
      primary_mys_shard_replica:
        database: primary_mys_shard_replica
        adapter: postgresql
        replica: true
      ...
    ```

2. Next, we'll make changes to the `ApplicationRecord` class to connect to the primary and replica databases, and to the `Shard` model to connect to the shard databases. We'll also define a method to choose the correct shard based on the `country_code`.


    ```ruby
    class ApplicationRecord < ActiveRecord::Base
      primary_abstract_class

      connects_to database: {
        primary: { writing: :primary, reading: :primary_replica },
      }
    end

    class Shard < ApplicationRecord
      validates :country_code, presence: true, uniqueness: true

      connects_to shards: {
        kor_shard: { writing: :primary_kor_shard, reading: :primary_kor_shard_replica },
        mys_shard: { writing: :primary_mys_shard, reading: :primary_mys_shard_replica },
        tha_shard: { writing: :primary_tha_shard, reading: :primary_tha_shard_replica }
      }

      def self.shard_for(country_code)
        case country_code
        when 'KOR'
          :kor_shard
        when 'MYS'
          :mys_shard
        when 'THA'
          :tha_shard
        else
          raise "Invalid country_code"
        end
      end
    end


    class User < ApplicationRecord
      # ...

      belongs_to :shard
    end
    ```

3. When a new user signs up, we need to choose the correct shard based on the user’s `country_code`. We can do this by using the `connected_to` method to connect to the correct shard and then create the user.


    ```ruby
    class UsersController < ApplicationController
      def create
        ApplicationRecord.connected_to(role: :writing, shard: Shard.shard_for(user_params[:country_code])) do
          @user = User.new(user_params)
          if @user.save
            redirect_to @user, notice: 'User was successfully created.'
          else
            render json: @user.errors, status: :unprocessable_entity
          end
        end
      end
    end
    ```

4. Choosing the Correct Shard for User Sign-In - Similarly, when a user signs in, we need to choose the correct shard to fetch the user data from the database.


    ```ruby
    class SessionsController < ApplicationController
      def create
        ApplicationRecord.connected_to(role: :reading, shard: Shard.shard_for(params[:country_code])) do
          @user = User.find_by(email: params[:email])
          if @user && @user.authenticate(params[:password])
            session[:user_id] = @user.id
            redirect_to @user
          else
            render json: { error: 'Invalid email or password' }, status: :unauthorized
          end
        end
      end
    end
    ```

Here is a basic implementation of database sharding in Rails. This example demonstrates how to distribute user data across multiple shards. This is a simplified example, and in a real-world scenario, you would need to consider additional factors such as data consistency, replication, and failover.


# Conclusion

Sharding, a strategy for horizontal database scaling, has both benefits and challenges. It adds complexity and potential failure points to your application. The effort and resources needed for a sharded architecture may outweigh its advantages for some. This article helps you understand sharding better to decide if it suits your application's needs.