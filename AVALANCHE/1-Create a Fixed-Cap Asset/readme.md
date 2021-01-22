### Create a Fixed Cap Asset

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
    "jsonrpc":"2.0",
    "id"     :2,
    "method" :"avm.createAddress",
    "params" :{
        "username": "ximore",
        "password": "AuPr-calc"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```



> Check the list of address
```bash
$ curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "avm.listAddresses",
    "params": {
        "username": "ximore",
        "password": "AuPr-calc"
    },
    "id": 1
}'  -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```

```bash
>> Result
{"jsonrpc":"2.0","result":{"addresses":["X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu","X-local18k6zvt8se0y572jsyh35f8gcdn60xtnf4g42s8","X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct","X-local1flz4gekx5xt7ag8gg64zqa7y000u9nqnakuf2l","X-local10hfe9wd6jgcflehk3hsw4egmfjeqjv8s0e3c6d","X-local1vjq0c9ptptyj46gkzxz7gzqxpkv6dyez9tj4cf"]},"id":1}
```

> Import private key
```bash
$ curl --location --request POST 'localhost:9650/ext/X' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "avm.importKey",
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
{"jsonrpc":"2.0","result":{"address":"X-local18jma8ppw3nhx5r4ap8clazz0dps7rv5u00z96u"},"id":1}
```


> Create Fixed Cap Asset
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     : 1,
    "method" :"avm.createFixedCapAsset",
    "params" :{
        "name": "Ximore",
        "symbol":"XMR",
        "denomination": 0,
        "initialHolders": [
            {
                "address": "X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu",
                "amount": 12345678
            }
        ],
        "from":["X-local18jma8ppw3nhx5r4ap8clazz0dps7rv5u00z96u"],
        "changeAddr":"X-local18k6zvt8se0y572jsyh35f8gcdn60xtnf4g42s8",
        "username": "ximore",
        "password": "AuPr-calc"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```

>> Result
```bash
{"jsonrpc":"2.0","result":{"assetID":"2RW9pTBa8MMXiJ2V6Npo89vbLPU9ppmDfZCFLDsQyRVshoPyBz","changeAddr":"X-local18k6zvt8se0y572jsyh35f8gcdn60xtnf4g42s8"},"id":1}
```


> Check Balance
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.getBalance",
    "params" :{
        "address":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu",
        "assetID":"2RW9pTBa8MMXiJ2V6Npo89vbLPU9ppmDfZCFLDsQyRVshoPyBz"
    },
        "username": "ximore",
        "password": "AuPr-calc"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"balance":"12345678","utxoIDs":[{"txID":"2RW9pTBa8MMXiJ2V6Npo89vbLPU9ppmDfZCFLDsQyRVshoPyBz","outputIndex":1}]},"id":1}
```

> Send Asset
```bash
$  curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.send",
    "params" :{
        "username": "ximore",
        "password": "AuPr-calc",
        "assetID" :"2RW9pTBa8MMXiJ2V6Npo89vbLPU9ppmDfZCFLDsQyRVshoPyBz",
        "amount"  : 123,
        "to"      :"X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X   
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"fuomp6sa64DkKkxfXrLcRcNW2DxmnGB71K8cqG78Jk8wYDk7N","changeAddr":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"},"id":1}
```



> Check Transaction Status
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.getTxStatus",
    "params" :{
        "txID":"fuomp6sa64DkKkxfXrLcRcNW2DxmnGB71K8cqG78Jk8wYDk7N"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"status":"Accepted"},"id":1}
```



> Check Balance of Address
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.getBalance",
    "params" :{
        "address":"X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct",
        "assetID":"2RW9pTBa8MMXiJ2V6Npo89vbLPU9ppmDfZCFLDsQyRVshoPyBz"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```

```bash
>> Result
{"jsonrpc":"2.0","result":{"balance":"123","utxoIDs":[{"txID":"fuomp6sa64DkKkxfXrLcRcNW2DxmnGB71K8cqG78Jk8wYDk7N","outputIndex":0}]},"id":1}
```
