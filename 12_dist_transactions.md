
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
<-------------yes/no--      _ 
                            |
----commit-->               |-- TIMEOUT-BLOCK
<-------ack--               |
-------------commit-->      -
<---------------ack--       
```

- Question
    1. what if B crashes after receiving prepare message, before sending the yes ?
        - B can easily abort the transaction
    2. what if B crashes after receiving commit message ?
        - A do its part of the transation and make it permanent
        - we're absolutedly required B that on recovery it be still prepared to complete its part of the transaction 
            - at that point whether B doesn't know whether it should commit or not , because it actually didn't receive the commit message.
        - that means the fact that we can't lose the state for a transaction across crashes and reboots.
            - before B replies to a prepare it must make the transaction state durable on disk
                - this sort of intermediate transaction state, the memory of all of the changes that's made which may have to be undone if there's an abort , 
                - PLUS, the record of all the locks the transactions held.
            - it's almost always in a log on disk
    3. what if B crashes before receiving the commit, or after , or might crash after actually committing ?
    4. what if TC failed ?
        - where things start getting critical is if any party might have commited, then we cannot forget about that.
        - if either of these participants have committed, or if the transaction coordinator might have aplied to the client, then we cannot have that transation go away, 
        - if the TC sent out a commit message to A, but hadn't gotten around to sending a commit to B, and it crashed at that point. THe TC must be prepared on restart to resend the commit messages to make sure that both parties know that the transaction is committed.
        - if TC crashes before sending the commit message, it can just abort the transaction.
            - anyways, it need LOG the transaction states before starting sending commit.
    5. When does TC can forget the transaction log ?
        - after it receiving all ACKs.

- Two-Phase commit you really only see it within relatively small domains, within a single machine room, within a single organization,  you don't see it for example did you transfers between different banks, you might possibly see it within a bank if it's sharded its database  but you would never see Two-Phase commit can it run between organizations that physically separated.



