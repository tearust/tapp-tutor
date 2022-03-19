## Conveyor belt: mutable vs inmutable areas

Each replica's conveyor belt has a mutable section at the front followed by an immutable section.

- **Mutable section**: transactions can be re-ordered in this section based on the earliest timestamp recorded in the followup messages. Note that any particular replica's conveyor belt must receive both the transaction and its corresponding followup message within this mutable block of time. If either is missing, the transaction is dropped from the conveyor belt before it reaches the next immutable section.
- **Immutable section**: after a transaction and its followup message has been on the conveyor belt for a mutable period of time, the transaction passes on to the immutable section. Transactions in the immutable section cannot be re-ordered; however, any conveyor belt's immutable section that's missing a transaction from other replicas will have the missing transaction added to its conveyor belt in the same relative order as the reporting replica.

A consequence of this conveyor algorithm is that even though a particular replica will have to drop a late arriving transaction (arrives after mutable period of time and is unable to be placed in the immutable section), the replica's conveyor belt will still be able to add the transaction to its immutable section through syncing with other replicas.

## Consensus on the transaction order

The end goal of the conveyor algorithm is to reach a consensus on the order of transactions. In the end, all replica conveyor belts should match the same order. This is achieved through each replica's immutable section of the conveyor belt. 

- Once at least 50% of all replicas agree on the transaction order in their immutable section, this will act as a successful vote on the transaction order contained in the immutable section.
- After a few sync intervals, the immutable sections of all replicas will reach consensus.
- Once consensus is reached on the order of transactions in the immutable section, the transactions pass to the execution point of the conveyor.
- From the execution point, the transactions are sent to the state machine actor.
- The state machine actor updates its state according to the transactions the replicas send to it.

![3](https://user-images.githubusercontent.com/86096370/159133556-678886b8-3d81-456e-a623-898be3bd1a41.png)
