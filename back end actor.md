Use TEA party as example, the https://github.com/tearust/tapp-sample-teaparty/tree/demo-code/party-actor compiles to the [[actor]] that runs inside a [[hosting CML]]. 

Since it is an [[actor]], it is loaded and run inside the [[enclave]](or called [[mini-runtime]]).

The only thing that back end actor does is to handle incoming request.

In the lib.rs file, you can find the `handle_adapter_http_request` function. All these branches are messages that this actor can handle.
