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

## Contributions