In contrast to[[queries]], commands can change state in the state machine. 

In order to keep the consistency of state machine, we cannot allow the hosting node to ask one state machine node to change its state directly. We have to put the change request - actually the command - to a [[conveyor]], and the conveyor algorithm will keep all state machine nodes to get the same sequence of all comands, and eventually update all of their state to a same new state.

Because it is a sync call, the caller (hosting node) cannot get the execution result of this command immediately. Instead, the caller will pull the result after few seconds to get the result.

