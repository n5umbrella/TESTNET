
# STORY

![image](https://github.com/user-attachments/assets/f6614e54-4bcb-4477-b8e8-6d9483185d73)


# Installation

## System Specs

Hardware	Requirement
CPU 4 Cores

RAM 8 GB

Disk 200 GB

Bandwidth 10 MBit/s

## TOOL

### GET IP:

```
curl -s -4 icanhazip.com
```

### Basic Install:
```
sudo apt update && sudo apt upgrade -y
sudo apt install make net-tools curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

```
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev libleveldb-dev jq build-essential bsdmainutils git make ncdu htop screen unzip bc fail2ban htop -y
```

### Install GO:

# Install Go:
```
cd $HOME
VER="1.22.4"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
go version
```


OR:

```
ver="1.22.4"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

## INSTALL

### Download Story-Geth binary

```
wget https://story-geth-binaries.s3.us-west-1.amazonaws.com/geth-public/geth-linux-amd64-0.9.2-ea9f0d2.tar.gz
tar -xzvf geth-linux-amd64-0.9.2-ea9f0d2.tar.gz
sudo cp geth-linux-amd64-0.9.2-ea9f0d2/geth $HOME/go/bin/story-geth
source $HOME/.bash_profile
story-geth version
```

### Download Story binary
```
wget https://story-geth-binaries.s3.us-west-1.amazonaws.com/story-public/story-linux-amd64-0.9.11-2a25df1.tar.gz
tar -xzvf story-linux-amd64-0.9.11-2a25df1.tar.gz
sudo cp story-linux-amd64-0.9.11-2a25df1/story $HOME/go/bin/story
source $HOME/.bash_profile
story version
```


### Init Iliad node
```
story init --network iliad
```

### Create story-geth service file
```
sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
[Unit]
Description=Story Geth Client
After=network.target

[Service]
User=root
ExecStart=/root/go/bin/story-geth --iliad --syncmode full
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```


### Create story service file
```
sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
[Unit]
Description=Story Consensus Client
After=network.target

[Service]
User=root
ExecStart=/root/go/bin/story run
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

### Reload and start story-geth
```
sudo systemctl daemon-reload && \
sudo systemctl start story-geth && \
sudo systemctl enable story-geth && \
sudo systemctl status story-geth
```


### Reload and start story
```
sudo systemctl daemon-reload && \
sudo systemctl start story && \
sudo systemctl enable story && \
sudo systemctl status story
```


### Check logs
```
sudo journalctl -u story-geth -f -o cat
```

_**Wait a minute for connect peers**_


```
sudo journalctl -u story -f -o cat
```


### Check sync status
```
curl localhost:26657/status | jq
```


_**Waiting for your node**_

```
catching_up
 is 
false
```

you can create validator.

## Create validator

### Export private key

```
story validator export --export-evm-key
```


Your `EVM Private Key` saved to: `/root/.story/story/config/private_key.txt`

_**Please keep your private key in a safe place**_

```
story validator create --stake 1000000000000000000 --private-key "your_private_key"
```


### Backup the validator key:

File location: 
`/root/.story/story/config/priv_validator_key.json`

`Copy this file to your local machine.`

Store it carefully; this is the most crucial key for your validator.

### Check health of node
```
curl localhost:26657/health
```

This will let you know if the node is healthy `-{}` indicates it is

# END
