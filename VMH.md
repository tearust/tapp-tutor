Virture Messaging Hub. This is a UDS (Unix Domain Socket) channel between enclave and outside parent instance. This is the only allowed information exchange tunnel. 

According to the enclave security rule, all the secret related information will need to be encrypted before sending out of enclave.

When sending message via VMH, a destination component name is specified. So that the cooresponding component will receive such message while other components won't.

