### Create a New Blockchain
##### 


### Install and run Avash node
##### Frist Terminal

```bash
 cd $GOPATH/src/github.com/ava-labs/avalanchego
 ./build/avalanchego --network-id=fuji
<!--  -->
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



> Create  Address | platform
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

> List Address
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.listAddresses",
    "params": {
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}'  -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/P
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"addresses":["P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg","P-local16twya5a8hvyqfe0usn67da5xhpap3cceqqxcvg","P-local1twynwqn634rgmu9qxwcmwd6sdef30nlj2st7gt","P-local1kk9etus9je6uhvaeafqf37aldyhrkug76mn806","P-local1jppr9kgqu0d76ass8e7c6cqh2jgnrq0gz9h6tq","P-local1d9mk259vl8te2sz3e92jzhqrxzpg73dqv7un90","P-local1twftwuzywesmrlt0e3t34gp6q3drtdz8prsn3x","P-local15zlkznd3u8sugrwgwpm4el2qg5487k68tkq20s","P-local1gd7yrqxadmyyg0zvaaxwfgf9lv9eh2n28xqk5r","P-local1272a76v5u5ynxzy50329w9vxyqfynahqx4ey98","P-local14hyxq4n47x7kg4msadzlmq4aw0h87079h25j3h","P-local18jma8ppw3nhx5r4ap8clazz0dps7rv5u00z96u"]},"id":1}
```
> Import private key
```bash
$ curl --location --request POST 'localhost:9650/ext/P' \
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


> Get Node ID

```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```


>> Result
```bash
{"jsonrpc":"2.0","result":{"nodeID":"NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg"},"id":1}
```


> Add Validator
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.addValidator",
    "params": {
        "nodeID":"NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg",
        "startTime":'$(date --date="10 minutes" +%s)',
        "endTime":'$(date --date="30 days" +%s)',
        "stakeAmount":2000000000000,
        "rewardAddress":"P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg",
        "changeAddr": "P-local16twya5a8hvyqfe0usn67da5xhpap3cceqqxcvg",
        "delegationFeeRate":10,
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```

> Adding a Subnet Validator
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.addSubnetValidator",
    "params": {
        "nodeID":"NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg",
        "subnetID":"2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA",
        "startTime":'$(date --date="10 minutes" +%s)',
        "endTime":'$(date --date="30 days" +%s)',
        "weight":1,
        "changeAddr": "P-local18jma8ppw3nhx5r4ap8clazz0dps7rv5u00z96u",
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```

```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"q18RMLzJ1vi4nyFsqxztge9tPSorkU7SZvVyfT4kxB3jrrCB9","changeAddr":"P-local18jma8ppw3nhx5r4ap8clazz0dps7rv5u00z96u"},"id":1}
```
```bash

$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.getPendingValidators",
    "params": {"subnetID":"2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA"},
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"validators":[{"txID":"q18RMLzJ1vi4nyFsqxztge9tPSorkU7SZvVyfT4kxB3jrrCB9","startTime":"1611271711","endTime":"1613863111","weight":"1","nodeID":"NodeID-7Xhw2mDxuDS44j42TCB6U5579esbSt3Lg"}],"delegators":[]},"id":1}
```

> Create the Genesis Data


>> Build Genesis | avm.buildGenesis
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "id"     : 1,
    "method" : "avm.buildGenesis",
    "params" : {
        "genesisData": {
            "asset1": {
                "name": "ximoreGen",
                "symbol":"XMR",
                "initialState": {
                    "fixedCap" : [
                        {
                            "amount":100000,
                            "address": "local1kk9etus9je6uhvaeafqf37aldyhrkug76mn806"
                        },
                        {
                            "amount":100000,
                            "address": "local1jppr9kgqu0d76ass8e7c6cqh2jgnrq0gz9h6tq"
                        },
                        {
                            "amount":50000,
                            "address": "local1d9mk259vl8te2sz3e92jzhqrxzpg73dqv7un90"
                        },
                        {
                            "amount":50000,
                            "address": "local1twftwuzywesmrlt0e3t34gp6q3drtdz8prsn3x"
                        }
                    ]
                }
            },
            "asset2": {
                "name": "ximoreFA",
                "symbol":"XMRF",
                "initialState": {
                    "variableCap" : [
                        {
                            "minters": [
                                "local14hyxq4n47x7kg4msadzlmq4aw0h87079h25j3h",
                                "local1gd7yrqxadmyyg0zvaaxwfgf9lv9eh2n28xqk5r"
                            ],
                            "threshold":1
                        },
                        {
                            "minters": [
                                "local15zlkznd3u8sugrwgwpm4el2qg5487k68tkq20s",
                                "local1272a76v5u5ynxzy50329w9vxyqfynahqx4ey98",
                                "local14hyxq4n47x7kg4msadzlmq4aw0h87079h25j3h"
                            ],
                            "threshold":2
                        }
                    ]
                }
            }
        }
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/vm/avm
```

```bash
>> Result
{"jsonrpc":"2.0","result":{"bytes":"111113CQ1MDf9CrobqtJnjsa6p5ZYWdQshdMFqVQd6PvGSy8PNJUnTX1oDT6pvXBUWAwnsaKSyAvDLGDRF8owDirPRicRnFZMam1BF532V2skF6JPhD95bKXinJz3jEHUkAxu7UwceM1mmuAMuJv8trB4WraBAT9KAYrJkhj8VSMwfJNyqgvD6zYp4YiVVP6vT7xz71YXN4YrW41qmoTWBtDcwtZTSvF6cYTy4qXB1DrcHPtdRUYTiXdn5XHsnPvpNZKbFigSSsWjyj5UQ3DopL98DqffXe9eTc99nbtfNZSv8DSRKuix5Kq29yH1ACscTAY8492c1qvg7fcAzqYK194N525GxyMeZUQ7Pf97sf9dPvUJyXbVSyk76Up7UY2erbi7adm8hgtGko42XJoKYfCMH7bYKPuJ7EZBJ9icVZNi6EvdpEEX1XQZZ96Lynt5H77ipqWhzRZsHroPv1ew36w5AsqMQkQHLxAapf4PfJErLmyBMqwnutAG53Tu4hG7tWsHzydHUU1TAHc7tsqdRVsSihQr6gRg6wspZyS8786ySjDmS4PxH4xofJpj3yzH1XA4fQxkzDPYLBQ91uPVpcZmHDqMW6Zgvj439SvSEHfKvvht22vKpJmxvdkMwJoQNJ6tLSibToeptUZ2Qxu3hAhQqPmAuf2GD3qxyw33GkXQKs79sDGkREyW","encoding":"cb58"},"id":1}
```
> Create the Blockchain
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.createBlockchain",
    "params" : {
        "subnetID": "2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA",
        "vmID":"avm",
        "name":"My new AVM",
        "genesisData": "111113CQ1MDf9CrobqtJnjsa6p5ZYWdQshdMFqVQd6PvGSy8PNJUnTX1oDT6pvXBUWAwnsaKSyAvDLGDRF8owDirPRicRnFZMam1BF532V2skF6JPhD95bKXinJz3jEHUkAxu7UwceM1mmuAMuJv8trB4WraBAT9KAYrJkhj8VSMwfJNyqgvD6zYp4YiVVP6vT7xz71YXN4YrW41qmoTWBtDcwtZTSvF6cYTy4qXB1DrcHPtdRUYTiXdn5XHsnPvpNZKbFigSSsWjyj5UQ3DopL98DqffXe9eTc99nbtfNZSv8DSRKuix5Kq29yH1ACscTAY8492c1qvg7fcAzqYK194N525GxyMeZUQ7Pf97sf9dPvUJyXbVSyk76Up7UY2erbi7adm8hgtGko42XJoKYfCMH7bYKPuJ7EZBJ9icVZNi6EvdpEEX1XQZZ96Lynt5H77ipqWhzRZsHroPv1ew36w5AsqMQkQHLxAapf4PfJErLmyBMqwnutAG53Tu4hG7tWsHzydHUU1TAHc7tsqdRVsSihQr6gRg6wspZyS8786ySjDmS4PxH4xofJpj3yzH1XA4fQxkzDPYLBQ91uPVpcZmHDqMW6Zgvj439SvSEHfKvvht22vKpJmxvdkMwJoQNJ6tLSibToeptUZ2Qxu3hAhQqPmAuf2GD3qxyw33GkXQKs79sDGkREyW",
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"2frjNZ17sDKEPn5qULZuaqEEkmVZSG8m2u1o516ZNbnEmhsQiV","changeAddr":"P-local1pk9p6vvwldc9hvvdsf2lhh6dktvuwmwx55c7tg"},"id":1}
```
> Verify Success
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"platform.getBlockchains",
    "params" :{}
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/P
```

```bash
>> Result
{"jsonrpc":"2.0","result":{"blockchains":[{"id":"2eNy1mUFdmaxXNj1eQHUe7Np4gju9sJsEtWQ4MX3ToiNKuADed","name":"X-Chain","subnetID":"11111111111111111111111111111111LpoYY","vmID":"jvYyfQTxGMJLuGWa55kdP2p2zSUYsQ5Raupu4TW34ZAUBAbtq"},{"id":"26sSDdFXoKeShAqVfvugUiUQKhMZtHYDLeBqmBfNfcdjziTrZA","name":"C-Chain","subnetID":"11111111111111111111111111111111LpoYY","vmID":"mgj786NP7uDwBCcq6YwThhaN8FLyybkCa4zBWTQbNgmK6k9A6"},{"id":"2frjNZ17sDKEPn5qULZuaqEEkmVZSG8m2u1o516ZNbnEmhsQiV","name":"My new AVM","subnetID":"2HRUpRCAkZAGpmV6zvhbAZELYgrLbfPHLwHZAKrzU6yecXffBA","vmID":"jvYyfQTxGMJLuGWa55kdP2p2zSUYsQ5Raupu4TW34ZAUBAbtq"}]},"id":1}
```
