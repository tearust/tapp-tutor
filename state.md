The wiki defines state like this https://en.wikipedia.org/wiki/State_(computer_science)
> In [information technology](https://en.wikipedia.org/wiki/Information_technology "Information technology") and [computer science](https://en.wikipedia.org/wiki/Computer_science "Computer science"), a system is described as **stateful** if it is designed to remember preceding events or user interactions;[[1]](https://en.wikipedia.org/wiki/State_(computer_science)#cite_note-1) the remembered information is called the **state** of the system.
> 

In a traditional cloud computing architecture, the database is most likely used as the state storage. In blockchain world, the whole blockchain is a giant distributed state machine. For example, the Ethereum itself is a state machine. Everytime clients send transactions to update the state. Every new block means a new updated state is released. 

In TEA Project, we do not store the application state in blockchain. Instead, the state is stored in a group of [[State_Machine_Replica]]. We use a new [[Proof_of_Time]] hardware consensus to get super fast (relatively to traditional blockchain) processing power without scrafice security and scalability.  