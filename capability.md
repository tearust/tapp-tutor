**Capability** is a property when build an TApp actor. The developer need to sign the actor executable with a list of capabilities. Each capability is pass key to call a specific provider when it is loaded into the [[mini-runtime]].

The list of capabilities of any actor is a public information. If can be verified by all TEA DAO member. 

If an actor is not signed with a specific capability, even there is code in this actor trying to call the specific provider API, it will be rejected. Furthermore, a violation will be reported, that may cause further punishment. 

The developer who sign the capability will need to carefully inspect if this capability is absollutely necessory. The more capability he signs, the more security concern other users may raise.

