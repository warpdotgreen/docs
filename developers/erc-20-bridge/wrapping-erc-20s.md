# Wrapping ERC-20s

Wrapping ERC-20 tokens involves calling a function on the `ERC20Bridge` contract to lock tokens & send a message, relaying the message to Chia, and interacting with a minter puzzle to create wrapped CATs.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Description of wrapping steps &#x26; final Chia transaction</p></figcaption></figure>

### Initiate Wrapping

There are 3 functions that allow the user to initiate bridging:

* `bridgeToChia`: Initiates the bridging of an ERC-20 token given its contract address and amount. Requires the `ERC20Bridge` contract to have prior approval to spend tokens as it uses the `transferFrom` method on the target ERC-20 contract.  &#x20;
* `bridgeToChiaWithPermit` - Similar to `bridgeToChia`, except it uses `permit` to include token spend approval and bridging in the same transaction.
* `bridgeEtherToChia` - Wraps any value over the per-message toll to an ERC-20 (either WETH or MilliETH -  see 'MilliETH') and initiates bridging.

Note that all of the above functions are also given a receiver puzzle hash as an argument. The specified puzzle hash will receive the wrapped CATs on Chia.&#x20;

### Offer

At first sight, one might wonder why an offer is needed on the Chia side. The offer serves two purposes:

* Provide a transaction fee: When the mempool is fool, transactions with no fees are rejected. Asking the user to create an offer gives them control over the fee that is paid (higher fee leads to lower confirmation time).
* Provide mojos: To mint wrapped CATs, a small amount of mojos needs to be used. Because the bridge layer is immutable and permissionless, the value has to come from the user.&#x20;

### Eve CAT Coin

Wrapped ERC-20 CATs have a special TAIL that allows them to be minted and melted. Even though the eve CAT coin is technically a CAT, a wallet would need to specifically add support for the ERC-20 Bridge to spend it, as its parent is a non-CAT (meaning that the TAIL would need to be run). To overcome this, the mint spend bundle also spends this CAT coin to create a new coin that any wallet can spend (named 'CAT Payout Coin' in the image).

### Security Coin

Because the user's offer does not request any asset (the receiver puzzle hash is included in the message contents), the spend bundle would be, by default, insecure. The security coin is a standard coin created with a newly-generated and temporary BLS key that:

* Adds a signature requirement to the spend bundle, so the offer cannot be 'extracted' by a malicious farmer to pocket the offered XCH amount and fee without unwrapping the CAT. In the spend above, the portal also has this effect (requires signatures of validators), but, in spends described on other pages, there would normally be no other coin doing this.
* Make necessary assertions so that the spend bundle cannot be 'split' by a malicious farmer. In this case, the security coin could, for example, assert that the CAT Eve Coin was spent to confirm that, if the offer is used, the user will receive their wrapped ERC-20 CATs to the puzzle hash they specified when they initiated the bridging on Ethereum/Base.
