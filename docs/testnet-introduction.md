---
title: Stratos Testnet Introduction
description: Stratos Network incentive testnet (Tropos-5) introduction.
---

## Introduction

The purpose of Tropos Incentive Testnet is to validate all the technical design and network topology that can achieve the goal of our project — to provide a fully decentralized high-efficiency storage. On Incentive testnet we will continue all the testing we employed on testnet; in addition, we will try to ensure the network is functional as in the real world, to build an environment as close to the mainnet as possible.

During the Stratos Incentive Network testing period, we will prove the architecture is working as we designed, verify the network's performance can reach the expected level, and confirm the rewarding mechanism is performing as described in the token economy whitepaper.

Our goal for the incentive testnet is to ensure we can migrate to the mainnet smoothly and provide powerful decentralized storage to real corporate world users.

<br>

---

## Incentive rewards distribution


Tropos Incentive Testnet will offer SDS resource node providers incentive token rewards TROS(1 TROS = 1,000,000,000 utros) for their effort the same way as the mainnet.<br>
`The total rewards will be 1% of the mainnet token supply, meaning 1,000,000 STOS`.

!!! tip ""
    Please be advised that you don't need to use your ERC20 STOS to participate in the incentive testnet. 

    Incentive testnet STOS has no value, only the TROS rewards will have value and TROS will be converted to mainnet STOS.

On the incentive testnet, the distribution will be similar but with some adjustments. All the rewards will not be in the form of testnet STOS; rewards token will be named as TROS which will be issued to the wallet of all participants during the period of incentive testnet. Each epoch will distribute 80 TROS as the mining reward.

First, there will be no meta node for public testing. Thus, rewards issued to meta nodes will be changed to a minimum number only for testing purposes.

Second, the staking token for testnet is distributed for free, so it doesn't reflect the contribution to the network in any sense, we cannot distribute the reward based on staking numbers.

The allocation plan for the reward will also be changed accordingly as follows:

```
🏆 Resource node: 100% of total reward

🏆 Meta node: 0% of total reward

🏆 Staking: 0% of total reward
```

At the end of the incentive testnet, we will calculate the total TROS issued and convert it to mainnet STOS based on the ratio of TROS to Total Incentive STOS, which is 1,000,000 tokens. 

For example, during the incentive testnet there is a total of 2,000,000 TROS issued; this will make the TROS: STOS converting rate be 2:1, which means that every 2 TROS will withdraw 1 STOS from the mainnet STOS pool.

<br>

---

## Highlight Changes in Tropos-5

After testing and building the binaries v0.9.0, we released the latest version of Stratos-Testnet, with a new chain-id: `tropos-5`.

This version covers several breakthrough improvements and significant performance enhancements. We will highlight the major exciting updates in this section. 

<br>


??? tip "EVM Module"
    ### EVM Module

    The integration of EVM module makes Stratos chain compatible with both Ethereum and Cosmos-based ecosystems, facilitating Stratos to apply a variety of DApps and introduce more features.



??? tip "Accounts"
    ### Accounts

    To compatible with various existing Ethereum DApps, the `address` and `type`(from `secp256k1` to `ethsecp256k1`) for account have been changed.

    For example, when checking an account, you will find the differences in `address` and `pub_key`.
  
    Before:
  
    ```
    ./stchaind query account st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
    |
    '@type': /cosmos.auth.v1beta1.BaseAccount
    account_number: "4"
    address: st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
    pub_key:
      "@type":"/cosmos.crypto.secp256k1.PubKey"
      key: AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v
    sequence: "3"
    ```
  
  
    **Tropos-5**:
  
    ```
    ./stchaind query account st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
    |
    '@type': /cosmos.auth.v1beta1.BaseAccount
    account_number: "5"
    address: st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
    pub_key:
      '@type': /stratos.crypto.v1.ethsecp256k1.PubKey
      key: AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v
    sequence: "1"
    ```
  
    From `tropos-5` on, all the accounts will be identified by `ethsecp256k1`, while the `secp256k1` will be unavailable.



??? tip "Token Denom and Decimal"
    ### Token Denom and Decimal

    Most Existing Ethereum DApps have 18 decimals, e.g., 1 ETH is represented by 10e18 of its natural unit ( 1Ether = 1,000,000,000,000,000,000wei ). Similarly, most ERC-20 tokens simply follow this standard.
  
    In order to get much greater precision and be consistent with this rule, the previous token denom `ustos` (9 decimals) is NOT in use anymore. Instead, a serial of new denominations `stos`(18 decimals), `gwei`(9 decimals) and `wei` have been brought into effect. That is,
  
    ```
    1stos = 1,000,000,000,000,000,000wei
    1stos = 1,000,000,000gwei
    1gwei = 1,000,000,000wei
    ```
  
    For simplicity, token amount can be easily represented as `value`+`denom`. In practice, you can enter `1stos`, or `1000000000gwei`, or `1000000000000000000wei` to indicate the same amount of tokens.
  
    In `tropos-5`, we keep the same reward issued to Resource Node previously. `utros`(9 decimals) is still the only valid denom for incentive reward as before. i.e.,
  
    ```
    1tros = 1,000,000,000utros
    ```



??? tip "Software Upgrade"
    ### Software Upgrade

    To avoid two critical vulnerabilities in the Cosmos-SDK ( [Dragonberry](https://forum.cosmos.network/t/ibc-security-advisory-dragonberry/7702) and [Elderflower](https://forum.cosmos.network/t/cosmos-sdk-security-advisory-elderflower/8584), discovered in October 2022), we have upgraded Cosmos-SDK to v0.45.9.
  
    Correspondingly, if you prefer to build and compile the binary from source code, you need to have [`Go 1.18+`](https://golang.org/doc/install) installed, rather than the former `Go 1.16+`.



??? tip "SDS Resource Node Maintenance"
    ### SDS Resource Node Maintenance
    
    Resource Node participates in Stratos Decentralized Storage(SDS) activities and thus earns rewards by providing its disk/bandwidth/computation power. It is expected to maintain regular hardware/software updates to prevent from unexpected misbehaves like node crashing, loosing connectivity or getting attacks.
  
    In `tropos-5`, `maintenance` functionality has been implemented for users to claim when a maintenance is required, without any worry being slashed.
  
    * `maintenance start <duration>` command starts a maintenance activity and switches the node into `maintenance` mode for the requested duration (in seconds);
    * While a Resource Node is in `maintenance` mode, it will be opt-out from any download/upload/backup tasks;
    * While a Resource Node is in `maintenance` mode, it will NOT be slashed for its off-line;
    * The maintenance allowance is maxed out after reach 1% up-time per year(around 87h). Then, any maintenance request will be rejected;
    * The maintenance allowance will be tracked and be reset every calendar year for all nodes;
    * When using the `maintenance stop` to stop the current maintenance, or the maintenance period is over, the node status reverts to `offline` and is ready to restart mining. It acts as usual to earn rewards or be slashed.



??? tip "Restriction of Transaction Fees"
    ### Restriction of Transaction Fees

    Before `tropos-5`, transaction fees are not strictly required. To reflect scenarios on mainnet, in `tropos-5`, transaction fees become a required parameter(s) in each transaction command.
  
    Transaction fees in `tropos-5` follows cosmos-SDK gas calculating rule `fees = gas * gas-prices`. As a result, there are two options to add up transaction fees to a command,
  
    * use `--fees=<fees amount>` 
    * e.g., `--fees=10000gwei`
  
    Or

    * use `--gas=<gas amount>/auto --gas-prices=<gas price>`
    * e.g., `--gas=100000 --gas-prices=0.1gwei`
  
    Or

    * `--gas=auto --gas-prices=0.01gwei`(to use the estimated amount of gas)
  
    In `tropos-5`, a `minimum-gas-prices` is defined by default in `config/app.toml` under the node folder:
  
    ```
    minimum-gas-prices = "1000000000wei"
    ```
  
    Indeed, users can determine their own customized value of `gas-prices` when starting a full node, like
    ```
    ./stchaind start --minimum-gas-prices=0.1gwei
    ```
  
    Note: the value of `minimum-gas-prices` should be always greater than `10,000,000wei`(`0.01gwei`).  When the given `--minimum-gas-prices` is less than this threshold, `10,000,000wei` will be taken instead.

<br>

---

## Migrate Rewards to `Tropos-5`

!!! tip ""

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

!!! tip ""
    #### Breakdown
    
  Let's break down this task step by step with an example.
  
  Suppose you have an existing `secp256k1` account. It was created as
  ```
  name: cosmosUser0
  address: st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
  mnemonic: "ocean jar useless fame slice history retreat banana life wing lesson work amount adult tray zone
  payment window since dash immune grunt army deer"
  ```

<br>

---

!!! tip ""
    #### Backup old wallet
    
  - Backup the previous `secp256k1` account files

    By default, under the node folder `$HOME/.stchaind`, there is `keyring-test` folder which holds the accounts' info.
    It looks like
      ```
      .
      ├── b37acf3e6daa547c8034d27893720a9fcf3eea44.address
      └── cosmosUser0.info
      ```
    Copy this `keyring-test` folder to a secure place.<br><br>

- Download the binary and config files for `tropos-5`. Follow the instructions in the next sections until the `Setup a wallet and faucet` step. Next, you need to create an `ethsecp256k1` account based on your old `secp256k1` account.

<br>

---

!!! tip ""
    #### Create new wallet
    
  - Create(Recover) a new corresponding `ethsecp256k1` account:

<br>

!!! tip ""
    ##### Using mnemonic

Using the same mnemonic(recommended)
    
➡️ Remove all the files in `keyring-test` folder

➡️ Recover your account with the same mnemonic, i.e.,

```
./stchaind keys add ethUser0 --recover --chain-id=tropos-5 --keyring-backend=test --hd-path="m/44'/606'/0'/0/0"
```

Result:

```
> Enter your bip39 mnemonic
   ocean jar useless fame slice history retreat banana life wing lesson work amount adult tray zone payment window since dash immune grunt army deer

- name: ethUser0
   address: st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
   pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
   mnemonic: ""
```

<br>

!!! tip ""
    ##### Using stchaind

Using `stchaind keys` commands
    
This method uses `stchaind keys` commands to convert your old `secp256k1` account into a new `ethsecp256k1` account in case you don't have the mnemonic words saved. It is an unsafe method  because your private/public key is explicitly print out in the terminal. Please do this on your own hardware machine instead of cloud server. 
      
➡️ Keep all the files in `keyring-test` folder

➡️ Export `cosmosUser0`
    
```
./stchaind keys export cosmosUser0 --keyring-backend=test --unsafe --unarmored-hex
```

Result:
```      
WARNING: The private key will be exported as an unarmored hexadecimal string. USE AT YOUR OWN RISK. Continue? [y/N]: y
18132eae6f96851959d2787aab9f018d9cfe977c619ee951490c6a1c692f2783          ## exported-key-hex
```

➡️ Delete the old `cosmosUser0` account
      
```
./stchaind keys delete cosmosUser0 --keyring-backend=test 
```

➡️ Unsafe-import-eth-key `ethUser0`
      
```
./stchaind keys unsafe-import-eth-key ethUser0 18132eae6f96851959d2787aab9f018d9cfe977c619ee951490c6a1c692f2783 --keyring-backend=test
```

Result:

```      
Enter passphrase to encrypt your key:   ## if you are using `--keyring-backend=test` in your command, use `12345678` as the passphrase
```

!!! info ""

    You should use default passphrase `12345678` if you are using `--keyring-backend=test`. 

    You can use your own if you are not using the `test` keyring-backend.
    

➡️ Confirm account conversion

```
./stchaind keys list --keyring-backend=test
```

Result:
```      
- name: ethUser0
 type: local
 address: st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
 pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
 mnemonic: ""                                       
```
  
<br>

---

!!! tip ""
    #### Activate new wallet

- Acquire `stos` tokens to activate the newly created `ethUser0` account

For example, you can `faucet` your account (please replace the value of `address` with your real account address)

```
curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4"}' https://faucet-tropos.thestratos.org/credit
```

<br>

---

!!! tip ""
    #### Start the chain

- Start the chain and wait for full synchronization

 ```
 ./stchaind start
 ```

<br>

---

!!! tip ""
    #### Check balance

- Check the balance in the new `ethsecp256k1` account

```
./stchaind query bank balances <your new `ethsecp256k1` account address>
```

The response to this command determines if your account has been migrated automatically. For example,

<br>

!!! tip ""
    ##### Automated migration

➡️ With a balance of `utros`:


```
./stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
balances:
- amount: "233606"
  denom: utros
- amount: "10000000000000000000"
  denom: wei
  pagination:
  next_key: null
  total: "0"
```

!!! tip ""

    In the above example, the balance includes `233606 utros`. It indicates your account has been migrated automatically. 

    Please skip the following steps, as you can operate the same as before.

<br>

!!! tip ""
    ##### Manual migration

➡️ Without a balance of `utros`
  
```
./stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
balances:
- amount: "10000000000000000000"
  denom: wei
  pagination:
  next_key: null
  total: "0"
``` 

!!! tip ""

    The response does not include a `utros` balance, while you actually had it in the old `secp256k1` account. 

    In other words, if you find your `utros` balance is missing, please follow the next steps to migrate the incentive reward to the new `ethsecp256k1` account manually.

<br>

!!! tip ""
    ###### Check rewards

- Check the rewards in the old `secp256k1` account

```
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

!!! tip ""

    The above response shows that there are `1696408013053utros` of mature reward held in your old `secp256k1` account. 

    Next we will withdraw `100ustros` to the new `ethUser0` account for instance.

<br>

!!! tip ""
    ###### Withdraw rewards

  - Withdraw mature reward from `secp256k1` account to the new corresponding `ethsecp256k1` account using `legacy-withdraw` command

```
./stchaind tx pot legacy-withdraw --target-address=st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4 --from=st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4 --amount=100utros --chain-id=tropos-5 --keyring-backend=test --gas=1000000 --gas-prices=1000000000wei -y
```

<br>

!!! tip ""
    ###### Verify

- Check the balance in the new `ethsecp256k1` account again

```
 ./stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
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

- Check the balance in the old `secp256k1` account again

```
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