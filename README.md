# FAQ

### What is warp.green?

warp.green is a protocol that allows messages to be passed between supported chains (Chia and Ethereum & Base) via a trusted set of validators. Any app can integrate with the protocol to communicate across blockchains. At launch, two basic apps were also provided: an ERC-20 bridge and a CAT bridge.

### What does 'Beta' mean?

Almost 300 tests check if our Solidity contracts behave as expected, and simulated bridging operations ensure our puzzles work correctly. We've been on testnet for a while, and have gone through a round of security audits. That being said, the bridge is a very important piece of infrastructure, so we'd like to be on the cautious side.

Beta means that more audits are coming to both sides of the bridge (see question below). It also means that we're monitoring everything  very closely to catch any bugs that might've not been fixed during the testnet phase. Validators are still experimenting with their infrastructure, so the bridge might also experience some downtime during beta.

### Are you secure?

Security is a continuous effort. For this bridge, two aspects are important:

* **Validators**: Compromising a majority of validators (i.e., 7 out of 11) could allow a malicious actor to take control of the bridge. During the selection process, technical ability and experience with highly-secure systems played a major role in the parties that were invited to become validators.&#x20;
* **Code**: Alternatively, an exploit could lead to warped assets losing their peg. Before mainnet launch, Chia Network, Inc. has reviewed our chialisp puzzles, and our solidity contracts have gone through [an audit](https://hacken.io/audits/warp.green) from Hacken, a firm specializing in blockchain security. We're also running a continuous [bug bounty program](https://github.com/warpdotgreen/cli/blob/master/SECURITY.md). Before getting out of beta, we're going to go through one extra audit for each side of the bridge (EVM and coinset).

### What are the fees associated with bridging?

There are three main 'fees' that users have to keep in mind:

* **Network transaction fees**: Each bridging operation involves transactions on the source and destination chains (one on each, plus a possible token spend approval transaction on EVM chains). Users initiate both transactions and have to pay the associated network fees.&#x20;
* **Message tolls**: In order to prevent spam, the user is expected to pay low amounts of ether/chia when sending a message. These tolls are intended to prevent spam and get redirected to the block builder (e.g., miner/farmer). Please note that tolls may be updated at any time.
  * For messages originating from Chia, the toll is `0.001 XCH`
  * For messages originating on Base/Ethereum chains, the toll is `0.00001 ETH`&#x20;
* **Bridge tips**: Bridge contracts redirect a small portion of the funds (0.3%) to the protocol. A minimum tip of 1 mojo is enforced by the contracts, which means that the minimum total transfer amount is 2 mojos worth of any token.

### Who are the validators?

The validators that secure the warp.green protocol are:

* yakuhito from TibetSwap & FireAcademy.io
* Bufflehead (The dexie Team)
* The SpaceTime Team
* The SpaceScan.io Team
* The Chain HQ Team
* The Ozone Wallet Team
* tjump from XCHscan.com
* M.G. from Primer Security
* The MIDL.DEV Team
* GIRITEC.COM
* The Goby Team

Please note that validators operate through several legal entities; the list above is only intended to give a sense of the people and teams maintaining them.

### What are the risks associated with bridging?

When bridging, you're exposed to multiple risks. Among them, we'd like to outline:

* **Underlying blockchain risks**: One of the involved chains might experience consensus failure. This would likely lead to assets that cannot be unwrapped.
* **Smart contract / puzzle risks**: A vulnerability in the puzzles, smart contracts or overall design might allow malicious actors to steal user funds. To minimize this risk, we have undergone audits for both sides of the bridge - see questions above for more information.&#x20;
* **Validator risk**: When relying the warp.green protocol, you're trusting that a majority of validators are honest and have not been compromised. For a list of the validators, please see the questions above.
