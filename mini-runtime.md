Mini-runtime is a WebAssembly runtime made by the TEA Project team based on existing WasCC runtime.

The mini-runtime has a host executable and a bunch of [[provider]]s and [[actor]]s. The actors are webassembly modules contains of [lambda](https://en.wikipedia.org/wiki/Lambda_calculus) functions. The providers are native executable library that provides features actor to call. Usually these features are forbidden from used by Actors. For example network or save data to persistent storage (IPFS/OrbitDB).

Mini-runtime is the only executable allowed to run inside the [[enclave]]. 