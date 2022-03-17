Every TApp has an AES key. 
This AES key is used to encrypt/decrypt any data stored to hosting nodes' local IPFS or OrbitDB.

## Generating App AES Key
When TApp is first created in TAppStore, the GenreateAesKeyTxn is created and then executed in the [[State Machine]].

During execution, a random AES key is generated in the state.

State machine algorithm can keep the random AES key consistent to all state machine nodes.

## Hosting nodes access to this AES Key
When hosting nodes start to host a TApp. The TApp actor can access the AES key from the state machine. The state machine only send AES key if the requestor is this TApp's actor.

