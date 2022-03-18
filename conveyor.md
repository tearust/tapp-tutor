The conveyor is an algorithm to make sure all state machine nodes have the same order of transactions. This is necessary as their will occassionally be conflicts when reconciling multiple, distributed versions of the app sending transaction data back. The conveyor algorithm is designed to merge the possibly conflicting transaction timestamps.

- Any particular hosting node shouldn't change the state of the state machine directly.
- It instead issues the state change command to a conveyor belt of one or more replicas.
- After a set period of time, the conveyor belt ensures that transactions have reached a steady order before updating all state machine nodes to the same new state.

## Workflow of the conveyor belt algorithm

The delegator actor receives user interaction, generates a hash of the transaction and sends it to the conveyor belt of one or more replicas. The timestamp of this send was recorded but not able to be included in the transaction, therefore a followup message must be sent out that includes:

- the hash of the previously sent transaction.
- the timestamp of the previously sent transaction.
- if this is not the first followup message (i.e. the transaction reported to other replicas' conveyor belts has already issued their followup messages), then the followup message will also record the existing timestamps as recorded in the other replicas' followup messages.

## Conveyor belt: mutable vs inmutable areas

Each replica's conveyor belt has a mutable section at the front followed by an immutable section.

- **Mutable section**: transactions can be re-ordered in this section based on the earliest timestamp recorded in the followup messages. Note that any particular replica's conveyor belt must receive both the transaction and its corresponding followup message within this mutable block of time. If either is missing, the transaction is dropped from the conveyor belt before it reaches the next immutable section.
- **Immutable section**: after a transaction and its followup message has been on the conveyor belt for a mutable period of time, the transaction passes on to the immutable section. Transactions in the immutable section cannot be re-ordered; however, any conveyor belt's immutable section that's missing a transaction from other replicas will have the missing transaction added to its conveyor belt in the same relative order as the reporting replica.

A consequence of this conveyor algorithm is that even though a particular replica will have to drop a late arriving transaction (arrives after mutable period of time and is unable to be placed in the immutable section), the replica's conveyor belt will still be able to add the transaction to its immutable section through syncing with other replicas.

## Consensus on the transaction order

The end goal of the conveyor algorithm is to reach a consensus on the order of transactions. In the end, all replica conveyor belts should match the same order. This is achieved through each replica's immutable section of the conveyor belt. 

- If a particular replica stops adding new transactions within the immutable period, then it individually has reached a steady ordering of local transaction data.
- Once more than half of the replicas can no longer change the ordering in their immutable section, that will act as a successful vote on the transaction order contained in the immutable section.
- After a few sync intervals, the immutable sections of all replicas will reach consensus.
- Once consensus is reached on the order of transactions in the immutable section, the transactions are ready to be picked up and added to the state machine.
