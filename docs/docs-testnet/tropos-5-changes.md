---
title: Stratos Testnet Changes
description: Stratos Network incentive testnet (Tropos-5) changes.
---

# Highlight Changes in Tropos-5

After testing and building the binaries v0.9.0, we released the latest version of Stratos-Testnet, with a new chain-id: `tropos-5`.

This version covers several breakthrough improvements and significant performance enhancements. We will highlight the major exciting updates in this section. 

<br>

## - EVM Module

The integration of EVM module makes Stratos chain compatible with both Ethereum and Cosmos-based ecosystems, facilitating Stratos to apply a variety of DApps and introduce more features.

<br>

## - Accounts

To compatible with various existing Ethereum DApps, the `address` and `type`(from `secp256k1` to `ethsecp256k1`) for account have been changed.

For example, when checking an account, you will find the differences in `address` and `pub_key`.
  
Before:
  
``` { .yaml .no-copy }
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
  
``` { .yaml .no-copy }
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

<br>

## - Token Denom and Decimal

Most Existing Ethereum DApps have 18 decimals, e.g., 1 ETH is represented by 10e18 of its natural unit ( 1Ether = 1,000,000,000,000,000,000wei ). Similarly, most ERC-20 tokens simply follow this standard.
  
In order to get much greater precision and be consistent with this rule, the previous token denom `ustos` (9 decimals) is NOT in use anymore. Instead, a serial of new denominations `stos`(18 decimals), `gwei`(9 decimals) and `wei` have been brought into effect. That is,
  
``` { .yaml .no-copy }
1stos = 1,000,000,000,000,000,000wei
1stos = 1,000,000,000gwei
1gwei = 1,000,000,000wei
```
  
For simplicity, token amount can be easily represented as `value`+`denom`. In practice, you can enter `1stos`, or `1000000000gwei`, or `1000000000000000000wei` to indicate the same amount of tokens.
  
In `tropos-5`, we keep the same reward issued to Resource Node previously. `utros`(9 decimals) is still the only valid denom for incentive reward as before. i.e.,
  
``` { .yaml .no-copy }
1tros = 1,000,000,000utros
```

<br>

## - Software Upgrade

To avoid two critical vulnerabilities in the Cosmos-SDK ( [Dragonberry](https://forum.cosmos.network/t/ibc-security-advisory-dragonberry/7702) and [Elderflower](https://forum.cosmos.network/t/cosmos-sdk-security-advisory-elderflower/8584), discovered in October 2022), we have upgraded Cosmos-SDK to v0.45.9.
  
Correspondingly, if you prefer to build and compile the binary from source code, you need to have [`Go 1.18+`](https://golang.org/doc/install) installed, rather than the former `Go 1.16+`.

<br>

## - SDS Resource Node Maintenance
    
Resource Node participates in Stratos Decentralized Storage(SDS) activities and thus earns rewards by providing its disk/bandwidth/computation power. It is expected to maintain regular hardware/software updates to prevent from unexpected misbehaves like node crashing, loosing connectivity or getting attacks.
  
In `tropos-5`, `maintenance` functionality has been implemented for users to claim when a maintenance is required, without any worry being slashed.

* `maintenance start <duration>` command starts a maintenance activity and switches the node into `maintenance` mode for the requested duration (in seconds);
* While a Resource Node is in `maintenance` mode, it will be opt-out from any download/upload/backup tasks;
* While a Resource Node is in `maintenance` mode, it will NOT be slashed for its off-line;
* The maintenance allowance is maxed out after reach 1% up-time per year(around 87h). Then, any maintenance request will be rejected;
* The maintenance allowance will be tracked and be reset every calendar year for all nodes;
* When using the `maintenance stop` to stop the current maintenance, or the maintenance period is over, the node status reverts to `offline` and is ready to restart mining. It acts as usual to earn rewards or be slashed.

<br>

## - Restriction of Transaction Fees

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
  
``` { .yaml .no-copy }
minimum-gas-prices = "1000000000wei"
```

Indeed, users can determine their own customized value of `gas-prices` when starting a full node, like
``` { .yaml .no-copy }
./stchaind start --minimum-gas-prices=0.1gwei
```
  
Note: the value of `minimum-gas-prices` should be always greater than `10,000,000wei`(`0.01gwei`).  When the given `--minimum-gas-prices` is less than this threshold, `10,000,000wei` will be taken instead.

<br>

---

<br>