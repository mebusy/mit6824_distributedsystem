
# Lecture 12: Distributed Transactions

- Transaction ACID
    - Atomic 
        - all or none vs failure
    - Consistant
    - Isolated
        - Serializable
            - you look at the results, and see if you can find some execution order that does produce the same results
        - can't see each other's intermediate states, but only complete transaction results
    - Durable



- Distributed Transactions
    - concurrency control 
        - mail tool we use to provide serializability , or isolation
    - atomic commit 


## concurrency control 

- 2 major approach
    1. pessimistic
        - lock , Two-Phase Locking
    2. optimistic
        - only at then you go and check whether actually maybe some other transaction might have been interfering
        - if somebody else was modifying the data in a conflicting way at the same time you were then you have to abort that transaction.
        - faster if conflicts are rare

- Name ?
    1. acquire a lock when using any record
    2. a transaction must hold any locks it acquires unitl after it commits or aborts.
        - required for correctness

- Two-Phase Locking
    - TC: Transaction Coordinator
    - A,B: Participant , for example, 2 servers

```bash
TC          A        B

-------get--> 
----------------put-->

---prepare-->
<----yes/no--
------------prepare-->  LOG 
<-------------yes/no--

----commit-->
<-------ack--
-------------commit-->
<---------------ack--
```

- Question
    1. what if B crashes after receiving prepare message, before sending the yes ?
    2. what if B crashes after receiving commit message ?
        - A do its part of the transation and make it permanent
            - B can easily abort the transaction
        - we're absolutedly required B that on recovery it be still prepared to complete its part of the transaction 
            - at that point whether B doesn't know whether it should commit or not , because it actually didn't receive the commit message.
        - that means the fact that we can't lose the state for a transaction across crashes and reboots.
            - before B replies to a prepare it must make the transaction state durable on disk
                - this sort of intermediate transaction state, the memory of all of the changes that's made which may have to be undone if there's an abort , 
                - PLUS, the record of all the locks the transactions held.
            - it's almost always in a log on disk







