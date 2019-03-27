---
layout: post
title: Daily Paper
skip_related: true
---

highlight important/interesting papers from published ones related to the database system every day.

### Mar 27, 2019

**Everything You Always Wanted to Know About Compiled and Vectorized Queries But Were Afraid to Ask**  
T. Kersten, V. Leis, A. Kemper, T. Neumann, A. Pavlo, P. Boncz. PVLDB, 11 (13): 2209 - 2222, 2018.  
http://www.vldb.org/pvldb/vol11/p2209-kersten.pdf  

Vectorization and data-centric code generation are two state-of-the-art query processing paradigms prevelant in modern database systems. Vectorization is pioneered by VectorWise, and data-centric code generation is explored by Hyper. Though these two paradigms are widely used and fundamentally different, it is not clear which paradigm yeilds faster query execution until today. This paper experimentally compares these two models by implementing both within the same test system, and find that both models are efficient with different strengths and weaknesses.