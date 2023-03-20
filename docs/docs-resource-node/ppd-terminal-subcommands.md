---
title: SDS ppd terminal subcommands
description: List and descriptions of all Stratos SDS resource node ppd terminal subcommands.
---

`ppd terminal` subcommands are used to operate PP(resource) node in `ppd terminal` command-line terminal

## `help`
Show all the `ppd terminal` sub-commands' description.

``` { .yaml .no-copy }
>help

help                                                           show all the commands
wallets                                                        acquire all wallet wallets address
newwallet                                                      create new wallet, input password in prompt
registerpeer                                                   register peer to index node
rp                                                             register peer to index node
activate <amount> <fee> optional<gas>                          send transaction to stchain to become an active PP node
updateStake <stakeDelta> <fee> optional<gas> <isIncrStake>     send transaction to stchain to update active pp's stake
deactivate <fee> optional<gas>                                 send transaction to stchain to stop being an active PP node
startmining                                                    start mining
prepay <amount> <fee> optional<gas>                            prepay stos to get ozone
put <filepath>                                                 upload file, need to consume ozone
putstream <filepath>                                           upload video file for streaming, need to consume ozone (alpha version, encode format config impossible)
list <filename>                                                query uploaded file by self
list <page id>                                                 query all files owned by the wallet, paginated
delete <filehash>                                              delete file
get <sdm://account/filehash> <saveAs>                          download file, need to consume ozone
    e.g: get sdm://st1jn9skjsnxv26mekd8eu8a8aquh34v0m4mwgahg/e2ba7fd2390aad9213f2c60854e2b7728c6217309fcc421de5aacc7d4019a4fe
sharefile <filehash> <duration> <is_private>                   share an uploaded file
allshare                                                       list all shared files
getsharefile <sharelink> <password>                            download a shared file, need to consume ozone
cancelshare <shareID>                                          cancel a shared file
ver                                                            version
monitor                                                        show monitor
stopmonitor                                                    stop monitor
monitortoken                                                   show token for pp monitor service
config  <key> <value>                                          set config key value
getoz <walletAddress> ->password                               get current ozone balance
status                                                         get current resource node status
maintenance start <duration>                                   put the node in maintenance mode for the requested duration (in seconds)
maintenance stop                                               stop the current maintenance
downgradeinfo                                                  get information of last downgrade happened on this pp node
performancemeasure                                             turn on performance measurement log for 60 seconds
```

<br>

---

## `rp` or `registerpeer`
Register a Resource Node to an SP(meta) node.

Example:

```shell
>rp
>[INFO] 2023/01/11 19:10:45 register_new_pp.go:25: get RspRegisterNewPP
[INFO] 2023/01/11 19:10:45 register_new_pp.go:31: get RspRegisterNewPP RES_SUCCESS 
[INFO] 2023/01/11 19:10:45 register_new_pp.go:40: registered as PP successfully, you can deposit by `activate`
```

!!! tip

    After receiving the response `registered as PP successfully, you can deposit by activate`, you can execute the next `activate` command to activate your resource node.

    `rp` or `registerpeer` command may raise an error because of its CPU chips which are not supported by SDS currently. For instance


```shell
# Run on Macs with an Arm-based M1 chip

>rp
[ERROR]2022/04/15 11:38:22 service.go:185: RPC method sds_registerPP crashed: runtime error: index out of range [0] with length 0

```

<br>

---

## `activate`
Send transaction to Stratos chain to become an active Resource Node

```shell
activate <amount> <fee> [gas] 
```

!!! tip

    `amount` is the amount of tokens you want to stake. 1stos = 10^9gwei = 10^18wei.

    `fee` is the amount of tokens to pay as a fee for the activation transaction. 10000wei would work. It will use default value if not provide.

    `gas` is the amount of gas to pay for the transaction. 1000000 would be a safe number. It will use default value if not provide.

Example:

```shell
>activate 2stos 0.01stos 1000000
Request Accepted
[INFO] 2023/01/12 18:49:39 activate.go:66: get RspActivatePP RES_SUCCESS 
[INFO] 2023/01/12 18:49:41 activate.go:83: The activation transaction was broadcast

```

<br>

---

## `startmining`(deprecated)
Resource node will start to receive tasks from meta nodes and thus gain mining rewards.
From SDS v0.7.0, user does not need to use this command any more since resource node will start mining automatically when the status of a resource node is `Mining: ONLINE`.

Example:
```shell
startmining
```

<br>

---

## `status`
Query the current status of a resource node.

```shell
status
```

Example:
```shell
>status
Request Accepted
>[INFO] 2023/01/12 10:57:01 get_pp_status.go:80: *** current node status ***
Activation: Active | Mining: ONLINE | Initial tier: 1 | Ongoing tier: 1 | Weight score: 5000
```

!!! tip

    `Activation` indicates the activation state of a resource node, including `Active`, `Inactive` and `Unbonding`.

    `Mining` indicates the mining state of a resource node, including `ONLINE`, `OFFLINE`, `SUSPEND` and `MAINTENANCE`.

    When a resource node becomes `Mining: SUSPEND` due to its poor performance, user may use `updateStake` command to update its state and re-start mining by increasing its stake.

    Meta Node assigns a `Weight score` to Resource Node depending on its performance. The more `Weight score` a Resource Node has, the more priority it has to be assigned tasks.

<br>

---

## `updateStake`
Update stake of an active resource node.

```shell
updateStake <stakeDelta> <fee> [gas] <isIncrStake> 
```

!!! tip

    `stakeDelta` is the absolute amount of difference between the original and the updated stake. It should be a positive valid token, in the unit of `stos`/`gwei`/`wei`.

    `isIncrStake` is a boolean flag with `false` for decreasing the original stake and `true` for increasing the original stake.

    When a resource node is suspended, use this command to update its state and re-start mining by increasing its stake.

Example:

The following command will increase 1stos to stake, use 10000gwei for tx fees and 1000000 for tx gas.

```shell
>updateStake 1stos 1000000gwei 1000000 true
Request Accepted
>[INFO] 2023/01/12 11:03:34 update_stake.go:27: Sending update stake message to SP! stsds1ttpyyh9p7udalhwcz7sh5f5zfzuhpm0h3c0hzc
[INFO] 2023/01/12 11:03:34 update_stake.go:40: get RspUpdateStakePP RES_SUCCESS 
[INFO] 2023/01/12 11:03:35 update_stake.go:54: The UpdateStake transaction was broadcast
```

!!! tip

    When a resource node is `Mining: SUSPEND` due to poor performance(e.g., frequently offline, task uncompleted, unstable connection, unreachable node, etc.), use this command to update its state and re-start mining by increasing its stake.

<br>

---

## `prepay`
Ozone is the unit of traffic used by SDS. Operations involving network traffic require ozone to be executed.
User can always `prepay` stos to get Ozone any time before uploading/downloading files.

```shell
prepay <amount> <fee> [gas]
```

!!! tip

    `amount` is the amount of tokens you want to spend to purchase ozone.

    The other two parameters are the same as above.

Example:

```shell
>prepay 1stos 6000000gwei 6000000
Request Accepted
>[INFO] 2023/01/12 10:59:07 prepay.go:24: Sending prepay message to SP! st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4
[INFO] 2023/01/12 10:59:07 prepay.go:37: get RspPrepay RES_SUCCESS 
[INFO] 2023/01/12 10:59:09 prepay.go:46: The prepay transaction was broadcast

```

<br>

---

## `getoz`
Query ozone balance of a node's wallet

```shell
getoz <walletAddress>
```

Example:

```shell
>getoz st14d6vt45ws2fz9kgma5wlcfc283xr6pqgp3sklu
input password
password: 
Request Accepted
> [INFO] 2023/01/12 11:01:27 get_wallet_oz.go:42: get GetWalletOz RSP, the current ozone balance of st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4 = 1008215085885, 
```

<br>

---

## `put`
upload a file. It will consume Ozone.

```shell
put <filepath>
```
`filepath` is the location of the file to upload, starting from your resource node folder. It is better to be an absolute path.

Example:

```shell
>put relayd/node1/relayd.toml
[INFO] 2023/01/12 12:00:05 requests.go:178: fileName~~~~~~~~~~~~~~~~~~~~~~~~ relayd.toml
[INFO] 2023/01/12 12:00:05 requests.go:184: fileHash~~~~~~~~~~~~~~~~~~~~~~ v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
Request Accepted
>[INFO] 2023/01/12 12:00:07 upload_slice.go:341: fileHash: v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg  uploaded：100.00 %
[INFO] 2023/01/12 12:00:07 print.go:20: ####################################################################################################
```

<br>

---

## `putstream`
Upload a media file for streaming

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

<br>

Then, use `putstream` command to upload a media file

```shell
putstream <filepath>
```

> `filepath` is the absolute path of the file to be uploaded, or a relative path starting from the root directory of the resource node.

Example:

```shell
putstream example_01.mp4
```

<br>

---

## `list` or `ls`
List all uploaded files

```shell
ls
```

<br>

---

## `list <filename>`
Query a specific uploaded file by name

```shell
ls filename
```

Example:

```shell
>ls relayd.toml
Request Accepted
>[INFO] 2023/01/12 12:01:58 find_file.go:71: _______________________________
[INFO] 2023/01/12 12:01:58 find_file.go:76: File name: relayd.toml
[INFO] 2023/01/12 12:01:58 find_file.go:77: File hash: v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
[INFO] 2023/01/12 12:01:58 find_file.go:79: CreateTime : 1673542807
[INFO] 2023/01/12 12:01:58 find_file.go:88: ===============================
[INFO] 2023/01/12 12:01:58 find_file.go:89: Total: 1  Page: 0
```

<br>

---

## `list <page_id>`
Query all files owned, paginated by 20 items per page by default, starting with page 0.

```shell
ls page_id
```

Example:

```shell
>ls 0
Request Accepted
>[INFO] 2023/01/12 12:03:07 find_file.go:71: _______________________________
[INFO] 2023/01/12 12:03:07 find_file.go:76: File name: relayd.toml
[INFO] 2023/01/12 12:03:07 find_file.go:77: File hash: v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
[INFO] 2023/01/12 12:03:07 find_file.go:79: CreateTime : 1673542807
[INFO] 2023/01/12 12:03:07 find_file.go:88: ===============================
[INFO] 2023/01/12 12:03:07 find_file.go:89: Total: 1  Page: 0
```

<br>

---

## `get`
Download an uploaded file

```shell
get <sdm://account/filehash> [saveAs]
```

!!! tip

    Every file uploaded to SDS is attributed with a unique file hash.

    View the file hash for each of your files when you `list` your uploaded files.

    Use the optional parameter `saveAs` to rename the file.

    The downloaded files will be saved into the folder `download` by default under the root directory of your resource node, like

``` { .yaml .no-copy }
.
├── accounts
│   ├── st1la7aj36gk88puz3026t3mkqfeu4q8paj3rs4gk.json
│   └── stsds1x5cse46ru8phd0435ncm04vh4mzj8ntp8rtpu4.json
├── configs
│   └── config.toml
├── download                # default downloading dir
│   └── README.md       # downloaded file
├── peers
│   └── pp-list
└── tmp
    └── logs

```

> The `download` folder is defined by `download_path` in the file `configs/config.toml` under the root directory of your resource node.

Example:

```shell
>get sdm://st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4/v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
Request Accepted
>[INFO] 2023/01/12 12:47:31 query_file_info.go:253: get，RspFileStorageInfo

[INFO] 2023/01/12 12:47:31 download_slice.go:391: download starts: 
[INFO] 2023/01/12 12:47:31 download_task.go:431: downloaded：100.00 % 

[INFO] 2023/01/12 12:47:31 print.go:20: ####################################################################################################
```

<br>

---

## `delete`
Delete an uploaded file

```shell
delete <filehash>
```

Example:

```shell
>delete v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
Request Accepted
>[INFO] 2023/01/12 12:04:51 delete_file.go:36: delete success
```

<br>

---

## `sharefile`
Share(public) an uploaded file

```shell
sharefile <filehash> <duration> <is_private>
```

!!! tip

    `duration` is time period(in seconds) when the file share expires. Put `0` for unlimited time.

    `is_private` is whether the file share should be protected by a password. Put `0` for public file without password, and `1` for private file with a password.

    After this command has been executed successfully, SDS will provide a password to this shared file, like `m216`.Please keep this password for future use.

Example:

```shell
>sharefile v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg 0 1
Request Accepted
>[INFO] 2023/01/12 12:53:04 share.go:131: ShareId 348f79a5a0963a56
[INFO] 2023/01/12 12:53:04 share.go:132: ShareLink hFu8ce_348f79a5a0963a56
[INFO] 2023/01/12 12:53:04 share.go:133: SharePassword m216
```

<br>

---

## `allshare`
List All Shared Files.

```shell
allshare
```

Example:
```shell
>allshare
Request Accepted
> [INFO] 2023/01/12 12:30:21 share.go:75: _______________________________
[INFO] 2023/01/12 12:30:21 share.go:76: file_name: relayd.toml
[INFO] 2023/01/12 12:30:21 share.go:77: file_hash: v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg
[INFO] 2023/01/12 12:30:21 share.go:78: file_size: 676
[INFO] 2023/01/12 12:30:21 share.go:79: link_time: 1673544576
[INFO] 2023/01/12 12:30:21 share.go:80: link_time_exp: 1689096576
[INFO] 2023/01/12 12:30:21 share.go:81: ShareId: e9546b33e3d63285
[INFO] 2023/01/12 12:30:21 share.go:82: ShareLink: 4RREV0_e9546b33e3d63285
```

<br>

---

## `getsharefile`
Download a shared file.

```shell
getsharefile <sharelink> [password]
```

!!! tip

    Leave the `password` blank if it's a public shared file.

    `sharelink` could be found when your use `allshare` to list all available shared files.

    The downloaded files will be saved into the folder `download` by default under the root directory of your resource node.

Example:

```shell
>getsharefile 4RREV0_e9546b33e3d63285
Request Accepted
> [INFO] 2023/01/12 12:31:38 share.go:216: get RspGetShareFile RES_SUCCESS request success
[INFO] 2023/01/12 12:31:38 share.go:222: FileInfo: [file_size:676  file_hash:"v05ahm51l6v6tm2vqc682b9sicom61fgkoqdl0pg"  file_name:"relayd.toml"  create_time:1673542807  owner_wallet_address:"st172v4u8ysfgaphjs8uyy0svvc6d6tzl6gp07kn4"  share_link:"4RREV0_e9546b33e3d63285"]
[INFO] 2023/01/12 12:31:38 query_file_info.go:253: get，RspFileStorageInfo

[INFO] 2023/01/12 12:31:38 download_slice.go:391: download starts: 
[INFO] 2023/01/12 12:31:39 download_task.go:431: downloaded：100.00 % 

[INFO] 2023/01/12 12:31:39 print.go:20: ####################################################################################################
```

<br>

---

## `cancelshare`
Cancel file share

```shell
cancelshare <shareID>
```
> `shareID` could be found when your use `allshare` to list all available shared files

Example:

```shell
>cancelshare e9546b33e3d63285
Request Accepted
>[INFO] 2023/01/12 12:52:20 share.go:172: cancel share success: e9546b33e3d63285
```

<br>

---

## `monitor`
View resource utilization.

```shell
monitor
```

Example:

```shell
# show the resource utilization monitor
>monitor
[INFO] 2023/01/12 11:13:09 sys.go:143: __________________________________________________________________________
[INFO] 2023/01/12 11:13:09 sys.go:144:         Mem         : 15967 MB  Free: 9497 MB Used:6127 Usage:38.372995%
[INFO] 2023/01/12 11:13:09 sys.go:149:         CPU          : Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz   1 cores 
[INFO] 2023/01/12 11:13:09 sys.go:149:         CPU          : Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz   1 cores 
[INFO] 2023/01/12 11:13:09 sys.go:149:         CPU          : Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz   1 cores 
[INFO] 2023/01/12 11:13:09 sys.go:149:         CPU          : Intel(R) Core(TM) i7-10510U CPU @ 1.80GHz   1 cores 
[INFO] 2023/01/12 11:13:09 sys.go:158:         CPU Used    : 93.367347% 
[INFO] 2023/01/12 11:13:09 sys.go:161:         HD          : 343 GB  Free: 160 GB Usage:51.431241% Path:/home/hong/stratos/sds-tropos5/sds/example/network/node1
[INFO] 2023/01/12 11:13:09 sys.go:201:         Upload      : 0.000000 MB/s 
[INFO] 2023/01/12 11:13:09 sys.go:202:         Download    : 0.000000 MB/s 
[INFO] 2023/01/12 11:13:09 sys.go:203: __________________________________________________________________________
```

<br>

---

## `stopmonitor`
Hide the resource utilization monitor

```shell
stopmonitor
```

<br>

---

## `monitortoken`
show token for Resource Node monitor service.

```shell
> monitortoken
Monitor token is: dddd608cc1c0efaf6b87267088dbc4b099b0db0758f476e625763580991a086c
```
The returned token can be used for logging in to resource node monitor.

<br>

---

## `wallets`
acquire all wallets' addresses

```shell
wallets
```

Example:

```shell
>wallets
[INFO] 2023/01/12 11:18:07 account.go:109: st16v5pcrj9m6fgmwm7w0fn6dyxe8er3dk2nqqrhf
[INFO] 2023/01/12 11:18:07 account.go:109: st1sqzsk8mplv5248gx6dddzzxweqvew8rtst96fx
```

<br>

---

## `ver`
show current SDS version

Example:

```shell
>ver
version: v0.9.0
```

<br>

---

## `deactivate`
send transaction to Stratos-chain to stop being an active resource node

```shell
deactivate <fee> <gas>
```

Example:

```shell
>deactivate 10000000gwei 1000000
```

<br>

---

## `newwallet`
create a new wallet or recover an existing wallet, input password after prompt

```shell
newwallet <wallet_name>
```

Example:

```shell
>newwallet mySecondWallet
Enter wallet nickname: mySecondWallet
Enter password: 
Enter password again: 
input bip39 mnemonic (leave blank to generate a new one)
input hd-path for the account, default: "m/44'/606'/0'/0/0" : 
save the mnemonic phase properly for future recovery: 
=======================================================================  
mother bracket treat warfare become win ivory harvest course reform theory issue group super alpha library despair sustain orient shrug lizard bulk mistake magnet
======================================================================= 

[INFO] 2023/01/12 11:17:00 setup_wallet.go:61: Wallet st16v5pcrj9m6fgmwm7w0fn6dyxe8er3dk2nqqrhf has been generated successfully
Do you want to use this wallet as your node wallet: Y(es)/N(o): y
[INFO] 2023/01/12 11:17:03 setting.go:291: finished changing configuration file  wallet_address:  st16v5pcrj9m6fgmwm7w0fn6dyxe8er3dk2nqqrhf
```

<br>

---

## `config`

Set config key value pairs in the file `configs/config.toml`, separated by one space(note: no quotes for string input).

```shell
config <key> <value>
```
Example:

```shell
config wallet_password stratos
```

<br>

---

## `maintenance start`
Claim a maintenance. Put the resource node in maintenance mode for the requested duration (in seconds).

```shell
maintenance start <duration> 
```

!!! tip

    * `maintenance start <duration>` command starts a maintenance activity and switches the node into `maintenance` mode for the requested duration (in seconds);
    * While a Resource Node is in `maintenance` mode, it will be opt-out from any download/upload/backup tasks;
    * While a Resource Node is in `maintenance` mode, it will NOT be slashed for its off-line;
    * The maintenance allowance is maxed out after reach 1% up-time per year(around 87h). Then, any maintenance request will be rejected;
    * The maintenance allowance will be tracked and be reset every calendar year for all nodes;
    * When using the `maintenance stop` to stop the current maintenance, or the maintenance period is over, the node status reverts to `offline` and is ready to restart mining. It acts as usual to earn rewards or be slashed.

Example:

```shell
>maintenance start 600
Request Accepted
>[INFO] 2023/01/12 12:57:21 maintenance.go:19: Sending maintenance start request to SP!

>status
Request Accepted
> [INFO] 2023/01/12 12:58:19 get_pp_status.go:80: *** current node status ***
Activation: Active | Mining: MAINTENANCE | Initial tier: 1 | Ongoing tier: 1 | Weight score: 5020
```
Note: `Mining: MAINTENANCE` indicates that this node is in `maintenance` mode.

<br>

---

## `maintenance stop`
stop the current maintenance.

```shell
>maintenance stop
Request Accepted
>[INFO] 2023/01/12 12:58:35 maintenance.go:26: Sending maintenance stop request to SP!
[INFO] 2023/01/12 12:58:38 register_to_sp.go:104: start mining

>status
Request Accepted
> [INFO] 2023/01/12 12:59:22 get_pp_status.go:80: *** current node status ***
Activation: Active | Mining: ONLINE | Initial tier: 1 | Ongoing tier: 1 | Weight score: 5020
```

<br>

---

## `downgradeinfo`
Get the information of last downgrade happened on this resource node.

```shell
> downgradeinfo
Request Accepted
> [INFO] 2023/01/12 10:43:12 get_pp_downgrade_info.go:24: PP downgrade happened at: 111624 (heights) ago, 
at SP node stsds15sya4n40da64z6n6hvk0p9f7sx72hqpjjnrf9y, score decreased by 9999

```

<br>

---

## `performancemeasure`
Turn on performance measurement log for 60 seconds.

```shell
> performancemeasure
```

You can exit the `ppd terminal` command-line terminal by typing `exit` and leave the `ppd start` terminal to run the resource node in background.

<br>

---

<br>