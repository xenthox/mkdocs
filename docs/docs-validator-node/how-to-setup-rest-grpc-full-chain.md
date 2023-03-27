---
title: HowTo Setup Full-Chain as REST or gRPC
description: Guide on how to configure Full-Chain node as REST or gRPC server.
---

In order to work properly, a SDS node needs to generate, sign and broadcast transaction to the Stratos chain through a Full-Chain node.

Stratos provides a public endpoint at _rest-tropos.thestratos.org_ for cases when there's only a SDS node running and a Full-Chain is not available.

However, this setup is particularly useful if you run both a Full-Chain and a SDS node because you can setup your own REST/gRPC endpoint in your Full-Chain node so it can be used for your SDS node. This means that you will have improved performance, faster responses and you won't be dependent on the public endpoint which may get overloaded at times.

This setup is also useful for server farms running multiple SDS nodes.

!!! tip

    Currently, in Tropos-5 incentive testnet, SDS nodes are using REST API but this will be changed to gRPC API once mainnet is launched.

<br>

## REST Setup

To enable REST API in your Full-Chain Node, edit the app.toml file:

```shell
nano ~/.stchaind/config/app.toml
```

And set `enable` to `true` and (optionally) edit the `address` variables. You can change the ip to listen only on local or lan requests and/or you can change the default port.

```toml
###############################################################################
###                           API Configuration                             ###
###############################################################################

# Enable defines if the API server should be enabled.
enable = true

# Address defines the API server to listen on.
address = "tcp://0.0.0.0:1317"
``` 

Usage example:

```json
curl http://localhost:1317/bank/balances/st1pgzvq9p5gyxu7gpe8l33g8nzq0xsfyeaeww3ru

{"height":"1062014","result":[
  {
    "denom": "utros",
    "amount": "166304372501"
  },
  {
    "denom": "wei",
    "amount": "1178011342927397401552"
  }
```

You can see all the REST queries [here](../stratos-chain-rest-apis).

To use your Full-Chain REST API for your SDS node, edit this file:

```shell
nano ~/rsnode/configs/config.toml
```

and edit this line with your REST url:

```toml
stratos_chain_url = 'http://127.0.0.1:1317'
```


<br>

## gRPC Setup

gRPC will be used by SDS nodes in mainnet. To enable or configure gRPC for your Full-Chain node, edit this file:

```shell
nano ~/.stchaind/config/app.toml
```

And search for this section:

```toml
###############################################################################
###                           gRPC Configuration                            ###
###############################################################################

# Enable defines if the gRPC server should be enabled.
enable = true

# Address defines the gRPC server address to bind to.
address = "0.0.0.0:9090"
```

Usage example:

!!! tip

    You can get the precompiled `grpcurl` binary [here](https://github.com/fullstorydev/grpcurl/releases).


```json
grpcurl -plaintext 127.0.0.1:9090 list stratos.register.v1.Query

stratos.register.v1.Query.BondedMetaNodeCount
stratos.register.v1.Query.BondedResourceNodeCount
stratos.register.v1.Query.MetaNode
stratos.register.v1.Query.Params
stratos.register.v1.Query.ResourceNode
stratos.register.v1.Query.StakeByNode
stratos.register.v1.Query.StakeByOwner
stratos.register.v1.Query.StakeTotal
```

You can see all the gRPC queries [here](../stratos-chain-grpc-queries).

To use your Full-Chain gRPC API for your SDS node (only after mainnet launch), edit your SDS config file:

```shell
nano ~/rsnode/configs/config.toml
```

and edit this line:

```toml
stratos_chain_url = '127.0.0.1:9090'
```

<br>

---

<br>