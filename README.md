RPC, API, gRPC
RPC:
https://humans-mainnet-rpc.itrocket.net
API:
https://humans-mainnet-api.itrocket.net
JSON-RPC:
https://humans-mainnet-evm.itrocket.net
gRPC:
humans-mainnet-grpc.itrocket.net:443
Peers, Seeds, Live Peers, Addrbook
peers:
5e51671241340f1d1e1409a9e0cc4474820bf782@humans-mainnet-peer.itrocket.net:17656
seeds:
7d8eea3d6d60c3e60e51b8f55db37e62dc0ec8b4@humans-mainnet-seed.itrocket.net:17656
live peers: (13 active)
PEERS="5e51671241340f1d1e1409a9e0cc4474820bf782@humans-mainnet-peer.itrocket.net:17656,6d25070551f6f623b08fea5f6641c6a143492f61@65.108.236.5:18456,9a3cc44be34b6ec42e84cee09f0acaa2540af135@35.88.103.221:26656,bb9903682f721d9f7b23dc2b661bc8131ee054d0@144.76.92.22:14656,7e99cb89f22ae507ca12063d91e51ab5d843a288@65.108.101.109:18456,90039404891999803bd44ef4b622371d4102ca1b@65.108.99.37:32656,076330a162cc3dddec1c0faec37d55e688931770@65.108.76.33:22256,4bae63e78310fb48d3971db3928bfdc6c077facb@142.132.152.46:33656,60aa6df96d45ffd74c45d15f7f5670f0009ca670@65.108.71.137:18456,b2e6a64c646b60e3097082f32b942ee7247893c5@65.109.115.172:18456,02daf8764bcb476c1091ea1cd85c8012f1f4d85a@94.130.32.7:26656,d70c9343af28023a78aceb653e885666c12fec3b@138.201.121.185:26687,94a589700c3cdff4a3d74c57bfd5721dbcbcdf8c@148.251.235.130:12656"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.humansd/config/config.toml
peers scanner:
Stale peers can lead to node inefficiency over time. Below are 8 active peers found by our network scanner. These are verified for decent uptime in real time.
PEERS="d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@humans.rpc.kjnodes.com:12256,4bae63e78310fb48d3971db3928bfdc6c077facb@142.132.152.46:33656,831d1f90e90912006a23b021785171555a64cfab@65.108.133.32:17656,5e51671241340f1d1e1409a9e0cc4474820bf782@humans-mainnet-rpc.itrocket.net:17656,5e51671241340f1d1e1409a9e0cc4474820bf782@65.109.69.119:17656,20f95f8b8dd32b94b593dc3e8fcf0b0aeb74b85d@94.237.93.65:26656,94a589700c3cdff4a3d74c57bfd5721dbcbcdf8c@148.251.235.130:12656,9a3cc44be34b6ec42e84cee09f0acaa2540af135@rpc.humans.nodestake.top:26656"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.humansd/config/config.toml
addrbook: (upd 1h)
wget -O $HOME/.humansd/config/addrbook.json https://server-1.itrocket.net/mainnet/humans/addrbook.json
genesis:
wget -O $HOME/.humansd/config/genesis.json https://server-1.itrocket.net/mainnet/humans/genesis.json
Snapshot
pruned
updated every 4h  available 24/7 (every server stores last 2 snapshots)
height: 1134332258m ago618 blocks agosize: 106MBdb: goleveldb
sudo systemctl stop humansd
cp $HOME/.humansd/data/priv_validator_state.json $HOME/.humansd/priv_validator_state.json.backup
rm -rf $HOME/.humansd/data
curl https://server-1.itrocket.net/mainnet/humans/humans_2025-06-26_11343322_snap.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.humansd
mv $HOME/.humansd/priv_validator_state.json.backup $HOME/.humansd/data/priv_validator_state.json
sudo systemctl restart humansd && sudo journalctl -u humansd -f
State Sync
If you don't want to wait for a long synchronization you can use:

sudo systemctl stop humansd

cp $HOME/.humansd/data/priv_validator_state.json $HOME/.humansd/priv_validator_state.json.backup
humansd tendermint unsafe-reset-all --home $HOME/.humansd

peers="5e51671241340f1d1e1409a9e0cc4474820bf782@humans-mainnet-peer.itrocket.net:17656,6d25070551f6f623b08fea5f6641c6a143492f61@65.108.236.5:18456,9a3cc44be34b6ec42e84cee09f0acaa2540af135@35.88.103.221:26656,bb9903682f721d9f7b23dc2b661bc8131ee054d0@144.76.92.22:14656,7e99cb89f22ae507ca12063d91e51ab5d843a288@65.108.101.109:18456,90039404891999803bd44ef4b622371d4102ca1b@65.108.99.37:32656,076330a162cc3dddec1c0faec37d55e688931770@65.108.76.33:22256,4bae63e78310fb48d3971db3928bfdc6c077facb@142.132.152.46:33656,60aa6df96d45ffd74c45d15f7f5670f0009ca670@65.108.71.137:18456,b2e6a64c646b60e3097082f32b942ee7247893c5@65.109.115.172:18456,02daf8764bcb476c1091ea1cd85c8012f1f4d85a@94.130.32.7:26656,d70c9343af28023a78aceb653e885666c12fec3b@138.201.121.185:26687,94a589700c3cdff4a3d74c57bfd5721dbcbcdf8c@148.251.235.130:12656"  
SNAP_RPC="https://humans-mainnet-rpc.itrocket.net:443"

sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.humansd/config/config.toml 

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.humansd/config/config.toml

mv $HOME/.humansd/priv_validator_state.json.backup $HOME/.humansd/data/priv_validator_state.json

sudo systemctl restart humansd && sudo journalctl -u humansd -fo cat
Node Sync Status Checker
#!/bin/bash
rpc_port=$(grep -m 1 -oP '^laddr = "\K[^"]+' "$HOME/.humansd/config/config.toml" | cut -d ':' -f 3)
while true; do
  local_height=$(curl -s localhost:$rpc_port/status | jq -r '.result.sync_info.latest_block_height')
  network_height=$(curl -s https://humans-mainnet-rpc.itrocket.net/status | jq -r '.result.sync_info.latest_block_height')

  if ! [[ "$local_height" =~ ^[0-9]+$ ]] || ! [[ "$network_height" =~ ^[0-9]+$ ]]; then
    echo -e "\033[1;31mError: Invalid block height data. Retrying...\033[0m"
    sleep 5
    continue
  fi

  blocks_left=$((network_height - local_height))
  echo -e "\033[1;33mNode Height:\033[1;34m $local_height\033[0m \033[1;33m| Network Height:\033[1;36m $network_height\033[0m \033[1;33m| Blocks Left:\033[1;31m $blocks_left\033[0m"

  sleep 5
done
Wasm
Sorry, this project does not support WebAssembly.
