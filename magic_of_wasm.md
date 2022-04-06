[WebAssembly](https://webassembly.org/)is getting more and more popular in blockchain world. This might be out of expectation since it was original designed to run inside browser. Anyway, javascript was originally designed to run inside browser too, but you know you can find javascript any where that code can run.

There are tons of reasons why TEA project use WebAseembly as the execution format. Name a few:
- Compact size
- Security by design
- Additional security by  running inside our [[mini-runtime]]  which inside our hardware [[enclave]]
- Can be compiled from many different high level programming languages
- Open source

In this article, we only focus on the security part. 

## No_std or std, smart contract or full programmable
For most new blockchain developers that start to write Substrate code, the non_std requirement would be the first barrier. Unlike most application, Substrate require no_std feature otherwise your code won't get compiled and run in substrate. Most of existing rust crates are written for std library. You cannot use them directly. This is makes programming substrate much harder. 

Of course, there is a reason for that. Substrate wants to make a much safer environment to run those wasm code as std library may be "unsafe" from a smart contract point of view.

Security and functionaility are always a trade off. The cost is that you cannot do too much in smart contract.  Again, smart contract is not a full featured application, it is just a "turing complete" state machine that mostly deal with accounting. Most of the functionalities of modern internet application are simply not possible in no_std.

# TEA Project is not a smart contract, it is Web3
The goal of the TEA Project is to run standard web3 application decentralized. Running a smart contract is not the goal. So TEA have to support std which may potentially cause some security issues as Substrate did not. Of course, we cannot sacrafy security in TEA Project, so we design the following complicate model to minimize the risk

##  Wasm model as "[[actor]]" and run inside [[a mini-runtime]]

The mini-runtime can be considered a special designed virtual machine. It loads [[actor]]s into isolated spaces. Although they are full features "std" wasm but still limited heavily by the mini-runtime.  Because mini-runtime runs inside the hardware [[enclave]], the enclave is a special designed linux mini core OS. We removed most of the common linux components to reduce the attack surface, such as network, file system are all gone. If any malicious code happent to run inside the enclave (we do not how it can, but just assume), it will not cause further damage since there is no network or file system to break. 
 
## Providers and capabilities
 
If the actor want to access anything outside the enclave, it has to call [[provider]]s and preset [[capability]]. If an actor code is not signed with the specific [[capability]] by the original developer, or the code try to call the provider unexpected, it will be rejected and result in furtuer panelty.  

If a hacker modified an actor execution binary code.

First, the CID won't match. The mini-runtime cannot load.

If it can break the first barrier, the capability signature check will fail, since the developer did not sign this actor with unauthorized capability. 

If somehow it can break this barrier too, and somehow it gain this capability and can successfully call the provider inside the mini-runtime. The provider has additional check before invoke any important function. Those additional check include (most commonly) the caller actor ID, destination peer_id, destination actor_id, traffic pattern etc. It will reject to any suspecious function call and report to Remote Attestation.

The providers are currently written by the TEA Project team, not the 3rd party open source developer. Upgrading [[providers]] are a very sensitive workflow to prevent any malicious code get introduced to the [[mini-rumtime]]. 

## Last barrier, the [[birth_control]] policy

If somehow the hacker has compromise less than 1/3 mini-runtime and malicious providers are introduced. As long as there are still more than 2/3 good nodes running. After few seconds, the next sync will cause state mismatch error. Due to BFT consensus, those 1/3 nodes will be quarantine and eventually removed from the network followed by slashing deposit. 

So as long as we keep the [[birth_control]] to grow the nodes slowly. Always keep good nodes over  2/3, the system could be secured.

