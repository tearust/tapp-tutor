**TEA Party** is a demo application running on the TEA Project. 

We built TEA Party to show: 

- What a typical TApp looks like.

- The building blocks of a typical TApp.

- How to use Tea Party as a boilderplate to build your own TApps.

The TEA Party TApp is a useful social media application. Users can post messages to a public board as well as send private messages with notifications.

As is the case with all TApps, TEA Party showcases the special features that are beyond the capabilities of other cloud based internet (web 2.0) applications. Instead of centralized server(s) hosting the app, the individual miners of the TEA network host TApps based solely on their own profitability. The inherent decentralization that all TApps including the TEA Party share gives these apps even more unique features:

- They cannot be turned off by any centralized source of power. As long as there are a minimal number of miners hosting any particular application, it will continue to run forever.

- No one, including the host miner, can control or censor the content. The content is owned and protected by its creator's private key. A miner can choose to stop hosting the TApp, but it cannot selectively choose what content to show or hide.

- There's no free lunch. Every action that costs any computing resources needs to be paid by someone. In TEA Party's particular case, every message sent need to be paid to be paid for. Additional charges also apply to store the message or to notify the recipient. 

In order to get the features above, the underlying technical layer is very different from the existing cloud computing and blockchain tech stacks. It's a new tech stack that's based on recent technologies. 

The following sections will explain the cutting edge technologies used in the TEA Party. We hope by explaining the underlying technologies and how they work together will help you make your own TEA applications (TApps).

# Three Major Parts
All TApps have three major parts that run in three different locations.

## Front-end 
This is a typical JS application (for webapp), or mobile application (for mobile app). They're running inside of a browser or mobile device.

## Back-end [[actor]]
This WebAssembly code is running inside of a hosting node. The hosting node is a miner's computer which has a CML planted. It's similar to the server logic running in backend servers or application servers in the traditional cloud computing architecture.

## State machine [[actor]]
This WebAssembly code is running inside the state machine's [[mini-runtime]]. It's similar to the stored procedure (SQL for example code) in the traditional 3-tier architecture's database.

## [[3-tiers architect]] basic workflow
The above 3 components are directly mapped to the traditional 3-tier architecture in the cloud computing application.

The basic workflow would be this: 
(let's use a web-based TApp for the example)

- The user generates a user action in the front-end. The Javascript web client catches the user action, generates a web request, and sends it to the backend.
- The back-end receives the web request and runs the Tea Party back-end code (we call it the back-end actor) to handle anything that does not need the state machine (traditionally, this is referred to as a database). But when it needs to query or update a state in the state machine, it will need to genreate a request to the state machine tier. These can be broken down into queries (will not change the state) and commands (potentially could change the state).
- The queries and commands are handled by the state machine replications. For queries, it will look up the local state and send the result back. For commands, as one of the replications, it should not modify on its own. Instead, it generates a txn and puts it in a global queue that we call the [[conveyor]]. The replicas run a Proof of Time consensus to guarantee that all state machines in all replicas get the same order of txns. This ensures that their state can always be kept identical after executing the command. This is the same methodology as is typically used by a distributed database system.
- 
# Requirements of building TApps
In this section, we'll list the knowledge and tools you'll need to build TApps.
## Tools
## Programming languages
## Architect knowledge
## Hardware

# Code walk through
In this section, we'll walk through the TEA Party application's sample code. 

The steps are:

- Clone the code to local.
- Install the build tools.
- Understand the folder structure.
- Understand the compile workflow.
- Run it.

# Basic workflow
In this section, we'll learn the basic workflow between all three tiers. How a user action get processed from the front-end to the state machine layer and back to the user.

# The magical Proof of Time state machine
In this section, we'll explain how the distributed state machine works, including how it handles consensus among different replicas.

# Understand WebAssembly Runtime
In this section, we'll go through how the WebAssembly code runs inside the [[mini-runtime]]. 
