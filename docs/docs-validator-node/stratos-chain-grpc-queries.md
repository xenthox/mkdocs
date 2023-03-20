---
title: Stratos Chain stchaind gRPC queries
description: Stratos Network Full-Chain stchaind gRPC queries.
---

Cosmos SDK gRPC definitions have been documented [here](https://crypto.org/docs/resources/cosmos-grpc-docs.html#cosmos.auth.v1beta1.QueryAccountRequest)

## Register Module

<br>

---

### gRPC Gateway

 |Method Name	| Request Type                                                                  | Response Type	                                                              | Description        |
 |-------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------|-------------------|
 |StakeTotal	| QueryTotalStakeRequest  fields: {}                                            | 	QueryTotalStakeResponse fields: {"totalStakes": TotalStakesResponse}         | Query total staking state of all registered resource nodes and meta nodes |
 |Params	| QueryParamsRequest  fields: {}                                                | 	QueryParamsResponse fields: {"params": Params}                               | Get params of Register Module  |
 |ResourceNode	| QueryResourceNodeRequest  fields: {"network_addr": string}                    | 	QueryResourceNodeResponse fields: {"node": ResourceNode}                     | Get info of a registered resource node |
 |MetaNode	| QueryMetaNodeRequest  fields: {"network_addr": string}                        | 	QueryMetaNodeResponse fields: {"node": MetaNode}                             | Get info of a registered meta node   |
 |StakeByNode	| QueryStakeByNodeRequest  fields: {"network_addr": string, query_type: uint32} | 	QueryStakeByNodeResponse fields: {"staking_info": StakingInfo }                 | Get staking info of a specific node |                                  
 |StakeByOwner	| QueryStakeByOwnerRequest  fields: {"owner_addr": string}                      | 	QueryStakeByOwnerResponse fields: {"staking_infos": []StakingInfo, "pagination": cosmos.base.query.v1beta1.PageResponse } | Get all staking info of a specific owner |
 |BondedResourceNodeCount	| QueryBondedResourceNodeCountRequest  fields: {}                                                | 	QueryBondedResourceNodeCountResponse fields: {"number": uint64}                               | Get params of Register Module  |
 |BondedMetaNodeCount	| QueryBondedMetaNodeCountRequest  fields: {}                                                | 	QueryBondedMetaNodeCountResponse fields: {"number": uint64}                               | Get params of Register Module  |

<br>

---

TotalStakesResponse: 

 |Field	| Type                        | Label |
 |------|-------|------------------------------------------------------|
|resource_nodes_total_stake	| cosmos.base.v1beta1.Coin  | 	     |
|meta_nodes_total_stake	| cosmos.base.v1beta1.Coin     |       |
|total_bonded_stake	| cosmos.base.v1beta1.Coin |       |
|total_unbonded_stake	| cosmos.base.v1beta1.Coin | 	     |
|total_unbonding_stake	| cosmos.base.v1beta1.Coin | 	     |

<br>

---

Params:

 |Field	| Type    | Label |
 |-------|-------|------------|
 |bond_denom	| string                      | 	     |
 |unbonding_threashold_time	| google.protobuf.Duration    |       |
 |unbonding_completion_time	| google.protobuf.Duration    |       |
 |max_entries	| uint32    | 	     |

<br>

---

ResourceNode:

|Field	| Type                              | Label |
|-----------------------------|-----------------------------------|------|
|network_address	| string                            | 	    |
|pubkey	| google.protobuf.Any               |      |
|suspend	| bool                              |      |
|status	| cosmos.staking.v1beta1.BondStatus | 	     |
|tokens	| string                            | 	    |
|owner_address	| string                            | 	    |
|description	| Description                       | 	    |
|creation_time	| google.protobuf.Timestamp                            | 	    |
|node_type	| uint32                            | 	    |

<br>

---

MetaNode:

|Field	| Type                              | Label |
|-----------------------------|-----------------------------------|------|
|network_address	| string                            |
|pubkey	| google.protobuf.Any               |      |
|suspend	| bool                              |      |
|status	| cosmos.staking.v1beta1.BondStatus | 	     |
|tokens	| string                            | 	    |
|owner_address	| string                            | 	    |
|description	| Description                       | 	    |
|creation_time	| google.protobuf.Timestamp                            | 	    |

<br>

---

StakingInfo:

|Field	| Type                              | Label |
|-----------------------------|-----------------------------------|------|
|network_address	| string                            | 	    |
|pubkey	| google.protobuf.Any               |      |
|suspend	| bool                              |      |
|status	| cosmos.staking.v1beta1.BondStatus | 	     |
|tokens	| string                            | 	    |
|owner_address	| string                            | 	    |
|description	| Description                       | 	    |
|creation_time	| google.protobuf.Timestamp                            | 	    |
|node_type	| uint32                            | 	    |
|bonded_stake	| cosmos.base.v1beta1.Coin                            | 	    |
|un_bonding_stake	| cosmos.base.v1beta1.Coin                     | 	    |
|un_bonded_stake	| cosmos.base.v1beta1.Coin                           | 	    |

<br>

---

Description:

|Field	| Type                              | Label |
|-----------------------------|-----------------------------------|------|
|moniker	| string                            |
|identity	|string               |      |
|website	| string                             |      |
|security_contact	| string | 	     |
|details	| string                          | 	    |


### - List

List all available grpc queries in Register Module

Request:

``` 
grpcurl -plaintext 127.0.0.1:9090 list stratos.register.v1.Query
```

Response:

``` { .yaml .no-copy }
stratos.register.v1.Query.BondedMetaNodeCount
stratos.register.v1.Query.BondedResourceNodeCount
stratos.register.v1.Query.MetaNode
stratos.register.v1.Query.Params
stratos.register.v1.Query.ResourceNode
stratos.register.v1.Query.StakeByNode
stratos.register.v1.Query.StakeByOwner
stratos.register.v1.Query.StakeTotal

```

<br>

### - StakeTotal

Query total staking state of all registered resource nodes and meta nodes


Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.StakeTotal
```

Response:

```json
{
    "totalStakes": {
        "resourceNodesTotalStake": {
            "denom": "wei",
            "amount": "0"
        },
        "metaNodesTotalStake": {
            "denom": "wei",
            "amount": "100000000000000000000"
        },
        "totalBondedStake": {
            "denom": "wei",
            "amount": "100000000000000000000"
        },
        "totalUnbondedStake": {
            "denom": "wei",
            "amount": "0"
        },
        "totalUnbondingStake": {
            "denom": "wei",
            "amount": "0"
        }
    }
}
```

<br>

### - Params

Get params of Register Module

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.Params
```

Response:

```json
{
 "params": {
  "bondDenom": "wei",
  "unbondingThreasholdTime": "15552000s",
  "unbondingCompletionTime": "1209600s",
  "maxEntries": 16,
  "resourceNodeRegEnabled": true
 }
}
```

<br>

### - ResourceNode

Get info of a registered resource node

Request:

```
grpcurl -plaintext -d '{"network_addr":"stsds1xg2jzku8gptq6sjpjd9zus8qec7a39phcj8md9"}' 127.0.0.1:9090 stratos.register.v1.Query.ResourceNode
```

Response:

```json
{
 "node": {
  "networkAddress": "stsds1xg2jzku8gptq6sjpjd9zus8qec7a39phcj8md9",
  "pubkey": {
   "@type": "/cosmos.crypto.ed25519.PubKey",
   "key": "JZhVcpSQU7rXzlaiqjpSW4u2n6lE/Q+xHG2GFaUkFkA="
  },
  "status": "BOND_STATUS_BONDED",
  "tokens": "2000000000000000000",
  "ownerAddress": "st1vvysda6ylqz2adauqg4djsz4rx6hv6mqv9fepp",
  "description": {
   "moniker": "stsds1xg2jzku8gptq6sjpjd9zus8qec7a39phcj8md9"
  },
  "creationTime": "2023-01-14T00:28:21.819038836Z",
  "nodeType": 4
 }
}
```

<br>


### - MetaNode

Get info of a registered meta node

Request:

```
grpcurl -plaintext -d '{"network_addr":"stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64"}' 127.0.0.1:9090 stratos.register.v1.Query.MetaNode
```

Response:

```json
{
 "node": {
  "networkAddress": "stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64",
  "pubkey": {
   "@type": "/cosmos.crypto.ed25519.PubKey",
   "key": "ltODy8zL5IjJwCutlIexqlBb3GH0+aHZOrpT7f/aKnQ="
  },
  "status": "BOND_STATUS_BONDED",
  "tokens": "100000000000000000000",
  "ownerAddress": "st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x",
  "description": {
   "moniker": "snode://stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64@127.0.0.1:8888"
  },
  "creationTime": "0001-01-01T00:00:00Z"
 }
}
```

<br>

### - StakeByNode

Get staking info of a specific node

Request:

```
grpcurl -plaintext -d '{"network_addr":"stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64","query_type": 1 }' 127.0.0.1:9090 stratos.register.v1.Query.StakeByNode
```

Response:

```json
{
 "stakingInfo": {
  "networkAddress": "stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64",
  "pubkey": {
   "@type": "/cosmos.crypto.ed25519.PubKey",
   "key": "ltODy8zL5IjJwCutlIexqlBb3GH0+aHZOrpT7f/aKnQ="
  },
  "status": "BOND_STATUS_BONDED",
  "tokens": "100000000000000000000",
  "ownerAddress": "st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x",
  "description": {
   "moniker": "snode://stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64@127.0.0.1:8888"
  },
  "creationTime": "0001-01-01T00:00:00Z",
  "bondedStake": {
   "denom": "wei",
   "amount": "100000000000000000000"
  },
  "unBondingStake": {
   "denom": "wei",
   "amount": "0"
  },
  "unBondedStake": {
   "denom": "wei",
   "amount": "0"
  }
 }
}
```

<br>

### - StakeByOwner

Get all staking info of a specific owner

Request:

```
grpcurl -plaintext -d '{"owner_addr":"st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x", "pagination": {"limit":20}}' 127.0.0.1:9090 stratos.register.v1.Query.StakeByOwner
```

Response:

```json
{
 "stakingInfos": [
  {
   "networkAddress": "stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64",
   "pubkey": {
    "@type": "/cosmos.crypto.ed25519.PubKey",
    "key": "ltODy8zL5IjJwCutlIexqlBb3GH0+aHZOrpT7f/aKnQ="
   },
   "status": "BOND_STATUS_BONDED",
   "tokens": "100000000000000000000",
   "ownerAddress": "st1a8ngk4tjvuxneyuvyuy9nvgehkpfa38hm8mp3x",
   "description": {
    "moniker": "snode://stsds1cw8qhgsxddak8hh8gs7veqmy5ku8f8za6qlq64@127.0.0.1:8888"
   },
   "creationTime": "0001-01-01T00:00:00Z",
   "bondedStake": {
    "denom": "wei",
    "amount": "100000000000000000000"
   },
   "unBondingStake": {
    "denom": "wei",
    "amount": "0"
   },
   "unBondedStake": {
    "denom": "wei",
    "amount": "0"
   }
  }
 ],
 "pagination": {
  "total": "1"
 }
}
```

<br>

### - BondedResourceNodeCount

Queries total number of Bonded ResourceNodes

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.BondedResourceNodeCount
```

Response:

```json
{
  "number": "2"
}
```

<br>

### - BondedMetaNodeCount

Queries total number of Bonded MetaNodes

Request:

```
grpcurl -plaintext 127.0.0.1:9090 stratos.register.v1.Query.BondedMetaNodeCount
```

Response:

```json
{
  "number": "4"
}
```

<br>


---

## SDS Module

### gRPC Gateway

|Method Name	| Request Type                                        | Response Type	                                                    | Description        |
 |-----------------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------------------|-------------------|
|Fileupload	| QueryFileUploadRequest  fields: {"file_hash": string} | 	QueryFileUploadResponse fields: {"file_info": FileInfo} | Query uploaded file info by hash |
|Params	| QueryParamsRequest  fields: {}                      | 	QueryParamsResponse fields: {"params": Params}                   | Get params of SDS Module  |
|Prepay	| QueryPrepayRequest  fields: {"acct_addr": string}     | 	QueryPrepayResponse fields: {"balance": string}                  | Query balance of prepayment in volume Poo |


Params:

|Field	| Type    | Label |
 |-------|-------|------------|
|bond_denom	| string                      | 	     |


FileInfo:

|Field	| Type    | Label |
 |-------|-------|------------|
|height	| string                      | 	     |
|reporter	| string    |       |
|string	| string    |       |


### - List

List all available grpc queries in SDS Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 list stratos.sds.v1.Query
```

Response:

``` { .yaml .no-copy }
stratos.sds.v1.Query.Fileupload
stratos.sds.v1.Query.Params
stratos.sds.v1.Query.Prepay
```

<br>

### - Params

Get params of SDS Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.sds.v1.Query.Params
```

Response:

```json
{
  "params": {
    "bondDenom": "wei"
  }
}
```

<br>


### - Prepay

Query balance of prepayment in volume Pool

Request:

```
 grpcurl -plaintext -d '{"acct_addr":"st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw"}' 127.0.0.1:9090 stratos.sds.v1.Query.Prepay
```

Response:

```json
{
  "balance": "9000000000000"
}

```

<br>


### - Fileupload

Query uploaded file info by hash

Request:

```
 grpcurl -plaintext -d '{"file_hash":"001A1FC0B82DD3B0353B59E90388EEA2B73DEECA872955B414EBC99ECD3E3C1F"}' 127.0.0.1:9090 stratos.sds.v1.Query.Fileupload
```

Response:

```json
{
  "fileInfo": {
    "height": "1147",
    "reporter": "stsds14c3em44vlh276cujnr2ez802uyjyeqrrsu9fuh",
    "uploader": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw"
  }
}

```

<br>

***

## POT Module

### gRPC Gateway

 |Method Name	| Request Type                                                                                                                        | Response Type	         | Description         |
 |-------------------------------------------------------------------------------------------------------------------------------------|-----------------|--------------------------------|-------------------|
 |PotRewardsByEpoch	| QueryPotRewardsByEpochRequest  fields: {"epoch":int64, "wallet_address":string, pagination":cosmos.base.query.v1beta1.PageResponse} | 	QueryPotRewardsByEpochResponse fields: {"rewards": []Reward, "height":int64, "pagination":cosmos.base.query.v1beta1.PageRequest} | Query pot reward by epoch      |
 |Params	| QueryParamsRequest  fields: {}                                                                                                      | 	QueryParamsResponse fields: {"params": Params}   | Get params of POT Module       |
 |PotRewardsByOwner	| QueryPotRewardsByOwnerRequest  fields: {"wallet_address": string}                                                                   | 	QueryPotRewardsByOwnerResponse fields: {"rewards": PotRewardByOwner,"height":int64}   | Get pot reward by owner        |
 |PotSlashingByOwner	| QueryPotSlashingByOwnerRequest  fields: {"wallet_address": string}                                                                  | 	QueryPotSlashingByOwnerResponse fields: {"slashing": string, "height":int64}                      | Get pot slashing by owner      |
 |VolumeReport	| QueryVolumeReportRequest  fields: {"epoch":int64}                                                                                   | 	QueryVolumeReportResponse fields: {"report_info": ReportInfo,"height":int64 }       | Get pot volume report by epoch | 

PotRewardByOwner:

|Field	| Type                              | Label |
|-----------------------------|-----------------------------------|------|
|wallet_address	| string                            | 	    |
|MatureTotalReward	| cosmos.base.v1beta1.Coin         |repeated  |
|ImmatureTotalReward	| cosmos.base.v1beta1.Coin     | repeated    |

ReportInfo:

|Field	| Type                    | Label |
|------|-------------------------|-----|
|epoch	| int64                   | 	   |
|reference	| string                  |  |


### - List

List all available grpc queries in POT Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 list stratos.pot.v1.Query
```

Response:

``` { .yaml .no-copy }
stratos.pot.v1.Query.Params
stratos.pot.v1.Query.PotRewardsByEpoch
stratos.pot.v1.Query.PotRewardsByOwner
stratos.pot.v1.Query.PotSlashingByOwner
stratos.pot.v1.Query.VolumeReport
```

<br>

### - Params

Get params of POT Module

Request:

```
 grpcurl -plaintext 127.0.0.1:9090 stratos.pot.v1.Query.Params
```

Response:

```json
{
 "params": {
  "bondDenom": "wei",
  "rewardDenom": "utros",
  "matureEpoch": "2016",
  "miningRewardParams": [
   {
    "totalMinedValveStart": {
     "denom": "utros",
     "amount": "0"
    },
    "totalMinedValveEnd": {
     "denom": "utros",
     "amount": "16819200000000000"
    },
    "miningReward": {
     "denom": "utros",
     "amount": "80000000000"
    },
    "blockChainPercentageInTenThousand": "2000",
    "resourceNodePercentageInTenThousand": "6000",
    "metaNodePercentageInTenThousand": "2000"
   },
   {
    "totalMinedValveStart": {
     "denom": "utros",
     "amount": "16819200000000000"
    },
    "totalMinedValveEnd": {
     "denom": "utros",
     "amount": "25228800000000000"
    },
    "miningReward": {
     "denom": "utros",
     "amount": "40000000000"
    },
    "blockChainPercentageInTenThousand": "2000",
    "resourceNodePercentageInTenThousand": "6200",
    "metaNodePercentageInTenThousand": "1800"
   },
   {
    "totalMinedValveStart": {
     "denom": "utros",
     "amount": "25228800000000000"
    },
    "totalMinedValveEnd": {
     "denom": "utros",
     "amount": "29433600000000000"
    },
    "miningReward": {
     "denom": "utros",
     "amount": "20000000000"
    },
    "blockChainPercentageInTenThousand": "2000",
    "resourceNodePercentageInTenThousand": "6400",
    "metaNodePercentageInTenThousand": "1600"
   },
   {
    "totalMinedValveStart": {
     "denom": "utros",
     "amount": "29433600000000000"
    },
    "totalMinedValveEnd": {
     "denom": "utros",
     "amount": "31536000000000000"
    },
    "miningReward": {
     "denom": "utros",
     "amount": "10000000000"
    },
    "blockChainPercentageInTenThousand": "2000",
    "resourceNodePercentageInTenThousand": "6600",
    "metaNodePercentageInTenThousand": "1400"
   },
   {
    "totalMinedValveStart": {
     "denom": "utros",
     "amount": "31536000000000000"
    },
    "totalMinedValveEnd": {
     "denom": "utros",
     "amount": "32587200000000000"
    },
    "miningReward": {
     "denom": "utros",
     "amount": "5000000000"
    },
    "blockChainPercentageInTenThousand": "2000",
    "resourceNodePercentageInTenThousand": "6800",
    "metaNodePercentageInTenThousand": "1200"
   },
   {
    "totalMinedValveStart": {
     "denom": "utros",
     "amount": "32587200000000000"
    },
    "totalMinedValveEnd": {
     "denom": "utros",
     "amount": "40000000000000000"
    },
    "miningReward": {
     "denom": "utros",
     "amount": "2500000000"
    },
    "blockChainPercentageInTenThousand": "2000",
    "resourceNodePercentageInTenThousand": "7000",
    "metaNodePercentageInTenThousand": "1000"
   }
  ],
  "communityTax": "20000000000000000"
 }
}
```

<br>

### - VolumeReport

Get pot volume report by epoch

Request:

```
grpcurl -plaintext -d '{"epoch": 30 }' 127.0.0.1:9090 stratos.pot.v1.Query.VolumeReport
```

Response:

```json
{
 "reportInfo": {
  "epoch": "30",
  "reference": "report for epoch 3",
  "txHash": "DFA2976152D554DC30FF9C1BC4C497B839F3ED4BEB9ED3E531B6A559E16BF411",
  "reporter": "stsds14c3em44vlh276cujnr2ez802uyjyeqrrsu9fuh"
 },
 "height": "2865"
}
```

<br>

### - PotRewardsByOwner

Get pot reward by owner

Request:

```
grpcurl -plaintext -d '{"wallet_address": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw"} ' 127.0.0.1:9090 stratos.pot.v1.Query.PotRewardsByOwner
```

Response:

```json
{
 "rewards": {
  "walletAddress": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw",
  "ImmatureTotalReward": [
   {
    "denom": "wei",
    "amount": "900"
   },
   {
    "denom": "utros",
    "amount": "144000002130"
   }
  ]
 },
 "height": "2961"
}
```

<br>



### - PotSlashingByOwner

Get pot slashing by owner 

Request:

```
grpcurl -plaintext -d '{"wallet_address": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw"} ' 127.0.0.1:9090 stratos.pot.v1.Query.PotSlashingByOwner
```

Response:

```json
{
 "slashing": "0",
 "height": "2977"
}
```

<br>


### - PotRewardsByEpoch

Query pot reward by epoch 

Request:

```
grpcurl -plaintext -d '{"epoch": 30, "wallet_address": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw" }' 127.0.0.1:9090 stratos.pot.v1.Query.PotRewardsByEpoch
```

Response:

```json
{
 "rewards": [
  {
   "walletAddress": "st16uzr20lx072gexwjuvg94hz3t8y73u4085s9sw",
   "rewardFromMiningPool": [
    {
     "denom": "utros",
     "amount": "48000000710"
    }
   ],
   "rewardFromTrafficPool": [
    {
     "denom": "wei",
     "amount": "300"
    }
   ]
  }
 ],
 "height": "129",
 "pagination": {

 }
}


```


<br>

---

<br>