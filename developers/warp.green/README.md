# warp.green

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>Messaging Protocol Overview - Validators observe events on one blockchain and generate signatures after the message has enough block confirmations. Users can then fetch the signatures from a public message board (Nostr) and relay the message to the destination chain.</p></figcaption></figure>

warp.green is, at its core, a cross-chain messaging protocol - it allows sending messages from one chain to another. A message consists of:

* **Nonce**: A unique 32-byte value that identifies the message on its source chain. Note that two messages can have the same nonce but different source chains.
* **Source chain**: A 3-byte id of the chain where this message was generated (origin). The value can be used to identify a specific network (see table under 'Chain Ids'), much like an EVM chain ID or a Chia genesis challenge.
* **Source**: This is the chain-specific identifier of the user or contract that created the message. On Chia, this is a puzzlehash. On Ethereum, the source is an address equal to `msg.sender`.
* **Destination chain**: Similar to 'source chain' (3-byte, same possible values) except this parameter specifies the chain where the message is intended to be received.
* **Destination**: The puzzle hash or address that will receive the message (similar to 'source').
* **Contents**: A list of 32-byte values that contain the data that is going to be relayed.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Message Structure Visualization</p></figcaption></figure>

While the structure of a message is the same, the process to send and retrieve on is different based on the type of blockchain (EVM/coinset) the operation is performed on. The pages in this section explain each process.

### Chain Ids

The list below contains ids of networks that are currently supported by the protocol. Note that it may be extended if support for new networks is added.

<table><thead><tr><th width="246">Network</th><th>3-byte ID (Source/Destination) </th></tr></thead><tbody><tr><td>Chia (mainnet)</td><td>"xch" / 0x786368</td></tr><tr><td>Ethereum (mainnet)</td><td>"eth" / 0x657468</td></tr><tr><td>Base (mainnet)</td><td>"bse" / 0x627365</td></tr></tbody></table>

### Message Toll

Besides associated transaction fees, users and contracts that send messages also pay a 'message toll.' This small amount is responsible for preventing protocol abuse. It is not given to validators - instead, the toll is sent to the miner of the block in which the message was sent.
