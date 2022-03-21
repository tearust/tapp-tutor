# Think out of the box
Consensus is the most improtant concept in blockchain world. It makes distributed nodes agree on what the next state should be. There are many different types of consensus for different blockchain project. There are many new consensus algorithm invented every few months. Although we tried a lot, the blockchain is still far from the efficiency of cloud computing. The reason is still the consensus. The nature of blockchain requires all the nodes to take a break every few minutes or seconds, they communicate with other nodes then run a complex algorithm to determine the next block. This make the blockchain slow and costy. The cloud computing doesn't require that. You probably never heard of the TPS concept in the blockchain world. The cloud computing has a big advantage of centralization. The nodes can fully trust other nodes without running complex consensus. Can we find a best consensus of blockchain that can run as fast as cloud computing? First lt's think out of the box
# The best consensus is no consensus
As we mentioned above, as long as there is a consensus, blockchain can never get to the same speed as cloud computing. What is the best consensus? No consensus is the best consensus .

Assuming there is a magic, no need to run consensus, every standalonel node can make decision on what the next state is supposed to be. All of the nodes can magically get the same state. Now we have set the goal, let's see how can make the magic into reality. 

# The key is the sequence of events
Consider the whole blockchain is a state machine. Event (we usually call it "transaction") is the outside trigger to change the state inside the state machine. Every node is a replica of the state machine. As long as every node can get the same sequence of event, and update the state accordingly, the new state will be the same across all replicas.

If we review all existing consensus no matter proof of whatever, they all do the same thing: Make all nodes agree on a sequence of events. 

Now, can we get the sequence of all events without consensus?

# Proof of Time
As long as we stay on the earth without travelling near light speed, we can consider the time is a stable physical value. If we give every event a timestamp before sending to replicas, this timestamp can be trusted. Then all the replica can sort events based on the timestamp without communicate with others. Of course, given the network latency, we allow a grace period or buffer period that replica wait prior to execute the event. 

Actually using time as the RoT (Root of Trust) of  consensus is commonly used in cloud computing. One example is Google Spanner. But it is not widely used in blockchain world simple because blockchain  cannot trust other nodes as centralized cloud computing do. 

Now the question is can we trust the timestamp attached of the event since the event was created outside of blockchain?

Well, we have another RoT(Root of Trust), the [[TPM]] chip.


# Proof of Trusted Computing
TEA Project is a project that rely on hardware RoT. Every TEA node need to have the [[TPM]] chip built-in. The TPM chip use Trusted Computing technology to collect the evidence of hardware integraty. Those TPM data verified by other TEA nodes (Verifier) via [[Remote Attestation]] (RA). The decision is made by the verifier independently, and finaly judgement is done by the blockchain's BFT. If any change on a TEA node, no matter hardware or software, it will be detected by verifier, furthermore marked "malicious" in the blockchain. Other TEA nodes immediately reject communication with the bad actor. 

If all the TEA nodes are protected by the TPM chip. All data generated inside this node (actually inside the enclave of this node) can be trusted. Including the timestamp we used to attach events.
# GPS as time source of atomic clock
Although we can trust the TEA node that protected by TPM. But the internal hardware clock (most likely quartz clock) cannot be trusted. It is not acurate and precious enough to be used to sort events. Require ever TEA node to have a atomic clock built-in is not practical financially. The best solution is to use GPS satelites. GPS sending free time signal to all GPS receiver. We do not need to use the signal to calculate geolocation, we only need the time as event timestamp. Because the protection of TPM, the timestamp can be trusted.

# Conclusion
All TEA nodes have TPM protected [[enclave]]. The TPM also protect the GPS receiver and the timestamp it generated. All event generated from those TEA nodes will have a timestamp attached. The events are sent to a group of [[State_Machine_Replica]]s. The events are insert and sorted in the [[conveyor]] before eventually executed by [[state_machine_actor]]. When the events are executed, it changes the [[state]].  

# Notes
## Sync messages between replicas
Although no conensus is required between replicas, but we still need to keep data sync between them. Mainly to fill in missed events and get the earliest timestamp for the same event. The detail is explaned in [[conveyor]]. 

## Event or Command?
In TEA Project, we call the events that could change the state a [[commands]]. If an event is just a query of current state, without any change of state, we call it a [[queries]]. 

We use event here in this article to be consistent of most common terminologies used in distributed computing. 

If an event is just a query, it doesn't need to go through the whole grace period process. It go direclty to the state machine and query. See [[queries]] for detail.

## Continuous state updates
There is no block in our new consensus. There is no need to wait every few minutes or seconds for next block. TEA Project state machine is continuous update similar to a distributed database without a central control.

## Grace period (or buffer period)
When an event is send to a [[State_Machine_Replica]], it will not be executed immediately. It will be staying in the [[conveyor]] for a short period of time. During this period , the sequence of events will be re-ordered based on timestamp. When they are executed, the sequence is confirmed. We are testing the best length of the grace period. At the time of this article is written, we set the period to 3 seconds.

## Finality
As long as the event is executed in the state machine, it is finality. 

## To learn more?
Please go to [[conveyor]] and [[State_Machine]] to learn more
