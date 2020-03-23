---
title: MapReduce运行流程
tags:
  - Big Data
date: 2019-03-13 10:25:50
---
input
    split
mapper
shuffle
    partitioner
    MemoryBuffer
    spill
    merge on disk
    sort
reducer
留坑待填