This actor is TApp's actor that loaded into [[mini-runtime]] of [[State_Machine_Replica]]. It handles the [[txn]](or called command) to update state. 

Only the [[txn]] moved to the last execution point of [[conveyor]] can be picked up and executed in this actor.
