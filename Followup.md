TEA Project [[State_Machine]] requires every event ([[commands]] or  [[queries]]) has a acurate and trusted timestamp attached. Assuming the P2P network is unreliable, every message is supposed redundancy. That means for any event, the [[hosting_CML]] has to send at least two identical messages to two different [[State_Machine_Replica]]s. The logic in the [[back_end_actor]] is single threaded, the two identical messages will be different timestamp (one after another). To make they are fully identical, we designed a followup message that will be sent right after a message was sent.

The information in this followup messages are
- The hash of the event message just sent out
- The actual timestamp when that message was sent out.
- If the event message is a duplicated (redundancy) message, use the timestamp of the first time it was sent (not this duplicated/redundancy message). 

Using this design, the multiple event messages will have the identical message body and identical followup messages. Because the timestamp (which is the only data could be different) is set to the timestamp of the first message was sent. The second (or any later) message's sent timestamp are ignored.

