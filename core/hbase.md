---
layout: page
title: HBase
categories: core-eng
---
## Basics
 * HBase is a column-based key-value store modeled on Google's BigTable. It runs over HDFS (Hadoop File System).
 * HBase has high write-throughput and scales well horizontally.
 * HBase works well for flexible datamodels of sparse records i.e. much of the fields are empty/null.
 * It also supports features such as automatic failover, versioning and compression out of the box.
 * Altering columns is expensive as HBase has to create a new columns and copy over all the data.
 * Each column can be fine tuned to affect read and write performance and space consumption. You miss out on fine-grained tuning if you de-normalize everything to fewer column families.
 * Column families stored in different directories allow you to only read columns you are interested in. This is great for read-heavy workloads.
 * Operations are atomic at the row-level, so data across multiple columns families in a record is always consistent.
 * Rows are sorted by the row-key.
 * Regions are chunks of rows stores on a region server. Each region server typically stores multiple regions.
 * The architecture comprises of a Master server and multiple region servers. The master server makes use of Zoo Keeper to manage configuration and synchronization. Read/write requests from clients directly go to the region servers.

## Internals
 * HBase uses Log Structured Merge Trees (LSTM) to store data.
 * Rows of data in HBase are split into shards called regions. Every region is stored on a region server. A region server may host one or more regions.
 * When data is written to HBase, it is first recorded in a WAL (Write-Ahead Log)  called HLog. The data is then stored in an in-memory MemStore, which is flushed to disk when its size reaches a specific threshold. The file written to disk is an immutable index-organized data file called an HFile.
 * HBase performs compaction to reduce read overhead by merging several smaller files into fewer larges ones. During this process, two or more sorted HFiles are merge-sorted into a single file. This helps co-locate all data for a given key on disk, as well as eliminate deleted data.
 * Automatic rollover is supported, enabling all regions on a failed region server to migrate over to another region server. This is done by replaying all the content associated with a region onto a different region server. This makes failovers expensive.
 * HDFS supports HBase with replication, end-to-end checksums and auto-matic rebalancing.