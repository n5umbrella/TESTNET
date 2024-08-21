# DRIA
## Docker install:
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```


## Install:
```
cd $HOME
git clone https://github.com/firstbatchxyz/dkn-compute-node
cd dkn-compute-node
cp .env.example .env
```

### your_private_key_xxxxx_metamask
```
YOUR_PRIVATE_KEY="xxxxx"
echo "DKN_WALLET_SECRET_KEY=$YOUR_PRIVATE_KEY" >> .env
```


### your_api_key_xxxxx here: `https://platform.openai.com/api-keys`
```
YOUR_OPENAI_API_KEY="xxxxx"
echo "OPENAI_API_KEY=$YOUR_OPENAI_API_KEY" >> .env
```


```
tmux new -s dria "./start.sh -m=gpt-4o-mini"
```
