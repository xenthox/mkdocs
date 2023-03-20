---
title: Stratos Chain stchaind REST APIs
description: A REST interface to communicate with Stratos-chain for state queries and transaction operations.
---

## Overview
Generally, all the APIs provided here could be grouped into HTTP GET and POST requests. We classified these APIs into sections based on their modules or their operations for an in-depth analysis.

- `GET` Request

The response content type is `application/json`

- `POST` Request

The response content type is `application/json`. If it has a request body, the request content is also in `application/json` format.

A `POST` request  will return an **unsigned** transaction, which equals to its equivalent `stchaind` command with a `--generate-only` flag.

<!--
<details>
    <summary>Comparison between REST API and its equivalent <code>stchaind</code> command</summary>

Suppose a `send` transaction that transfers tokens from one account to another.
The following comparison demonstrates we can get the same response in both methods.

## REST API
Http `POST` request
```http
https://rest-tropos.thestratos.org/bank/accounts/st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6/transfers
```
Request body
```json
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Send Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
  "amount": [
    {
      "denom": "ustos",
      "amount": "1000000"
    }
  ]
}
```
Response
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgSend",
                "value": {
                    "from_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                    "to_address": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6",
                    "amount": [
                        {
                            "denom": "ustos",
                            "amount": "1000000"
                        }
                    ]
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Send Tx Example"
    }
}
```

## `stchaind` command.
```shell
$ ./build/stchaind tx send st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2 st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6 1000000ustos --fees 100ustos --chain-id=test-chain --keyring-backend=test --memo "Send Tx Example" --generate-only --gas=auto
```
Output
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgSend",
                "value": {
                    "from_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                    "to_address": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6",
                    "amount": [
                        {
                            "denom": "ustos",
                            "amount": "1000000"
                        }
                    ]
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Send Tx Example"
    }
}
```
</details>
<br>

-->

***
## Stratos-chain REST APIs

### Node Status

<details>
    <summary><code>GET /node_info</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;queries information about the connected node</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/node_info
```
Response Example:
```json
{
    "node_info": {
        "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
        },
        "id": "16a0758d175cbf5c08d41dffa73eb5c0190869ed",
        "listen_addr": "tcp://0.0.0.0:26656",
        "network": "test-chain",
        "version": "0.34.21",
        "channels": "40202122233038606100",
        "moniker": "node",
        "other": {
            "tx_index": "on",
            "rpc_address": "tcp://127.0.0.1:26657"
        }
    },
    "application_version": {
        "name": "",
        "server_name": "stchaind",
        "version": "v0.8.0",
        "commit": "",
        "build_tags": "",
        "go": "go version go1.19.4 linux/amd64",
        "build_deps": [
            "filippo.io/edwards25519@v1.0.0-beta.2",
            "github.com/99designs/keyring@v1.1.6 => github.com/cosmos/keyring@v1.1.7-0.20210622111912-ef00f8ac3d76",
            "github.com/ChainSafe/go-schnorrkel@v0.0.0-20200405005733-88cbf1b4c40d",
            "github.com/VictoriaMetrics/fastcache@v1.6.0",
            "github.com/Workiva/go-datastructures@v1.0.53",
            "github.com/armon/go-metrics@v0.3.10",
            "github.com/beorn7/perks@v1.0.1",
            "github.com/bgentry/speakeasy@v0.1.0",
            "github.com/btcsuite/btcd@v0.22.1",
            "github.com/btcsuite/btcd/chaincfg/chainhash@v1.0.1",
            "github.com/btcsuite/btcutil@v1.0.3-0.20201208143702-a53e38424cce",
            "github.com/cenkalti/backoff/v4@v4.1.1",
            "github.com/cespare/xxhash/v2@v2.1.2",
            "github.com/coinbase/rosetta-sdk-go@v0.7.0",
            "github.com/confio/ics23/go@v0.7.0 => github.com/cosmos/cosmos-sdk/ics23/go@v0.8.0",
            "github.com/cosmos/btcutil@v1.0.4",
            "github.com/cosmos/cosmos-sdk@v0.45.9",
            "github.com/cosmos/go-bip39@v1.0.0",
            "github.com/cosmos/iavl@v0.19.3",
            "github.com/cosmos/ibc-go/v3@v3.0.0",
            "github.com/creachadair/taskgroup@v0.3.2",
            "github.com/davecgh/go-spew@v1.1.1",
            "github.com/deckarep/golang-set@v1.8.0",
            "github.com/desertbit/timer@v0.0.0-20180107155436-c41aec40b27f",
            "github.com/dvsekhvalnov/jose2go@v0.0.0-20200901110807-248326c1351b",
            "github.com/edsrzf/mmap-go@v1.0.0",
            "github.com/ethereum/go-ethereum@v1.10.16",
            "github.com/felixge/httpsnoop@v1.0.1",
            "github.com/fsnotify/fsnotify@v1.5.4",
            "github.com/gballet/go-libpcsclite@v0.0.0-20190607065134-2772fd86a8ff",
            "github.com/go-kit/kit@v0.12.0",
            "github.com/go-kit/log@v0.2.1",
            "github.com/go-logfmt/logfmt@v0.5.1",
            "github.com/go-stack/stack@v1.8.0",
            "github.com/godbus/dbus@v0.0.0-20190726142602-4481cbc300e2",
            "github.com/gogo/gateway@v1.1.0",
            "github.com/gogo/protobuf@v1.3.3 => github.com/regen-network/protobuf@v1.3.3-alpha.regen.1",
            "github.com/golang/protobuf@v1.5.2",
            "github.com/golang/snappy@v0.0.4",
            "github.com/google/btree@v1.0.0",
            "github.com/google/orderedcode@v0.0.1",
            "github.com/google/uuid@v1.3.0",
            "github.com/gorilla/handlers@v1.5.1",
            "github.com/gorilla/mux@v1.8.0",
            "github.com/gorilla/websocket@v1.5.0",
            "github.com/grpc-ecosystem/go-grpc-middleware@v1.3.0",
            "github.com/grpc-ecosystem/grpc-gateway@v1.16.0",
            "github.com/gsterjov/go-libsecret@v0.0.0-20161001094733-a6f4afe4910c",
            "github.com/gtank/merlin@v0.1.1",
            "github.com/gtank/ristretto255@v0.1.2",
            "github.com/hashicorp/go-immutable-radix@v1.3.1",
            "github.com/hashicorp/golang-lru@v0.5.5-0.20210104140557-80c98217689d",
            "github.com/hashicorp/hcl@v1.0.0",
            "github.com/hdevalence/ed25519consensus@v0.0.0-20210204194344-59a8610d2b87",
            "github.com/holiman/bloomfilter/v2@v2.0.3",
            "github.com/holiman/uint256@v1.2.0",
            "github.com/huin/goupnp@v1.0.2",
            "github.com/improbable-eng/grpc-web@v0.15.0",
            "github.com/ipfs/go-cid@v0.1.0",
            "github.com/jackpal/go-nat-pmp@v1.0.2",
            "github.com/klauspost/compress@v1.15.9",
            "github.com/klauspost/cpuid/v2@v2.0.4",
            "github.com/lib/pq@v1.10.6",
            "github.com/libp2p/go-buffer-pool@v0.1.0",
            "github.com/magiconair/properties@v1.8.6",
            "github.com/mattn/go-colorable@v0.1.12",
            "github.com/mattn/go-isatty@v0.0.14",
            "github.com/mattn/go-runewidth@v0.0.9",
            "github.com/matttproud/golang_protobuf_extensions@v1.0.2-0.20181231171920-c182affec369",
            "github.com/mimoo/StrobeGo@v0.0.0-20181016162300-f8f6d4d2b643",
            "github.com/minio/blake2b-simd@v0.0.0-20160723061019-3f5f724cb5b1",
            "github.com/minio/highwayhash@v1.0.2",
            "github.com/minio/sha256-simd@v1.0.0",
            "github.com/mitchellh/mapstructure@v1.5.0",
            "github.com/mr-tron/base58@v1.2.0",
            "github.com/mtibben/percent@v0.2.1",
            "github.com/multiformats/go-base32@v0.0.3",
            "github.com/multiformats/go-base36@v0.1.0",
            "github.com/multiformats/go-multibase@v0.0.3",
            "github.com/multiformats/go-multihash@v0.0.15",
            "github.com/multiformats/go-varint@v0.0.6",
            "github.com/olekukonko/tablewriter@v0.0.5",
            "github.com/pelletier/go-toml/v2@v2.0.2",
            "github.com/pkg/errors@v0.9.1",
            "github.com/pmezard/go-difflib@v1.0.0",
            "github.com/prometheus/client_golang@v1.12.2",
            "github.com/prometheus/client_model@v0.2.0",
            "github.com/prometheus/common@v0.34.0",
            "github.com/prometheus/procfs@v0.7.3",
            "github.com/prometheus/tsdb@v0.7.1",
            "github.com/rakyll/statik@v0.1.7",
            "github.com/rcrowley/go-metrics@v0.0.0-20200313005456-10cdbea86bc0",
            "github.com/regen-network/cosmos-proto@v0.3.1",
            "github.com/rjeczalik/notify@v0.9.1",
            "github.com/rs/cors@v1.8.2",
            "github.com/rs/zerolog@v1.27.0",
            "github.com/shirou/gopsutil@v3.21.4-0.20210419000835-c7a38de76ee5+incompatible",
            "github.com/spf13/afero@v1.8.2",
            "github.com/spf13/cast@v1.5.0",
            "github.com/spf13/cobra@v1.5.0",
            "github.com/spf13/jwalterweatherman@v1.1.0",
            "github.com/spf13/pflag@v1.0.5",
            "github.com/spf13/viper@v1.12.0",
            "github.com/status-im/keycard-go@v0.0.0-20200402102358-957c09536969",
            "github.com/stretchr/testify@v1.8.0",
            "github.com/subosito/gotenv@v1.4.0",
            "github.com/syndtr/goleveldb@v1.0.1-0.20210819022825-2ae1ddf74ef7",
            "github.com/tendermint/btcd@v0.1.1",
            "github.com/tendermint/crypto@v0.0.0-20191022145703-50d29ede1e15",
            "github.com/tendermint/go-amino@v0.16.0",
            "github.com/tendermint/tendermint@v0.34.21",
            "github.com/tendermint/tm-db@v0.6.7",
            "github.com/tklauser/go-sysconf@v0.3.5",
            "github.com/tklauser/numcpus@v0.2.2",
            "github.com/tyler-smith/go-bip39@v1.1.0",
            "golang.org/x/crypto@v0.0.0-20220525230936-793ad666bf5e",
            "golang.org/x/exp@v0.0.0-20220722155223-a9213eeb770e",
            "golang.org/x/net@v0.0.0-20220726230323-06994584191e",
            "golang.org/x/sync@v0.0.0-20220722155255-886fb9371eb4",
            "golang.org/x/sys@v0.0.0-20220727055044-e65921a090b8",
            "golang.org/x/term@v0.0.0-20220722155259-a9ba230a4035",
            "golang.org/x/text@v0.3.7",
            "google.golang.org/genproto@v0.0.0-20220725144611-272f38e5d71b",
            "google.golang.org/grpc@v1.48.0 => google.golang.org/grpc@v1.33.2",
            "google.golang.org/protobuf@v1.28.0",
            "gopkg.in/ini.v1@v1.66.6",
            "gopkg.in/yaml.v2@v2.4.0",
            "gopkg.in/yaml.v3@v3.0.1",
            "nhooyr.io/websocket@v1.8.6"
        ],
        "cosmos_sdk_version": "v0.45.9"
    }
}
```
</details>
<br>

***

### Tendermint RPC
Tendermint APIs, such as query blocks, transactions and validator set

<details>
    <summary><code>GET /syncing</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries if the node is currently syning with other nodes</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/syncing
```
Response Example:
```json
{
    "syncing": true
}
```
</details>
<br>

<details>
    <summary><code>GET /blocks/{height | latest}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries a block at a specific {height | latest}</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/blocks/latest
```
Response Example:
```json
{
    "block_id": {
        "hash": "97E6AF89C839417D1FC1B10DEFC747C14BF0F20F42ECF8116EE1EA55758556D1",
        "parts": {
            "total": 1,
            "hash": "4A32889C023AF838666A90D69A28238C4A62BBA692B85F8AB754A19BB0DAEC3D"
        }
    },
    "block": {
        "header": {
            "version": {
                "block": "11"
            },
            "chain_id": "test-chain",
            "height": "1101",
            "time": "2023-01-11T02:01:16.294185637Z",
            "last_block_id": {
                "hash": "EF2D1E436E3DC8D60CD3F52839DBF7347FB6C37863DFF0FB8F86496F594268BF",
                "parts": {
                    "total": 1,
                    "hash": "1A05B4D2C3813E05B7742AF1B2AB6024F2E68F174698DEC8B4A570D3F5DA068B"
                }
            },
            "last_commit_hash": "395C284AEB59B7B1C9EDE5D22E39281A5EFAE2864A896EB42639F6CABF49F8B7",
            "data_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
            "validators_hash": "5234BD91A3A751E055C35876578DE4A466311A80D540B59885AF68EF6D4D56DE",
            "next_validators_hash": "5234BD91A3A751E055C35876578DE4A466311A80D540B59885AF68EF6D4D56DE",
            "consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            "app_hash": "FE115D4597A8CD9F7C508D43156778FF88CF85D95BE508D28FA55DDE5C947784",
            "last_results_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
            "evidence_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
            "proposer_address": "18A7169C1B427D994133F7B3D4504E92789DB37C"
        },
        "data": {
            "txs": []
        },
        "evidence": {
            "evidence": []
        },
        "last_commit": {
            "height": "1100",
            "round": 0,
            "block_id": {
                "hash": "EF2D1E436E3DC8D60CD3F52839DBF7347FB6C37863DFF0FB8F86496F594268BF",
                "parts": {
                    "total": 1,
                    "hash": "1A05B4D2C3813E05B7742AF1B2AB6024F2E68F174698DEC8B4A570D3F5DA068B"
                }
            },
            "signatures": [
                {
                    "block_id_flag": 2,
                    "validator_address": "18A7169C1B427D994133F7B3D4504E92789DB37C",
                    "timestamp": "2023-01-11T02:01:16.294185637Z",
                    "signature": "pLQMUuJJ23T2u5oNpR5xIt4+T/73TXxe6bj/LF0ue+hg8UseqFEGTDybLeAukUUMcudPnE3BS5oQoRwB9245BA=="
                }
            ]
        }
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /validatorsets/{height | latest}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries validator set at certain {height | latest}</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/validatorsets/latest
```
Response Example:
```json
{
    "height": "0",
    "result": {
        "block_height": "1115",
        "validators": [
            {
                "address": "stvalcons1rzn3d8qmgf7ejsfn77eag5zwjfufmvmu7sn802",
                "pub_key": {
                    "type": "tendermint/PubKeyEd25519",
                    "value": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
                },
                "proposer_priority": "0",
                "voting_power": "500000"
            }
        ],
        "total": "1"
    }
}
```
</details>
<br>

***

### Auth

<details>
    <summary><code>GET /cosmos/auth/v1beta1/accounts</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the account information on blockchain</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/auth/v1beta1/accounts
```
Response Example:
```json
{
    "accounts": [
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
            "pub_key": {
                "@type": "/stratos.crypto.v1.ethsecp256k1.PubKey",
                "key": "Agkwb1xacHBqeqGBIqRacXgf0qKTnEBPCEtH2vTE01Ke"
            },
            "account_number": "0",
            "sequence": "4"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1zdv2enwn0eldmu9e2zwwz2zmgqswjzdv5w5g5j",
            "pub_key": null,
            "account_number": "6",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1r2gh2h8kjtz4slek6aua95ukyd8zmey2y9uatt",
            "pub_key": null,
            "account_number": "7",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1rwnmgk0x2n2wry876dkxq2hhcce8k7kzspppax",
            "pub_key": null,
            "account_number": "19",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st1y0ujk7aqvuxt6t058gn6k2kq8f3pe96vzwknau",
                "pub_key": null,
                "account_number": "29",
                "sequence": "0"
            },
            "name": "foundation_account",
            "permissions": [
                "minter",
                "burner"
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st1yl6hdjhmkf37639730gffanpzndzdpmhjjzmk7",
                "pub_key": null,
                "account_number": "27",
                "sequence": "0"
            },
            "name": "transfer",
            "permissions": [
                "minter",
                "burner"
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1g3wp24epahx2u8xn3jzh5ry8ugkh239l38wgrc",
            "pub_key": null,
            "account_number": "9",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1g54kyu2tk2e80wqq4q3t2dl4mm5ydwwyyu4pq5",
            "pub_key": null,
            "account_number": "8",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st1fz67scxv3hjy0nxafuf0c4made74gfcf7myjqg",
                "pub_key": null,
                "account_number": "28",
                "sequence": "0"
            },
            "name": "meta_node_bonded_pool",
            "permissions": [
                "minter"
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1f4d2tdddwv8304sfhe39r83d4ldz2dc3z40qtk",
            "pub_key": null,
            "account_number": "12",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st1fl48vsnmsdzcv85q5d2q4z5ajdha8yu3fkaac2",
                "pub_key": null,
                "account_number": "23",
                "sequence": "0"
            },
            "name": "bonded_tokens_pool",
            "permissions": [
                "burner",
                "staking"
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st1tygms3xhhs3yv487phx3dw4a95jn7t7lakpvw7",
                "pub_key": null,
                "account_number": "24",
                "sequence": "0"
            },
            "name": "not_bonded_tokens_pool",
            "permissions": [
                "burner",
                "staking"
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1vznztham7975ukx4hve497kxftejwatsjfw4gc",
            "pub_key": null,
            "account_number": "15",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1vvysda6ylqz2adauqg4djsz4rx6hv6mqv9fepp",
            "pub_key": null,
            "account_number": "3",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st10d07y265gmmuvt4z0w9aw880jnsr700jx08hhw",
                "pub_key": null,
                "account_number": "25",
                "sequence": "0"
            },
            "name": "gov",
            "permissions": [
                "burner"
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx",
            "pub_key": null,
            "account_number": "1",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st1jv65s3grqf6v6jl3dp4t6c9t9rk99cd8mjswgz",
                "pub_key": null,
                "account_number": "22",
                "sequence": "0"
            },
            "name": "distribution",
            "permissions": [
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1nvh4nrjh6hhy7pxr2d9zp4zrzphg8tdccmf47x",
            "pub_key": null,
            "account_number": "11",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st14rtmcurt5rvgy8pu3srleskfjjyry3n9a00nw9",
            "pub_key": null,
            "account_number": "5",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st144ykkar9fhl8khs7lwz0s7py9vj4w9adp37kt9",
            "pub_key": null,
            "account_number": "2",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1k9hfqps9s2tpnfxch2avvevyvtry0zth39gdzc",
            "pub_key": null,
            "account_number": "18",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1kwtqfxk654urrannhsl7shsm3r7q9tyn40yq4e",
            "pub_key": null,
            "account_number": "16",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1ewlfmhl8j0p2jesfd2xrqp0qjeh2222gs9uefh",
            "pub_key": null,
            "account_number": "20",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1mgatj2a65ksmhznf6lseweq835m7gyjvzmphhm",
            "pub_key": null,
            "account_number": "14",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st1m3h30wlvsf8llruxtpukdvsy0km2kum85un2xa",
                "pub_key": null,
                "account_number": "26",
                "sequence": "0"
            },
            "name": "mint",
            "permissions": [
                "minter"
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1uuxxf8kt5ey7rgwdgpch2l7ye2597cz5y4077n",
            "pub_key": null,
            "account_number": "13",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x",
            "pub_key": null,
            "account_number": "17",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st1afcwl869ehc6m4zsqp096dj9t9m75uh5wq4qrs",
            "pub_key": null,
            "account_number": "10",
            "sequence": "0"
        },
        {
            "@type": "/cosmos.auth.v1beta1.ModuleAccount",
            "base_account": {
                "address": "st17xpfvakm2amg962yls6f84z3kell8c5lv5hj2q",
                "pub_key": null,
                "account_number": "21",
                "sequence": "0"
            },
            "name": "fee_collector",
            "permissions": [
            ]
        },
        {
            "@type": "/cosmos.auth.v1beta1.BaseAccount",
            "address": "st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4",
            "pub_key": null,
            "account_number": "4",
            "sequence": "0"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "30"
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/auth/v1beta1/accounts/{address}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the account information on blockchain</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/auth/v1beta1/accounts/st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk
```
Response Example:
```json
{
    "account": {
        "@type": "/cosmos.auth.v1beta1.BaseAccount",
        "address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
        "pub_key": {
            "@type": "/stratos.crypto.v1.ethsecp256k1.PubKey",
            "key": "Agkwb1xacHBqeqGBIqRacXgf0qKTnEBPCEtH2vTE01Ke"
        },
        "account_number": "0",
        "sequence": "4"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/auth/v1beta1/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all parameters of Auth module.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/auth/v1beta1/params
```
Response Example:
```json
{
    "params": {
        "max_memo_characters": "256",
        "tx_sig_limit": "7",
        "tx_size_cost_per_byte": "10",
        "sig_verify_cost_ed25519": "590",
        "sig_verify_cost_secp256k1": "1000"
    }
}
```
</details>
<br>

<!--

<details>
    <summary><code>GET /auth/accounts/{address}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries account information on blockchain</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/auth/accounts/st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk
```
Response Example:
```json
{
    "height": "14",
    "result": {
        "type": "cosmos-sdk/BaseAccount",
        "value": {
            "address": "st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk",
            "public_key": {
                "type": "tendermint/PubKeySecp256k1",
                "value": "Aj/sbA5nIEpPunm5lG3ITuuPPGVPvYWVPXBR2Rme0jYj"
            },
            "sequence": "1"
        }
    }
}
```
</details>
<br>
-->

***

### Bank

<details>
    <summary><code>GET /cosmos/bank/v1beta1/balances/{address}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the balance of all coins for a single account</summary>

    Alias: /bank/balances/{address}

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/bank/v1beta1/balances/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
or 
https://rest-tropos.thestratos.org/bank/balances/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
```
Response Example:
```json
{
    "balances": [
        {
            "denom": "utros",
            "amount": "1000000000000000"
        },
        {
            "denom": "wei",
            "amount": "959989996222216000000000"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "2"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/bank/v1beta1/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the parameters of Bank module.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/bank/v1beta1/params
```
Response Example:
```json
{
    "params": {
        "send_enabled": [],
        "default_send_enabled": true
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/bank/v1beta1/supply</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; returns total supply of coins in the chain</summary>

    Alias: /bank/total

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/bank/v1beta1/supply
or
https://rest-tropos.thestratos.org/bank/total
```
Response Example:
```json
{
    "supply": [
        {
            "denom": "utros",
            "amount": "1000000000000000"
        },
        {
            "denom": "wei",
            "amount": "21000515645592831741552108"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "2"
    }
}
```
</details>
<br>



<details>
    <summary><code>GET /cosmos/bank/v1beta1/supply/{denom}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the supply of a single coin</summary>

    Alias: /bank/total/{denom}

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/bank/v1beta1/supply/wei
or
https://rest-tropos.thestratos.org/bank/total/wei
```
Response Example:
```json
{
    "amount": {
        "denom": "wei",
        "amount": "21000519539308644119443444"
    }
}
```
</details>
<br>


<!--
<details>
    <summary><code>POST /bank/accounts/{address}/transfers</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Send coins from one account to another</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/bank/accounts/st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6/transfers
```
Request Body
```json
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Send Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
  "amount": [
    {
      "denom": "ustos",
      "amount": "1000000"
    }
  ]
}
```
Response Example:
```json
{
  "type": "cosmos-sdk/StdTx",
  "value": {
    "msg": [
      {
        "type": "cosmos-sdk/MsgSend",
        "value": {
          "from_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
          "to_address": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6",
          "amount": [
            {
              "denom": "ustos",
              "amount": "1000000"
            }
          ]
        }
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "ustos",
          "amount": "100"
        }
      ],
      "gas": "200000"
    },
    "signatures": null,
    "memo": "Send Tx Example"
  }
}
```
</details>
<br>

-->

***

### Distribution

<details>
    <summary><code>GET /cosmos/distribution/v1beta1/community_pool</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the community pool coins</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/community_pool
```
Response Example:
```json
{
    "pool": [
        {
            "denom": "wei",
            "amount": "10529239257213782433.160000000000000000"
        }
    ]
}
```
</details>
<br>




<details>
    <summary><code>GET /cosmos/distribution/v1beta1/delegators/{delegator_address}/rewards</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the total rewards accrued by each validator.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/rewards
```
Response Example:
```json
{
    "rewards": [
        {
            "validator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
            "reward": [
                {
                    "denom": "wei",
                    "amount": "470444828785397799437.412000000000000000"
                }
            ]
        }
    ],
    "total": [
        {
            "denom": "wei",
            "amount": "470444828785397799437.412000000000000000"
        }
    ]
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/distribution/v1beta1/delegators/{delegator_address}/rewards/{validator_address}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the total rewards accrued by a delegation</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/rewards/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
```

Response Example:
```json
{
    "rewards": [
        {
            "denom": "wei",
            "amount": "513182961214751918939.940000000000000000"
        }
    ]
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/distribution/v1beta1/delegators/{delegator_address}/validators</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the validators of a delegator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/validators
```

Response Example:
```json
{
    "validators": [
        "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu"
    ]
}
```
</details>
<br>



<details>
    <summary><code>GET /cosmos/distribution/v1beta1/delegators/{delegator_address}/withdraw_address</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries withdraw address of a delegator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/withdraw_address
```

Response Example:
```json
{
    "height": "1430",
    "result": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh"
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/distribution/v1beta1/validators/{validator_address}/commission</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries accumulated commission for a validator.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/commission
```
Response Example:
```json
{
    "commission": {
        "commission": [
            {
                "denom": "wei",
                "amount": "61811505831634070383.438000000000000000"
            }
        ]
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /distribution/validators/{validatorAddr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries validator distribution information</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/distribution/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
```
Response Example:
```json
{
    "height": "1544",
    "result": {
        "operator_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
        "self_bond_rewards": [
            {
                "denom": "wei",
                "amount": "589121578147958674973.028000000000000000"
            }
        ],
        "val_commission": {
            "commission": [
                {
                    "denom": "wei",
                    "amount": "65457953127550963885.892000000000000000"
                }
            ]
        }
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/distribution/v1beta1/validators/{validator_address}/outstanding_rewards</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries outstanding rewards of a validator address</summary>

    Alias: /distribution/validators/{validatorAddr}/outstanding_rewards

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/outstanding_rewards
or
https://rest-tropos.thestratos.org/distribution/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/outstanding_rewards
```
Response Example:
```json
{
    "rewards": {
        "rewards": [
            {
                "denom": "wei",
                "amount": "664331752904189049948.100000000000000000"
            }
        ]
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /distribution/validators/{validator_address}/rewards</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the total rewards of a single validator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/distribution/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/rewards
```
Response Example:
```json
{"height":"1578","result":[
    {
        "denom": "wei",
        "amount": "602096285784320679462.390000000000000000"
    }
]}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/distribution/v1beta1/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries params of the distribution module</summary>

    Alias: /distribution/parameters

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/params
or 
https://rest-tropos.thestratos.org/distribution/parameters
```
Response Example:
```json
{
    "params": {
        "community_tax": "0.020000000000000000",
        "base_proposer_reward": "0.010000000000000000",
        "bonus_proposer_reward": "0.040000000000000000",
        "withdraw_addr_enabled": true
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/distribution/v1beta1/validators/{validator_address}/slashes</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries slash events of a validator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/distribution/v1beta1/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/slashes
```
Response Example:
```json
{
    "slashes": [
    ],
    "pagination": {
        "next_key": null,
        "total": "0"
    }
}
```
</details>
<br>

<!--
<details>
    <summary><code>POST /distribution/delegators/{delegatorAddr}/rewards</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Withdraw all the delegator's delegation rewards</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/distribution/delegators/st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2/rewards
```
Request Body
```json
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Withdraw Rewards Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "1000"
      }
    ],
    "simulate": false
  }
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgWithdrawDelegationReward",
                "value": {
                    "delegator_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p"
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "1000"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Withdraw Rewards Tx Example"
    }
}
```
</details>
<br>



<details>
    <summary><code>POST /distribution/delegators/{delegatorAddr}/rewards/{validatorAddr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Withdraw a delegator's delegation reward from a single validator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/distribution/delegators/st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2/rewards/stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p
```
Request Body
```json
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Withdraw Rewards From a Single Validator Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  }
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgWithdrawDelegationReward",
                "value": {
                    "delegator_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p"
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Withdraw Rewards From a Single Validator Tx Example"
    }
}
```
</details>
<br>


<details>
    <summary><code>POST /distribution/delegators/{delegatorAddr}/withdraw_address</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Replace the delegations' rewards withdrawal address for a new one.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/distribution/delegators/st1wkya79c9dvqrwc7um4n9vljc0duds3z5y56j7f/withdraw_address
```
Request Body
```json
{
  "base_req": {
    "from": "st1wkya79c9dvqrwc7um4n9vljc0duds3z5y56j7f",
    "memo": "Replace the Rewards Withdrawal Address Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
  "withdraw_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2"
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgModifyWithdrawAddress",
                "value": {
                    "delegator_address": "st1wkya79c9dvqrwc7um4n9vljc0duds3z5y56j7f",
                    "withdraw_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2"
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Replace the Rewards Withdrawal Address Tx Example"
    }
}
```
</details>
<br>

<details>
    <summary><code>POST /distribution/validators/{validatorAddr}/rewards</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Withdraw the validator's self-delegation and commissions rewards</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/distribution/validators/stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p/rewards
```
Request Body
```json
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Withdraw the Validator's Rewards Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  }
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgWithdrawValidatorCommission",
                "value": {
                    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p"
                }
            },
            {
                "type": "cosmos-sdk/MsgWithdrawDelegationReward",
                "value": {
                    "delegator_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p"
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Withdraw the Validator's Rewards Tx Example"
    }
}
```
</details>
<br>
-->

***

### Gov

<details>
    <summary><code>GET /gov/proposals</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all proposals information</summary>


Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals
```
Response Example:
```json
{
    "height": "6224",
    "result": [
        {
            "id": "1",
            "content": {
                "type": "cosmos-sdk/ParameterChangeProposal",
                "value": {
                    "title": "Parameter changes for validator downtime",
                    "description": "If passed, this governance proposal will do two things:\n\n1. Increase the slashing penalty for downtime from 0.01% to 0.50%\n2. Decrease the window \n\nIf this proposal passes, validators must sign at least 5% of 5,000 blocks, which is 250 blocks. That means that a validator that misses 4,750 consecutive blocks will be considered by the system to have committed a liveness violation, where previously 9,500 consecutive blocks would need to have been missed to violate these system rules. Assuming 7s block times, validators offline for approximately 9.25 consecutive hours (instead of ~18.5 hours) will be slashed 0.5% (instead of 0.01%).",
                    "changes": [
                        {
                            "subspace": "slashing",
                            "key": "SlashFractionDowntime",
                            "value": "\"0.005000000000000000\""
                        },
                        {
                            "subspace": "slashing",
                            "key": "SignedBlocksWindow",
                            "value": "\"5000\""
                        }
                    ]
                }
            },
            "status": 2,
            "final_tally_result": {
                "yes": "0",
                "abstain": "0",
                "no": "0",
                "no_with_veto": "0"
            },
            "submit_time": "2022-07-19T23:37:29.577871179Z",
            "deposit_end_time": "2022-07-21T23:37:29.577871179Z",
            "total_deposit": [
                {
                    "denom": "utros",
                    "amount": "100000000000"
                }
            ],
            "voting_start_time": "2022-07-19T23:37:29.577871179Z",
            "voting_end_time": "2022-07-21T23:37:29.577871179Z"
        },
        {
            "id": "2",
            "content": {
                "type": "cosmos-sdk/ParameterChangeProposal",
                "value": {
                    "title": "Param-Change",
                    "description": "This is a test to update deposit params in gov Module",
                    "changes": [
                        {
                            "subspace": "gov",
                            "key": "depositparams",
                            "value": "{\"max_deposit_period\":\"72800000000000\"}"
                        }
                    ]
                }
            },
            "status": 2,
            "final_tally_result": {
                "yes": "0",
                "abstain": "0",
                "no": "0",
                "no_with_veto": "0"
            },
            "submit_time": "2022-07-19T23:40:40.359241471Z",
            "deposit_end_time": "2022-07-21T23:40:40.359241471Z",
            "total_deposit": [
                {
                    "denom": "utros",
                    "amount": "1000000000000"
                }
            ],
            "voting_start_time": "2022-07-19T23:40:40.359241471Z",
            "voting_end_time": "2022-07-21T23:40:40.359241471Z"
        }
    ]
}
```
</details>
<br>


<details>
    <summary><code>GET /gov/proposals?{params}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries proposals information with parameters</summary>

Parameters:
```diff
+ voter               voter address
+ depositor           depositor addressvoter address
+ proposal_status     status of the proposals
```

Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals?status=PROPOSAL_STATUS_UNSPECIFIED
```
Response Example:
```json
{
    "height": "6187",
    "result": [
        {
            "id": "1",
            "content": {
                "type": "cosmos-sdk/ParameterChangeProposal",
                "value": {
                    "title": "Parameter changes for validator downtime",
                    "description": "If passed, this governance proposal will do two things:\n\n1. Increase the slashing penalty for downtime from 0.01% to 0.50%\n2. Decrease the window \n\nIf this proposal passes, validators must sign at least 5% of 5,000 blocks, which is 250 blocks. That means that a validator that misses 4,750 consecutive blocks will be considered by the system to have committed a liveness violation, where previously 9,500 consecutive blocks would need to have been missed to violate these system rules. Assuming 7s block times, validators offline for approximately 9.25 consecutive hours (instead of ~18.5 hours) will be slashed 0.5% (instead of 0.01%).",
                    "changes": [
                        {
                            "subspace": "slashing",
                            "key": "SlashFractionDowntime",
                            "value": "\"0.005000000000000000\""
                        },
                        {
                            "subspace": "slashing",
                            "key": "SignedBlocksWindow",
                            "value": "\"5000\""
                        }
                    ]
                }
            },
            "status": 2,
            "final_tally_result": {
                "yes": "0",
                "abstain": "0",
                "no": "0",
                "no_with_veto": "0"
            },
            "submit_time": "2022-07-19T23:37:29.577871179Z",
            "deposit_end_time": "2022-07-21T23:37:29.577871179Z",
            "total_deposit": [
                {
                    "denom": "utros",
                    "amount": "100000000000"
                }
            ],
            "voting_start_time": "2022-07-19T23:37:29.577871179Z",
            "voting_end_time": "2022-07-21T23:37:29.577871179Z"
        }
    ]
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/gov/v1beta1/proposals/{proposal_id}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries proposal details based on ProposalID</summary>

    Alias: /gov/proposals/{proposalId}

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/gov/v1beta1/proposals/2
or
https://rest-tropos.thestratos.org/gov/proposals/2
```
Response Example:
```json
{
    "proposal": {
        "proposal_id": "2",
        "content": {
            "@type": "/cosmos.params.v1beta1.ParameterChangeProposal",
            "title": "Param-Change",
            "description": "This is a test to update deposit params in gov Module",
            "changes": [
                {
                    "subspace": "gov",
                    "key": "depositparams",
                    "value": "{\"max_deposit_period\":\"72800000000000\"}"
                }
            ]
        },
        "status": "PROPOSAL_STATUS_VOTING_PERIOD",
        "final_tally_result": {
            "yes": "0",
            "abstain": "0",
            "no": "0",
            "no_with_veto": "0"
        },
        "submit_time": "2022-07-19T23:40:40.359241471Z",
        "deposit_end_time": "2022-07-21T23:40:40.359241471Z",
        "total_deposit": [
            {
                "denom": "utros",
                "amount": "1000000000000"
            }
        ],
        "voting_start_time": "2022-07-19T23:40:40.359241471Z",
        "voting_end_time": "2022-07-21T23:40:40.359241471Z"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /gov/proposals/{proposalId}/proposer</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries for the proposer for a proposal</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals/1/proposer
```
Response Example:
```json
{
    "height": "0",
    "result": {
        "proposal_id": "1",
        "proposer": "st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk"
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/gov/v1beta1/params/{params_type}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all parameters of the gov module</summary>

Request Example:

```diff
+ params_type      params_type defines which parameters to query for, can be one of "voting", "tallying" or "deposit".
```

```http
https://rest-tropos.thestratos.org/cosmos/gov/v1beta1/params/deposit
```
Response Example:
```json
{
    "voting_params": {
        "voting_period": "0s"
    },
    "deposit_params": {
        "min_deposit": [
            {
                "denom": "utros",
                "amount": "10000000000"
            }
        ],
        "max_deposit_period": "172800s"
    },
    "tally_params": {
        "quorum": "0.000000000000000000",
        "threshold": "0.000000000000000000",
        "veto_threshold": "0.000000000000000000"
    }
}
```
</details>
<br>



<details>
    <summary><code>GET /cosmos/gov/v1beta1/proposals/{proposal_id}/deposits</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all deposits of a single proposal</summary>

    Alias: /gov/proposals/{proposalId}/deposits

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/gov/v1beta1/proposals/2/deposits
or
https://rest-tropos.thestratos.org/gov/proposals/2/deposits
```
Response Example:
```json
{
    "deposits": [
        {
            "proposal_id": "2",
            "depositor": "st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk",
            "amount": [
                {
                    "denom": "utros",
                    "amount": "1000000000000"
                }
            ]
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/gov/v1beta1/proposals/{proposal_id}/deposits/{depositor}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries single deposit information based proposalID, depositAddr</summary>

    Alias: /gov/proposals/{proposalId}/deposits/{depositor}

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/gov/v1beta1/proposals/2/deposits/st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk
or
https://rest-tropos.thestratos.org/gov/proposals/2/deposits/st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk
```
Response Example:
```json
{
    "deposit": {
        "proposal_id": "2",
        "depositor": "st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk",
        "amount": [
            {
                "denom": "utros",
                "amount": "1000000000000"
            }
        ]
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/gov/v1beta1/proposals/{proposal_id}/votes</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries votes of a given proposal</summary>

    Alias: /gov/proposals/{proposalId}/votes

Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals/1/votes
or
https://rest-tropos.thestratos.org/gov/proposals/1/votes
```
Response Example:
```json
{
    "height": "0",
    "result": [
        {
            "proposal_id": "1",
            "voter": "st1fw6tcpku363yz6le7569wzzg84val68e9eayq7",
            "option": 1,
            "options": [
                {
                    "option": 1,
                    "weight": "1.000000000000000000"
                }
            ]
        },
        {
            "proposal_id": "1",
            "voter": "st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk",
            "option": 1,
            "options": [
                {
                    "option": 1,
                    "weight": "1.000000000000000000"
                }
            ]
        }
    ]
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/gov/v1beta1/proposals/{proposal_id}/votes/{voter}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries voted information based on proposalID, voterAddr</summary>

    Alias: /gov/proposals/{proposalId}/votes/{voter}

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/gov/v1beta1/proposals/1/votes/st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk
or
https://rest-tropos.thestratos.org/gov/proposals/1/votes/st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk
```
Response Example:
```json
{
    "vote": {
        "proposal_id": "1",
        "voter": "st1v33vxhmu9kp9yrncfldvt0zg9qlcepc75lyggk",
        "option": "VOTE_OPTION_YES",
        "options": [
            {
                "option": "VOTE_OPTION_YES",
                "weight": "1.000000000000000000"
            }
        ]
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/gov/v1beta1/proposals/{proposal_id}/tally</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the tally of a proposal vote</summary>


    Alias: /gov/proposals/{proposalId}/tally

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/gov/v1beta1/proposals/1/tally
or
https://rest-tropos.thestratos.org/gov/proposals/1/tally
```
Response Example:
```json
{
    "tally": {
        "yes": "500000000900",
        "abstain": "0",
        "no": "0",
        "no_with_veto": "0"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /gov/parameters/deposit</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries gov deposit parameters</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/parameters/deposit
```
Response Example:
```json
{
    "height": "6625",
    "result": {
        "min_deposit": [
            {
                "denom": "utros",
                "amount": "10000000000"
            }
        ],
        "max_deposit_period": "172800000000000"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /gov/parameters/tallying</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries governance tally parameters</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/parameters/tallying
```
Response Example:
```json
{
    "height": "6629",
    "result": {
        "quorum": "0.334000000000000000",
        "threshold": "0.500000000000000000",
        "veto_threshold": "0.334000000000000000"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /gov/parameters/voting</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries governance voting parameters</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/parameters/voting
```
Response Example:
```json
{
    "height": "6635",
    "result": {
        "voting_period": "172800000000000"
    }
}
```
</details>
<br>

<!--
<details>
    <summary><code>POST /gov/proposals</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Send transaction to submit a proposal</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals
```
Request Body:
```json
{
  "base_req": {
    "from": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
    "memo": "Submit Proposal Tx Example",
    "chain_id": "test-chain",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
  "title": "Text Proposal",
  "description": "This is a text proposal example",
  "proposal_type": "text",
  "proposer": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
  "initial_deposit": [
    {
      "denom": "ustos",
      "amount": "1000000"
    }
  ]
}
```
Response Example:
```json
{
  "type": "cosmos-sdk/StdTx",
  "value": {
    "msg": [
      {
        "type": "cosmos-sdk/MsgSubmitProposal",
        "value": {
          "content": {
            "type": "cosmos-sdk/TextProposal",
            "value": {
              "title": "Text Proposal",
              "description": "This is a text proposal example"
            }
          },
          "initial_deposit": [
            {
              "denom": "ustos",
              "amount": "1000000"
            }
          ],
          "proposer": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr"
        }
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "ustos",
          "amount": "100"
        }
      ],
      "gas": "200000"
    },
    "signatures": null,
    "memo": "Submit Proposal Tx Example"
  }
}
```
</details>
<br>

<details>
    <summary><code>POST /gov/proposals/param_change</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Generate a parameter change proposal transaction</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals/param_change
```
Request Body:
```json
{
  "base_req": {
    "from": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
    "memo": "Generate a parameter-change proposal Tx Example",
    "chain_id": "test-chain",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
  "title": "Param-Change Staking MaxValidators to 100",
  "description": "This is a test to update MaxValidators to 100 in staking Module",
  "proposer": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
  "deposit": [
    {
      "denom": "ustos",
      "amount": "10000000"
    }
  ],
  "changes": [
    {
      "subspace": "staking",
      "key": "MaxValidators",
      "value": 100
    }
  ]
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgSubmitProposal",
                "value": {
                    "content": {
                        "type": "cosmos-sdk/ParameterChangeProposal",
                        "value": {
                            "title": "Param-Change Staking MaxValidators to 100",
                            "description": "This is a test to update MaxValidators to 100 in staking Module",
                            "changes": [
                                {
                                    "subspace": "staking",
                                    "key": "MaxValidators",
                                    "value": "100"
                                }
                            ]
                        }
                    },
                    "initial_deposit": [
                        {
                            "denom": "ustos",
                            "amount": "10000000"
                        }
                    ],
                    "proposer": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr"
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Generate a parameter-change proposal Tx Example"
    }
}
```
</details>
<br>

<details>
    <summary><code>POST /gov/proposals/{proposalId}/deposits</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Deposit tokens to a proposal</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals/1/deposits
```
Request Body:
```json
{
  "base_req": {
    "from": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
    "memo": "Deposit tokens to Proposal 1 Tx Example",
    "chain_id": "test-chain",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
   "depositor": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
  "amount": [
    {
      "denom": "ustos",
      "amount": "10000000"
    }
  ]
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgDeposit",
                "value": {
                    "proposal_id": "1",
                    "depositor": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
                    "amount": [
                        {
                            "denom": "ustos",
                            "amount": "10000000"
                        }
                    ]
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Deposit tokens to Proposal 1 Tx Example"
    }
}
```
</details>
<br>

<details>
    <summary><code>POST /gov/proposals/{proposalId}/votes</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Send transaction to vote a proposal</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/gov/proposals/1/votes
```
Request Body:
```json
{
  "base_req": {
    "from": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
    "memo": "Vote Proposal 1 Tx Example",
    "chain_id": "test-chain",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
  "voter": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
  "option": "yes"
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgVote",
                "value": {
                    "proposal_id": "1",
                    "voter": "st1g3saypgcxzfzpsx94lmr30gzk0rrfc892guayr",
                    "option": "Yes"
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Vote Proposal 1 Tx Example"
    }
}
```
</details>
<br>
-->

***

### Mint

<details>
    <summary><code>GET /cosmos/mint/v1beta1/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries mint module parameters</summary>

    Alias: /minting/parameters

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/mint/v1beta1/params
or 
https://rest-tropos.thestratos.org/minting/parameters
```
Response Example:
```json
{
    "params": {
        "mint_denom": "wei",
        "inflation_rate_change": "0.130000000000000000",
        "inflation_max": "0.200000000000000000",
        "inflation_min": "0.070000000000000000",
        "goal_bonded": "0.670000000000000000",
        "blocks_per_year": "6311520"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/mint/v1beta1/inflation</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries current minting inflation value</summary>

    Alias: /minting/inflation    

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/mint/v1beta1/inflation
 or
https://rest-tropos.thestratos.org/minting/inflation
```
Response Example:
```json
{
  "inflation": "0.130016465508894587"
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/mint/v1beta1/annual_provisions</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries current minting annual provisions value</summary>

    Alias: /minting/annual-provisions
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/mint/v1beta1/annual_provisions
or 
https://rest-tropos.thestratos.org/minting/annual-provisions
```
Response Example:
```json
{
  "annual_provisions": "130019024060848.545708142618272810"
}
```
</details>
<br>

***

### Slashing

<details>
    <summary><code>GET /cosmos/slashing/v1beta1/signing_infos</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries signing info of all validators</summary>

    Alias: /slashing/signing_infos
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/slashing/v1beta1/signing_infos
or 
https://rest-tropos.thestratos.org/slashing/signing_infos
```
Response Example:
```json
{
    "info": [
        {
            "address": "stvalcons1rzn3d8qmgf7ejsfn77eag5zwjfufmvmu7sn802",
            "start_height": "0",
            "index_offset": "2986",
            "jailed_until": "1970-01-01T00:00:00Z",
            "tombstoned": false,
            "missed_blocks_counter": "0"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/slashing/v1beta1/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the current slashing parameters</summary>

    Alias: /slashing/parameters

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/slashing/v1beta1/params
or
https://rest-tropos.thestratos.org/slashing/parameters
```
Response Example:
```json
{
    "params": {
        "signed_blocks_window": "100",
        "min_signed_per_window": "0.500000000000000000",
        "downtime_jail_duration": "600s",
        "slash_fraction_double_sign": "0.050000000000000000",
        "slash_fraction_downtime": "0.010000000000000000"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/slashing/v1beta1/signing_infos/{cons_address}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the signing info of given cons address</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/slashing/v1beta1/signing_infos/stvalcons1rzn3d8qmgf7ejsfn77eag5zwjfufmvmu7sn802
```
Response Example:
```json
{
    "val_signing_info": {
        "address": "stvalcons1rzn3d8qmgf7ejsfn77eag5zwjfufmvmu7sn802",
        "start_height": "0",
        "index_offset": "3004",
        "jailed_until": "1970-01-01T00:00:00Z",
        "tombstoned": false,
        "missed_blocks_counter": "0"
    }
}
```
</details>
<br>

<!--
<details>
    <summary><code>POST /slashing/validators/{validatorAddr}/unjail</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Send transaction to unjail a jailed validator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/slashing/validators/stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p/unjail
```
Request Body
```shell
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Unjail a Jailed Validator Tx Example",
    "chain_id": "test-chain",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  }
}
```
Response Example:
```json
{
  "type": "cosmos-sdk/StdTx",
  "value": {
    "msg": [
      {
        "type": "cosmos-sdk/MsgUnjail",
        "value": {
          "address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p"
        }
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "ustos",
          "amount": "100"
        }
      ],
      "gas": "200000"
    },
    "signatures": null,
    "memo": "Unjail a Jailed Validator Tx Example"
  }
}
```
</details>
<br>
-->

***

### Staking

<details>
    <summary><code>GET /cosmos/staking/v1beta1/delegations/{delegator_addr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all delegations of a given delegator address</summary>

    Alias: /staking/delegators/{delegatorAddr}/delegations

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/delegations/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
or
https://rest-tropos.thestratos.org/staking/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/delegations
```
Response Example:
```json
{
    "delegation_responses": [
        {
            "delegation": {
                "delegator_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
                "validator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
                "shares": "500000000000.000000000000000000"
            },
            "balance": {
                "denom": "wei",
                "amount": "500000000000"
            }
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /staking/delegators/{delegatorAddr}/delegations/{validatorAddr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries current delegation between a delegator and a validator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/staking/delegators/st1fw6tcpku363yz6le7569wzzg84val68e9eayq7/delegations/stvaloper1v33vxhmu9kp9yrncfldvt0zg9qlcepc7rndg5a
```
Response Example:
```json
{
    "height": "3060",
    "result": {
        "delegation": {
            "delegator_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
            "validator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
            "shares": "500000000000.000000000000000000"
        },
        "balance": {
            "denom": "wei",
            "amount": "500000000000"
        }
    }
}
```
</details>
<br>


<!--
<details>
    <summary><code>GET /staking/redelegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all redelegations</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/staking/redelegations
```
Response Example:
```json
{
  "height": "2823",
  "result": [
    {
      "delegator_address": "st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76",
      "validator_src_address": "stvaloper1gamc7ajhzukp08nle9z9asyfx4u4dlz53dquzj",
      "validator_dst_address": "stvaloper1zgqhnz69jppcwg9z27vtq3zq9r3du5v6vjqvpq",
      "entries": [
        {
          "creation_height": 1909,
          "completion_time": "2021-09-02T19:33:26.890343914Z",
          "initial_balance": "10000",
          "shares_dst": "10000.000000000000000000",
          "balance": "10000"
        }
      ]
    }
  ]
}
```
</details>
<br>
-->

<details>
    <summary><code>GET /cosmos/staking/v1beta1/delegators/{delegator_addr}/redelegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries redelegations of given address.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/delegators/st1fw6tcpku363yz6le7569wzzg84val68e9eayq7/redelegations
```
Response Example:
```json
{
    "redelegation_responses": [
        {
            "redelegation": {
                "delegator_address": "string",
                "validator_src_address": "string",
                "validator_dst_address": "string",
                "entries": [
                    {
                        "creation_height": "string",
                        "completion_time": "2022-07-19T19:56:04.456Z",
                        "initial_balance": "string",
                        "shares_dst": "string"
                    }
                ]
            },
            "entries": [
                {
                    "redelegation_entry": {
                        "creation_height": "string",
                        "completion_time": "2022-07-19T19:56:04.456Z",
                        "initial_balance": "string",
                        "shares_dst": "string"
                    },
                    "balance": "string"
                }
            ]
        }
    ],
    "pagination": {
        "next_key": "string",
        "total": "string"
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/staking/v1beta1/delegators/{delegator_addr}/unbonding_delegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all unbonding delegations of a given delegator address</summary>

    Alias:  /staking/delegators/{delegatorAddr}/unbonding_delegations

    
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/delegators/st1fw6tcpku363yz6le7569wzzg84val68e9eayq7/unbonding_delegations
or
https://rest-tropos.thestratos.org/staking/delegators/st1fw6tcpku363yz6le7569wzzg84val68e9eayq7/unbonding_delegations
```
Response Example:
```json
{
    "unbonding_responses": [
        {
            "delegator_address": "string",
            "validator_address": "string",
            "entries": [
                {
                    "creation_height": "string",
                    "completion_time": "2022-07-19T19:59:10.339Z",
                    "initial_balance": "string",
                    "balance": "string"
                }
            ]
        }
    ],
    "pagination": {
        "next_key": "string",
        "total": "string"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /staking/delegators/{delegatorAddr}/unbonding_delegations/{validatorAddr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all unbonding delegations between a delegator and a validator</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/staking/delegators/st12adksjsd7gcsn23h5jmvdygzx2lfw5q4kgq5zh/unbonding_delegations/stvaloper12adksjsd7gcsn23h5jmvdygzx2lfw5q4pyf57u
```
Response Example:
```json
{
    "height": "2742",
    "result": {
        "delegator_address": "st12adksjsd7gcsn23h5jmvdygzx2lfw5q4kgq5zh",
        "validator_address": "stvaloper12adksjsd7gcsn23h5jmvdygzx2lfw5q4pyf57u",
        "entries": [
            {
                "creation_height": "2739",
                "completion_time": "2021-09-03T00:26:44.825391686Z",
                "initial_balance": "10000",
                "balance": "10000"
            }
        ]
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/delegators/{delegator_addr}/validators</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all validators info for given delegator address.</summary>

    Alias: /staking/delegators/{delegatorAddr}/validators
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/validators
or
https://rest-tropos.thestratos.org/staking/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/validators
```
Response Example:
```json
{
    "validators": [
        {
            "operator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
            "consensus_pubkey": {
                "@type": "/cosmos.crypto.ed25519.PubKey",
                "key": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
            },
            "jailed": false,
            "status": "BOND_STATUS_BONDED",
            "tokens": "500000000000",
            "delegator_shares": "500000000000.000000000000000000",
            "description": {
                "moniker": "node",
                "identity": "",
                "website": "",
                "security_contact": "",
                "details": ""
            },
            "unbonding_height": "0",
            "unbonding_time": "1970-01-01T00:00:00Z",
            "commission": {
                "commission_rates": {
                    "rate": "0.100000000000000000",
                    "max_rate": "0.200000000000000000",
                    "max_change_rate": "0.010000000000000000"
                },
                "update_time": "2023-01-09T17:08:58.489050300Z"
            },
            "min_self_delegation": "1"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/delegators/{delegator_addr}/validators/{validator_addr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries validator info for given delegator validator pair.</summary>

    Alias: /staking/delegators/{delegatorAddr}/validators/{validatorAddr}
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
or
https://rest-tropos.thestratos.org/staking/delegators/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
```

Response Example:
```json
{
    "height": "3158",
    "result": {
        "operator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
        "consensus_pubkey": {
            "type": "tendermint/PubKeyEd25519",
            "value": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
        },
        "status": 3,
        "tokens": "500000000000",
        "delegator_shares": "500000000000.000000000000000000",
        "description": {
            "moniker": "node"
        },
        "unbonding_time": "1970-01-01T00:00:00Z",
        "commission": {
            "commission_rates": {
                "rate": "0.100000000000000000",
                "max_rate": "0.200000000000000000",
                "max_change_rate": "0.010000000000000000"
            },
            "update_time": "2023-01-09T17:08:58.4890503Z"
        },
        "min_self_delegation": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/validators</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all validator candidates</summary>

    Alias: /staking/validators

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/validators
or
https://rest-tropos.thestratos.org/staking/validators
```


|:warning: By default it returns only the bonded validators|
|:------------------------------------|

Response Example:
```json
{
    "validators": [
        {
            "operator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
            "consensus_pubkey": {
                "@type": "/cosmos.crypto.ed25519.PubKey",
                "key": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
            },
            "jailed": false,
            "status": "BOND_STATUS_BONDED",
            "tokens": "500000000000",
            "delegator_shares": "500000000000.000000000000000000",
            "description": {
                "moniker": "node",
                "identity": "",
                "website": "",
                "security_contact": "",
                "details": ""
            },
            "unbonding_height": "0",
            "unbonding_time": "1970-01-01T00:00:00Z",
            "commission": {
                "commission_rates": {
                    "rate": "0.100000000000000000",
                    "max_rate": "0.200000000000000000",
                    "max_change_rate": "0.010000000000000000"
                },
                "update_time": "2023-01-09T17:08:58.489050300Z"
            },
            "min_self_delegation": "1"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/validators/{validator_addr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries validator info for given validator address</summary>

    Alias: /staking/validators/{validatorAddr}

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
or
https://rest-tropos.thestratos.org/staking/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu
```
Response Example:
```json
{
    "validator": {
        "operator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
        "consensus_pubkey": {
            "@type": "/cosmos.crypto.ed25519.PubKey",
            "key": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
        },
        "jailed": false,
        "status": "BOND_STATUS_BONDED",
        "tokens": "500000000000",
        "delegator_shares": "500000000000.000000000000000000",
        "description": {
            "moniker": "node",
            "identity": "",
            "website": "",
            "security_contact": "",
            "details": ""
        },
        "unbonding_height": "0",
        "unbonding_time": "1970-01-01T00:00:00Z",
        "commission": {
            "commission_rates": {
                "rate": "0.100000000000000000",
                "max_rate": "0.200000000000000000",
                "max_change_rate": "0.010000000000000000"
            },
            "update_time": "2023-01-09T17:08:58.489050300Z"
        },
        "min_self_delegation": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/validators/{validator_addr}/delegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries delegate info for given validator</summary>

    Alias: /staking/validators/{validatorAddr}/delegations
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/delegations
or
https://rest-tropos.thestratos.org/staking/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/delegations
```
Response Example:
```json
{
    "delegation_responses": [
        {
            "delegation": {
                "delegator_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
                "validator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
                "shares": "500000000000.000000000000000000"
            },
            "balance": {
                "denom": "wei",
                "amount": "500000000000"
            }
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/validators/{validator_addr}/delegations/{delegator_addr}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries delegate info for given validator delegator pair</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/validators/stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu/delegations/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
```
Response Example:
```json
{
    "delegation_response": {
        "delegation": {
            "delegator_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
            "validator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
            "shares": "500000000000.000000000000000000"
        },
        "balance": {
            "denom": "wei",
            "amount": "500000000000"
        }
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/staking/v1beta1/validators/{validator_addr}/delegations/{delegator_addr}/unbonding_delegation</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries unbonding info for given validator delegator pair</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/validators/stvaloper1v33vxhmu9kp9yrncfldvt0zg9qlcepc7rndg5a/delegations/st1fw6tcpku363yz6le7569wzzg84val68e9eayq7/unbonding_delegation
```
Response Example:
```json
{
    "unbond": {
        "delegator_address": "st1fw6tcpku363yz6le7569wzzg84val68e9eayq7",
        "validator_address": "stvaloper1v33vxhmu9kp9yrncfldvt0zg9qlcepc7rndg5a",
        "entries": [
            {
                "creation_height": "4336",
                "completion_time": "2022-08-09T21:02:38.208383315Z",
                "initial_balance": "100",
                "balance": "100"
            }
        ]
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/staking/v1beta1/validators/{validator_addr}/unbonding_delegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries unbonding delegations of a validator.</summary>

    Alias: /staking/validators/{validatorAddr}/unbonding_delegations
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/validators/stvaloper1v33vxhmu9kp9yrncfldvt0zg9qlcepc7rndg5a/unbonding_delegations
or
https://rest-tropos.thestratos.org/staking/validators/stvaloper1v33vxhmu9kp9yrncfldvt0zg9qlcepc7rndg5a/unbonding_delegations
```
Response Example:
```json
{
    "unbonding_responses": [
        {
            "delegator_address": "st1fw6tcpku363yz6le7569wzzg84val68e9eayq7",
            "validator_address": "stvaloper1v33vxhmu9kp9yrncfldvt0zg9qlcepc7rndg5a",
            "entries": [
                {
                    "creation_height": "4336",
                    "completion_time": "2022-08-09T21:02:38.208383315Z",
                    "initial_balance": "100",
                    "balance": "100"
                }
            ]
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/pool</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the current state of the staking pool</summary>

    Alias: /staking/pool
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/pool
or
https://rest-tropos.thestratos.org/staking/pool
```
Response Example:
```json
{
    "height": "3295",
    "result": {
        "not_bonded_tokens": "0",
        "bonded_tokens": "500000000000"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the current staking parameter values</summary>

    Alias: /staking/parameters
Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/params
or
https://rest-tropos.thestratos.org/staking/parameters
```
Response Example:
```json
{
    "height": "3306",
    "result": {
        "unbonding_time": "1814400000000000",
        "max_validators": 100,
        "max_entries": 7,
        "historical_entries": 10000,
        "bond_denom": "wei"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/staking/v1beta1/historical_info/{height}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the historical info for given height</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/staking/v1beta1/historical_info/3306
```
Response Example:
```json
{
    "hist": {
        "header": {
            "version": {
                "block": "11",
                "app": "0"
            },
            "chain_id": "test-chain",
            "height": "3306",
            "time": "2023-01-11T16:52:59.055776222Z",
            "last_block_id": {
                "hash": "m9Oo8OpUP0fhPJdidZlFKtAPlQhwSgEfiYKrEkqvUF8=",
                "part_set_header": {
                    "total": 1,
                    "hash": "pUakkavHHERRXfzunIB4hyPB2wPl9DeTqmgunmTsmXY="
                }
            },
            "last_commit_hash": "x7G3rcph4rtTJDmXOn/hdHwnq6jb3dLV9thcS2zv8fc=",
            "data_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "validators_hash": "UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=",
            "next_validators_hash": "UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=",
            "consensus_hash": "BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8=",
            "app_hash": "I769v0BCHX/DgctOF5/Y+mnM8m+ia11goQXvUM2uto8=",
            "last_results_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "evidence_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "proposer_address": "GKcWnBtCfZlBM/ez1FBOknids3w="
        },
        "valset": [
            {
                "operator_address": "stvaloper1pvyjzlhwrpgklu0044at4t6qh7m23k3k5xpswu",
                "consensus_pubkey": {
                    "@type": "/cosmos.crypto.ed25519.PubKey",
                    "key": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
                },
                "jailed": false,
                "status": "BOND_STATUS_BONDED",
                "tokens": "500000000000",
                "delegator_shares": "500000000000.000000000000000000",
                "description": {
                    "moniker": "node",
                    "identity": "",
                    "website": "",
                    "security_contact": "",
                    "details": ""
                },
                "unbonding_height": "0",
                "unbonding_time": "1970-01-01T00:00:00Z",
                "commission": {
                    "commission_rates": {
                        "rate": "0.100000000000000000",
                        "max_rate": "0.200000000000000000",
                        "max_change_rate": "0.010000000000000000"
                    },
                    "update_time": "2023-01-09T17:08:58.489050300Z"
                },
                "min_self_delegation": "1"
            }
        ]
    }
}
```
</details>
<br>



<!--
<details>
    <summary><code>POST /staking/delegators/{delegatorAddr}/delegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Submit delegation</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/staking/delegators/st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2/delegations
```
Request Body:
```json
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Submit Delegation Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
   "delegator_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p",
    "amount": {
        "denom": "ustos",
        "amount": "10000"
    }
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgDelegate",
                "value": {
                    "delegator_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p",
                    "amount": {
                        "denom": "ustos",
                        "amount": "10000"
                    }
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Submit Delegation Tx Example"
    }
}
```
</details>
<br>

<details>
    <summary><code>POST /staking/delegators/{delegatorAddr}/unbonding_delegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Submit an unbonding delegation</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/staking/delegators/st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2/unbonding_delegations
```
Request Body:
```json
{
  "base_req": {
    "from": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "memo": "Submit Unbonding-delegation Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
   "delegator_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p",
    "amount": {
        "denom": "ustos",
        "amount": "10000"
    }
}
```
Response Example:
```json
{
    "type": "cosmos-sdk/StdTx",
    "value": {
        "msg": [
            {
                "type": "cosmos-sdk/MsgUndelegate",
                "value": {
                    "delegator_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                    "validator_address": "stvaloper1xnhfx7c0nev9me835409efjj7whd672x8ky28p",
                    "amount": {
                        "denom": "ustos",
                        "amount": "10000"
                    }
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "ustos",
                    "amount": "100"
                }
            ],
            "gas": "200000"
        },
        "signatures": null,
        "memo": "Submit Unbonding-delegation Tx Example"
    }
}
```
</details>
<br>

<details>
    <summary><code>POST /staking/delegators/{delegatorAddr}/redelegations</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Submit a redelegation</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/staking/delegators/st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76/redelegations
```
Request Body:
```json
{
  "base_req": {
    "from": "st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76",
    "memo": "Submit Re-delegation Tx Example",
    "chain_id": "test-chain",
    "account_number": "0",
    "gas": "200000",
    "gas_adjustment": "1.2",
    "fees": [
      {
        "denom": "ustos",
        "amount": "100"
      }
    ],
    "simulate": false
  },
  "delegator_address": "st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76",
  "validator_src_address": "stvaloper1gamc7ajhzukp08nle9z9asyfx4u4dlz53dquzj",
  "validator_dst_address": "stvaloper1zgqhnz69jppcwg9z27vtq3zq9r3du5v6vjqvpq",
  "amount": {
    "denom": "ustos",
    "amount": "10000"
  }
}
```
Response Example:
```json
{
  "type": "cosmos-sdk/StdTx",
  "value": {
    "msg": [
      {
        "type": "cosmos-sdk/MsgBeginRedelegate",
        "value": {
          "delegator_address": "st15xlpwafgnvvs5hdk8938dp2ve6cjmy4vcf4l76",
          "validator_src_address": "stvaloper1gamc7ajhzukp08nle9z9asyfx4u4dlz53dquzj",
          "validator_dst_address": "stvaloper1zgqhnz69jppcwg9z27vtq3zq9r3du5v6vjqvpq",
          "amount": {
            "denom": "ustos",
            "amount": "10000"
          }
        }
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "ustos",
          "amount": "100"
        }
      ],
      "gas": "200000"
    },
    "signatures": null,
    "memo": "Submit Re-delegation Tx Example"
  }
}
```
</details>
<br>
-->

***

### Service

<details>
    <summary><code>GET /cosmos/base/tendermint/v1beta1/blocks/latest</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; returns the latest block</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/base/tendermint/v1beta1/blocks/latest
```
Response Example:
```json
{
    "block_id": {
        "hash": "sMqrEks0H7oR/8svEPNfQNDs/ohLHMRF63VOd5nUCIo=",
        "part_set_header": {
            "total": 1,
            "hash": "KAPRCqsANSLnnJy5SplBnGtP3c1xPZ+IUe0k2pUU8aE="
        }
    },
    "block": {
        "header": {
            "version": {
                "block": "11",
                "app": "0"
            },
            "chain_id": "test-chain",
            "height": "3342",
            "time": "2023-01-11T16:55:59.811290254Z",
            "last_block_id": {
                "hash": "FdTnVbBsS5nnK66URj5Lv6v6/889XdJTyShJC6eREvY=",
                "part_set_header": {
                    "total": 1,
                    "hash": "ucD3c65YyXhsqe21apRAs9R4Ytw3TBM42yVNV1hohKg="
                }
            },
            "last_commit_hash": "/oU+N+xWq/0a1vHz9hte2BlMGn33LQcJAdNWaYOaX1s=",
            "data_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "validators_hash": "UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=",
            "next_validators_hash": "UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=",
            "consensus_hash": "BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8=",
            "app_hash": "Pgf0fbYlN8UPoOOI5c0qQZCuuL3Q32NV8swM+OJnvLo=",
            "last_results_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "evidence_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "proposer_address": "GKcWnBtCfZlBM/ez1FBOknids3w="
        },
        "data": {
            "txs": []
        },
        "evidence": {
            "evidence": []
        },
        "last_commit": {
            "height": "3341",
            "round": 0,
            "block_id": {
                "hash": "FdTnVbBsS5nnK66URj5Lv6v6/889XdJTyShJC6eREvY=",
                "part_set_header": {
                    "total": 1,
                    "hash": "ucD3c65YyXhsqe21apRAs9R4Ytw3TBM42yVNV1hohKg="
                }
            },
            "signatures": [
                {
                    "block_id_flag": "BLOCK_ID_FLAG_COMMIT",
                    "validator_address": "GKcWnBtCfZlBM/ez1FBOknids3w=",
                    "timestamp": "2023-01-11T16:55:59.811290254Z",
                    "signature": "tspmnLBjoCTfUbfh1gv1/YTnCOlcJAjadfbguSFvWB+GwROVoxcPvGjxqHBiFbKvyG/yJTjr4FSauLXDvoBgAw=="
                }
            ]
        }
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/base/tendermint/v1beta1/blocks/{height}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries block for given height</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/base/tendermint/v1beta1/blocks/latest
```

Response Example:
```json
{
    "block_id": {
        "hash": "v3fqwUVL/XL0rwCxmO4x96Euvy2V7ZJso8+4rqohl9o=",
        "part_set_header": {
            "total": 1,
            "hash": "t8wCFw2/VHBOHYAIO4k7MZNgUIzjBazvNjQS3R89NuM="
        }
    },
    "block": {
        "header": {
            "version": {
                "block": "11",
                "app": "0"
            },
            "chain_id": "test-chain",
            "height": "3355",
            "time": "2023-01-11T16:57:05.012011668Z",
            "last_block_id": {
                "hash": "IdID2P6phDleoQAdMrLwzVr2DY02Omx3VnlATf4TwKI=",
                "part_set_header": {
                    "total": 1,
                    "hash": "D/UcqWz7vvjUZ+yBezcVNymswrPpsYNMC0YfW5veVBM="
                }
            },
            "last_commit_hash": "//C9EwF2qjAPjGaykugAv4N4kRY3+DqiXJI/QRMtSfk=",
            "data_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "validators_hash": "UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=",
            "next_validators_hash": "UjS9kaOnUeBVw1h2V43kpGYxGoDVQLWYha9o721NVt4=",
            "consensus_hash": "BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8=",
            "app_hash": "vJ+axVi3DlFFnA6bPzqAaco9J3mXsObourreZUhz01M=",
            "last_results_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "evidence_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "proposer_address": "GKcWnBtCfZlBM/ez1FBOknids3w="
        },
        "data": {
            "txs": [
            ]
        },
        "evidence": {
            "evidence": [
            ]
        },
        "last_commit": {
            "height": "3354",
            "round": 0,
            "block_id": {
                "hash": "IdID2P6phDleoQAdMrLwzVr2DY02Omx3VnlATf4TwKI=",
                "part_set_header": {
                    "total": 1,
                    "hash": "D/UcqWz7vvjUZ+yBezcVNymswrPpsYNMC0YfW5veVBM="
                }
            },
            "signatures": [
                {
                    "block_id_flag": "BLOCK_ID_FLAG_COMMIT",
                    "validator_address": "GKcWnBtCfZlBM/ez1FBOknids3w=",
                    "timestamp": "2023-01-11T16:57:05.012011668Z",
                    "signature": "dHppKwAZFYzv19VLgmngOKq/Un2zJpZ5Fg7llx0iTNo72pbXXvSPi7BSvsqOzd4AKWTtO3XgQ6X97jxOpKd0CQ=="
                }
            ]
        }
    }
}
```
</details>
<br>


<details>
    <summary><code>GET /cosmos/base/tendermint/v1beta1/node_info</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries the current node info</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/base/tendermint/v1beta1/node_info
```

Response Example:
```json
{
    "default_node_info": {
        "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
        },
        "default_node_id": "16a0758d175cbf5c08d41dffa73eb5c0190869ed",
        "listen_addr": "tcp://0.0.0.0:26656",
        "network": "test-chain",
        "version": "0.34.21",
        "channels": "QCAhIiMwOGBhAA==",
        "moniker": "node",
        "other": {
            "tx_index": "on",
            "rpc_address": "tcp://127.0.0.1:26657"
        }
    },
    "application_version": {
        "name": "",
        "app_name": "stchaind",
        "version": "v0.8.0",
        "git_commit": "",
        "build_tags": "",
        "go_version": "go version go1.19.4 linux/amd64",
        "build_deps": [
            {
                "path": "filippo.io/edwards25519",
                "version": "v1.0.0-beta.2",
                "sum": "h1:/BZRNzm8N4K4eWfK28dL4yescorxtO7YG1yun8fy+pI="
            },
            {
                "path": "github.com/99designs/keyring",
                "version": "v1.1.6",
                "sum": ""
            },
            {
                "path": "github.com/ChainSafe/go-schnorrkel",
                "version": "v0.0.0-20200405005733-88cbf1b4c40d",
                "sum": "h1:nalkkPQcITbvhmL4+C4cKA87NW0tfm3Kl9VXRoPywFg="
            },
            {
                "path": "github.com/VictoriaMetrics/fastcache",
                "version": "v1.6.0",
                "sum": "h1:C/3Oi3EiBCqufydp1neRZkqcwmEiuRT9c3fqvvgKm5o="
            },
            {
                "path": "github.com/Workiva/go-datastructures",
                "version": "v1.0.53",
                "sum": "h1:J6Y/52yX10Xc5JjXmGtWoSSxs3mZnGSaq37xZZh7Yig="
            },
            {
                "path": "github.com/armon/go-metrics",
                "version": "v0.3.10",
                "sum": "h1:FR+drcQStOe+32sYyJYyZ7FIdgoGGBnwLl+flodp8Uo="
            },
            {
                "path": "github.com/beorn7/perks",
                "version": "v1.0.1",
                "sum": "h1:VlbKKnNfV8bJzeqoa4cOKqO6bYr3WgKZxO8Z16+hsOM="
            },
            {
                "path": "github.com/bgentry/speakeasy",
                "version": "v0.1.0",
                "sum": "h1:ByYyxL9InA1OWqxJqqp2A5pYHUrCiAL6K3J+LKSsQkY="
            },
            {
                "path": "github.com/btcsuite/btcd",
                "version": "v0.22.1",
                "sum": "h1:CnwP9LM/M9xuRrGSCGeMVs9iv09uMqwsVX7EeIpgV2c="
            },
            {
                "path": "github.com/btcsuite/btcd/chaincfg/chainhash",
                "version": "v1.0.1",
                "sum": "h1:q0rUy8C/TYNBQS1+CGKw68tLOFYSNEs0TFnxxnS9+4U="
            },
            {
                "path": "github.com/btcsuite/btcutil",
                "version": "v1.0.3-0.20201208143702-a53e38424cce",
                "sum": "h1:YtWJF7RHm2pYCvA5t0RPmAaLUhREsKuKd+SLhxFbFeQ="
            },
            {
                "path": "github.com/cenkalti/backoff/v4",
                "version": "v4.1.1",
                "sum": "h1:G2HAfAmvm/GcKan2oOQpBXOd2tT2G57ZnZGWa1PxPBQ="
            },
            {
                "path": "github.com/cespare/xxhash/v2",
                "version": "v2.1.2",
                "sum": "h1:YRXhKfTDauu4ajMg1TPgFO5jnlC2HCbmLXMcTG5cbYE="
            },
            {
                "path": "github.com/coinbase/rosetta-sdk-go",
                "version": "v0.7.0",
                "sum": "h1:lmTO/JEpCvZgpbkOITL95rA80CPKb5CtMzLaqF2mCNg="
            },
            {
                "path": "github.com/confio/ics23/go",
                "version": "v0.7.0",
                "sum": ""
            },
            {
                "path": "github.com/cosmos/btcutil",
                "version": "v1.0.4",
                "sum": "h1:n7C2ngKXo7UC9gNyMNLbzqz7Asuf+7Qv4gnX/rOdQ44="
            },
            {
                "path": "github.com/cosmos/cosmos-sdk",
                "version": "v0.45.9",
                "sum": "h1:Z4s1EZL/mfM8uSSZr8WmyEbWp4hqbWVI5sAIFR432KY="
            },
            {
                "path": "github.com/cosmos/go-bip39",
                "version": "v1.0.0",
                "sum": "h1:pcomnQdrdH22njcAatO0yWojsUnCO3y2tNoV1cb6hHY="
            },
            {
                "path": "github.com/cosmos/iavl",
                "version": "v0.19.3",
                "sum": "h1:cESO0OwTTxQm5rmyESKW+zESheDUYI7CcZDWWDwnuxg="
            },
            {
                "path": "github.com/cosmos/ibc-go/v3",
                "version": "v3.0.0",
                "sum": "h1:XUNplHVS51Q2gMnTFsFsH9QJ7flsovMamnltKbEgPQ4="
            },
            {
                "path": "github.com/creachadair/taskgroup",
                "version": "v0.3.2",
                "sum": "h1:zlfutDS+5XG40AOxcHDSThxKzns8Tnr9jnr6VqkYlkM="
            },
            {
                "path": "github.com/davecgh/go-spew",
                "version": "v1.1.1",
                "sum": "h1:vj9j/u1bqnvCEfJOwUhtlOARqs3+rkHYY13jYWTU97c="
            },
            {
                "path": "github.com/deckarep/golang-set",
                "version": "v1.8.0",
                "sum": "h1:sk9/l/KqpunDwP7pSjUg0keiOOLEnOBHzykLrsPppp4="
            },
            {
                "path": "github.com/desertbit/timer",
                "version": "v0.0.0-20180107155436-c41aec40b27f",
                "sum": "h1:U5y3Y5UE0w7amNe7Z5G/twsBW0KEalRQXZzf8ufSh9I="
            },
            {
                "path": "github.com/dvsekhvalnov/jose2go",
                "version": "v0.0.0-20200901110807-248326c1351b",
                "sum": "h1:HBah4D48ypg3J7Np4N+HY/ZR76fx3HEUGxDU6Uk39oQ="
            },
            {
                "path": "github.com/edsrzf/mmap-go",
                "version": "v1.0.0",
                "sum": "h1:CEBF7HpRnUCSJgGUb5h1Gm7e3VkmVDrR8lvWVLtrOFw="
            },
            {
                "path": "github.com/ethereum/go-ethereum",
                "version": "v1.10.16",
                "sum": "h1:3oPrumn0bCW/idjcxMn5YYVCdK7VzJYIvwGZUGLEaoc="
            },
            {
                "path": "github.com/felixge/httpsnoop",
                "version": "v1.0.1",
                "sum": "h1:lvB5Jl89CsZtGIWuTcDM1E/vkVs49/Ml7JJe07l8SPQ="
            },
            {
                "path": "github.com/fsnotify/fsnotify",
                "version": "v1.5.4",
                "sum": "h1:jRbGcIw6P2Meqdwuo0H1p6JVLbL5DHKAKlYndzMwVZI="
            },
            {
                "path": "github.com/gballet/go-libpcsclite",
                "version": "v0.0.0-20190607065134-2772fd86a8ff",
                "sum": "h1:tY80oXqGNY4FhTFhk+o9oFHGINQ/+vhlm8HFzi6znCI="
            },
            {
                "path": "github.com/go-kit/kit",
                "version": "v0.12.0",
                "sum": "h1:e4o3o3IsBfAKQh5Qbbiqyfu97Ku7jrO/JbohvztANh4="
            },
            {
                "path": "github.com/go-kit/log",
                "version": "v0.2.1",
                "sum": "h1:MRVx0/zhvdseW+Gza6N9rVzU/IVzaeE1SFI4raAhmBU="
            },
            {
                "path": "github.com/go-logfmt/logfmt",
                "version": "v0.5.1",
                "sum": "h1:otpy5pqBCBZ1ng9RQ0dPu4PN7ba75Y/aA+UpowDyNVA="
            },
            {
                "path": "github.com/go-stack/stack",
                "version": "v1.8.0",
                "sum": "h1:5SgMzNM5HxrEjV0ww2lTmX6E2Izsfxas4+YHWRs3Lsk="
            },
            {
                "path": "github.com/godbus/dbus",
                "version": "v0.0.0-20190726142602-4481cbc300e2",
                "sum": "h1:ZpnhV/YsD2/4cESfV5+Hoeu/iUR3ruzNvZ+yQfO03a0="
            },
            {
                "path": "github.com/gogo/gateway",
                "version": "v1.1.0",
                "sum": "h1:u0SuhL9+Il+UbjM9VIE3ntfRujKbvVpFvNB4HbjeVQ0="
            },
            {
                "path": "github.com/gogo/protobuf",
                "version": "v1.3.3",
                "sum": ""
            },
            {
                "path": "github.com/golang/protobuf",
                "version": "v1.5.2",
                "sum": "h1:ROPKBNFfQgOUMifHyP+KYbvpjbdoFNs+aK7DXlji0Tw="
            },
            {
                "path": "github.com/golang/snappy",
                "version": "v0.0.4",
                "sum": "h1:yAGX7huGHXlcLOEtBnF4w7FQwA26wojNCwOYAEhLjQM="
            },
            {
                "path": "github.com/google/btree",
                "version": "v1.0.0",
                "sum": "h1:0udJVsspx3VBr5FwtLhQQtuAsVc79tTq0ocGIPAU6qo="
            },
            {
                "path": "github.com/google/orderedcode",
                "version": "v0.0.1",
                "sum": "h1:UzfcAexk9Vhv8+9pNOgRu41f16lHq725vPwnSeiG/Us="
            },
            {
                "path": "github.com/google/uuid",
                "version": "v1.3.0",
                "sum": "h1:t6JiXgmwXMjEs8VusXIJk2BXHsn+wx8BZdTaoZ5fu7I="
            },
            {
                "path": "github.com/gorilla/handlers",
                "version": "v1.5.1",
                "sum": "h1:9lRY6j8DEeeBT10CvO9hGW0gmky0BprnvDI5vfhUHH4="
            },
            {
                "path": "github.com/gorilla/mux",
                "version": "v1.8.0",
                "sum": "h1:i40aqfkR1h2SlN9hojwV5ZA91wcXFOvkdNIeFDP5koI="
            },
            {
                "path": "github.com/gorilla/websocket",
                "version": "v1.5.0",
                "sum": "h1:PPwGk2jz7EePpoHN/+ClbZu8SPxiqlu12wZP/3sWmnc="
            },
            {
                "path": "github.com/grpc-ecosystem/go-grpc-middleware",
                "version": "v1.3.0",
                "sum": "h1:+9834+KizmvFV7pXQGSXQTsaWhq2GjuNUt0aUU0YBYw="
            },
            {
                "path": "github.com/grpc-ecosystem/grpc-gateway",
                "version": "v1.16.0",
                "sum": "h1:gmcG1KaJ57LophUzW0Hy8NmPhnMZb4M0+kPpLofRdBo="
            },
            {
                "path": "github.com/gsterjov/go-libsecret",
                "version": "v0.0.0-20161001094733-a6f4afe4910c",
                "sum": "h1:6rhixN/i8ZofjG1Y75iExal34USq5p+wiN1tpie8IrU="
            },
            {
                "path": "github.com/gtank/merlin",
                "version": "v0.1.1",
                "sum": "h1:eQ90iG7K9pOhtereWsmyRJ6RAwcP4tHTDBHXNg+u5is="
            },
            {
                "path": "github.com/gtank/ristretto255",
                "version": "v0.1.2",
                "sum": "h1:JEqUCPA1NvLq5DwYtuzigd7ss8fwbYay9fi4/5uMzcc="
            },
            {
                "path": "github.com/hashicorp/go-immutable-radix",
                "version": "v1.3.1",
                "sum": "h1:DKHmCUm2hRBK510BaiZlwvpD40f8bJFeZnpfm2KLowc="
            },
            {
                "path": "github.com/hashicorp/golang-lru",
                "version": "v0.5.5-0.20210104140557-80c98217689d",
                "sum": "h1:dg1dEPuWpEqDnvIw251EVy4zlP8gWbsGj4BsUKCRpYs="
            },
            {
                "path": "github.com/hashicorp/hcl",
                "version": "v1.0.0",
                "sum": "h1:0Anlzjpi4vEasTeNFn2mLJgTSwt0+6sfsiTG8qcWGx4="
            },
            {
                "path": "github.com/hdevalence/ed25519consensus",
                "version": "v0.0.0-20210204194344-59a8610d2b87",
                "sum": "h1:uUjLpLt6bVvZ72SQc/B4dXcPBw4Vgd7soowdRl52qEM="
            },
            {
                "path": "github.com/holiman/bloomfilter/v2",
                "version": "v2.0.3",
                "sum": "h1:73e0e/V0tCydx14a0SCYS/EWCxgwLZ18CZcZKVu0fao="
            },
            {
                "path": "github.com/holiman/uint256",
                "version": "v1.2.0",
                "sum": "h1:gpSYcPLWGv4sG43I2mVLiDZCNDh/EpGjSk8tmtxitHM="
            },
            {
                "path": "github.com/huin/goupnp",
                "version": "v1.0.2",
                "sum": "h1:RfGLP+h3mvisuWEyybxNq5Eft3NWhHLPeUN72kpKZoI="
            },
            {
                "path": "github.com/improbable-eng/grpc-web",
                "version": "v0.15.0",
                "sum": "h1:BN+7z6uNXZ1tQGcNAuaU1YjsLTApzkjt2tzCixLaUPQ="
            },
            {
                "path": "github.com/ipfs/go-cid",
                "version": "v0.1.0",
                "sum": "h1:YN33LQulcRHjfom/i25yoOZR4Telp1Hr/2RU3d0PnC0="
            },
            {
                "path": "github.com/jackpal/go-nat-pmp",
                "version": "v1.0.2",
                "sum": "h1:KzKSgb7qkJvOUTqYl9/Hg/me3pWgBmERKrTGD7BdWus="
            },
            {
                "path": "github.com/klauspost/compress",
                "version": "v1.15.9",
                "sum": "h1:wKRjX6JRtDdrE9qwa4b/Cip7ACOshUI4smpCQanqjSY="
            },
            {
                "path": "github.com/klauspost/cpuid/v2",
                "version": "v2.0.4",
                "sum": "h1:g0I61F2K2DjRHz1cnxlkNSBIaePVoJIjjnHui8QHbiw="
            },
            {
                "path": "github.com/lib/pq",
                "version": "v1.10.6",
                "sum": "h1:jbk+ZieJ0D7EVGJYpL9QTz7/YW6UHbmdnZWYyK5cdBs="
            },
            {
                "path": "github.com/libp2p/go-buffer-pool",
                "version": "v0.1.0",
                "sum": "h1:oK4mSFcQz7cTQIfqbe4MIj9gLW+mnanjyFtc6cdF0Y8="
            },
            {
                "path": "github.com/magiconair/properties",
                "version": "v1.8.6",
                "sum": "h1:5ibWZ6iY0NctNGWo87LalDlEZ6R41TqbbDamhfG/Qzo="
            },
            {
                "path": "github.com/mattn/go-colorable",
                "version": "v0.1.12",
                "sum": "h1:jF+Du6AlPIjs2BiUiQlKOX0rt3SujHxPnksPKZbaA40="
            },
            {
                "path": "github.com/mattn/go-isatty",
                "version": "v0.0.14",
                "sum": "h1:yVuAays6BHfxijgZPzw+3Zlu5yQgKGP2/hcQbHb7S9Y="
            },
            {
                "path": "github.com/mattn/go-runewidth",
                "version": "v0.0.9",
                "sum": "h1:Lm995f3rfxdpd6TSmuVCHVb/QhupuXlYr8sCI/QdE+0="
            },
            {
                "path": "github.com/matttproud/golang_protobuf_extensions",
                "version": "v1.0.2-0.20181231171920-c182affec369",
                "sum": "h1:I0XW9+e1XWDxdcEniV4rQAIOPUGDq67JSCiRCgGCZLI="
            },
            {
                "path": "github.com/mimoo/StrobeGo",
                "version": "v0.0.0-20181016162300-f8f6d4d2b643",
                "sum": "h1:hLDRPB66XQT/8+wG9WsDpiCvZf1yKO7sz7scAjSlBa0="
            },
            {
                "path": "github.com/minio/blake2b-simd",
                "version": "v0.0.0-20160723061019-3f5f724cb5b1",
                "sum": "h1:lYpkrQH5ajf0OXOcUbGjvZxxijuBwbbmlSxLiuofa+g="
            },
            {
                "path": "github.com/minio/highwayhash",
                "version": "v1.0.2",
                "sum": "h1:Aak5U0nElisjDCfPSG79Tgzkn2gl66NxOMspRrKnA/g="
            },
            {
                "path": "github.com/minio/sha256-simd",
                "version": "v1.0.0",
                "sum": "h1:v1ta+49hkWZyvaKwrQB8elexRqm6Y0aMLjCNsrYxo6g="
            },
            {
                "path": "github.com/mitchellh/mapstructure",
                "version": "v1.5.0",
                "sum": "h1:jeMsZIYE/09sWLaz43PL7Gy6RuMjD2eJVyuac5Z2hdY="
            },
            {
                "path": "github.com/mr-tron/base58",
                "version": "v1.2.0",
                "sum": "h1:T/HDJBh4ZCPbU39/+c3rRvE0uKBQlU27+QI8LJ4t64o="
            },
            {
                "path": "github.com/mtibben/percent",
                "version": "v0.2.1",
                "sum": "h1:5gssi8Nqo8QU/r2pynCm+hBQHpkB/uNK7BJCFogWdzs="
            },
            {
                "path": "github.com/multiformats/go-base32",
                "version": "v0.0.3",
                "sum": "h1:tw5+NhuwaOjJCC5Pp82QuXbrmLzWg7uxlMFp8Nq/kkI="
            },
            {
                "path": "github.com/multiformats/go-base36",
                "version": "v0.1.0",
                "sum": "h1:JR6TyF7JjGd3m6FbLU2cOxhC0Li8z8dLNGQ89tUg4F4="
            },
            {
                "path": "github.com/multiformats/go-multibase",
                "version": "v0.0.3",
                "sum": "h1:l/B6bJDQjvQ5G52jw4QGSYeOTZoAwIO77RblWplfIqk="
            },
            {
                "path": "github.com/multiformats/go-multihash",
                "version": "v0.0.15",
                "sum": "h1:hWOPdrNqDjwHDx82vsYGSDZNyktOJJ2dzZJzFkOV1jM="
            },
            {
                "path": "github.com/multiformats/go-varint",
                "version": "v0.0.6",
                "sum": "h1:gk85QWKxh3TazbLxED/NlDVv8+q+ReFJk7Y2W/KhfNY="
            },
            {
                "path": "github.com/olekukonko/tablewriter",
                "version": "v0.0.5",
                "sum": "h1:P2Ga83D34wi1o9J6Wh1mRuqd4mF/x/lgBS7N7AbDhec="
            },
            {
                "path": "github.com/pelletier/go-toml/v2",
                "version": "v2.0.2",
                "sum": "h1:+jQXlF3scKIcSEKkdHzXhCTDLPFi5r1wnK6yPS+49Gw="
            },
            {
                "path": "github.com/pkg/errors",
                "version": "v0.9.1",
                "sum": "h1:FEBLx1zS214owpjy7qsBeixbURkuhQAwrK5UwLGTwt4="
            },
            {
                "path": "github.com/pmezard/go-difflib",
                "version": "v1.0.0",
                "sum": "h1:4DBwDE0NGyQoBHbLQYPwSUPoCMWR5BEzIk/f1lZbAQM="
            },
            {
                "path": "github.com/prometheus/client_golang",
                "version": "v1.12.2",
                "sum": "h1:51L9cDoUHVrXx4zWYlcLQIZ+d+VXHgqnYKkIuq4g/34="
            },
            {
                "path": "github.com/prometheus/client_model",
                "version": "v0.2.0",
                "sum": "h1:uq5h0d+GuxiXLJLNABMgp2qUWDPiLvgCzz2dUR+/W/M="
            },
            {
                "path": "github.com/prometheus/common",
                "version": "v0.34.0",
                "sum": "h1:RBmGO9d/FVjqHT0yUGQwBJhkwKV+wPCn7KGpvfab0uE="
            },
            {
                "path": "github.com/prometheus/procfs",
                "version": "v0.7.3",
                "sum": "h1:4jVXhlkAyzOScmCkXBTOLRLTz8EeU+eyjrwB/EPq0VU="
            },
            {
                "path": "github.com/prometheus/tsdb",
                "version": "v0.7.1",
                "sum": "h1:YZcsG11NqnK4czYLrWd9mpEuAJIHVQLwdrleYfszMAA="
            },
            {
                "path": "github.com/rakyll/statik",
                "version": "v0.1.7",
                "sum": "h1:OF3QCZUuyPxuGEP7B4ypUa7sB/iHtqOTDYZXGM8KOdQ="
            },
            {
                "path": "github.com/rcrowley/go-metrics",
                "version": "v0.0.0-20200313005456-10cdbea86bc0",
                "sum": "h1:MkV+77GLUNo5oJ0jf870itWm3D0Sjh7+Za9gazKc5LQ="
            },
            {
                "path": "github.com/regen-network/cosmos-proto",
                "version": "v0.3.1",
                "sum": "h1:rV7iM4SSFAagvy8RiyhiACbWEGotmqzywPxOvwMdxcg="
            },
            {
                "path": "github.com/rjeczalik/notify",
                "version": "v0.9.1",
                "sum": "h1:CLCKso/QK1snAlnhNR/CNvNiFU2saUtjV0bx3EwNeCE="
            },
            {
                "path": "github.com/rs/cors",
                "version": "v1.8.2",
                "sum": "h1:KCooALfAYGs415Cwu5ABvv9n9509fSiG5SQJn/AQo4U="
            },
            {
                "path": "github.com/rs/zerolog",
                "version": "v1.27.0",
                "sum": "h1:1T7qCieN22GVc8S4Q2yuexzBb1EqjbgjSH9RohbMjKs="
            },
            {
                "path": "github.com/shirou/gopsutil",
                "version": "v3.21.4-0.20210419000835-c7a38de76ee5+incompatible",
                "sum": "h1:Bn1aCHHRnjv4Bl16T8rcaFjYSrGrIZvpiGO6P3Q4GpU="
            },
            {
                "path": "github.com/spf13/afero",
                "version": "v1.8.2",
                "sum": "h1:xehSyVa0YnHWsJ49JFljMpg1HX19V6NDZ1fkm1Xznbo="
            },
            {
                "path": "github.com/spf13/cast",
                "version": "v1.5.0",
                "sum": "h1:rj3WzYc11XZaIZMPKmwP96zkFEnnAmV8s6XbB2aY32w="
            },
            {
                "path": "github.com/spf13/cobra",
                "version": "v1.5.0",
                "sum": "h1:X+jTBEBqF0bHN+9cSMgmfuvv2VHJ9ezmFNf9Y/XstYU="
            },
            {
                "path": "github.com/spf13/jwalterweatherman",
                "version": "v1.1.0",
                "sum": "h1:ue6voC5bR5F8YxI5S67j9i582FU4Qvo2bmqnqMYADFk="
            },
            {
                "path": "github.com/spf13/pflag",
                "version": "v1.0.5",
                "sum": "h1:iy+VFUOCP1a+8yFto/drg2CJ5u0yRoB7fZw3DKv/JXA="
            },
            {
                "path": "github.com/spf13/viper",
                "version": "v1.12.0",
                "sum": "h1:CZ7eSOd3kZoaYDLbXnmzgQI5RlciuXBMA+18HwHRfZQ="
            },
            {
                "path": "github.com/status-im/keycard-go",
                "version": "v0.0.0-20200402102358-957c09536969",
                "sum": "h1:Oo2KZNP70KE0+IUJSidPj/BFS/RXNHmKIJOdckzml2E="
            },
            {
                "path": "github.com/stretchr/testify",
                "version": "v1.8.0",
                "sum": "h1:pSgiaMZlXftHpm5L7V1+rVB+AZJydKsMxsQBIJw4PKk="
            },
            {
                "path": "github.com/subosito/gotenv",
                "version": "v1.4.0",
                "sum": "h1:yAzM1+SmVcz5R4tXGsNMu1jUl2aOJXoiWUCEwwnGrvs="
            },
            {
                "path": "github.com/syndtr/goleveldb",
                "version": "v1.0.1-0.20210819022825-2ae1ddf74ef7",
                "sum": "h1:epCh84lMvA70Z7CTTCmYQn2CKbY8j86K7/FAIr141uY="
            },
            {
                "path": "github.com/tendermint/btcd",
                "version": "v0.1.1",
                "sum": "h1:0VcxPfflS2zZ3RiOAHkBiFUcPvbtRj5O7zHmcJWHV7s="
            },
            {
                "path": "github.com/tendermint/crypto",
                "version": "v0.0.0-20191022145703-50d29ede1e15",
                "sum": "h1:hqAk8riJvK4RMWx1aInLzndwxKalgi5rTqgfXxOxbEI="
            },
            {
                "path": "github.com/tendermint/go-amino",
                "version": "v0.16.0",
                "sum": "h1:GyhmgQKvqF82e2oZeuMSp9JTN0N09emoSZlb2lyGa2E="
            },
            {
                "path": "github.com/tendermint/tendermint",
                "version": "v0.34.21",
                "sum": "h1:UiGGnBFHVrZhoQVQ7EfwSOLuCtarqCSsRf8VrklqB7s="
            },
            {
                "path": "github.com/tendermint/tm-db",
                "version": "v0.6.7",
                "sum": "h1:fE00Cbl0jayAoqlExN6oyQJ7fR/ZtoVOmvPJ//+shu8="
            },
            {
                "path": "github.com/tklauser/go-sysconf",
                "version": "v0.3.5",
                "sum": "h1:uu3Xl4nkLzQfXNsWn15rPc/HQCJKObbt1dKJeWp3vU4="
            },
            {
                "path": "github.com/tklauser/numcpus",
                "version": "v0.2.2",
                "sum": "h1:oyhllyrScuYI6g+h/zUvNXNp1wy7x8qQy3t/piefldA="
            },
            {
                "path": "github.com/tyler-smith/go-bip39",
                "version": "v1.1.0",
                "sum": "h1:5eUemwrMargf3BSLRRCalXT93Ns6pQJIjYQN2nyfOP8="
            },
            {
                "path": "golang.org/x/crypto",
                "version": "v0.0.0-20220525230936-793ad666bf5e",
                "sum": "h1:T8NU3HyQ8ClP4SEE+KbFlg6n0NhuTsN4MyznaarGsZM="
            },
            {
                "path": "golang.org/x/exp",
                "version": "v0.0.0-20220722155223-a9213eeb770e",
                "sum": "h1:+WEEuIdZHnUeJJmEUjyYC2gfUMj69yZXw17EnHg/otA="
            },
            {
                "path": "golang.org/x/net",
                "version": "v0.0.0-20220726230323-06994584191e",
                "sum": "h1:wOQNKh1uuDGRnmgF0jDxh7ctgGy/3P4rYWQRVJD4/Yg="
            },
            {
                "path": "golang.org/x/sync",
                "version": "v0.0.0-20220722155255-886fb9371eb4",
                "sum": "h1:uVc8UZUe6tr40fFVnUP5Oj+veunVezqYl9z7DYw9xzw="
            },
            {
                "path": "golang.org/x/sys",
                "version": "v0.0.0-20220727055044-e65921a090b8",
                "sum": "h1:dyU22nBWzrmTQxtNrr4dzVOvaw35nUYE279vF9UmsI8="
            },
            {
                "path": "golang.org/x/term",
                "version": "v0.0.0-20220722155259-a9ba230a4035",
                "sum": "h1:Q5284mrmYTpACcm+eAKjKJH48BBwSyfJqmmGDTtT8Vc="
            },
            {
                "path": "golang.org/x/text",
                "version": "v0.3.7",
                "sum": "h1:olpwvP2KacW1ZWvsR7uQhoyTYvKAupfQrRGBFM352Gk="
            },
            {
                "path": "google.golang.org/genproto",
                "version": "v0.0.0-20220725144611-272f38e5d71b",
                "sum": "h1:SfSkJugek6xm7lWywqth4r2iTrYLpD8lOj1nMIIhMNM="
            },
            {
                "path": "google.golang.org/grpc",
                "version": "v1.48.0",
                "sum": ""
            },
            {
                "path": "google.golang.org/protobuf",
                "version": "v1.28.0",
                "sum": "h1:w43yiav+6bVFTBQFZX0r7ipe9JQ1QsbMgHwbBziscLw="
            },
            {
                "path": "gopkg.in/ini.v1",
                "version": "v1.66.6",
                "sum": "h1:LATuAqN/shcYAOkv3wl2L4rkaKqkcgTBQjOyYDvcPKI="
            },
            {
                "path": "gopkg.in/yaml.v2",
                "version": "v2.4.0",
                "sum": "h1:D8xgwECY7CYvx+Y2n4sBz93Jn9JRvxdiyyo8CTfuKaY="
            },
            {
                "path": "gopkg.in/yaml.v3",
                "version": "v3.0.1",
                "sum": "h1:fxVm/GzAzEWqLHuvctI91KS9hhNmmWOoWu0XTYJS7CA="
            },
            {
                "path": "nhooyr.io/websocket",
                "version": "v1.8.6",
                "sum": "h1:s+C3xAMLwGmlI31Nyn/eAehUlZPwfYZu2JXM621Q5/k="
            }
        ],
        "cosmos_sdk_version": "v0.45.9"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/base/tendermint/v1beta1/syncing</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries node syncing.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/base/tendermint/v1beta1/syncing
```
Response Example:
```json
{
    "syncing": false
}
```
</details>
<br>




<details>
    <summary><code>GET /cosmos/base/tendermint/v1beta1/validatorsets/latest</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries latest validator-set.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/base/tendermint/v1beta1/validatorsets/latest
```
Response Example:
```json
{
    "block_height": "3392",
    "validators": [
        {
            "address": "stvalcons1rzn3d8qmgf7ejsfn77eag5zwjfufmvmu7sn802",
            "pub_key": {
                "@type": "/cosmos.crypto.ed25519.PubKey",
                "key": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
            },
            "voting_power": "500000",
            "proposer_priority": "0"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>



<details>
    <summary><code>GET /cosmos/base/tendermint/v1beta1/validatorsets/{height}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries validator-set at a given height.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/base/tendermint/v1beta1/validatorsets/1000
```
Response Example:
```json
{
    "block_height": "1000",
    "validators": [
        {
            "address": "stvalcons1rzn3d8qmgf7ejsfn77eag5zwjfufmvmu7sn802",
            "pub_key": {
                "@type": "/cosmos.crypto.ed25519.PubKey",
                "key": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
            },
            "voting_power": "500000",
            "proposer_priority": "0"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

***

### Transactions
Search, encode, or broadcast transactions.

<details>
    <summary><code>GET /cosmos/tx/v1beta1/txs</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; fetches txs by event.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/tx/v1beta1/txs?events=tx.height=611
```
Response Example:
```json
{
    "txs": [
        {
            "body": {
                "messages": [
                    {
                        "@type": "/cosmos.bank.v1beta1.MsgSend",
                        "from_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
                        "to_address": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx",
                        "amount": [
                            {
                                "denom": "wei",
                                "amount": "1000000000"
                            }
                        ]
                    }
                ],
                "memo": "",
                "timeout_height": "0",
                "extension_options": [],
                "non_critical_extension_options": []
            },
            "auth_info": {
                "signer_infos": [
                    {
                        "public_key": {
                            "@type": "/stratos.crypto.v1.ethsecp256k1.PubKey",
                            "key": "Agkwb1xacHBqeqGBIqRacXgf0qKTnEBPCEtH2vTE01Ke"
                        },
                        "mode_info": {
                            "single": {
                                "mode": "SIGN_MODE_DIRECT"
                            }
                        },
                        "sequence": "3"
                    }
                ],
                "fee": {
                    "amount": [
                        {
                            "denom": "wei",
                            "amount": "200000000000000"
                        }
                    ],
                    "gas_limit": "200000",
                    "payer": "",
                    "granter": ""
                }
            },
            "signatures": [
                "7FmgB+sTnP5Kk4q121YyVdJJkdEq3Gioydu8fTP+pxoMC/Tl77uJlCRBanSP7jx1xEjwTxt3znGL9KNQLRAA2QA="
            ]
        }
    ],
    "tx_responses": [
        {
            "height": "611",
            "txhash": "AB0EF3761603145EDC1B4121C91B51001249186E1362E7148C82E7DB12F7BDF0",
            "codespace": "",
            "code": 0,
            "data": "0A1E0A1C2F636F736D6F732E62616E6B2E763162657461312E4D736753656E64",
            "raw_log": "[{\"events\":[{\"type\":\"coin_received\",\"attributes\":[{\"key\":\"receiver\",\"value\":\"st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx\"},{\"key\":\"amount\",\"value\":\"1000000000wei\"}]},{\"type\":\"coin_spent\",\"attributes\":[{\"key\":\"spender\",\"value\":\"st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh\"},{\"key\":\"amount\",\"value\":\"1000000000wei\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"/cosmos.bank.v1beta1.MsgSend\"},{\"key\":\"sender\",\"value\":\"st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx\"},{\"key\":\"sender\",\"value\":\"st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh\"},{\"key\":\"amount\",\"value\":\"1000000000wei\"}]}]}]",
            "logs": [
                {
                    "msg_index": 0,
                    "log": "",
                    "events": [
                        {
                            "type": "coin_received",
                            "attributes": [
                                {
                                    "key": "receiver",
                                    "value": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx"
                                },
                                {
                                    "key": "amount",
                                    "value": "1000000000wei"
                                }
                            ]
                        },
                        {
                            "type": "coin_spent",
                            "attributes": [
                                {
                                    "key": "spender",
                                    "value": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh"
                                },
                                {
                                    "key": "amount",
                                    "value": "1000000000wei"
                                }
                            ]
                        },
                        {
                            "type": "message",
                            "attributes": [
                                {
                                    "key": "action",
                                    "value": "/cosmos.bank.v1beta1.MsgSend"
                                },
                                {
                                    "key": "sender",
                                    "value": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh"
                                },
                                {
                                    "key": "module",
                                    "value": "bank"
                                }
                            ]
                        },
                        {
                            "type": "transfer",
                            "attributes": [
                                {
                                    "key": "recipient",
                                    "value": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx"
                                },
                                {
                                    "key": "sender",
                                    "value": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh"
                                },
                                {
                                    "key": "amount",
                                    "value": "1000000000wei"
                                }
                            ]
                        }
                    ]
                }
            ],
            "info": "",
            "gas_wanted": "200000",
            "gas_used": "88709",
            "tx": {
                "@type": "/cosmos.tx.v1beta1.Tx",
                "body": {
                    "messages": [
                        {
                            "@type": "/cosmos.bank.v1beta1.MsgSend",
                            "from_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
                            "to_address": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx",
                            "amount": [
                                {
                                    "denom": "wei",
                                    "amount": "1000000000"
                                }
                            ]
                        }
                    ],
                    "memo": "",
                    "timeout_height": "0",
                    "extension_options": [],
                    "non_critical_extension_options": []
                },
                "auth_info": {
                    "signer_infos": [
                        {
                            "public_key": {
                                "@type": "/stratos.crypto.v1.ethsecp256k1.PubKey",
                                "key": "Agkwb1xacHBqeqGBIqRacXgf0qKTnEBPCEtH2vTE01Ke"
                            },
                            "mode_info": {
                                "single": {
                                    "mode": "SIGN_MODE_DIRECT"
                                }
                            },
                            "sequence": "3"
                        }
                    ],
                    "fee": {
                        "amount": [
                            {
                                "denom": "wei",
                                "amount": "200000000000000"
                            }
                        ],
                        "gas_limit": "200000",
                        "payer": "",
                        "granter": ""
                    }
                },
                "signatures": [
                    "7FmgB+sTnP5Kk4q121YyVdJJkdEq3Gioydu8fTP+pxoMC/Tl77uJlCRBanSP7jx1xEjwTxt3znGL9KNQLRAA2QA="
                ]
            },
            "timestamp": "2023-01-11T01:20:11Z",
            "events": [
                {
                    "type": "coin_spent",
                    "attributes": [
                        {
                            "key": "c3BlbmRlcg==",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                            "index": true
                        },
                        {
                            "key": "YW1vdW50",
                            "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "coin_received",
                    "attributes": [
                        {
                            "key": "cmVjZWl2ZXI=",
                            "value": "c3QxN3hwZnZha20yYW1nOTYyeWxzNmY4NHoza2VsbDhjNWx2NWhqMnE=",
                            "index": true
                        },
                        {
                            "key": "YW1vdW50",
                            "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "transfer",
                    "attributes": [
                        {
                            "key": "cmVjaXBpZW50",
                            "value": "c3QxN3hwZnZha20yYW1nOTYyeWxzNmY4NHoza2VsbDhjNWx2NWhqMnE=",
                            "index": true
                        },
                        {
                            "key": "c2VuZGVy",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                            "index": true
                        },
                        {
                            "key": "YW1vdW50",
                            "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "message",
                    "attributes": [
                        {
                            "key": "c2VuZGVy",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "tx",
                    "attributes": [
                        {
                            "key": "ZmVl",
                            "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                            "index": true
                        },
                        {
                            "key": "ZmVlX3BheWVy",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "tx",
                    "attributes": [
                        {
                            "key": "YWNjX3NlcQ==",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamgvMw==",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "tx",
                    "attributes": [
                        {
                            "key": "c2lnbmF0dXJl",
                            "value": "N0ZtZ0Irc1RuUDVLazRxMTIxWXlWZEpKa2RFcTNHaW95ZHU4ZlRQK3B4b01DL1RsNzd1SmxDUkJhblNQN2p4MXhFandUeHQzem5HTDlLTlFMUkFBMlFBPQ==",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "message",
                    "attributes": [
                        {
                            "key": "YWN0aW9u",
                            "value": "L2Nvc21vcy5iYW5rLnYxYmV0YTEuTXNnU2VuZA==",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "coin_spent",
                    "attributes": [
                        {
                            "key": "c3BlbmRlcg==",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                            "index": true
                        },
                        {
                            "key": "YW1vdW50",
                            "value": "MTAwMDAwMDAwMHdlaQ==",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "coin_received",
                    "attributes": [
                        {
                            "key": "cmVjZWl2ZXI=",
                            "value": "c3Qxc3F6c2s4bXBsdjUyNDhneDZkZGR6enh3ZXF2ZXc4cnRzdDk2Zng=",
                            "index": true
                        },
                        {
                            "key": "YW1vdW50",
                            "value": "MTAwMDAwMDAwMHdlaQ==",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "transfer",
                    "attributes": [
                        {
                            "key": "cmVjaXBpZW50",
                            "value": "c3Qxc3F6c2s4bXBsdjUyNDhneDZkZGR6enh3ZXF2ZXc4cnRzdDk2Zng=",
                            "index": true
                        },
                        {
                            "key": "c2VuZGVy",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                            "index": true
                        },
                        {
                            "key": "YW1vdW50",
                            "value": "MTAwMDAwMDAwMHdlaQ==",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "message",
                    "attributes": [
                        {
                            "key": "c2VuZGVy",
                            "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                            "index": true
                        }
                    ]
                },
                {
                    "type": "message",
                    "attributes": [
                        {
                            "key": "bW9kdWxl",
                            "value": "YmFuaw==",
                            "index": true
                        }
                    ]
                }
            ]
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```
</details>
<br>

<details>
    <summary><code>GET /cosmos/tx/v1beta1/txs/{hash}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; fetches a tx by hash.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/cosmos/tx/v1beta1/txs/AB0EF3761603145EDC1B4121C91B51001249186E1362E7148C82E7DB12F7BDF0
```
Response Example:
```json
{
    "tx": {
        "body": {
            "messages": [
                {
                    "@type": "/cosmos.bank.v1beta1.MsgSend",
                    "from_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
                    "to_address": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx",
                    "amount": [
                        {
                            "denom": "wei",
                            "amount": "1000000000"
                        }
                    ]
                }
            ],
            "memo": "",
            "timeout_height": "0",
            "extension_options": [],
            "non_critical_extension_options": []
        },
        "auth_info": {
            "signer_infos": [
                {
                    "public_key": {
                        "@type": "/stratos.crypto.v1.ethsecp256k1.PubKey",
                        "key": "Agkwb1xacHBqeqGBIqRacXgf0qKTnEBPCEtH2vTE01Ke"
                    },
                    "mode_info": {
                        "single": {
                            "mode": "SIGN_MODE_DIRECT"
                        }
                    },
                    "sequence": "3"
                }
            ],
            "fee": {
                "amount": [
                    {
                        "denom": "wei",
                        "amount": "200000000000000"
                    }
                ],
                "gas_limit": "200000",
                "payer": "",
                "granter": ""
            }
        },
        "signatures": [
            "7FmgB+sTnP5Kk4q121YyVdJJkdEq3Gioydu8fTP+pxoMC/Tl77uJlCRBanSP7jx1xEjwTxt3znGL9KNQLRAA2QA="
        ]
    },
    "tx_response": {
        "height": "611",
        "txhash": "AB0EF3761603145EDC1B4121C91B51001249186E1362E7148C82E7DB12F7BDF0",
        "codespace": "",
        "code": 0,
        "data": "0A1E0A1C2F636F736D6F732E62616E6B2E763162657461312E4D736753656E64",
        "raw_log": "[{\"events\":[{\"type\":\"coin_received\",\"attributes\":[{\"key\":\"receiver\",\"value\":\"st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx\"},{\"key\":\"amount\",\"value\":\"1000000000wei\"}]},{\"type\":\"coin_spent\",\"attributes\":[{\"key\":\"spender\",\"value\":\"st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh\"},{\"key\":\"amount\",\"value\":\"1000000000wei\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"/cosmos.bank.v1beta1.MsgSend\"},{\"key\":\"sender\",\"value\":\"st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx\"},{\"key\":\"sender\",\"value\":\"st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh\"},{\"key\":\"amount\",\"value\":\"1000000000wei\"}]}]}]",
        "logs": [
            {
                "msg_index": 0,
                "log": "",
                "events": [
                    {
                        "type": "coin_received",
                        "attributes": [
                            {
                                "key": "receiver",
                                "value": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx"
                            },
                            {
                                "key": "amount",
                                "value": "1000000000wei"
                            }
                        ]
                    },
                    {
                        "type": "coin_spent",
                        "attributes": [
                            {
                                "key": "spender",
                                "value": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh"
                            },
                            {
                                "key": "amount",
                                "value": "1000000000wei"
                            }
                        ]
                    },
                    {
                        "type": "message",
                        "attributes": [
                            {
                                "key": "action",
                                "value": "/cosmos.bank.v1beta1.MsgSend"
                            },
                            {
                                "key": "sender",
                                "value": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh"
                            },
                            {
                                "key": "module",
                                "value": "bank"
                            }
                        ]
                    },
                    {
                        "type": "transfer",
                        "attributes": [
                            {
                                "key": "recipient",
                                "value": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx"
                            },
                            {
                                "key": "sender",
                                "value": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh"
                            },
                            {
                                "key": "amount",
                                "value": "1000000000wei"
                            }
                        ]
                    }
                ]
            }
        ],
        "info": "",
        "gas_wanted": "200000",
        "gas_used": "88709",
        "tx": {
            "@type": "/cosmos.tx.v1beta1.Tx",
            "body": {
                "messages": [
                    {
                        "@type": "/cosmos.bank.v1beta1.MsgSend",
                        "from_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
                        "to_address": "st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx",
                        "amount": [
                            {
                                "denom": "wei",
                                "amount": "1000000000"
                            }
                        ]
                    }
                ],
                "memo": "",
                "timeout_height": "0",
                "extension_options": [],
                "non_critical_extension_options": []
            },
            "auth_info": {
                "signer_infos": [
                    {
                        "public_key": {
                            "@type": "/stratos.crypto.v1.ethsecp256k1.PubKey",
                            "key": "Agkwb1xacHBqeqGBIqRacXgf0qKTnEBPCEtH2vTE01Ke"
                        },
                        "mode_info": {
                            "single": {
                                "mode": "SIGN_MODE_DIRECT"
                            }
                        },
                        "sequence": "3"
                    }
                ],
                "fee": {
                    "amount": [
                        {
                            "denom": "wei",
                            "amount": "200000000000000"
                        }
                    ],
                    "gas_limit": "200000",
                    "payer": "",
                    "granter": ""
                }
            },
            "signatures": [
                "7FmgB+sTnP5Kk4q121YyVdJJkdEq3Gioydu8fTP+pxoMC/Tl77uJlCRBanSP7jx1xEjwTxt3znGL9KNQLRAA2QA="
            ]
        },
        "timestamp": "2023-01-11T01:20:11Z",
        "events": [
            {
                "type": "coin_spent",
                "attributes": [
                    {
                        "key": "c3BlbmRlcg==",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                        "index": true
                    },
                    {
                        "key": "YW1vdW50",
                        "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                        "index": true
                    }
                ]
            },
            {
                "type": "coin_received",
                "attributes": [
                    {
                        "key": "cmVjZWl2ZXI=",
                        "value": "c3QxN3hwZnZha20yYW1nOTYyeWxzNmY4NHoza2VsbDhjNWx2NWhqMnE=",
                        "index": true
                    },
                    {
                        "key": "YW1vdW50",
                        "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                        "index": true
                    }
                ]
            },
            {
                "type": "transfer",
                "attributes": [
                    {
                        "key": "cmVjaXBpZW50",
                        "value": "c3QxN3hwZnZha20yYW1nOTYyeWxzNmY4NHoza2VsbDhjNWx2NWhqMnE=",
                        "index": true
                    },
                    {
                        "key": "c2VuZGVy",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                        "index": true
                    },
                    {
                        "key": "YW1vdW50",
                        "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                        "index": true
                    }
                ]
            },
            {
                "type": "message",
                "attributes": [
                    {
                        "key": "c2VuZGVy",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                        "index": true
                    }
                ]
            },
            {
                "type": "tx",
                "attributes": [
                    {
                        "key": "ZmVl",
                        "value": "MjAwMDAwMDAwMDAwMDAwd2Vp",
                        "index": true
                    },
                    {
                        "key": "ZmVlX3BheWVy",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                        "index": true
                    }
                ]
            },
            {
                "type": "tx",
                "attributes": [
                    {
                        "key": "YWNjX3NlcQ==",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamgvMw==",
                        "index": true
                    }
                ]
            },
            {
                "type": "tx",
                "attributes": [
                    {
                        "key": "c2lnbmF0dXJl",
                        "value": "N0ZtZ0Irc1RuUDVLazRxMTIxWXlWZEpKa2RFcTNHaW95ZHU4ZlRQK3B4b01DL1RsNzd1SmxDUkJhblNQN2p4MXhFandUeHQzem5HTDlLTlFMUkFBMlFBPQ==",
                        "index": true
                    }
                ]
            },
            {
                "type": "message",
                "attributes": [
                    {
                        "key": "YWN0aW9u",
                        "value": "L2Nvc21vcy5iYW5rLnYxYmV0YTEuTXNnU2VuZA==",
                        "index": true
                    }
                ]
            },
            {
                "type": "coin_spent",
                "attributes": [
                    {
                        "key": "c3BlbmRlcg==",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                        "index": true
                    },
                    {
                        "key": "YW1vdW50",
                        "value": "MTAwMDAwMDAwMHdlaQ==",
                        "index": true
                    }
                ]
            },
            {
                "type": "coin_received",
                "attributes": [
                    {
                        "key": "cmVjZWl2ZXI=",
                        "value": "c3Qxc3F6c2s4bXBsdjUyNDhneDZkZGR6enh3ZXF2ZXc4cnRzdDk2Zng=",
                        "index": true
                    },
                    {
                        "key": "YW1vdW50",
                        "value": "MTAwMDAwMDAwMHdlaQ==",
                        "index": true
                    }
                ]
            },
            {
                "type": "transfer",
                "attributes": [
                    {
                        "key": "cmVjaXBpZW50",
                        "value": "c3Qxc3F6c2s4bXBsdjUyNDhneDZkZGR6enh3ZXF2ZXc4cnRzdDk2Zng=",
                        "index": true
                    },
                    {
                        "key": "c2VuZGVy",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                        "index": true
                    },
                    {
                        "key": "YW1vdW50",
                        "value": "MTAwMDAwMDAwMHdlaQ==",
                        "index": true
                    }
                ]
            },
            {
                "type": "message",
                "attributes": [
                    {
                        "key": "c2VuZGVy",
                        "value": "c3QxcHZ5anpsaHdycGdrbHUwMDQ0YXQ0dDZxaDdtMjNrM2tyMmdzamg=",
                        "index": true
                    }
                ]
            },
            {
                "type": "message",
                "attributes": [
                    {
                        "key": "bW9kdWxl",
                        "value": "YmFuaw==",
                        "index": true
                    }
                ]
            }
        ]
    }
}
```
</details>
<br>



***


### Register

<details>
    <summary><code> GET /register/staking</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries total staking state of all registered resource nodes and meta nodes</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/register/staking
```
Response Example:
```json
{
    "height": "3573",
    "result": {
        "resource_nodes_total_stake": {
            "denom": "wei",
            "amount": "0"
        },
        "meta_nodes_total_stake": {
            "denom": "wei",
            "amount": "100000000000000000000"
        },
        "total_bonded_stake": {
            "denom": "wei",
            "amount": "100000000000000000000"
        },
        "total_unbonded_stake": {
            "denom": "wei",
            "amount": "0"
        },
        "total_unbonding_stake": {
            "denom": "wei",
            "amount": "0"
        }
    }
}
```
</details>
<br>

<details>
    <summary><code> GET /register/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries params of registered module</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/register/params
```
Response Example:
```json
{
    "height": "3588",
    "result": {
        "bond_denom": "wei",
        "unbonding_threashold_time": "15552000000000000",
        "unbonding_completion_time": "1209600000000000",
        "max_entries": 16,
        "resource_node_reg_enabled": true
    }
}
```
</details>
<br>

<details>
    <summary><code> GET /register/resource-node/{nodeAddress} </code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries info of a registered resource node</summary>


Request Example:
```http
https://rest-tropos.thestratos.org/register/resource-node/stsds1gl9ywg6jdfdgcja70ffum4ectq4fmt26ay4znv
```
Response Example:
```json
{
    "height": "3712",
    "result": {
        "network_address": "stsds1gl9ywg6jdfdgcja70ffum4ectq4fmt26ay4znv",
        "pubkey": {
            "type": "tendermint/PubKeyEd25519",
            "value": "2OAeLO0+KrBkSxuFKU1ofJqGb4RtA8GpD8XCZlMYw2A="
        },
        "suspend": true,
        "status": 3,
        "tokens": "10000000000000000000",
        "owner_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
        "description": {
            "moniker": "resource-node0",
            "identity": "",
            "website": "",
            "security_contact": "",
            "details": ""
        },
        "creation_time": "2023-01-11T17:26:06.410263787Z",
        "node_type": 1
    }
}
```
</details>
<br>

<details>
    <summary><code> GET /register/meta-node/{nodeAddress}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; returns info of a registered meta node</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/register/meta-node/stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64
```
Response Example:
```json
{
    "height": "3731",
    "result": {
        "network_address": "stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64",
        "pubkey": {
            "type": "tendermint/PubKeyEd25519",
            "value": "ltODy8zL5IjJwCutlIexqlBb3GH0+aHZOrpT7f/aKnQ="
        },
        "suspend": false,
        "status": 3,
        "tokens": "100000000000000000000",
        "owner_address": "st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x",
        "description": {
            "moniker": "snode://stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64@127.0.0.1:8888",
            "identity": "",
            "website": "",
            "security_contact": "",
            "details": ""
        },
        "creation_time": "1970-01-01T00:00:00Z"
    }
}
```
</details>
<br>

<details>
    <summary><code> GET /register/staking/address/{nodeAddress}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries staking info of a specific node</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/register/staking/address/stsds1gl9ywg6jdfdgcja70ffum4ectq4fmt26ay4znv
```
Response Example:
```json
{
    "height": "3749",
    "result": {
        "network_address": "stsds1gl9ywg6jdfdgcja70ffum4ectq4fmt26ay4znv",
        "pubkey": {
            "type": "tendermint/PubKeyEd25519",
            "value": "2OAeLO0+KrBkSxuFKU1ofJqGb4RtA8GpD8XCZlMYw2A="
        },
        "suspend": true,
        "status": 3,
        "tokens": "10000000000000000000",
        "owner_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
        "description": {
            "moniker": "resource-node0",
            "identity": "",
            "website": "",
            "security_contact": "",
            "details": ""
        },
        "creation_time": "2023-01-11T17:26:06.410263787Z",
        "node_type": 1,
        "bonded_stake": {
            "denom": "wei",
            "amount": "10000000000000000000"
        },
        "un_bonding_stake": {
            "denom": "wei",
            "amount": "0"
        },
        "un_bonded_stake": {
            "denom": "wei",
            "amount": "0"
        }
    }
}
```
</details>
<br>

<details>
    <summary><code> GET /register/staking/owner/{ownerAddress}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all staking info of a specific owner</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/register/staking/owner/st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh
```
Response Example:
```json
{
    "height": "3765",
    "result": [
        {
            "network_address": "stsds1gl9ywg6jdfdgcja70ffum4ectq4fmt26ay4znv",
            "pubkey": {
                "type": "tendermint/PubKeyEd25519",
                "value": "2OAeLO0+KrBkSxuFKU1ofJqGb4RtA8GpD8XCZlMYw2A="
            },
            "suspend": true,
            "status": 3,
            "tokens": "10000000000000000000",
            "owner_address": "st1pvyjzlhwrpgklu0044at4t6qh7m23k3kr2gsjh",
            "description": {
                "moniker": "resource-node0",
                "identity": "",
                "website": "",
                "security_contact": "",
                "details": ""
            },
            "creation_time": "2023-01-11T17:26:06.410263787Z",
            "node_type": 1,
            "bonded_stake": {
                "denom": "wei",
                "amount": "10000000000000000000"
            },
            "un_bonding_stake": {
                "denom": "wei",
                "amount": "0"
            },
            "un_bonded_stake": {
                "denom": "wei",
                "amount": "0"
            }
        }
    ]
}
```
</details>
<br>


<details>
    <summary><code> GET /register/resource-count</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries total number of bonded resource nodes</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/register/resource-count
```
Response Example:
```json
{
    "height": "1093",
    "result": "2"
}
```
</details>
<br>


<details>
    <summary><code> GET /register/meta-count</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries total number of bonded meta nodes</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/register/meta-count
```
Response Example:
```json
{
    "height": "1118",
    "result": "4"
}
```
</details>
<br>

***

### Proof of Traffic (PoT)

<details>
    <summary><code> GET /pot/report/epoch/{epoch}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries Pot volume report info at a specific epoch</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/report/epoch/1
```
Response Example:
```json
{
    "height": "1358",
    "result": {
        "reporter": "stsds13yakj2xgzzdfcw7kd5gtfcfv2k6sn5eg4vdfem",
        "report_reference": " report for epoch 1",
        "tx_hash": "CF404452FF47EDCA99787FD5E79355D7A3BDB0E65E6FE3A54F4914F0E9EE0DF6"
    }
}
```
</details>
<br>

<!--  
<details>
    <summary><code> GET /pot/rewards/epoch/{epoch}?wallet_address={wallet_address}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries  Pot rewards info of a wallet_address at a specific epoch</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/rewards/epoch/3?wallet_address=st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw
```
Response Example:
```json
{
    "height": "2613",
    "result": [
        {
            "wallet_address": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw",
            "reward_from_mining_pool": [
                {
                    "denom": "utros",
                    "amount": "48000000710"
                }
            ],
            "reward_from_traffic_pool": [
                {
                    "denom": "ustos",
                    "amount": "300"
                }
            ]
        }
    ]
}
```
</details>
<br>

-->

<details>
    <summary><code> GET /pot/rewards/wallet/{walletAddress}[?height={BlockHeight}, optional]</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries owner's Pot rewards info at a specific height</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/rewards/wallet/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg/3877
```
Response Example:
```json
{
    "height": "3877",
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
</details>
<br>



<details>
    <summary><code> GET /pot/rewards/wallet/{walletAddress}[?epoch={epoch}, optional]</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries owner's Pot rewards info at a specific epoch</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/rewards/wallet/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg?epoch=50
```
Response Example:
```json
{
    "height": "4471",
    "result": [
        {
            "wallet_address": "st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg",
            "reward_from_mining_pool": [
                {
                    "denom": "utros",
                    "amount": "48000000710"
                }
            ],
            "reward_from_traffic_pool": [
                {
                    "denom": "wei",
                    "amount": "2918"
                }
            ]
        }
    ]
}
```
</details>
<br>

<details>
    <summary><code> GET /pot/rewards/wallet/{walletAddress}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries latest owner's Pot rewards info</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/rewards/wallet/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
```
Response Example:
```json
{
    "height": "3877",
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
</details>
<br>

<details>
    <summary><code> GET /pot/rewards/epoch/{epoch}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries all rewards info at a specific epoch</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/rewards/epoch/1?page=1
```
Response Example:
```json
{
    "height": "4404",
    "result": [
        {
            "wallet_address": "st19cpsaw2q2dcmc5s7vu09xntqacakfcfdw8dg8m",
            "reward_from_mining_pool": [
                {
                    "denom": "utros",
                    "amount": "2003083802"
                }
            ],
            "reward_from_traffic_pool": [
                {
                    "denom": "wei",
                    "amount": "3863162732376"
                }
            ]
        },
        {
            "wallet_address": "st1xlhwxzexp4fjnf2xt8zsd6jh7y3ap0sgddaf4r",
            "reward_from_mining_pool": [
                {
                    "denom": "utros",
                    "amount": "2003083802"
                }
            ],
            "reward_from_traffic_pool": [
                {
                    "denom": "wei",
                    "amount": "3863162732376"
                }
            ]
        }
    ]
}
```
</details>
<br>


<details>
    <summary><code>GET /pot/slashing/{walletAddress} [?height={BlockHeight}, optional]</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries owner's Pot slashing info at a specific height</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/slashing/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg?height=3877
```
Response Example:
```json
{
    "height": "3877",
    "result": 0
}
```
</details>
<br>

<details>
    <summary><code>GET /pot/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Query params of POT module</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/pot/params
```
Response Example:
```json
{
    "height": "3997",
    "result": {
        "bond_denom": "wei",
        "reward_denom": "utros",
        "mature_epoch": "2016",
        "mining_reward_params": [
            {
                "total_mined_valve_start": {
                    "denom": "utros",
                    "amount": "0"
                },
                "total_mined_valve_end": {
                    "denom": "utros",
                    "amount": "16819200000000000"
                },
                "mining_reward": {
                    "denom": "utros",
                    "amount": "80000000000"
                },
                "block_chain_percentage_in_ten_thousand": "2000",
                "resource_node_percentage_in_ten_thousand": "6000",
                "meta_node_percentage_in_ten_thousand": "2000"
            },
            {
                "total_mined_valve_start": {
                    "denom": "utros",
                    "amount": "16819200000000000"
                },
                "total_mined_valve_end": {
                    "denom": "utros",
                    "amount": "25228800000000000"
                },
                "mining_reward": {
                    "denom": "utros",
                    "amount": "40000000000"
                },
                "block_chain_percentage_in_ten_thousand": "2000",
                "resource_node_percentage_in_ten_thousand": "6200",
                "meta_node_percentage_in_ten_thousand": "1800"
            },
            {
                "total_mined_valve_start": {
                    "denom": "utros",
                    "amount": "25228800000000000"
                },
                "total_mined_valve_end": {
                    "denom": "utros",
                    "amount": "29433600000000000"
                },
                "mining_reward": {
                    "denom": "utros",
                    "amount": "20000000000"
                },
                "block_chain_percentage_in_ten_thousand": "2000",
                "resource_node_percentage_in_ten_thousand": "6400",
                "meta_node_percentage_in_ten_thousand": "1600"
            },
            {
                "total_mined_valve_start": {
                    "denom": "utros",
                    "amount": "29433600000000000"
                },
                "total_mined_valve_end": {
                    "denom": "utros",
                    "amount": "31536000000000000"
                },
                "mining_reward": {
                    "denom": "utros",
                    "amount": "10000000000"
                },
                "block_chain_percentage_in_ten_thousand": "2000",
                "resource_node_percentage_in_ten_thousand": "6600",
                "meta_node_percentage_in_ten_thousand": "1400"
            },
            {
                "total_mined_valve_start": {
                    "denom": "utros",
                    "amount": "31536000000000000"
                },
                "total_mined_valve_end": {
                    "denom": "utros",
                    "amount": "32587200000000000"
                },
                "mining_reward": {
                    "denom": "utros",
                    "amount": "5000000000"
                },
                "block_chain_percentage_in_ten_thousand": "2000",
                "resource_node_percentage_in_ten_thousand": "6800",
                "meta_node_percentage_in_ten_thousand": "1200"
            },
            {
                "total_mined_valve_start": {
                    "denom": "utros",
                    "amount": "32587200000000000"
                },
                "total_mined_valve_end": {
                    "denom": "utros",
                    "amount": "40000000000000000"
                },
                "mining_reward": {
                    "denom": "utros",
                    "amount": "2500000000"
                },
                "block_chain_percentage_in_ten_thousand": "2000",
                "resource_node_percentage_in_ten_thousand": "7000",
                "meta_node_percentage_in_ten_thousand": "1000"
            }
        ],
        "community_tax": "0.020000000000000000"
    }
}
```
</details>
<br>

***

### SDS

<details>
    <summary><code> GET /sds/simulatePrepay/{amtToPrepay}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries a simulated prepay result </summary>

Request Example:
```http
https://rest-tropos.thestratos.org/sds/simulatePrepay/8000000000
```
Response Example:
```json
{
    "height": "4036",
    "result": "8799"
}
```
</details>
<br>

<details>
    <summary><code> GET /sds/nozPrice</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries current nozPrice </summary>

Request Example:
```http
https://rest-tropos.thestratos.org/sds/nozPrice
```
Response Example:
```json
{
    "height": "4088",
    "result": "909090.909090909090909091"
}
```
</details>
<br>

<details>
    <summary><code> GET /sds/nozSupply</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries current nozSupply </summary>

Request Example:
```http
https://rest-tropos.thestratos.org/sds/nozSupply
```
Response Example:
```json
{
    "height": "4078",
    "result": {
        "Remaining": "110000000000000",
        "Total": "110000000000000"
    }
}
```
</details>
<br>

<details>
    <summary><code> GET /sds/params</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; queries params of SDS module </summary>

Request Example:
```http
https://rest-tropos.thestratos.org/sds/params
```
Response Example:
```json
{
    "height": "4055",
    "result": {
        "bond_denom": "wei"
    }
}
```
</details>
<br>

***



<!--

## Transactions
Search, encode, or broadcast transactions.

<details>
    <summary><code>GET /txs/{hash}</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Retrieve a transaction using its hash.</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/txs/0CA946EBB823903004056BEA3CFAFE4F184EF616D72F38433763006534AA0E2E
```
Response Example:
```json
{
  "height": "18",
  "txhash": "0CA946EBB823903004056BEA3CFAFE4F184EF616D72F38433763006534AA0E2E",
  "raw_log": "[{\"msg_index\":0,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"send\"},{\"key\":\"sender\",\"value\":\"st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"st1rw6xln8xaa532key7dmcznwlv62lvs54h96h6h\"},{\"key\":\"sender\",\"value\":\"st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s\"},{\"key\":\"amount\",\"value\":\"1000000ustos\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "log": "",
      "events": [
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "send"
            },
            {
              "key": "sender",
              "value": "st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s"
            },
            {
              "key": "module",
              "value": "bank"
            }
          ]
        },
        {
          "type": "transfer",
          "attributes": [
            {
              "key": "recipient",
              "value": "st1rw6xln8xaa532key7dmcznwlv62lvs54h96h6h"
            },
            {
              "key": "sender",
              "value": "st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s"
            },
            {
              "key": "amount",
              "value": "1000000ustos"
            }
          ]
        }
      ]
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "65927",
  "tx": {
    "type": "cosmos-sdk/StdTx",
    "value": {
      "msg": [
        {
          "type": "cosmos-sdk/MsgSend",
          "value": {
            "from_address": "st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s",
            "to_address": "st1rw6xln8xaa532key7dmcznwlv62lvs54h96h6h",
            "amount": [
              {
                "denom": "ustos",
                "amount": "1000000"
              }
            ]
          }
        }
      ],
      "fee": {
        "amount": [
          {
            "denom": "ustos",
            "amount": "100"
          }
        ],
        "gas": "200000"
      },
      "signatures": [
        {
          "pub_key": {
            "type": "tendermint/PubKeySecp256k1",
            "value": "A4oRGeSYDImfg5OhXPPOQ1p0Sepc5PkxhCV3sDe5uNao"
          },
          "signature": "0NOxqgG7s7v54lnwkRJfDGV2SZrWhmE7M2A2lQs4T2le3WOnYrdF8LXrVGR06uwhBRNcwQE4kZxxHeYbZjUXhQ=="
        }
      ],
      "memo": ""
    }
  },
  "timestamp": "2021-08-05T03:16:39Z"
}
```
</details>
<br>

<details>
    <summary><code>GET /txs</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Search transactions.</summary>

parameters

> * `message.action`: transaction events such as 'message.action=send' which results in the following endpoint: 'GET /txs?message.action=send'.
> * `message.sender`: transaction tags with sender: 'GET /txs?message.action=send&message.sender=st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s'
> * `page`: page number
> * `limit`: maximum number of items per page
> * `tx.minheight`: transactions on blocks with height greater or equal this value
> * `tx.maxheight`: transactions on blocks with height less than or equal this value

Request Example:
```http
https://rest-tropos.thestratos.org/txs?message.action=send&message.sender=st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s
```
Response Example:
```json
{
  "total_count": "1",
  "count": "1",
  "page_number": "1",
  "page_total": "1",
  "limit": "30",
  "txs": [
    {
      "height": "18",
      "txhash": "0CA946EBB823903004056BEA3CFAFE4F184EF616D72F38433763006534AA0E2E",
      "raw_log": "[{\"msg_index\":0,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"send\"},{\"key\":\"sender\",\"value\":\"st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"st1rw6xln8xaa532key7dmcznwlv62lvs54h96h6h\"},{\"key\":\"sender\",\"value\":\"st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s\"},{\"key\":\"amount\",\"value\":\"1000000ustos\"}]}]}]",
      "logs": [
        {
          "msg_index": 0,
          "log": "",
          "events": [
            {
              "type": "message",
              "attributes": [
                {
                  "key": "action",
                  "value": "send"
                },
                {
                  "key": "sender",
                  "value": "st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s"
                },
                {
                  "key": "module",
                  "value": "bank"
                }
              ]
            },
            {
              "type": "transfer",
              "attributes": [
                {
                  "key": "recipient",
                  "value": "st1rw6xln8xaa532key7dmcznwlv62lvs54h96h6h"
                },
                {
                  "key": "sender",
                  "value": "st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s"
                },
                {
                  "key": "amount",
                  "value": "1000000ustos"
                }
              ]
            }
          ]
        }
      ],
      "gas_wanted": "200000",
      "gas_used": "65927",
      "tx": {
        "type": "cosmos-sdk/StdTx",
        "value": {
          "msg": [
            {
              "type": "cosmos-sdk/MsgSend",
              "value": {
                "from_address": "st1l76s0ukw0r77fydhqtqpexax8m64mzaq04830s",
                "to_address": "st1rw6xln8xaa532key7dmcznwlv62lvs54h96h6h",
                "amount": [
                  {
                    "denom": "ustos",
                    "amount": "1000000"
                  }
                ]
              }
            }
          ],
          "fee": {
            "amount": [
              {
                "denom": "ustos",
                "amount": "100"
              }
            ],
            "gas": "200000"
          },
          "signatures": [
            {
              "pub_key": {
                "type": "tendermint/PubKeySecp256k1",
                "value": "A4oRGeSYDImfg5OhXPPOQ1p0Sepc5PkxhCV3sDe5uNao"
              },
              "signature": "0NOxqgG7s7v54lnwkRJfDGV2SZrWhmE7M2A2lQs4T2le3WOnYrdF8LXrVGR06uwhBRNcwQE4kZxxHeYbZjUXhQ=="
            }
          ],
          "memo": ""
        }
      },
      "timestamp": "2021-08-05T03:16:39Z"
    }
  ]
}
```
</details>
<br>



<details>
    <summary><code>POST /txs</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Broadcast a signed tx to a full node</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/txs
```
The tx must be a signed StdTx. The supported broadcast modes include "block"(return after tx commit), "sync"(return afer CheckTx) and "async"(return right away).
Request Body:
```json
{
  "tx": {
    "msg": [
      {
        "type": "cosmos-sdk/MsgSend",
        "value": {
          "from_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
          "to_address": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6",
          "amount": [
            {
              "denom": "ustos",
              "amount": "2000000"
            }
          ]
        }
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "ustos",
          "amount": "100"
        }
      ],
      "gas": "200000"
    },
    "signatures": [
      {
        "pub_key": {
          "type": "tendermint/PubKeySecp256k1",
          "value": "AolrbtnyTqnxmIjQJTmQfo/Gb2LlN9XPO/Qb2tSI/eRh"
        },
        "signature": "THrgfsKFIVlZvwzI7rHh3nRdC2VXJhaPDMyolZEsWklDmkxI7ecEA4bQgmkgXDpS7suKGvApsUIxeG4Um0vzWw=="
      }
    ],
    "memo": "Send Tx Example"
  },
  "mode": "block"
}
```
Response Example:
```json
{
    "height": "3495",
    "txhash": "3F96F021622ED95820D425522319FBE9C4510200FB18A2E18D75A209ABF05323",
    "raw_log": "[{\"msg_index\":0,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"send\"},{\"key\":\"sender\",\"value\":\"st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6\"},{\"key\":\"sender\",\"value\":\"st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2\"},{\"key\":\"amount\",\"value\":\"2000000ustos\"}]}]}]",
    "logs": [
        {
            "msg_index": 0,
            "log": "",
            "events": [
                {
                    "type": "message",
                    "attributes": [
                        {
                            "key": "action",
                            "value": "send"
                        },
                        {
                            "key": "sender",
                            "value": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2"
                        },
                        {
                            "key": "module",
                            "value": "bank"
                        }
                    ]
                },
                {
                    "type": "transfer",
                    "attributes": [
                        {
                            "key": "recipient",
                            "value": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6"
                        },
                        {
                            "key": "sender",
                            "value": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2"
                        },
                        {
                            "key": "amount",
                            "value": "2000000ustos"
                        }
                    ]
                }
            ]
        }
    ],
    "gas_wanted": "200000",
    "gas_used": "63379"
}
```
> * The tx must be a signed StdTx. The supported broadcast modes include "block"(return after tx commit), "sync"(return after CheckTx) and "async"(return right away).
> * Block log
> ```shell
> I[2021-08-08|14:53:19.215] Executed block                               module=state height=3495 validTxs=1 invalidTxs=0
> I[2021-08-08|14:53:19.220] Committed state                              module=state height=3495 txs=1 appHash=FA3D11545126DED06F31890C4B0E83B985AEB54DE7FC2
> ```

Check this Tx
```http
https://rest-tropos.thestratos.org/txs/3F96F021622ED95820D425522319FBE9C4510200FB18A2E18D75A209ABF05323
```
Response
```json
{
    "height": "3495",
    "txhash": "3F96F021622ED95820D425522319FBE9C4510200FB18A2E18D75A209ABF05323",
    "raw_log": "[{\"msg_index\":0,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"send\"},{\"key\":\"sender\",\"value\":\"st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6\"},{\"key\":\"sender\",\"value\":\"st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2\"},{\"key\":\"amount\",\"value\":\"2000000ustos\"}]}]}]",
    "logs": [
        {
            "msg_index": 0,
            "log": "",
            "events": [
                {
                    "type": "message",
                    "attributes": [
                        {
                            "key": "action",
                            "value": "send"
                        },
                        {
                            "key": "sender",
                            "value": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2"
                        },
                        {
                            "key": "module",
                            "value": "bank"
                        }
                    ]
                },
                {
                    "type": "transfer",
                    "attributes": [
                        {
                            "key": "recipient",
                            "value": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6"
                        },
                        {
                            "key": "sender",
                            "value": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2"
                        },
                        {
                            "key": "amount",
                            "value": "2000000ustos"
                        }
                    ]
                }
            ]
        }
    ],
    "gas_wanted": "200000",
    "gas_used": "63379",
    "tx": {
        "type": "cosmos-sdk/StdTx",
        "value": {
            "msg": [
                {
                    "type": "cosmos-sdk/MsgSend",
                    "value": {
                        "from_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
                        "to_address": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6",
                        "amount": [
                            {
                                "denom": "ustos",
                                "amount": "2000000"
                            }
                        ]
                    }
                }
            ],
            "fee": {
                "amount": [
                    {
                        "denom": "ustos",
                        "amount": "100"
                    }
                ],
                "gas": "200000"
            },
            "signatures": [
                {
                    "pub_key": {
                        "type": "tendermint/PubKeySecp256k1",
                        "value": "AolrbtnyTqnxmIjQJTmQfo/Gb2LlN9XPO/Qb2tSI/eRh"
                    },
                    "signature": "THrgfsKFIVlZvwzI7rHh3nRdC2VXJhaPDMyolZEsWklDmkxI7ecEA4bQgmkgXDpS7suKGvApsUIxeG4Um0vzWw=="
                }
            ],
            "memo": "Send Tx Example"
        }
    },
    "timestamp": "2021-08-08T18:53:14Z"
}
```

Check this block
```http
https://rest-tropos.thestratos.org/blocks/3495
```
Response
```json
{
    "block_id": {
        "hash": "082FD6C7397FD7F298D36289678242F0CDE2737DB4D9630B1AE62919A08057D0",
        "parts": {
            "total": "1",
            "hash": "DFB4268FAB528270752EB30FC3A8FFD67BB87B46856B3596A57057E01254A2DF"
        }
    },
    "block": {
        "header": {
            "version": {
                "block": "10",
                "app": "0"
            },
            "chain_id": "test-chain",
            "height": "3495",
            "time": "2021-08-08T18:53:14.172144192Z",
            "last_block_id": {
                "hash": "AE9834662BBEE22FC3C5E4F10CEEC64564AB9401A950BCC3AA795EA42F7612C0",
                "parts": {
                    "total": "1",
                    "hash": "AA0A15014CAE1BC085E104EEEDB06399205932B2E82551477EB10D8BF33190FE"
                }
            },
            "last_commit_hash": "EBB097637FF021777AAAA233064FE0E2B3F5BF1E63FF2D92DC06C8422F6441E6",
            "data_hash": "86B32C45FDF5B55C1B4BD6B943AE7BA86B1D03E4B7220280A51918389B6CD097",
            "validators_hash": "46DA95672ADBB099305219FB1C2B4A07A536984F94B9FD800DE0E3216ED375E5",
            "next_validators_hash": "46DA95672ADBB099305219FB1C2B4A07A536984F94B9FD800DE0E3216ED375E5",
            "consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            "app_hash": "5FFFD8BD063FBFDD6F3C30D75212AC4E13527948E757F491BF1488E396B5E217",
            "last_results_hash": "",
            "evidence_hash": "",
            "proposer_address": "38DD39B8E028C18399B21BD8CE78B433D3B61C0D"
        },
        "data": {
            "txs": [
                "2QEoKBapCkKoo2GaChQ07pN7D55YXeTxpV5cplLzrt15RhIUklkfkbrzqcI4RHfgxEybfe5YA/waEAoFdXN0b3MSBzIwMDAwMDASEgoMCgV1c3RvcxIDMTAwEMCaDBpqCibrWumHIQKJa27Z8k6p8ZiI0CU5kH6Pxm9i5TfVzzv0G9rUiP3kYRJATHrgfsKFIVlZvwzI7rHh3nRdC2VXJhaPDMyolZEsWklDmkxI7ecEA4bQgmkgXDpS7suKGvApsUIxeG4Um0vzWyIPU2VuZCBUeCBFeGFtcGxl"
            ]
        },
        "evidence": {
            "evidence": null
        },
        "last_commit": {
            "height": "3494",
            "round": "0",
            "block_id": {
                "hash": "AE9834662BBEE22FC3C5E4F10CEEC64564AB9401A950BCC3AA795EA42F7612C0",
                "parts": {
                    "total": "1",
                    "hash": "AA0A15014CAE1BC085E104EEEDB06399205932B2E82551477EB10D8BF33190FE"
                }
            },
            "signatures": [
                {
                    "block_id_flag": 2,
                    "validator_address": "38DD39B8E028C18399B21BD8CE78B433D3B61C0D",
                    "timestamp": "2021-08-08T18:53:14.172144192Z",
                    "signature": "MSNnYWdyczRWkYGuuoiD4G2XaezOzJZfsfNQsh/yfEI+ky191BSDj/Avn1gGEJIZ9k66lHCy+krqp8YyUX9LAw=="
                }
            ]
        }
    }
}
```
</details>
<br>

<details>
    <summary><code>POST /txs/decode</code> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Decode a transaction from the Amino wire format</summary>

Request Example:
```http
https://rest-tropos.thestratos.org/txs/decode
```
Request Body:
```json
{"tx":"3QEoKBapCkKoo2GaChQ07pN7D55YXeTxpV5cplLzrt15RhIUklkfkbrzqcI4RHfgxEybfe5YA/waEAoFdXN0b3MSBzIwMDAwMDASEgoMCgV1c3RvcxIDMTAwEMCaDBpqCibrWumHIQKJa27Z8k6p8ZiI0CU5kH6Pxm9i5TfVzzv0G9rUiP3kYRJATHrgfsKFIVlZvwzI7rHh3nRdC2VXJhaPDMyolZEsWklDmkxI7ecEA4bQgmkgXDpS7suKGvApsUIxeG4Um0vzWyITRW5jb2RpbmcgVHggRXhhbXBsZQ=="}
```
Response Example:
```json
{
  "height": "0",
  "result": {
    "msg": [
      {
        "type": "cosmos-sdk/MsgSend",
        "value": {
          "from_address": "st1xnhfx7c0nev9me835409efjj7whd672xs6d2m2",
          "to_address": "st1jfv3lyd67w5uywzywlsvgnym0hh9sqlujrw5l6",
          "amount": [
            {
              "denom": "ustos",
              "amount": "2000000"
            }
          ]
        }
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "ustos",
          "amount": "100"
        }
      ],
      "gas": "200000"
    },
    "signatures": [
      {
        "pub_key": {
          "type": "tendermint/PubKeySecp256k1",
          "value": "AolrbtnyTqnxmIjQJTmQfo/Gb2LlN9XPO/Qb2tSI/eRh"
        },
        "signature": "THrgfsKFIVlZvwzI7rHh3nRdC2VXJhaPDMyolZEsWklDmkxI7ecEA4bQgmkgXDpS7suKGvApsUIxeG4Um0vzWw=="
      }
    ],
    "memo": "Encoding Tx Example"
  }
}
```
</details>
<br>

-->

<br>

---

<br>