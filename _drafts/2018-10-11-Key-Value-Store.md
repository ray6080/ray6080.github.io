---
layout: post
title: Implementing LusterKV - A Key-Value Store Optimized for SSDs
comments: true
categories:
- blog
tags:
- system
---

## Motivation
Recently, I took part in a competition to implemente a key-value store on solid state drives(SSDs).
I've played around with some key-value stores, such as Redis, etcd and TiKV, I even read some source code of Redis years ago.
However, I've never been determined to implement one from scratch by myself.
So, this post is also a qucik note of my thoughts and problems.

## A quick overview of key-value stores
Key-value stores are ubiquitous in the real world.
It has one of the simplest form of programming interfaces, which can be found in almost any programming language.
Data in a key-value store is organized as associations (a key and a value).
Generally, key-value stores share the following interface:

**Get(key)**: Get some data by "key", or fail if no data is associated with this "key".

**Set(key, value)**: Associate a "value" with the "key". If some data was already associated with this "key", this data will be replcaed.

**Delete(key)**: Delete the association of the "key".

Unlike relational databases, key-value stores have no pre-defined schema, thus, it is flexible, and adaptive.
And some time, its data model is too simple to match real-world applications.

## A quick introduction to SSD