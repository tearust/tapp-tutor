From 10k feet high, all blockchains are nothing but different types of  [[State_Machine]].  This machine accept user tx as input, execute logic (for example smart contract) to update the [[state]]. Every block is a new updates of the state. Any node can rebuild the latest state by recalculate from the genesis block. This recalculation is also a process of validation.

TEA Proejct is also a state machine, but a very different type of state machine. There is no block, there is no TPS, no smart contract, no validator. It is more likely a distributed database. 

The smart contract is likely the stored procedue in regular database, but it is called [[state_machine_actor]] in TEA Project. It can do more than just basic operation as smart contract can do, and it is much faster because there is no block limitation. That means the update of state is continous updating local state seperately without stop every few seconds waiting for consensus. There is also no size limitation of block, beause there is no block at all.

To achieve there, the most important thing is the sequence of all transactions (someone call them events) has to be identical to every [[State_Machine_Replica]]s. No one get more or less transactions, no one get them in different order. Otherwise, the state would no longer be sync among all nodes.

The sequence of the transactions are garranteed by [[conveyor]]. The algorithm is also called **proof of time**.

This unique way of thinking makes TEA Project a unique platform. Of course, this is based on the trust from layer1 and [[Remote_Attestation]]. Otherwise, any [[hosting_CML]]nodes can cheat [[State_Machine_Replica]] with wrong [[GPS]] timestamp and cauase the whole system stop working. 