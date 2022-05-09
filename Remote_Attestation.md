Remote attestaiton is one of the most improtant concept of **Trsuted Computing**, it is also the corner stone of the TEA Project.

If we send a bunch of code and data to a computer, how do it know the computer is doing what it is supposed to do and the result is trustable? As a human, we only can feel the external look of a machine but no way to figure out what the real firmware software running inside. What if a hacker has modified the firmware or software inside the machine, the computer will look exactly the same as it was before the breach. 

Trusted computing was invented to solve this problem. The computer itself can detect the integrity (for example **secured boot**) or detect another computer's integrity(this is called **remote attestation**).

Validation of integrity is basicly comapre the hash of a hardware/firmware/software stack with a series known correct hash values. If any of these values changes and longer match what they supposed to be, the remote attestatation failed. Those verifable hash values are provided by the [[TPM]] security chips.

The attestors are selected randomly by the layer1 blockchain. This is out of human control. Every individual attestor made its own decision seperately, and the result was sent to layer1. Layer1 smart contract runs a BFT algorithm to determine a tester is trustable or not. The attestors and layer1 works as members of jury duty and juedge. 

The detail of Trust Computing and Remote Attestation are beyond the coverage of this document. But it is very important and worthy a reading. 

For a quick overview of Trusted Computing please go to this [Stanford page](https://cs.stanford.edu/people/eroberts/cs201/projects/trusted-computing/what.html) or for more detail visit [the trusted computing group](https://trustedcomputinggroup.org/).

For more detail go to [tpm_key_attestation ](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)