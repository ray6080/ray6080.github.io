---
layout: post
title: List of Papers
skip_related: true
---

This is a list of recently interested papers.

### Self Driving
Andrew Pavlo

### Distributed Storage; Self Organizing
1. S.Idreos,M.L.Kersten,andS.Manegold.Self-organizing tuple reconstruction in column-stores. In SIGMOD, pages 297–308, 2009.

### Interactive Analysis
Adity
探索式分析：查询预测、prefetch

### System Paper
1. Spanner
2. GFS and its following works
3. MonetDB

### AMPLab有两篇partition相关的论文
1. Fine-grained Partitioning for Aggressive Data Skipping
2. A. Jindal et al.Trojan data layouts: Right shoes for a running elephant.In SOCC, pages 21:1–21:14, New York, NY, USA, 2011.
3. I. Alagiannis, S. Idreos, and A. Ailamaki. H2O: A hands-free adaptive store. In SIGMOD, pages 1103–1114, New York, NY, USA, 2014. ACM.
4. M.Grundetal. Hyrise: A main memory hybrid storage engine. PVLDB, 4(2):105–116, Nov. 2010.
5. J.Zhou and K. Ross. A multi-resolution block storage model for database design. In IDEAS, pages 22–31, July 2003.

### 消息队列系统论文

### Database Cracking
1. S.Idreos,M.L.Kersten,andS.Manegold.Databasecracking.InCIDR, pages 68–78, 2007.
2. F.M.Schuhknecht,A.Jindal,andJ.Dittrich.The uncracked pieces in database cracking. PVLDB, 7(2):97–108, Oct. 2013.

### Streaming Ingestion
1. Data Ingestion for the Connected World. John Meehan, Nesime Tatbul, Jiang Du, Stan Zdonik. CIDR'17
2. Ubiq: A Scalable and Fault-tolerant Log Processing Infrastructure.
3. Handling Shared, Mutable State in Stream Processing with Correctness Guarantees. Nesime Tatbul, Stan Zdonik, John Meehan, Cansu Aslantas, Michael Stonebraker, Kristin Tufte, Chris Giossi, Hong Quach. IEEE Data Eng. Bull. 38(4): 94-104 (2015)
4. Gobblin: Unifying Data Ingestion for Hadoop

1. Integrating real-time and batch processing in a polystore. John Meehan, Stan Zdonik, Shaobo Tian, et al. HPEC'16
2. Towards Dynamic Data Replacement for Polystore Ingestion. Jiang Du, John Meehan, Nesime Tatbul, Stan Zdonik. BIRTE'17

### Stream Processing
1. Continuous Analytics Over Discontinuous Streams. SIGMOD'10.
这篇文章针对的现实场景中数据流中的数据存在顺序不连续（不一致）的问题。
2. Discretized Streams: Fault-tolerant Streaming Computation at Scale. SOSP'13.
3. Highly Available, Fault-tolerant, Parallel Dataflows. SIGMOD'04.
4. Naiad: A Timely Dataflow System. SOSP'13.
5. Transactional Stream Processing. EDBT'12.
6. STREAM: The Stanford Data Stream Management System. Data Stream Management: Processing High-Speed Data Streams'06.
7. MillWheel: Fault-Tolerant Stream Processing at Internet Scale. PVLDB'13.
8. The Design of the Borealis Stream Processing Engine. CIDR'05.
9. Aurora: A New Model and Architecture for Data Stream Management. VLDBJ'03.
10. Fault-tolerance in the Borealis Distributed Stream Processing System. TODS'08.
11. S-Store: Streaming Meets Transaction Processing. John Meehan, Nesime Tatbul, Stan Zdonik, et al. VLDB'15

+ Flink
+ Samza
+ Storm

### Sleev and APWeb paper
1. Hash based vs. LSM-based data storage.
2. In-memory streaming DAG.

### SMR
1. Skylight: A Window on Shingled Disk Operation
2. SMaRT: An Approach to Shingld Magnetic Recording Translation

### 有影响力的系统论文
1. The Google File System
2. MapReduce: Simplified Data Processing on Large Clusters
3. Resilient Distributed Datasets
4. Dryad: Distributed Data-parallel Programs from Sequential Building
5. Bigtable: A Distributed Storage System for Structured Data
6. Dynamo: Amazon's Highly Available Key-value
7. Spanner: Google’s Globally-Distributed Database
8. F1: A Distributed SQL Database That Scales
9. Cassandra