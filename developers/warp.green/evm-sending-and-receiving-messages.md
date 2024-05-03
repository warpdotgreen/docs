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
