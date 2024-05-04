# Wrapping CATs

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Overview of the Chia-side transaction that initiates CAT wrapping, along with steps.</p></figcaption></figure>

To wrap CATs, they first need to be locked into vault coins. The 'CAT Locker' puzzle asserts the creation of a new vault coin and then creates a message to the WrappedCAT contract. The creation of the coin is done by asserting an announcement from the settlement payments puzzle, which also powers Chia offers.

On the EVM side, the user has to simply relay the message by calling the `receiveMessage` function on the `Portal` contract, which will transmit message contents and other information to the `WrappedCAT` contract. The ERC-20 tokens representing the CAT will then be minted to the address specified when bridging was initiated on Chia.

Note that a WrappedCAT contract needs to be deployed and properly initialized on the destination chain before a CAT can be bridged.
