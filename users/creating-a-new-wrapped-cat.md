---
description: Make your CAT available on Base
---

# Creating a New Wrapped CAT

As a result of many requests from the Chia community, the team has decided to create a process for making new memecoins available for bridging. The CAT creator is expected to cover the contract deployment fee on Base. Note that following the steps outlined here does **NOT** guarantee the token will be listed on the official bridge interface - the warp.green team reserves the right to approve/reject pull requests on a case-by-case basis. As such, it's highly recommended to [reach out](contact-us.md) before starting the process.

**Note**: Before beginning, make sure your token details are consistent and up-to-date on Dexie, SpaceScan, and TibetSwap.

**Note 2**: An example PR can be found [here](https://github.com/warpdotgreen/warp-ui/pull/35).

## Step 1: Deploy WrappedCAT Contract on Base

The first step in the process required you to deploy a special ERC-20 contract for the future warped CAT. This contract uses the bridge portal contract as an 'oracle' to parse messages from Chia and mint equivalent warped CATs. Moreover, the contract allows users to burn their warped CATs in order to send an 'unlock' message to Chia, where they get the same amount (minus a fee) of real CATs.

To do this, please use the following page: [https://warp.green/bridge/deploy](https://warp.green/bridge/deploy)

Note that the CAT symbol needs to match the 'code' on Dexie/SpaceScan (it's not the CAT's full name). On Base, the ERC-20 will have a symbol of 'w\[SYMBOL]' and a name of 'Chia Warped \[SYMBOL]'.

After deploying the contract, store the contract address, as it will be used in future steps.

## Step 2: Add your WrappedCAT Address to the Docs

The [Contract Addresses](../developers/contract-addresses.md) page maintains an up-to-date list of all currently deployed WrappedCAT contracts. Make a PR to add your new contract address in this file: [https://github.com/warpdotgreen/docs/blob/master/developers/contract-addresses.md](../developers/contract-addresses.md)&#x20;

New entries must be added at the end of the table under 'Base Mainnet.' No special description is needed for the PR, as context will be provided in a future PR. Please name your PR request "Add \[SYMBOL]".

## Step 3: Add your WrappedCAT Address to the Watcher

The [watcher](https://github.com/warpdotgreen/watcher) follows sent messages on-chain and tries to parse their contents. Its API is used in the explorer page, which shows pending and finished transfers.

Make a PR to add your token symbol, asset id, and WrappedCAT contract address to this file: [https://github.com/warpdotgreen/watcher/blob/master/config.mainnet.json](https://github.com/warpdotgreen/watcher/blob/master/config.mainnet.json)

The new entry should be added in the 'tokens' array for the xch-bse CAT bridge, near the end of the JSON file. No special description is needed for the PR, as context will be provided in a future PR. Please name your PR request "Add \[SYMBOL]".

## Step 4: Add your WrappedCAT Address to the UI

It's now time to add your CAT to the [bridge UI](https://github.com/warpdotgreen/warp-ui). More specifically, you will need to modify the following file: [https://github.com/warpdotgreen/warp-ui/blob/master/src/app/bridge/config.tsx](https://github.com/warpdotgreen/warp-ui/blob/master/src/app/bridge/config.tsx)&#x20;

Use the definition of `BEPE_MEMECOIN_TOKEN_BASE_ONLY` as a template. Make sure the `memecoin`  attribute of your token is set to `true`  and that there is an `additionalWarning` if the team indicated one is required.

A test transfer is required to add the token to the UI. Before running 'npm run dev', make sure your '.env.local' file contains `NEXT_PUBLIC_TESTNET=false`. With the local UI running, do a test bridge to Base (token amount can be small), and then bridge a small amount back. Keep track of the transaction ids as they'll need to be included in the final PR.

To make the final PR, please follow this template: [https://github.com/warpdotgreen/warp-ui/blob/master/.github/pull\_request\_template.md](https://github.com/warpdotgreen/warp-ui/blob/master/.github/pull_request_template.md) . All the fields of the table should be completed with values from previous steps.  Please name your PR request "Add \[SYMBOL]".

After making the final PR, please [contact us](contact-us.md), making sure to include a link to the final PR. A lot of checks have to be made before a token can be added to the UI - response times may vary.
