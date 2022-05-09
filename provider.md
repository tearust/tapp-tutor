Providers are utility static libraries inside [[mini-runtime]]. They are called by [[actor]]s inside the [[mini-runtime]]. Actors are webassembly code that can only run inside wasm virtual machine. But providers are native rust code that can run outside of the virtual machine. If you read the [[magic_of_wasm]] part of this document, you will know that webassembly is a very secure execution format due to it can run inside the wasm virtual machine. On the other hand, running inside virtual machine means it cannot call system function directly. Assuming you are writing Solidify smart contract, you will know that you cannot call OS API direclty because the smart contract will be running inside EVM. 

What if the actor code wants to call a OS function to send data over network or save a number to the [[state]]? Providers are here for help.

# Providers security concern
There are many providers inside [[mini-runtime]]. They are all writen by the TEA Project core team at this moment. Because provider code is native code that run in OS privillage, it is much more powerful than the code inside actors. Powerful means more damage if abused. Actors code cannot do too much damage because the isolation and limitation of virtual machine. But if the actor code call provider code, the actor can make big damage if misused. In order to mitigate this thread, 
- For every type of system function, we developed separate provider
- Actor's developer need to choose and sign which providers it will use. This is also called [[capability]]
- The capability information is public data. If an actor is not supposed to use a provider but claimed it will, the DAO or end user will reject this application
- All providers code is carefully designed and audited.

# Calling provider
All functions inside provider are pure funcitonal function. That means they are **stateless**. The caller (from actor) needs to send all necessory parameters in the function and will get the result (bytes) in return value.

Every function has a OP_CODE. All input values and out put values are Protobuf encoded. In most cases, the actor call to provider is happening inside the same encalve. There is no need for encryption and all calls are sync call.

# Error handling
TODO: