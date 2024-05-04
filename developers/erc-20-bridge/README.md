# ERC-20 Bridge

Anyone can build on top of the warp.green protocol. At launch, we provided two simple example applications, one of which is called the ERC-20 bridge. As the name suggests, it allows wrapping and unwrapping ERC-20s on Chia by converting them to equivalent CATs. The contracts can be found [here](https://github.com/warpdotgreen/cli/tree/master/contracts).

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>The two apps presented in the docs, ERC-20 Bridge and CAT Bridge, are fully immutable apps. They use the warp.green cross-chain messaging protocol as an oracle, meaning that they trust the protocol to relay truthful data. </p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>ERC20Bridge.sol overview - While it trusts an oracle to be truthful, the contract itself is immutable once deployed and initialized. No account has any specific privileges - ERC-20 tokens are owned by the contract until unwrapped.  </p></figcaption></figure>



### Message Contents

As previously mentioned, each message sent through the warp.green protocol has a contents field which consists of 32-byte values. In the case of `ERC20Bridge`, the contents array contains 3 elements:

* **ERC-20 Contract Address**: The contract address of the token being wrapped, which is essentially used as an identifier
* **Receiver**: The address that will receive either the wrapped ERC-20 CATs (when wrapping) or original ERC-20 tokens (when unwrapping).  This value is either a puzzle hash (when wrapping) or an address (when unwrapping).
* **Amount in mojos**: Amount, in mojos, of wrapped CATs to be minted or that have been burned.

Note that all values are automatically padded with leading 0s to be 32-byte.
