---
layout: post
title: Presto Internal
comments: true
categories:
- blog
tags:
- system
---

This post is to dive into some details of file systems.

## Prototypical System Architecture
Buses: memory bus; I/O bus; PCI; peripheral bus(SCSI, SATA, USB).

## Device
### Interface
Registers: Status; Command; Data
### Internals
### Protocol
DMA: A DMA engine is essentially a very specific device within a system that can orchestrate transfers between devices and main memory without much CPU intervention.

## Interruptions

## Device Interactions
Port IO vs. Memory-Mapped IO

## The File System Stack
Fig 36.3 in ThreePieces.

## A Simple IDE Disk Driver
### Registers: control, command block, status, and error

## Page Cache and Direct IO

## Fault Tolerance and Recovery

## RAID
### Raid0
### Raid1
### Raid4
### Raid5

## File System Implementation

### NTFS

### Ext2

### Ext3
 
### Ext4

### FAT32

### ZFS

## Databases and File System
### LSM

### References
[1]:https://medium.com/@ifesdjeen/on-disk-io-part-1-flavours-of-io-8e1ace1de017