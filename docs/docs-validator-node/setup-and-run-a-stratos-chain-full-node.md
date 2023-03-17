---
title: Full-Chain Node
description: This document describes how to setup and run a Stratos-chain full-node in testnet. It offers sample code, implementation tips and reference material.
---

## Introduction

Stratos blockchain facilitates all decentralized ledger transactions and functionalities, providing settlement services and related financial payment services for network providers and network users in an efficient, fair and transparent manner.

The Stratos-chain full-nodes are dedicated servers with sufficient computing power that participate in block generation cycle. It is necessary in order to be a validator.

In practice, running a full-node only implies running a non-compromised and up-to-date version of the software with low network latency and without downtime. It is encouraged to run a full-node even if you do not plan to be a validator.

The Stratos-chain validator is a full-node that participates in the Stratos Chain block generation cycle and also voting for the validity of a block proposed.


<br>

---

## Requirements

Here are the recommended hardware/software to run a Stratos-chain full-node:

<b>Recommended Hardware</b>

* CPU           i5 (4 cores)
* RAM           16GB
* Hard disk     2TB

<b>Software(tested version)</b>

* Ubuntu 18.04+
* Go 1.18+ linux/amd64


<br>

---

## Setup Environment

In order to run a Stratos-chain full-node, you may need to build `stratos-chain` source code yourself which requires `Go 1.18+`, `git`, `curl` and `make` installed.

This process depends on your operating system.

<br>

### Linux Users
 

The following example is based on Ubuntu 18.04+ 64-bit(Debian) and assumes you are using a terminal environment by default.

Please run the equivalent commands if you are running other Linux distributions.

```shell
# Update the system
sudo apt update
sudo apt upgrade
    
# Install git, snap and make(you can also install them separately as your needs)
sudo apt install git build-essential curl snapd --yes
    
# Install Go 1.18+ with Snap and export environment variables(You can also install Go 1.18+ in your way)
sudo snap install go --classic
echo 'export GOPATH="$HOME/go"' >> ~/.profile
echo 'export GOBIN="$GOPATH/bin"' >> ~/.profile
echo 'export PATH="$GOBIN:$PATH"' >> ~/.profile
source ~/.profile
```

<br>


### MacOS Users


To install the required build tools, you can easily [install Xcode from the Mac App Store](https://apps.apple.com/hk/app/xcode/id497799835?l=en&mt=12).

The best practice to install Go is to use [Homebrew](https://brew.sh/).

```shell
# Install software using Homebrew
brew install go git curl

# Export environment variables
echo 'export GOPATH="$HOME/go"' >> ~/.profile
echo 'export GOBIN="$GOPATH/bin"' >> ~/.profile
echo 'export PATH="$GOBIN:$PATH"' >> ~/.profile
source ~/.profile
```

<br>

### Windows Users


It is possible to build and run the software on Windows. However, we did not test it on Windows completely.

It may give you unexpected results, or it may require additional setup.

An alternative option is to install a separate virtual Linux system using [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [VMware Workstation](https://www.vmware.com/ca/products/workstation-player/workstation-player-evaluation.html).

<br>

---

## Setup a Stratos-chain full-node

<br>

### Create a user account

To create a separated and more secure environment, it is recommended to create a separated user account `stratos` to run your node.

```shell
sudo adduser stratos --home /home/stratos
```

Once the user account `stratos` is created, switch and login the system using `stratos`. You will proceed with the following steps in context of that user.

<br>

### Get release files

!!! tip

    There are two ways to get the these binary executables:

    - Download pre-compiled executabled (for Ubuntu 18.04+ x86_64).
    - Download source code and compile it yourself.
    
    Please choose only one of them based on your operating system.

<br>

#### Pre-compiled executables


The following binary `stchaind` has been built and ready to be downloaded directly.

```shell
# Make sure we are inside the $HOME folder
cd $HOME
wget https://github.com/stratosnet/stratos-chain/releases/download/v0.9.0/stchaind
```

!!! tip

    💡 This binary is built for Ubuntu 18.04+ amd64. if you have other Linux kernels, please follow the next step to build your own binary with source code. For ease of use, we recommend saving this binary in your `$HOME` folder. 

    If you are not sure what is your `$HOME` folder, in terminal, use `echo $HOME` to check. In the following instruction, we assume you have entered the `$HOME` folder(use `cd $HOME`)

<br>

- Check the granularity

```shell
# Make sure we are inside the $HOME folder and check these two binary executables
cd $HOME

# Check granularity
md5sum stchain*

## Expected output
## 1843b162a7d2b1f4363938fc73d421e8  stchaind
```

<br>

- Add execute permission to this binary

```shell
# Make sure the file can be executed
chmod +x stchaind
```

<br>

- Add the binary to the search path

```shell
echo 'export PATH="$HOME:$PATH"' >> ~/.profile
source ~/.profile
```

<br>

#### Compile the source code

Before the following steps, please make sure you have `Go 1.18+` installed [link](https://golang.org/doc/install).

<br>

- Build the extracted source code

```shell
git clone https://github.com/stratosnet/stratos-chain.git
cd stratos-chain
git checkout tags/v0.9.0
make build
```

<br>

- Installing the binary executable

The binary can be installed to the default $GOPATH/bin folder by running:

```shell
make install
```

<br>

---

## Get genesis and config file

- Initialize your node

```shell
# Make sure we are inside the home directory
cd $HOME
    
# Create folders and initialize the node
./stchaind init "<your_node_moniker>"
    
# ignore the output since you need to download the genesis file 
```

!!! tip

    💡 You can choose any `your_node_moniker` you prefer. It will be saved in the `config.toml` under the `.stchaind/config/` directory.

<br>

- Download the `genesis.json` and `config.toml` files

```shell
wget https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/genesis.json
wget https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/config.toml
```


!!! tip

    💡 We strongly recommend using this downloaded `config.toml` for v0.9.0, instead of the ones for previous versions to avoid any mismatching. 

    A sample of `config.toml` can be found below
        
<br>

<details>
    <summary>Example of config.toml</summary>

```shell

# This is a TOML config file.
# For more information, see https://github.com/toml-lang/toml# NOTE: Any path below can be absolute (e.g. "/var/myawesomeapp/data") or
# relative to the home directory (e.g. "data"). The home directory is
# "$HOME/.tendermint" by default, but could be changed via $TMHOME env variable
# or --home cmd flag.#######################################################################
###                   Main Base Config Options                      ###
#######################################################################
      
# TCP or UNIX socket address of the ABCI application,
# or the name of an ABCI application compiled in with the Tendermint binary
proxy_app = "tcp://127.0.0.1:26658"
      
# A custom human readable name for this node
moniker = "node"
    
# If this node is many blocks behind the tip of the chain, FastSync
# allows them to catchup quickly by downloading blocks in parallel
# and verifying their commits
fast_sync = true
      
# Database backend: goleveldb | cleveldb | boltdb | rocksdb | badgerdb
# * goleveldb (github.com/syndtr/goleveldb - most popular implementation)
#   - pure go
#   - stable
# * cleveldb (uses levigo wrapper)
#   - fast
#   - requires gcc
#   - use cleveldb build tag (go build -tags cleveldb)
# * boltdb (uses etcd's fork of bolt - github.com/etcd-io/bbolt)
#   - EXPERIMENTAL
#   - may be faster is some use-cases (random reads - indexer)
#   - use boltdb build tag (go build -tags boltdb)
# * rocksdb (uses github.com/tecbot/gorocksdb)
#   - EXPERIMENTAL
#   - requires gcc
#   - use rocksdb build tag (go build -tags rocksdb)
# * badgerdb (uses github.com/dgraph-io/badger)
#   - EXPERIMENTAL
#   - use badgerdb build tag (go build -tags badgerdb)
db_backend = "goleveldb"
     
# Database directory
db_dir = "data"
    
# Output level for logging, including package level options
log_level = "info"
      
# Output format: 'plain' (colored text) or 'json'
log_format = "plain"
     
##### additional base config options #####
      
# Path to the JSON file containing the initial validator set and other meta data
genesis_file = "config/genesis.json"
     
# Path to the JSON file containing the private key to use as a validator in the consensus protocol
priv_validator_key_file = "config/priv_validator_key.json"
     
# Path to the JSON file containing the last sign state of a validator
priv_validator_state_file = "data/priv_validator_state.json"
      
# TCP or UNIX socket address for Tendermint to listen on for
# connections from an external PrivValidator process
priv_validator_laddr = ""
      
# Path to the JSON file containing the private key to use for node authentication in the p2p protocol
node_key_file = "config/node_key.json"
      
# Mechanism to connect to the ABCI application: socket | grpc
abci = "socket"
      
# If true, query the ABCI app on connecting to a new peer
# so the app can decide if we should keep the connection or not
filter_peers = false
      
      
#######################################################################
###                 Advanced Configuration Options                  ###
#######################################################################
      
#######################################################
###       RPC Server Configuration Options          ###
#######################################################
[rpc]
      
# TCP or UNIX socket address for the RPC server to listen on
laddr = "tcp://127.0.0.1:26657"
      
# A list of origins a cross-domain request can be executed from
# Default value '[]' disables cors support
# Use '["*"]' to allow any origin
cors_allowed_origins = []
      
# A list of methods the client is allowed to use with cross-domain requests
cors_allowed_methods = ["HEAD", "GET", "POST", ]
      
# A list of non simple headers the client is allowed to use with cross-domain requests
cors_allowed_headers = ["Origin", "Accept", "Content-Type", "X-Requested-With", "X-Server-Time", ]
      
# TCP or UNIX socket address for the gRPC server to listen on
# NOTE: This server only supports /broadcast_tx_commit
grpc_laddr = ""
      
# Maximum number of simultaneous connections.
# Does not include RPC (HTTP&WebSocket) connections. See max_open_connections
# If you want to accept a larger number than the default, make sure
# you increase your OS limits.
# 0 - unlimited.
# Should be < {ulimit -Sn} - {MaxNumInboundPeers} - {MaxNumOutboundPeers} - {N of wal, db and other open files}
# 1024 - 40 - 10 - 50 = 924 = ~900
grpc_max_open_connections = 900
      
# Activate unsafe RPC commands like /dial_seeds and /unsafe_flush_mempool
unsafe = false
      
# Maximum number of simultaneous connections (including WebSocket).
# Does not include gRPC connections. See grpc_max_open_connections
# If you want to accept a larger number than the default, make sure
# you increase your OS limits.
# 0 - unlimited.
# Should be < {ulimit -Sn} - {MaxNumInboundPeers} - {MaxNumOutboundPeers} - {N of wal, db and other open files}
# 1024 - 40 - 10 - 50 = 924 = ~900
max_open_connections = 900
    
# Maximum number of unique clientIDs that can /subscribe
# If you're using /broadcast_tx_commit, set to the estimated maximum number
# of broadcast_tx_commit calls per block.
max_subscription_clients = 100
      
# Maximum number of unique queries a given client can /subscribe to
# If you're using GRPC (or Local RPC client) and /broadcast_tx_commit, set to
# the estimated # maximum number of broadcast_tx_commit calls per block.
max_subscriptions_per_client = 5
      
# Experimental parameter to specify the maximum number of events a node will
# buffer, per subscription, before returning an error and closing the
# subscription. Must be set to at least 100, but higher values will accommodate
# higher event throughput rates (and will use more memory).
experimental_subscription_buffer_size = 200
      
# Experimental parameter to specify the maximum number of RPC responses that
# can be buffered per WebSocket client. If clients cannot read from the
# WebSocket endpoint fast enough, they will be disconnected, so increasing this
# parameter may reduce the chances of them being disconnected (but will cause
# the node to use more memory).
#
# Must be at least the same as "experimental_subscription_buffer_size",
# otherwise connections could be dropped unnecessarily. This value should
# ideally be somewhat higher than "experimental_subscription_buffer_size" to
# accommodate non-subscription-related RPC responses.
experimental_websocket_write_buffer_size = 200
      
# If a WebSocket client cannot read fast enough, at present we may
# silently drop events instead of generating an error or disconnecting the
# client.
#
# Enabling this experimental parameter will cause the WebSocket connection to
# be closed instead if it cannot read fast enough, allowing for greater
# predictability in subscription behaviour.
experimental_close_on_slow_client = false
      
# How long to wait for a tx to be committed during /broadcast_tx_commit.
# WARNING: Using a value larger than 10s will result in increasing the
# global HTTP write timeout, which applies to all connections and endpoints.
# See https://github.com/tendermint/tendermint/issues/3435
timeout_broadcast_tx_commit = "10s"
      
# Maximum size of request body, in bytes
max_body_bytes = 1000000
      
# Maximum size of request header, in bytes
max_header_bytes = 1048576
      
# The path to a file containing certificate that is used to create the HTTPS server.
# Might be either absolute path or path related to Tendermint's config directory.
# If the certificate is signed by a certificate authority,
# the certFile should be the concatenation of the server's certificate, any intermediates,
# and the CA's certificate.
# NOTE: both tls_cert_file and tls_key_file must be present for Tendermint to create HTTPS server.
# Otherwise, HTTP server is run.
tls_cert_file = ""
    
# The path to a file containing matching private key that is used to create the HTTPS server.
# Might be either absolute path or path related to Tendermint's config directory.
# NOTE: both tls-cert-file and tls-key-file must be present for Tendermint to create HTTPS server.
# Otherwise, HTTP server is run.
tls_key_file = ""
      
# pprof listen address (https://golang.org/pkg/net/http/pprof)
pprof_laddr = "localhost:6060"
      
#######################################################
###           P2P Configuration Options             ###
#######################################################
[p2p]
      
# Address to listen for incoming connections
laddr = "tcp://0.0.0.0:26656"
      
# Address to advertise to peers for them to dial
# If empty, will use the same port as the laddr,
# and will introspect on the listener or use UPnP
# to figure out the address. ip and port are required
# example: 159.89.10.97:26656
external_address = ""
      
# Comma separated list of seed nodes to connect to
seeds = ""
    
# Comma separated list of nodes to keep persistent connections to
persistent_peers = ""
      
# UPNP port forwarding
upnp = false
      
# Path to address book
addr_book_file = "config/addrbook.json"
      
# Set true for strict address routability rules
# Set false for private or local networks
addr_book_strict = true
    
# Maximum number of inbound peers
max_num_inbound_peers = 40
      
# Maximum number of outbound peers to connect to, excluding persistent peers
max_num_outbound_peers = 10
      
# List of node IDs, to which a connection will be (re)established ignoring any existing limits
unconditional_peer_ids = ""
      
# Maximum pause when redialing a persistent peer (if zero, exponential backoff is used)
persistent_peers_max_dial_period = "0s"
     
# Time to wait before flushing messages out on the connection
flush_throttle_timeout = "100ms"
      
# Maximum size of a message packet payload, in bytes
max_packet_msg_payload_size = 1024
      
# Rate at which packets can be sent, in bytes/second
send_rate = 5120000
      
# Rate at which packets can be received, in bytes/second
recv_rate = 5120000
      
# Set true to enable the peer-exchange reactor
pex = true
      
# Seed mode, in which node constantly crawls the network and looks for
# peers. If another node asks it for addresses, it responds and disconnects.
#
# Does not work if the peer-exchange reactor is disabled.
seed_mode = false
      
# Comma separated list of peer IDs to keep private (will not be gossiped to other peers)
private_peer_ids = ""
      
# Toggle to disable guard against peers connecting from the same ip.
allow_duplicate_ip = false
      
# Peer connection configuration.
handshake_timeout = "20s"
dial_timeout = "3s"
      
#######################################################
###          Mempool Configuration Option          ###
#######################################################
[mempool]
      
recheck = true
broadcast = true
wal_dir = ""
      
# Maximum number of transactions in the mempool
size = 5000
      
# Limit the total size of all txs in the mempool.
# This only accounts for raw transactions (e.g. given 1MB transactions and
# max_txs_bytes=5MB, mempool will only accept 5 transactions).
max_txs_bytes = 1073741824
      
# Size of the cache (used to filter transactions we saw earlier) in transactions
cache_size = 10000
      
# Do not remove invalid transactions from the cache (default: false)
# Set to true if it's not possible for any invalid transaction to become valid
# again in the future.
keep-invalid-txs-in-cache = false
      
# Maximum size of a single transaction.
# NOTE: the max size of a tx transmitted over the network is {max_tx_bytes}.
max_tx_bytes = 1048576
      
# Maximum size of a batch of transactions to send to a peer
# Including space needed by encoding (one varint per transaction).
# XXX: Unused due to https://github.com/tendermint/tendermint/issues/5796
max_batch_bytes = 0
      
#######################################################
###         State Sync Configuration Options        ###
#######################################################
[statesync]
# State sync rapidly bootstraps a new node by discovering, fetching, and restoring a state machine
# snapshot from peers instead of fetching and replaying historical blocks. Requires some peers in
# the network to take and serve state machine snapshots. State sync is not attempted if the node
# has any local state (LastBlockHeight > 0). The node will have a truncated block history,
# starting from the height of the snapshot.
enable = false
      
# RPC servers (comma-separated) for light client verification of the synced state machine and
# retrieval of state data for node bootstrapping. Also needs a trusted height and corresponding
# header hash obtained from a trusted source, and a period during which validators can be trusted.
#
# For Cosmos SDK-based chains, trust_period should usually be about 2/3 of the unbonding time (~2
# weeks) during which they can be financially punished (slashed) for misbehavior.
rpc_servers = ""
trust_height = 0
trust_hash = ""
trust_period = "168h0m0s"
      
# Time to spend discovering snapshots before initiating a restore.
discovery_time = "15s"
      
# Temporary directory for state sync snapshot chunks, defaults to the OS tempdir (typically /tmp).
# Will create a new, randomly named directory within, and remove it when done.
temp_dir = ""
      
# The timeout duration before re-requesting a chunk, possibly from a different
# peer (default: 1 minute).
chunk_request_timeout = "10s"
      
# The number of concurrent chunk fetchers to run (default: 1).
chunk_fetchers = "4"
      
#######################################################
###       Fast Sync Configuration Connections       ###
#######################################################
[fastsync]
      
# Fast Sync version to use:
#   1) "v0" (default) - the legacy fast sync implementation
#   2) "v1" - refactor of v0 version for better testability
#   2) "v2" - complete redesign of v0, optimized for testability & readability
version = "v0"
      
#######################################################
###         Consensus Configuration Options         ###
#######################################################
[consensus]
      
wal_file = "data/cs.wal/wal"
      
# How long we wait for a proposal block before prevoting nil
timeout_propose = "3s"
# How much timeout_propose increases with each round
timeout_propose_delta = "500ms"
# How long we wait after receiving +2/3 prevotes for “anything” (ie. not a single block or nil)
timeout_prevote = "1s"
# How much the timeout_prevote increases with each round
timeout_prevote_delta = "500ms"
# How long we wait after receiving +2/3 precommits for “anything” (ie. not a single block or nil)
timeout_precommit = "1s"
# How much the timeout_precommit increases with each round
timeout_precommit_delta = "500ms"
# How long we wait after committing a block, before starting on the new
# height (this gives us a chance to receive some more precommits, even
# though we already have +2/3).
timeout_commit = "5s"
      
# How many blocks to look back to check existence of the node's consensus votes before joining consensus
# When non-zero, the node will panic upon restart
# if the same consensus key was used to sign {double_sign_check_height} last blocks.
# So, validators should stop the state machine, wait for some blocks, and then restart the state machine to avoid panic.
double_sign_check_height = 0
      
# Make progress as soon as we have all the precommits (as if TimeoutCommit = 0)
skip_timeout_commit = false
      
# EmptyBlocks mode and possible interval between empty blocks
create_empty_blocks = true
create_empty_blocks_interval = "0s"
      
# Reactor sleep duration parameters
peer_gossip_sleep_duration = "100ms"
peer_query_maj23_sleep_duration = "2s"
      
#######################################################
###   Transaction Indexer Configuration Options     ###
#######################################################
[tx_index]
      
# What indexer to use for transactions
#
# The application will set which txs to index. In some cases a node operator will be able
# to decide which txs to index based on configuration set in the application.
#
# Options:
#   1) "null"
#   2) "kv" (default) - the simplest possible indexer, backed by key-value storage (defaults to levelDB; see DBBackend).
# 		- When "kv" is chosen "tx.height" and "tx.hash" will always be indexed.
indexer = "kv"
      
#######################################################
###       Instrumentation Configuration Options     ###
#######################################################
[instrumentation]
      
# When true, Prometheus metrics are served under /metrics on
# PrometheusListenAddr.
# Check out the documentation for the list of available metrics.
prometheus = false
      
# Address to listen for Prometheus collector(s) connections
prometheus_listen_addr = ":26660"
      
# Maximum number of simultaneous connections.
# If you want to accept a larger number than the default, make sure
# you increase your OS limits.
# 0 - unlimited.
max_open_connections = 3
      
# Instrumentation namespace
namespace = "tendermint"
```


</details>
        
   <br>

- Change `moniker` in the downloaded `config.toml` file

Please change your node moniker by modifying the `config.toml` file. Open this file with an editor, search `moniker` (usually at Line #16) in the file to find the “moniker” field. 

Change it to any value you like. It’s your node name that will show on the network.

```shell
# A custom human readable name for this node
moniker = "<your_node_moniker>"
```

- Move the downloaded `config.toml` and `genesis.json` files to `$HOME/.stchaind/config/` folder. Replace if you already have these files.

```shell
mv config.toml $HOME/.stchaind/config/
mv genesis.json $HOME/.stchaind/config/
```

<br>

---

## Directory structure

After you finished the above steps, your `$HOME` folder should include the following directories and files.

``` { .yaml .no-copy }
.
├── ...
├── .stchaind
│   ├── config
│   │   ├── app.toml
│   │   ├── config.toml
│   │   ├── genesis.json
│   │   ├── node_key.json
│   │   └── priv_validator_key.json
│   ├── data
│   │    └── priv_validator_state.json 
│   └── keyring-test
├── ...
```

!!! tip

    💡 By default, directory `.stchaind` will be created in the `$HOME` folder. The `.stchaind` folder contains the node's configurations and data.

<br>

---

## Start the full-chain node


There are three ways to run your Stratos-chain full-node. 

Please choose ONE of them to start the node.

<br>

### In foreground


```shell
# Make sure we are inside the home directory
cd $HOME

# run your node
./stchaind start

# Use `Ctrl+c` to stop the node.
```

<br>

### In background


```shell
# Make sure we are inside the home directory
cd $HOME

# run your node in backend
./stchaind start 2>&1 >> chain.log & 
```

Use an editor to check your node log at `chain.log`

Use the following Linux Command to stop your node.

```shell
pkill stchaind
```

<br>

### As service

All below steps require root privileges

<br>

- Create the service file

Create the `/lib/systemd/system/stratos.service` file with the following content

```shell
[Unit]
Description=Stratos Chain Node
After=network-online.target

[Service]
User=stratos
ExecStart=/home/stratos/stchaind start --home=/home/stratos/.stchaind
Restart=on-failure
RestartSec=3
LimitNOFILE=8192

[Install]
WantedBy=multi-user.target
```

!!! tip

    💡 In the [service] section:

    - `User` is your system login username
    - `ExecStart` designates the absolute path to the binary `stchaind`
    - `--home` is the absolute path to your node folder.
    - We used the default values for these variables. If you use a different username, group or folder to hold your node data instead of the default values, please modify these values according to your situations. Make sure the above values are correct.

<br>

- Start your service

Once you have successfully created the service, you need to enable and start it by running

```shell
systemctl daemon-reload
systemctl enable stratos.service
systemctl start stratos.service
```

<br>

#### Service operations

- Check the service status

```shell
systemctl status stratos.service
```

- Check service log

```shell
journalctl -u stratos.service -f 

# exit with ctrl+c
```

- Stop the service

```shell
systemctl stop stratos.service
```

<br>

---

## Check node status

Once you start your full-node, it will connect to the peers and start syncing. You can check the status of the node by running the following command

```shell
# Check the status of the node
./stchaind status
```

The output will be similar to

```json
./stchaind status
{
    "NodeInfo": {
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
    "SyncInfo": {
        "latest_block_hash": "697A2DB243E5191C6D85285A2ADD4924526924969C6C70FE71827C9FE41D4373",
        "latest_app_hash": "E978F87BB23D351B853F5F0CF9FBBBA4464FF5D7CE3746BF3E2357F28CBCE041",
        "latest_block_height": "497",
        "latest_block_time": "2023-01-11T01:10:37.562405326Z",
        "earliest_block_hash": "139676534FECFA507D56A06B03BD528E70ACA6D4DB6560219707011966613DE4",
        "earliest_app_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "earliest_block_height": "1",
        "earliest_block_time": "2023-01-09T17:08:58.4890503Z",
        "catching_up": false
    },
    "ValidatorInfo": {
        "Address": "18A7169C1B427D994133F7B3D4504E92789DB37C",
        "PubKey": {
            "type": "tendermint/PubKeyEd25519",
            "value": "69gothWTE9FJBZ5gBjjSNhg8y/5SsI1hBaD81Dum7lo="
        },
        "VotingPower": "500000"
    }
}
```

If the `catching_up` value is `false` in the `sync_info` section, it means that you are fully synced.

If it is `true`, it means your node is still syncing.

<br>

---

## Setup a wallet


Once the node finishes catch-up, you are ready to operate your node for various transactions(tx) and queries.

!!! tip

    💡 By default, the following commands can be applied in the node folder(`$HOME`) directory.

In order to hold the tokens that you will later delegate to your validator node, or pay staking for your SDS resource node, first, you need to create a local wallet account.

<br>

### Create a new wallet

To create a new wallet account, type the following command

```shell
./stchaind keys add <your wallet name> --hd-path="m/44'/606'/0'/0/0" --keyring-backend=<keyring's backend>
```

!!! tip

    💡 In the testing phase, the `keyring's backend` is `test`, i.e., `--keyring-backend=test`

    Please select a wallet name that you will easily remember. This name will be used all over the places inside other commands later.

After creating a new local wallet account, you will get its `address` and `pubkey`.

In addition, you will have a secret recovery phrase(mnemonic phrase) which can be used to recover an existing wallet account and should be kept secret.

Example:

``` { .yaml .no-copy }
./stchaind keys add myWallet --hd-path="m/44'/606'/0'/0/0" --keyring-backend=test

- name: myWallet
type: local
address: st1x2c6gy4vr8alsyzuqr2x8x8xxtvs97sk3jt6dp
pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A7HCZTlHEarBPabkOgId5SlyQKdqEsbXJHit7y9LXRy+"}'
mnemonic: ""


**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

venue chest pattern tool certain identify adult theme thing public foster promote pave topple thing uncle brisk suffer present popular envelope wrap holiday goddess
```

<br>

### Recover an existing wallet

If you already have a Stratos wallet account, you can recover it by typing the following command

```shell
./stchaind keys add <your wallet name> --recover --hd-path="m/44'/606'/0'/0/0" --keyring-backend=<keyring's backend> 
```

!!! tip
    💡 In the testing phase, `--keyring-backend=test`

Example:

```shell
./stchaind keys add myWallet1 --recover --hd-path="m/44'/606'/0'/0/0" --keyring-backend=test  
```

<br>

After the above `keys add` command executed, a `keyring-test` folder will be created which contains your wallets' information with their addresses. 
 
 The `keyring-test` folder looks like

``` { .yaml .no-copy }
.
├── 32b1a412ac19fbf8105c00d46398e632d902fa16.address
├── d0c57269c450f81234307a33bd148ac4f90549e5.address
├── myWallet1.info
└── myWallet.info
```

<br>

### Check your wallet

There are two ways to check your local wallets

<br>

- Check all local wallet accounts

```shell
./stchaind keys list --keyring-backend=<keyring's backend> 
```

Example:

``` { .yaml .no-copy }
./stchaind keys list --keyring-backend=test
   - name: user0
     type: local
     address: st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A/wF15Wd3ogCXstE7S4Zf3DA4KXb0W7exQhP004PLTi3"}'
     mnemonic: ""
   - name: user1
     type: local
     address: st1dz20dmhjkuc2tur3amgl8t45w807a640leh8p0
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AgnhB5EkHL8+jD0/zRDR11nIpfOirTRrjgCX6uibhmDW"}'
     mnemonic: ""
   - name: user10
     type: local
     address: st1lkcrz3ktt2p7ppu9arglpqcn94pcdd9a9pmatf
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A2sZ2Z9BU9oDELC06Gj8Lfc5UycxTaPux3sEIq8sIzSW"}'
     mnemonic: ""
   - name: user2
     type: local
     address: st16czjk2ym0prgvy4gl970t84vrp96s5kayfqmf2
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AwfcJTOVWdx6ai61cy8VGJ1SdWHzwm2CCmr/+PwSpFeR"}'
     mnemonic: ""
   - name: user3
     type: local
     address: st17patveqxcq42rguc7nayr2g3jtawpzvhfmmumt
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AtFxbuB4s+2SYzImGPIBwe0H0mKCXbIPu1T63OvbgE/3"}'
     mnemonic: ""
```

<br>

- Check a specific local wallet account

```shell
./stchaind keys show <your wallet name> --keyring-backend=<keyring's backend> 
```

Example:

``` { .yaml .no-copy }
./stchaind keys show myWallet1 --keyring-backend=test
   - name: myWallet1
     type: local
     address: st16rzhy6wy2rupydps0gem69y2cnus2j09n42ksx
     pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A13YKi3/7p9FsFPTfVgxEO0YK8bnDHmBPfA3ID+k37ET"}'
     mnemonic: ""
```

<br>

---

## Faucet

Faucet will be available at *faucet-tropos.thestratos.org* to get test tokens into your wallet.

```shell
curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"your wallet address"} ' https://faucet-tropos.thestratos.org/credit
```

!!! tip

    Replace "your wallet address" with your st1xx wallet address

    💡1stos = 1,000,000,000gwei = 1,000,000,000,000,000,000wei

<br>

- Check wallet account balance

You can query your account info using this command:

```shell
./stchaind query account <your wallet address>
```

Example:

``` { .yaml .no-copy }
./stchaind query account st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx
|
'@type': /cosmos.auth.v1beta1.BaseAccount
account_number: "1"
address: st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx
pub_key: null
sequence: "0"
```

<br>

  You can query your wallet balances using this command:

```shell
./stchaind query bank balances <your wallet address>
```

Example:

``` { .yaml .no-copy }
./stc./stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
|
balances:
- amount: "200"
  denom: utros
- amount: "9998000000000000000"
  denom: wei
pagination:
next_key: null
total: "0
```

<br>

- Try your first tx - `send`


 This tx command will send an amount of tokens from one wallet address to another:

 ```shell
 ./stchaind tx bank send <from address> <to address> <amount> --keyring-backend=<keyring's backend> --chain-id=<current chain-id> --gas=auto --gas-prices=1000000000wei
 ```
 
!!! tip

    * The current `chain-id` can be found on the [`Stratos Explorer`](https://explorer-tropos.thestratos.org/) right next to the search bar at the top of the page.
    * In the testing phase, `--keyring-backend=test`
    * Make sure your `<from address>` has enough tokens
    * Please wait for around 7 seconds for block generation after a transaction.

Example:

Let us assume:

* `from address`: st1dz20dmhjkuc2tur3amgl8t45w807a640leh8p0
* `to address`: st123wun5lnwerdrt0mk2uxtusgawpfr228a0sseg
* `amount`: 10stos

``` { .yaml .no-copy }
./stchaind tx bank send st1dz20dmhjkuc2tur3amgl8t45w807a640leh8p0 st123wun5lnwerdrt0mk2uxtusgawpfr228a0sseg 10stos --chain-id=tropos-5  --keyring-backend=test --gas=100000 --gas-prices=1000000000wei -y
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: BA96CF87646592487ABB9DDDE8FA86FE71441226281B04E15C5C66EDE415FBC6
```

<br>

---

<br>