# Unwrapping ERC-20s

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Overview of steps needed to unwrap ERC-20s &#x26; Chia-side initial transaction model</p></figcaption></figure>

Unwrapping ERC-20s is initiated by burning wrapped ERC-20 CATs on Chia by building and submitting a spend bundle as depicted above. This operation generates a message coming from a trusted 'CAT Burner' puzzle, which can then be relayed to the destination chain. After the user calls the portal's `receiveMessage` function, the message will be relayed to the `ERC20Bridge` contract (message destination), which will send the equivalent amount of ERC-20 tokens (minus the bridging tip) to the destination address that was specified when bridging was initiated on Chia.
