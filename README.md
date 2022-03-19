**TEA Party** is a demo application running on the TEA Project. 

We built TEA Party to show: 

- What a typical TApp looks like.

- The building blocks of a typical TApp.

- How to use Tea Party as a boilerplate to build your own TApps.

The TEA Party TApp is a useful social media application. Users can post messages to a public board as well as send private messages with notifications. See [[how to use TEA Party]]

As is the case with all TApps, TEA Party showcases the special features that are beyond the capabilities of other cloud based internet (web 2.0) applications. Instead of centralized server(s) hosting the app, the individual miners of the TEA network host TApps based solely on their own [[hosting profitability]]. The inherent decentralization that all TApps including the TEA Party share gives these apps even more unique features:

- They cannot be turned off by any centralized power. As long as there are a minimal number of miners hosting any particular application, it will continue to run forever.

- No one, including the host miner, can control or censor the content. The content is owned and protected by its creator's private key. A miner can choose to stop hosting the TApp, but it cannot selectively choose what content to show or hide.

- There's no free lunch. Every action that costs any computing resources needs to be paid by someone. In TEA Party's particular case, every message sent need to be paid for. Additional charges also apply to store the message or to notify the recipient.  Further link [[Where the message is stored?]]

In order to get the features above, the underlying technical layer is very different from the existing cloud computing and blockchain tech stacks. It's a new tech stack that's based on recent technologies. 

The following sections will explain the cutting edge technologies used in the TEA Party. We hope by explaining the underlying technologies and how they work together will help you make your own TEA applications (TApps).

# Three Major Parts
All TApps have three major parts that run in three different locations.

## Front-end 
This is a typical JS application (for webapp), or mobile application (for mobile app). They're running inside of a browser or mobile device.

## Back-end [[actor]]
This WebAssembly code is running inside of a hosting node. The hosting node is a miner's computer which has a CML planted. It's similar to the server logic running in backend servers or application servers in the traditional cloud computing architecture.

## State machine [[actor]]
This WebAssembly code is running inside the state machine's [[mini-runtime]]. It's equivalent to the stored procedure (SQL for example code) in the traditional 3-tier architecture's database.

## [[3-tiers architect]] basic workflow
The above 3 components are directly mapped to the traditional 3-tier architecture in the cloud computing application.

The basic workflow would be this: 
(let's use a web-based TApp for the example)

- The user generates a user action in the front-end. The Javascript web client catches the user action, generates a web request, and sends it to the backend.
- The back-end receives the web request and runs the Tea Party back-end code (we call it the back-end actor) to handle anything that does not need the state machine (traditionally, this is referred to as a database). But when it needs to query or update a state in the state machine, it will need to genreate a request to the state machine tier. These can be broken down into [[queries]] (will not change the state) and [[commands]] (potentially could change the state).
- The queries and commands are handled by the state machine replications. For queries, it will look up the local state and send the result back. For commands, as one of the replications, it should not modify on its own. Instead, it generates a txn and puts it in a global queue that we call the [[conveyor]]. The replicas run a Proof of Time consensus to guarantee that all state machines in all replicas get the same order of txns. This ensures that their state can always be kept identical after executing the command. This is the same methodology as is typically used by a distributed database system.

# Comparison with cloud webapp 3-tier architect
| User action | step  | Eth based dApps |cloud webapp | TEA project |  Note |
|-------------|-------| ------|---------------|-------------|-------|
| Click the app to start | Start a web app | N/A | Go to a domain name, usually https://yourapp.com | Click the app name in your TEA wallet, you will receive a list of hosting CMLs, click any of them | Cloud webapp has a centralized http/https domain name, but TEA doesn't have such a centralized control. Every miner hosting is seperated |
| Show the UI in browser | Load front end code to browser | N/A | Download the [[front end]] code (js/html/css) from a webserver | Download the front end from IPFS or any decentralized storage | TEA doesn't have a traditional web server. The front end code and all static resources are stored in IPFS or other decentralized storage. User use the CID (hash) as a key to load the fornt end code to browser |
| Show dyanmic content, such as List of all messages | Query database | Any client to query block state | Browser send request to back end server, back end server then query database for data. Send data all the way back to browser to show on the UI | Browser request to hosting CML. The [[back end actor]] handle the request then send P2P request to [[State machine replica]]. [[State machine actor]] query the [[state machine]] then send the data all the way back to [[front end]] | Depends on what type of content the UI query. Some content can be directly query from hosting CML local OrbitDB. Accounting information needs go to state machine. TEA project also provide a Glue SQL if the data is stored in SQL database. |
|  Create or update dynamic content, such as post new message or extend existing message | Send command to modify state | Send transaction to any Eth miner and wait for new block |The same as above | [[Front end]] send command to [[back end actor]]. [[Back end actor]] generate a transaction (or call command) send to [[State Machine Replica]] via P2P. Statemachine replica put this transaction into [[conveyor]] then wait a grace period until the sequence of transaction reach consensus between more than 50% replcas. Then load this transaction to [[Back end actor]] to execute the transaction to update state | There are many state machine replicas that keep a consistent state among them. So the Proof of Time is required to sync between replicas. |



# Requirements of building TApps
In this section, we'll list the knowledge and tools you'll need to build TApps.

## Tools
To build and run the demo locally, you will need:

- a Linux or Mac. 
- Docker and docker-compose installed.
- Rust compiler.
- Web browser

If you want too build your own TApp and try to host your own app in your own mining node. you will also need a mining node. A mining node is any type of TEA node with CML planted in it. If you don't own a physical TEA machine, you can rent an Amazon Nitro VM which is TEA-compatible.

## Programming languages

The demo TEA party app is written in the following languages:

- **Front-end** is written in JS and the Vue framework.
- **Back-end and State machien actors** are written using Rust and then compiled to WebAssembly.

The TEA Project doesn't require the developer to use the Rust programming language. You can use any programming language that compiles to WebAssembly. But at this moment, in order to understand our existing demo code, you'll need to use the Rust langauge. 

## Architecture knowledge
## Hardware
The TEA Project is very different from many other blockchain projects. TEA relies on two types of hardware in order to reach special consensus: 

- TPM
- GPS

The TPM is a popular security chip that exists in almost every computer and most phones. We rely on each node's TPM to generate Proof of Trust data that can undergo remote attestation by a verifier.

We need GPS but we don't use this for navigation. Our TEA nodes are stationary and not moving on the street. Instead of location, we use the acurate time stamps from GPS satellites. Every GPS satellite has an atomic clock, and it constantly sends time signals to any terrestrial GPS receiver. We use this as the source of the time stamps for all events.

If you just want to run the code in your local simulator, you don't need such hardware. You can run it using docker and our simulator.

If you want to host your application in the production environment, you will need a TEA node. If you do not own one, the easiest way is to rent a Amazon Nitro VM.

# Code walk through

In this section, we'll walk through the TEA Party application's sample code. 

The steps are:

- Clone the code to local.
- Install the build tools.
- Understand the folder structure.
- Understand the compile workflow.
- Run it.
## Code location and structure
https://github.com/tearust/tapp-sample-teaparty
Please clone the above github repo to your location repo.

There are 4 folders
- party-actor: This is the [[back end actor]]
- party-fe: This is the [[Front end]]
- party-share: This is the common data structure or library that shared by both [[back end  actor]] and [[state machine actor]]
- party-state-actor: This is the [[statemachine-actor]]

Please click the above link for more detail.

# Basic workflow
In this section, we'll learn the basic workflow between all three tiers. How a user action get processed from the front-end to the state machine layer and back to the user.

# The magical Proof of Time state machine
In this section, we'll explain how the distributed state machine works, including how it handles consensus among different replicas.

# Understand WebAssembly Runtime
In this section, we'll go through how the WebAssembly code runs inside the [[mini-runtime]]. 
