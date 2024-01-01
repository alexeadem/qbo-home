
# Command Line Interface

> Qbo is a client CLI that connects to the Qbo API using websockets. It can send a single command as a message and disconnect. Or it can keep the websocket open to continue receiving state messages from the [mirror](?id=qbo-cloud). It runs in docker. 

See all available [commands](?id=commands).

# Qbo Terminal

> It is already preconfigured when qbo is deployed and ready to use.  

# External Access
> If you are accessing the cluster outside the `qbo` terminal you can install the CLI as follows:

```bash
git clone https://git.eadem.com/alex/qbo-cli.git
cd qbo-cli
. ./alias
```

# Configuration priority

Qbo CLI processes configuration input in the following order:

1. Environment variables
* QBO_UID
* QBO_AUX
* QBO_HOST
* QBO_PORT
  
> Can be set in with the export command
```bash
export QBO_UID=33820cc1-d513-4fa8-88ac-1adb008c3864
```

2. $HOME/.qbo/cli.json
> It is in a json format and be retrived with the folowing command
```bash
qbo get user -l | jq .cli[]?
```

```json
{
    "qbo_port": 443,
    "qbo_host": "172.17.0.1",
    "qbo_uid": "33820cc1-d513-4fa8-88ac-1adb008c3864"
}
```
3. $HOME/.qbo/.cli.db
> Qbo internal db. Configured by the system.


  

# Cluster Operations
## Add Cluster
> Create two new cluster `dev prod` cluster
```bash
qbo add cluster dev prod | jq

```

> Create new cluster `test` with `5` nodes
```bash
qbo add cluster test -n 5 | jq

```

## Stop cluster
> Stop cluster `test`. All nodes in cluster `test` will be stopped
```bash
qbo stop cluster test | jq

```
## Start cluster
> Start cluster `test`.

```bash
qbo start cluster test | jq

```
## Delete cluster
> Delete cluster `test`. Cluster will be deleted. Operation is irreversible  

```bash
qbo delete cluster test | jq

```

 > Delete `all` clusters.  


```bash
qbo delete cluster -A | jq

```

# Node Operations

## Stop Node
> Stops node with name `node-8a774663.localhost` `bfc61532`
```bash
qbo stop node node-123 node-567 | jq

```

## Start Node
> Starts nodes `bfc61532`

```bash
qbo start node node-123 | jq

```
## Add Node

> Add new a new node to cluster `test`

```bash
qbo add node test | jq

```

> Add new `2` new nodes to cluster `test`

```bash
qbo add node test -n 2 | jq

```
## Delete Node
> Scale cluster down by deleting node `node-8a774663.localhost`

```bash
qbo del node node-123 | jq

```

# Network operations
## Get Networks
> Get all cluster networks
```bash
qbo get networks -A
```
> Get `dev` and `prod` cluster networks
```bash
qbo get network dev prod
```



