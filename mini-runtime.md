Mini-runtime is a WebAssembly runtime made by the TEA Project team based on the existing WasCC runtime.

The mini-runtime has a host executable and a bunch of [[provider]]s and [[actor]]s. The actors are WebAssembly modules containing [lambda](https://en.wikipedia.org/wiki/Lambda_calculus) functions. The providers are native executable libraries that provide features actors can call. Usually these features are forbidden from used by actors (for example, networking or saving data to persistent storage (IPFS/OrbitDB)).

# Protected by the enclave
The mini-runtime is the only executable allowed to run inside the [[enclave]]. The benefits are
- It is fully isolated from the rest components of the node. The operating system has no access to the inside of enclave. Even the owner who has the full access to the node hardware and its operating system cannot know what is happening inside the enclave
- To achieve the security above without using complex math algorithm (MPC, FHE, ZK etc). No overhead nor energy waste.
- Although the inside is unknown to outside, the TPM chip can still send verification data to the remote verifiers. If anything wrong inside, the verifiers know. Of course, the verifier need to be trusted first. 
- If we trusted the hardware and the code/data loaded into the enclave, the computational results can be trusted as well.


