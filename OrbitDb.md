OrbitDb (https://orbitdb.org/) is a Peer-to-Peer Databases for the Decentralized Web
> [OrbitDB](https://github.com/orbitdb/orbit-db) is a serverless, distributed, peer-to-peer database. OrbitDB uses [IPFS](https://ipfs.io/) as its data storage and [IPFS Pubsub](https://github.com/ipfs/go-ipfs/blob/master/core/commands/pubsub.go#L23) to automatically sync databases with peers. It’s an eventually consistent database that uses [CRDTs](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type) for conflict-free database merges making OrbitDB an excellent choice for decentralized apps (dApps), blockchain applications and offline-first web applications.

In TEA Project, every [[hosting_CML]] has a OrbitDb instance running **outside** of enclave. 

It is used to store large data blob. OrbitDb provides limited non-relational database features but at much lower cost. For relational database, developers can use [[GlueSQL]]. For more essential account balance (money) data, we would suggest to use [[state]] instead.

See [[README#Storage| the storage section]] for more detail comparison.