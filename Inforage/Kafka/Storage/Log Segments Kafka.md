The hierarchy of data in Kafka is:

- topic
    - partition(s)
        - replica(s)
            - segment(s)
                

The basic storage unit in Kafka is a replica.  
The basic storage unit in the OS is a file.

A broker stores a replica of a partition and said replica is simply _**a bunch of files**_ on the filesystem.

A _**Kafka Log Segment**_ is a **file** on the filesystem containing data of a Kafka partition.

At any given time, there is _**only one**_ “==active==” segment. The active segment is the one where new records are written to.

Combined together in consecutive order, all the files form the full partition.

![[segments.avif]]


# How Large is a Segment?

A **Segment Roll** is when we convert an active segment into an **inactive** segment.
The process simply closes the file and opens it in **read-only mode**.
\
It is configurable. You can limit size based on **time** or **bytes**.

**Static Per-Broker Configs:**

- `log.segment.bytes` - the maximum size (in bytes) of a single segment (file). Defaults to 1GB.
- `log.segment.ms` - the duration (in milliseconds) from file creation that Kafka will wait until it closes the segment, only in case `log.segment.bytes` is not reached before that

NOTE: Only **non-active** Kafka segments can be considered for deletion.

Therefore, the **larger** the segment size (relative to the producer throughput for that partition), the **longer** it will take for the segment to be closed.

It is only _**after**_ it is closed that the retention settings (`retention.ms`/ `retention.bytes`) begin to take effect in counting down when it’ll be deleted.

Note that these settings are different from what we’ve mentioned so far.
# Recourse

- [Kafka Log Segments Explained](https://2minutestreaming.beehiiv.com/p/kafka-log-segment-files-explained)
- 

