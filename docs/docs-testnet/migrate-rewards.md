---
title: Stratos Testnet Migrate Rewards
description: Stratos Network incentive testnet (Tropos-5) how to migrate rewards from previous testnets.
---

# Migrate Rewards to Tropos-5

!!! tip

    * For newcomers, just skip this section and follow the instructions in the rest of this guide to set up `tropos-5`.


    * For existing users, the incentive reward issued previously will be kept with the same private/public key combination.
    However, as mentioned above, the account address will be changed in `tropos-5`.

  The balance in the new `ethsecp256k1` account address will keep the same as that in the old `secp256k1` account address.
  If it is impossible to retrieve the old secp256k1 account public key, resulting in the new `ethsecp256k1` account
  cannot be generated based on the old one, the balance will be transferred to the `mature reward` of the old `secp256k1` account.
  In this case, you need to migrate the reward to your new `ethsecp256k1` account.

  Do not worry about the balance. The only thing you need to do is to check your incentive reward of the old `secp256k1` account
  and withdraw it to the new `ethsecp256k1` account.

<br>

---

## Breakdown
    
  Let's break down this task step by step with an example.
  
  Suppose you have an existing `secp256k1` account. It was created as

  ``` { .yaml .no-copy }
  name: cosmosUser0
  address: st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
  mnemonic: "ocean jar useless fame slice history retreat banana life wing lesson work amount adult tray zone
  payment window since dash immune grunt army deer"
  ```

<br>

---

## Backup old wallet
    
- Backup the previous `secp256k1` account files

By default, under the node folder `$HOME/.stchaind`, there is `keyring-test` folder which holds the accounts' info.

It looks like

``` { .yaml .no-copy }
.
├── b37acf3e6daa547c8034d27893720a9fcf3eea44.address
└── cosmosUser0.info
```

Copy this `keyring-test` folder to a secure place.<br><br>

- Download the binary and config files for `tropos-5`. Follow the instructions in the next sections until the `Setup a wallet and faucet` step. Next, you need to create an `ethsecp256k1` account based on your old `secp256k1` account.

<br>

---

## Create new wallet
    
- Create(Recover) a new corresponding `ethsecp256k1` account:

<br>

### Using mnemonic

Using the same mnemonic(recommended)
    
- Remove all the files in `keyring-test` folder

- Recover your account with the same mnemonic, i.e.,

```shell
stchaind keys add ethUser0 --recover --chain-id=tropos-5 --keyring-backend=test --hd-path="m/44'/606'/0'/0/0"
```

Result:

``` { .yaml .no-copy }
> Enter your bip39 mnemonic
   ocean jar useless fame slice history retreat banana life wing lesson work amount adult tray zone payment window since dash immune grunt army deer

- name: ethUser0
   address: st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
   pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
   mnemonic: ""
```

<br>

### Using stchaind

Using `stchaind keys` commands
    
This method uses `stchaind keys` commands to convert your old `secp256k1` account into a new `ethsecp256k1` account in case you don't have the mnemonic words saved. It is an unsafe method  because your private/public key is explicitly print out in the terminal. Please do this on your own hardware machine instead of cloud server. 
      
- Keep all the files in `keyring-test` folder

- Export `cosmosUser0`
    
```shell
stchaind keys export cosmosUser0 --keyring-backend=test --unsafe --unarmored-hex
```

Result:
``` { .yaml .no-copy } 
WARNING: The private key will be exported as an unarmored hexadecimal string. USE AT YOUR OWN RISK. Continue? [y/N]: y
18132eae6f96851959d2787aab9f018d9cfe977c619ee951490c6a1c692f2783          ## exported-key-hex
```

<br>

- Delete the old `cosmosUser0` account
      
```shell
stchaind keys delete cosmosUser0 --keyring-backend=test 
```

<br>

- Unsafe-import-eth-key `ethUser0`
      
```shell
stchaind keys unsafe-import-eth-key ethUser0 18132eae6f96851959d2787aab9f018d9cfe977c619ee951490c6a1c692f2783 --keyring-backend=test
```

Result:

``` { .yaml .no-copy }    
Enter passphrase to encrypt your key:   ## if you are using `--keyring-backend=test` in your command, use `12345678` as the passphrase
```

<br>

!!! tip

    You should use default passphrase `12345678` if you are using `--keyring-backend=test`. 

    You can use your own if you are not using the `test` keyring-backend.
    

<br>

- Confirm account conversion

```shell
stchaind keys list --keyring-backend=test
```

Result:
``` { .yaml .no-copy } 
- name: ethUser0
 type: local
 address: st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
 pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
 mnemonic: ""                                       
```
  
<br>

---

## Activate new wallet

- Acquire `stos` tokens to activate the newly created `ethUser0` account

For example, you can `faucet` your account (please replace the value of `address` with your real account address)

```shell
curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4"}' https://faucet-tropos.thestratos.org/credit
```

<br>

---

## Start the chain

- Start the chain and wait for full synchronization

 ```shell
 stchaind start
 ```

<br>

---

## Check balance

- Check the balance in the new `ethsecp256k1` account

```shell
stchaind query bank balances <your new `ethsecp256k1` account address>
```

The response to this command determines if your account has been migrated automatically. For example,

<br>

### With a balance of _utros_

``` { .yaml .no-copy }
stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
balances:
- amount: "233606"
  denom: utros
- amount: "10000000000000000000"
  denom: wei
  pagination:
  next_key: null
  total: "0"
```

!!! tip

    In the above example, the balance includes `233606 utros`. It indicates your account has been migrated automatically. 

    Please skip the following steps, as you can operate the same as before.

<br>

### Without a balance of _utros_
  
``` { .yaml .no-copy }
stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
balances:
- amount: "10000000000000000000"
  denom: wei
  pagination:
  next_key: null
  total: "0"
``` 

!!! tip

    The response does not include a `utros` balance, while you actually had it in the old `secp256k1` account. 

    In other words, if you find your `utros` balance is missing, please follow the next steps to migrate the incentive reward to the new `ethsecp256k1` account manually.

<br>

- Check the rewards in the old `secp256k1` account

```shell
curl https://rest-tropos.thestratos.org/pot/rewards/wallet/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
```

Response Example:

```json
{
    "height": "1826",
    "result": {
        "wallet_address": "st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg",
        "MatureTotalReward": [
            {
                "denom": "utros",
                "amount": "1696408013053"
            },
            {
                "denom": "wei",
                "amount": "3576778"
            }
        ],
        "ImmatureTotalReward": [
            {
                "denom": "utros",
                "amount": "872451398"
            },
            {
                "denom": "wei",
                "amount": "278865"
            }
        ]
    }
}
```

!!! tip

    The above response shows that there are `1696408013053utros` of mature reward held in your old `secp256k1` account. 

    Next we will withdraw `100ustros` to the new `ethUser0` account for instance.

<br>

- Withdraw mature reward from `secp256k1` account to the new corresponding `ethsecp256k1` account using `legacy-withdraw` command

```shell
stchaind tx pot legacy-withdraw --target-address=st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4 --from=st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4 --amount=100utros --chain-id=tropos-5 --keyring-backend=test --gas=1000000 --gas-prices=1000000000wei -y
```

<br>

- Check the balance in the new `ethsecp256k1` account again

``` { .yaml .no-copy }
 stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
  balances:
   - amount: "100"
     denom: utros
   - amount: "9999000000000000000"
     denom: wei
 pagination:
 next_key: null
 total: "0"
```

An amount of `100ustros` mature reward has been transferred to the new account.

<br>

- Check the balance in the old `secp256k1` account again

```shell
curl https://rest-tropos.thestratos.org/pot/rewards/wallet/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
```

Response Example:

```json
{
    "height": "1937",
    "result": {
        "wallet_address": "st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg",
        "MatureTotalReward": [
            {
                "denom": "utros",
                "amount": "1696408012953"
            },
            {
                "denom": "wei",
                "amount": "3576778"
            }
        ],
        "ImmatureTotalReward": [
            {
                "denom": "utros",
                "amount": "872451398"
            },
            {
                "denom": "wei",
                "amount": "278865"
            }
        ]
    }
}
```

Correspondingly, an amount of `100ustros` mature reward has been withdrawn from the old account.

Similarly, you can withdraw all the mature rewards to the new `ethsecp256k1` account using `legacy-withdraw` command. Note that only mature reward can be withdrawn immediately.

You have migrated your TROS reward to tropos-5 successfully.

<br>

---

<br>