[[State Machine]] is the database tier that contains multiple replications. Every replication is a **State Machine Replica**.

Every replica is a standlone state machine CML node. It sync with other state machine CML nodes. 

Our [[Proof of Time]] algorithm to guarantee that all state machine replicas run all transacctions (or called commands ) at the same sequence, therefore, the state will be kept the same along all replicas.
