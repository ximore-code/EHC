### Create NFT

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


> Create NFT Asset
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.createNFTAsset",
    "params" :{
        "name":"nFT XMR",
        "symbol":"NFTX",
        "minterSets":[
            {
                "minters": [
                    "X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"
                ],
                "threshold": 1
            }
        ],
        "username": "ximore",
        "password": "AuPr-calc"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"assetID":"WhwaycbFJ3vC2hqDTPfdMqy33RrpkBw4915e74Y77PoPSCEwd","changeAddr":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"},"id":1}
```
> Get UTXOs for NFT
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     : 1,
    "method" :"avm.getUTXOs",
    "params" :{
        "addresses":["X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"]
    },
    "username": "ximore",
    "password": "AuPr-calc"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"numFetched":"4","utxos":["11AMG8ZnGaS3niXWMXdn9SWmrNoAiEJkpnY3boor41tMiRf95DrFNoTGL1KJnryQpNv8pLyk3WGk7c8WufQqs2F8GXusqBFYHv2abj768svpPGN3abFstdXMZsywTdX9QSbPrt7SpKqVLFemLyx722yACvNMhmNcV7ip8H","11KkgAdCqCJ8gkfrgpiwq3whG3JASkkeQRupdZYUx7D1ijSwrTbpPK3HpsHtYDHXAe7Ws1Jfn2w3LML8eTfzFrKVsU5WVZHdJC2wimzH7JCmcd1EZJtDuk6aZrehihKF5mW83NXT25ivMvKX8Sb8VYjcbuStqcKUAkCgby","11DFHUx3TAosr9CM5p9R1PvHeXuRons3rhbVZZqVVxrMBCiphwkb185MQPUSjfGteYZSGVC8ESpuZL5My7dBpLeCq75FY2y4K82312nLdkZ4VwkKMZr8Trra651JJh7ZN9PLYNvihByseVj8Zy69EnA2otg8vDYCkdyP4P","112RsMvddxDiDQa2wZQVL6yEq9GhuSPP9YddktFfhMUrJ8Rarq9Y7QncyuFntbjyaA7ZyLrDa1gqD5wg6MfV2hMDWnjaNo27D4M6XdqBMN2BN86J3aEWSHQ85cuYwAEEGzrueD2G1ZVGnqM7b5pGimunby4GA94HS"],"endIndex":{"address":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu","utxo":"2KEoat8oHF6Lg1fik5FqTyjxUq2QEk7vohNMw14FLrAP1ZUe4a"},"encoding":"cb58"},"id":1}
```
> Mint the Asset
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     : 1,
    "method" :"avm.mintNFT",
    "params" :{
        "assetID":"WhwaycbFJ3vC2hqDTPfdMqy33RrpkBw4915e74Y77PoPSCEwd",
        "payload":"2EWh72jYQvEJF9NLk",
        "to":"X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct",
    "username": "ximore",
    "password": "AuPr-calc"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"g7b2pxpKDwBdcWX5emvRcDCeXmaKSLeNcvVqNoqumCBaK6jy8","changeAddr":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"},"id":1}
```
> Get UTXOs for NFT Mint
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     : 1,
    "method" :"avm.getUTXOs",
    "params" :{
        "addresses":["X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct"]
    },
    "username": "ximore",
    "password": "AuPr-calc"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"numFetched":"3","utxos":["11DFHUx3TAosr9CM5p9R1PvHeXuRons3rhbVZZqVVxrMBCiphwkGkNX9nHroZ7nXWuRqtp16DwoRm22ZLqXspDajhVVSV4NHsx9xxwR5iEv85VFsGoojrN5vNEZnQj2KcQvChi57cACwQcUxm8qU6AKvCzkNpLm7KmLU5Y","112zfFHgLHVh5zeroqdwpgtbRmuk8DGHXExMoWgzfihJmqEDam1DRKPmJdWEKMJxdttfQ8Vq1tjnwHj1YRNFakYMWwSMAVyPya6GchfJkMJpyfdfsYs8vqQ9ujnjFxmm9HviiyVPaaFKS4q3w3Hd4W7Rh5EPLdnEWXEZ7LAGsGjhKz4bpaQcieFnZv7gXaA4FN6Hf62eycXCY9ab8x","11A67eNfZVM5R1Rx2rCqnMUJdcrCkBEpAPWQ9XUbVv7RZ2S4u7zxXCDqruiyU6hunTU9ANqd2YAbtv8ZccU2oVi58r7Uq1tcW3VJJp6VgV14KwshBFptA8JWEhVnKdZvzXZZEgmBK65bihJXuy3gU3vnCAFbt8hWjFN3A6EkAvDVjgCLU"],"endIndex":{"address":"X-local1arp5ekecmy565zth68ggkzmpn2yc9fk67850ct","utxo":"wYR4utkSVrnYh12rcP765qvpahgaZZqnu75H2vp1YDmvkvYKh"},"encoding":"cb58"},"id":1}
```
> Send NFT
```bash
$ curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"avm.sendNFT",
    "params" :{
        "assetID" :"WhwaycbFJ3vC2hqDTPfdMqy33RrpkBw4915e74Y77PoPSCEwd",
        "to"      :"X-local1flz4gekx5xt7ag8gg64zqa7y000u9nqnakuf2l",
        "groupID" : 0,
        "username": "ximore",
        "password": "AuPr-calc"
    }

}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/X
```
```bash
>> Result
{"jsonrpc":"2.0","result":{"txID":"5aEtivDjEhNNyQ997pJxrUcTkfvgfDLDNSGYTsUBrmSX4jXPV","changeAddr":"X-local1u6zn9m72u6au6dy73uvl0lrfce4vlut38e47eu"},"id":1}
```