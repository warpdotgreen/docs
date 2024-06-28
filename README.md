# FAQ

### What is warp.green?

warp.green is a protocol that allows messages to be passed between supported chains (Chia and Ethereum & Base) via a trusted set of validators. Any app can integrate with the protocol to communicate across blockchains. At launch, two basic apps were also provided: an ERC-20 bridge and a CAT bridge.

### What does 'Beta' mean?

Almost 300 tests check if our Solidity contracts behave as expected, and simulated messaging/bridging operations ensure our puzzles work correctly. We've been on testnet for a while, and have gone through a round of security audits. That being said, the bridge is a very important piece of infrastructure, so we'd like to be very cautious.

Beta means that we're monitoring everything  very closely to catch any bugs that might've not been fixed during the testnet phase. Validators might still change their infrastructure, so  some downtime might occur during beta.

### I've encountered an error while bridging - what should I do?

Refresh the page. If you want to resume an ongoing transfer, please go to [warp.green/explorer](https://www.warp.green/explorer), find it, and click 'Complete Relay'.

If you still have problems, please [contact us](users/contact-us.md).

### Are you secure?

Security is a continuous effort. For this bridge, two aspects are important:

* **Validators**: Compromising a supermajority of validators (i.e., 7 out of 11) could allow a malicious actor to take control of the protocol and apps built on top of it (e.g., bridges). During the selection process, technical ability and experience with highly-secure systems were a major consideration in the decision of whether to invite a party to become a validator or not.&#x20;
* **Code**: Alternatively, an exploit could lead to invalid messages being passed (in which case warped assets could lose their peg, and other apps using the bridge would also suffer). Before mainnet launch, Chia Network, Inc. has reviewed our chialisp puzzles, and our Solidity contracts have gone through [an audit](https://hacken.io/audits/warp.green) from Hacken, a firm specializing in blockchain security. We're also maintaining a [bug bounty ](https://github.com/warpdotgreen/cli/blob/master/SECURITY.md)pot with funds to be given for responsible disclosures. Shortly after launch, Zellic, another security-focused company, has performed [another audit](https://github.com/Zellic/publications/blob/master/warpdotgreen-cli%20-%20Zellic%20Audit%20Report.pdf) on our Solidity contracts.

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

Please note that validators operate through several legal entities; the list above is only intended to give a sense of the people and teams maintaining them. For any message to be successfully relayed, 7 of the 11 validators need to sign it.

### What are the risks associated with bridging?

When bridging, you're exposed to multiple risks. Among them, we'd like to outline:

* **Underlying blockchain risks**: One of the involved chains might experience consensus failure. This would likely lead to assets that cannot be unwrapped, and messages that have been delivered might become invalid.
* **Smart contract / puzzle risks**: A vulnerability in the puzzles, smart contracts or overall design might allow malicious actors to steal user funds. To minimize this risk, we have undergone audits for both sides of the bridge - see questions above for more information.&#x20;
* **Validator risk**: When relying the warp.green protocol, you're trusting that a majority of validators are honest and have not been compromised. For a list of the validators, please see the questions above.
