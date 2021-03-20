
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
    - the client asks the master , I would like to append to this named file , please tell me where to look for the last chunk in the file.
    - Unfortunately now the writing needs to be a primary.
        - no primary on master 
            - find up-to-date replicas
            - pick primary, secondary
            - increment version number
            - tells P,S the V#
                - it also give the primary alease , look you're allowed to be primary for the next 60 seconds. after 60 seconds you have to stop. and this is part of the machinery for making sure that we don't end up with 2 primaries. 
            - master writes v# to its disk

- "SPLIT BRAIN"
    - it usually said to be caused by netword partition , that is  some network error that the master can not talk to primary but the primary can talk to clients.
    - These are the hardest problems to deal with. 
    - To solve that, what the master does, is to give a primary lease. If the master can't talk to the primary and the master whould like to designate a new primary , the master must wait for the lease to expire.

50:00 TODO









