---
layout: post
title: Presto Internal
comments: true
categories:
- blog
tags:
- system
---

## Memory Block
1. AbstractArrayBlock
    1.1 ArrayBlock
    1.2 ArrayBlockBuilder [BlockBuilder]
2. AbstractArrayElementBlock
    2.1 ArrayElementBlockWriter [BlockBuilder]
3. AbstractFixedWidthBlock
    3.1 FixedWidthBlock
    3.2 FixedWidthBlockBuilder [BlockBuilder]
4. AbstractVariableWidthBlock
    4.1 SliceArrayBlock
    4.2 VariableWidthBlock
    4.3 VariableWidthBlockBuilder [BlockBuilder]
5. AbstractInterleavedBlock
    5.1 InterleaveBlock
    5.2 InterleaveBlockBuilder [BlockBuilder]
6. ByteArrayBlock / ByteArrayBlockBuilder [BlockBuilder]
7. DictionaryBlock
8. GroupByIdBlock
9. IntArrayBlock / IntArrayBlockBuilder [BlockBuilder]
10.LazyBlock
11.LongArrayBlock / LongArrayBlockBuilder [BlockBuilder]
12.RunLengthEncodedBlock
13.ShortArrayBlock / ShortArrayBlockBuilder [BlockBuilder]
14.SliceArrayBlock

### Block Encoding
