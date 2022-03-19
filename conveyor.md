The conveyor is an algorithm to make sure all state machine nodes have the same order of transactions. This is necessary as there will occassionally be conflicts due to network latency and the redundancy of transactions sent to multiple replicas. The conveyor algorithm is designed to shuffle the transaction order, which includes merging duplicate transactions and re-ordering transactions according to their timestamps.

- Any particular hosting CML node shouldn't change the state of the state machine directly.
- It requests to change the state by sending its transactions (along with followup messages) to the conveyor belts of two or more replicas.
- After a set period of time, the conveyor belt ensures that transactions have reached a steady order before all state machine actors update to the same new state.
- The updated app state is sent back to the application which updates its interface accordingly.

TApp developers are responsible for sending out their application's transactions and followup messages to a minimum of 2 replicas (this is the default wheen using the API). More replicas may be chosen by the developer in certain instances according to the importance of the transaction to their app.

## Workflow of the conveyor belt algorithm

The hosting CML node receives user interaction, generates a hash of the transaction and sends it to the conveyor belt of two or more replicas. The timestamp of this send was recorded but not able to be included in the transaction, therefore a followup message must be sent out that includes:

- the hash of the previously sent transaction.
- the timestamp of the previously sent transaction.
- if this is not the first followup message (i.e. the transaction reported to other replicas' conveyor belts has already issued their followup messages), then the followup message will also record the existing timestamps as recorded in the other replicas' followup messages.

![1](https://user-images.githubusercontent.com/86096370/159111310-7184f1f5-462b-47d4-af83-dd6978a51528.png)

![2](https://user-images.githubusercontent.com/86096370/159133553-2742a99b-6823-402a-869c-19b2229192f3.png)

[[conveyor mutable immutable]]

## Sending the updated state back to the apps
The successfully confirmed transactions have updated the current state of the app. These state changes must now be sent back to the app from where they first came. The final workflow where the state changes are reflected back in the actual app look like this:

1. After the immutable section of replicas' conveyor belts has reached a consensus, an executable actor sends the transactions to the state machine actor in the replica.
2. The state machine actor deals with the possibility of failed transactions:

- If it's successful, then modify / update the app state.
- If the transaction is unsuccessful, then issue an error message and don't modify the state.

3. The result of the previous process, either an updated state or an error message, is then sent back to the original application.
4. The application updates its UI to reflect the updated state for transactions that successfully updated the state, and it shows the error message for unsuccessful transactions.

![4](https://user-images.githubusercontent.com/86096370/159111316-4c175a29-082c-4de4-bd5f-958bf30face1.png)

