# Hedge

Website: https://hedgeblock.io

Twitter: https://twitter.com/hedgeblockio

Discord: https://discord.gg/fxmUNYTayQ

# 1. Minimum hardware requirement

4 Cores, 8G Ram, 200G SSD, Ubuntu 22.04

# 2. Auto Install
```
sudo apt install curl -y && source <(curl -s https://nodesync.top/hedge_auto)
```

## 2.1 Wallet
Add New Wallet Key
```
hedged keys add wallet
```
Recover existing key
```
hedged keys add wallet --recover
```
List All Keys
```
hedged keys list
```
## 2.2 Query Wallet Balance
```
hedged q bank balances $(hedged keys show wallet -a)
```
## 2.3 Check sync status
**False is synced**
```
hedged status 2>&1 | jq .SyncInfo.catching_up
```
## 2.4 Create Validator

Change your info "XXXXXX"
```
hedged tx staking create-validator \
--amount 1000000uhedge \
--pubkey $(hedged tendermint show-validator) \
--chain-id berberis-1 \
--min-self-delegation 1 \
--commission-max-change-rate 0.01 \
--commission-max-rate 0.2 \
--commission-rate 0.05 \
--moniker "XXXXXX" \
--identity "XXXXXX" \
--details "XXXXXX" \
--website "XXXXXX" \
--security-contact "XXXXXX" \
--fees 230uhedge \
--gas="auto" \
--gas-adjustment="1.5" \
--from "wallet"
```
## 2.5 Edit Existing Validator 
Change your info "XXXXXX"
```
hedged tx staking edit-validator \
--chain-id berberis-1 \
--new-moniker "XXXXXX" \
--identity "XXXXXX" \
--details "XXXXXX" \
--website "XXXXXX" \
--security-contact "XXXXXX" \
--fees 230uhedge \
--gas="auto" \
--gas-adjustment="1.5" \
--from wallet
```
## 2.6 Delegate Token to your own validator
```
hedged tx staking delegate $(hedged keys show wallet --bech val -a) 125000000000uhedge --from wallet --chain-id berberis-1 --gas-prices=0.005uhedge  --gas-adjustment 1.5 --gas auto -y
```

## 2.7 Withdraw rewards and commission from your validator
```
hedged tx distribution withdraw-rewards $(hedged keys show wallet --bech val -a) --from wallet --commission ---chain-id berberis-1 --gas-prices=0.005uhedge  --gas-adjustment 1.5 --gas auto -y
```

# 4. Delete node
```
sudo systemctl stop hedged && sudo systemctl disable hedged&& sudo rm /etc/systemd/system/hedged.service && sudo systemctl daemon-reload && rm -rf $HOME/.hedge && rm -rf hedge
```
