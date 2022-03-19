This is a typical JS application (for webapp), or mobile application (for mobile app). They're running inside of a browser or mobile device.

For every TApp, the front end code is a bunch of js/html/css code that stored in IPFS(or other decentralized storage). The CID (Content ID, actually the hash of the content) is the key to retrieve the content.

When the end user load a TApp, it actually load the static resource using CID from one of IPFS nodes.  

The front end must be **static** resource. All dymanic content will be query from the [[hosting CML]]. 