# CAT Bridge

Anyone can build on top of the warp.green cross-chain messaging protocol. At launch, we provided two simple example applications, one of which is called the CAT Bridge. This app allows XCH (native currency of the Chia blockchain) and CATs (tokens on the Chia blockchain following the official token standard) to be wrapped into ERC-20 tokens on EVM chains (Ethereum/Base) . The WrappedCAT contract can be found [here](https://github.com/warpdotgreen/cli/blob/master/contracts/WrappedCAT.sol), and puzzles can be found [here](https://github.com/warpdotgreen/cli/tree/master/puzzles/wrapped\_cats).

Instead of a contract or singleton owning funds, CATs that have been wrapped are locked in a special puzzle to form a vault coin. The vault coin can only be spent by an unlocker puzzle, which only runs when the equivalent ERC-20 token is burned.
