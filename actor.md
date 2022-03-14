Actor in TEA project is an executable module. It works as a dynamic link library. It can be written in any programming languages but need to compile into WebAssembly format. At runtime, it is loaded into the [[mini-runtime]]. The mini-runtime waits for the incoming request, parse the request, find the corresponding actor, and dispatch the request to the coresponding actor's handler function. The handler function handles the request then generate response back to the mini-runtime. Mini-runtime then send back the response.

In this case the actor works as a Function-as-a-Service Lambda.

# Some Notes
There are a lot of limitations for actors.
For example:
- The actors are stateless. You cannot store any state inside the actor. 
- The actors cannot control anything besides its own internal memory. That means it cannot send network data, cannot read/write any file, cannot read/write any memory outside of its own.
- The actors are short life and cannot keep running as a long running service. Once the function executation is done, it losts all resources it occupied. 

But you can use [[providers]] to somehow overcome those limitations. Of course, the providers will check the [[capability]] of such an actor. If the actor doesn't have such capability, the request will be rejected. This is one layer of security control

