# Hedge

Website: https://hedgeblock.io

Twitter: https://twitter.com/hedgeblockio

Discord: https://discord.gg/fxmUNYTayQ

Explorer: https://berberis.hedgescan.io/

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
--gas-prices="0.025uhedge" \
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
--gas-prices="0.025uhedge" \
--gas="auto" \
--gas-adjustment="1.5" \
--from wallet
```
## 2.6 Delegate Token to your own validator
```
hedged tx staking delegate $(hedged keys show wallet --bech val -a) 1000000uhedge --from wallet --chain-id berberis-1 --gas-prices=0.005uhedge  --gas-adjustment 1.5 --gas auto -y
```

## 2.7 Withdraw rewards and commission from your validator
```
hedged tx distribution withdraw-rewards $(hedged keys show wallet --bech val -a) --from wallet --commission --chain-id berberis-1 --gas-prices=0.005uhedge  --gas-adjustment 1.5 --gas auto -y
```
## 2.8 Unjail validator
```
hedged tx slashing unjail --from wallet --chain-id berberis-1 --gas-prices=0.005uhedge  --gas-adjustment 1.5 --gas auto -y
```
## 2.9 Services Management
```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable hedged

# Disable Service
sudo systemctl disable hedged

# Start Service
sudo systemctl start hedged

# Stop Service
sudo systemctl stop hedged

# Restart Service
sudo systemctl restart hedged

# Check Service Status
sudo systemctl status hedged

# Check Service Logs
sudo journalctl -u hedged -f --no-hostname -o cat
```

# 3. Backup Validator
```
cat $HOME/.hedge/config/priv_validator_key.json
```

# 4. Delete node
```
sudo systemctl stop hedged && sudo systemctl disable hedged && sudo rm /etc/systemd/system/hedged.service && sudo systemctl daemon-reload && rm -rf $HOME/.hedge && rm -rf hedge
```
# 5. Example gRPC usage
```
wget https://github.com/fullstorydev/grpcurl/releases/download/v1.7.0/grpcurl_1.7.0_linux_x86_64.tar.gz
tar -xvf grpcurl_1.7.0_linux_x86_64.tar.gz
chmod +x grpcurl
./grpcurl  -plaintext  localhost:$GRPC_PORT list
### MAKE SURE gRPC is enabled in app.toml
# grep -A 3 "\[grpc\]" $HOME/.hedge/config/app.toml
```
# 6. Example REST API query
```
curl localhost:$API_PORT/cosmos/staking/v1beta1/validators
### MAKE SURE API is enabled in app.toml
# grep -A 3 "\[api\]" $HOME/.hedge/config/app.toml
```
