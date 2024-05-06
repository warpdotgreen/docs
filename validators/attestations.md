# Attestations

Attestations are a way to prove that you still have access to your cold XCH key. Each week (and during deployment), validator 0 will generate a random challenge via:

```
python3 cli.py rekey create-challenge
```

{% hint style="info" %}
The attestations are part of the 'rekey' module - you'll follow a similar process to [rekey](rekeying.md) or update the portal.
{% endhint %}

Example challenge:

```
e5c6ef33e66149c6e0d694ebe3f2325ea5ac2a84e66635ce0a4355ce912443ca
```

The challenge will be sent to you via a message. Once you have it, run:

```
python3 cli.py rekey sign-challenge --challenge [challenge] --validator-index [index]
```

{% hint style="info" %}
You can run all the attestation commands on a machine that does NOT have your hot keys in the config. Simply clone the 'cli' repository on your computer and copy config.json (with hot keys removed).
{% endhint %}

{% hint style="info" %}
By default, your public cold key is taken from the config. If you're just setting things up, specify your public key using the optional `--pubkey [tangem-public-key]` switch.
{% endhint %}

Remember to replace `[challenge]` with the given challenge and `[index]` with your assigned validator index. The command will generate a QR code and save it to 'qr.png.'

Example command output:

```
$ python3 cli.py rekey sign-challenge --challenge e5c6ef33e66149c6e0d694ebe3f2325ea5ac2a84e66635ce0a4355ce912443ca --validator-index 2
Message: b'Validator #2 attests having access to their cold private XCH key by signing this message with the following challenge: e5c6ef33e66149c6e0d694ebe3f2325ea5ac2a84e66635ce0a4355ce912443ca'
Message hash: e9a9144b1d4455448cf317b4d5da231c0e0893355c323e438ac1e3234f7fc7e9
Message to sign: e9a9144b1d4455448cf317b4d5da231c0e0893355c323e438ac1e3234f7fc7e9
Your validator index: 2
Validator public key: 8a5c3c9d08d667775d0045335b8c90941763cd00a8cd6ed867c03db243da9b4c227a7012859b9355376df297bd5d8811
Validator puzzle hash: 0830d088496e4d541012794d1ee6efcec4fdbe15fc7a4e2315a3ff4a9f939d0f
Validator address: xch1pqcdpzzfdex4gyqj09x3aeh0emz0m0s4l3ayugc450l548unn58s98cm7f
QR code data: {"validator_index": 2, "address": "xch1pqcdpzzfdex4gyqj09x3aeh0emz0m0s4l3ayugc450l548unn58s98cm7f", "message": "0xe9a9144b1d4455448cf317b4d5da231c0e0893355c323e438ac1e3234f7fc7e9", "bridge": true}
QR code saved to qr.png
```

To sign the challenge, open the Ozone Wallet app on your phone and sign in to the account associated with your validator Tangem card. Press the settings icon in the top right of the screen, and select 'Sign Messages.'

On the new screen, you'll have a QR icon in the top right corner - press on it, and scan the contents of the previously-generated QR code. The message field should be auto-filled with the `Message hash` outputted by the previous `cli.py` command.

After following the on-screen instructions (tap Tangem card, enter PIN, etc.), you'll see a box saying 'Signature' that contains JSON. Your attestation is a string in the following format: `{validator_index}-{signature}`. Take both values from the JSON and send them for verification.

Example attestation:

```
2-8bec3988424a2939facc24d83baf571e4dd4a91f7267ed4f61e3d85de9970e02c075e21f43ed98bb529b17f86d43e42f01d6767fa8439576a9708
45de508a41374a3774f8f357d5f90b539a3cf98334d89f9b11dd546244f6f15e1725128ba7e
```

Anyone can  verify the given attestation using:

```
python3 cli.py rekey verify-challenge --challenge [challenge] --sig [attestation]
```

{% hint style="info" %}
If your config has not been set up yet, this command also supports the `--pubkey [tangem-public-key]` switch.
{% endhint %}

Example command output:

```
python3 cli.py rekey verify-challenge --challenge e5c6ef33e66149c6e0d694ebe3f2325ea5ac2a84e66635ce0a4355ce912443ca --sig 2-8bec3988424a2939facc24d83baf571e4dd4a91f7267ed4f61e3d85de9970e02c075e21f43ed98bb529b17f86d43e42f01d6767fa8439576a9708
45de508a41374a3774f8f357d5f90b539a3cf98334d89f9b11dd546244f6f15e1725128ba7e
Message: b'Validator #2 attests having access to their cold private XCH key by signing this message with the following challenge: e5c6ef33e66149c6e0d694ebe3f2325ea5ac2a84e66635ce0a4355ce912443ca'
Message hash: e9a9144b1d4455448cf317b4d5da231c0e0893355c323e438ac1e3234f7fc7e9
Signature is valid!
```

Once validator 0 verifies an attestation,  he will reach with a 'check' emoji to the message containing it. Any validator is welcome to verify any attestations and react the same way.
