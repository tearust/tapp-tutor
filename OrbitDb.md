OrbitDb (https://orbitdb.org/) is a Peer-to-Peer Database for the Decentralized Web
> [OrbitDB](https://github.com/orbitdb/orbit-db) is a serverless, distributed, peer-to-peer database. OrbitDB uses [IPFS](https://ipfs.io/) as its data storage and [IPFS Pubsub](https://github.com/ipfs/go-ipfs/blob/master/core/commands/pubsub.go#L23) to automatically sync databases with peers. It’s an eventually consistent database that uses [CRDTs](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type) for conflict-free database merges making OrbitDB an excellent choice for decentralized apps (dApps), blockchain applications, and offline-first web applications.

In the TEA Project, every [[hosting_CML]] has an OrbitDb instance running **outside** of the enclave. 

OrbitDb is used to store large data blobs. OrbitDb provides limited non-relational database features but at a much lower cost. For a relational database, developers can use [[GlueSQL]]. For more essential account balance (money) data, we'd suggest using [[state]] instead.

See [[get_started#Storage| the storage section]] for a more detailed comparison.
