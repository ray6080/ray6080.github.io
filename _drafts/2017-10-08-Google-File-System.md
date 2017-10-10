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

GFS is Google's first distribtued file systems.

Assumptions:
1. component failures are the norm rather than the exception.
2. files are huge by traditional standards.
3. most files are mutated by appending new data rather than overwriting existing data.
4. the workloads primarily consits of two kinds of reads: large streaming reads and small random reads.