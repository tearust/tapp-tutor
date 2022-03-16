# What is TEA Party

TEA Party is a demo application running on TEA Project. 

We build TEA Party in order to demostrate 

- What a typical TApp looks like

- The building blocks of a typical TApp

- How to use Tea Party as a boilderplate to build your own TApps

  

TEA Party is a useful social media application. User can post message to public board, send private message with notification.

  

As all other TApps, TEA party shows the typical specities beyond other cloud based internet application (web2.0) applications. There is a centralized server(s) hosting the app. The individual miners host this application sole by their own profitability consideration. Because of this decentralization mother nature, all TApps include TEA Party:

- Cannot be turn off by any super power. As long as there are a minimal number of miners host this application, it will run forever

- No one, including the host miner can control sensorship of the content. The content is owned and protected by their creator's private key. A miner can choose to stop hosting but cannot selectively choose what content to show or hide.

- There is no free lunch. Every action that cost any computing resources need to get paid by someone. In TEA Party's particular case, every message need to be paid to send, to store, to notify receipiant. 

  

In order to get the features above, the under technical layer is very different from existing cloud computing and blockchain. It is a new tech stack based on later 2020's technologies. 

  

We will go through the cutting edge technologies in TEA Party, and tutor you around and help you make your own TEA Applications (TApp).

  

# Three major parts
All TApps has three major parts that running in three different locations.
## Front end 
This is the typical JS application (for webapp), or mobile applicaiton (for mobile app). They are running inside a browser or a mobile devices.

## Back end [[actor]]
This WebAssembly code is running inside a hosting nodes. The hosting node is a miners computer which has CML planted. It is similar to the server logic running in backend servers or application servers in traditional cloud computing architect.

## State machine [[actor]]
This WebAssembly code is running inside the state machine 's [[mini-runtime]]. It is similar to the stored procedure (SQL for example code) in traditional 3 tier architect's database.

## [[3-tiers architect]] basic workflow
The above 3 components are directly mapped to the traditional 3 tier architect in the cloud computing applicaiton.

The basic workflow would be this: (let's use web tapp for example)
- The user traffic user action in front end. Javascript web client catch the user action, generate a web request sending to the backend
- Backend receives the web request, running the Tea Party back end code (we call it backend actor) to handle anything that do not need state machine(traditional we call it database). But when it will need to query or update a state in the state machine, it will need to genreate a request to the state machine tier. Those are query (do not change state) and Command (potentially change the state)
- The query and command is handled by the state machines replications. For querys, it will look up local state and send result back. For command, as one of the replications, it should not modify on their own, Instead, it generate a txn and put it in a global queue, we call the queue the [[conveyor]]. The replicas run a Proof of Time consensus to garantee that all state machine in all replicas get the same order of txns, so that their state can always keep identity after execute the command. This is the same as a typical distributed database system.
# The requiements of building TApps
In this section, we will list the knowledge and tools you will need to build TApps.
## Tools
## Programming languages
## Architect knowledge
## Hardware

# Code walk through
In this section, we will walk through the tea party application sample code. 
The steps are:
- Clone the code to local
- Install building tools
- Understand the folder structure
- Understand the compiling workflow
- Run it
# Basic workflow
In this section, we will learn the basic workflow between all the three tiers. How a user action get processed from the front end to the state machine layer and back to the user.

# The magical Proof of Time state machine
In this section, we will explain how the distributed state machine works. How it handles the consensus from different replicas

# Understand WebAssembly Runtime
In this section, we will go through how the WebAssembly code runs inside the [[mini-runtime]. 
]