# Deployment

## Prerequisites

Before beginning this process, you should have:

* Synced Chia full node
* Synced Ethereum full node
* A Coinbase Cloud account and access to your base RPC URL (details in step 1)
* A Nostr relay (empty public key whitelist)
* Access to the machine you intend to run the validator software on
* Two hardware wallets, one for Ethereum (e.g., Trezor) and one for Chia (Tangem)
* &#x20;The Ozone wallet app installed on your phone

{% hint style="info" %}
Note: For testnet deployment, you don't need the Chia/Ethereum full nodes to get everything set up (see details in step 1). However, you should still sync nodes to test the setup is working as a whole.
{% endhint %}

## Step 0: Generate Keys

First, install the validator software from [here](https://github.com/warpdotgreen/cli) by following the [instructions](https://github.com/warpdotgreen/cli?tab=readme-ov-file#install). You'll then need generate your two (Chia/EVM) hot keys and a Nostr private key by running:

```bash
python3 cli.py keys generate-xch-key
python3 cli.py keys generate-eth-key
python3 cli.py keys generate-nostr-key
```

{% hint style="info" %}
Even if you're deploying on testnet, take the scenario seriously and handle keys proeprly.
{% endhint %}

It is up to you if you want to back up mnemonics - a majority of validators can always rekey the portal. In the next step, you're going to use:

* Chia/Ethereum private keys (hex format)
* Nostr mnemonic

Before proceeding, please send a message that follows the template below on the deployment channel:

```
Validator #{validator_index}

Nostr relay: wss://{your-relay-domain}
Nostr public key: {your-newly-generated-nostr-pubkey-hex}

Ethereum hot address: {your-newly-generated-eth-address}
Ethereum cold address: {your-eth-hardware-wallet-address}

Chia hot key: {your-newly-generated-xch-pubkey-hex}
Chia cold key: {your-tangem-pubkey}

Attestation: {your-attestation}
```

{% hint style="info" %}
To get your Tangem public key, you'll need to use Ozone. Connect your Tangem card to the app. Once you're on the main screen (which displays your wallet balance), press on 'Settings' in the top right corner. Select 'Sign Messages' and put any message in the message field. Once you sign it, your card's key will be found in the response JSON ("pubkey").
{% endhint %}

{% hint style="info" %}
Please see '[Attestations](attestations.md)' to see how to generate an attestation for your cold XCH key.
{% endhint %}

{% hint style="info" %}
A validator's index can always be used to get their details from lists. For example, in the `multisig_keys` list we'll set below, `multisig_keys[index]` will contain the multisig (cold) key of the validator for a given index. Note that these are 'code' lists - i.e., numbering starts at 0. Please do NOT make everything confusing by identifying yourself with a negative index.
{% endhint %}

Once you send the message, you may proceed to the next step and save the private keys in the config file.

## Step 1: Set up config.json

In the root of the 'cli' local copy, create a file called 'config.json' and paste the following template:

```json
{
  "xch": {
    "chia_root_or_chia_url": "",
    "agg_sig_data": "",
    "my_hot_private_key": "",
    "portal_threshold": 0,
    "portal_keys": [],
    "multisig_threshold": 0,
    "multisig_keys": [],
    "per_message_toll": 1000000000,
    "portal_launcher_id": "",
    "min_height": 0,
    "sign_min_height": 0
  },
  "eth": {
    "rpc_url": "",
    "my_hot_private_key": "",
    "hot_addresses": [],
    "portal_threshold": 0,
    "deployer_safe_address": "",
    "create_call_address": "0x7cbB62EaA69F79e6873cD1ecB2392971036cFAa4",
    "portal_address": "",
    "erc20_bridge_address": "",
    "wei_per_message_toll": 10000000000000,
    "min_height": 0,
    "sign_min_height": 0
  },
  "bse": {
    "rpc_url": "",
    "my_hot_private_key": "",
    "hot_addresses": [],
    "portal_threshold": 0,
    "l1_block_contract_address": "0x4200000000000000000000000000000000000015",
    "deployer_safe_address": "",
    "create_call_address": "0x9b35Af71d77eaf8d7e40252370304687390A1A52",
    "portal_address": "",
    "erc20_bridge_address": "",
    "wei_per_message_toll": 10000000000000,
    "min_height": 0,
    "sign_min_height": 0
  },
  "nostr": {
    "my_mnemonic": "",
    "relays": []
  }
}
```

From the previous step, you should fill:

* `xch.my_hot_private_key` with your hot XCH key
* `eth.my_hot_private_key` and `bse.my_hot_private_key` with your hot Ethereum key
* `nostr.my_mnemonic` with your Nostr mnemonic

After all validators have submitted their details, validator 0 will send a message containing values for:

* `xch.portal_keys` and `xch.multisig_keys`
* `eth.hot_addresses` = `bse.hot_addresses`
* `nostr.relays`

The message will also contain a list of public keys to add to your Nostr server whitelist. At this point, you may can set:

* `xch.chia_root_or_chia_url`
  * If you're running a node locally, replace this key with `xch.chia_root` and set it to the location of the 'mainnet' folder (e.g., `/home/yakuhito/.chia-testnet11/mainnet`)
  * If you are on testnet and your node is still syncing, replace the key with `xch.chia_url` and set it to `https://testnet.fireacademy.io`. Please make sure to switch it with the option above (`xch.chia_root`) once you're synced!
* `xch.agg_sig_data`: This is a constant that is specific to the network you're running.&#x20;
  * Testnet: `37a90eb5185a9c4439a91ddc98bbadce7b4feba060d50116a067de66bf236615` ([source](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/util/initial-config.yaml#L90))&#x20;
  * Mainnet: `ccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb` ([source](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/util/initial-config.yaml#L18))
* `xch.portal_threshold` ,  `xch.multisig_threshold`, `eth.portal_threshold` ,  `bse.portal_threshold`: Set to 5 for testnet, 8 for mainnet
* `xch.sign_min_height`: The minimum number of confirmations a message needs to have on Chia for it to be signed. Used to prevent attacks exploiting re-orgs - set this value to 5 for testnet and 32 for mainnet ([source](https://docs.chia.net/consensus-analysis/)).
* `eth.rpc_url`: Point this to your Ethereum RPC URL.
  * If you're on testnet and waiting for your node to sync, you can set the value to an Alchemy/Infura/etc. RPC URL. However, please make sure to make it point to your node later, as you should test your whole infrastructure.
* `eth.sign_min_height` and `bse.sign_min_height`: The minimum number of confirmations a message needs to have on L1 for it to be signed. Please see [this post](https://jumpcrypto.com/writing/bridging-and-finality-op-and-arb/) about finality. For mainnet, set this value to 64 (2 epochs). For testnet, set the value to 10 (\~2 minutes) for Base and 64 for Ethereum (Sepolia).
* `bse.rpc_url`: You can create a Coinbase Cloud account [here](https://portal.cloud.coinbase.com/). You'll be allowed to create a node for free (i.e., get an API key).
  * For mainnet, the value should look like this: `https://api.developer.coinbase.com/rpc/v1/base/[api-key]`
  * For testnet, please ensure that you select 'Base Sepolia' (i.e., testnet) before copying the URL. It's somewhere on the page, and the default is mainnet even if you 'created' a testnet node. The value should look like this: `https://api.developer.coinbase.com/rpc/v1/base-sepolia/[api-key]`

{% hint style="info" %}
Don't forget to update your Nostr server's pubkey whitelist.
{% endhint %}

## Step 2:  Deploy contracts

Validator 0 will deploy the portal singleton on XCH and a multisig on each EVM network (Ethereum and Base). Once addresses are shared, fill in the following config values:

* `xch.portal_launcher_id` - Launcher id of portal singleton (will be sent in a message)
* `xch.min_height` - First block when messages can be sent. Will also be included in the message.
* `eth.deployer_safe_address` and `bse.deployer_safe_address`: This is the cold key multisig. Get the addresses from the message and access them using the Safe{Wallet} app. Check that the threshold and addresses are set appropriately (same values as config).

In the Safe app, a deployment transaction will also be created shortly after. After updating the config, you can verify the transactions using:

```
python3 cli.py deployment get-evm-deployment-data --weth-address meth --tip 30 --chain [chain-id]
```

Where `[chain-id]` is either 'eth' or 'bse.' For each EVM network, you can go on to fill the following config values:

* `portal_address`: The address of the portal. In the deploy command output, you can find it under `Tx 2: deploy TransparentUpgradeableProxy` as `Predicted address`.
* `erc20_bridge_address`: The address of the `ERC20Bridge` contract. It's the `Predicted address` under `Tx 3: deploy ERC20Bridge`.

{% hint style="info" %}
To allow the Portal contract to be updated, it'll be deployed behind a standard proxy called "TransparentUpgradeableProxy." The address of the 'Portal' contract is the 'logic address' (it implements the current logic of the portal, which means the address can change). However, when specifying the portal address, always refer to the address of the proxy contract, as that is the contract that runs the logic (e.g., sends and receives messages).
{% endhint %}

Once the transaction has enough confirmations, it will be executed by validator 0. You'll then be able to fill in the last two config values, `eth.min_height` and `bse.min_height`. Their value should be the block that the contract deployment transaction was confirmed at.

## Step 3: Run validator

You're now ready to run the validator software! Start it using:

```
python3 cli.py listen
```
