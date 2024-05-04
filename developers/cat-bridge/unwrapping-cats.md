# Unwrapping CATs

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>CAT unwrapping transaction model &#x26; process steps</p></figcaption></figure>

To unwrap a CAT, a user first needs to call the `bridgeBack` method on the WrappedCAT ERC-20 contract, specifying a receiver and the desired amount to bridge back. The transaction will generate a message which can be relayed to Chia via the warp.green protocol.&#x20;

On Chia, the unlocker puzzle is used to return CATs to the users. The coin accepts a list of vault coins, which can then be spent. The last vault in the list will create a CAT with an inner puzzle hash equal to the one specified when unwrapping was initiated on Ethereum, effectively 'sending' the CAT amount to the intended receiver. If the total value of the vault coins being spent is higher than the payout value, a new vault coin is created to contain the change.

Notably, vault coins are locked with a puzzle called `p2_controller_puzzle_hash`, which allows the coin to be controlled by another coin with a different puzzle hash (the unlocker puzzle hash, in this case).
