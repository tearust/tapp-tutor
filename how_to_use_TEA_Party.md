TEA Party user guide (epoch 9, March, 2022): https://teaproject.medium.com/tea-party-tapp-epoch-9-users-guide-2bd8ddd87daa


Youtube video tutorial:
<iframe width="560" height="315" src="https://www.youtube.com/embed/yl7DUnyE_0g" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# Why TEA Party is a typical Web3 application?
Before we get into this question, please read [[What_makes_a_Web3_application]]? first.

Web3 applications are different than web2 application in multiple ways
- No centralized hosting services
- No centralized database
- No centralized authority and sensorship
- Tokenomic driven

## No centralized hosting
When you start a TEA party, you probably have noticed that it is different than a regular web2 application. You are not going to a domain name such as `teaparty.com`. Instead, you are given several URL with different IP addresses. Such as the screen shot below.

![urls](https://user-images.githubusercontent.com/1761809/160294873-a61c21b8-e8ee-4cbf-bc41-05ae097a47bb.png)


You will noticed that the IP addresses are different, but the rest are identical. Those identialh part is the IPFS CID of the current version of TEA Party [[front_end]]. The IP addresses are the IP of [[hosting_CML]]. In the screenshot above, you will know there are three TEA mining machine (with [[hosting_CML]] planted) currently hosting this TEA Party application. 
There is no **"domain name" and "server"** concepts in TApps.  As you can see, any one (for example yourself) can also become a host and earn the hosting fee. There is no centralized control over this. On the other word, if some super power does not like this application, there is no way for them to off shelf this application. Say bye bye to cloud server hosting.

## No sensorship

The code and data are running inside a hardware protected [[enclave]]. No one, including the developers and the hosting miners can see what is actually inside. Therefore there is no way to run any kind of sensorship. If any individual miner does not like this TApp, he can stop hosting this TApp. But as long as other miners hosting this TApp, it is still accessable and content are still available.

## No centralized database
Not only the hosting is fully decentralized, the database is fully decentralized too. You probably cannot see this from the front end. But if you read the full developer document, you will understand how [[State_Machine]] and [[OrbitDb]] work together to [[get_started#Storage| store]] the application data.

## Tokenomic driven
In Web2.0, end users sell their privacy to the tech giants in return for the "free" application service. In Web 3.0, because there is no centralized "operator" to steal your privacy for profit, there won't be any free lunch provided.  Someone has to pay for the service as the miners and developers need to make a living. Do not think this is bad, this is actually what things should work.  Because all the API call to the [[public service]] need to be paid, all [[get_started#Storage]] needs RAM or hard drive space. Those are eventually paid by end users. So you can see the price of posting message or sending private notifications. 

TODO: Screenshot

There might be some new business model that end user can have "free" services in exchange of some work-to-pay or data-rent-to-pay. We will continue trying those models in our future version of TEA Party.
