AuthKey is similiar to seesion in web2. It is used to identify users and their authroity of operation in TApps

# Why not using username/password
Almost all user activies happen in layer2 instead of layer1, but the user authentication happens in layer 1 only. The only thing we can determine user authentication is the user signature using blockchain wallet (such as Metamask or Ledger). You may ask why we cannot use the traditional username/password as most web2 application did. The reasons are:
- We try to make all users annoymously by not connecting user_id with any personal information, such as email/phone number
- Save password (even hashed) anywhere in layer2 may attract hacker attack
- Many users cannot get a strong password to protect themselves
- Many users forget their password and we can hardly help them to get back and reset due to annoymous nature

In this case, we would like to use end user's blockchain signature as the solo way to identify

# Operation auth in AuthKey
The AuthKey includes the allowed operation list. User will get promoted in the UI before sign the authkey on what operations are allowed. Such as allowance to spend by this tapp (max expense), Allowance to transfer, Withdraw etc.

Tihis operation list is a JSON string that signed using user's blockchain private key. [[hosting_CML]] will not verify this signature, but the [[provider]] inside [[State_Machine]] will. Even the developer made [[state_machine_actor]] allowed an operation to transfer user's fund, the [[provider]] of the [[state]] will double confirm this operation is allowed by the user again. In this case, even the developer is bad actor that tryig to steal user's money, it will be blocked by the [[State_Machine]] eventually. 

In other words, the design of TEA Project has assumption that the developers have motivation to steal user's money but the AuthKey logic inside  [[provider]] of  [[state]] prevent this from happening.

# How it work?
The tapp developer will set a AuthKey profile string (in JSON format) based what end users supposed to do after login. This profile should only include the necessary operations. More than required operations may cause end user to reject signing and even appearling to the DAO.

When user login this tapp, this string will be part of the to-be-signed string and show in the wallet UI. The end users should review these allowed operations before signing. 

The signing process is the same as using Metamask to sign an Ethereum transaction. The only difference is this signed string will not be sent to the blockchain, but to the [[hosting_CML]] instead.

Once signed, the user agree this tapp can do what has been allowed in this string. The signed string is sent to the [[State_Machine]] and stored there temporarily with a nonce handle. This nonce handle is called **AuthKey**. This AuthKey is returned to the [[hosting_CML]] and the user's browser. Every time this user want to run any [[queries]] or [[commands]] that require authorization, this AuthKey will be sent to the [[hosting_CML]] and eventually to the [[state_machine_actor]]. When the [[state_machine_actor]] actually call the [[provider]] of [[state]], the AuthKey will be checked before it can actually be allowed to run. If there is any voilation, an Error will be throw.

# AuthKey expiration
Like regular session in Web2. The AuthKey has expiration time. If user stop sending any activity for 30 minutes (this length of time is configurable), the AuthKey is expired. The end user has to re-login to create a new AuthKey to continue operation.

# AuthKey security
If a snifer get this AuthKey, he can impersonable the user to cheat the system. So the security of AuthKey is very important. In [[hosting_CML]] and [[State_Machine]], the AuthKey is always stay inside hardware [[enclave]]. During transportation between [[enclave]]s, TLS security is always applied. Between browser and the [[hosting_CML]], a special end-to-end encryption applied to keep the authkey secure. 
