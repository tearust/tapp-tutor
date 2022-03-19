Mini-runtime is a WebAssembly runtime made by the TEA Project team based on the existing WasCC runtime.

The mini-runtime has a host executable and a bunch of [[provider]]s and [[actor]]s. The actors are WebAssembly modules containing [lambda](https://en.wikipedia.org/wiki/Lambda_calculus) functions. The providers are native executable libraries that provide features actors can call. Usually these features are forbidden from used by actors (for example, networking or saving data to persistent storage (IPFS/OrbitDB)).

The mini-runtime is the only executable allowed to run inside the [[enclave]]. 
