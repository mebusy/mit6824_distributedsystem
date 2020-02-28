
# Lecture 1: Introduction

If you can solve a problem on a single computer, you should do it that way.  Distribted system is not simple. 

- The reason to build a distributed system
    1. parallelism 
        - more CPU, Memory, Disk 
    2. fault tolerance
        - have 2 computers do the exact same things, if one fails you can cut over to the other one
    3. physical
        - some problems are just naturally spread out in space, i.e. interback transfers.
    4. security / isolated

- Challenges 
    - concurrency 
    - partial failure
    - performance


- infrastructure
    - storage
    - communication 
    - computation 

- Implementation 
    - RPC , which goal is to mask the fact that we're communicating over an unreliable network.
    - threads 
    - concurrency control, things like locks

- Performance 
    - scalability

- Fault Tolerance
    - Availability
    - REcoverability
        - tools to solve these problems
            1. non-volatile storage
            2. Replication

- Final Topic
    - consistency
        - strong consistency is often extremely  expensive



## MapReduce

[mapreduce](mapreduce.md)

example: word count

```
input 1 → Map →  a:1, b:1
input 2 → Map →       b:1
input 3 → Map →  a:1,     c:1
                   |   |   |
                   ↳---╋---╋--------> Reduce → a:2
                          ↳---╋-----> Reduce → b:2
                              ↳-----> Reduce → c:1
``` 

The map function doesn't know anything about distribution. 






