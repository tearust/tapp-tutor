# Existing Layer2
Since Blockchain was invented, genies never stop improving the technology trying to make blockchain run faster and cheaper. Two major directions
- Improve the consensus: PoW -> PoS and other PoSomethingElse
- Off chain computation: Layer2

Basically, what the typical layer2 does
- Collect a bunch of txns from layer1
- Execute those txns off chain in layer2
- Send the result back to layer1.
- Layer1 run some kinds of verification then accept the result and update the blockchain

The main problems here are
- A layer2 has to be tightly bundled with a specific layer1
- Verification is not cheap (i.e., ZKP). Possible resolver would be either new algorithm invented (unlikely) or hardware accelerator(ASIC)

# A new type of Layer2
Can we make a new layer2 that
- Agnostic of any type of layer1. 
- Above and across all major blockchains 
- Minimized or zero verification

Let's think out of box by make a series of PoV changes

# Verify result -> Verify envionrment
If I ask you to verify the correctness of 82986.862 x 916019.1128 = 76017551703.3, what would you usually do? 
I bet very few people will use pencil and paper to verify. If there is, how about Sqft(76017551703)? Well, I am pretty sure most of us just pick your phone and use the calculator or some kind of app to run such a formula. If the result shows correctly as I said, you verify it successfully. 

Hold on! How do you know the reuslt of your calculator was correct? Have you recalculated manually? No, most likely you have not. Why? Because you trust Steve Jobs and his iPhone? Have you ever thought of this is a blind trust? 

It might be a blind trust, but most of us accepted this kind of trust because we trust the enviroinment. Because it is an iphone, or it is a CASIO. They are mass produced and widely used, reliable computation environment. If someone still doubt, they can just buy a few calculators from different brand, and run the same task to see if the result matches. 

Let's move one step further. Assuming the calculator is another layer2 node. In what circumastances you can believe the result from that node without running the computing yourself again? Let's see, if 
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

