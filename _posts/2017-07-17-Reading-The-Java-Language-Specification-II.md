---
layout: post
title: Reading The Java Language Specification II
comments: true
categories:
- blog
tags:
- system
---

I've been used Java as my first weapon for years, yet I'm alway confused by language details and running internals. Thus I want to take a closer look at the inside of this language, and reading **The Java Language Specification** is a good way. Since Java 8 is popular and widely used now, I choose *Java SE 8 edition* as the standard. This is a start of a series of posts to reading digests and thoughts recording.

<hr>

## Classes

Class declarations define new reference types and describe how they are implemented. There are some rules for class definition:
+ A named class may be declared `abstract` and must be declared if it is incompletely unimplemented. Abstract class can not be instantiated directly, but can be extended.
+ A class may be declared `final`, which means it cannot have subclasses.
+ A class declared `public` can be referred to from other packages.
+ Each class except `Object` is an extension of a single existing class, and may implement several interfaces.
+ Classes my be `generic`.

