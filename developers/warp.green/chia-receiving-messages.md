# Chia - Receiving Messages

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>Overview of receiving messages on Chia. Message data is assembled by observing the source chain. The user is responsible for collecting validator signatures from Nostr and assembling a spend involving the portal receiver coin, which creates a message coin. The message coin can then be spent along a destination coin.</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Puzzle structure and overview for the portal receiver &#x26; message puzzles. The portal receiver can be spent by anyone to relay messages, given enough signatures. It also has a built-in alternative spend path that allows updates in case of a protocol update. Message coins have a fairly simple puzzle, which can be found below.</p></figcaption></figure>

A message should only be received once. Because of this, messages can only be received through a portal singleton, the source code of which can be found [here](https://github.com/warpdotgreen/cli/blob/master/puzzles/portal\_receiver.clsp). The singleton takes the message data and associated validator signatures and creates a message coin with [this puzzle](https://github.com/warpdotgreen/cli/blob/master/puzzles/message\_coin.clsp):

```
(mod (
    ; first curry (automated)
    PORTAL_RECEIVER_SINGLETON_STRUCT

    ; second curry
    (SOURCE_CHAIN . NONCE)
    SOURCE
    DESTINATION ; destination
    MESSAGE_HASH

    (receiver_parent_info . receiver_amount) ; receiver_proof
    parent_proof ; (parent_parent_info . parent_inner_puzzle_hash)
    my_coin_id
  )

  (include condition_codes.clib)
  (include curry.clib)
  (include sha256tree.clib)

  (defun main (
    PORTAL_RECEIVER_SINGLETON_STRUCT
    (parent_parent_info . parent_inner_puzzle_hash)
    my_coin_id
    receiver_coin_id
  )
    (list
      (list ASSERT_MY_COIN_ID my_coin_id)
      (list ASSERT_MY_PARENT_ID (sha256
        parent_parent_info
        (curry_hashes_inline (f PORTAL_RECEIVER_SINGLETON_STRUCT)
          (sha256tree PORTAL_RECEIVER_SINGLETON_STRUCT)
          parent_inner_puzzle_hash
        )
        1 ; parent amount
      ))
      (list CREATE_COIN_ANNOUNCEMENT receiver_coin_id) ; puzzle contains message info
      (list ASSERT_COIN_ANNOUNCEMENT (sha256 receiver_coin_id my_coin_id))
    ) 
  )

  (defun-inline value_b32_or_x (v)
    (if (= (strlen v) 32) v (x))
  )

  (main
    PORTAL_RECEIVER_SINGLETON_STRUCT
    parent_proof
    my_coin_id
    (sha256
      (value_b32_or_x receiver_parent_info)
      (value_b32_or_x DESTINATION)
      receiver_amount
    ) ; receiver_coin_id
  )
)
```

The message coin has an amount of 0 and locks in to a receiver coin via announcements. The message coin can optionally be spent in the same transaction that it was created so a user both relays a message and uses the intended receiver puzzle in the same transaction.
