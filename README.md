# FAQ

### What is warp.green?

warp.green is a protocol that allows messages to be passed between supported chains (Chia and Ethereum & Base) via a trusted set of validators. Any app can integrate with the protocol to communicate across blockchains. At launch, two basic apps were also provided: an ERC-20 bridge and a CAT bridge.

### Who audited you?

\[todo: evm + clsp + link to repo yet to be created]

### What are the fees associated with bridging?

There are three main 'fees' that users have to keep in mind:

* **Network transaction fees**: Each bridging operation involves transactions on the source and destination chains (one on each, plus a possible token spend approval transaction on EVM chains). Users initiate both transactions and have to pay the associated network fees.&#x20;
* **Message tolls**: In order to prevent spam, the user is expected to pay low amounts of ether/chia when sending a message. These tolls are intended to prevent spam and get redirected to the block builder (e.g., miner/farmer). Please note that tolls may be updated at any time.
  * For messages originating from Chia, the toll is `0.001 XCH`
  * For messages originating on Base/Ethereum chains, the toll is `0.00001 ETH`&#x20;
* **Bridge tips**: Bridge contracts redirect a small portion of the funds (0.3%) to the protocol.

### Who are the validators?

The validators that secure the warp.green protocol are:

* \[list of validators will come here]

Please note that validators operate through several legal entities; the list above is only intended to give a sense of the people and teams maintaining them.

### What are the risks associated with bridging?

When bridging, you're exposed to multiple risks. Among them, we'd like to outline:

* **Underlying blockchain risks**: One of the involved chains might experience consensus failure. This would likely lead to assets that cannot be unwrapped.
* **Smart contract / puzzle risks**: A vulnerability in the puzzles, smart contracts or overall design might allow malicious actors to steal user funds. To minimize this risk, we have undergone audits for both sides of the bridge - see questions above for more information.&#x20;
* **Validator risk**: When relying the warp.green protocol, you're trusting that a majority of validators are honest and have not been compromised. For a list of the validators, please see the questions above.
