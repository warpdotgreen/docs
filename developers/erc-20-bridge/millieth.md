# MilliETH

On Ethereum and other EVM chains, applications that do not want to add support for the native currency of the chain (e.g., ether) can still support it by using `WETH`, a simple ERC-20 token that is issued 1:1 when ether is deposited into its contract. It also allows unwrapping - users burn `WETH` and receive ether back at the same ratio, 1:1.

A similar solution can be also used to allow wrapping ether on Chia. However, a limitation in the current CAT standard makes WETH a less than optimal choice. Mainly, CATs are each made up of 1000 mojos, meaning that a CAT's smallest denomination is 0.001. Assuming a $3000 per ether price, that would mean $3, which would make trading difficult. Taking into account the possibility of future price increase, our team decided that this might become a significant problem.

[MilliETH](https://github.com/warpdotgreen/cli/blob/master/contracts/MilliETH.sol) is the solution we've come up with. It functions the same way as WETH, except it mints 1000 milliETH for every 1 ether that is deposited. Similarly, the contract gives back 1 ether for every 1000 milliETH that is burned. The contract is immutable, and can be used by any DeFi application that needs it.

Because some L2s use other native currencies, using milliETH is an 'option' that can be set at bridge deployment. Namely, the `_iweth` and `_wethToEthRatio` arguments can either be set to make the bridge use milliETH or any version of WETH.&#x20;
