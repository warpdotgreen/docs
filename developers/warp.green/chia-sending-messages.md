# Chia - Sending Messages

For Chia, sending messages is as simple as creating a coin. Specifically, the 'CREATE\_COIN' condition should contain the following arguments:

* `puzzle_hash`: see below
* `amount`: at least the current per-message toll (1000000000 mojos)
* `memos`: a list of values as defined below
  * `destination_chain`: 3-byte identifier of the chain the message will be relayed to
  * `destination`: identifier (i.e., address) of the contract that will receive the message
  * `contents`: all remaining memo values will be padded to 32 bytes and be used as contents

### Resulting Coin

The resulting coin's id will be used as a nonce, as Chia consensus enforces unique coin ids. The puzzle hash of the said coin should be `a09eb1ea8c6e83c0166801dabcf4a70d361cc7f6d89c4a46bcd400ac57719037`, which corresponds to the following puzzle:

```
(mod (
 my_amount 
)
  (include condition_codes.clib)

  (list
    (list ASSERT_MY_AMOUNT my_amount)
    (list RESERVE_FEE my_amount)
  )
)
```

The puzzle above is responsible for sending the message toll to the block farmer - i.e., use the coin's value as a transaction fee.
