The API is based on JSON-RPC 2.0 specs. The user works as a client and a resource node provides service as a server.

When the client sends a request to the server by calling to a method, the server sends the response back as the return.

The format of a request message is:
```
{
    "jsonrpc":"2.0",
    "id":1,
    "method":"user_methodName",
    "params":[
        {
            "param1":"valueOfParam1",
            "param2":valueOfParam2,
            ...
        }
    ]
}
```
The format of a response message is:
```
{
    "jsonrpc":"2.0",
    "id":1,
    "result":
        {
            "return":"1",
            "extra_result_object1":value_object1,
            "extra_result_object2":value_object2,
            ...
        }
}
```
When "return" object in "result" is a string encoded negative number, it carries an error. 
```
	"-1":  GENERIC_ERR           
	"-3":  SIGNATURE_FAILURE 
	"-4":  WRONG_FILE_SIZE 
	"-5":  OPERATION_TIME_OUT 
	"-6":  FILE_REQ_FAILURE 
	"-7":  WRONG_INPUT 
	"-8":  WRONG_PP_ADDRESS 
	"-9":  INTERNAL_DATA_FAILURE 
	"-10": INTERNAL_COMM_FAILURE 
	"-11": WRONG_FILE_INFO 
```

# Upload a File

Two methods are used to accomplish uploading a file.
* user_requestUpload: start uploading a file
* user_uploadData: upload a piece of file data 

## user_requestUpload
To request to upload a file. The result could carry the offsets of a piece of the file to be uploaded if the request succeeded.

### Parameters

| name         | type   | comment                            |
|--------------|--------|------------------------------------|
| filename     | string | name of the file                   |
| filesize     | number | size of the file, in byte          |
| filehash     | string | file hash to identify a file [^1]  |
| walletaddr   | string | wallet address of the user account |
| walletpubkey | string | public key of wallet address       |
| signature    | string | signed on a message [^2][^3]       |
  
  

### Returns
| name        | type   | comment                                                                                   |
|-------------|--------|-------------------------------------------------------------------------------------------|
 | return      | string | negative: errors; "1": success and expect for next user_uploadData; other values: invalid |
 | offsetstart | number | the offset of beginning of the piece of file data, inclusive                              |
 | offsetend   | number | the offset of end of the piece of file data, exclusive                                    |

### Example
Request
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "user_requestUpload",
  "params": [
    {
      "filename": "test_16m.bin",
      "filesize": 16777216,
      "filehash": "v05ahm53rcu3b7hrcllpmevfje4vckvriaftg3b0",
      "walletaddr": "st1macvxhdy33kphmwv7kvvk28hpg0xn7nums5klu",
      "walletpubkey": "stsdspub1q0u5yz7t24whqqgy9rsw85m8r2936qqtvty6rwp8zcc688w38zkdwcqm5tz",
      "signature": "1a6c5a67f906553c1d2d0b0c0e856303617dd8b0ab5901321e930fae076c0fdf77d4ba7d84c576d581802f04ea1a8641928fc11ba90a58e430a0a0f09f9515af"
    }
  ]
}
```
Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "return": "1",
    "offsetstart": 0,
    "offsetend": 3500000
  }
}
```

## user_uploadData
Send a piece of file data to server according to the offset previously provided by the server.

### Parameters

| name     | type   | comment                            |
|----------|--------|------------------------------------|
| filehash | string | file hash to identify a file       |
| data     | string | data of the piece of the file [^4] |
### Returns
| name        | type   | comment                                                                                                             |
|-------------|--------|---------------------------------------------------------------------------------------------------------------------|
| return      | string | negative: errors; "1": success and offsets for next user_uploadData; "0": finished uploading; other values: invalid |
| offsetstart | number | the offset of begining of the piece of file data, inclusive                                                         |
| offsetend   | number | the offset of end of the piece of file data, exclusive                                                              |

### Example
Request
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "user_uploadData",
  "params": [
    {
      "filehash": "v05ahm53rcu3b7hrcllpmevfje4vckvriaftg3b0",
      "data": "OQP9h/WvknuPaiWwvJ0MKIjcFs5WGGn4CoYwKLfXsvj0rAjwROuplLOBHjp6N42+/3tjtbdyZlcLk8+BWh+4ZilplkofOsTdGIYoPsbou5 ... "
    }
  ]
}
```
Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "return": "1",
    "offsetstart": 3500000,
    "offsetend": 7000000
  }
}
```

# List Files
List files owned by this account.
## user_requestList
Request listing files owned by the account with the wallet address.
### Parameters
| name       | type   | comment                                          |
|------------|--------|--------------------------------------------------|
| walletaddr | string | wallet address of the user account               |
| page       | number | the list is paginated. Page number start from 0. |

### Returns
| name     | type    | comment                                              |
|----------|---------|------------------------------------------------------|
| return   | string  | negative: errors; "0": success; other value: invalid |
| fileinfo | objects | information for each file                            |
 

In fileinof, these objects are included

| name       | type    | comment                                   |
|------------|---------|-------------------------------------------|
| filehash   | string  | file hash to identify the file [^1]       |
| filesize   | number  | size of the file, in byte                 |
| filename   | string  | name of the file                          |
| createtime | number  | unix epoch time when the file was created |
### Examples
Request
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "user_requestList",
  "params": [
    {
      "walletaddr": "st1macvxhdy33kphmwv7kvvk28hpg0xn7nums5klu",
      "page": 0
    }
  ]
}
```
Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "return": "0",
    "fileinfo": [
      {
        "filehash": "v05ahm51atjqkpte7gnqa94bl3p731odvvdvfvo8",
        "filesize": 200000000,
        "filename": "file1_200M_jan22",
        "createtime": 1674433580
      },
      {
        "filehash": "v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
        "filesize": 10000000,
        "filename": "file2_10M_jan20",
        "createtime": 1674250085
      },
      {
        "filehash": "v05ahm52po4iteumn1v58o3marnruc7l75km9rv8",
        "filesize": 50000000,
        "filename": "file3_50M_jan20",
        "createtime": 1674250338
      },
      {
        "filehash": "v05ahm53ec2f5c9lh92cqapp0mvtfcdphj1deb00",
        "filesize": 100000000,
        "filename": "file1_100M_jan20",
        "createtime": 1674240637
      },
      {
        "filehash": "v05ahm54ia4o2p8vjpluolshiugn1mrgqqhht6o0",
        "filesize": 209715200,
        "filename": "test_200m.bin",
        "createtime": 1674489434
      },
      {
        "filehash": "v05ahm54qtdk0oogho52ujtk5v6rdlpbhumfshmg",
        "filesize": 10000000,
        "filename": "file4_10M_jan20",
        "createtime": 1674253605
      }
    ],
    "totalnumber": 6
  }
}
```

# Download a File
To download a file, there are three methods to be used. 
* user_requestDownload: to start downloading the file 
* user_downloadData: to request a piece of file data
* user_downloadedFileInfo: request server verification of the downloaded file
## user_requestDownload
To start downloading a file. A piece of fire data is carried in the response while successfully started.  
### Parameters
| name         | type   | comment                                              |
|--------------|--------|------------------------------------------------------|
| filehandle   | string | url of the file in sdm:// format [^5]                |
| walletaddr   | string | wallet address of the user account                   |
| walletpubkey | string | public key of wallet address                         |
| signature    | string | signed on message "filehash" + "walletaddr" [^2][^3] |
### Returns
| name        | type   | comment                                                                   |
|-------------|--------|---------------------------------------------------------------------------|
| return      | string | negative: errors; "2": file data provided; other value: invalid           |
| reqid       | string | to identify download instances when multiple download happen at same time |
| offsetstart | number | the offset of beginning of the piece of file data, inclusive              |
| offsetend   | number | the offset of end of the piece of file data, exclusive                    |
| filedata    | string | data of the piece of the file [^4]                                        |
### Example
Request
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "user_requestDownload",
  "params": [
    {
      "filehandle": "sdm://st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u/v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
      "walletaddr": "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u",
      "walletpubkey": "stpub1qdaazld397esglujfxsvwwtd8ygytzqnj5ven52guvvdpvaqdnn52ecsjms",
      "signature": "f598a58bcbf579390e03b3a1ee41c77cff48a714547ae2784124d3c9d873d8a509f615a45c6cad35e94562984e6775a87dbf3ccb2e47af52e23bffc31c698f8901"
    }
  ]
}
```
Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "return": "2",
    "reqid": "c97eafef-401f-49d1-bff3-7ce9eaa2c2dd",
    "offsetstart": 0,
    "offsetend": 3145728,
    "filedata": "JHg2MxpnGfXLcyZc6CpRSRGOHrL1U1jwQWDCnBXi81JmnwR8SrNHcidJZJcYYb+RdXG/bap7RmkXpPmJArEAvrlADlChhG9YOR1JeeKIiS16UCPsYby ... "
  }
}
```
## user_downloadData
After the user handles previous piece of file data, this method is called to get the next piece.
### Parameters
| name     | type   | comment                                                  |
|----------|--------|----------------------------------------------------------|
| filehash | string | file hash to identify a file [^1]                        |
| reqid    | string | the same reqid get from response of user_requestDownload |
### Returns
| name        | type   | comment                                                                                             |
|-------------|--------|-----------------------------------------------------------------------------------------------------|
| return      | string | negative: errors; "2": file data provided; "3": no data and ask for calling user_downloadedFileInfo |
| reqid       | string | to identify download instances when multiple download happen at same time                           |
| offsetstart | number | the offset of beginning of the piece of file data, inclusive                                        |
| offsetend   | number | the offset of end of the piece of file data, exclusive                                              |
| filedata    | string | data of the piece of the file [^4]                                                                  |
### Example
Request
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "method": "user_downloadData",
 "params": [
  {
   "filehash": "v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
   "reqid": "c97eafef-401f-49d1-bff3-7ce9eaa2c2dd"
  }
 ]
}
```
Response
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "result": {
  "return": "2",
  "offsetstart": 3145728,
  "offsetend": 6291456,
  "filedata": "MgWLqUng/xnuoF921Q35Pd2jq9PlWFWpYcLFTTJd1on+HUUJAXwDNbtZZPL1hh+EM0VCSwax ... "
 }
}
```
Another Instance of Response
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "result": {
  "return": "3",
  "reqid": "c97eafef-401f-49d1-bff3-7ce9eaa2c2dd"
 }
}
```
## user_downloadedFileInfo
After the user received all pieces of the file and a response of user_downloadData with return value "3", this method is called to let the server verify file information and finish downloading.
### Parameters
| name     | type    | comment                                                  |
|----------|---------|----------------------------------------------------------|
| filehash | string  | recalculated file hash upon the received file [^1]       |
| filesize | number  | size of the file, in byte                                |
| reqid    | string  | the same reqid get from response of user_requestDownload |
### Returns
| name        | type   | comment                                                           |
|-------------|--------|-------------------------------------------------------------------|
| return      | string | negative: errors; "0": successful finished; other values: invalid |
### Example
Request
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "method": "user_downloadedFileInfo",
 "params": [
  {
   "filehash": "v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
   "filesize": 10000000,
   "reqid": "c97eafef-401f-49d1-bff3-7ce9eaa2c2dd"
  }
 ]
}
```
Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "return": "0"
  }
}
```
# Share a File
## user_requestShare
### Parameters
| name        | type   | comment                             |
|-------------|--------|-------------------------------------|
| filehash    | string | file hash to identify a file [^1]   |
| walletaddr  | string | wallet address of the user account  |
| duration    | number | duration in second sharing the file |
| privateflag | bool   | if the file is private              |
### Returns
| name      | type   | comment                                               |
|-----------|--------|-------------------------------------------------------|
| return    | string | negative: errors; "0": success; other values: invalid |
| shareid   | string | uniq identifier for this sharing                      |
| sharelink | string | link for accessing this shared file                   |
### Example
Request
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "method": "user_requestShare",
 "params": [
  {
   "filehash": "v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
   "walletaddr": "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u",
   "duration": 3600,
   "privateflag": false
  }
 ]
}
```
Response
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "result": {
  "return": "0",
  "shareid": "78a8fe38a826fed4",
  "sharelink": "RHumTB_78a8fe38a826fed4"
 }
}
```
# Stop Sharing a File
## user_requestStopShare
### Parameters
| name       | type   | comment                            |
|------------|--------|------------------------------------|
| walletaddr | string | wallet address of the user account |
| shareid    | string | a uniq identifier for this sharing |
### Returns
| name   | type   | comment                                               |
|--------|--------|-------------------------------------------------------|
| return | string | negative: errors; "0": success; other values: invalid |
### Example
Request
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "method": "user_requestStopShare",
 "params": [
  {
   "walletaddr": "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u",
   "shareid": "78a8fe38a826fed4"
  }
 ]
}
```
Response
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "result": {
  "return": "0"
 }
}
```
# List shared files
## user_requestListShare
### Parameters
| name       | type   | comment                                          |
|------------|--------|--------------------------------------------------|
| walletaddr | string | wallet address of the user account               |
| page       | number | the list is paginated. Page number start from 0. |
### Returns
| name     | type   | comment                                               |
|----------|--------|-------------------------------------------------------|
| return   | string | negative: errors; "0": success; other values: invalid |
| fileinfo | array  | list of shared files                                  |

In fileinof, these objects are included

| name        | type   | comment                                            |
|-------------|--------|----------------------------------------------------|
| filesize    | number | size of the file, in byte                          |
| filehash    | string | file hash to identify the file [^1]                |
| filename    | string | name of the file                                   |
| linktime    | number | unix epoch time when the file started being shared |
| linktimeexp | number | unix epoch time when file share is expired         |
| shareid     | string | a uniq identifier for this sharing                 |
| sharelink   | string | the link for accessing this shared file            |

### Example
Request
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "method": "user_requestListShare",
 "params": [
  {
   "walletaddr": "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u",
   "page": 0
  }
 ]
}
```
Response
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "result": {
  "return": "0",
  "fileinfo": [
   {
    "filehash": "v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
    "filesize": 10000000,
    "filename": "file2_10M_jan20",
    "linktime": 1675051834,
    "linktimeexp": 1675055434,
    "shareid": "23929411ce338824",
    "sharelink": "udixcc_23929411ce338824"
   },
   {
    "filehash": "v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
    "filesize": 10000000,
    "filename": "file2_10M_jan20",
    "linktime": 1675051919,
    "linktimeexp": 1675055519,
    "shareid": "76d88022afb10203",
    "sharelink": "OqhU3X_76d88022afb10203"
   },
   {
    "filehash": "v05ahm51buqelg70rjmcbqtn2qijc7um0ds1oedo",
    "filesize": 10000000,
    "filename": "file2_10M_jan20",
    "linktime": 1675051426,
    "linktimeexp": 1690603426,
    "shareid": "9025a905e28fe791",
    "sharelink": "UfBayn_9025a905e28fe791"
   }
  ],
  "totalnumber": 3
 }
}
```
# Download a Shared File
There are for methods to be used to download a shared file. 
* user_requestGetShared: get information of shared file
* user_requestDownloadShared: similar to user_requestDownload method for downloading a file, start downloading the shared file
* user_downloadData: same method used for downloading a file, downloading a piece of file data  
* user_downloadedFileInfo: same method used for downloading a file, requesting file verification
## user_requestGetShared
### Parameters
| name         | type   | comment                             |
|--------------|--------|-------------------------------------|
| walletaddr   | string | wallet address of the user account  |
| walletpubkey | string | public key of wallet address        |
| sharelink    | string | link for accessing this shared file |

### Returns
| name     | type   | comment                                                                   |
|----------|--------|---------------------------------------------------------------------------|
| return   | string | negative: errors; "4": got shared file info; other values: invalid        |
| reqid    | string | to identify download instances when multiple download happen at same time |
| filehash | string | file hash to identify a file                                              |
### Example
Request
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "user_requestGetShared",
  "params": [
    {
      "walletaddr": "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u",
      "walletpubkey": "stpub1qdaazld397esglujfxsvwwtd8ygytzqnj5ven52guvvdpvaqdnn52ecsjms",
      "sharelink": "j05U4M_25926f167e917c01"
    }
  ]
}
```
Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "return": "4",
    "reqid": "d9bb9abe-32eb-4215-b33b-7d6372127774",
    "filehash": "v05ahm52b88i4lh1epel0cmce6606duatmml4o48"
  }
}
```
## user_requestDownloadShared
### Parameters
| name         | type   | comment                                                   |
|--------------|--------|-----------------------------------------------------------|
| filehash     | string | file hash to identify a file [^1]                         |
| walletaddr   | string | wallet address of the user account                        |
| walletpubkey | string | public key of wallet address                              |
| signature    | string | signed on message "filehash" + "walletaddr"               |
| reqid        | string | the same reqid get from response of user_requestGetShared |
### Returns
| name        | type   | comment                                                                   |
|-------------|--------|---------------------------------------------------------------------------|
| return      | string | negative: errors; "4": got shared file info; other values: invalid        |
| reqid       | string | to identify download instances when multiple download happen at same time |
| offsetstart | number | the offset of beginning of the piece of file data, inclusive              |
| offsetend   | number | the offset of end of the piece of file data, exclusive                    |
| filedata    | string | data of the piece of the file [^4]                                        |
### Example
Request
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "method": "user_requestDownloadShared",
 "params": [
  {
   "filehash": "v05ahm52b88i4lh1epel0cmce6606duatmml4o48",
   "walletaddr": "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u",
   "walletpubkey": "stpub1qdaazld397esglujfxsvwwtd8ygytzqnj5ven52guvvdpvaqdnn52ecsjms",
   "signature": "c45dff7b83a8410cbaea8af8b0d7296634cb7a11f7621e2fa6c6f5537f978b1733ab19ef38659bf0f9c9f9e568827926d26b30540023607f3863dfb944b1633d00",
   "reqid": "d9bb9abe-32eb-4215-b33b-7d6372127774"
  }
 ]
}
```
Response
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "result": {
  "return": "2",
  "reqid": "d9bb9abe-32eb-4215-b33b-7d6372127774",
  "offsetstart": 0,
  "offsetend": 3145728,
  "filedata": "toQwnWMGAYSAn5vyv11p8XYM5FcIgOiavQnczxhaNjsKWvOskMK9QQjkfzzyrBbiCmxUjqk ... "
 }
}
```
## user_requestDownloadData
Please see same method under section _[Download a File](#download-a-file)_
## user_downloadedFileInfo
Please see same method under section _[Download a File](#download-a-file)_

# Get Ozone Balance
## user_requestGetOzone
### Parameters
| name       | type   | comment                            |
|------------|--------|------------------------------------|
| walletaddr | string | wallet address of the user account |
### Returns
| name   | type   | comment                                                            |
|--------|--------|--------------------------------------------------------------------|
| return | string | negative: errors; "0": got shared file info; other values: invalid |
| ozone  | string | value of ozone balance                                             |
### Example
Request
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "method": "user_requestGetOzone",
 "params": [
  {
   "walletaddr": "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u"
  }
 ]
}
```
Response
```json
{
 "jsonrpc": "2.0",
 "id": 1,
 "result": {
  "return": "0",
  "ozone": "5982936846405"
 }
}
```

[^1]: filehash uses MD5
[^2]: the message for signature is \<file_hash> + \<walletaddr>, e.g. the string of "v05ahm52b88i4lh1epel0cmce6606duatmml4o48st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u" when file hash is "v05ahm52b88i4lh1epel0cmce6606duatmml4o48" and wallet address is "st19nn9fnlzkpm3hah3pstz0wq496cehclpru8m3u" 
[^3]: after getting signed, the signature bytes are encoded into hex string.
[^4]: data is encoded using Base64
[^5]: smd://\<owner wallet address>/\<file hash>