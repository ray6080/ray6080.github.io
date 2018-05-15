---
layout: writeup
title: Google File System
full_title: "Skipping-oriented Partitioning for Columnar Layouts"
author: Liwen Sun, Michael J. Franklin, Jiannan Wang and Eugene Wu
link: https://www.cs.sfu.ca/~jnwang/papers/vldb2017-gsop.pdf
publication: SOSP'03
comments: true
categories:
- writeup
---

Previous Work: Fine-grained partitioning for aggressive data skipping. SIGMOD'04

Modern analytics applications can involve wide tables and complex workloads with diverse filter predicates and column-access patterns.

Previous work generates a single horizontal partitioning scheme that incorporates all features, when there are many highly conflicting features, it may be rendered ineffective.

Skipping-aware column grouping technique. Proposed an objective function to quantify the trade-off of skipping effectiveness vs. tuple-reconstruction overhead.