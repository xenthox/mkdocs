---
title: SDS RPC client usage
description: List and descriptions of SDS RPC Client commands.
---

[\\]: <> (Add how to install when rpc_client is available in main repo.)


## - `help`

``` { .yaml .no-copy }
Usage:
  rpc_client [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  get         download a file
  getozone    get the ozone of the wallet
  getshared   download a shared file
  help        Help about any command
  list        list files
  listshared  list shared files
  put         upload a file
  putstream   upload a file
  share       share a file from uploaded files
  stopshare   stop sharing a file

Flags:
  -h, --help            help for rpc_client
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)

Use "rpc_client [command] --help" for more information about a command.
```

<br>

---

## - completion

Generates the autocompletion script for rpc_client for the specified shell.

``` { .yaml .no-copy }
Usage:
  rpc_client completion [command]

Available Commands:
  bash        Generate the autocompletion script for bash
  fish        Generate the autocompletion script for fish
  powershell  Generate the autocompletion script for powershell
  zsh         Generate the autocompletion script for zsh

Flags:
  -h, --help   help for completion

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)

Use "rpc_client completion [command] --help" for more information about a command.
```

<br>

### for bash

To load completions in your current shell session:

```shell
source <(rpc_client completion bash)
```

To load completions for every new session, execute once:

!!! tip

    This script depends on the `bash-completion` package.
    
    If it is not installed already, you can install it via your OS's package manager.

- For Linux:

```shell
rpc_client completion bash > /etc/bash_completion.d/rpc_client
```

- For MacOS:

```shell
rpc_client completion bash > $(brew --prefix)/etc/bash_completion.d/rpc_client
```

> You will need to start a new shell for this setup to take effect.


<br>

### for fish

To load completions in your current shell session:

```shell
rpc_client completion fish | source
```

To load completions for every new session, execute once:

```shell
rpc_client completion fish > ~/.config/fish/completions/rpc_client.fish
```

> You will need to start a new shell for this setup to take effect.

<br>

### for powershell

To load completions in your current shell session:

```shell
rpc_client completion powershell | Out-String | Invoke-Expression
```

To load completions for every new session, add the output of the above command to your powershell profile.

<br>

### for zsh

If shell completion is not already enabled in your environment you will need to enable it.  You can execute the following once:

```shell
echo "autoload -U compinit; compinit" >> ~/.zshrc
```

To load completions in your current shell session:

```shell
source <(rpc_client completion zsh); compdef _rpc_client rpc_client
```

To load completions for every new session, execute once:

- For Linux:

```shell
rpc_client completion zsh > "${fpath[1]}/_rpc_client"
```

- For MacOS:

```shell
rpc_client completion zsh > $(brew --prefix)/share/zsh/site-functions/_rpc_client
```

>You will need to start a new shell for this setup to take effect.

<br>

---

## - get

Downloads a file.

``` { .yaml .no-copy }
Usage:
  rpc_client get <sdm://account/filehash> [flags]

Flags:
  -h, --help   help for get

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client get sdm://st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72/v05ahm500bfpivst07iti9krii5llj608mduoo82 \
--url http://127.0.0.1:4444 \
--wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
[INFO] 2023/03/20 11:56:15 main.go:463: - start downloading the file:  sdm://st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72/v05ahm500bfpivst07iti9krii5llj608mduoo82
[INFO] 2023/03/20 11:56:15 main.go:475: - request downloading the file (method: user_requestDownload)
[INFO] 2023/03/20 11:56:25 main.go:576: - downloading is done
```

<br>

---

## - getozone

Queries ozone amount from a wallet address.

``` { .yaml .no-copy }
Usage:
  rpc_client getozone [flags]

Flags:
  -h, --help   help for getozone

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client getozone --url http://127.0.0.1:4444 --wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
[INFO] 2023/03/20 12:01:20 main.go:697: - request ozone balance (method: user_requestGetOzone)
[INFO] 2023/03/20 12:01:20 main.go:719: - received response (return: SUCCESS)
[INFO] 2023/03/20 12:01:20 main.go:721: OZONE balance:  45511.741533115
```

<br>

---

## - getshared

Downloads a shared file.

``` { .yaml .no-copy }
Usage:
  rpc_client getshared <filehash> [flags]

Flags:
  -h, --help   help for getshared

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client getshared v05ahm500bfpivst07iti9krii5llj608mduoo82 \
--url http://127.0.0.1:4444 \
--wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
[INFO] 2023/03/20 12:04:59 main.go:1015: - start downloading the file: v05ahm500bfpivst07iti9krii5llj608mduoo82
[INFO] 2023/03/20 12:04:59 main.go:1022: - request shared file information (method: user_requestGetShared)
```

<br>

---

## - list

Lists all files uploaded by account (wallet).

``` { .yaml .no-copy }

Usage:
  rpc_client list [flags]

Flags:
  -h, --help   help for list

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client list --url http://127.0.0.1:4444 --wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
[INFO] 2023/03/20 12:38:17 main.go:635: - request listing files (method: user_requestList)
[INFO] 2023/03/20 12:38:17 main.go:656: - received response (return: SUCCESS)

File Name            File Hash                                 File Size Create Time
_____________________________________________________________________________________
testsds1-202303142214 v05ahm5000qs4viph3u4biqt67tnels4qpguvjl8   31457280 1678824883
testsds1-202301221144 v05ahm5000u52005ij1osr15t0e7v8df2voh74mo   31457280 1674380707
testsds1-202302240758 v05ahm50010b993c3gdc28nk90bhf4aabdhaujg8   31457280 1677218311
testsds1-202301240119 v05ahm5001j1g84vpu28upa06n8jca40vtqbc6v8   31457280 1674516005
testsds1-202302092312 v05ahm500289uhi4obev6gi3vmmv5s48289vorbg   31457280 1675977162
testsds1-202301240902 v05ahm5002bitsbccaqv5i568vvtsnr1d2avtaa8   31457280 1674543742
testsds1-202301242002 v05ahm5003v4fei49lpj6rb0p1rqpjp52johdkmo   31457280 1674583378
testsds1-202301262228 v05ahm50040vne6f3tuk618h3me8mq4tqor3j6a0   31457280 1674764909
testsds1-202302181817 v05ahm5004b9epvd49uhddmmhs6g5lpfpo0h0eho   31457280 1676737093
testsds1-202303171340 v05ahm5004ogv5a1aa5np1m0u0q98djg0p679ih0   31457280 1679053247
testsds1-202301232120 v05ahm5004pvblrcqgvbu2gtmsl108dlfnsgrg80   31457280 1674501610
testsds1-202303041845 v05ahm5005304r88pucigun0ta3u9uirjkkroiug   31457280 1677948336
testsds1-202302202236 v05ahm5005bgrltqtp6eq57q8n8phsa9krtrvpsg   31457280 1676925407
testsds1-202302282052 v05ahm5005dpvr5p5en3hj62hqpvcjinjhqdavo0   31457280 1677610366
testsds1-202301211702 v05ahm5006gncteg3o7ubfl5u3g20er3jme5gkvg   31457280 1674313350
testsds1-202301251935 v05ahm5006is8d937529te36mpl7so1kh3des3go   31457280 1674668116
testsds1-202301311242 v05ahm5006tgr22ac68leu5gqitaurmqler3vt78   31457280 1675161765
testsds1-202301250041 v05ahm5007mtri1o1tdc9m50d2ruqspir70kejd0   31457280 1674600151
testsds1-202301221100 v05ahm5007n1tgrhmqbb6h9inktvqb6q7vn3in70   31457280 1674378057
testsds1-202301261717 v05ahm5007rcft4obagneqdv1gdnj1266bno8nco   31457280 1674746284
_____________________________________________________________________________________
Total: 20    Page: 0
```

<br>

---

## - listshared

Lists all files uploaded and shared by account (wallet).

``` { .yaml .no-copy }
Usage:
  rpc_client listshared [flags]

Flags:
  -h, --help   help for listshared

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client listshared --url http://127.0.0.1:4444 --wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
[INFO] 2023/03/20 11:50:08 main.go:786: - request listing files (method: user_requestListShare)
[INFO] 2023/03/20 11:50:08 main.go:805: - received response (return: SUCCESS)

File Name            File Hash                                 File Size Link Time  Link Exp   Share ID         Share Link     
________________________________________________________________________________________________________________________________________
testsds1-202301201653 v05ahm500bfpivst07iti9krii5llj608mduoo88   31457280 1674242684 1689794684 2addd621913e02e8 Xpg6U6_2addd621913e02e8
testsds1-202301201451 v05ahm504onf44326v8fupkl96gogma8vs85toro   31457280 1674242858 1689794858 476a8ecddbdd29a6 TS7eFc_476a8ecddbdd29a6
testsds1-202301201553 v05ahm503qb6igipcbmd9q1l1gbqpkiju37c59fo   31457280 1674242786 1689794786 59cf272db46dbbdb UkDPm3_59cf272db46dbbdb
testsds1-202301201853 v05ahm504aftmgtqerm5npdd34bb0u1lcflpgbb8   31457280 1674242825 1689794825 6615100040081212 OQ9ndm_6615100040081212
testsds1-202301200915 v05ahm5041mu88dr71ndp1cag0s8kgqvlr85lce0   31457280 1674242813 1689794813 6789246235828987 qAw0PV_6789246235828987
testsds1-202301200808 v05ahm500l93v2ghdju34h10ehpuj05c6hnqdst8   31457280 1674242748 1689794748 858408741b47a972 LwyBT8_858408741b47a972
testsds1-202301201816 v05ahm501ajcimbjcvrijuuts1n7d939b180o2rg   31457280 1674242769 1689794769 9cbc8114ebf949ae SGF8Uf_9cbc8114ebf949ae
testsds1-202301201730 v05ahm500dpnbihbch8s1qtq29ggfebft18keah8   31457280 1674242726 1689794726 a01893d8931c4639 mAvWSE_a01893d8931c4639
testsds1-202301200944 v05ahm5008ps226hc4etihet1gs2el8g7gvsqqd8   31457280 1674242572 1689794572 b1649c6ddf63a93a ouX59c_b1649c6ddf63a93a
testsds1-202301201822 v05ahm504b5g4nn3ovi5njgu1v1odbhef10p7k68   31457280 1674242841 1689794841 d8a81f46294532d5 ZpCKeK_d8a81f46294532d5
testsds1-202301200705 v05ahm5008hlp1k2278ojgpc1i4t30d21pb9555g   31457280 1674242538 1689794538 ecc6169ec9a95258 5Y4amn_ecc6169ec9a95258
________________________________________________________________________________________________________________________________________
Total: 11       Page: 0
```

<br>

---

## - put

Uploads a file.

``` { .yaml .no-copy }
Usage:
  rpc_client put <filepath> [flags]

Flags:
  -h, --help   help for put

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client put /home/user/tmp/100MB.zip \
--url http://127.0.0.1:4444 \
--wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
[INFO] 2023/03/20 12:14:17 main.go:179: - start uploading the file: /home/user/tmp/100MB.zip
[INFO] 2023/03/20 12:14:17 main.go:187: - request uploading file (method: user_requestUpload)
[INFO] 2023/03/20 12:14:33 main.go:212: - received response (return: UPLOAD_DATA)
[INFO] 2023/03/20 12:14:33 main.go:225: - request upload date (method: user_uploadData)
[INFO] 2023/03/20 12:14:32 main.go:244: - uploading is done
```

<br>

---

## - putstream

Uploads a media file for streaming

Streaming is the continuous transmission of audio or video files(media files) from a server to a client. 

In order to upload a streaming file, first you need to install a tool ffmpeg for transcoding multimedia files.

- In Linux Terminal:

```shell
sudo apt update
sudo apt install ffmpeg

# use ffmpeg -version to check its version
ffmpeg -version
```

- In MacOS Terminal:

```shell
brew update
brew install ffmpeg
```

``` { .yaml .no-copy }
Usage:
  rpc_client putstream <filepath> [flags]

Flags:
  -h, --help   help for putstream

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

Example:

```shell
rpc_client putstream /home/user/tmp/file_example_MP4_640_3MG.mp4 \
--url http://127.0.0.1:4444 \
--wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
...
```


[//]: <> (Returns)
[//]: <> ([WARN] 2023/03/20 12:17:20 handler.go:285: Served user_requestUploadStream [reqid 1 t 11.969µs err the method user_requestUploadStream does not exist/is not available])

<br>

---

## - share

Shares a previously uploaded file.

``` { .yaml .no-copy }
Usage:
  rpc_client share <filehash> <duration> <is_private> [flags]

Flags:
  -h, --help   help for share

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

!!! tip

    `duration` is time period(in seconds) when the file share expires. Put 0 for unlimited time.

    `is_private` is whether the file share should be protected by a password. Put 0 for public file without password, and 1 for private file with a password.

    If `is_private` has been set to '1', SDS will provide a password to this shared file, like `m216`.

Example:

```shell
rpc_client share v05ahm500bfpivst07iti9krii5llj608mduoo82 \
--url http://127.0.0.1:4444 \
--wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Response:

``` { .yaml .no-copy }
[INFO] 2023/03/24 13:00:01 main.go:957: - request sharing file (method: user_requestShare)
[INFO] 2023/03/24 13:00:03 main.go:976: - received response (return: SUCCESS)
ShareId:  78912f5d9bbe939r
ShareLink:  VzW5KW_78912f5d9bbe939r
```

<br>

---

## - stopshare

Stops sharing a previously uploaded file.

```{ .yaml .no-copy }
Usage:
  rpc_client stopshare <ShareID> [flags]

Flags:
  -h, --help   help for stopshare

Global Flags:
  -u, --url string      url to the RPC server, e.g. http://3.24.59.6:8235 (default "http://127.0.0.1:4444")
  -w, --wallet string   wallet address to be used (default: the first wallet in folder ./account/)
```

!!! tip

    You can get the `ShareID` from the [listshared](#-listshared) command.

Example:

```shell
rpc_client stopshare 6789246235828987 \
--url http://127.0.0.1:4444 \
--wallet st14rhrt576gvj6cl46tjn4pctghllmn63tm69e72
```

Result:

``` { .yaml .no-copy }
[INFO] 2023/03/20 12:34:53 main.go:913: - request stop sharing (method: user_requestStopShare)
[INFO] 2023/03/20 12:34:54 main.go:932: - received response (return: SUCCESS)
```

<br>

---

<br>