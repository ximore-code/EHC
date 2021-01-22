### Create a Variable Cap Asset

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
{"jsonrpc":"2.0","result":{"addresses":["X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu","X-local18k6zvt8se0y572jsyh35f8gcdn60xtnf4g42s8","X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct","X-local1flz4gekx5xt7ag8gg64zqa7y000u9nqnakuf2l","X-local10hfe9wd6jgcflehk3hsw4egmfjeqjv8s0e3c6d","X-local1vjq0c9ptptyj46gkzxz7gzqxpkv6dyez9tj4cf","X-local18jma8ppw3nhx5r4ap8clazz0dps7rv5u00z96u","X-local1zkm0pj22rp4zhqycxyykw6sl64jushz8v0hnhg","X-local1s98xlkjqhnv8gn4t8maapm9k8rjmfmq6qqghxu","X-local1jrp639he3dp0yq3n6e5pw8894wkwhslnus6jjf"]},"id":1}
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
> Send Asset
```bash
$  curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.send",
    "params" :{
        "username": "ximore",
        "password": "AuPr-calc",
        "assetID" : "AVAX",
        "amount"  : 12345,
        "to"      :"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X   
```
>> Result
```bash
{"jsonrpc":"2.0","result":{"txID":"2MbrWcvY93VQzmFwfw6emtrbe2omQ2kRsaGtWnAx8RUWYDUTkD","changeAddr":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"},"id":1}
```
> Create Variable Cap Asset
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     : 1,
    "method" :"avm.createVariableCapAsset",
    "params" :{
        "name":"XIMORE xmr",
        "symbol":"XMR",
        "minterSets":[
            {
                "minters": [
                    "X-local18k6zvt8se0y572jsyh35f8gcdn60xtnf4g42s8"
                ],
                "threshold": 1
            },
            {
                "minters": [
"X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct",
"X-local1flz4gekx5xt7ag8gg64zqa7y000u9nqnakuf2l",
"X-local10hfe9wd6jgcflehk3hsw4egmfjeqjv8s0e3c6d"
                ],
                "threshold": 2
            }
        ],
        "from":["X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"],
        "changeAddr":"X-local1vjq0c9ptptyj46gkzxz7gzqxpkv6dyez9tj4cf",
        "username": "ximore",
        "password": "AuPr-calc"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```

>> Result
```bash
{"jsonrpc":"2.0","result":{"assetID":"NL5M6WkCLd89iWbBH6mvT34MyCk4dBrXa3zU79vT2FypyTGW5","changeAddr":"X-local1vjq0c9ptptyj46gkzxz7gzqxpkv6dyez9tj4cf"},"id":1}
```
> Send Asset
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     : 1,
    "method" :"avm.mint",
    "params" :{
        "amount":1250000,
        "assetID":"NL5M6WkCLd89iWbBH6mvT34MyCk4dBrXa3zU79vT2FypyTGW5",
        "to":"X-local1zkm0pj22rp4zhqycxyykw6sl64jushz8v0hnhg",
        "username": "ximore",
        "password": "AuPr-calc"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"imbVKp1J4YxbPSALEmkXZUvK7gviRV4nEPCNj3Pg5DKZN4cyW","changeAddr":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"},"id":1}
```
> Check Balance
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.getBalance",
    "params" :{
        "address":"X-local1zkm0pj22rp4zhqycxyykw6sl64jushz8v0hnhg",
        "assetID":"NL5M6WkCLd89iWbBH6mvT34MyCk4dBrXa3zU79vT2FypyTGW5"
    },
        "username": "ximore",
        "password": "AuPr-calc"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"balance":"1250000","utxoIDs":[{"txID":"imbVKp1J4YxbPSALEmkXZUvK7gviRV4nEPCNj3Pg5DKZN4cyW","outputIndex":2}]},"id":1}
```
> Send Assets
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.send",
    "params" :{
        "username": "ximore",
        "password": "AuPr-calc",
        "assetID" :"NL5M6WkCLd89iWbBH6mvT34MyCk4dBrXa3zU79vT2FypyTGW5",
        "amount"  : 300,
        "to"      :"X-local1s98xlkjqhnv8gn4t8maapm9k8rjmfmq6qqghxu"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"22auKdKvm9Fc2EC8yMBttv3duVuUURNt9MhDLwBmmSZ3ry3Epz","changeAddr":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"},"id":1}
```
> Check Balance of the Asset
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.getBalance",
    "params" :{
        "address":"X-local1s98xlkjqhnv8gn4t8maapm9k8rjmfmq6qqghxu",
        "assetID":"NL5M6WkCLd89iWbBH6mvT34MyCk4dBrXa3zU79vT2FypyTGW5"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash

>> Result
{"jsonrpc":"2.0","result":{"balance":"300","utxoIDs":[{"txID":"22auKdKvm9Fc2EC8yMBttv3duVuUURNt9MhDLwBmmSZ3ry3Epz","outputIndex":0}]},"id":1}
```