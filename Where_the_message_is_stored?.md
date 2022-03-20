In the TEA Party, the messages are stored in an [OrbitDb](orbitdb.org) database.
OrbitDB is a non-relational database running on top of IPFS.

Read this for the detail of [[README#Storage| all storage options]]

# Security
For TEA Party's public messages, there's no need to encrypt.
They're stored in OrbitDB (and eventually in IPFS ) in plain text.

But private messages aren't saved in plain text. The hosting nodes will encrypt the message using the [[App_AES_Key]] and then save the cypher to OrbitDB. 

Because only TEA Party hosting nodes have the [[App_AES_Key]] in their own enclave, other apps or users cannot get the content of these messages.

## Cost
Orbit/IPFS is very cheap compared to using the [[State_Machine]] which uses only RAM to store the state.

