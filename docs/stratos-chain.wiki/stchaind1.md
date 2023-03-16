# Overview

This document is the first part that describes a summarized list of `stchaind` commands for Stratos Chain.

## Hardware and Software Requirements
Unlike other projects, Stratos does not require expensive GPUs and high wattage power supplies,
but if the node wants to obtain revenue, it needs to provide enough bandwidth and storage capacity to ensure the traffic
on the node can reach the reward requirements. We recommend the following to run your node:
```shell
* CPU i5 4 cores
* 16GB memory
* 2TB hard disk
```
Software(tested version):
```shell
* Ubuntu 18.04+
* Go 1.18+ linux/amd64 (optional, if compile the binary with source code)
```

## Get Connected to Stratos Chain Testnet
please refer to [Tropos-Incentive-Testnet](https://github.com/stratosnet/sds/wiki/Tropos-Incentive-Testnet#Topics) to
- [x] download related files
- [x] start your node to catch up to the latest block height(synchronization)
- [x] create your Stratos Chain Wallet
- [x] `Faucet` or `send` an amount of tokens to this wallet

## Directory Structure
After the node has caught up to the latest block, your Stratos-chain Wallet has been created and fed with an amount of tokens,
`$HOME/.stchaind` directory will include the following directories and files.

```shell
.
├── config
│   ├── addrbook.json
│   ├── app.toml
│   ├── client.toml
│   ├── config.toml
│   ├── genesis.json
│   ├── node_key.json
│   └── priv_validator_key.json
├── data
│   ├── application.db
│   ├── blockstore.db
│   ├── cs.wal
│   ├── evidence.db
│   ├── priv_validator_state.json
│   ├── snapshots
│   ├── state.db
│   └── tx_index.db
└── keyring-test
    ├── 6894f6eef2b730a5f071eed1f3aeb471dfeeeaaf.address
    ├── d6052b289b78468612a8f97cf59eac184ba852dd.address
    ├── d704353fe67f948c99d2e3105adc5159c9e8f2af.address
    ├── f07ab66406c02aa1a398f4fa41a91192fae08997.address
    ├── fdb03146cb5a83e08785e8d1f083132d4386b4bd.address
    ├── user0.info
    ├── user10.info
    ├── user1.info
    ├── user2.info
    └── user3.info
```
> In `stchaind/config` folder:
> * `addrbook.json` stores peer addresses.
> * `app.toml` contains the default settings required for app.
> * `config.toml` contains various options pertaining to the stratos-chain configurations.
> * `genesis.json` defines the initial state upon genesis of stratos-chain.
> * `node_key.json` contains the node private key and should thus be kept secret.
> * `priv_validator_key.json` contains the validator address, public key and private key, and should thus be kept secret.

> In `stchaind/data` folder:
> * All `*.db` folders are `Tendermint` databases
> * `Tendermint` uses a `write ahead log` (WAL) for consensus
> * `priv_validator_state.json`holds the validator's state
    > By default, the binary executable `stchaind` has been saved or created in the `$HOME` folder. If you are not sure what is your `$HOME` folder, in terminal, use `echo $HOME` to check. In the following instruction, we suppose you have entered the `$HOME` folder(use `cd $HOME`)

> `stchaind/keyring-test` folder holds the user's information and address in the keyring-backend 


In Linux:
```shell
$ echo $HOME
/home/<your login name>
```

In Mac:
```shell
$ echo $HOME
/Users/<your login name>
```

# 'stchaind' Commands
For ease of use, these commands have been classified by the following modules:

* [`Bank`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part1)#bank-module)
* [`Distribution`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part1)#distribution-module)
* [`Gov`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part1)#gov-module)
* [`Slashing`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part1)#slashing-module)
* [`Staking`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part1)#staking-module)
* [`Register`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part2)#register-module)
* [`Sds`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part2)#sds-module)
* [`Pot`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part2)#pot-module)
* [`Tendermint`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part2)#tendermint)
* [`Others`](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part2)#others)

## Global Flags
Each command has its specific flags as well as several global flags. The specific flags will be explained along with each command,
while the global flags are summarized as the following that can be used for all `stchaind` commands.
```shell
Global Flags(can be used for all stchaind commands):
  -b, --broadcast-mode string    Transaction broadcasting mode (sync|async|block) (default "sync")
      --chain-id string          Specify Chain ID for sending Tx (default "testnet")
      --fees string              Fees to pay along with transaction; eg: 10wei
      --from string              Name or address of private key with which to sign
      --gas-adjustment float     adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string        Gas prices to determine the transaction fee (e.g. 10wei)
  -h, --help                     help for stchaind
      --home string              directory for config and data (default "/home/hong/.stchaind")
      --keyring-backend string   Select keyring's backend (default "os")
      --log_format string        The logging format (json|plain) (default "plain")
      --log_level string         The logging level (trace|debug|info|warn|error|fatal|panic) (default "info")
      --node string              <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --trace                    print out full stack trace on errors

```
| 👉 : `--chain-id`: the current `chain-id` may change when updating in testing phase . When it is applied, user needs to point out current `chain-id` which can be found on [this page](https://explorer-tropos.thestratos.org/), right next to the search bar at the top of the page |
|:-------------------------------------------------|

| 👉 : `--home`: this directory contains node's account information. By default, node's account info is saved or created under `$HOME/.stchaind`. In this case, user does not need to add `--home` flag in the commands. Otherwise, user has to use this flag to specify the path to the node's root directory(default '$HOME') explicitly if not using the default directory. In the following instruction, we suppose the node info has been installed or created under `$HOME/.stchaind` and skip the `--home` flag. User can add it where applicable |
|:-------------------------------------------------|


## Bank Module

### Transaction

#### -[`send`](#-send)
Create and sign a `send` transaction.
```shell
Usage:
  stchaind tx bank send [from_key_or_address] [to_address] [amount] [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for send
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes 
  In testing phase, --keyring-backend="test"
```
<details>
  <summary><b>Example</b></summary>

```shell
./stchaind tx bank send user0 st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx 1gwei --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei -y
```
</details>
<br>

### Query

#### [`balances`](#balances)
query account bank balances.

```shell
Usage:
  stchaind query bank balances [account address] [flags]

Flags:
      --count-total       count total number of records in all balances to query for
      --denom string      The specific balance denomination to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for balances
      --limit uint        pagination limit of all balances to query for (default 100)
      --offset uint       pagination offset of all balances to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of all balances to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of all balances to query for
      --reverse           results are sorted in descending order
```

<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query bank balances st16czjk2ym0prgvy4gl970t84vrp96s5kayfqmf2
```
</details>

***

## Distribution Module

### Transactions

#### -[`withdraw-rewards`](#-withdraw-rewards)
Withdraw rewards from a given delegation address and optionally withdraw validator's commission if the delegation address given is a validator operator.
```shell
Usage:
  stchaind tx distribution withdraw-rewards [validator-addr] [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --commission            Withdraw the validator's commission in addition to the rewards
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for withdraw-rewards
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation

  In testing phase, --keyring-backend="test"
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx distribution withdraw-rewards stvaloper1fmdh9vf262qxe5ehmp9jvgkqzaeye4qmxjrr3k --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei -y
```
</details>
<br>

#### -[`withdraw-all-rewards`](#-withdraw-all-rewards)
Withdraw all delegation rewards for a delegator.
```shell
Usage:
  stchaind tx distribution withdraw-all-rewards [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for withdraw-all-rewards
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --max-msgs int          Limit the number of messages per tx (0 for unlimited)
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation
  
  In testing phase, --keyring-backend="test"
```

<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx distribution withdraw-all-rewards --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei -y
```
</details>
<br>

### Queries

#### [`commission`](#commission)
Query validator commission rewards from delegators to that validator.
```shell
Usage:
  stchaind query distribution commission [validator] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for commission
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><code>commission</code>Example</summary>

```shell
$ ./stchaind query distribution commission stvaloper1gtw399h9vfnekqsz3dg4n6mj0qgdpnh30x66xa
```
</details>
<br>

#### [`rewards`](#rewards)
Query all rewards earned by a delegator, optionally restrict to reward from a single validator.
```shell
Usage:
  stchaind query distribution rewards [delegator-addr] [validator-addr] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for rewards
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><code>rewards</code>Example</summary>

```shell
$ ./stchaind query distribution rewards st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --height=9765
```
</details>
<br>

#### [`validator-outstanding-rewards`](#validator-outstanding-rewards)
Query distribution outstanding (un-withdrawn) rewards for a validator and all their delegations.
```shell
Usage:
  stchaind query distribution validator-outstanding-rewards [validator] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for validator-outstanding-rewards
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><code>validator-outstanding-rewards</code>Example</summary>

```shell
$ ./stchaind query distribution validator-outstanding-rewards stvaloper1fmdh9vf262qxe5ehmp9jvgkqzaeye4qmxjrr3k
```
</details>
<br>

#### [`community-pool`](#community-pool)
Query all coins in the community pool which is under Governance control.
```shell
Usage:
  stchaind query distribution community-pool [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for community-pool
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><code>community-pool</code>Example</summary>

```shell
$ ./stchaind query distribution community-pool --height=9765
```
</details>
<br>

#### [`slashes`](#slashes)
Query all slashes of a validator for a given block range.
```shell
Usage:
  stchaind query distribution slashes [validator] [start-height] [end-height] [flags]

Flags:
      --count-total       count total number of records in validator slashes to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for slashes
      --limit uint        pagination limit of validator slashes to query for (default 100)
      --offset uint       pagination offset of validator slashes to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of validator slashes to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of validator slashes to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><code>slashes</code>Example</summary>

```shell
$ ./stchaind query distribution slashes stvaloper1095s2f3m60qz48spy3wr52gw8xmy7xqywnxnrq 0 500
```
</details>
<br>

#### [`distribution-params`](#distribution-params)
Query distribution params.
```shell
Usage:
  stchaind query distribution params [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for params
  -o, --output string   Output format (text|json) (default "text")

```
<details>
  <summary><code>params</code>Example</summary>

```shell
$ ./stchaind query distribution params
```
</details>
<br>

***


## Gov Module

### Transactions

#### -[`submit-proposal`](#-submit-proposal)

Submit a proposal along with an initial deposit. Proposal title, description, type and deposit can be given directly or through a proposal JSON file.

> 💡 <b>In the `Tropos Incentive Testnet` testing phase, the deposit should use `utros`(NOT `stos`, `gwei` or `wei`). 
> Otherwise, you will get an error and the deposit tokens won't be back.</b>
>
> <b>The minimum deposit is `10000000000utros`</b>

Except for itself, the command `submit-proposal` also provides three sub-commands, `param-change`,
`community-pool-spend` and `software-upgrade`, to submit a proposal for changing global parameters,
distributing funds in `community-pool` and upgrading software.

```shell
Usage:
  stchaind tx gov submit-proposal [flags]
  stchaind tx gov submit-proposal [command]

Available Commands:
  cancel-software-upgrade Cancel the current software upgrade proposal
  community-pool-spend    Submit a community pool spend proposal
  param-change            Submit a parameter change proposal
  software-upgrade        Submit a software upgrade proposal

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --deposit string        The proposal deposit
      --description string    The proposal description
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for submit-proposal
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
      --proposal string       Proposal file path (if this path is given, other proposal flags are ignored)
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
      --title string          The proposal title
      --type string           The proposal Type
  -y, --yes                   Skip tx broadcasting prompt confirmation

  
  In testing phase, --keyring-backend="test"
```
<details>
    <summary><b><code>submit-proposal</code> Example </b></summary>

```shell
stchaind tx gov submit-proposal <proposal.json> --from=<Name|address of private key>
```
Where `proposal.json` contains:
```json
    {
      "title": "Test Proposal",
      "description": "My awesome proposal",
      "type": "Text",
      "deposit": "100000000000utros"
    }
```
Which is equivalent to:
```shell
stchaind tx gov submit-proposal --title="Test Proposal" --description="My awesome proposal" --type="Text" --deposit="100000000000utros" --from=<name|address of private key>
```

`submit-proposal` Tx command:

```shell
$ ./stchaind tx gov submit-proposal --title="Test Proposal" --description="My awesome proposal" --type="Text" --deposit="100000000000utros" --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>

<details>
<summary><b><code>param-change</code> Example</b></summary>

Submit a parameter proposal along with an initial deposit. The proposal details must be supplied via a JSON file.
For values that contains objects, only non-empty fields will be updated.
```shell
Usage:
  stchaind tx gov submit-proposal param-change [proposal-file] [flags]
```

`param-change` tx command:
```shell
stchaind tx gov submit-proposal param-change <proposal-file> --from=<name|address of private key> --chain-id=<current chain-id> --keyring-backend=<keyring's backend'> --gas=auto --gas-prices=1000000000wei
```
A sample of `param_change.json` could be:

```json
{
    "title": "Param-Change",
    "description": "This is a test to update deposit params in gov Module",
    "changes": [
        {
            "subspace": "gov",
            "key": "depositparams",
            "value": {"max_deposit_period":"72800000000000"}
        }
    ],
    "deposit": "1000000000000utros"
}
```

```shell
$ ./stchaind tx gov submit-proposal param-change ./helpers/param_change.json --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>

<details>
<summary><b><code>community-pool-spend</code> Example</b></summary>

Submit a community pool spend proposal along with an initial deposit. The proposal details must be supplied via a JSON file.
```shell
Usage:
  stchaind tx gov submit-proposal community-pool-spend [proposal-file] [flags]
```

`community-pool-spend` tx command:

The `proposal.json` could be:
```json
    {
      "title": "Community Pool Spend",
      "description": "Pay me some STOSes!",
      "recipient": "st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda",
      "amount": [
        {
          "denom": "wei",
          "amount": "1000000000000"
        }
      ],
      "deposit": [
        {
          "denom": "utros",
          "amount": "1000000000000"
        }
      ]
    }
```

```shell
$ ./stchaind tx gov submit-proposal community-pool-spend ./helpers/proposal.json --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>

<details>
<summary><b><code>software-upgrade</code> Example</b></summary>

Submit a software upgrade along with an initial deposit.
```shell
Usage:
  stchaind tx gov submit-proposal software-upgrade [name] (--upgrade-height [height]) (--upgrade-info [info]) [flags]
```

`software-upgrade` tx command:
```shell
stchaind tx gov submit-proposal software-upgrade="v0.3.1" --upgrade-height=1000 --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --description=test1 --title=test1 --deposit=100000000000utros --info=testinfo --chain-id=tropos-5 --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>
<br>

#### -[`deposit`](#-deposit)
Deposit tokens for an active proposal by `proposal-id` which can be found with the command `stchaind query gov proposals`.

> 💡 <b>In the `Tropos Incentive Testnet` testing phase, the deposit should use `utros`, NOT `stos`, `gwei` or `wei`. 
> Otherwise, you will get an error and the deposit tokens won't be back.</b>
>
> <b>The minimum deposit is `10000000000utros`</b>


```shell
Usage:
  stchaind tx gov deposit [proposal-id] [deposit] [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for deposit
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation
  
  In testing phase, --keyring-backend="test"
```

<details>
  <summary><b>Example</b></summary>


```shell
$ ./stchaind tx gov deposit 7 100000000utros --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>
<br>

#### -[`vote`](#-vote)
Submit a vote for an active proposal. Vote options include `yes`/`no`/`no_with_veto`/`abstain`.
```shell
Usage:
  stchaind tx gov vote [proposal-id] [option] [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for vote
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation
    
    In testing phase, --keyring-backend="test"
```

<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx gov vote 7 yes --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>
<br>

### Queries

#### [`proposal`](#proposal)
Query details for a proposal. You can find the `proposal-id` by running `./stchaind query gov proposals`
```shell
Usage:
  stchaind query gov proposal [proposal-id] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for proposal
  -o, --output string   Output format (text|json) (default "text")
```

<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query gov proposal 7
    content:
      title: Param-Change Staking MaxValidators to 100
      description: This is a test to update MaxValidators to 100 in staking Module
      changes:
      - subspace: staking
        key: MaxValidators
        value: "100"
    id: 7
    proposal_status: 3
    final_tally_result:
      "yes": "400000000"
      abstain: "0"
      "no": "0"
      no_with_veto: "0"
    submit_time: 2021-07-23T14:40:04.976927421Z
    deposit_end_time: 2021-07-23T14:41:44.976927421Z
    total_deposit:
    - denom: utros
      amount: "100010000"
    voting_start_time: 2021-07-23T14:40:41.961523583Z
    voting_end_time: 2021-07-23T14:42:21.961523583Z
```
</details>
<br>

#### [`proposals`](#proposals)
Query details of all proposals with optional filters(flags).
```shell
Usage:
  stchaind query gov proposals [flags]

Flags:
      --count-total        count total number of records in proposals to query for
      --depositor string   (optional) filter by proposals deposited on by depositor
      --height int         Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help               help for proposals
      --limit uint         pagination limit of proposals to query for (default 100)
      --offset uint        pagination offset of proposals to query for
  -o, --output string      Output format (text|json) (default "text")
      --page uint          pagination page of proposals to query for. This sets offset to a multiple of limit (default 1)
      --page-key string    pagination page-key of proposals to query for
      --reverse            results are sorted in descending order
      --status string      (optional) filter proposals by proposal status, status: deposit_period/voting_period/passed/rejected
      --voter string       (optional) filter by proposals voted on by voted

    stchaind query gov proposals --depositor st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
    stchaind query gov proposals --voter st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
    stchaind query gov proposals --status (DepositPeriod|VotingPeriod|Passed|Rejected)
    stchaind query gov proposals --page=2 --limit=100
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query gov proposals
    - content:
        title: Param-Change Staking MaxValidators to 5
        description: This is a test to update MaxValidators to 5 in staking Module
        changes:
        - subspace: staking
          key: MaxValidators
          value: "5"
      id: 1
      proposal_status: 3
      final_tally_result:
        "yes": "383333332"
        abstain: "0"
        "no": "0"
        no_with_veto: "0"
      submit_time: 2021-07-19T15:38:08.619640056Z
      deposit_end_time: 2021-07-19T15:39:48.619640056Z
      total_deposit:
      - denom: utros
        amount: "100010000"
      voting_start_time: 2021-07-19T15:38:23.789218262Z
      voting_end_time: 2021-07-19T15:40:03.789218262Z

    ...

    - content:
        title: Param-Change Staking MaxValidators to 100
        description: This is a test to update MaxValidators to 100 in staking Module
        changes:
        - subspace: staking
          key: MaxValidators
          value: "100"
      id: 7
      proposal_status: 3
      final_tally_result:
        "yes": "400000000"
        abstain: "0"
        "no": "0"
        no_with_veto: "0"
      submit_time: 2021-07-23T14:40:04.976927421Z
      deposit_end_time: 2021-07-23T14:41:44.976927421Z
      total_deposit:
      - denom: utros
        amount: "100010000"
      voting_start_time: 2021-07-23T14:40:41.961523583Z
      voting_end_time: 2021-07-23T14:42:21.961523583Z
```
</details>
<br>

#### [`vote`](#vote)
Query details for a single vote on a proposal given its identifier.

```shell
Usage:
  stchaind query gov vote [proposal-id] [voter-addr] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for vote
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query gov vote 7 st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
    proposal_id: 7
    voter: st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
    option: 1
```
</details>
<br>

#### [`votes`](#votes)
Query vote details for a single proposal by its identifier.
```shell
Usage:
  stchaind query gov votes [proposal-id] [flags]

Flags:
      --count-total       count total number of records in votes to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for votes
      --limit uint        pagination limit of votes to query for (default 100)
      --offset uint       pagination offset of votes to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of votes to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of votes to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query gov votes 7
    - proposal_id: 7
      voter: st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
      option: 1
    - proposal_id: 7
      voter: st1m4f4hnyfhpaeqlcgv7lfhgzjwmrvfeggwnpygz
      option: 1
    - proposal_id: 7
      voter: st1kuhyf59qvukk8r5manky062d6c66utvytm7az6
      option: 1
    - proposal_id: 7
      voter: st1gtw399h9vfnekqsz3dg4n6mj0qgdpnh3c2n66k
      option: 1
```
</details>
<br>

#### [`deposit`](#deposit)
Query details for a single proposal deposit on a proposal by its identifier.
```shell
Usage:
  stchaind query gov deposit [proposal-id] [depositer-addr] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for deposit
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
root@00af23eab845:/stchaind# ./stchaind query gov deposit 7 st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
proposal_id: 7
depositor: st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
amount:
- denom: utros
  amount: "100000000"
```
</details>
<br>

#### [`deposits`](#deposits)
Query details for all deposits on a proposal.
```shell
Usage:
  stchaind query gov deposits [proposal-id] [flags]

Flags:
      --count-total       count total number of records in deposits to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for deposits
      --limit uint        pagination limit of deposits to query for (default 100)
      --offset uint       pagination offset of deposits to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of deposits to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of deposits to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><b>Example</b></summary>

```shell
root@00af23eab845:/stchaind# ./stchaind query gov deposits 7
    - proposal_id: 7
      depositor: st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
      amount:
      - denom: utros
        amount: "100000000"
```
</details>
<br>

***


## Slashing Module

### Transaction

#### -[`unjail`](#-unjail)
Unjail a jailed validator.
```shell
Usage:
  stchaind tx slashing unjail [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for unjail
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation
  
  In testing phase, --keyring-backend="test"
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx slashing unjail --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>
<br>

### Queries

#### [`signing-info`](#signing-info)
Use a validators' consensus public key to find the signing-info for that validator.
```shell
Usage:
  stchaind query slashing signing-info [validator-conspub] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for signing-info
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query slashing signing-info stvalconspub1zcjduepqsnwlx7rv0ghyvh9tm99zle39df99jt8hccwt8jdrvjs26zqrzh9shdmgyc 
    address: stvalcons1sa58sznp26ftquypx994q2eurq6qy38tfm3rn3
    start_height: 0
    index_offset: 3874
    jailed_until: 1970-01-01T00:00:00Z
    tombstoned: false
    missed_blocks_counter: 0
```
</details>
<br>

#### [`slashing-params`](#slashing-params)
Query genesis parameters for the slashing module.
```shell
Usage:
  stchaind query slashing params [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for params
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query slashing params 
signed_blocks_window: 10000
min_signed_per_window: "0.500000000000000000"
downtime_jail_duration: 10m0s
slash_fraction_double_sign: "0.050000000000000000"
slash_fraction_downtime: "0.010000000000000000"
```
</details>
<br>

***


## Staking Module

### Transactions

#### -[`delegate`](#-delegate)
Delegate an amount of liquid coins to a validator from your wallet.
```shell
Usage:
  stchaind tx staking delegate [validator-addr] [amount] [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for delegate
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation
  
  In testing phase, --keyring-backend="test"
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx staking delegate stvaloper1fmdh9vf262qxe5ehmp9jvgkqzaeye4qmxjrr3k 1000gwei --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>
<br>

#### -[`redelegate`](#-redelegate)
Redelegate an amount of illiquid staking tokens from one validator to another.
```shell
Usage:
  stchaind tx staking redelegate [src-validator-addr] [dst-validator-addr] [amount] [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for redelegate
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation
  
  In testing phase, --keyring-backend="test"
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx staking redelegate stvaloper1fmdh9vf262qxe5ehmp9jvgkqzaeye4qmxjrr3k stvaloper1gtw399h9vfnekqsz3dg4n6mj0qgdpnh30x66xa 1000gwei --from=st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>
<br>


#### -[`unbond`](#-unbond)
Unbond an amount of bonded shares from a validator.
```shell
Usage:
  stchaind tx staking unbond [validator-addr] [amount] [flags]

Flags:
  -a, --account-number uint   The account number of the signing account (offline mode only)
      --dry-run               ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string    Fee account pays fees for the transaction instead of deducting from the signer
      --gas string            gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only         Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                  help for unbond
      --keyring-dir string    The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                Use a connected Ledger device
      --note string           Note to add a description to the transaction (previously --memo)
      --offline               Offline mode (does not allow any online functionality
  -o, --output string         Output format (text|json) (default "json")
  -s, --sequence uint         The sequence number of the signing account (offline mode only)
      --sign-mode string      Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint   Set a block timeout height to prevent the tx from being committed past a certain height
  -y, --yes                   Skip tx broadcasting prompt confirmation
  
  In testing phase, --keyring-backend="test"
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx staking unbond stvaloper12adksjsd7gcsn23h5jmvdygzx2lfw5q4pyf57u 10000gwei --from=st12adksjsd7gcsn23h5jmvdygzx2lfw5q4kgq5zh --chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei
```
</details>
<br>

#### -[`create-validator`](#-create-validator)
Create new validator initialized with a self-delegation to it.

```shell
Usage:
  stchaind tx staking create-validator [flags]

Flags:
  -a, --account-number uint                 The account number of the signing account (offline mode only)
      --amount string                       Amount of coins to bond
      --commission-max-change-rate string   The maximum commission change rate percentage (per day)
      --commission-max-rate string          The maximum commission rate percentage
      --commission-rate string              The initial commission rate percentage
      --details string                      The validator's (optional) details
      --dry-run                             ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string                  Fee account pays fees for the transaction instead of deducting from the signer
      --gas string                          gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only                       Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                                help for create-validator
      --identity string                     The optional identity signature (ex. UPort or Keybase)
      --ip string                           The node's public IP. It takes effect only when used in combination with --generate-only
      --keyring-dir string                  The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                              Use a connected Ledger device
      --min-self-delegation string          The minimum self delegation required on the validator
      --moniker string                      The validator's name
      --node-id string                      The node's ID
      --note string                         Note to add a description to the transaction (previously --memo)
      --offline                             Offline mode (does not allow any online functionality
  -o, --output string                       Output format (text|json) (default "json")
      --pubkey string                       The validator's Protobuf JSON encoded public key
      --security-contact string             The validator's (optional) security contact email
  -s, --sequence uint                       The sequence number of the signing account (offline mode only)
      --sign-mode string                    Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint                 Set a block timeout height to prevent the tx from being committed past a certain height
      --website string                      The validator's (optional) website
  -y, --yes                                 Skip tx broadcasting prompt confirmation
  
  In testing phase, --keyring-backend="test"
```
> * `moniker`: the validator's name
> * `pubkey`: the private key associated with this Tendermint PubKey is used to sign prevotes and precommits
> * `website`: website(Optional)
> * `description`: description(Optional)
> * `commission-rate`: the commission rate on block rewards and fees charged to delegators
> * `commission-max-rate`: the maximum commission rate which this validator can charge. This parameter cannot be changed after create-validator is processed.
> * `commission-max-change-rate`: the maximum daily increase of the validator commission. This parameter cannot be changed after create-validator is processed.
> * `min-self-delegation`: minimum amount of tokens the validator needs to have bonded at all time. If the validator's self-delegated stake falls below this limit, their entire staking pool will unbond. "1" = 1000000gwei.
> * `amount`: the amount of tokens to be bonded to the validator at creation. This value should be greater than the value of `min-self-delegation`
<details>
  <summary><b>Example</b></summary>

```shell
./stchaind tx staking create-validator \
--amount=100stos \
--pubkey='{"@type":"/cosmos.crypto.ed25519.PubKey","key":"JwtmYzaX0b+zjuDypUI2+qy8wa/LFtUUUg0+vr11tpg="}' \
--moniker="myValidator" \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=user0 \
--chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei -y 
```

The value of `--pubkey` can be retrieved by using the command `./stchaind tendermint show-validator`

</details>
<br>


#### -[`edit-validator`](#-edit-validator)
Edit an existing validator account.
```shell
Usage:
  stchaind tx staking edit-validator [flags]

Flags:
  -a, --account-number uint          The account number of the signing account (offline mode only)
      --commission-rate string       The new commission rate percentage
      --details string               The validator's (optional) details (default "[do-not-modify]")
      --dry-run                      ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fee-account string           Fee account pays fees for the transaction instead of deducting from the signer
      --gas string                   gas limit to set per-transaction; set to "auto" to calculate sufficient gas automatically (default 200000)
      --generate-only                Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible)
  -h, --help                         help for edit-validator
      --identity string              The (optional) identity signature (ex. UPort or Keybase) (default "[do-not-modify]")
      --keyring-dir string           The client Keyring directory; if omitted, the default 'home' directory will be used
      --ledger                       Use a connected Ledger device
      --min-self-delegation string   The minimum self delegation required on the validator
      --moniker string               The validator's name (default "[do-not-modify]")
      --note string                  Note to add a description to the transaction (previously --memo)
      --offline                      Offline mode (does not allow any online functionality
  -o, --output string                Output format (text|json) (default "json")
      --security-contact string      The validator's (optional) security contact email (default "[do-not-modify]")
  -s, --sequence uint                The sequence number of the signing account (offline mode only)
      --sign-mode string             Choose sign mode (direct|amino-json), this is an advanced feature
      --timeout-height uint          Set a block timeout height to prevent the tx from being committed past a certain height
      --website string               The validator's (optional) website (default "[do-not-modify]")
  -y, --yes                          Skip tx broadcasting prompt confirmation

  In testing phase, --keyring-backend="test"
```
> WARNING
> * `min_self_delegation` allows to increase only
> * `commission-max-rate` cannot be changed after create-validator
> * `commission-max-change-rate `cannot be changed after create-validator

<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind tx staking edit-validator \
--from=user0 \
--keyring-backend=test \
--min-self-delegation=100  \
--memo="Change 'min-self-delegation' from 1 to 100" \
--chain-id=tropos-5  --keyring-backend=test --gas=auto --gas-prices=1000000000wei -y
```
</details>
<br>


### Queries

#### [`delegation`](#delegation)
Query a delegation based on delegator address and validator address.
```shell
Usage:
  stchaind query staking delegation [delegator-addr] [validator-addr] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for delegation
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking delegation st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda stvaloper1fmdh9vf262qxe5ehmp9jvgkqzaeye4qmxjrr3k
    delegation:
      delegator_address: st1fmdh9vf262qxe5ehmp9jvgkqzaeye4qm372rda
      validator_address: stvaloper1fmdh9vf262qxe5ehmp9jvgkqzaeye4qmxjrr3k
      shares: "100000000000000000000.000000000000000000"
    balance:
      denom: wei
      amount: "1000000000000000000000"
```
</details>
<br>

#### [`delegations`](#delegations)
Query delegations for an individual delegator on all validators.
```shell
Usage:
  stchaind query staking delegations [delegator-addr] [flags]

Flags:
      --count-total       count total number of records in delegations to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for delegations
      --limit uint        pagination limit of delegations to query for (default 100)
      --offset uint       pagination offset of delegations to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of delegations to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of delegations to query for
      --reverse           results are sorted in descending order

```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking delegations st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
delegation_responses:
- balance:
    amount: "500000000000"
    denom: wei
  delegation:
    delegator_address: st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
    shares: "500000000000.000000000000000000"
    validator_address: stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
pagination:
  next_key: null
  total: "0"
```
</details>
<br>

#### [`delegations-to`](#delegations-to)
Query all delegations made to one validator.
```shell
Usage:
  stchaind query staking delegations-to [validator-addr] [flags]

Flags:
      --count-total       count total number of records in validator delegations to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for delegations-to
      --limit uint        pagination limit of validator delegations to query for (default 100)
      --offset uint       pagination offset of validator delegations to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of validator delegations to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of validator delegations to query for
      --reverse           results are sorted in descending ord
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking delegations-to stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
delegation_responses:
- balance:
    amount: "500000000000"
    denom: wei
  delegation:
    delegator_address: st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
    shares: "500000000000.000000000000000000"
    validator_address: stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
pagination:
  next_key: null
  total: "0"
```
</details>
<br>


#### [`unbonding-delegations`](#unbonding-delegations)
Query unbonding delegations for an individual delegator.
```shell
Usage:
  stchaind query staking unbonding-delegations [delegator-addr] [flags]

Flags:
      --count-total       count total number of records in unbonding delegations to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for unbonding-delegations
      --limit uint        pagination limit of unbonding delegations to query for (default 100)
      --offset uint       pagination offset of unbonding delegations to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of unbonding delegations to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of unbonding delegations to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind q staking unbonding-delegations st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2
- delegator_address: st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2
  validator_address: stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p
  entries:
  - creation_height: 5805
    completion_time: 2021-08-30T19:53:31.144199109Z
    initial_balance: "10000"
    balance: "10000"
```
</details>
<br>

#### [`unbonding-delegation`](#unbonding-delegation)

Query unbonding delegations for an individual delegator on an individual validator.

```shell
Usage:
  stchaind query staking unbonding-delegation [delegator-addr] [validator-addr] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for unbonding-delegation
  -o, --output string   Output format (text|json) (default "text")
```

<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind q staking unbonding-delegation st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2 stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p
```
</details>
<br>

#### [`unbonding-delegations-from`](#unbonding-delegations-from)
Query all unbonding delegatations from a validator.
```shell
Usage:
  stchaind query staking unbonding-delegations-from [validator-addr] [flags]

Flags:
      --count-total       count total number of records in unbonding delegations to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for unbonding-delegations-from
      --limit uint        pagination limit of unbonding delegations to query for (default 100)
      --offset uint       pagination offset of unbonding delegations to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of unbonding delegations to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of unbonding delegations to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking unbonding-delegations-from stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p
- delegator_address: st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2
  validator_address: stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p
  entries:
  - creation_height: 5805
    completion_time: 2021-08-30T19:53:31.144199109Z
    initial_balance: "10000"
    balance: "10000"
```
</details>
<br>

#### [`redelegation`](#redelegation)
Query a redelegation record based on delegator and a source and destination validator address.
```shell
Usage:
  stchaind query staking redelegation [delegator-addr] [src-validator-addr] [dst-validator-addr] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for redelegation
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking redelegation st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76 stvaloper1gamc7ajhzukp08nle9z9asyfx4u4dlz53dquzj stvaloper1zgqhnz69jppcwg9z27vtq3zq9r3du5v6vjqvpq
- redelegation:
    delegator_address: st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76
    validator_src_address: stvaloper1gamc7ajhzukp08nle9z9asyfx4u4dlz53dquzj
    validator_dst_address: stvaloper1zgqhnz69jppcwg9z27vtq3zq9r3du5v6vjqvpq
    entries: []
  entries:
  - redelegationentry:
      creation_height: 1909
      completion_time: 2021-09-02T19:33:26.890343914Z
      initial_balance: "10000"
      shares_dst: "10000.000000000000000000"
    balance: "10000"
```
</details>
<br>


#### [`redelegations`](#redelegations)
Query all redelegations records for one delegator.
```shell
Usage:
  stchaind query staking redelegations [delegator-addr] [flags]

Flags:
      --count-total       count total number of records in delegator redelegations to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for redelegations
      --limit uint        pagination limit of delegator redelegations to query for (default 100)
      --offset uint       pagination offset of delegator redelegations to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of delegator redelegations to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of delegator redelegations to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><b>Example</b></summary>


```shell
$ ./stchaind query staking redelegations st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76
- redelegation:
    delegator_address: st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76
    validator_src_address: stvaloper1gamc7ajhzukp08nle9z9asyfx4u4dlz53dquzj
    validator_dst_address: stvaloper1zgqhnz69jppcwg9z27vtq3zq9r3du5v6vjqvpq
    entries: []
  entries:
  - redelegationentry:
      creation_height: 1909
      completion_time: 2021-09-02T19:33:26.890343914Z
      initial_balance: "10000"
      shares_dst: "10000.000000000000000000"
    balance: "10000"
```
</details>
<br>

#### [`redelegations-from`](#redelegations-from)
Query all unbonding delegatations from a validator.
```shell
Usage:
  stchaind query staking unbonding-delegations-from [validator-addr] [flags]

Flags:
      --count-total       count total number of records in unbonding delegations to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for unbonding-delegations-from
      --limit uint        pagination limit of unbonding delegations to query for (default 100)
      --offset uint       pagination offset of unbonding delegations to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of unbonding delegations to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of unbonding delegations to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking unbonding-delegations-from stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p
- delegator_address: st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2
  validator_address: stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p
  entries:
  - creation_height: 5805
    completion_time: 2021-08-30T19:53:31.144199109Z
    initial_balance: "10000"
    balance: "10000"
```
</details>
<br>

#### [`historical-info`](#historical-info)
Query historical info at given height.
```shell
Usage:
  stchaind query staking historical-info [height] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for historical-info
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
- Note:
- The response of `historical-info` is depended on the `skating` parameter `HistoricalEntries`. If `HistoricalEntries` is "0", the response will always be

ERROR: no historical info found
```

```shell
$ ./stchaind query staking historical-info 300
header:
  app_hash: fun5OdjHvsMZU1g+mcpgnfDuVBDSTTQjrTjJ3jvEkpo=
  chain_id: test-chain
  consensus_hash: BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8=
  data_hash: 47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=
  evidence_hash: 47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=
  height: "300"
  last_block_id:
    hash: heoL6s+ZfzE4xdhvUuKe5OKppwYIklXVvV+hDQe17G0=
    part_set_header:
      hash: wHoreN7ckwhF3a4dTDRKi47wvrIq0gme2AgNXBf/E3U=
      total: 1
  last_commit_hash: sk5idFtJj7qZFHyVbQ/PsB/TQfovdKn2SEekPWF7ZJc=
  last_results_hash: 47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=
  next_validators_hash: UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=
  proposer_address: GKcWnBtCfZlBM/ez1FBOknids3w=
  time: "2023-01-11T00:51:55.887814534Z"
  validators_hash: UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=
  version:
    app: "0"
    block: "11"
valset:
- commission:
    commission_rates:
      max_change_rate: "0.010000000000000000"
      max_rate: "0.200000000000000000"
      rate: "0.100000000000000000"
    update_time: "2023-01-09T17:08:58.489050300Z"
  consensus_pubkey:
    '@type': /cosmos.crypto.ed25519.PubKey
    key: 69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo=
  delegator_shares: "500000000000.000000000000000000"
  description:
    details: ""
    identity: ""
    moniker: node
    security_contact: ""
    website: ""
  jailed: false
  min_self_delegation: "1"
  operator_address: stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
  status: BOND_STATUS_BONDED
  tokens: "500000000000"
  unbonding_height: "0"
  unbonding_time: "1970-01-01T00:00:00Z"

```
</details>
<br>


#### [`pool`](#pool)
Query values for amounts stored in the staking pool.
```shell
Usage:
  stchaind query staking pool [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for pool
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking pool
    bonded_tokens: "500000000000"
    not_bonded_tokens: "0"
```
</details>
<br>

#### [`staking-params`](#staking-params)
Query values set as staking parameters.
```shell
Usage:
  stchaind query staking params [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for params
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking params
    bond_denom: wei
    historical_entries: 10000
    max_entries: 7
    max_validators: 100
    unbonding_time: 1814400s
```
</details>
<br>


#### [`validator`](#validator)
Query details about an individual validator
```shell
Usage:
  stchaind query staking validator [validator-addr] [flags]

Flags:
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for validator
  -o, --output string   Output format (text|json) (default "text")
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking validator stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
- |
commission:
  commission_rates:
    max_change_rate: "0.010000000000000000"
    max_rate: "0.200000000000000000"
    rate: "0.100000000000000000"
  update_time: "2023-01-09T17:08:58.489050300Z"
consensus_pubkey:
  '@type': /cosmos.crypto.ed25519.PubKey
  key: 69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo=
delegator_shares: "500000000000.000000000000000000"
description:
  details: ""
  identity: ""
  moniker: node
  security_contact: ""
  website: ""
jailed: false
min_self_delegation: "1"
operator_address: stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
status: BOND_STATUS_BONDED
tokens: "500000000000"
unbonding_height: "0"
unbonding_time: "1970-01-01T00:00:00Z"
```
</details>
<br>

#### [`validators`](#validators)
Query values for amounts stored in the staking pool.
```shell
Usage:
  stchaind query staking validators [flags]

Flags:
      --count-total       count total number of records in validators to query for
      --height int        Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help              help for validators
      --limit uint        pagination limit of validators to query for (default 100)
      --offset uint       pagination offset of validators to query for
  -o, --output string     Output format (text|json) (default "text")
      --page uint         pagination page of validators to query for. This sets offset to a multiple of limit (default 1)
      --page-key string   pagination page-key of validators to query for
      --reverse           results are sorted in descending order
```
<details>
  <summary><b>Example</b></summary>

```shell
$ ./stchaind query staking validators
    - |
    pagination:
      next_key: null
      total: "0"
    validators:
    - commission:
        commission_rates:
          max_change_rate: "0.010000000000000000"
          max_rate: "0.200000000000000000"
          rate: "0.100000000000000000"
        update_time: "2023-01-09T17:08:58.489050300Z"
      consensus_pubkey:
        '@type': /cosmos.crypto.ed25519.PubKey
        key: 69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo=
      delegator_shares: "500000000000.000000000000000000"
      description:
        details: ""
        identity: ""
        moniker: node
        security_contact: ""
        website: ""
      jailed: false
      min_self_delegation: "1"
      operator_address: stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
      status: BOND_STATUS_BONDED
      tokens: "500000000000"
      unbonding_height: "0"
      unbonding_time: "1970-01-01T00:00:00Z"

```
</details>
<br>
