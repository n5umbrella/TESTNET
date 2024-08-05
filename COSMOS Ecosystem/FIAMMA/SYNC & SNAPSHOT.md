# SYNC & SNAPSHOT

## RPC, API, gRPC

### RPC: 
```
https://fiamma-testnet-rpc.itrocket.net
```

### API:
```
https://fiamma-testnet-api.itrocket.net
```

### gRPC:
```
fiamma-testnet-grpc.itrocket.net:443
```

### peers:
```
16b7389e724cc440b2f8a2a0f6b4c495851934ff@fiamma-testnet-peer.itrocket.net:49656
```

### seeds:
```
1e8777199f1edb3a35937e653b0bb68422f3c931@fiamma-testnet-seed.itrocket.net:50656
```

### live peers: (11 active)
```
PEERS="16b7389e724cc440b2f8a2a0f6b4c495851934ff@fiamma-testnet-peer.itrocket.net:49656,74ec322e114b6757ac066a7b6b55cd224cdb8885@65.21.167.216:37656,37e2b149db5558436bd507ecca2f62fe605f92fe@88.198.27.51:60556,e30701492127fdd86ccf243a55b9dc4146772235@213.199.42.85:37656,421beadda6355465be81703fd8d25c30b2233df0@5.78.71.69:26656,21a5cae23e835f99735798024eef39fa0875bc62@65.109.30.110:17456,dd09c5a54d233d7b1b238eecedf7d855b4cb549c@65.108.81.145:26656,043da1f559e0f83eff52ff65f76b012f0f0ee9b3@198.7.119.198:37656,5a6bdb09c087012e9aa9bbdaa95694a82d489a94@144.76.155.11:26856,a03a1a53fafb669bfcce53b8b2a1362aa153cf99@77.90.13.137:37656,6a191379e4b0c13888714c0e83d58d803418be71@37.60.250.188:26656"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.fiamma/config/config.toml
```

### peers scanner:

Stale peers can lead to node inefficiency over time. Below are 13 active peers found by our network scanner. These are verified for decent uptime in real time.

```
PEERS="526d13f3ce3e0b56fa3ac26a48f231e559d4d60c@35.73.202.182:26656,cedc2b0f16422718c320d17fc44935ad1c39e62d@18.179.17.155:26656,37e2b149db5558436bd507ecca2f62fe605f92fe@88.198.27.51:60556,08bf5047605aad07ac5ce32a0505c9dbab41f22f@188.40.85.207:12656,5d6828849a45cf027e035593d8790bc62aca9cef@18.182.20.173:26656,9c87bf6872f2ca15c5f0b73348e6315be512aaa8@65.108.10.239:60956,e00ecc7687e29f09f694dd4dd4a21988ce6a43f9@178.205.102.224:26696,98a1b70770aefa8dbfcc2511ab7c3142e422423b@35.74.243.172:26656,f1e941fa754357115f491dd1e138ac70610ab4a4@t-fiamma.rpc.utsa.tech:56656,f1e941fa754357115f491dd1e138ac70610ab4a4@5.9.87.231:56656,67fffd28af7cc2a928c52fae6a09fe2812a6638d@217.66.20.45:26696,dd09c5a54d233d7b1b238eecedf7d855b4cb549c@65.108.81.145:26656,7016526816a21635d87cee81905b875137007cb9@fiamma.test.rpc.nodeshub.online:28856"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.fiamma/config/config.toml
```

### addrbook: (upd 1h)
```
wget -O $HOME/.fiamma/config/addrbook.json https://server-5.itrocket.net/testnet/fiamma/addrbook.json
```

### genesis:
```
wget -O $HOME/.fiamma/config/genesis.json https://server-5.itrocket.net/testnet/fiamma/genesis.json
```

## Snapshot

height: `435874` | 29s ago | size: `27MB` | db: `goleveldb` | pruning: `custom`: `100/0/10` | indexer: `null`

```
sudo systemctl stop fiammad
```

```
cp $HOME/.fiamma/data/priv_validator_state.json $HOME/.fiamma/priv_validator_state.json.backup
```

```
rm -rf $HOME/.fiamma/data $HOME/.fiamma/wasm
curl https://server-5.itrocket.net/testnet/fiamma/fiamma_2024-08-05_435874_snap.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.fiamma
```

```
mv $HOME/.fiamma/priv_validator_state.json.backup $HOME/.fiamma/data/priv_validator_state.json
```

```
sudo systemctl restart fiammad && sudo journalctl -u fiammad -f
```

## State Sync
If you don't want to wait for a long synchronization you can use:

```
sudo systemctl stop fiammad
```

```
cp $HOME/.fiamma/data/priv_validator_state.json $HOME/.fiamma/priv_validator_state.json.backup
fiammad tendermint unsafe-reset-all --home $HOME/.fiamma
```

```
peers="16b7389e724cc440b2f8a2a0f6b4c495851934ff@fiamma-testnet-peer.itrocket.net:49656,74ec322e114b6757ac066a7b6b55cd224cdb8885@65.21.167.216:37656,37e2b149db5558436bd507ecca2f62fe605f92fe@88.198.27.51:60556,e30701492127fdd86ccf243a55b9dc4146772235@213.199.42.85:37656,421beadda6355465be81703fd8d25c30b2233df0@5.78.71.69:26656,21a5cae23e835f99735798024eef39fa0875bc62@65.109.30.110:17456,dd09c5a54d233d7b1b238eecedf7d855b4cb549c@65.108.81.145:26656,043da1f559e0f83eff52ff65f76b012f0f0ee9b3@198.7.119.198:37656,5a6bdb09c087012e9aa9bbdaa95694a82d489a94@144.76.155.11:26856,a03a1a53fafb669bfcce53b8b2a1362aa153cf99@77.90.13.137:37656,6a191379e4b0c13888714c0e83d58d803418be71@37.60.250.188:26656"  
SNAP_RPC="https://fiamma-testnet-rpc.itrocket.net:443"
```

```
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.fiamma/config/config.toml 
```

```
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 
```

```
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2
```

```
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.fiamma/config/config.toml
```

```
curl https://server-5.itrocket.net/testnet/fiamma/wasm_fiamma.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.fiamma
mv $HOME/.fiamma/priv_validator_state.json.backup $HOME/.fiamma/data/priv_validator_state.json
```

```
sudo systemctl restart fiammad && sudo journalctl -u fiammad -f
```

## Wasm

updates every hour

```
curl https://server-5.itrocket.net/testnet/fiamma/wasm_fiamma.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.fiamma 
sudo systemctl restart fiammad && sudo journalctl -u fiammad -f
```

