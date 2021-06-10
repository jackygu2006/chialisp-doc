## RPC Document

### Menu:
* 1- Full Node RPC
* 2- Standard Wallet
* 3- Coloured Coin Wallet
* 4- Rate Limited Wallet
* 5- Distributed identity Wallet
* 6- Connection Management

---
### 1- FullNode
#### 1.1- get_blockchain_state
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_blockchain_state | python -m json.tool
```

#### 1.2- get_block
* Params:
    * header_hash
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{"header_hash": "0x278eb26be9d36be51748ca459bd285cf3d8538f20ab880c2bc660126aab66969"}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_block | python -m json.tool
```

#### 1.3- get_blocks
* Params: 
  * start
  * end
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{"start": 0, "end": 10}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_blocks | python -m json.tool
```

#### 1.4- get_block_record_by_height
* Params
    * height
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{"height": 1000}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_block_record_by_height | python -m json.tool
```

#### 1.5- get_block_record
* Params:
  * header_hash
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{"header_hash": "0x278eb26be9d36be51748ca459bd285cf3d8538f20ab880c2bc660126aab66969"}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_block_record | python -m json.tool
```

#### 1.6- get_block_records
* Params:
    * start
    * end
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{"start": 0, "end": 10}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_block_records | python -m json.tool
```
#### 1.7- get_unfinished_block_headers
* Params: None
* Examples:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_unfinished_block_headers | python -m json.tool
```

#### 1.8- get_network_space
* Params: 
  * newer_block_header_hash
  * older_block_header_hash
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{"newer_block_header_hash": "0x278eb26be9d36be51748ca459bd285cf3d8538f20ab880c2bc660126aab66969", "older_block_header_hash": "0x278eb26be9d36be51748ca459bd285cf3d8538f20ab880c2bc660126aab66969"}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_network_space | python -m json.tool
```

#### 1.9- get_additions_and_removals
* Params
    * header_hash
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{"header_hash": "0x278eb26be9d36be51748ca459bd285cf3d8538f20ab880c2bc660126aab66969"}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_additions_and_removals | python -m json.tool
```

#### 1.10- get_initial_freeze_period
Get initial freeze periods
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_initial_freeze_period | python -m json.tool
```

#### 1.11- get_network_info
Get network informaton
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_network_info | python -m json.tool
```
* Response:
```
{
    "network_name": "testnet7",
    "network_prefix": "txch",
    "success": true
}
```

#### 1.12- get_coin_records_by_puzzle_hash

#### 1.13- get_coin_records_by_puzzle_hashes

#### 1.14- get_coin_record_by_name

#### 1.15- push_tx
* Params:
    * spend_bundle: spend bundle to submit, in JSON
* Response:
```
{"status": "SUCCESS"}
```
* spend_bundle data structure:
```
"spend_bundle": {
    "aggregated_signature": "0xa5e5ea1f5ae2335a72fe0a7ed7ca39e8f142e2e1f6e37a348482290e88eb9cea2d973acf6145e34d0afeee7ba22f99850641e21a549b2c092bb49aa393acd938825bccca9413c1a268ba44367bc8433cd0fc0eb82e87bebe23817aa695bdb566",
    "coin_solutions": [
        {
            "coin": {
                "amount": 1750000000000,
                "parent_coin_info": "0xccd5bb71183532bff220ba46c268991a00000000000000000000000000004082",
                "puzzle_hash": "0x94c6db00186900418ef7c1f05e127ee1a647cbe6e514ec3bc57acb7bbe6dfb10"
            },
            "puzzle_reveal": "0xff02ffff01ff02ffff01ff02ffff03ff0bffff01ff02ffff03ffff09ff05ffff1dff0bffff1effff0bff0bffff02ff06ffff04ff02ffff04ff17ff8080808080808080ffff01ff02ff17ff2f80ffff01ff088080ff0180ffff01ff04ffff04ff04ffff04ff05ffff04ffff02ff06ffff04ff02ffff04ff17ff80808080ff80808080ffff02ff17ff2f808080ff0180ffff04ffff01ff32ff02ffff03ffff07ff0580ffff01ff0bffff0102ffff02ff06ffff04ff02ffff04ff09ff80808080ffff02ff06ffff04ff02ffff04ff0dff8080808080ffff01ff0bffff0101ff058080ff0180ff018080ffff04ffff01b0aec9c2e5984fe928406abca942d55ec6b56340af8315bfefa55889dbaade669b9fd3f330af2af44c2a0626d383e64757ff018080",
            "solution": "0xff80ffff01ffff33ffa03fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0ff840098968080ffff33ffa055b9fe4c9ce0cef8ad574bf5a9158dc0db7848b96be1a98ab2806d8f0a376a08ff860197738845808080ff8080"
        }
    ]
},
```
Get `spend_bundle` by calling `create_signed_transaction`

#### 1.16- get_all_mempool_tx_ids
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:8555/get_all_mempool_tx_ids | python -m json.tool
```

#### 1.17- get_all_mempool_items

#### 1.18- get_mempool_item_by_tx_id

----
### 2- Standard Wallet

#### 2.1- log_in
Login a `key` from `keychain`

* Params:
    * fingerprint
    * type: `start` | `restore_backup`
    * host: default "https://backup.chia.net"
    * file_path：if `type` is `restore_backup`, need this param for key file path
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"fingerprint": 1345224564, "type": "start", "host": "https://backup.chia.net"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/log_in | python -m json.tool
```

#### 2.2- get_public_keys
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_public_keys | python -m json.tool
```

#### 2.3- get_private_key
* Params:
    * fingerprint
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"fingerprint": 1345224564}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_private_key | python -m json.tool
```

#### 2.4- generate_mnemonic
Generate a new mnemonic

* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/generate_mnemonic | python -m json.tool
```

#### 2.5- add_key
Add mnemonic/private key into keychain, , same as `chia keys generate`.

* Params:
    * mnemonic
    * type: `new_wallet`
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"mnemonic": [
        "slide",
        "jungle",
        "canal",
        ...
    ], "type": "new_wallet"}' -H "Content-Type: application/json" -X POST https://localhost:9256/add_key
```

#### 2.6- delete_key
Delete key from keychain. Same as `chia key delete`.

* Params:
    * fingerprint
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"fingerprint": 789546845}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/delete_key | python -m json.tool
``` 

#### 2.7- delete_all_keys
Delete all keys，BE CAREFUL!!!，Same as `chia keys delete_all`

* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/delete_all_keys | python -m json.tool
```

#### 2.8- get_sync_status
Get sync status.

* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_sync_status | python -m json.tool
```

#### 2.9- get_height_info
Get wallet height, this is not block height
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_height_info | python -m json.tool
```

#### 2.10- get_wallets
Get all wallets
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_wallets | python -m json.tool
```
* Response:
```
{
    "success": true,
    "wallets": [
        {
            "data": "",
            "id": 1,
            "name": "Chia Wallet",
            "type": 0
        },
        {
            "data": "01ff02ffff01ff02ffff03ffff09ff5bff8080ffff01ff0101ffff01ff02ffff03ffff09ff13ff0280ffff01ff0101ff8080ff018080ff0180ffff04ffff01a09b3a8d052feec7f28d71910ed42c24aab8e8b396485f86a799014bd6f0bed5a6ff01808000000000",
            "id": 2,
            "name": "CC Wallet",
            "type": 6
        }
    ]
}
```

#### 2.11- create_new_wallet
Create different type wallets under the currenty fingerprint.

* Params:
    * host: ex.`localhost`, `https://backup.chia.net`
    * wallet_type：can be `cc_wallet` for coloured coin wallet, `rl_wallet` for rate limited wallet and `did_wallet` for distributed identity wallet
  
* 1- Create a `Coloured coin wallet`(cc_wallet)
    
    set `wallet_type` as `cc_wallet`，and add param `mode`, `mode` should be `new` for creating a new cc_wallet or `existing` for add a coloured wallet which the coin has been created.

    You need add another params `amount` as initialized issuing amont, the default decimals is 1000.

    ```
    curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
    --key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
    -d '{"host": "https://backup.chia.net", "wallet_type": "cc_wallet", "mode": "new", "amount": 1000000}' \
    -H "Content-Type: application/json" -X POST https://localhost:9256/create_new_wallet | python -m json.tool
    ```

    Response:
    ```
    {
    "colour": "ff02ffff01ff02ffff03ffff09ff5bff8080ffff01ff0101ffff01ff02ffff03ffff09ff13ff0280ffff01ff0101ff8080ff018080ff0180ffff04ffff01a09b3a8d052feec7f28d71910ed42c24aab8e8b396485f86a799014bd6f0bed5a6ff018080",
    "success": true,
    "type": 6,
    "wallet_id": 2
    }
    ```

    After creating successfully, using `get_wallets` to check the wallets, you will find a new wallet with type `CC_WALLET` has been established.

    If you want to add an existing coloured coin wallet, set `mode` as `existing`, and `colour` as colour of existing coloured coin.
    
    ```
    curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
    --key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
    -d '{"host": "https://backup.chia.net", "wallet_type": "cc_wallet", "mode": "existing", "colour": "ff02ffff01ff02ffff03ffff09ff5bff8080ffff01ff0101ffff01ff02ffff03ffff09ff13ff0280ffff01ff0101ff8080ff018080ff0180ffff04ffff01a09b3a8d052feec7f28d71910ed42c24aab8e8b396485f86a799014bd6f0bed5a6ff018080"}' \
    -H "Content-Type: application/json" -X POST https://localhost:9256/create_new_wallet | python -m json.tool
    ```    

* 2- Create a `rate limited wallet`(rl_wallet)
    
    set `wallet_type` as `rl_wallet`
    
    **Do the following steps**

    * **STEP 1: create a `user` rl_wallet and get `pubkey`**
    ```
    curl --insecure --cert /Users/guqianfeng/.chia/testnet_7/config/ssl/daemon/private_daemon.crt \
    --key /Users/guqianfeng/.chia/testnet_7/config/ssl/daemon/private_daemon.key \
    -d '{"host":"localhost","wallet_type":"rl_wallet","rl_type":"user"}' \
    -H "Content-Type: application/json" \
    -X POST https://localhost:9256/create_new_wallet | python -m json.tool
    ```
    The aboved operation will return:
    ```
    {
      id: 5,
      pubkey: '98d539639a44337ce2a99cbb832c92b969a74b028546fe8cdebbfdaafca40d3299af424973b08b5d44d8cd86faa3b2eb',
      success: true,
      type: 1
    }
    ```
    Using `pubkey` for creating `admin` rl_wallet.

    * **STEP 2: Create `admin` rl_wallet**
    ```
    curl --insecure --cert /Users/guqianfeng/.chia/testnet_7/config/ssl/daemon/private_daemon.crt \
    --key /Users/guqianfeng/.chia/testnet_7/config/ssl/daemon/private_daemon.key \
    -d '{"host":"localhost","wallet_type":"rl_wallet","rl_type":"admin","interval":3,"limit":10,"pubkey":"98d539639a44337ce2a99cbb832c92b969a74b028546fe8cdebbfdaafca40d3299af424973b08b5d44d8cd86faa3b2eb","amount":1000,"fee":1}' \
    -H "Content-Type: application/json" \
    -X POST https://localhost:9256/create_new_wallet | python -m json.tool
    ```
    Attn: `amount` decimal should be `10^12`。

    Return:
    ```
    {
      id: 5,
      origin: {
        amount: 1000000000000,
        parent_coin_info: '0x9e6bb70f450a79caac8f38eb176672a04c651c99964956fa30741bbc7e743436',
        puzzle_hash: '0x92cf8450576b98ee910367784304b88049a02073ebd9400c638fcf398ad32351'
      },
      pubkey: '811962d408eb6472d4f3657ba684f6d4d37414cabac948c9f8a7b24f13a78bde2491c6a00e40b949f19da0ef23b27301',
      success: true,
      type: 1
    }
    ```
    After run successfully, using `get_wallets()` to check the wallets, you will find a new `rate limited` wallet.

* 3- Create `distributed identity wallets`(did_wallet)

    set `wallet_type` as `did_wallet`
    (to be continued)


#### 2.12- get_next_address
Get address of current fingerprint.

ATTN. You should login a fingerprint before this operation. Type `chia wallet show` in terminal or using `log_in` command by `curl`(See above)

* Params:
    * wallet_id
    * new_address: Default is true. In Chia, a fingerprint is similar as `public key`, unlimited address can be issue from this `public key`. That means you can get coin into different addresses, but all in your same account. If you don't want new address, set `new_address` as `false`, you will get current address.
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 1, "new_address":true}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_next_address | python -m json.tool
```

#### 2.13- get_wallet_balance
Get current wallet balance.

* Params:
    * wallet_id
* Examples:

```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 1}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_wallet_balance | python -m json.tool
```

#### 2.14- create_signed_transaction

* Params:
    * additions: This is an array, each element including:
        * amount:
        * puzzle_hash:
    * fee
    * coins
* Response:`signed_tx`
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"additions": [{"amount": 10000000, "puzzle_hash": "3fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0"}]}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/create_signed_transaction | python -m json.tool
```
The return back data including: `spend_bundle`, this will be params of `push_tx` to broadcast transaction:

```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/full_node/private_full_node.crt \
--key ~/.chia/testnet_7/config/ssl/full_node/private_full_node.key \
-d '{        
        "spend_bundle": {
            "aggregated_signature": "0xb4b9b8a09feaf43dfb7d9c2650b09df273515d89607b07398370249abd8d97ac3c5b6417c909c3370b934576601662970f34e114025f23ace55d26bad8832a3e0a5f27def88c135190edaf429266a0d26bb7c5ec749df739b44e0ba05fc610bc",
            "coin_solutions": [
                {
                    "coin": {
                        "amount": 88000000000000,
                        "parent_coin_info": "0x5fe426d0461b5cec6f92d1a6585e34029563ae0c17e4e2a211684c28ba38c8fe",
                        "puzzle_hash": "0xf446f12f59d8bd5c7f9ac7189e8ce17853e476c8e292aebc9a6be9bb544c771e"
                    },
                    "puzzle_reveal": "0xff02ffff01ff02ffff01ff02ffff03ff0bffff01ff02ffff03ffff09ff05ffff1dff0bffff1effff0bff0bffff02ff06ffff04ff02ffff04ff17ff8080808080808080ffff01ff02ff17ff2f80ffff01ff088080ff0180ffff01ff04ffff04ff04ffff04ff05ffff04ffff02ff06ffff04ff02ffff04ff17ff80808080ff80808080ffff02ff17ff2f808080ff0180ffff04ffff01ff32ff02ffff03ffff07ff0580ffff01ff0bffff0102ffff02ff06ffff04ff02ffff04ff09ff80808080ffff02ff06ffff04ff02ffff04ff0dff8080808080ffff01ff0bffff0101ff058080ff0180ff018080ffff04ffff01b0843313d267b34fd00b6f98a07b5e8b8539ae2c21e55b88d147bb164d96228eb81ac40661bee7a84b4d6e030c97168062ff018080",
                    "solution": "0xff80ffff01ffff33ffa03fa549a708302b401c45cf387f8f03b4f76b7c9eabf567bea974f61dedf721e0ff834c4b4080ffff33ffa0fd905a7451fa0d205de192b883c0f5cd47dbe4f92385e37cd7c8a05d8588c0beff865009187134c080ffff3cffa043c672e9cc574b48e23a0c21aa6164114d7e2cd0d31b6a06c5d0b251703ca05a8080ff8080"
                }
            ]
        }}' \
        -H "Content-Type: application/json" -X POST https://localhost:8555/push_tx | python -m json.tool
```

#### 2.15- send_transaction
Sending `XCH` to other address. This is only for standard wallet. If you want to send coloured coin, you have to use `cc_spend`.

* Params:
    * wallet_id
    * amount
    * address
    * fee
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 1, "amount": 1000, "address": "txch1gn8zaepqmg4rk9s49xs6ms625vdk3k85sd3pk0jh2p65yvr2qfuqd07qn9"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/send_transaction | python -m json.tool
```
* Response:`transaction_id`


#### 2.16- get_transaction

* Params:
    * transaction_id
* Example:

```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"transaction_id": "0xa507e88a32c722ec9d194b5ba91295d8e43e74ef61b44d686d5b58b497b1a48e"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_transaction | python -m json.tool
```

#### 2.17- get_transactions
Get all transactions

* Params:
    * wallet_id
    * start
    * end
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 1, "start": 0, "end": 10}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_transactions | python -m json.tool
```

#### 2.18- get_transaction_count
Get transaction count

* Params:
    * wallet_id
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 1}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_transaction_count | python -m json.tool
```

#### 2.19- farm_block
Farm block
* Params: 
    * address

#### 2.20- get_farmed_amount
* Params: none

#### 2.21- create_backup
Create backup wallets
* Params: 
    * file_path

---
### 3- Coloured Coin Wallet(CC Wallet)
#### 3.1- cc_set_name
Set a name for coloured coin wallet.
* Params:
    * wallet_id
    * name
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 2, "name": "GUGU"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/cc_set_name | python -m json.tool
```

#### 3.2- cc_get_name
Get coloured coin wallet name
* Params:
    * wallet_id
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 2}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/cc_get_name | python -m json.tool
```

#### 3.3- cc_spend
Spend coloured coin
* Params:
    * wallet_id
    * inner_address
    * amount: decimal is `1000`
    * fee (option)
* Example: 
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 2, "inner_address": "txch173r0zt6emz74clu6cuvfar8p0pf7gakgu2f2a0y6d05mk4zvwu0qwqmhcf", "amount": 15}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/cc_spend | python -m json.tool

```
  *Bug: Check https://github.com/Chia-Network/chia-blockchain/issues/6477, you have to amend the source code.*

#### 3.4- cc_get_colour
Get colour code of coloured-coin
* Params:
    * wallet_id
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 2}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/cc_get_colour | python -m json.tool
```

#### 3.5- create_offer_for_ids
Make an offer to swap Token A to Token B, you can send this offer as a file to anybody to confirm the trade.
* Params:
    * ids: An object including 2 elements, in each element, the key is wallet_id, and the value is exchange amount. They are always one positive number and one negative number.
    * filename: the offer filename
* Example: 

  Making an offer to send 1xch from wallet#1 and get 0.05 coloured coin in wallet#4

```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"ids": {"1": -1000000000000, "4": 50}, "filename": "erddaewaddfa.offer"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/create_offer_for_ids | python -m json.tool
```

#### 3.6- respond_to_offer
Accept the offer
* Params: 
    * filename
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"filename": "erddaewaddfa.offer"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/respond_to_offer | python -m json.tool
```

#### 3.7- get_discrepancies_for_offer
Get Exchange details of trade.
* Params:
    * filename
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"filename": "erddaewaddfa.offer"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_discrepancies_for_offer | python -m json.tool
```
* Return:
```
{
    "discrepancies": {
        "chia": 1000000000000,
        "ff02ffff01ff02ffff03ffff09ff5bff8080ffff01ff0101ffff01ff02ffff03ffff09ff13ff0280ffff01ff0101ff8080ff018080ff0180ffff04ffff01a0d9448a215a8a96384173272ccda626eafc0c0510ecdcc78b0cc725c235072124ff018080": -50
    },
    "success": true
}
```

#### 3.8- get_trade
* Params: 
    * trade_id
* Example:

```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"trade_id": "e586d1a315f18de83959c90e15dde8d4f8280967a204856b3d382889dcce370a"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_trade | python -m json.tool
```
*BUG: There are bugs in `Wallet`RPC, you have to amend the source: `wallet_rpc_api.py`, find function `get_trade(self, request: Dict)`, change `request["trade_id"]` to `hexstr_to_bytes(request["trade_id"])`*

#### 3.9- get_all_trades
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_all_trades | python -m json.tool
```

#### 3.10- cancel_trade
* Params: 
    * trade_id
    * secure
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"trade_id": "e586d1a315f18de83959c90e15dde8d4f8280967a204856b3d382889dcce370a"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/get_all_trades | python -m json.tool
```

### 4- Rate Limited Wallet(RL Wallet)
#### 4.1- Create RL Wallet
see `create_new_wallet`

#### 4.2- rl_set_user_info
* Params
    * wallet_id
    * origin: object with `{parent_coin_info: string, puzzle_hash: string, amount: number}`, `origin` can get from the response data after create new admin rl_wallet by `create_new_wallet` (See above)
    * interval
    * limit
    * admin_pubkey
* Example
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 5, "orgin": {"parent_coin_info": "0x9e6bb70f450a79caac8f38eb176672a04c651c99964956fa30741bbc7e743436", "puzzle_hash": "0x92cf8450576b98ee910367784304b88049a02073ebd9400c638fcf398ad32351", "amount": 600000000000}, "interval": 3, "limit": 10, "admin_pubkey": "0x811962d408eb6472d4f3657ba684f6d4d37414cabac948c9f8a7b24f13a78bde2491c6a00e40b949f19da0ef23b27301"}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/rl_set_user_info | python -m json.tool
```

*Bug: Open `wallet_rpc_api.py`, find function `rl_set_user_info()`, Change as following*
```
async with self.service.wallet_state_manager.lock:
  await rl_user.set_user_info(
      uint64(request["interval"]),
      uint64(request["limit"]),
      origin["parent_coin_info"],
      origin["puzzle_hash"],
      origin["amount"],
      request["admin_pubkey"],
  )
```
To:
```
async with self.service.wallet_state_manager.lock:
  await rl_user.set_user_info(
      uint64(request["interval"]),
      uint64(request["limit"]),
      origin["parent_coin_info"].hex(),
      origin["puzzle_hash"].hex(),
      origin["amount"],
      request["admin_pubkey"].hex(),
  )
```
After amending code, you should run `chia start wallet -r` to restart `wallet`.

#### 4.3- send_clawback_transaction
* Params:
  * wallet_id
  * fee
* Example
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 5, "fee": 1}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/send_clawback_transaction | python -m json.tool
```

#### 4.4- add_rate_limited_fund
* Params:
    * wallet_id
    * puzzle_hash
    * amount
    * fee
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/wallet/private_wallet.crt \
--key ~/.chia/testnet_7/config/ssl/wallet/private_wallet.key \
-d '{"wallet_id": 5, "puzzle_hash": "0x92cf8450576b98ee910367784304b88049a02073ebd9400c638fcf398ad32351", "amount": 300000000000}' \
-H "Content-Type: application/json" -X POST https://localhost:9256/add_rate_limited_fund | python -m json.tool
```

### 5- Distributed identity wallet(DID Wallet)

### 6- Connecting management
#### 6.1- get_connections
Get all connections
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/daemon/private_daemon.crt --key ~/.chia/testnet_7/config/ssl/daemon/private_daemon.key -d '{}' -H "Content-Type: application/json" -X POST https://localhost:8555/get_connections | python -m json.tool
```

#### 6.2- open_connection
* Params: 
    * host: ex. "node.chia.net" for mainnet, "localhost" for testnet
    * port: ex. 8444 for mainnet, 58444 for testnet
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/daemon/private_daemon.crt --key ~/.chia/testnet_7/config/ssl/daemon/private_daemon.key -d '{"host":"localhost","port":9256}' -H "Content-Type: application/json" -X POST https://localhost:8555/open_connection | python -m json.tool
```

#### 6.3- close_connection
* Params:
    * node_id 
* Example: 
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/daemon/private_daemon.crt --key ~/.chia/testnet_7/config/ssl/daemon/private_daemon.key -d '{"node_id": ""}' -H "Content-Type: application/json" -X POST https://localhost:8555/close_connection | python -m json.tool
```
You can find all connections' `node_id` by `get_connections`.

#### 6.4- stop_node
* Params: None
* Example:
```
curl --insecure --cert ~/.chia/testnet_7/config/ssl/daemon/private_daemon.crt --key ~/.chia/testnet_7/config/ssl/daemon/private_daemon.key -d '{}' -H "Content-Type: application/json" -X POST https://localhost:8555/stop_node | python -m json.tool
```
The post target url will be closed. Above demo command will close port 8555(full node). If you want to close wallet, set port `9256` as `-X POST https://localhost:9256/stop_node`

Attn: If you stoped the node port, you can not open it again from curl or javascript, you have to restart in cli mode or restart the client wallet by `chia start all/wallet/full_node/etc.`

