### Create a SUBNET
### Install and run Avash node
##### Frist Terminal

```bash
$ cd $GOPATH/src/github.com/ava-labs/avash
$ ./avash
$ runscript scripts/five_node_staking.lua
```
##### Second Terminal
> Check Bootstrapped Info
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.isBootstrapped",
    "params": {
        "chain":"X"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```



> Create Keystore 
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "keystore.createUser",
    "params": {
        "username": "ximore",
        "password": "AuPr-calc"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/keystore
```



> Create  Address 
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.createAddress",
    "params": {
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```



> Check the list of address
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.listAddresses",
    "params": {
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}'  -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```

```bash
>> Result
{"jsonrpc":"2.0","result":{"addresses":["P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg","P-local16twya5a8hvyqfe0usn67da5xhpap3cceqqxcvg","P-local1twynwqn634rgmu9qxwcmwd6sdef30nlj2st7gt","P-local1kk9etus9je6uhvaeafqf37aldyhrkug76mn806","P-local1jppr9kgqu0d76ass8e7c6cqh2jgnrq0gz9h6tq","P-local1d9mk259vl8te2sz3e92jzhqrxzpg73dqv7un90","P-local1twftwuzywesmrlt0e3t34gp6q3drtdz8prsn3x","P-local15zlkznd3u8sugrwgwpm4el2qg5487k68tkq20s","P-local1gd7yrqxadmyyg0zvaaxwfgf9lv9eh2n28xqk5r","P-local1272a76v5u5ynxzy50329w9vxyqfynahqx4ey98","P-local14hyxq4n47x7kg4msadzlmq4aw0h87079h25j3h"]},"id":1}
```

> Import private key
```bash
$ curl --location --request POST 'localhost:9650/ext/platform' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "platform.importKey",
    "params":{
        "username": "ximore",
        "password": "AuPr-calc",
          "privateKey":"PrivateKey-ewoqjP7PxY4yr3iLTpLisriqt94hdyDFNgchSxGGztUrTXtNN"
    },
    "id": 1
}'
```
>> Result
```bash
{"jsonrpc":"2.0","result":{"address":"P-local18jma8ppw3nhx5r4ap8clazz0dps7rv5u00z96u"},"id":1}
```

> Create Subnet
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.createSubnet",
    "params": {
        "controlKeys":[
            "P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg",
            "P-local16twya5a8hvyqfe0usn67da5xhpap3cceqqxcvg"
        ],
        "threshold":2,
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA","changeAddr":"P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg"},"id":1}
```
> Get All Subnets
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.getSubnets",
    "params": {},
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"subnets":[{"id":"2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA","controlKeys":["P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg","P-local16twya5a8hvyqfe0usn67da5xhpap3cceqqxcvg"],"threshold":"2"},{"id":"11111111111111111111111111111111LpoYY","controlKeys":[],"threshold":"0"}]},"id":1}
```
> Check info of the Subnets
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.getSubnets",
    "params": {"ids":["2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA"]},
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"subnets":[{"id":"2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA","controlKeys":["P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg","P-local16twya5a8hvyqfe0usn67da5xhpap3cceqqxcvg"],"threshold":"2"}]},"id":1}
```