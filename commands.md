In contrast to [[queries]], commands can change the state in the [[state machine]]. 

In order to keep the consistency of the state machine, we cannot allow the hosting node to ask one state machine node to change its state directly. We have to put the change request - actually a command - to a [[conveyor]]. The conveyor algorithm will make sure all state machine nodes get the same sequence of all comands, and eventually update all of their states to the same new state.

Because it's a sync call, the caller (hosting node) cannot get the execution result of this command immediately. Instead, the caller will poll the result after a few seconds to get the result.

