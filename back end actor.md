Using the TEA Party TApp as an example, the https://github.com/tearust/tapp-sample-teaparty/tree/demo-code/party-actor compiles to the [[actor]] that runs inside a [[hosting CML]]. 

Since it's an [[actor]], it's loaded and runs inside the [[enclave]] (also called the [[mini-runtime]]).

The only thing that the back-end actor does is handle incoming requests.

In the lib.rs file, you'll find the `handle_adapter_http_request` function. All of these branches are messages that this actor can handle.
