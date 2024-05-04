# EVM - Sending And Receiving Messages

On EVM-based chains (Ethereum/Base), messages are sent and received via a [Portal](https://github.com/warpdotgreen/cli/blob/master/contracts/Portal.sol) contract, which usually sits behind an upgradeable proxy.

### Sending Messages

Any contract (or user) that want to send a message can call the `sendMessage` function:

```
function sendMessage(
        bytes3 _destination_chain,
        bytes32 _destination,
        bytes32[] calldata _contents
    ) external payable 
```

A per-message toll of `0.00001 ETH` will need to be sent for each message. Note that the function identifies the sender as `msg.sender`.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Schematic of sending a message from an EVM chain. A source (usually a contract) calls the portal's <code>sendMessage</code> function with the protocol-set message toll (denoted as $fee$ to highlight that the call also includes some value in ETH). A <code>MessageSent</code> is then generated, which the validators observe. After enough confirmations have passed, the validators generate signatures and post them on Nostr.</p></figcaption></figure>

### Receiving Messages

After the required signatures have been collected, anyone can call the following function:

```
function receiveMessage(
    bytes32 _nonce,
    bytes3 _source_chain,
    bytes32 _source,
    address _destination,
    bytes32[] calldata _contents,
    bytes memory _sigs
) external {
```

The `receiveMessage` function will call the destination contract and provide details about the message that has been relayed. It then proceeds to mark the nonce and source chain combination as 'used', thus preventing a message from being relayed twice.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Messages are relayed by users. Message data is first fetched from the source chain. After that, Nostr is queried (see 'Collecting Signatures') until enough validator signatures are obtained. The <code>receiveMessage</code> function is then called, which will call the destination contract. Note that warp.green is used as an oracle by the destination contract, which trusts the provided values (nonce, source chain id, source, contents) given that the Portal contract calls it.</p></figcaption></figure>
