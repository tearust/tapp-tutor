In TEA Party, the messages are stored in [OrbitDb](orbitdb.org).
OrbitDB is a non-relational database on top of IPFS.

# Security
For those public messages, there is no need to encrypt.
They are stored in Orbit DB (eventually in IPFS ) in plain text.

But for those private messages, no plain text is saved. The hosting nodes will encrypt the message using the [[App AES Key]], then save the cypher to the OrbitDB. 

Because only TEA Party hosting nodes have such [[App AES Key]] in their own enclave, other apps or users cannot get the content of these messages.

## Cost
Orbit/IPFS is very cheap comparing the [[State Machine]], since the state is stored in RAM only.


