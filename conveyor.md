The conveyor is an algorithm to make sure all state machine nodes have the same order of transactions. This is necessary as their will occassionally be conflicts due to network latency and the redundancy of transactions sent to multiple replicas. The conveyor algorithm is designed to shuffle the transaction order, which includes merging duplicate transactions and re-ordering transactions according to their timestamps.

- Any particular hosting CML node shouldn't change the state of the state machine directly.
- It requests to change the state by sending its transactions (along with followup messages) to the conveyor belts of two or more replicas.
- After a set period of time, the conveyor belt ensures that transactions have reached a steady order before all state machine actors update to the same new state.
- The updated app state is sent back to the application which updates its interface accordingly.

TApp developers are responsible for sending out their application's transactions and followup messages to a minimum of 2 replicas. Using the API, the default number of replicas sent to is two. More replicas may be chosen by the developer in certain instances according to the importance of the transaction to their app.

## Workflow of the conveyor belt algorithm

The hosting CML node receives user interaction, generates a hash of the transaction and sends it to the conveyor belt of two or more replicas. The timestamp of this send was recorded but not able to be included in the transaction, therefore a followup message must be sent out that includes:

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

- Once at least 50% of all replicas agree on the transaction order in their immutable section, this will act as a successful vote on the transaction order contained in the immutable section.
- After a few sync intervals, the immutable sections of all replicas will reach consensus.
- Once consensus is reached on the order of transactions in the immutable section, the transactions are sent to the state machine actor.
- The state machine actor updates its state according to the transactions the replicas send to it.

## Sending the updated state back to the apps
The successfully confirmed transactions have updated the current state of the app. These state changes must now be sent back to the app from where they first came. The final workflow where the state changes are reflected back in the actual app look like this:

1. After the immutable section of replicas' conveyor belts has reached a consensus, an executable actor sends the transactions to the state machine actor in the replica.
2. The state machine actor deals with the possibility of failed transactions:

- If it's successful, then modify / update the app state.
- If the transaction is unsuccessful, then issue an error message and don't modify the state.

3. The result of the previous process, either an updated state or an error message, is then sent back to the original application.
4. The application updates its UI to reflect the updated state for transactions that successfully updated the state, and it shows the error message for unsuccessful transactions.
