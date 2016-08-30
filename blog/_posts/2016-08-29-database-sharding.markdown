---
layout: post
title:  "Database Sharding Notes"
date:   2016-08-29
categories: notes
---
Sharding is the process of partitioning a database horizontally; that is, distributing database data across multiple nodes.

The benefits of sharding:
- Ability to support storing large data sets.
- Table sizes are smaller, providing faster access.
- Database request loads are spread out across multiple servers, as opposed to one.

The downsides:
- Additional complexity.
- Limitations as stated by the CAP theorem apply.

## Sharding Strategies
If the number of shards we maintain will be constant, we can group data together based on some common value or property, which is assigned to a shard. One example of this is given some business logic where we store data grouped by countries, one shard can store data for country A while another shard can store data for country B.

Another strategy is to calculate a hash for each piece of data we plan to insert and use that hash to determine which shard to write to; for example, a numeric hash based on the table name and primary key H could be mapped to a shard using the following function:  
~~~
H % n
~~~
where n is the number of available shards.

### Consistent Hashing
For a variable number of shards, consistent hashing can be utilized. A visual description of consistent hashing can be drawn with a circle; storage buckets are distributed randomly across the same circle edge, and each object is mapped to a point on the edge of a circle, in which points between two buckets are mapped to the latter bucket. When a bucket is added, the data before the new bucket and the previous bucket are remapped into the new bucket; vice versa, when a bucket is removed, the data in that bucket is redistributed to the other remaining buckets, with the existing data in the other buckets remaining untouched.

One example of this is storage buckets being assigned hash value intervals, where the interval boundaries are determined by calculating the bucket identifier hash. The calculated hash values of the data are then used to determine the appropriate bucket.

This results in only K/n data remaps, as opposed to K remaps following a H % n function.

## Resources
http://stackoverflow.com/questions/992988/what-is-sharding-and-why-is-it-important
https://en.wikipedia.org/wiki/Consistent_hashing
https://www.interviewbit.com/problems/sharding-a-database/
http://instagram-engineering.tumblr.com/post/10853187575/sharding-ids-at-instagram
