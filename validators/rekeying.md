# Rekeying

Rekeying is the process of changing hot or cold keys, as well as signature thresholds. It is expected to be used when one of the validators loses access to their key(s) or suspects that they were compromised.

For EVM chains, Safe{Wallet} makes any possible re-key operation simple through their interface. Validator 0 will generate a normal multisig transaction, which the other validators will verify and sign.

For Chia, the portal singleton comes with a built-in update mechanism. After validators have been notified that a rekey is needed, they will each run the following command:

```
python3 cli.py rekey sign-tx --new-message-keys [new-message-keys] --new-message-threshold [new-message-threshold] --new-update-keys [new-update-keys] --new-update-threshold [new-update-threshold] --validator-index [validator-index]
```

A list of the options and their respective descriptions can be viewed via:

```
python3 cli.py rekey sign-tx --help
```

The process to generate the key is identical to the one used during [attestations](attestations.md) (generate QR code, scan with Ozone wallet, get signature). The `{validator-index}-{signature}` string should be sent to validator 0via either private message or on Keybase.

{% hint style="info" %}
Your signature authorizes the transition from the keys/threshold in the current config to the newly-specified values. It can be used to update _any_ portal coin with the settings in your config to the new settings. This means that messages can be relayed while the signatures are gathered, leading to minimal service disruption.
{% endhint %}

Once enough signatures have been gathered, validator 0 will assemble a spend bundle that upgrades the portal via:

```
python3 cli.py rekey broadcast-spend --help
```

Once the transaction is confirmed, validators should update the affected `config.json` files. The frontend code configuration will also need to be updated.
