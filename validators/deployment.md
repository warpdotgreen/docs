# Deployment

## Prerequisites

Before beginning this process, you should have:

* Synced Chia full node
* Synced Ethereum full node
* A Coinbase Cloud account and access to your base RPC URL (details in step 1)
* A Nostr relay set up (empty public key whitelist)
* A machine you intend to run the validator software on
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
Please see 'attestations' to see how to generate an attestation for your cold XCH key.
{% endhint %}

Once you send the message, you may proceed to the next step and save the private keys in the config file.

## Step 1: Set up config.json

In the root of the 'cli' local copy, create a file called 'config.json' and paste the following template:

```json
{
  "xch": {
    "chia_root_or_chia_url": "",
    "agg_sig_data": "",
    "prefix": "",
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

At this point, you may also set:

* `xch.chia_root_or_chia_url`
  * If you're running a node locally, replace this key with `xch.chia_root` and set it to the location of the 'mainnet' folder (e.g., `/home/yakuhito/.chia-testnet11/mainnet`)
  * If you are on testnet and your node is still syncing, replace the key with `xch.chia_url` and set it to `https://testnet.fireacademy.io`. Please make sure to switch it with the option above (`xch.chia_root`) once you're synced!
* `xch.agg_sig_data`: This is a constant that is specific to the network you're running.&#x20;
  * Testnet: `37a90eb5185a9c4439a91ddc98bbadce7b4feba060d50116a067de66bf236615` ([source](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/util/initial-config.yaml#L90))&#x20;
  * Mainnet: `ccd5bb71183532bff220ba46c268991a3ff07eb358e8255a65c30a2dce0e5fbb` ([source](https://github.com/Chia-Network/chia-blockchain/blob/main/chia/util/initial-config.yaml#L18))
* `xch.prefix`: 'txch' for testnet and 'xch' for mainnet&#x20;
* `xch.portal_threshold` ,  `xch.multisig_threshold`, `eth.portal_threshold` ,  `bse.portal_threshold`: Set to 5 for testnet, 8 for mainnet
* `xch.sign_min_height`: The minimum number of confirmations a message needs to have on Chia for it to be signed. Used to prevent attacks exploiting re-orgs - set this value to 5 for testnet and 32 for mainnet ([source](https://docs.chia.net/consensus-analysis/)).
* `eth.rpc_url`: Point this to your Ethereum RPC URL.
  * If you're on testnet and waiting for your node to sync, you can set the value to an Alchemy/Infura/etc. RPC URL. However, please make sure to make it point to your node later, as you should test your whole infrastructure.
* `eth.sign_min_height` and `bse.sign_min_height`: The minimum number of confirmations a message needs to have on L1 for it to be signed. Please see [this post](https://jumpcrypto.com/writing/bridging-and-finality-op-and-arb/) about finality. For mainnet, set this value to 64 (2 epochs). For testnet, set the value to 10 (\~2 minutes) for Base and 64 for Ethereum (Sepolia).
* `bse.rpc_url`: You can create a Coinbase Cloud account [here](https://portal.cloud.coinbase.com/). You'll be allowed to create a node for free (i.e., get an API key).
  * For mainnet, the value should look like this: `https://api.developer.coinbase.com/rpc/v1/base/[api-key]`
  * For testnet, please ensure that you select 'Base Sepolia' (i.e., testnet) before copying the URL. It's somewhere on the page, and the default is mainnet even if you 'created' a testnet node. The value should look like this: `https://api.developer.coinbase.com/rpc/v1/base-sepolia/[api-key]`

## Step 3:&#x20;
