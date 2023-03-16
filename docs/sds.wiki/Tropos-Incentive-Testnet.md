# Introduction

The purpose of Tropos Incentive Testnet is to validate all the technical design and network topology that can achieve the goal of our project — to provide a fully decentralized high-efficiency storage. On Incentive testnet we will continue all the testing we employed on testnet; in addition, we will try to ensure the network is functional as in the real world, to build an environment as close to the mainnet as possible.

During the Stratos Incentive Network testing period, we will prove the architecture is working as we designed, verify the network’s performance can reach the expected level, and confirm the rewarding mechanism is performing as described in the token economy whitepaper.

Our goal for the incentive testnet is to ensure we can migrate to the mainnet smoothly and provide powerful decentralized storage to real corporate world users.

<br>

<details>
    <summary><b>Incentive rewards distribution</b></summary>

<br>

Tropos Incentive Testnet will offer validator providers and SDS resource node providers incentive token rewards TROS(1 TROS = 1,000,000,000 utros) for their effort the same way as the mainnet.
<code>The total rewards will be 1% of the mainnet token supply, meaning 1,000,000 STOS.</code>

On the incentive testnet, the distribution will be similar but with some adjustments. All the rewards will not be in the form of testnet STOS; rewards token will be named as TROS which will be issued to the wallet of all participants during the period of incentive testnet. Each epoch will distribute 80 TROS as the mining reward.

<b>💡 Please be advised that you don't need to use your ERC20 STOS to participate in the incentive testnet. Incentive testnet STOS have no value, only the TROS rewards will have value and TROS will be converted to mainnet STOS</b>.

In our <code>token economy whitepaper</code>, we have described that the mining reward per epoch is 80 STOS that will be distributed as the following:

    🏆 Resource node: 60% of total reward
    
    🏆 Meta node: 20% of total reward
    
    🏆 Staking: 20% of total reward

> For resource nodes, the reward will be issued to <code>the top 80% of traffic contributors at the end of each epoch.</code>
>
> For blockchain, all participants who have staking or delegation will be rewarded in each epoch based on their staking or delegation.
>
> For Meta nodes, the reward will be evenly distributed to all meta nodes.

On the incentive testnet, the distribution will be similar but with some adjustments. All the rewards will not be in the form of testnet STOS; rewards token will be named as TROS which will be issued to the wallet of all participants during the period of incentive testnet. Each epoch will distribute 80 TROS as the mining reward.

First, there will be no meta node for public testing. Thus, rewards issued to meta nodes will be changed to a minimum number only for testing purposes.

Second, the staking token for testnet is distributed for free, so it doesn’t reflect the contribution to the network in any sense, we cannot distribute the reward based on staking numbers.

The allocation plan for the reward will also be changed accordingly as follows:

    🏆 Resource node: 100% of total reward
    
    🏆 Meta node: 0% of total reward
    
    🏆 Staking: 0% of total reward

At the end of the incentive testnet, we will calculate the total TROS issued and convert it to mainnet STOS based on the ratio of TROS to Total Incentive STOS, which is 1,000,000 tokens. For example, during the incentive testnet there is a total of 2,000,000 TROS issued; this will make the TROS: STOS converting rate be 2:1, which means that every 2 TROS will withdraw 1 STOS from the mainnet STOS pool.

<br>

</details>

<br>

---

# Topics
This document describes how to connect to Tropos Incentive Testnet and run a Stratos-chain full-node as well as SDS resource node. It offers sample code, implementation tips and reference material.

This guide contains information about the following topics:

<br>
<details>
    <summary><b>Highlight Changes in `tropos-5`</b></summary><blockquote>
  
<br>

After testing and building the binaries v0.9.0, we release the latest version of Stratos-Testnet, with a new chain-id `tropos-5`.

This version covers several breakthrough improvements and significant performance enhancements.
we will highlight the major exciting updates in this section. 

<br>

<details>
    <summary><b>New Changes</b></summary>


- EVM Module

  The integration of EVM module makes Stratos chain compatible with both Ethereum and Cosmos-based ecosystems,
  facilitating Stratos to apply a variety of DApps and introduce more features.

###

- Accounts

  To compatible with various existing Ethereum DApps, the `address` and `type`(from `secp256k1` to `ethsecp256k1`)
  for account have been changed.

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
  
  
  `tropos-5`:
  
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

###

- Token Denom and Decimal

  Most Existing Ethereum DApps have 18 decimals, e.g., 1 ETH is represented by 10e18 of its natural unit
  ( 1Ether = 1,000,000,000,000,000,000wei ). Similarly, most ERC-20 tokens simply follow this standard.
  
  In order to get much greater precision and be consistent with this rule, the previous token denom `ustos`
  (9 decimals) is NOT in use anymore. Instead, a serial of new denominations `stos`(18 decimals), `gwei`(9 decimals)
  and `wei` have been brought into effect. That is,
  
  ```
  1stos = 1,000,000,000,000,000,000wei
  1stos = 1,000,000,000gwei
  1gwei = 1,000,000,000wei
  ```
  
  For simplicity, token amount can be easily represented as `value`+`denom`. In practice, you can enter
  `1stos`, or `1000000000gwei`, or `1000000000000000000wei` to indicate the same amount of tokens.
  
  In `tropos-5`, we keep the same reward issued to Resource Node previously. `utros`(9 decimals) is still the
  only valid denom for incentive reward as before. i.e.,
  
  ```
  1tros = 1,000,000,000utros
  ```
  
###

- Software Upgrade

  To avoid two critical vulnerabilities in the Cosmos-SDK ( [Dragonberry](https://forum.cosmos.network/t/ibc-security-advisory-dragonberry/7702)
  and [Elderflower](https://forum.cosmos.network/t/cosmos-sdk-security-advisory-elderflower/8584), discovered in October 2022),
  we have upgraded Cosmos-SDK to v0.45.9.
  
  Correspondingly, if you prefer to build and compile the binary from source code,
  you need to have [`Go 1.18+`](https://golang.org/doc/install) installed, rather than the former `Go 1.16+`.


###

- SDS Resource Node Maintenance

  Resource Node participates in Stratos Decentralized Storage(SDS) activities and thus earns rewards by providing its
  disk/bandwidth/computation power. It is expected to maintain regular hardware/software updates to prevent from
  unexpected misbehaves like node crashing, loosing connectivity or getting attacks.
  
  In `tropos-5`, `maintenance` functionality has been implemented for users to claim when a maintenance is required, without any worry
  being slashed.
  
    * `maintenance start <duration>` command starts a maintenance activity and switches the node into `maintenance` mode
    for the requested duration (in seconds);
    * While a Resource Node is in `maintenance` mode, it will be opt-out from any download/upload/backup tasks;
    * While a Resource Node is in `maintenance` mode, it will NOT be slashed for its off-line;
    * The maintenance allowance is maxed out after reach 1% up-time per year(around 87h). Then, any maintenance request
      will be rejected;
    * The maintenance allowance will be tracked and be reset every calendar year for all nodes;
    * When using the `maintenance stop` to stop the current maintenance, or the maintenance period is over,
      the node status reverts to `offline` and is ready to restart mining. It acts as usual to earn rewards or be slashed.
  
###

- Restriction of Transaction Fees

  Before `tropos-5`, transaction fees are not strictly required. To reflect scenarios on mainnet, in `tropos-5`,
  transaction fees become a required parameter(s) in each transaction command.
  
  Transaction fees in `tropos-5` follows cosmos-SDK gas calculating rule `fees = gas * gas-prices`. As a result, there are
  two options to add up transaction fees to a command,
  
  * use `--fees=<fees amount>`. e.g., `--fees=10000gwei`.
  
  Or
  * use `--gas=<gas amount>/auto --gas-prices=<gas price>`. e.g., `--gas=100000 --gas-prices=0.1gwei`
  
  Or
  `--gas=auto --gas-prices=0.01gwei`(to use the estimated amount of gas)
  
  In `tropos-5`, a `minimum-gas-prices` is defined by default in `config/app.toml` under the node folder:
  
  ```
  minimum-gas-prices = "1000000000wei"
  ```
  
  Indeed, users can determine their own customized value of `gas-prices` when starting a full node, like
  ```
  ./stchaind start --minimum-gas-prices=0.1gwei
  ```
  
  Note: the value of `minimum-gas-prices` should be always greater than `10,000,000wei`(`0.01gwei`).  When the given 
  `--minimum-gas-prices` is less than this threshold, `10,000,000wei` will be taken instead.

</details>


<br>

<details>
    <summary><b>Migrate Mature Reward to `tropos-5`</b></summary>

####

* For newcomers, just skip this section and follow the instructions in the rest of this guide to set up `tropos-5`.

####

* For existing users, the incentive reward issued previously will be kept with the same private/public key combination.
  However, as mentioned above, the account address will be changed in `tropos-5`.

  The balance in the new `ethsecp256k1` account address will keep the same as that in the old `secp256k1` account address.
  If it is impossible to retrieve the old secp256k1 account public key, resulting in the new `ethsecp256k1` account
  cannot be generated based on the old one, the balance will be transferred to the `mature reward` of the old `secp256k1` account.
  In this case, you need to migrate the reward to your new `ethsecp256k1` account.

  Do not worry about the balance. The only thing you need to do is to check your incentive reward of the old `secp256k1` account
  and withdraw it to the new `ethsecp256k1` account.

  Let's break down this task step by step with an example.
  
  ####
  Suppose you have an existing `secp256k1` account. It was created as
  ```
  name: cosmosUser0
  address: st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
  mnemonic: "ocean jar useless fame slice history retreat banana life wing lesson work amount adult tray zone
  payment window since dash immune grunt army deer"
  ```
  
  ####

  - Backup the previous `secp256k1` account files

    By default, under the node folder `$HOME/.stchaind`, there is `keyring-test` folder which holds the accounts' info.
    It looks like
      ```
      .
      ├── b37acf3e6daa547c8034d27893720a9fcf3eea44.address
      └── cosmosUser0.info
      ```
    Copy this `keyring-test` folder to a secure place.

  ####

  - Download the binary and config files for `tropos-5`. Follow the instructions in the next sections

    until the `Setup a wallet and faucet` step. Next, you need to create an `ethsecp256k1` account based on
    your old `secp256k1` account.

  ####

  - Create(Recover) a new corresponding `ethsecp256k1` account 
    - Using the same mnemonic(recommended)
    
      1. Remove all the files in `keyring-test` folder 
      2. Recover your account with the same mnemonic, i.e.,

     ```
      ./stchaind keys add ethUser0 --recover --chain-id=tropos-5 --keyring-backend=test --hd-path="m/44'/606'/0'/0/0"
       > Enter your bip39 mnemonic
       ocean jar useless fame slice history retreat banana life wing lesson work amount adult tray zone payment window since dash immune grunt army deer

      - name: ethUser0
       address: st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
       pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
       mnemonic: ""
     ```
   
    - Using `stchaind keys` commands
    
      This method uses `stchaind keys` commands to convert your old `secp256k1` account into a 
      new `ethsecp256k1` account in case you don't have the mnemonic words saved. It is an unsafe method  because 
      your private/public key is explicitly print out in the terminal. Please do this on your own hardware machine instead of cloud server. 
      
      1. Keep all the files in `keyring-test` folder
      2. Export `cosmosUser0`
    
      ```
        ./stchaind keys export cosmosUser0 --keyring-backend=test --unsafe --unarmored-hex
      
        WARNING: The private key will be exported as an unarmored hexadecimal string. USE AT YOUR OWN RISK. Continue? [y/N]: y
        18132eae6f96851959d2787aab9f018d9cfe977c619ee951490c6a1c692f2783          ## exported-key-hex
      ```
      3. Delete the old `cosmosUser0` account
      
      ```
         ./stchaind keys delete cosmosUser0 --keyring-backend=test 
      ```

      4. Unsafe-import-eth-key `ethUser0`
      
       ```
         ./stchaind keys unsafe-import-eth-key ethUser0 18132eae6f96851959d2787aab9f018d9cfe977c619ee951490c6a1c692f2783 --keyring-backend=test
      
         Enter passphrase to encrypt your key:   ## if you are using `--keyring-backend=test` in your command, use `12345678` as the passphrase
       ```
       💡 You should use default passphrase `12345678` if you are using `--keyring-backend=test`. 
          You can use your own if you are not using the `test` keyring-backend.
    
       5. Confirm account conversion

       ```
         ./stchaind keys list --keyring-backend=test
      
         - name: ethUser0
           type: local
           address: st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
           pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"AmPEqFHLzLPORjHpTCG/Q6SPYnRae7ehf7cKuzvfRs8v"}'
           mnemonic: ""                                       
       ```
  
  ####

  - Acquire `stos` tokens to activate the newly created `ethUser0` account

    For example, you can `faucet` your account (please replace the value of `address` with your real account address)
      ```
       curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4"}' https://faucet-tropos.thestratos.org/credit
      ```

  ####

  - Start the chain and wait for full synchronization
      ```
       ./stchaind start
      ```

  ####

  - Check the balance in the new `ethsecp256k1` account
    ```
      ./stchaind query bank balances <your new `ethsecp256k1` account address>
    ```
    The response to this command determines if your account has been migrated automatically. For example,

    1. With a balance of `utros`
    
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
     ```http request
      💡 In the above example, the balance includes `233606 utros`. It indicates your account has been migrated automatically. 
         Please skip the following steps, and you can operate the same as before.
     ```

    2. Without a balance of `utros`
    
      ```
      ./stchaind query bank balances st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4
      balances:
      - amount: "10000000000000000000"
        denom: wei
      pagination:
      next_key: null
      total: "0"
     ``` 
    The response does not include a `utros` balance, while you actually had it in the old `secp256k1` account. 
    In another word, if you find your `utros` balance is missing, please follow the next steps to migrate the incentive 
    reward to the new `ethsecp256k1` account manually.

    ####

  - Check the rewards in the old `secp256k1` account
      ```
       https://rest-tropos.thestratos.org/pot/rewards/wallet/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
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
    The above response shows that there are `1696408013053utros` of mature reward held in your old `secp256k1`
    account. Next we will withdraw `100ustros` to the new `ethUser0` account for instance.

    ####

  - Withdraw mature reward from `secp256k1` account to the new corresponding `ethsecp256k1`
    account using `legacy-withdraw` command
      ```
        ./stchaind tx pot legacy-withdraw --target-address=st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4 --from=st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4 --amount=100utros --chain-id=tropos-5 --keyring-backend=test --gas=1000000 --gas-prices=1000000000wei -y
      ```

    ####

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

  ####

  - Check the balance in the old `secp256k1` account again
     ```
       https://rest-tropos.thestratos.org/pot/rewards/wallet/st1am40hqkacscgwvvsjcxzxk49r8cuamgyrcgppg
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

    Similarly, you can withdraw all the mature rewards to the new `ethsecp256k1` account using
    `legacy-withdraw` command. Note that only mature reward can be withdrawn immediately.

    You have migrated your TROS reward to tropos-5 successfully.



</details>


</blockquote></details>

<br>

<details>
    <summary><b>Setup and Run a Stratos-chain Full-node</b></summary><blockquote>

<br>

Stratos blockchain facilitates all decentralized ledger transactions and functionalities, providing settlement services and related financial payment services for network providers and network users in an efficient, fair and transparent manner.

The Stratos-chain full-nodes are dedicated servers with sufficient computing power that participate in block generation cycle. It is necessary in order to be a validator.

In practice, running a full-node only implies running a non-compromised and up-to-date version of the software with low network latency and without downtime. It is encouraged to run a full-node even if you do not plan to be a validator.

The Stratos-chain validator is a full-node that participates in the Stratos Chain block generation cycle and also voting for the validity of a block proposed.


<br>
<details>
    <summary><b>Requirements</b></summary>

<br>
Here are the recommended hardware/software to run a Stratos-chain full-node

###

- <b>Recommended Hardware</b>

        * CPU           i5 (4 cores)
        * RAM           16GB
        * Hard disk     2TB

###

- <b>Software(tested version)</b>

        * Ubuntu 18.04+
        * Go 1.18+ linux/amd64

#

</details>

<details>
    <summary><b>Setup Environment</b></summary>

<br>

In order to run a Stratos-chain full-node, you may need to build `stratos-chain` source code yourself which requires `Go 1.18+`, `git`, `curl` and `make` installed.

This process depends on your operating system.

- <b>Linux Users</b>

  #### 

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

  ####

  To install the required build tools, you can easily [install Xcode from the Mac App Store](https://apps.apple.com/hk/app/xcode/id497799835?l=en&mt=12)

  The best practice to install Go is to use [Homebrew](https://brew.sh/)

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

  ####

  It is possible to build and run the software on Windows. However, we did not test it on Windows completely.
  It may give you unexpected results, or it may require additional setup.

  An alternative option is to install a separate virtual Linux system using [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [VMware Workstation](https://www.vmware.com/ca/products/workstation-player/workstation-player-evaluation.html)

#

</details>

<details>
    <summary><b>Setup a Stratos-chain full-node</b></summary>

<br>

- <b>Create a user account <code>stratos</code>(optional)</b>

  ####

  To create a separated and more secure environment, it is recommended to create a separated user account `stratos` to run your node.

    ```shell
    sudo adduser stratos --home /home/stratos
    ```

  Once the user account `stratos` is created, switch and login the system using `stratos`. You will proceed with the following steps in context of that user.

<br>


- <b>Get release binary executable files</b>

  ####

  There are two ways to get the these binary executables. Please choose either of them based on your working system.
  ###

    - <b>Ubuntu 18.04+</b>

      ####

        - <b>Download binary executables</b>

          ####

          The following binary `stchaind` has been built and ready to be downloaded directly.

          ```shell
            # Make sure we are inside the $HOME folder
            cd $HOME
            wget https://github.com/stratosnet/stratos-chain/releases/download/v0.9.0/stchaind
           ```

          > 💡 This binary is built for Ubuntu 18.04+ amd64. if you have other Linux kernels, please follow Step 1.2 to build your own binary with source code.
          >
          > For ease of use, we recommend saving this binary in your `$HOME` folder. If you are not sure what is your `$HOME` folder, in terminal, use `echo $HOME` to check. In the following instruction, we assume you have entered the `$HOME` folder(use `cd $HOME`)

            - <b>Check the granularity</b>

              #### 

              ```shell
              # Make sure we are inside the $HOME folder and check these two binary executables
              cd $HOME

              # Check granularity
              md5sum stchain*

              ## Expected output
              ## 1843b162a7d2b1f4363938fc73d421e8  stchaind
              ```

            - <b>Add execute permission to this binary </b>

              ####

              ```shell
              # Make sure the file can be executed
              chmod +x stchaind
              ```
            - <b>Add the binary to the search path</b>

              ####

              ```shell
              echo 'export PATH="$HOME:$PATH"' >> ~/.profile
              source ~/.profile
              ```
            <br>

    - <b>Compile the binary executables with source code</b>

      ####

      Before the following steps, please make sure you have `Go 1.18+` installed [link](https://golang.org/doc/install).

        - <b>Build the extracted source code</b>

          ```shell
          git clone https://github.com/stratosnet/stratos-chain.git
          cd stratos-chain
          git checkout tags/v0.9.0
          make build
          ```
          <br>

        - <b>Installing the binary executable</b>

          The binary can be installed to the default $GOPATH/bin folder by running:

          ```shell
          make install
          ```
        <br>

- <b>Get the genesis and configuration files</b>

  ####

    - <b>Initialize your node</b>

      ####

        ```bash
        # Make sure we are inside the home directory
        cd $HOME
      
        # Create folders and initialize the node
        ./stchaind init "<your_node_moniker>"
      
        # ignore the output since you need to download the genesis file 
        ```

      > 💡 You can choose any `your_node_moniker` you prefer. It will be saved in the `config.toml` under the `.stchaind/config/` directory.

- <b>Download the `genesis.json` and `config.toml` files</b>

  ####        

  ```bash
  wget https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/genesis.json
  wget https://raw.githubusercontent.com/stratosnet/stratos-chain-testnet/main/config.toml
  ```

    > 💡 We strongly recommend using this downloaded `config.toml` for v0.9.0, instead of the ones for previous versions to avoid any mismatching. A sample of `config.toml` can be found below
        
    <br>

    <details>
    <summary><b>Example of config.toml</b></summary>
            
      # This is a TOML config file.
      # For more information, see https://github.com/toml-lang/toml

      # NOTE: Any path below can be absolute (e.g. "/var/myawesomeapp/data") or
      # relative to the home directory (e.g. "data"). The home directory is
      # "$HOME/.tendermint" by default, but could be changed via $TMHOME env variable
      # or --home cmd flag.

      #######################################################################
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

            
   </details>
        
   <br>

    - <b>Change `moniker` in the downloaded `config.toml` file</b>

      ####

      Please change your node moniker by modifying the `config.toml` file. Open this file with an editor,
      search `moniker` (usually at Line #16) in the file to find the “moniker” field. Change it to any value you like. It’s your node name that will show on the network.

        ```shell
        # A custom human readable name for this node
        moniker = "<your_node_moniker>"
        ```

    - <b>Move the downloaded `config.toml` and `genesis.json` files to `$HOME/.stchaind/config/` folder. Replace if you already have these files.</b>

      ####

        ```bash
        mv config.toml $HOME/.stchaind/config/
        mv genesis.json $HOME/.stchaind/config/
        ```

#

</details>

<details>
    <summary><b>Directory Structure</b></summary>

<br>
After you finished the above steps, your `$HOME` folder should include the following directories and files.

    ```
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

> 💡 By default, directory `.stchaind` will be created in the `$HOME` folder. The `.stchaind` folder contains the node's configurations and data.

#

  </details>

<details>
    <summary><b>Run your Stratos-chain full-node</b></summary>

<br>

There are three ways to run your Stratos-chain full-node. Please choose ONE of them to start the node.

- <b>`stchaind start` command</b>

  ####

    ```shell
    # Make sure we are inside the home directory
    cd $HOME
    
    # run your node
    ./stchaind start
    
    # Use `Ctrl+c` to stop the node.
    ```

- <b>Run node in background</b>

  ####

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

- <b>Run node as a service</b>

  ####

  All below steps require root privileges

  ####

    - <b>Create the service file</b>

      ####

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

      > 💡 In the [service] section
      > - `User` is your system login username
      > - `ExecStart` designates the absolute path to the binary `stchaind`
      > - `--home` is the absolute path to your node folder.
      > - We used the default values for these variables. If you use a different username, group or folder to hold your node data instead of the default values, please modify these values according to your situations. Make sure the above values are correct.

    - <b>Start your service</b>

      ####

      Once you have successfully created the service, you need to enable and start it by running

      ```shell
      systemctl daemon-reload
      systemctl enable stratos.service
      systemctl start stratos.service
      ```
  ###

    - <b>Service operations</b>

      ###

        - <b>Check the service status</b>

          ####

          ```shell
          systemctl status stratos.service
          ```
        - <b>Check service log</b>

          ####

          ```shell
          journalctl -u stratos.service -f 
      
          # exit with ctrl+c
          ```

        - <b>Stop the service</b>

          ####

          ```shell
          systemctl stop stratos.service
          ```
  #

    </details>


<details>
    <summary><b>Check node status</b></summary>

<br>

Once you start your full-node, it will connect to the peers and start syncing. You can check the status of the node by running the following command

```shell
# Check the status of the node
./stchaind status
```

The output will be similar to

```shell
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

#

</details>


<details>
    <summary><b>Setup a wallet and faucet</b></summary>

<br>

Once the node finishes catch-up, you are ready to operate your node for various transactions(tx) and queries.

> 💡 By default, the following commands can be applied in the node folder(`$HOME`) directory.


- <b>Create a wallet account</b>

  ####

  In order to hold the tokens that you will later delegate to your validator node, or pay staking for your SDS resource node, first, you need to create a local wallet account.

  ###

    - <b>Create a new wallet account</b>

      ####

      To create a new wallet account, type the following command

      ```shell
      ./stchaind keys add <your wallet name> --hd-path="m/44'/606'/0'/0/0" --keyring-backend=<keyring's backend>
      ```

      > 💡 In the testing phase, the `keyring's backend` is `test`, i.e., `--keyring-backend=test`
      >
      > Please select a wallet name that you will easily remember. This name will be used all over the places inside other commands later.

      After creating a new local wallet account, you will get its `address` and `pubkey`.

      In addition, you will have a secret recovery phrase(mnemonic phrase) which can be used to recover an existing wallet account and should be kept secret.

      ###

      <details>
          <summary><b>Example</b></summary>

      ```shell
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

      </details>  

      ###

    - <b>Recover an existing wallet account</b>

      ####

      If you already have a Stratos wallet account, you can recover it by typing the following command

      ```shell
      ./stchaind keys add <your wallet name> --recover --hd-path="m/44'/606'/0'/0/0" --keyring-backend=<keyring's backend> 
      ```
      > 💡 In the testing phase, `--keyring-backend=test`

      <details>
          <summary><b>Example</b></summary>

      ```shell
      ./stchaind keys add myWallet1 --recover --hd-path="m/44'/606'/0'/0/0" --keyring-backend=test  
      ```

        </details>

      ###

- <b>Check your local wallet accounts</b>

  ####

  There are two ways to check your local wallets

  ####

    - <b>Check all local wallet accounts</b>

      ####

       ```shell
       ./stchaind keys list --keyring-backend=<keyring's backend> 
       ```
       <details>
           <summary><b>Example</b></summary>

       ```shell
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
      </details>

      ###

    - <b>Check a specific local wallet account</b>

      ####

      ```shell
      ./stchaind keys show <your wallet name> --keyring-backend=<keyring's backend> 
      ```

      <details>
      <summary><b>Example</b></summary>

      ```shell
      ./stchaind keys show myWallet1 --keyring-backend=test
       - name: myWallet1
         type: local
         address: st16rzhy6wy2rupydps0gem69y2cnus2j09n42ksx
         pubkey: '{"@type":"/stratos.crypto.v1.ethsecp256k1.PubKey","key":"A13YKi3/7p9FsFPTfVgxEO0YK8bnDHmBPfA3ID+k37ET"}'
         mnemonic: "
      ```

      </details>

      <br>


- <b>Check node directory</b>

  ####

  After the above `keys add` command executed, a `keyring-test` folder will be created which contains your wallets' information with their addresses. 
  The `keyring-test` folder looks like

    ```shell
    .
    ├── 32b1a412ac19fbf8105c00d46398e632d902fa16.address
    ├── d0c57269c450f81234307a33bd148ac4f90549e5.address
    ├── myWallet1.info
    └── myWallet.info
    ```

  <br>


- <b>Faucet</b>

  ####

  Faucet will be available at https://faucet-tropos.thestratos.org/ to get test tokens into your wallet.

    ```shell
    // send request to faucet server
    curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"your wallet address"} ' https://faucet-tropos.thestratos.org/credit
    ```
  > 💡
  > * 1stos = 1,000,000,000gwei = 1,000,000,000,000,000,000wei
###


- <b>Check wallet account balance</b>

  ####

  You can query your account info using this command
    ```shell
    ./stchaind query account <your wallet address>
    ```

    <details>
    <summary><b>Example</b></summary>

    ```shell
    ./stchaind query account st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx
    |
    '@type': /cosmos.auth.v1beta1.BaseAccount
    account_number: "1"
    address: st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx
    pub_key: null
    sequence: "0"
    ```

    </details>

  You can query your wallet balances using this command
    ```shell
    ./stchaind query account <your wallet address>
    ```

    <details>
    <summary><b>Example</b></summary>

    ```shell
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

    </details>

  ###

- <b>Try your first tx - `send`</b>

  ####

  This tx command will send an amount of tokens from one wallet address to another

    ```shell
    ./stchaind tx bank send <from address> <to address> <amount> --keyring-backend=<keyring's backend> --chain-id=<current chain-id> --gas=auto --gas-prices=1000000000wei
    ```
  > 💡
  > * The current `chain-id` can be found on the [`Stratos Explorer`](https://explorer-tropos.thestratos.org/) right next to the search bar at the top of the page.
  > * In the testing phase, `--keyring-backend=test`
  > * Make sure your `<from address>` has enough tokens
  > * Please wait for around 7 seconds for block generation after a transaction.

  <details>
    <summary><b>Example</b></summary>

  ####

  Let us assume:

    * `from address`: st1dz20dmhjkuc2tur3amgl8t45w807a640leh8p0

    * `to address`: st123wun5lnwerdrt0mk2uxtusgawpfr228a0sseg

    * `amount`: 10stos

      ```shell
       ./stchaind tx bank send user0 st1d3qtsjyypa639q9kf0wmuf2dn4a7zrnujw84q4 10stos --chain-id=tropos-5  --keyring-backend=test --gas=100000 --gas-prices=1000000000wei -y
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
    </details>
# 

</details>


🔗 <b>[How to become a validator](https://github.com/stratosnet/stratos-chain/wiki/How-to-Become-a-Validator)</b>


</blockquote></details>

<br>

<details>
    <summary><b>Setup and Run SDS Resource Node</b></summary><blockquote>

<br>

The Stratos Decentralized Storage (SDS) network is a scalable, reliable, self-balancing elastic acceleration network. We can simply take it as a decentralized file system suitable for running on general-purpose hardware.

SDS is composed of many Resource Nodes that store data, and a few Meta Nodes that coordinate with each other.

Note that provides their resource(disk/bandwidth/computation power) for SDS is called Resource Node.

###

<details>
    <summary><b>Requirements</b></summary>

<br>

Unlike other projects, Stratos does not require expensive GPUs and high wattage power supplies, but the node needs to provide enough bandwidth and storage capacity to ensure the traffic on the node can reach the reward requirements.

####

- <b>Recommended Hardware</b>

    * CPU           i5 (4 cores)
    * RAM           16GB
    * Hard disk     2TB
    * Bandwidth     100M

####

- <b>Software(tested version)</b>

    * Ubuntu 18.04+
    * Go 1.18+ linux/amd64

###

</details>


<details>
    <summary><b>Keywords</b></summary>
<br>

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

#

</details>

<details>
    <summary><b>Setup Environment</b></summary>

<br>

In order to run an SDS resource node, you need to build SDS source code which requires `Go 1.18+`, `git`, `curl` and `make` installed.
If you have installed them previously, just skip this section. Otherwise, please install them as the following

This process depends on your operating system.

- <b>Linux Users</b>

  #### 

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

  ####

  To install the required build tools, you can easily [install Xcode from the Mac App Store](https://apps.apple.com/hk/app/xcode/id497799835?l=en&mt=12)

  The best practice to install Go is to use [Homebrew](https://brew.sh/)

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

  ####

  It is possible to build and run the software on Windows. However, we did not test it on Windows completely.
  It may give you unexpected results, or it may require additional setup.

  An alternative option is to install a separate virtual Linux system using [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [VMware Workstation](https://www.vmware.com/ca/products/workstation-player/workstation-player-evaluation.html)

#

</details>


<details>
    <summary><b>Setup SDS resource node</b></summary>

<br>

- <b>Compile the binary executables with source code</b>

  ####

  Before the following steps, please make sure you have `Go 1.18+` installed ([link](https://golang.org/doc/install)).

    ```shell
    git clone https://github.com/stratosnet/sds.git
    cd sds
    git checkout tags/v0.9.0
    make build
    ```
  Then you will find the binary executable `ppd` under the folder `target`

  ###

- <b>Installing the binary executable</b>

  ####

  The binary can be installed to the default `$GOPATH/bin` folder by running:

  ```shell
  make install
  ```
  The binary should then be runnable from any folder if you have set up `go env` properly.

  #
  </details>

<details>
    <summary><b>Create SDS resource node</b></summary>

###

- <b>Creating a root directory</b>

  ####

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

  ###

    - <b>Configuring SDS resource node</b>

      ####

      Next, you need to generate the configuration file and its accounts of this resource node. The command `ppd config` will help you to generate necessary configurations.
      ####

        - <b>Generate/Recover local wallet</b>
          ###

          The `ppd config` command consists of several flags or subcommand. Let take a look at its general definition using `ppd config -h`.

          ```shell
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

          If you already have an existing wallet account, you can use the same one here by entering its mnemonic phrase.
          Otherwise, you can generate a new wallet account.

          There are two ways to get the configuration file and its accounts of this resource node.

          ###

            - Option 1: Using <b><code>ppd config -w -p</code></b> command
              >   💡 This command provides an interactive way to communicate with SDS and makes it possible to create the same wallet account for multiple resource nodes.
              >
              >   When asking `input bip39 mnemonic`,
              >
              >>   Input your mnemonic -> recovering an existing wallet account;
              >>
              >>   keep it blank -> generating a new wallet account

              Please refer to the following examples

              ```shell
              # Make sure we are inside the root directory of the resource node
              cd rsnode
        
              # to create config with interactive key creation
              ppd config -w -p
              ```

              <details>
                  <summary><b>Example(recovering an existing wallet account)</b></summary>

              ####    
              You will get the same wallet account as your existing one

              ```shell
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
                    <summary><b>Example(creating a new wallet account)</b></summary>

              ####
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
              generated mnemonic is :  
              =======================================================================  
              interest liberty thrive maple trip fringe nurse deal fresh sport cool hip gate indoor brown mansion what table three wise design master warm apple
              =======================================================================
  
              input hd-path for the account, default: "m/44'/606'/0'/0/0" :           # Make sure to use the default hd-path
              [INFO]2022/02/14 10:19:48 setting.go:245: finished changing configuration file  WalletAddress:  st1cmu0e9qlypg6j2ck8v5gfty6sxj2jszz84h8gf
              save wallet password to config file: Y(es)/N(o): Y                      # please input Y or Yes
              save the mnemonic phase properly for future recover:
              =======================================================================  
              interest liberty thrive maple trip fringe nurse deal fresh sport cool hip gate indoor brown mansion what table three wise design master warm apple
              =======================================================================
              ```

              </details>

              ###

            - Option 2: Using <b>`ppd config`</b> and <b>`ppd config accounts -n <Wallet-Name> -p <Wallet-Password> -s`</b> commands

              `ppd config`: generating a default config file

              `ppd config accounts  -n <Wallet-Name> -p <Wallet-Password> -s`: adding wallet-name and password to the resource node, then save to configurations to`./configs/config.toml`

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

              ```shell
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

          ###

        - <b>Directory structure</b>

          ####

          After the above command executed successfully, Your `rsnode` folder should include directories and files similar to the following.

            ```shell
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

        - <b>Configuration Modification</b>

          ####

          You will need to edit a few lines in the file `configs/config.toml` to specify the blockchain you want to connect to. Please make the following changes:
          ####

            - <b>Make sure/Change the SDS version</b>

              ####
              make sure or change the SDS version section in the `configs/config.toml` file as the following.

              ```toml
              [version]
                app_ver = 9
                min_app_ver = 9
                show = 'v0.9.0'
              ```
              <br>

            - <b>Connect to the Stratos-chain testnet</b>

              ####

              ```toml
              stratos_chain_url = 'https://rest-tropos.thestratos.org:443' 
              ```
              <br>

            - <b>Designate the meta node list</b>

              ####

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

              ####

              The value of `chain_id` is visible on [`Stratos Explorer`](https://explorer-tropos.thestratos.org/) right next to the search bar at the top of the page.
              Currently, for the Tropos Incentive Testnet, set `chain_id` as:

              ```shell
               chain_id = 'tropos-5'
              ```
              <br>

            - <b>Set the value of `network_address`</b>

              ####

              Finally, make sure to set the `network_address` to your public IP address and port.
              If your resource node is behind a router, you probably need to configure port forwarding on the router, like

                ```toml
                # if your node is behind a router, you probably need to configure port forwarding on the router
                port = '18081'
                network_address = 'your node external ip' 
                ```
              > 💡 It is not the meta node network_address in `[[sp_list]]` section

              ##

</details>

<details> 
    <summary><b>Run SDS resource node</b></summary>

<br>

- <b>Acquiring test STOS tokens(`faucet`)</b>

  ####

  Before manipulating your resource node, you need to acquire some STOS tokens.
  You can get test tokens through the faucet API

  ```shell
  curl --header "Content-Type: application/json" --request POST --data '{"denom":"stos","address":"your wallet address"} ' https://faucet-tropos.thestratos.org/credit
  ```

    <br>

- <b>Starting SDS resource node</b>

  ####

  After set up configuration properly, filled your wallet with some tokens, you can now start your resource node.

    - <b>Start resource node</b>

      ####

      It will start the resource node as a daemon in background without interactivity.

      ```shell
      # Make sure we are inside the root directory of the resource node
      cd rsnode
      # start the resource node
      ppd start
      ```
  ##

  </details>


<details>
    <summary><b>Interact with SDS resource node</b></summary>

###

- <b>Interactive operation via `ppd terminal`</b>

  ####

  In order to interact with the resource node, you need to open A NEW COMMAND-LINE TERMINAL, and enter the root directory of the same resource node.
  Then, use `ppd terminal` commands to start the interaction with resource node.

  ```shell
  # Open a new command-line terminal
  # Make sure we are inside the root directory of the same resource node
  cd rsnode
  # Interact with resource node through a set of "ppd terminal" subcommands
  ppd terminal
  ```

  > `ppd terminal` needs to be executed in A NEW COMMAND-LINE TERMINAL(called as `ppd terminal` terminal).
  >
  > All `ppd terminal` [subcommands](https://github.com/stratosnet/sds/wiki/%60ppd-terminal%60--subcommands) should be executed in this `ppd terminal` terminal.

  Hereafter, we will use a set of `ppd terminal` [subcommands](https://github.com/stratosnet/sds/wiki/%60ppd-terminal%60--subcommands) 
  to communicate with the resource node in `ppd terminal`.

  ####

- <b>Registering the resource node to a meta node</b>
  The resource node(PP) should be registered to a meta node(SP) before doing anything else.
  In `ppd terminal`, input one of the two following identical subcommands

    ```shell
    rp
    
    #or
    registerpeer
    ```
  ####

  
- <b>Uploading/Downloading files without staking</b>

  ####

  You do not need to stake anything if you just want to upload/download files. 
  After registering your resource node(`rp` subcommand), purchase enough `ozone` using the `prepay` subcommand. 
  Then, use `put` or `get` subcommands to upload/download files.

    ```shell
    prepay <amount> <fee> [gas]     # prepay 1stos 6000000gwei 6000000 
    ```
  
    ```shell
    put <filepath>     # put config.toml 
    
    get <sdm://account/filehash> [saveAs]  # get sdm://st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4/v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
    ```
  This is a quick way for users to upload/download their files. Resource node can go offline at any time without being punished.
  On the other hand, since the resource node is not activated, users will not receive mining rewards(`utros`).

###

- <b>Activating the resource node with staking</b>

  ####

  You can activate your resource node by staking an amount of tokens. After it is activated successfully,
  your resource node starts to receive tasks from meta nodes and thus gaining mining rewards automatically.

    ```shell
    activate <amount> <fee> [gas] 
    ```
  > `amount` is the amount of tokens you want to stake. 1stos = 10^9gwei = 10^18wei.
  >
  > `fee` is the amount of tokens to pay as a fee for the activation transaction. 10000wei would work. it will use default value if not provide.
  >
  > `gas` is the amount of gas to pay for the transaction. 1000000 would be a safe number. it will use default value if not provide.

  Example:
    ```shell
    >activate 2stos 0.01stos 1000000
    ```

##

</details>


<details>
    <summary><b>Use other <code>ppd terminal</code> subcommands to operate resource node</b></summary>

####

Here we introduce a set of these `ppd terminal` subcommands in brief that you can try in the `ppd terminal` terminal.
Please refer to [ppd terminal subcommands](https://github.com/stratosnet/sds/wiki/%60ppd-terminal%60--subcommands) for more details.

###

- <b>Check the current status of a resource node</b>

  ####

    ```shell
    status
    ```
###

- <b>Update stake of an active resource node</b>

  ####

    ```shell
    updateStake <stakeDelta> <fee> [gas] <isIncrStake>  
    ```
  > `stakeDelta` is the absolute amount of difference between the original and the updated stake. It should be a positive valid
  token, in the unit of `stos`/`gwei`/`wei`.
  >
  > `isIncrStake` is a boolean flag with `false` for decreasing the original stake and `true` for increasing the original stake.
  >
  > When a resource node is suspended, use this command to update its state and re-start mining by increasing its stake.

  Example:
    ```shell
    updateStake 1stos 1000000gwei 1000000 true
    ```
  The above command will increase 1stos to stake, use 1000000gwei for tx fee and 1000000 for tx gas.

###

- <b>Purchase `ozone`</b>

  ####

  Ozone is the unit of traffic used by SDS. Operations involving network traffic require `ozone` to be executed.
  You can purchase ozone with the following command

    ```shell
    prepay <amount> <fee> [gas]
    ```
  > `amount` is the amount of tokens you want to spend to purchase ozone.

###


- <b>Query ozone balance of a node's wallet</b>

  ####

    ```shell
    getoz <walletAddress>
    ```
###

- <b>Create a new wallet, input password after prompt</b>
  ###

    ```shell
    newwallet <walletName> 
    ```

###

- <b>Upload a file</b>

  ###

    ```shell
    put <filepath>
    ```
  > `filepath` is the location of the file to upload, starting from your resource node folder. It is better to be an absolute path.
###

- <b>Upload a media file for streaming</b>

  ###

  Streaming is the continuous transmission of audio or video files(media files) from a server to a client.
  In order to upload a streaming file, first you need to install a tool `ffmpeg` for transcoding multimedia files.

  In Linux Terminal:

    ```shell
    sudo apt update
    sudo apt install ffmpeg
    
    # use ffmpeg -version to check its version
    ffmpeg -version
    ```
  In MacOS Terminal:

    ```shell
    brew update
    brew install ffmpeg
    ```

  ###
  Then, back to the `ppd terminal` terminal, use `putstream` command to upload a media file

    ```shell
    putstream <filepath>
    ```

  > `filepath` is the absolute path of the file to be uploaded, or a relative path starting from the root directory of the resource node.

###

- <b>List all uploaded files</b>

  `list` or `ls`

  ####

    ```shell
    ls
    ```
###


- <b>Download an uploaded file</b>

  ####

    ```shell
    get <sdm://account/filehash> [saveAs]
    ```
  > 💡 Every file uploaded to SDS is attributed with a unique file hash.
  >
  > You can view the file hash for each of your files when you `list` your uploaded files.
  >
  > Use the optional parameter `saveAs` to rename the file
  > 
  > The downloaded file will be saved into `download` folder by default under the root directory of the SDS resource node.


###

- <b>Delete an uploaded file</b>

  ####

    ```shell
    delete <filehash>
    ```

###

- <b>Share(public) an uploaded file</b>

  ####

    ```shell
    sharefile <filehash> <duration> <is_private>
    ```

> `duration` is time period(in seconds) when the file share expires. Put `0` for unlimited time.
>
> `is_private` is whether the file share should be protected by a password. Put `0` for public file without password, and `1` for private file with a password.
>
> After this command has been executed successfully, SDS will provide a password to this shared file, like `m216`. Please keep this password for future use.

####

- <b>List All Shared Files</b>

  ####

    ```shell
    allshare
    ```
####

- <b>Download a shared file</b>

  ####

    ```shell
    getsharefile <sharelink> [password]
    ```
> Leave the `password` blank if it's a public shared file.
>
> `sharelink` could be found when your use `allshare` to list all available shared files.
>
> The downloaded files will be saved into the folder `download` by default under the root directory of your resource node.


###

- <b>Cancel file share</b>

  ####

    ```shell
    cancelshare <shareID> 
    ```
  > `shareID` could be found when your use `allshare` to list all available shared files

###

- <b>View resource utilization</b>

  ####

    ```shell
    # show the resource utilization monitor
    monitor
    
    # hide the resource utilization monitor
    stopmonitor
    ```
###

</details>

<details>
    <summary><b>Check resource node status and PoT rewards</b></summary>

###

There are a set of Restful APIs to check resource node status and Proof of Traffic(PoT) rewards.
You can input the following APIs in an explorer directly. We list some of them here and more details as well as examples can be found in [Stratos Chain REST APIs](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-REST-APIs)

- Check node registration status(`register` module)
  ####

    - <b>Query total staking state of all registered resource nodes and meta nodes</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/register/staking
        ```    
      ####

    - <b>Query params of `register` module</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/register/params
        ```    
      ####

    - <b>Get all staking info of a specific owner</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/register/staking/owner/{owner wallet address}
        ```    
      ###

    - <b>Get info of a registered resource node</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/register/resource-node/{resource node network address}
        ```    
      ###

    - <b>Get info of a registered meta node</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/register/meta-node/{meta node network address}
        ```    
      ###

    - <b>Get total number of registered resource nodes</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/register/resource-count
        ```    
      ###
  
    - <b>Get total number of registered meta nodes</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/register/meta-count
        ```    
      ###

- Check PoT rewards(`PoT` module)
  ####

    - <b>Query PoT rewards of a wallet_address at a specific epoch</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/pot/rewards/wallet/{wallet_address}?epoch={epoch}
        ```    
      ####

    - <b>Query current Pot rewards of a wallet_address</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/pot/rewards/wallet/{wallet_address}
        ```  
      ####

    - <b>Query owner's Pot slashing info at a specific height</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/pot/slashing/{wallet_adress}?height={height}
        ```  
      ####

- Check SDS `prepay` and `Ozone`(`SDS` module)
  ####

    - <b>Get a simulated prepay result</b>

      ####

        ```shell
        https://rest-tropos.thestratos.org/sds/simulatePrepay/<amount of `wei` to prepay>
        ```    
      ####   

    - <b>Get current nozPrice</b>

      ####

      ```shell
      https://rest-tropos.thestratos.org/sds/nozPrice
      ```    
      #### 

    - <b>Get current nozSupply</b>

      ####

      ```shell
      https://rest-tropos.thestratos.org/sds/nozSupply
      ```    
      #### 

<br>

</details>

#

</blockquote></details>

<br>

<details>
    <summary><b>Reference Information</b></summary><blockquote>

<br>

<details>
    <summary><b><code>Stratos-chain</code> Commands</b></summary>

<br>

* ['stchaind' Commands(part1)](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part1))
* ['stchaind' Commands(part2)](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-%60stchaind%60-Commands(part2))

* [REST APIs](https://github.com/stratosnet/stratos-chain/wiki/Stratos-Chain-REST-APIs)

* [How to become a validator](https://github.com/stratosnet/stratos-chain/wiki/How-to-Become-a-Validator)

</details>

<br>

<details>
    <summary><b><code>SDS</code> Commands</b></summary>


<br>

* [`ppd terminal` subcommands](https://github.com/stratosnet/sds/wiki/%60ppd-terminal%60--subcommands)

</details>

<br>

<details>
    <summary><b><code>Stratos-chain</code> Explorers</b></summary>

<br>

* [Stratos Explorer](https://explorer-tropos.thestratos.org/)


</details>

</blockquote></details>

<br>

<details>
    <summary><b>Technical Support</b></summary><blockquote>

<br>

* [Discord](https://discord.com/channels/799573145019219968/799573146020741162)


</blockquote></details>

<br>

---

# Contribution

Thank you for considering to help out with the source code! We welcome contributions
from anyone on the internet, and are grateful for even the smallest of fixes!

If you'd like to contribute to SDS(Stratos Decentralized Storage), please fork, fix, commit and send a pull request
for the maintainers to review and merge into the main code base.

Please make sure your contributions adhere to our coding guidelines:

* Code must adhere to the official Go [formatting](https://golang.org/doc/effective_go.html#formatting)
  guidelines (i.e. uses [gofmt](https://golang.org/cmd/gofmt/)).
* Code must be documented adhering to the official Go [commentary](https://golang.org/doc/effective_go.html#commentary)
  guidelines.
* Pull requests need to be based on and opened against the `dev` branch, PR name should follow `conventional commits`.
* Commit messages should be prefixed with the package(s) they modify.
    * e.g. "pp: make trace configs optional"

<br>

---

# License

Copyright 2023 Stratos

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the [License](https://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

<br>

