![image](https://github.com/Mozgiii9/PryzmSetupTheNode/assets/74683169/1fbfcd0e-ec3e-439a-9c40-740cc8524975)

# Pryzm

### Project Description:

**[Pryzm](https://pryzm.zone/) is an L1 blockchain designed to optimize returns. The project offers a wide range of features including: LSD, yield tokenization, liquidity management authority, yield DEX, liquidity confirmation, and more.**

**Investment information:**

I couldn't find official information, but a16z funders and other funders of well-known funds are signed up on Twitter.

**Official Resources:**

- Website : [go](https://pryzm.zone/)
- Twitter : [go](https://twitter.com/Pryzm_Zone)
- Discord : [go](http://discord.gg/sJN5Q2DBcP)
- Medium : [go](https://pryzm.medium.com/)
- Docs : [go](https://docs.pryzm.zone/)
- Telegram : [go](https://t.me/pryzm_zone)
- Official Noda Installation Guide : [go](https://docs.pryzm.zone/overview/maintain-guides/run-node/running-pryzmd/)
- Link to faucet : [go](https://testnet.pryzm.zone/faucet)
- Explorer link : [go](https://testnet.chainsco.pe/pryzm/validators)

**Server Requirements:**

- *CPU : 4 CORES 2.5 GHz;*
- *RAM : 8 GB;*
- *Storage : 500 GB SSD;*
- *OS : Ubuntu 20.04 / Ubuntu 22.04.

### Instructions for installing the node:

**1. Update packages:**
```
sudo apt update && sudo apt upgrade -y
```

**2. Install the Bash script:**   
```
source <(curl -s https://itrocket.net/api/testnet/pryzm/autoinstall/)
```

**3. Fill in the wallet name, the name of the node (moniker'a), set the default port (26). The script will start to set the software of the node.**

**4. As soon as the logs are displayed, press CTRL+C to exit the logs display mode.**

**5. Install the latest updates:**

```
cd $HOME
```

```
wget -O pryzmd https://storage.googleapis.com/pryzm-zone/core/0.13.0/pryzmd-0.13.0-linux-amd64
```

```
chmod +x $HOME/pryzmd
```

```
sudo mv $HOME/pryzmd $(which pryzmd)
```

```
sudo systemctl restart pryzmd && sudo journalctl -u pryzmd -f
```

**Logs will be displayed, also exit them by CTRL+C.**

**6. Check the status of node synchronization. Wait for the value of "catching_up" to change from "true" to "false":**

```
pryzmd status 2>&1 | jq .SyncInfo
```

**7. Once "catching_up" has changed to "false", we move on to creating the wallet. Replace "$WALLET" with the wallet name you assigned in the Bash script:**

```
pryzmd keys add $WALLET
```

**Create passpharse (your wallet password). Enter it twice, then save the wallet address and the seed phrase to a safe place.

**8. Next, we execute the following command. Replace "$WALLET" with the name of your wallet:**
```
WALLET_ADDRESS=$(pryzmd keys show $WALLET -a)
```

**9. Also change "$WALLET" to the name of your wallet. Enter passphrase:**

```
VALOPER_ADDRESS=$(pryzmd keys show $WALLET --bech val -a)
```

**10. Before creating the validator, let's check the node synchronization status once again:** 

```
pryzmd status 2>&1 | jq .SyncInfo
```

**11. Go to [faucet](https://testnet.pryzm.zone/faucet), request tokens.** 

**12. Once received, let's create a validator. To do this, enter the following command:**

```
pryzmd tx staking create-validator \
--amount 1000000upryzm \
--from $WALLET \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(pryzmd tendermint show-validator) \
--moniker "moniker1" \
--identity "" \
--details "I love blockchain" \
--chain-id indigo-1 \
--fees 3000upryzm \
-y
```

**Replace the following values with your own values:**

*- "moniker" : Specify in quotes the name of your node that you specified when you ran the Bash script;*- "moniker" : Specify any description you wish.

*- "details" : Specify any description. You can leave it as in the example.

**13. Enter the wallet password, the transaction should be successfully executed. Copy the transaction hash returned by the server and go to [explorer](https://testnet.chainsco.pe/pryzm/validators)**.

** IMPORTANT: Your validator will have an "inactive" status. In order for your validator to change its status to "active" you need to start delegating tokens to other validators, as well as make it so that tokens are also delegated to you. To do this, you can go to the Discord of the project and ask to delegate tokens to your validator's address. You can find out the address of the validator in Explorer.

**Cheat sheet on commands:**

**1. Log validation:**

```
sudo journalctl -u pryzmd -f
```

**2. Check node synchronization:** 

```
pryzmd status 2>&1 | jq .SyncInfo
```

**3. Checking node information:**

```
pryzmd status 2>&1 | jq .NodeInfo
```

**4.  Restarting the service:**

```
sudo systemctl restart pryzmd
```

**5. Restore the wallet:** 

```
pryzmd keys add $WALLET --recover
```

**6. Get the list of available wallets:**

```
pryzmd keys list
```

**7. Check balance:**

```
pryzmd q bank balances $(pryzmd keys show $WALLET -a)
```

**8. Validator information:** 

```
pryzmd status 2>&1 | jq .ValidatorInfo
```

**9. Prison information:** ``` pryzmd status 2>&1 | jq .ValidatorInfo 

```
pryzmd q slashing signing-info $(pryzmd tendermint show-validator)
```

**10. Get the validator out of jail:** 

```
pryzmd tx slashing unjail --from $WALLET --chain-id indigo-1 --fees 3000upryzm -y
```

**11. You can put in an auto update. Do not exit the logs, otherwise the script will stop working:**

```
cd $HOME && \
wget -O pryzmd https://storage.googleapis.com/pryzm-zone/core/0.13.0/pryzmd-0.13.0-linux-amd64 && \
chmod +x $HOME/pryzmd && \
old_bin_path=$(which pryzmd) && \
home_path=$HOME && \
rpc_port=$(grep -m 1 -oP '^laddr = "\K[^"]+' "$HOME/.pryzm/config/config.toml" | cut -d ':' -f 3) && \
tmux new -s pryzm-upgrade "sudo bash -c 'curl -s https://raw.githubusercontent.com/itrocket-team/testnet_guides/main/utils/autoupgrade/upgrade.sh | bash -s -- -u \"1865300\" -b pryzmd -n \"$HOME/pryzmd\" -o \"$old_bin_path\" -h \"$home_path\" -p \"https://pryzm-testnet-api.itrocket.net/cosmos/gov/v1/proposals/449\" -r \"$rpc_port\'""
```

**12. Delegate tokens to yourself:**

```
pryzmd tx staking delegate $(pryzmd keys show $WALLET --bech val -a) 1000000upryzm --from $WALLET --chain-id indigo-1 --fees 3000upryzm -y
```

**13. Delegate tokens to another validator. Replace <TO_VALOPER_ADDRESS> with the address of the validator you want to delegate tokens to:**

```
pryzmd tx staking delegate <TO_VALOPER_ADDRESS> 1000000upryzm --from $WALLET --chain-id indigo-1 --fees 3000upryzm -y
```
