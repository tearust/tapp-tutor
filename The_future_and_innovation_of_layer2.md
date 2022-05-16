# Existing Blockchains Use Layer2s for Scalability
Ever since Blockchain was invented, many projects have tried improving the technology to make blockchain run faster and cheaper. To make blockchains more scalable, there are two primary innovation paths:

- Increasing scalability through improving the consensus: PoW -> PoS and other Proof of Whatever.
- Increasing scalability through off-chain computation, i.e. a layer2.

Improving consensus can only go so far as nodes inherently have to wait for each other to reach consensus while making allowances for Byzantine fault tolerance. That leaves layer2s as an area of focus for achieving scalability. Basically, a typical layer2 does the following:

- Collects and batches txns from layer1.
- Execute those txns off-chain in layer2.
- Send the result back to layer1.
- Layer1 runs some kind of verification, then accepts the result and updates the blockchain.

The main problems with layer2 are that:

- Each layer2 has to be tightly bundled with a specific layer1.
- Verification is not cheap (e.g., ZKP). Possible resolver could be created either through inventing a new algorithm (unlikely) or hardware acceleration (ASIC).

# A New Type of Layer2
Although layer2s seem like a promising start towards scaling blockchain, there is still room for improvement. 
Can we make a new layer2 that is:

- Agnostic of any type of layer1. Many innovative layer2s are currently tied to specific blockchains giving them only niche applicability in context of the entire crypto ecosystem.
- Able to run above and across all major blockchains. If this new layer2 could run across any blockchain, then we have also solved another thorny problem currently in crypto, that of bridging funds from one chain to another.
- Able to verify results with minimal or even zero verification. Calls back to layer1 to verify the result are a bottleneck that incurs conventional consensus and its typical transaction fees. What if we could verify the result without involving layer1 at all?

# Verify the Result by Verifying the Environment
If I asked you to verify the correctness of 82986.862 x 916019.1128 = 76017551703.3, how would you go about doing so? 
I bet very few people would use pencil and paper to verify the result. You could counter that it's technically doable, so let's try something harder like verifying that the Sqrt(251666496) = 15864. Well, I'm pretty sure most of us would just pick up our phone and use the calculator or some kind of app and key in the problem. If the result shows correctly, you'll likely be confident that you've verified it successfully. 

Hold on! How do you know the result of your calculator app was correct? Have you recalculated it manually? No, most likely you have not. Why? Because you trust Steve Jobs and his iPhone? Have you ever thought of this is a blind trust? 

It might be a blind trust, but most of us accept this kind of trust because we trust the environment. Because it's an iPhone or it's a CASIO, they're all mass-produced and widely used, reliable computation environments. Of course doubters can buy a few calculators from different brands to run the same calculation themselves to see if the result matches. 

Let's move one step further. Assuming the calculator is another layer2 node. In what circumstances you can believe the result from that node without running the computing yourself again? Let's see, if 
- The integrity of that node can be remotely verified. We know it runs in a trustable condition
- There are several randomly selected nodes running the same task seperately, and get the same result.

Can we trrust the result? Hmmmm... Well, you may say there are still a little corner case that
- All of the nodes were infected by some kind of virus but cannot be detected remotely, or 
- Somehow they can collute together and send me the same wrong answer.

Understood, if we can use technologies to reduce the chance to be so little that can be toleranced, we would be OK to accept. Car accidents happen all the time, but we still drive, right?

Now you can see, instead of verify the result by recalculate, we just trust the process. The process means
- The environment can be trusted (i.e. the brand of calculation)
- The input code and data are correct. (i.e. Hash of those data and code are verified)
- Multiple randomly selected (uncollutable) nodes get the same result seperately.

# Benefits of switching to verifying environment
## Cost effective
As the example I showed just now. Verify a brand of a calculator is much easier than recalculating manually (on pencil and paper). Even adding on the cost of multiple brands of calculators to run the same formula, it is still much cheaper. 

If we extend this methodology to the layer2 solutions. Instead of verifying the rollup results from layer2, we just verify the integrity of layer2 nodes and "blind" trust the result and put it to the blockchain. This change will significantly reduce the cost and complexity of layer1 verification.

## Layer1 agnostic
One step further, if verification of the integrity of layer2 nodes can be done agnostic of the types of layer1, this layer2 would be possible to run on any kind of layer1. For example, we can have the same layer2 solution run on top of Ethereum, Polkadot, Cosmos, BSC... as long as the layer1 support basic touring completed smart contract. This can make the layer2 solution a blockchain agnostic, above and run across all major blockchains.

## General-purpose computing
One more step future. Since the layer2 is agnostic of layer1, there is no limit on what kind of txns it can process. General-purpose computing would be available in layer2. In this case, we do not really care if the layer1 blockchain is Solidity-based EVM or Rust, Wasm,ink etc. They are totally inrelavent. The layer2 can run a general-purpose functions which might be totally unrelated to the under layer1. For example, you can have a Tensorflow task on layer2 without any problem, although we never expect Solidity to run Tensorflow. 

## Cloud computing as an Oracle
Since we can run general-purpose computing, running a cloud computing Oracle in layer2 won't be hard. As long as the environment can be trusted, we can have a distributed cloud-based SQL database running on the layer2. Using SQL to write a smart contract is no longer a dream!

# How? Controversial hardware integrity
Hardware integrity remove attestation is always controversial.Just like the arguments between pure cryptographic algorithm vs hardware trusted execution environment. There are always pros and cons. My opinion is before we can find a perfect math solution, using hardware integrity remote attestation is always a cost-effective compromised solution. As I said before, car accidents happen every day, we still drive. 

Let alone blockchain, hardware integrity remote attestation was in the IT industry for decades. All cybersecurity engineers know the Trusted Computing technologies has been used in every computer and most phones. That is how Microsoft and Apple knows that your computer is jailbroken or hacked by boot virus. How it works is that all those hardware has a Trusted Computing Secure Chips called TPM to record hashes of your hardware before OS was loaded. Then report those hashes to Microsoft and Apple and validated there. No doubt this is centralized remote attestation. In the blockchain world, we do every decentralzied, so we just use other nodes as remote attestators. They are randomly selected by a VRF in layer1 blockchain to reduce the chance of collution. All those remote attestators make their own decision seperatedly and report their decision back to the layer1 blockchain. A simple voting in the smart contract can determine a testee is trusted or not.

Besides Trusted Computing technology, TEE is also widely used nowaday, such as Intel SGX, ARM Trust zone. The only cons of TEE is that they are tightly bonded to CPU manufacturer that causes new kind of centralization. 

# Conclusion and future of layer2
I believe the future of layer2 should be a standalone general-purpose cloud computing oracle, that run on top of multiple blockchains. Majority of computing tasks can be elevated from layer1 to layer2. Because layer2 do not need expensive ZKP when sending result back to layer1, the overall cost is much lower. Since layer2 is verified trusted by layer1, there is no blockchain type of consensus needed, the performance can catch the traditioanl cloud computing. In TEA Project, we are benefit of the advantage of hardware trust. We use the timestamp directly received from GPS satelites to run a  "Proof of Time" distributed state machine in trusted layer2 nodes. This is similar to Google Spanner database, but fully decentralized. Therefore, TEA Project is a decentralized cloud computing platform that runs so-called TApps. TApps run like typical web apps or mobile apps, but there is no centralized application server or database server behind. No domain name or Verisign certificate. They are unstoppable, unbreakable, sensorship free. 

