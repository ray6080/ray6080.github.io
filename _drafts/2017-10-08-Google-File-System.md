---
layout: writeup
title: Google File System
full_title: "Google File System"
author: Sanjay Ghemawat, Howard Gobioff, and Shun-Tak Leung
link: https://pdos.csail.mit.edu/6.824/papers/gfs.pdf
publication: SOSP'03
comments: true
categories:
- writeup
---

GFS is Google's first publicly known distribtued file systems.

### Assumptions(Application Needs):
1. component failures are the norm rather than the exception.
2. files are huge by traditional standards.
3. most files are mutated by appending new data rather than overwriting existing data.
4. the workloads primarily consits of two kinds of reads: large streaming reads and small random reads.
5. the workloads also have many large, sequential writes that append data to files.
6. high sustained bandwidth is more important than low latency.

### Architecture:
Master + Chunk Servers
Master:
1. maintains metadata. (namespace, access control information, mapping from files to chunks, current locations of chunks)
2. chunk lease management
3. garbage collection

### Client Interactions
#### Read
Client asks master for chunk handle and locations of the replicas, and client caches this info. Then client sends a request to the closest replica. Further reads of the same chunk require no more client-master interaction.
### Write

### Chunk Size
Default chunk size is 64MB, which is much larger than typical file system block sizes.
Advantages:
1. reduce client's interaction with master.
2. reduce size of the metadata stored on the master.
3. on a large chunk, a client is more likely to perform many operations on a given chunk, reducing network overhead by keeping a long TPC connection.
Actually, I think the biggest advantages are reducing master overhead and taking advantage of sequential disk I/O.

### Metadata
the file and chunk namespaces and the mapping from files to chunks are kept persistent by logging mutations to an operation log store on the master's local disk.
the locations of each chunk's replicas are not stored persistently on master. it asks each chunkserver about its chunks at master startup and whenever a chunkserver joins the cluster.