
# GFS

- Why hard ?
    - performance -> sharding
    - Faults -> tolerance
    - Tolerance -> replication , gonna get out of sync
    - Replication -> inconsistency
    - Consistency -> low performance


- GFS
    - Big, fast
    - Global
    - Sharding
    - Automatic recovery

GFS was designed to run in a single data center, just for internal use, big sequential (not random) access.


```gfs
master 

cs 
cs
cs 
...
```

- 1 master, mulitple chunck server 
- master data maintain 2 tables 
    1. file name -> array of chunk handles (chunk ids)  (nv)
    2. chunk handle -> 
        - list of chunk servers  (v)
        - version number for each chunk  (nv)
        - primary  (v)
        - lease expiration  (v)
- master has log and checkpoint on disk
    - start from checkpoint, then recover from log
- client read
    1. file name , offset/range -> master
    2. master sends chunk handle, list of servers 
    3. C -> CS
- write 







