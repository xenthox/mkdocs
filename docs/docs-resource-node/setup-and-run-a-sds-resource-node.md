---
title: Setup and run a SDS Resource Node
description: How to setup and run a Stratos SDS Resource node. Tutorial and examples.
---


The Stratos Decentralized Storage (SDS) network is a scalable, reliable, self-balancing elastic acceleration network. We can simply take it as a decentralized file system suitable for running on general-purpose hardware.

SDS is composed of many Resource Nodes that store data, and a few Meta Nodes that coordinate with each other.

Note that provides their resource(disk/bandwidth/computation power) for SDS is called Resource Node.

---

## Requirements

!!! tip

    Unlike other projects, Stratos does not require expensive GPUs and high wattage power supplies, but the node needs to provide enough bandwidth and storage capacity to ensure the traffic on the node can reach the reward requirements.


- <b>Recommended Hardware</b>

    * CPU           i5 (4 cores)
    * RAM           16GB
    * Hard disk     2TB
    * Bandwidth     100M

<br>

- <b>Software(tested version)</b>

    * Ubuntu 18.04+
    * Go 1.18+ linux/amd64

---

## Keywords

There are some keywords that are widely used in SDS. We describe them as

* `resource node`(`PP` node): Node that participates in the Stratos Resource Network by providing their disk/bandwidth/computation power to earn rewards in the Proof-of-Traffic(PoT) model.

* `meta node`(`SP` nodes): Node that manages the tasks in the Resource Network between resource nodes, including indexing all content, auditing the traffic report and communicating between Resource Network and Stratos-chain through a relay mechanism.

* `active resource node`: A resource node that has been activated by depositing to the Stratos-chain and registering to a meta node. It is ready to receive tasks assigned by the meta node.

* `suspended resource node`: A resource node that has not satisfied the performance KPI evaluation criteria and is suspended from receiving further tasks from the meta node.

* `traffic`: The data volume evaluated in the Resource Network. The incentive for all participants in the Stratos Ecosystem is based on traffic.

* `STOS`(Stratos Tokens): The native token facilitating value circulation in Stratos Ecosystem.

* `ozone`(oz): The traffic unit used in Stratos Ecosystem.

* `epoch`: The Proof-of-Traffic evaluation periodic window. The traffic for the Resource Network is evaluated at the end of each epoch.

* `value network`: The Stratos-chain, the network that circulates all values in the Stratos Ecosystem.

---

## Setup Environment

!!! tip

    In order to run an SDS resource node, you need to build SDS source code which requires `Go 1.18+`, `git`, `curl` and `make` installed.
    
    If you have installed them previously, just skip this section. Otherwise, please install them as the following

This process depends on your operating system.

<br>

- <b>Linux Users</b>


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


- <b>MacOS Users</b>

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

- <b>Windows Users</b>


It is possible to build and run the software on Windows. However, we did not test it on Windows completely.
It may give you unexpected results, or it may require additional setup.

An alternative option is to install a separate virtual Linux system using [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [VMware Workstation](https://www.vmware.com/ca/products/workstation-player/workstation-player-evaluation.html)

---

## Setup SDS resource node

<br>

- Compile the binary executables with source code

Before the following steps, please make sure you have `Go 1.18+` installed ([link](https://golang.org/doc/install)).

```shell
git clone https://github.com/stratosnet/sds.git
cd sds
git checkout tags/v0.9.0
make build
```

Then you will find the binary executable `ppd` under the folder `target`

<br>

- Installing the binary executable

The binary can be installed to the default `$GOPATH/bin` folder by running:

```shell
make install
```

The binary should then be runnable from any folder if you have set up `go env` properly.

---

## Create SDS resource node

<br>

### Create a root directory

To start a resource node, you need to be in a directory dedicated to your resource node.

Create a new directory, or go to the root directory of your existing node.

In the following instruction, we assume you have entered the root directory of the resource node.

```shell
# create a new folder 
cd $HOME
mkdir rsnode
# Make sure we are inside the root directory of the resource node
cd rsnode
```

<br>

### Configure SDS resource node

Next, you need to generate the configuration file and its accounts of this resource node. The command `ppd config` will help you to generate necessary configurations.

<br>

#### Generate/Recover wallet


The `ppd config` command consists of several flags or subcommand. Let take a look at its general definition using `ppd config -h`.

``` { .yaml .no-copy }
ppd config -h

create default configuration file

Usage:
  ppd config [flags]
  ppd config [command]

Available Commands:
  accounts    create accounts for the node

Flags:
  -p, --create-p2p-key   create p2p key with config file, need interactive input
  -w, --create-wallet    create wallet with config file, need interactive input
  -h, --help             help for config

Global Flags:
  -c, --config string   configuration file path  (default "./configs/config.toml")
  -r, --home string     path for the node (default "<root directory of your resource node>")

Use "ppd config [command] --help" for more information about a command.
```

<br>

!!! tip

    There are two ways to generate a configuration file and create (or recover) a wallet:

    - Option 1 will generate a configuration file and allow you to create or recover a wallet.

    - Option 2 will generate a configuration file and a wallet automatically.


<br>

Option 1: Using <b><code>ppd config -w -p</code></b> command (recommended)


When asked to `input bip39 mnemonic`,

>Input your mnemonic -> recovers an existing wallet account;

>keep it blank -> generates a new wallet account

Usage:

```shell
# Make sure we are inside the root directory of the resource node
cd rsnode
# to create config with interactive key creation
ppd config -w -p
```
<details>
  <summary><b>Example (recovering an existing wallet account)</b></summary>

You will get the same wallet account as your existing one

``` { .yaml .no-copy }
# Make sure we are inside the root directory of the resource node
cd rsnode
ppd config -w -p

[INFO]2022/02/22 11:25:19 setting.go:125: The config at location /home/hong/stratos/sds0113/sds/rsnode2/configs/config.toml does not exist
generating default config file
[INFO]2022/02/22 11:25:19 node.go:67: No P2P key specified in config. Attempting to create one...
Enter password:               # enter your P2P key password
Enter password again:
[INFO]2022/02/22 11:25:21 setting.go:251: finished changing configuration file  P2PAddress:  stsdsp2p1arpvg0wthng0yty06ay0uup3vw7k3upm586few
No wallet key specified in config. Attempting to create one...
Enter wallet nickname: wallet1            # enter your wallet nickname
Enter password:                           # enter your wallet key password
Enter password again:
        
# Inputting the mnemonic phrase here will recovering an existing wallet account(mnemonic will not show on the screen). 

input bip39 mnemonic (leave blank to generate a new one)   # It is better to use copy and paste for inputting. 
input hd-path for the account, default: "m/44'/606'/0'/0/0" :      # Make sure to use the default hd-path
[INFO] 2022/02/22 11:25:34 setting.go:251: finished changing configuration file  WalletAddress:  st10t5chdnhx6myggwwhfq7q39hnjhzapau9yy6tv
save wallet password to config file: Y(es)/N(o): Y        # please input Y or Yes
save the mnemonic phase properly for future recover:
=======================================================================  
climb work able lock find blind fire cement exotic outdoor eyebrow panther repeat veteran prosper speak identify wolf mind decorate genre arctic bean gauge
=======================================================================
```

</details>

<details>
  <summary><b>Example (creating a new wallet account)</b></summary>

You will get a new wallet account

```shell
# Make sure we are inside the root directory of the resource node
cd rsnode
          
ppd config -w -p
[INFO]2022/02/14 10:19:09 setting.go:122: The config at location ./configs/config.toml does not exist
generating default config file
[INFO]2022/02/14 10:19:09 node.go:67: No P2P key specified in config. Attempting to create one...
Enter password:                           # enter your P2P key password
Enter password again:
[INFO]2022/02/14 10:19:29 setting.go:245: finished changing configuration file  P2PAddress:  stsdsp2p1k70qsfx70s2vkyn737hu3yj50ghrn5yz53dzwx
No wallet key specified in config. Attempting to create one...
Enter wallet nickname: wallet2            # enter your wallet nickname
Enter password:                           # enter your wallet key password
Enter password again:
       
# Leaving the following blank will generate a new wallet account automatically.

input bip39 mnemonic (leave blank to generate a new one)  
input hd-path for the account, default: "m/44'/606'/0'/0/0" :           # Make sure to use the default hd-path
[INFO]2022/02/14 10:19:48 setting.go:245: finished changing configuration file  WalletAddress:  st1cmu0e9qlypg6j2ck8v5gfty6sxj2jszz84h8gf
save wallet password to config file: Y(es)/N(o): Y                      # please input Y or Yes
save the mnemonic phase properly for future recover:
=======================================================================  
interest liberty thrive maple trip fringe nurse deal fresh sport cool hip gate indoor brown mansion what table three wise design master warm apple
=======================================================================
```
</details>

<br>

Option 2: Using <b>`ppd config`</b> and <b>`ppd config accounts`</b> commands


```shell
# Make sure we are inside the root directory of the resource node
cd rsnode
      
# to create config without interactive key creation
ppd config
          
# to create wallet key and p2p key
ppd config accounts -n <Wallet-Name> -p <Wallet-Password> -s
```

<details>
 <summary><b>Example</b></summary>

``` { .yaml .no-copy }
ppd config
[INFO]2022/02/14 12:27:36 setting.go:122: The config at location ./configs/config.toml does not exist
generating default config file

ppd config accounts  -n wallet2 -p 123 -s
generating new p2p key
[INFO]2022/02/14 12:38:17 setting.go:245: finished changing configuration file  P2PAddress:  stsdsp2p10kxlukmq8a50erv8f5s3mfx8endrrnnseeljlq
generated mnemonic is :  
=======================================================================  
climb work able lock find blind fire cement exotic outdoor eyebrow panther repeat veteran prosper speak identify wolf mind decorate genre arctic bean gauge
=======================================================================
```

</details>

<br>

#### Directory structure

After the above command executed successfully, Your `rsnode` folder should include directories and files similar to the following.

``` { .yaml .no-copy }
.
├── accounts
│   ├── st10t5chdnhx6myggwwhfq7q39hnjhzapau9yy6tv.json
│   └── stsds1hez7aewx6srjtrw3064w3qy4dk22uv0cx7jxww.json
│   └── config.toml
└── tmp
  └── logs
      └── stdout.log

```
>   `accounts` folder keeps important account info, including the `Wallet Address`(starting with `st`) and `P2P Address`(starting with `stsds`) of your SDS resource node.
>
>   `configs` folder includes all configurations for this SDS resource node. User may need to modify `configs/config.toml` file to adapt to specific requirements for the Tropos Incentive Testnet
>
>   `tmp` folder is hols the logs and outputs.

<br>

#### Edit configuration file

You will need to edit a few lines in the file `configs/config.toml` to specify the blockchain you want to connect to.

Open config file and make the following modifications:

```shell
nano config/config.toml
```

<br>

- <b>Verify/Change the SDS version</b>

Make sure or change the SDS version section in the `configs/config.toml` file as the following.

```toml
[version]
app_ver = 9
min_app_ver = 9
show = 'v0.9.0'
```

<br>

- <b>Connect to the Stratos-chain testnet</b>

```toml
stratos_chain_url = 'https://rest-tropos.thestratos.org:443' 
```

<br>

- <b>Designate the meta node list</b>

```toml
[[sp_list]]
p2p_address = 'stsds12uufhp4wunhy2n8y5p07xsvy9htnp6zjr40tuw'
p2p_public_key = 'stsdspub1kst98p2642fv8eh8297ppx7xuzu7qjz67s9hjjhxjxs834md7e0sdnut0p'
network_address = '18.130.202.53:8888'
[[sp_list]]
p2p_address = 'stsds1wy6xupax33qksaguga60wcmxpk6uetxt3h5e3e'
p2p_public_key = 'stsdspub1yyfl7ljwc68jh2kuaqmy84hawfkak4fl2sjlpf8t3dd00ed2eqeqxtawdt'
network_address = '35.74.33.155:8888'
[[sp_list]]
p2p_address = 'stsds1nds6cwl67pp7w4sa5ng5c4a5af9hsjknpcymxn'
p2p_public_key = 'stsdspub16mz8w7dygzrsarhh76tnpz0hkqdq44u7usvtnt2qd9qgp8hs8wssx2rrlq'
network_address = '52.13.28.64:8888'
[[sp_list]]
p2p_address = 'stsds1403qtm2t7xscav9vd3vhu0anfh9cg2dl6zx2wg'
p2p_public_key = 'stsdspub1zarvtl2ulqzw3t42dcxeryvlj6yf80jjchvsr3s8ljsn7c25y3hqnetwsy'
network_address = '3.9.152.251:8888'
[[sp_list]]
p2p_address = 'stsds1mr668mxu0lyfysypq88sffurm5skwjvjgxu2xt'
p2p_public_key = 'stsdspub14v8yu6nzem787nfnwvzrfvpc5f7thktsqjts6xp4cy4a2j4rgm7s3ar0jv'
network_address = '35.73.160.68:8888'
[[sp_list]]
p2p_address = 'stsds18xg40a4msgr5ndu2l7k5hv6pudemr9dufcel4w'
p2p_public_key = 'stsdspub1wwhlr2jsfupjsp87ucd3ddy4s5ykcd4khqy3wg7san5kjlw8da5qa7cgcy'
network_address = '18.223.175.117:8888'
[[sp_list]]
p2p_address = 'stsds1ftcvm2h9rjtzlwauxmr67hd5r4hpxqucjawpz6'
p2p_public_key = 'stsdspub1q9rk5zwkzfnnszt5tqg524meeqd9zts0jrjtqk2ly2swm5phlc2qjnlj5c'
network_address = '46.51.251.196:8888'
```

<br>

- <b>Change the value of `chain_id`</b>

The value of `chain_id` is visible on [`Stratos Explorer`](https://explorer-tropos.thestratos.org/) right next to the search bar at the top of the page.

Currently, for the Tropos Incentive Testnet, set `chain_id` as:

```toml
chain_id = 'tropos-5'
```

<br>
 
 - <b>Set the value of `network_address`</b>

Finally, make sure to set the `network_address` to your public IP address and port.

If your resource node is behind a router, you probably need to configure port forwarding on the router, like

```toml
# if your node is behind a router, you probably need to configure port forwarding on the router
port = '18081'
network_address = 'your node external ip' 
```

> 💡 It is not the meta node network_address in `[[sp_list]]` section


---

## Run SDS resource node

<br>

- Acquire test STOS tokens from (`faucet`)

Before manipulating your resource node, you need to acquire some STOS tokens.

You can get test tokens through the faucet API

```shell
curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"your-wallet-address"} ' https://faucet-tropos.thestratos.org/credit
```

> Replace _your-wallet-address_ with the wallet address generated earlier.

<br>

- Start the SDS resource node

After setting up configuration properly, filled your wallet with some tokens, you can now start your resource node.

!!! tip

    `ppd start` must be running in the background at all times so it's recommended to start it in a tmux window by running this command first:

    ```shell
    tmux new -s rsnode
    ```

```shell
# Make sure we are inside the root directory of the resource node
cd rsnode

# start the resource node
ppd start
```


---

## Interact with SDS resource node

<br>

- Interactive operation via `ppd terminal`

In order to interact with the resource node, you need to open A NEW COMMAND-LINE TERMINAL, and enter the root directory of the same resource node.

Then, use `ppd terminal` commands to start the interaction with resource node.

```shell
# Open a new command-line terminal
# Make sure we are inside the root directory of the same resource node
cd rsnode

# Interact with resource node through a set of "ppd terminal" subcommands
ppd terminal
```

> `ppd terminal` needs to be executed in A NEW COMMAND-LINE TERMINAL (called as `ppd terminal` terminal).
>
> All `ppd terminal` [subcommands](../ppd-terminal-subcommands) should be executed in this `ppd terminal` terminal.

  Hereafter, we will use a set of `ppd terminal` [subcommands](../ppd-terminal-subcommands) 
  to communicate with the resource node in `ppd terminal`.

<br>

- <b>Registering the resource node to a meta node</b>

The resource node(PP) should be registered to a meta node(SP) before doing anything else.

In `ppd terminal`, input one of the two following identical subcommands

```shell
rp
  
#or

registerpeer
```

<br>
  
- <b>Uploading/Downloading files without staking</b>

You do not need to stake anything if you just want to upload/download files. 

After registering your resource node(`rp` subcommand), purchase enough `ozone` using the `prepay` subcommand. 

Then, use `put` or `get` subcommands to upload/download files.

```shell
prepay <amount> <fee> [gas]

# example: prepay 1stos 6000000gwei 6000000 
```
  
```shell
put <filepath>

# example: put /home/user/files/movie.mp4
    
get <sdm://account/filehash> [saveAs]  

# example: get sdm://st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4/v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
```

This is a quick way for users to upload/download their files. Resource node can go offline at any time without being punished.
On the other hand, since the resource node is not activated, users will not receive mining rewards(`utros`).

<br>

- Activating the resource node with staking

You can activate your resource node by staking an amount of tokens. 

After it is activated successfully,your resource node starts to receive tasks from meta nodes and thus gaining mining rewards automatically.

```shell
activate <amount> <fee> [gas] 
```

> `amount` is the amount of tokens you want to stake. 1stos = 10^9gwei = 10^18wei.
>
> `fee` is the amount of tokens to pay as a fee for the activation transaction. 10000wei would work. It will use default value if no fee amount is provided.
>
> `gas` is the amount of gas to pay for the transaction. 1000000 would be a safe number. It will use default value if no gas amount is provided.

Example:

```shell
activate 2stos 0.01stos 1000000
```


---

## Other <code>ppd terminal</code> commands

Here we introduce a set of these `ppd terminal` subcommands in brief that you can try in the `ppd terminal` terminal.

Please refer to [ppd terminal subcommands](../ppd-terminal-subcommands) for more details.

<br>

- Check the current status of a resource node

```shell
status
```

<br>

- Update stake of an active resource node

```shell
updateStake <stakeDelta> <fee> [gas] <isIncrStake>  
```

> `stakeDelta` is the absolute amount of difference between the original and the updated stake. It should be a positive valid token, in the unit of `stos`/`gwei`/`wei`.
>
> `isIncrStake` is a boolean flag with `false` for decreasing the original stake and `true` for increasing the original stake.
>
> When a resource node is suspended, use this command to update its state and re-start mining by increasing its stake.

Example:

```shell
updateStake 1stos 1000000gwei 1000000 true
```

The above command will increase 1stos to stake, use 1000000gwei for tx fee and 1000000 for tx gas.

<br>

- Purchase `ozone`

Ozone is the unit of traffic used by SDS. Operations involving network traffic require `ozone` to be executed.

You can purchase ozone with the following command:

```shell
prepay <amount> <fee> [gas]
```

> `amount` is the amount of stos tokens you want to spend to purchase ozone.

<br>

- Query ozone balance of a node's wallet

```shell
getoz <walletAddress>
```

<br>

- Create a new wallet, input password after prompt

```shell
newwallet <walletName> 
```

<br>

- Upload a file

```shell
put <filepath>
```

> `filepath` is the location of the file to upload, starting from your resource node folder. It is better to be an absolute path.

<br>

- Upload a media file for streaming

Streaming is the continuous transmission of audio or video files(media files) from a server to a client.

In order to upload a streaming file, first you need to install a tool `ffmpeg` for transcoding multimedia files.

<br>

In Linux Terminal:

```shell
sudo apt update
sudo apt install ffmpeg
    
# use ffmpeg -version to check its version
ffmpeg -version
```

<br>

In MacOS Terminal:

```shell
brew update
brew install ffmpeg
```

Then, back to the `ppd terminal` terminal, use `putstream` command to upload a media file

```shell
putstream <filepath>
```

> `filepath` is the absolute path of the file to be uploaded, or a relative path starting from the root directory of the resource node.

<br>

- List all uploaded files

  `list` or `ls`

```shell
ls
```

<br>

- Download an uploaded file

```shell
get <sdm://account/filehash> [saveAs]
```

!!! tip

      Every file uploaded to SDS is attributed with a unique file hash.
 
     You can view the file hash for each of your files when you `list` your uploaded files.
  
     Use the optional parameter `saveAs` to rename the file

     The downloaded file will be saved into `download` folder by default under the root directory of the SDS resource node.


<br>

- Delete an uploaded file


```shell
delete <filehash>
```

<br>

- Share(public) an uploaded file

```shell
  sharefile <filehash> <duration> <is_private>
```

> `duration` is time period(in seconds) when the file share expires. Put `0` for unlimited time.
>
> `is_private` is whether the file share should be protected by a password. Put `0` for public file without password, and `1` for private file with a password.
>
> After this command has been executed successfully, SDS will provide a password to this shared file, like `m216`. Please keep this password for future use.

<br>

- List All Shared Files

```shell
allshare
```

<br>

- Download a shared file


```shell
getsharefile <sharelink> [password]
```

> Leave the `password` blank if it's a public shared file.
>
> `sharelink` could be found when your use `allshare` to list all available shared files.
>
> The downloaded files will be saved into the folder `download` by default under the root directory of your resource node.

<br>

- Cancel file share

```shell
cancelshare <shareID> 
```

> `shareID` could be found when your use `allshare` to list all available shared files

<br>

- View resource utilization


```shell
# show the resource utilization monitor
monitor
    
# hide the resource utilization monitor
stopmonitor
```

---

## Check resource node status


There are a set of Restful APIs to check resource node status and Proof of Traffic(PoT) rewards.

You can input the following APIs in an explorer directly. We list some of them here and more details as well as examples can be found in [Stratos Chain REST APIs](../../docs-validator-node/stratos-chain-rest-apis/)

<br>

Check node registration status(`register` module)

<br>

- Query total staking state of all registered resource nodes and meta nodes

```shell
https://rest-tropos.thestratos.org/register/staking
```    

<br>

- Query params of `register` module

```shell
https://rest-tropos.thestratos.org/register/params
```    

<br>

- Get all staking info of a specific owner

```shell
https://rest-tropos.thestratos.org/register/staking/owner/{owner wallet address}
```    

<br>

- Get info of a registered resource node

```shell
https://rest-tropos.thestratos.org/register/resource-node/{resource node network address}
```    

<br>

- Get info of a registered meta node

```shell
https://rest-tropos.thestratos.org/register/meta-node/{meta node network address}
```    

<br>

- Get total number of registered resource nodes

```shell
https://rest-tropos.thestratos.org/register/resource-count
```    

<br>
  
- Get total number of registered meta nodes

```shell
https://rest-tropos.thestratos.org/register/meta-count
```    

<br>

## Check PoT rewards

  - Query PoT rewards of a wallet_address at a specific epoch

```shell
https://rest-tropos.thestratos.org/pot/rewards/wallet/{wallet_address}?epoch={epoch}
```    

<br>

- Query current Pot rewards of a wallet_address

```shell
https://rest-tropos.thestratos.org/pot/rewards/wallet/{wallet_address}
```  

<br>

- Query owner's Pot slashing info at a specific height

```shell
https://rest-tropos.thestratos.org/pot/slashing/{wallet_adress}?height={height}
```  

<br>

- Check SDS `prepay` and `Ozone`(`SDS` module)

- Get a simulated prepay result

```shell
https://rest-tropos.thestratos.org/sds/simulatePrepay/<amount of `wei` to prepay>
```    
<br>

- Get current nozPrice

```shell
https://rest-tropos.thestratos.org/sds/nozPrice
```    
<br>

- Get current nozSupply

```shell
https://rest-tropos.thestratos.org/sds/nozSupply
```    

<br>

---

</br>