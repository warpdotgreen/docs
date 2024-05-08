# Wallets

As a validator, all of your hot key signatures will be generated automatically by the validator software. But there are some cases when you need to use your cold keys - attestations, rekeys, withdrawals, upgrades, and so on. This guide will give you a brief overview of the two wallets we'll be working with.



### Ozone

Ozone is the wallet of choice to sign messages using Tangem cards. A usual flow consists of:

* **Generating data to sign** via the validator software CLI. After this step, you should have a QR code encoding a JSON containing your validator index, the message to be signed, your public key, as well as the signing mode (`"bridge": true`)
* **Signing the data** by logging into your Tangem account in Ozone, tapping the 'Settings' icon on the top right corner, selecting 'Sign Message', pressing the top-right QR icon, and scanning the QR code. After pressing 'Sign', you will tap your Tangem cards against the device to generate the actual signature.
* **Sending the data** for a validator (usually 0) to collect all signatures and aggregate them into a spend bundle that is then sent to the network.

&#x20;

### Safe{Wallet}

Formerly known as Gnosis Safe, this is the go-to multisig solution for Ethereum. See [their website](https://safe.global/) for a better presentation. Overall, the interface at [app.safe.global](https://app.safe.global/) is pretty intuitive - but don't be afraid to ask for help! Feel free to find what works for you - for example, Safe also offers a mobile app.

As soon as Safes have been created by validator 0 during deployment, you should be able to see them by connecting your Ethereum hardware wallet to the website - make sure your address is the same as the cold key address you announced.&#x20;

{% hint style="info" %}
It's recommended that the first thing you do once you log into a Safe for the first time is go to 'Settings' and enable notifications.
{% endhint %}

The main tab that you'll be dealing with is 'Transactions.' The queue shows all pending transactions - actions that have been built by one of the validators but have not yet been executed on-chain. If the number of signatures is below the needed threshold, you can sign it using your cold wallet.

{% hint style="info" %}
Always know what you're signing. The validator generating the transaction will usually send a message explaining what the transaction does - you're encouraged to expand the transaction on Safe and verify that it's indeed correct, which might sometimes involve using the CLI.
{% endhint %}

{% hint style="info" %}
A malicious transaction can theoretically make its way in the queue. Any action signed by at least one validator will show up - it's vital that you check transactions and point out when something looks strange.
{% endhint %}

{% hint style="info" %}
If you're the last signer, you'll have the option to execute the transaction. You can always select 'No' and just send your signature - that way, any other validator (even those that have already signed) will be able to execute the transaction - i.e., send it on-chain by paying the required transaction fees.
{% endhint %}
