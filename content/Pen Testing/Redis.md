---
dg-publish: true
---
Databases are a collection of organized information that can be easily accessed, managed and updated. In most environments, database systems are very important because they communicate information related to your sales transactions, product inventory, customer profiles and marketing activities.

There are different types of databases and one among them is Redis, which is an 'in-memory' database. In- memory databases are the ones that rely essentially on the primary memory for data storage (meaning that the database is managed in the RAM of the system); in contrast to databases that store data on the disk or SSDs. As the primary memory is significantly faster than the secondary memory, the data retrieval time in the case of 'in-memory' databases is very small, thus offering very efficient & minimal response times.

In-memory databases like Redis are typically used to cache data that is frequently requested for quick retrieval. For example, if there is a website that returns some prices on the front page of the site. The website may be written to first check if the needed prices are in Redis, and if not, then check the traditional database (like MySQL or MongoDB). When the value is loaded from the database, it is then stored in Redis for some shorter period of time (seconds or minutes or hours), to handle any similar requests that arrive during that timeframe. For a site with lots of traffic, this configuration allows for much faster retrieval for the majority of requests, while still having stable long term storage in the main database.

This lab focuses on enumerating a Redis server remotely and then dumping its database in order to retrieve the flag. In this process, we learn about the usage of the redis-cli command line utility which helps interact with the Redis service. We also learn about some basic redis-cli commands, which are used to interact with the Redis server and the key-value database.
Let us now dive straight into the lab.
