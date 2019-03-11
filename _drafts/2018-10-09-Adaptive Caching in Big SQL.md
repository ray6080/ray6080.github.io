---
layout: writeup
title: Adaptive Caching in Big SQL
full_title: "Adaptive Caching in Big SQL using the HDFS Cache"
author: Avrilia Floratou and Nimrod Megiddo, IBM; Navneet Potti, University of Wisconsin-Mandison; Fatma Ozcan and Uday Kale and Jan Schmitz-Hermes, IBM
link: https://dl.acm.org/citation.cfm?id=2987553
publication: SoCC'16
comments: true
categories:
- writeup
---

This paper is done by researchers from IBM jointly with Univ. of Wisconsin-Mandison. It takes use of HDFS cache to optimize query executions in Big SQL, which is a big data analytical system inside IBM, and enable in-memory data to be shared with other Big Data applications. The paper designs novel adaptive caching algorithms for Big SQL tailored to the challenges of such an external cache scenario.

<hr>

## Motivation
Traditional databases take ownership of their data and store it in proprietary formats both on-disk and in dedicated memory pools.
SQL-on-Hadoop systems share data with other applications in the Big Data ecosystem by storing their data in HDFS, using open file formats, but do not provide automatic caching mechanism.
To exploit larger memories, the current generation of Big Data platforms provide external, distributed caching mechanisms such as HDFS caching and Tachyon to cache HDFS data in memory. External caches allow us to retain the performance benefits of avoiding disk I/O, not only in Big SQL but also in other data analytics applications that share the cache with it.

## Challenges
1. Insertions into the external cache are costlier, as they may be asynchronously executed by a separate cache management process, competing for resources with the application that needs the data. Traditional caching algorithms assuming that all data accesses go through the cache, often result in worse performance than simply bypassing the cache and reading the data directly from secondary storage.

2. Since applications can bypass the cache on a cache miss, the decision of what to insert into the cache is as important as what to evict from it.

3. Since a shared cache attempts to ensure a high cache hit rate for various different data processing applications the caching algorithms must necessarily adapt to the workload access patterns.

## Contributions
+ Propose selective, adaptive caching algorithms (Adaptive SLRU-K, Adaptive EXD)
+ Design and implement a caching service in Big SQL using HDFS cache.
+ Proposed algorithms outperform existing static algorithms on diverse workloads.

# Caching Algorithms
The task of maximizing the expected performance of a cache has been modeled in literature as a knapsack problem. In this well-known formulation, it is assumed that caching an object provides certain benefit (future accesses to the object will be hits) and the cache policy has to maximize the total expected benefit from the cache given that the total size of the cached objects cannot exceed the size of the cache.

## References
