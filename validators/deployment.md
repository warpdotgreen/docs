# Deployment

## Prerequisites

Before beginning this process, you should have:

* Synced Chia full node
* Synced Ethereum full node
* A Coinbase Cloud account and access to your base RPC URL
* A Nostr relay set up (empty public key whitelist)
* A machine you intend to run the validator software on
* Two hardware wallets, one for Ethereum (e.g., Trezor) and one for Chia (Tangem)
* &#x20;The Ozone wallet app installed on your phone

{% hint style="info" %}
Note: For testnet deployment, you don't need the Chia/Ethereum full nodes to get everything running. Simply use an Alchemy (or similar) RPC for Ethereum. For Chia, there'll be a note on how to change the config so you're using a temporary FireAcademy testnet11 instance.
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
    "chia_root_or_chia_url": "/home/yakuhito/.chia-testnet11/mainnet",
    "agg_sig_data": "37a90eb5185a9c4439a91ddc98bbadce7b4feba060d50116a067de66bf236615",
    "prefix": "txch",
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

* `xch.my_hot_private_key` with&#x20;
* `nostr.my_mnemonic` with your Nostr mnemonic
